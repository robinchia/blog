---
layout: post
title: "Java 工具（jmap-jstack）在linux上的源码分析"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 内存分析
--- 

# Java 工具（jmap,jstack）在linux上的源码分析

[Java 工具（jmap,jstack）在linux上的源码分析（一）](http://blog.csdn.net/raintungli/article/details/7023092)

在我们常用的Jstack, Jmap 用于分析java虚拟机的状态的工具，通过起另一个虚拟机通过运行sun.tools包下的java文件，去跟踪另一个虚拟机的状态。

如果让你设计一个跟踪另一个进程的方法，你也通常会考虑这几种常用的方式。

第一种，就是通知被跟踪的进程，让进程执行相应的消息，同时对该消息做出反应。

第二种，就是通过内核的调用，直接能够访问进程的内存，堆栈情况，通过分析被跟踪的进程的内存结构，从而知道当前被跟踪的进程的状态。

第一种方式

优势：

对调用者和被调用者只要达成简单的通讯协议，调用者无需知道被调用者的逻辑，结构，只需要简单的发送命令的方式，被调用者能够接受到命令，并且对该命令进行回应就可以。

缺点：

如果被调用者当时的状态本来就不正常，或者繁忙，没办法对该命令做出响应，那这个跟踪进程往往是在规定的等待时间里，无法返回正确的需要的信息。其次被调用者在分析的过程中，有可能需要暂停进程中的其他的线程，而对被跟踪的进程有一定的影响。

第二种方式

优势：

通过内核的支持，访问被跟踪的内存，并作出快照，后台分析，很少影响被跟踪的进程。

缺点：

这种方式需要对被跟踪程的内存分配和使用非常的了解，无法解耦，而本身系统内核调用也会出问题。

Java工具类中也是大致实现了这2中方式，工具中会先选择第一种方式，如果发现第一种方式不能成功，将会建议使用-F参数，也就是第二种方式。

我们先讲第一种方式。

既然是需要向被跟踪进程发出命令，在linux中可以选择多种方式进行进程中通讯 共享内存，文件之类，其中创建socket的文件实现通讯是比较简单的方法。

下面是整个的流程图：

![]()
来源： <[http://blog.csdn.net/raintungli/article/details/7023092](http://blog.csdn.net/raintungli/article/details/7023092)> [Java 工具（jmap,jstack）在linux上的源码分析（二）信号处理](http://blog.csdn.net/raintungli/article/details/7034005)

当java虚拟机启动的时候，会启动很多内部的线程，这些线程主要在thread.cpp里的create_vm方法体里实现

而在thread.cpp里主要起了2个线程来处理信号相关的
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7034005# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7034005# "copy")

1. JvmtiExport::enter_live_phase();  
1.   
1. // Signal Dispatcher needs to be started before VMInit event is posted  
1. os::signal_init();  
1.   
1. // Start Attach Listener if +StartAttachListener or it can't be started lazily  
1. if (!DisableAttachMechanism) {  
1.   if (StartAttachListener || AttachListener::init_at_startup()) {  
1.     AttachListener::init();  
1.   }  
1. }  

 

### []()1. Signal Dispatcher 线程

在os.cpp中的signal_init()函数中，启动了signal dispatcher 线程，对signal dispather 线程主要是用于处理信号，等待信号并且分发处理，可以详细看signal_thread_entry的方法
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7034005# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7034005# "copy")

1. static void signal_thread_entry(JavaThread* thread, TRAPS) {  
1.   os::set_priority(thread, NearMaxPriority);  
1.   while (true) {  
1.     int sig;  
1.     {  
1.       // FIXME : Currently we have not decieded what should be the status  
1.       //         for this java thread blocked here. Once we decide about  
1.       //         that we should fix this.  
1.       sig = os::signal_wait();  
1.     }  
1.     if (sig == os::sigexitnum_pd()) {  
1.        // Terminate the signal thread  
1.        return;  
1.     }  
1.   
1.     switch (sig) {  
1.       case SIGBREAK: {  
1.         // Check if the signal is a trigger to start the Attach Listener - in that  
1.         // case don't print stack traces.  
1.         if (!DisableAttachMechanism && AttachListener::is_init_trigger()) {  
1.           continue;  
1.         }  
1.         // Print stack traces  
1.         // Any SIGBREAK operations added here should make sure to flush  
1.         // the output stream (e.g. tty->flush()) after output.  See 4803766.  
1.         // Each module also prints an extra carriage return after its output.  
1.         VM_PrintThreads op;  
1.         VMThread::execute(&op);  
1.         VM_PrintJNI jni_op;  
1.         VMThread::execute(&jni_op);  
1.         VM_FindDeadlocks op1(tty);  
1.         VMThread::execute(&op1);  
1.         Universe::print_heap_at_SIGBREAK();  
1.         if (PrintClassHistogram) {  
1.           VM_GC_HeapInspection op1(gclog_or_tty, true /* force full GC before heap inspection */,  
1.                                    true /* need_prologue */);  
1.           VMThread::execute(&op1);  
1.         }  
1.         if (JvmtiExport::should_post_data_dump()) {  
1.           JvmtiExport::post_data_dump();  
1.         }  
1.         break;  
1.       }  
1.       default: {  
1.         // Dispatch the signal to java  
1.         HandleMark hm(THREAD);  
1.         klassOop k = SystemDictionary::resolve_or_null(vmSymbolHandles::sun_misc_Signal(), THREAD);  
1.         KlassHandle klass (THREAD, k);  
1.         if (klass.not_null()) {  
1.           JavaValue result(T_VOID);  
1.           JavaCallArguments args;  
1.           args.push_int(sig);  
1.           JavaCalls::call_static(  
1.             &result,  
1.             klass,  
1.             vmSymbolHandles::dispatch_name(),  
1.             vmSymbolHandles::int_void_signature(),  
1.             &args,  
1.             THREAD  
1.           );  
1.         }  
1.         if (HAS_PENDING_EXCEPTION) {  
1.           // tty is initialized early so we don't expect it to be null, but  
1.           // if it is we can't risk doing an initialization that might  
1.           // trigger additional out-of-memory conditions  
1.           if (tty != NULL) {  
1.             char klass_name[256];  
1.             char tmp_sig_name[16];  
1.             const char* sig_name = "UNKNOWN";  
1.             instanceKlass::cast(PENDING_EXCEPTION->klass())->  
1.               name()->as_klass_external_name(klass_name, 256);  
1.             if (os::exception_name(sig, tmp_sig_name, 16) != NULL)  
1.               sig_name = tmp_sig_name;  
1.             warning("Exception %s occurred dispatching signal %s to handler"  
1.                     "- the VM may need to be forcibly terminated",  
1.                     klass_name, sig_name );  
1.           }  
1.           CLEAR_PENDING_EXCEPTION;  
1.         }  
1.       }  
1.     }  
1.   }  
1. }  

可以看到通过os::signal_wait();等待信号，而在linux里是通过sem_wait()来实现，接受到SIGBREAK(linux 中的QUIT)信号的时候（关于信号处理请参考笔者的另一篇博客[:java 中关于信号的处理在linux下的实现](http://blog.csdn.net/raintungli/article/details/7178472)），第一次通过调用 AttachListener::is_init_trigger（）初始化attach listener线程，详细见2.Attach Listener 线程。

1. 第一次收到信号，会开始初始化，当初始化成功，将会直接返回，而且不返回任何线程stack的信息（通过socket file的操作返回），并且第二次将不在需要初始化。如果初始化不成功，将直接在控制台的outputstream中打印线程栈信息。
1. 第二次收到信号，如果已经初始化过，将直接在控制台中打印线程的栈信息。如果没有初始化，继续初始化，走和第一次相同的流程。

### []()2. Attach Listener 线程

Attach Listener 线程是负责接收到外部的命令，而对该命令进行执行的并且吧结果返回给发送者。在jvm启动的时候，如果没有指定+StartAttachListener，该线程是不会启动的，刚才我们讨论到了在接受到quit信号之后，会调用 AttachListener::is_init_trigger（）通过调用用AttachListener::init（）启动了Attach Listener 线程，同时在不同的操作系统下初始化，在linux中 是在attachListener_Linux.cpp文件中实现的。

在linux中如果发现文件.attach_pid#pid存在，才会启动attach listener线程，同时初始化了socket 文件，也就是通常jmap,jstack tool干的事情，先创立attach_pid#pid文件，然后发quit信号,通过这种方式暗式的启动了Attach Listener线程（见博客：[http://blog.csdn.net/raintungli/article/details/7023092](http://blog.csdn.net/raintungli/article/details/7023092)）。

线程的实现在 attach_listener_thread_entry 方法体中实现
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7034005# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7034005# "copy")

1. static void attach_listener_thread_entry(JavaThread* thread, TRAPS) {  
1.   os::set_priority(thread, NearMaxPriority);  
1.   
1.   if (AttachListener::pd_init() != 0) {  
1.     return;  
1.   }  
1.   AttachListener::set_initialized();  
1.   
1.   for (;;) {  
1.     AttachOperation* op = AttachListener::dequeue();    
1.      if (op == NULL) {  
1.       return;   // dequeue failed or shutdown  
1.     }  
1.   
1.     ResourceMark rm;  
1.     bufferedStream st;  
1.     jint res = JNI_OK;  
1.   
1.     // handle special detachall operation  
1.     if (strcmp(op->name(), AttachOperation::detachall_operation_name()) == 0) {  
1.       AttachListener::detachall();  
1.     } else {  
1.       // find the function to dispatch too  
1.       AttachOperationFunctionInfo* info = NULL;  
1.       for (int i=0; funcs[i].name != NULL; i++) {  
1.         const char* name = funcs[i].name;  
1.         assert(strlen(name) <= AttachOperation::name_length_max, "operation <= name_length_max");  
1.         if (strcmp(op->name(), name) == 0) {  
1.           info = &(funcs[i]);  
1.           break;  
1.         }  
1.       }  
1.   
1.       // check for platform dependent attach operation  
1.       if (info == NULL) {  
1.         info = AttachListener::pd_find_operation(op->name());  
1.       }  
1.   
1.       if (info != NULL) {  
1.         // dispatch to the function that implements this operation  
1.         res = (info->func)(op, &st);  
1.       } else {  
1.         st.print("Operation %s not recognized!", op->name());  
1.         res = JNI_ERR;  
1.       }  
1.     }  
1.   
1.     // operation complete - send result and output to client  
1.     op->complete(res, &st);  
1.   }  
1. }  

在AttachListener::dequeue(); 在liunx里的实现就是监听刚才创建的socket的文件，如果有请求进来，找到请求对应的操作，调用操作得到结果并把结果写到这个socket的文件，如果你把socket的文件删除，jstack/jmap会出现错误信息 unable to open socket file:........

 

我们经常使用 kill -3 pid的操作打印出线程栈信息，我们可以看到具体的实现是在Signal Dispatcher 线程中完成的，因为kill -3 pid 并不会创建.attach_pid#pid文件，所以一直初始化不成功，从而线程的栈信息被打印到控制台中。
来源： <[http://blog.csdn.net/raintungli/article/details/7034005](http://blog.csdn.net/raintungli/article/details/7034005)>

 
### [Java 工具（jmap,jstack）在linux上的源码分析（三）执行的线程vm thread](http://blog.csdn.net/raintungli/article/details/7045024)

### 
在前面的博客中（[http://blog.csdn.net/raintungli/article/details/7034005](http://blog.csdn.net/raintungli/article/details/7034005)）所提到的信号转发线程，Attach Listener 线程都只是操作socket文件，并没有去执行比如stack 分析，或者heap的分析，真正的工作线程其实是vm thread.

（一）启动vm thread
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. jint Threads::create_vm(JavaVMInitArgs* args, bool* canTryAgain) {  
1. ...  
1.   // Create the VMThread  
1.   { TraceTime timer("Start VMThread", TraceStartupTime);  
1.     VMThread::create();  
1.     Thread* vmthread = VMThread::vm_thread();  
1.   
1.     if (!os::create_thread(vmthread, os::vm_thread))  
1.       vm_exit_during_initialization("Cannot create VM thread. Out of system resources.");  
1.   
1.     // Wait for the VM thread to become ready, and VMThread::run to initialize  
1.     // Monitors can have spurious returns, must always check another state flag  
1.     {  
1.       MutexLocker ml(Notify_lock);  
1.       os::start_thread(vmthread);  
1.       while (vmthread->active_handles() == NULL) {  
1.         Notify_lock->wait();  
1.       }  
1.     }  
1.   }  
1. ...  
1.   
1.   
1. }  

我们可以看到，在thread.cpp里启动了线程vm thread，在这里我们同时也稍微的略带的讲一下jvm在linux里如何启动线程的。

通常在linux中启动线程，是调用
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. int pthread_create((pthread_t *__thread, __const pthread_attr_t *__attr,void *(*__start_routine) (void *), void *__arg));  

而在java里却增加了os:create_thread --初始化线程 和os:start_thread--启动线程

我们去看一下jvm里面是如何在linux里做到的

在os_linux.cpp中来看create_thread的方法
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. bool os::create_thread(Thread* thread, ThreadType thr_type, size_t stack_size) {  
1. ....  
1. int ret = pthread_create(&tid, &attr, (void* (*)(void*)) java_start, thread);  
1. ....  
1. }  

继续看java_start方法

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. static void *java_start(Thread *thread) {  
1. ....  
1.   // handshaking with parent thread  
1.   {  
1.     MutexLockerEx ml(sync, Mutex::_no_safepoint_check_flag);  
1.   
1.     // notify parent thread  
1.     osthread->set_state(INITIALIZED);  
1.     sync->notify_all();  
1.   
1.     // wait until os::start_thread()  
1.     while (osthread->get_state() == INITIALIZED) {  
1.       sync->wait(Mutex::_no_safepoint_check_flag);  
1.     }  
1.   }  
1.   
1.   // call one more level start routine  
1.   thread->run();  
1.   
1.   return 0;  
1. }  

首先jvm先设置了当前线程的状态是Initialized, 然后notify所有的线程，

 while (osthread->get_state() == INITIALIZED) {
      sync->wait(Mutex::_no_safepoint_check_flag);
    }

不停的查看线程的当前状态是不是Initialized, 如果是的话，调用了sync->wait()的方法等待。

来看os:start_thread的方法 os.cpp
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. void os::start_thread(Thread* thread) {  
1.   // guard suspend/resume  
1.   MutexLockerEx ml(thread->SR_lock(), Mutex::_no_safepoint_check_flag);  
1.   OSThread* osthread = thread->osthread();  
1.   osthread->set_state(RUNNABLE);  
1.   pd_start_thread(thread);  
1. }  

这时候设置了线程的状态为runnable,但没有notify线程

在 pd_start_thread(thread)中, os_linux.cpp中
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. void os::pd_start_thread(Thread* thread) {  
1.   OSThread * osthread = thread->osthread();  
1.   assert(osthread->get_state() != INITIALIZED, "just checking");  
1.   Monitor* sync_with_child = osthread->startThread_lock();  
1.   MutexLockerEx ml(sync_with_child, Mutex::_no_safepoint_check_flag);  
1.   sync_with_child->notify();  
1. }  

这时候我们看到了notify 线程的操作

也就是这时候notify了线程，因为这时候的线程的状态是RUNNABLE, 方法java_start继续往下执行，于是调用了thread->run()的方法

 

对于线程vm Thread 也就是调用了vmthread::run方法

vmThread.cpp
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. void VMThread::run() {  
1. ...  
1. this->loop();  
1. ...  
1. }  

调用了loop函数，处理了VM_Operation 的queue 关于queue的级别和优先级处理算法：可以参考 另一篇博客：[http://blog.csdn.net/raintungli/article/details/6553337](http://blog.csdn.net/raintungli/article/details/6553337)

 

（二）Jstack 运行在vm thread里的VM_Operation

jstack 处理也就是在前面博客所提到的attach Listener 线程所做的 operation
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. static jint thread_dump(AttachOperation* op, outputStream* out) {  
1.   bool print_concurrent_locks = false;  
1.   if (op->arg(0) != NULL && strcmp(op->arg(0), "-l") == 0) {  
1.     print_concurrent_locks = true;  
1.   }  
1.   
1.   // thread stacks  
1.   VM_PrintThreads op1(out, print_concurrent_locks);  
1.   VMThread::execute(&op1);  
1.   
1.   // JNI global handles  
1.   VM_PrintJNI op2(out);  
1.   VMThread::execute(&op2);  
1.   
1.   // Deadlock detection  
1.   VM_FindDeadlocks op3(out);  
1.   VMThread::execute(&op3);  
1.   
1.   return JNI_OK;  
1. }  

简单看一下类VM_PrintThreads 它 继承了VM_Operation

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7045024# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7045024# "copy")

1. class VM_PrintThreads: public VM_Operation {  
1.  private:  
1.   outputStream* _out;  
1.   bool _print_concurrent_locks;  
1.  public:  
1.   VM_PrintThreads()                                                { _out = tty; _print_concurrent_locks = PrintConcurrentLocks; }  
1.   VM_PrintThreads(outputStream* out, bool print_concurrent_locks)  { _out = out; _print_concurrent_locks = print_concurrent_locks; }  
1.   VMOp_Type type() const                                           {  return VMOp_PrintThreads; }  
1.   void doit();  
1.   bool doit_prologue();  
1.   void doit_epilogue();  
1. };  

当调用VMThread::execute()也就是将VM_PrintThreads 放入了_vm_queue中，交给vm thread 处理，对vm thread来说取出queue里的VM_Operation,并且调用doit方法。

在jstack里，attach listener 的线程产生了VM_PrintThreads，VM_PrintJNI，VM_FindDeadlocks 3个operations，交给了vm thread  的线程处理。
来源： <[http://blog.csdn.net/raintungli/article/details/7045024](http://blog.csdn.net/raintungli/article/details/7045024)> [Java 工具（jmap,jstack）在linux上的源码分析（四）safe point](http://blog.csdn.net/raintungli/article/details/7162468)

safe point 顾明思意，就是安全点，当需要jvm做一些操作的时候，需要把当前正在运行的线程进入一个安全点的状态（也可以说停止状态），这样才能做一些安全的操作，比如线程的dump，堆栈的信息。

在jvm里面通常vm_thread（我们一直在谈论的做一些属于vm 份内事情的线程） 和cms_thread（内存回收的线程）做的操作，是需要将其他的线程通过调用SafepointSynchronize::begin 和 SafepointSynchronize:end来实现让其他的线程进入或者退出safe point 的状态。

## []()通常safepoint 的有三种状态

_not_synchronized 说明没有任何打断现在所有线程运行的操作，也就是vm thread, cms thread 没有接到操作的指令 _synchronizing vm thread,cms thread 接到操作指令，正在等待所有线程进入safe point _synchronized 所有线程进入safe point, vm thread, cms thread 可以开始指令操作

 

## []()Java线程的状态

通常在java 进程中的Java 的线程有几个不同的状态，如何让这些线程进入safepoint 的状态中，jvm是采用不同的方式

### []()a. 正在解释执行

由于java是解释性语言，而线程在解释java 字节码的时候，需要dispatch table,记录方法地址进行跳转的，那么这样让线程进入停止状态就比较容易了，只要替换掉dispatch table 就可以了，让线程知道当前进入softpoint 状态。

java里会设置3个DispatchTable，  _active_table，  _normal_table， _safept_table

_active_table 正在解释运行的线程使用的dispatch table

_normal_table 就是正常运行的初始化的dispatch table

_safept_table safe point需要的dispatch table

 

解释运行的线程一直都在使用_active_table,关键处就是在进入saftpoint 的时候，用_safept_table替换_active_table, 在退出saftpoint 的时候，使用_normal_table来替换_active_table

具体实现可以查看源码
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7162468# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7162468# "copy")

1. void TemplateInterpreter::notice_safepoints() {  
1.   if (!_notice_safepoints) {  
1.     // switch to safepoint dispatch table  
1.     _notice_safepoints = true;  
1.     copy_table((address*)&_safept_table, (address*)&_active_table, sizeof(_active_table) / sizeof(address));  
1.   }  
1. }  
1.   
1. // switch from the dispatch table which notices safepoints back to the  
1. // normal dispatch table.  So that we can notice single stepping points,  
1. // keep the safepoint dispatch table if we are single stepping in JVMTI.  
1. // Note that the should_post_single_step test is exactly as fast as the  
1. // JvmtiExport::_enabled test and covers both cases.  
1. void TemplateInterpreter::ignore_safepoints() {  
1.   if (_notice_safepoints) {  
1.     if (!JvmtiExport::should_post_single_step()) {  
1.       // switch to normal dispatch table  
1.       _notice_safepoints = false;  
1.       copy_table((address*)&_normal_table, (address*)&_active_table, sizeof(_active_table) / sizeof(address));  
1.     }  
1.   }  
1. }  

### []()
b. 运行在native code

如果线程运行在native code的时候，vm thread 是不需要等待线程执行完的，只需要在从native code 返回的时候去判断一下 _state 的状态就可以了。

在方法体里就是前面博客也出现过的 SafepointSynchronize::do_call_back()
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7162468# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7162468# "copy")

1. inline static bool do_call_back() {  
1.   return (_state != _not_synchronized);  
1. }  

判断了_state 不是_not_synchronized状态

为了能让线程从native code 回到java 的时候为了能读到/设置正确线程的状态，通常的解决方法使用memory barrier，java 使用OrderAccess::fence(); 在汇编里使用__asm__ volatile ("lock; addl $0,0(%%rsp)" : : : "cc", "memory"); 保证从内存里读到正确的值，但是这种方法严重影响系统的性能，于是java使用了每个线程都有独立的内存页来设置状态。通过使用使用参数-XX:+UseMembar  参数使用memory barrier，默认是不打开的，也就是使用独立的内存页来设置状态。

 

### []() c. 运行编译的代码

### []()1. Poling page 页面

Poling page是在jvm初始化启动的时候会初始化的一个单独的内存页面，这个页面是让运行的编译过的代码的线程进入停止状态的关键。

在linux里面使用了mmap初始化，源码如下
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7162468# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7162468# "copy")

1. address polling_page = (address) ::mmap(NULL, Linux::page_size(), PROT_READ, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0);  

### []()
2. 编译

java 的JIT 会直接编译一些热门的源码到机器码，直接执行而不需要在解释执行从而提高效率，在编译的代码中，当函数或者方法块返回的时候会去访问一个内存poling页面.

x86架构下
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7162468# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7162468# "copy")

1. void LIR_Assembler::return_op(LIR_Opr result) {  
1.   assert(result->is_illegal() || !result->is_single_cpu() || result->as_register() == rax, "word returns are in rax,");  
1.   if (!result->is_illegal() && result->is_float_kind() && !result->is_xmm_register()) {  
1.     assert(result->fpu() == 0, "result must already be on TOS");  
1.   }  
1.   
1.   // Pop the stack before the safepoint code  
1.   __ remove_frame(initial_frame_size_in_bytes());  
1.   
1.   bool result_is_oop = result->is_valid() ? result->is_oop() : false;  
1.   
1.   // Note: we do not need to round double result; float result has the right precision  
1.   // the poll sets the condition code, but no data registers  
1.   AddressLiteral polling_page(os::get_polling_page() + (SafepointPollOffset % os::vm_page_size()),  
1.                               relocInfo::poll_return_type);  
1.   
1.   // NOTE: the requires that the polling page be reachable else the reloc  
1.   // goes to the movq that loads the address and not the faulting instruction  
1.   // which breaks the signal handler code  
1.   
1.   __ test32(rax, polling_page);  
1.   
1.   __ ret(0);  
1. }  

在前面提到的SafepointSynchronize::begin 函数源码中

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7162468# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7162468# "copy")

1. if (UseCompilerSafepoints && DeferPollingPageLoopCount < 0) {  
1.   // Make polling safepoint aware  
1.   guarantee (PageArmed == 0, "invariant") ;  
1.   PageArmed = 1 ;  
1.   os::make_polling_page_unreadable();  
1. }  

这里提到了2个参数 UseCompilerSafepoints 和 DeferPollingPageLoopCount ，在默认的情况下这2个参数是true和-1

函数体将会调用os:make_polling_page_unreadable();在linux os 下具体实现是调用了mprotect(bottom,size,prot) 使polling 内存页变成不可读。

 

### []()3. 信号

到当编译好的程序尝试在去访问这个不可读的polling页面的时候，在系统级别会产生一个错误信号SIGSEGV, 可以参考笔者的一篇博客中曾经讲过[java 的信号处理](http://blog.csdn.net/raintungli/article/details/7178472)，可以知道信号SIGSEGV的处理函数在x86体系下见下源码：
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7162468# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7162468# "copy")

1. JVM_handle_linux_signal(int sig,  
1.                         siginfo_t* info,  
1.                         void* ucVoid,  
1.                         int abort_if_unrecognized){  
1.    ....  
1.    if (sig == SIGSEGV && os::is_poll_address((address)info->si_addr)) {  
1.         stub = SharedRuntime::get_poll_stub(pc);  
1.       }   
1.    ....  
1. }  

在linux x86,64 bit的体系中，poll stub 的地址 就是 SafepointSynchronize::handle_polling_page_exception 详细程序可见shareRuntime_x86_64.cpp

回到safepoint.cpp中，SafepointSynchronize::handle_polling_page_exception通过取出线程的safepoint_stat,调用函数void ThreadSafepointState::handle_polling_page_exception，最后通过调用SafepointSynchronize::block(thread()); 来block当前线程。

 

### []()d. block 状态

当线程进入block状态的时候，继续保持block状态。
来源： <[http://blog.csdn.net/raintungli/article/details/7162468](http://blog.csdn.net/raintungli/article/details/7162468)>[Java 工具（jmap,jstack）在linux上的源码分析(五) -F 参数的bug](http://blog.csdn.net/raintungli/article/details/7245709)

当使用jmap,jstack是用-F参数的时候，是通过调用系统调用ptrace来取的寄存器的信息，关于linux下的ptrace实现可以参考我的博客（[http://blog.csdn.net/raintungli/article/details/6563867](http://blog.csdn.net/raintungli/article/details/6563867)）

在jdk6u23版本之前你会发现，当你使用jstack -F的时候 经常在logger 里面 看到错误信息，直接抛出异常，根本无法看到堆栈信息。

Thread 26724: (state = BLOCKED)
Error occurred during stack walking:
sun.jvm.hotspot.debugger.DebuggerException: sun.jvm.hotspot.debugger.DebuggerException: get_thread_regs failed for a lwp
        at sun.jvm.hotspot.debugger.linux.LinuxDebuggerLocal$LinuxDebuggerLocalWorkerThread.execute(LinuxDebuggerLocal.java:152)
        at sun.jvm.hotspot.debugger.....

通过查看源码，最后调用的函数是process_get_lwp_regs /ps_proc.c

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7245709# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7245709# "copy")

1. static bool process_get_lwp_regs(struct ps_prochandle* ph, pid_t pid, struct user_regs_struct *user) {  
1.   // we have already attached to all thread 'pid's, just use ptrace call  
1.   // to get regset now. Note that we don't cache regset upfront for processes.  
1. // Linux on x86 and sparc are different.  On x86 ptrace(PTRACE_GETREGS, ...)  
1. // uses pointer from 4th argument and ignores 3rd argument.  On sparc it uses  
1. // pointer from 3rd argument and ignores 4th argument  
1. #if defined(sparc) || defined(sparcv9)  
1. #define ptrace_getregs(request, pid, addr, data) ptrace(request, pid, addr, data)  
1. #else  
1. #define ptrace_getregs(request, pid, addr, data) ptrace(request, pid, data, addr)  
1. #endif  
1.   
1. #ifdef _LP64  
1. #ifdef PTRACE_GETREGS64  
1. #define PTRACE_GETREGS_REQ PTRACE_GETREGS64  
1. #endif  
1. #else  
1. #if defined(PTRACE_GETREGS) || defined(PT_GETREGS)  
1. #define PTRACE_GETREGS_REQ PTRACE_GETREGS  
1. #endif  
1. #endif /* _LP64 */  
1.   
1. #ifdef PTRACE_GETREGS_REQ  
1.  if (ptrace_getregs(PTRACE_GETREGS_REQ, pid, user, NULL) < 0) {  
1.    print_debug("ptrace(PTRACE_GETREGS, ...) failed for lwp %d\n", pid);  
1.    return false;  
1.  }  
1.  return true;  
1. #else  
1.  print_debug("ptrace(PTRACE_GETREGS, ...) not supported\n");  
1.  return false;  
1. #endif  
1.   
1. }  

无法判断究竟是否是因为没有定义参数PTRACE_GETREGS_REQ，还是因为ptrace的调用参数错误所导致的，这样就必须打开print_debug，查看打印的信息。

通过源码，可以查到print_debug函数是通过环境变量LIBSAPROC_DEBUG来控制

设置 

export LIBSAPROC_DEBUG=1

运行

jstack -F processid

我们能看到错误中多了一行

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7245709# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7245709# "copy")

1. Thread 26724: (state = BLOCKED)  
1. libsaproc DEBUG: ptrace(PTRACE_GETREGS, ...) not supported  

产生的原因就非常清楚了，bug主要是因为宏定义PTRACE_GETREGS_REQ缺失，查看源码

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7245709# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7245709# "copy")

1. #ifdef _LP64  
1. #ifdef PTRACE_GETREGS64  
1. #define PTRACE_GETREGS_REQ PTRACE_GETREGS64  
1. #endif  
1. #else  
1. #if defined(PTRACE_GETREGS) || defined(PT_GETREGS)  
1. #define PTRACE_GETREGS_REQ PTRACE_GETREGS  
1. #endif  
1. #endif /* _LP64 */  

_LP64 是64位机器的宏定义，而对ptrace的参数PTRACE_GETREGS64，显然Linux kernel 2.6.35里面并没有支持，导致了没有宏定义PTRACE_GETREGS_REQ，这里明显是jvm没有考虑到的情况。

解决办法

a. 因为这是jvm 编译级别的bug，除非你重现修改编译libsaproc.so，覆盖目录/jdk1.6.0_23/jre/lib/amd64

笔者自己编译了这个lib,可以在csdn上下载（[http://download.csdn.net/detail/raintungli/4065304](http://download.csdn.net/detail/raintungli/4065304)），笔者编译的jdk版本是1.6.23 build(19.0)

b. 建议升级jvm到1.6.30版本，该版本已经测试过，已经修复该bug.

后话：

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7245709# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7245709# "copy")

1. if (ptrace_getregs(PTRACE_GETREGS_REQ, pid, user, NULL) < 0) {  
1.   print_debug("ptrace(PTRACE_GETREGS, ...) failed for lwp %d\n", pid);  
1.   return false;  
1. }  

jvm可以在此处更清楚点，不是简单的判断<0，而是在判断<0的时候把errno打印出来，能更容易的判断出是什么原因无法ptrace 上。

来源： <[http://blog.csdn.net/raintungli/article/details/7245709](http://blog.csdn.net/raintungli/article/details/7245709)>[Java 工具（jmap,jstack）在linux上的源码分析(六) -F 参数 读取动态链接共享库文件中的符号表](http://blog.csdn.net/raintungli/article/details/7289639)

通常我们使用jmap,jstack 去检查堆栈信息的时候，是不会使用-f参数的，但有的时候系统在无法打印出堆栈信息的时候，会建议你使用参数-F。

关于-F参数与非-F参数的区别笔者已经在前面的博客中讲述（[http://blog.csdn.net/raintungli/article/details/7023092](http://blog.csdn.net/raintungli/article/details/7023092)），简单的说也就是一种是让jvm进程自己打印出堆栈信息，另有一种是直接访问jvm的堆栈区通过固定的结构找出我们需要的信息。

# []()1. Linux-F参数的实现

在linux中可以使用ptrace的系统调用去访问运行中的进程的内存信息，具体如何实现可以参考笔者的博客（[http://blog.csdn.net/raintungli/article/details/6563867](http://blog.csdn.net/raintungli/article/details/6563867)）

在java中使用动态加载的方式加载jvm自己的链接共享库，jvm的核心链接共享库是libjvm.so，linux中如何动态加载可以参考（[http://www.ibm.com/developerworks/cn/linux/l-dynamic-libraries/#dynamiclinking](http://www.ibm.com/developerworks/cn/linux/l-dynamic-libraries/#dynamiclinking)）

因为是动态共享库，当想查找具体的参数的值，内存的信息的时候，就需要计算出正确的参数或者函数的地址。

# []()2. 共享库中的符号相对地址偏移

可运行程序，共享库使用ELF格式，当运行一个程序的时候，内核会把ELF加载到用户空间，里面记录了程序的函数和数据的地址和内容，elf文件格式就不具体描述了。

在linux 中可以使用结构体ELF_EHDR，ELF_PHDR，ELF_SHDR读出elf 的program header, section header, section data.

在jvm中源码具体实现请参考 /hotspot/agent/os/linux/salibelf.c 

在linux中本身就自带一个读取elf格式的工具，readelf 你可以使用不同的参数读取不同的内容。

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7289639# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7289639# "copy")

1. readelf -s libjvm.so  

显示共享库中的方法参数的虚拟地址，类型，名字

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7289639# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7289639# "copy")

1. readelf -l libjvm.so  

读取program headers，其中出现2个LOAD的类型，第一个是程序的指令虚拟的起始地址，另一个是程序数据的起始地址

通过2个地址我们就能找到共享库中的参数，函数的相对地址的偏移

# []()3. 进程中的符号地址

在第二章节中，得到的只是相对的地址偏移，并不是真实运行中的进程的符号地址，如何得到真实的地址在linux中就相对比较简单。

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7289639# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7289639# "copy")

1. cat /proc/$processid/maps  
在maps里详细记录了进程的堆栈分配的地址，包括共享库的地址，那么起始地址就是这个库分配的最小地址
**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7289639# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7289639# "copy")

1. 进程中共享库分配的最小地址+相对地址的偏移 =真实的进程中该函数或变量的真实地址  

# []()4. Java tool 保存的符号表

在jmap/jstack 中，为了提高读取符号地址的性能，避免每一次要找符号的地址从elf文件中查找，只是在初始话的时候将符号表保存成哈希表，其中key是符号的名字，内容是符号的地址，长度。

具体实现可以参考  /hotspot/src/os/linux/symtab.c build_symtab_internal 函数

来源： <[http://blog.csdn.net/raintungli/article/details/7289639](http://blog.csdn.net/raintungli/article/details/7289639)>
 
