---
layout: post
title: "jvm crash 的崩溃日志详细分析及注意点"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - 内存分析
--- 

# jvm crash 的崩溃日志详细分析及注意点

## 生成

1. 生成error 文件的路径：你可以通过参数设置-XX:ErrorFile=/path/hs_error%p.log, 默认是在java运行的当前目录 [default: ./hs_err_pid%p.log]

2. 参数-XX:OnError  可以在crash退出的时候执行命令，格式是-XX:OnError=“string”,  <string> 可以是命令的集合，用分号做分隔符, 可以用"%p"来取到当前进程的ID.
例如：

// -XX:OnError="pmap %p"                // show memory map

// -XX:OnError="gcore %p; dbx - %p"     // dump core and launch debugger

在linux中系统会fork出一个子进程去执行shell的命令，因为是用fork可能会内存不够的情况，注意修改你的 /proc/sys/vm/overcommit_memory 参数,不清楚为什么这里不使用vfork

3. -XX:+ShowMessageBoxOnError 参数，当jvm crash的时候在linux里会启动gdb 去分析和调式，适合在测试环境中使用。

## []()什么情况下不会生成error文件

linux 内核在发生OOM的时候会强制kill一些进程, 可以在/var/logs/messages中查找

## []()Error crash 文件的几个重要部分

**a.  错误信息概要**

**[plain]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. # A fatal error has been detected by the Java Runtime Environment:  
1. #  
1. #  SIGSEGV (0xb) at pc=0x0000000000043566, pid=32046, tid=1121192256  
1. #  
1. # JRE version: 6.0_17-b04  
1. # Java VM: Java HotSpot(TM) 64-Bit Server VM (14.3-b01 mixed mode linux-amd64 )  
1. # Problematic frame:  
1. # C  0x0000000000043566  
1. #  
1. # If you would like to submit a bug report, please visit:  
1. #   http://java.sun.com/webapps/bugreport/crash.jsp  
1. # The crash happened outside the Java Virtual Machine in native code.  
1. # See problematic frame for where to report the bug.  
SIGSEGV 错误的信号类型 

pc 就是IP/PC寄存器值也就是执行指令的代码地址

pid 就是进程id

# Problematic frame:

# V  [libjvm.so+0x593045]

就是导致问题的动态链接库函数的地址

pc 和 +0x593045 指的是同一个地址，只是一个是动态的偏移地址，一个是运行的虚拟地址

**b.信号信息**

Java中在linux 中注册的信号处理函数，中间有2个参数info, ucvoid

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. static void crash_handler(int sig, siginfo_t* info, void* ucVoid) {  
1.   // unmask current signal  
1.   sigset_t newset;  
1.   sigemptyset(&newset);  
1.   sigaddset(&newset, sig);  
1.   sigprocmask(SIG_UNBLOCK, &newset, NULL);  
1.   
1.   VMError err(NULL, sig, NULL, info, ucVoid);  
1.   err.report_and_die();  
1. }  
在crash report中的信号错误提示

**[plain]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. siginfo:si_signo=SIGSEGV: si_errno=0, si_code=1 (SEGV_MAPERR), si_addr=0x0000000000043566  
信号的详细信息和si_addr 出错误的内存，都保存在siginfo_t的结构体中，也就是信号注册函数crash_handler里的参数info,内核会保存导致错误的内存地址在用户空间的信号结构体中siginfo_t，这样在进程在注册的信号处理函数中可以取得导致错误的地址。

**c.寄存器信息**

**[plain]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. Registers:  
1. RAX=0x00002aacb5ae5de2, RBX=0x00002aaaaf46aa48, RCX=0x0000000000000219, RDX=0x00002aaaaf46b920  
1. RSP=0x0000000042d3f968, RBP=0x0000000042d3f9c8, RSI=0x0000000042d3f9e8, RDI=0x0000000045aef9b8  
1. R8 =0x0000000000000f80, R9 =0x00002aaab3d30ce8, R10=0x00002aaaab138ea1, R11=0x00002b017ae65110  
1. R12=0x0000000042d3f6f0, R13=0x00002aaaaf46aa48, R14=0x0000000042d3f9e8, R15=0x0000000045aef800  
1. RIP=0x0000000000043566, EFL=0x0000000000010202, CSGSFS=0x0000000000000033, ERR=0x0000000000000014  
1.   TRAPNO=0x000000000000000e  
寄存器的信息就保存在b部分的信号处理函数参数 (ucontext_t*)usVoid中

在X86架构下：

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. void os::print_context(outputStream *st, void *context) {  
1.   if (context == NULL) return;  
1.   
1.   ucontext_t *uc = (ucontext_t*)context;  
1.   st->print_cr("Registers:");  
1. #ifdef AMD64  
1.   st->print(  "RAX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RAX]);  
1.   st->print(", RBX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RBX]);  
1.   st->print(", RCX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RCX]);  
1.   st->print(", RDX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RDX]);  
1.   st->cr();  
1.   st->print(  "RSP=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RSP]);  
1.   st->print(", RBP=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RBP]);  
1.   st->print(", RSI=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RSI]);  
1.   st->print(", RDI=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RDI]);  
1.   st->cr();  
1.   st->print(  "R8 =" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R8]);  
1.   st->print(", R9 =" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R9]);  
1.   st->print(", R10=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R10]);  
1.   st->print(", R11=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R11]);  
1.   st->cr();  
1.   st->print(  "R12=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R12]);  
1.   st->print(", R13=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R13]);  
1.   st->print(", R14=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R14]);  
1.   st->print(", R15=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_R15]);  
1.   st->cr();  
1.   st->print(  "RIP=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_RIP]);  
1.   st->print(", EFL=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EFL]);  
1.   st->print(", CSGSFS=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_CSGSFS]);  
1.   st->print(", ERR=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_ERR]);  
1.   st->cr();  
1.   st->print("  TRAPNO=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_TRAPNO]);  
1. #else  
1.   st->print(  "EAX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EAX]);  
1.   st->print(", EBX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EBX]);  
1.   st->print(", ECX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_ECX]);  
1.   st->print(", EDX=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EDX]);  
1.   st->cr();  
1.   st->print(  "ESP=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_UESP]);  
1.   st->print(", EBP=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EBP]);  
1.   st->print(", ESI=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_ESI]);  
1.   st->print(", EDI=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EDI]);  
1.   st->cr();  
1.   st->print(  "EIP=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EIP]);  
1.   st->print(", CR2=" INTPTR_FORMAT, uc->uc_mcontext.cr2);  
1.   st->print(", EFLAGS=" INTPTR_FORMAT, uc->uc_mcontext.gregs[REG_EFL]);  
1. #endif // AMD64  
1.   st->cr();  
1.   st->cr();  
1.   
1.   intptr_t *sp = (intptr_t *)os::Linux::ucontext_get_sp(uc);  
1.   st->print_cr("Top of Stack: (sp=" PTR_FORMAT ")", sp);  
1.   print_hex_dump(st, (address)sp, (address)(sp + 8*sizeof(intptr_t)), sizeof(intptr_t));  
1.   st->cr();  
1.   
1.   // Note: it may be unsafe to inspect memory near pc. For example, pc may  
1.   // point to garbage if entry point in an nmethod is corrupted. Leave  
1.   // this at the end, and hope for the best.  
1.   address pc = os::Linux::ucontext_get_pc(uc);  
1.   st->print_cr("Instructions: (pc=" PTR_FORMAT ")", pc);  
1.   print_hex_dump(st, pc - 16, pc + 16, sizeof(char));  
1. }  
寄存器的信息在分析出错的时候是非常重要的
打印出执行附近的部分机器码

**[plain]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. Instructions: (pc=0x00007f48f14ef51a)  
1. 0x00007f48f14ef4fa:   90 90 55 48 89 e5 48 81 ec 98 9f 00 00 48 89 bd  
1. 0x00007f48f14ef50a:   f8 5f ff ff 48 89 b5 f0 5f ff ff b8 00 00 00 00  
1. 0x00007f48f14ef51a:   c7 00 01 00 00 00 c6 85 00 60 ff ff ff c9 c3 90  
1. 0x00007f48f14ef52a:   90 90 90 90 90 90 55 48 89 e5 53 48 8d 1d 94 00  
在instruction 部分中会打印出部分的机器码

格式是

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. 地址：机器码  
第一种使用udis库里带的udcli工具来反汇编

命令：

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. echo '90 90 55 48 89 e5 48 81 ec 98 9f 00 00 48 89 bd' | udcli -intel -x -64 -o 0x00007f48f14ef4fa  
显示出对应的汇编

第二种可以用

**[plain]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. objectdump -d -C libjvm.so >> jvmsodisass.dump   

查找偏移地址  0x593045， 就是当时的执行的汇编，然后结合上下文，源码推测出问题的语句。

**d.寄存器对应的内存的值**

**[cpp]** [view plain](http://blog.csdn.net/raintungli/article/details/7642575# "view plain")[copy](http://blog.csdn.net/raintungli/article/details/7642575# "copy")

1. RAX=0x0000000000000000 is an unknown value  
1. RBX=0x000000041a07d1e8 is an oop  
1. {method}  
1.  - klass: {other class}  
1. RCX=0x0000000000000000 is an unknown value  
1. RDX=0x0000000040111800 is a thread  
1. RSP=0x0000000041261b88 is pointing into the stack for thread: 0x0000000040111800  
1. RBP=0x000000004126bb20 is pointing into the stack for thread: 0x0000000040111800  
1. RSI=0x000000004126bb80 is pointing into the stack for thread: 0x0000000040111800  
1. RDI=0x00000000401119d0 is an unknown value  
1. R8 =0x0000000040111c40 is an unknown value  
1. R9 =0x00007f48fcc8b550: <offset 0xa85550> in /usr/java/jdk1.6.0_30/jre/lib/amd64/server/libjvm.so at 0x00007f48fc206000  
1. R10=0x00007f48f8ca7d41 is an Interpreter codelet  
1. method entry point (kind = native)  [0x00007f48f8ca7ae0, 0x00007f48f8ca8320]  2112 bytes  
1. R11=0x00007f48fc98f270: <offset 0x789270> in /usr/java/jdk1.6.0_30/jre/lib/amd64/server/libjvm.so at 0x00007f48fc206000  
1. R12=0x0000000000000000 is an unknown value  
1. R13=0x000000041a07d1e8 is an oop  
1. {method}  
1.  - klass: {other class}  
1. R14=0x000000004126bb88 is pointing into the stack for thread: 0x0000000040111800  
1. R15=0x0000000040111800 is a thread  
jvm 会通过寄存器的值对找对应的对象，也是一个比较好的参考

**e. 其他的信息**

error 里面还有一些线程信息，还有当时内存映像信息，这些都可以作为分析的部分参考

crash 报告可以大概的反应出一个当时的情况，特别是在没有core dump的时候，是比较有助于帮助分析的，但如果有core dump的话，最终还是core dump能快速准确的发现问题原因。
来源： <[http://blog.csdn.net/raintungli/article/details/7642575#comments](http://blog.csdn.net/raintungli/article/details/7642575#comments)> 
