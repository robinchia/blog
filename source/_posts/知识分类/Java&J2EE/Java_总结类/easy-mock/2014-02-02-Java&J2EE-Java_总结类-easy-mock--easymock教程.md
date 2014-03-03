---
layout: post
title: "easymock教程"
categories: Java&J2EE
tags: 
 - Java&J2EE
 - Java_总结类
 - easy-mock
--- 

# easymock教程

![]() ![]() ![]() http://www.javaeye.com - 做最棒的软件开发交流社区
easymock教程
作者: skydream http://skydream.javaeye.com
easymock教程，详细的介绍easymock的使用
第 1 / 65 页
本书由JavaEye提供的电子书DIY功能自动生成于 2010-12-06
![]() http://skydream.javaeye.com
目录
1. software test
1.1 easymock教程-目录 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3 1.2 easymock教程-mock和stub . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6 1.3 easymock教程-单元测试中的主要测试对象和依赖 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 1.4 easymock教程-record-replay-verify模型 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15 1.5 easymock教程-easymock的典型使用 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 18 1.6 easymock教程-class mocking . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 21 1.7 easymock教程-mock的限制 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24 1.8 easymock教程-strict和nice . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 28 1.9 easymock教程-创建stub对象 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 35 1.10 easymock教程-放宽调用次数 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 40 1.11 easymock教程-参数匹配 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 42 1.12 easymock教程-partial class mocking . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 46 1.13 easymock教程-运行时返回值或者异常 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 51 1.14 easymock教程-改变同一个方法调用的行为 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 55 1.15 easymock教程-自定义参数匹配器 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 57 1.16 easymock教程-命名mock对象 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 61 1.17 easymock教程-使用MockControl . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 64
第 2 / 65 页
![]() http://skydream.javaeye.com
1.1 easymock教程-目录
1.1 easymock教程-目录
发表时间: 2010-10-14 easymock是目前比较流行的java mock 工具，这个教程将比较详细的介绍easymock的使用。
主要内容来自easymock的官网介绍和教程，我针对日常使用情况进行了一些筛选和补充，另外增加一些个 人的理解和认识，希望能对不熟悉easymock的朋友们有所帮助。
如有疏漏，错误或者疑惑，欢迎大家指正和提醒，不胜感激。
一. 基本概念
这里讲述一些测试相关的基本概念，不仅仅适用于easymock，也同样适用于其他的mock框架如jmock， jmockit等。
1) mock 和 stub
2) 单元测试中的主要测试对象和依赖
3) record-replay-verify 模型
二. 基础教程
1) 典型使用
2) Class Mocking
第 3 / 65 页
![]() http://skydream.javaeye.com
1.1 easymock教程-目录
3) mock的限制
4) strict和nice
5) 创建stub对象
6) 放宽调用次数
7) 参数匹配
三. 高级教程
1) Partial class mocking
2) 运行时返回值或者异常
3) 改变同一个方法调用的行为
4) 自定义参数匹配器
四. 最佳实践 1) 命名mock对象
第 4 / 65 页
![]() http://skydream.javaeye.com
1.1 easymock教程-目录
2) 使用 MocksControl
第 5 / 65 页
![]() http://skydream.javaeye.com
1.2 easymock教程-mock和stub
1.2 easymock教程-mock和stub
发表时间: 2010-08-26 作为测试的基本概念，在开发测试中经常遇到mock和stub。之前认为自己对这两个概念已经很明白了，但是 当决定要写下来并写清楚以便能让不明白的人也能弄明白，似乎就很有困难。 试着写下此文，以检验自己是不是真的明白mock和stub。 一. 相同点 先看看两者的相同点吧，非常明确的是，mock和stub都可以用来对系统(或者将粒度放小为模块，单元)进行 隔离。 在测试，尤其是单元测试中，我们通常关注的是主要测试对象的功能和行为，对于主要测试对象涉及到的次 要对象尤其是一些依赖，我们仅仅关注主要测试对象和次要测试对象的交互，比如是否调用，何时调用，调用 的参数，调用的次数和顺序等，以及返回的结果或发生的异常。但次要对象是如何执行这次调用的具体细节， 我们并不关注，因此常见的技巧就是用mock对象或者stub对象来替代真实的次要对象，模拟真实场景来进行对 主要测试对象的测试工作。 因此从实现上看，mock和stub都是通过创建自己的对象来替代次要测试对象，然后按照测试的需要控制这个 对象的行为。 二. 不同点 1. 类实现的方式 从类的实现方式上看，stub有一个显式的类实现，按照stub类的复用层次可以实现为普通类(被多个测试案 例复用)，内部类(被同一个测试案例的多个测试方法复用)乃至内部匿名类(只用于当前测试方法)。对于stub的方 法也会有具体的实现，哪怕简单到只有一个简单的return语句。 而mock则不同，mock的实现类通常是有mock的工具包如easymock, jmock来隐式实现，具体mock的 方法的行为则通过record方式来指定。 以mock一个UserService, UserDao为例，最简单的例子，只有一个查询方法：
第 6 / 65 页
![]() http://skydream.javaeye.com
1.2 easymock教程-mock和stub
public interface UserService { User query(String userId); }
public class UserServiceImpl implements UserService { private UserDao userDao; public User query(String userId) { return userDao.getById(userId); } //setter for userDao }
public interface UserDao { User getById(String userId); }
stub的标准实现，需要自己实现一个类并实现方法:
public class UserDaoStub implements UserDao { public User getById(String id) { User user = new User(); user.set..... return user; } }
@Test public void testGetById() { UserServiceImpl service = new UserServiceImpl(); UserDao userDao = new UserDaoStub();
service.setUserDao(userDao);
User user = service.query("1001");
第 7 / 65 页
![]() http://skydream.javaeye.com
1.2 easymock教程-mock和stub
... }
mock的实现，以easymock为例，只要指定mock的类并record期望的行为，并没有显式的构造新类:
@Test public void testGetById() { UserDao dao = Easymock.createMock(UserDao.class); User user = new User(); user.set..... Easymock.expect(dao.getById("1001")).andReturn(user); Easymock.reply(dao);
UserServiceImpl service = new UserServiceImpl(); service.setUserDao(userDao); User user = service.query("1001"); ... Easymock.verify(dao)； }
对比可以看出，mock编写相对简单，只需要关注被使用的函数，所谓"just enough"。stub要复杂一些， 需要实现逻辑，即使是不需要关注的方法也至少要给出空实现。 2. 测试逻辑的可读性 从上面的代码可以看出，在形式上，mock通常是在测试代码中直接mock类和定义mock方法的行为，测 试代码和mock的代码通常是放在一起的，因此测试代码的逻辑也容易从测试案例的代码上看出来。 Easymock.expect(dao.getById("1001")).andReturn(user); 直截了当的指明了当前测试案例对UserDao这个 依赖的预期: getById需要被调用，调用的参数应该是"1001"，调用次数为1(不明确指定调用次数时easymock 默认为1)。 而stub的测试案例的代码中只有简单的UserDao userDao = new UserDaoStub ();构造语句和
第 8 / 65 页
![]() http://skydream.javaeye.com
1.2 easymock教程-mock和stub
service.setUserDao(userDao);设置语句，我们无法直接从测试案例的代码中看出对依赖的预期，只能进入具体 的UserServiceImpl类的query()方法，看到具体的实现是调用userDao.getById(userId)，这个时候才能明白完 整的测试逻辑。因此当测试逻辑复杂，stub数量多并且某些stub需要传入一些标记比如true，false之类的来制 定不同的行为时，测试逻辑的可读性就会下降。 3. 可复用性 Mock通常很少考虑复用，每个mock对象通过都是遵循"just enough"原则，一般只适用于当前测试方 法。因此每个测试方法都必须实现自己的mock逻辑，当然在同一个测试类中还是可以有一些简单的初始化逻辑 可以复用。 stub则通常比较方便复用，尤其是一些通用的stub，比如jdbc连接之类。spring框架就为此提供了大量的 stub来方便测试，不过很遗憾的是，它的名字用错了：spring-mock！ 4. 设计和使用 接着我们从mock和stub的设计和使用上来比较两者，这里需要引入两个概念：interaction-based和 state-based。 具体关于interaction-based和state-based，不再本文阐述，强烈推荐Martin Fowler 的一篇文 章，"Mocks Aren't Stubs"。地址为http://martinfowler.com/articles/mocksArentStubs.html(PS：当在 google中输入mock stub两个关键字做搜索时，出来结果的第一条就是此文，向Martin Fowler致敬，向 google致敬)，英文不好的同学，可以参考这里的一份中文翻译：http://www.cnblogs.com/anf/archive/ 2006/03/27/360248.html。 总结来说，stub是state-based，关注的是输入和输出。mock是interaction-based，关注的是交互过 程。 5. expectiation/期望 这个才是mock和stub的最重要的区别：expectiation/期望。 对于mock来说，exception是重中之重：我们期待方法有没有被调用，期待适当的参数，期待调用的次 数，甚至期待多个mock之间的调用顺序。所有的一切期待都是事先准备好，在测试过程中和测试结束后验证是 否和预期的一致。 而对于stub，通常都不会关注exception，就像上面给出的UserDaoStub的例子，没有任何代码来帮助判 断这个stub类是否被调用。虽然理论上某些stub实现也可以通过自己编码的方式增加对expectiation的内容，
第 9 / 65 页
![]() http://skydream.javaeye.com 比如增加一个计数器，每次调用+1之类，但是实际上极少这样做。 6. 总结
1.2 easymock教程-mock和stub
关于mock和stub的不同，在Martin Fowler的"Mocks Aren't Stubs"一文中，有以下结束，我将它列出来 作为总结： (1) Dummy 对象被四处传递，但是从不被真正使用。通常他们只是用来填充参数列表。 (2) Fake 有实际可工作的实现，但是通常有一些缺点导致不适合用于产品(基于内存的数据库就是一个好 例子)。 (3) Stubs 在测试过程中产生的调用提供预备好的应答，通常不应答计划之外的任何事。stubs可能记录关 于调用的信息，比如 邮件网关的stub 会记录它发送的消息，或者可能仅仅是发送了多少信息。 (4) Mocks 如我们在这里说的那样：预先计划好的对象，带有各种期待，他们组成了一个关于他们期待接 受的调用的详细说明。 三. 退化和转化 在实际的开发测试过程中，我们会发现其实mock和stub的界限有时候很模糊，并没有严格的划分方式， 从而造成我们理解上的含糊和困惑。 主要的原因在于现实使用中，我们经常将mock做不同程度的退化，从而使得mock对象在某些程度上如 stub一样工作。以easymock为例，我们可以通过anyObject(), isA(Class)等方式放宽对参数的检测，以 atLeastOnce(),anytimes()来放松对调用次数的检测，我们可以使用Easymock.createControl()而不是 Easymock.createStrictControl()来放宽对调用顺序的检测(或者调用checkOrder(false))，我们甚至可以通过 createNiceControl(), createNiceMock()来创建完全不限制调用方式而且自动返回简单值的mock，这和stub 就几乎没有本质区别了。 目前大多数的mock工具都提供mock退化为stub的支持，比如easyock中，除了上面列出的 any***,NiceMock之外，还提供诸如 andStubAnswer(),andStubDelegateTo(),andStubReturn(),andStubThrow()和asStub()。
第 10 / 65 页
![]() http://skydream.javaeye.com
1.2 easymock教程-mock和stub
前面也谈到过stub也是可以通过增加代码来实现一些expectiation的特性，stub理论上也是可以向mock 的方向做转化，而从使得两者的界限更加的模糊。
第 11 / 65 页
![]() http://skydream.javaeye.com
1.3 easymock教程-单元测试中的主要测试对象和依赖
1.3 easymock教程-单元测试中的主要测试对象和依赖
发表时间: 2010-10-14 在单元测试中，通常我们都会有一个明确的测试对象，我们测试的主要目的就是为了验证这个类的工作如我 们预期。
以下面的简单代码为例：
public interface UserService { User query(String userId); }
public class UserServiceImpl implements UserService { private UserDao userDao; public User query(String userId) { return userDao.getById(userId); }
public void setUserDao(UserDao userDao) { this.userDao = userDao; } }
public interface UserDao { User getById(String userId); }
public class UserDaoImpl implements UserDao {
private Datasource dataSource;
public User getById(String id) { User user = new User(); //execute database query
第 12 / 65 页
![]() http://skydream.javaeye.com
user.set.. return user; }
1.3 easymock教程-单元测试中的主要测试对象和依赖
public void setDatasource(Datasource datasource) { this.datasource = datasource; }
}
这里我们定义有两个interface: UserService 和 UserDao， 并给出了两个实现类UserServiceImpl 和 UserDaoImpl。 其中UserServiceImpl依赖到UserDao，通过setter方法可以注入一个UserDao实现。而 UserDaoImpl的实现则依赖到Datasource。
然后我们来为实现类UserServiceImpl 和 UserDaoImpl编写单元测试:
1. UserServiceImplTest
public class UserServiceImplTest {
@Test public void testQuery() { User expectedUser = new User(); expectedUser.setId("1001"); expectedUser.setAge(30); expectedUser.setName("user-1001"); UserDao userDao = EasyMock.createMock(UserDao.class); EasyMock.expect(userDao.getById("1001")).andReturn(expectedUser); EasyMock.replay(userDao);
UserServiceImpl service = new UserServiceImpl(); service.setUserDao(userDao); User user = service.query("1001");
第 13 / 65 页
![]() http://skydream.javaeye.com
assertNotNull(user); assertEquals("1001", user.getId()); assertEquals(30, user.getAge()); assertEquals("user-1001", user.getName());
1.3 easymock教程-单元测试中的主要测试对象和依赖
EasyMock.verify(userDao); } }
在这个测试类中，主要测试对象就是UserServiceImpl，对于UserServiceImpl的依赖UserDao，我们采取 mock这个UserDao来满足UserServiceImpl的测试需要。
2. UserDaoImplTest 代码示例就不详细写了，和上面的类似，主要测试对象就是UserDaoImpl, 我们将通过mock Datasource来 满足UserDaoImpl对datasource的测试需要。 可以从上面的例子中简单的看出，通常单元测试都遵循这样的惯例: AClass的单元测试类命名为 AclassTest，主要职责是测试AClass的行为，理所当然的主要测试对象就是AClass。而所有被AClass的依赖则 自然而然的成为次要测试对象，通常我们都不关注这些依赖的内部实现，也不会要求在AClass的单元测试案例 中对这些依赖的实现进行测试和验证。 这也符合单元测试的理念: 我们将类AClass定义为单元，测试这个单元的行为是否如预期。同时也符合 UserServiceImpl的实现逻辑：UserServiceImpl依赖到UserDao接口，并不直接依赖到UserDaoImpl，因此在 UserServiceImpl的单元测试中，也不应该引入UserDaoImpl这样的真实类，mock框架在这个时候是最适合出 场表演的了：我们可以通过mock UserDao来模拟出UserDao的各种行为以便检测UserServiceImpl在这些行为 下的处理是否正确: 不同的返回值，错误场景，异常场景。这也是mock框架在单元测试中被广泛使用的原因： 还有什么比mock 类更能方便的做到这些？
第 14 / 65 页
![]() http://skydream.javaeye.com
1.4 easymock教程-record-replay-verify模型
1.4 easymock教程-record-replay-verify模型
发表时间: 2010-10-15 record-replay-verify 模型容许记录mock对象上的操作然后重演并验证这些操作。这是目前mock框架领域 最常见的模型，几乎所有的mock框架都是用这个模型，有些是现实使用如easymock，有些是隐式使用如 jmockit。
以easymock为例，典型的easymock使用案例一般如下, 援引上一章中的例子：
public class UserServiceImplTest {
/** * this is a classic test case to use EasyMock. */ @Test public void testQuery() { User expectedUser = new User(); expectedUser.setId("1001"); expectedUser.setAge(30); expectedUser.setName("user-1001"); UserDao userDao = EasyMock.createMock(UserDao.class); EasyMock.expect(userDao.getById("1001")).andReturn(expectedUser); EasyMock.replay(userDao);
UserServiceImpl service = new UserServiceImpl(); service.setUserDao(userDao); User user = service.query("1001"); assertNotNull(user); assertEquals("1001", user.getId()); assertEquals(30, user.getAge()); assertEquals("user-1001", user.getName());
EasyMock.verify(userDao);
第 15 / 65 页
![]() http://skydream.javaeye.com
} }
1.4 easymock教程-record-replay-verify模型
在这里有两句非常明显的调用语句: Easymock.replay(...)和Easymock.verify(...)。这两个语句将上述代码分 成三个部分，分别对应record-replay-verify 3个阶段
1. record
User expectedUser = new User(); expectedUser.setId("1001"); expectedUser.setAge(30); expectedUser.setName("user-1001"); UserDao userDao = EasyMock.createMock(UserDao.class); EasyMock.expect(userDao.getById("1001")).andReturn(expectedUser);
这里我们开始创建mock对象，并期望这个mock对象的方法被调用，同时给出我们希望这个方法返回的结 果。
这就是所谓的"记录mock对象上的操作", 同时我们也会看到"expect"这个关键字。
总结说，在record阶段，我们需要给出的是我们对mock对象的一系列期望：若干个mock对象被调用，依从 我们给定的参数，顺序，次数等，并返回预设好的结果(返回值或者异常).
2. replay
UserServiceImpl service = new UserServiceImpl(); service.setUserDao(userDao); User user = service.query("1001");
第 16 / 65 页
![]() http://skydream.javaeye.com
1.4 easymock教程-record-replay-verify模型
在replay阶段，我们关注的主要测试对象将被创建，之前在record阶段创建的相关依赖被关联到主要测试对 象，然后执行被测试的方法，以模拟真实运行环境下主要测试对象的行为。
在测试方法执行过程中，主要测试对象的内部代码被执行，同时和相关的依赖进行交互：以一定的参数调用 依赖的方法，获取并处理返回。我们期待这个过程如我们在record阶段设想的交互场景一致，即我们期望在 replay阶段所有在record阶段记录的行为都将被完整而准确的重新演绎一遍，从而到达验证主要测试对象行为 的目的。
3. verify
assertNotNull(user); assertEquals("1001", user.getId()); assertEquals(30, user.getAge()); assertEquals("user-1001", user.getName()); EasyMock.verify(userDao);
在verify阶段，我们将验证测试的结果和交互行为。
通常验证分为两部分，如上所示： 一部分是验证结果，即主要测试对象的测试方法返回的结果(对于异常测试 场景则是抛出的异常)是否如预期，通常这个验证过程需要我们自行编码实现。另一部分是验证交互行为，典型 如依赖是否被调用，调用的参数，顺序和次数，这部分的验证过程通常是由mock框架来自动完成，我们只需要 简单调用即可。
在easymock的实现中，verify的部分交互行为验证工作，会提前在replay阶段进行：比如未记录的调用，调 用的参数等。如果验证失败，则直接结束replay以致整个测试案例。
record-replay-verify 模型非常好的满足了大多数测试场景的需要：先指定测试的期望，然后执行测试，再 验证期望是否被满足。这个模型简单直接，易于实现，也容易被开发人员理解和接受，因此被各个mock框架广 泛使用。
第 17 / 65 页
![]() http://skydream.javaeye.com
1.5 easymock教程-easymock的典型使用
1.5 easymock教程-easymock的典型使用
发表时间: 2010-10-15
关于easymock的典型使用方式，在easymock的官网文档中，有非常详尽的讲解，文档地址为 http://easymock.org/EasyMock3_0_Documentation.html，文档的开头一部分内容都是easymock中最基本 的使用介绍，虽然是英文，但是非常容易看懂，适用新学者入门。
这里只罗列一些简单的常用功能，依然以前面教程中使用到的测试案例为例：
public class UserServiceImplTest extends Assert {
@Test public void testQuery() {
User expectedUser = new User(); user.set.; UserDao userDao = Easymock.createMock(UserDao.class); Easymock.expect(userDao.getById("1001")).andReturn(expectedUser); Easymock.replay(userDao);
UserServiceImpl service = new UserServiceImpl(); service.setUserDao(userDao); user user = service.query("1001"); assertNotNull(user); assertEquals(); //veify return user
Easymock.verify(userDao);
}
}
第 18 / 65 页
![]() http://skydream.javaeye.com
1.5 easymock教程-easymock的典型使用
这段简短的代码中包含以下easymock的功能：
1. 创建mock对象 UserDao userDao = Easymock.createMock(UserDao.class);
2. 记录mock对象期望的行为 Easymock.expect(userDao.getById("1001")).andReturn(expectedUser); 这里记录了mock对象的行为：getById()方法被调用，调用次数为1(easymock之中如果没有明确指出调 用次数，默认为1),参数为"1001"，expectedUser将作为返回值。
3. 进入replay阶段 Easymock.replay(userDao);
4. 对mock对象执行验证 Easymock.verify(userDao);
对上面上面的代码稍加改动以展示easymock的其他基本功能：
1. 指定期望的调用次数 Easymock.expect(userDao.getById("1001")).andReturn(expectedUser).times(3);
2. 指定抛出期望的异常 Easymock.expect(userDao.getById("1001")).andThrow(new RuntimeException("no user exist"));
3. 记录void 方法的行为
第 19 / 65 页
![]() http://skydream.javaeye.com
1.5 easymock教程-easymock的典型使用
Easymock.expect(userDao.getById("1001")) 这样的用法只能使用与mock对象的有返回值的方法，如 果mock对象的方法是void，则需要使用expectLastCall()： userDao.someVoidMethod(); Easymock.expectLastCall(); 和Easymock.expect(***)一样，同样支持指定调用次数，抛出异常等： Easymock.expectLastCall().times(3); Easymock.expectLastCall().andThrow(new RuntimeException("some error"));
4. 灵活的参数匹配 Easymock.expect(userDao.getById(Easymock.isA(String.class))).andReturn(expectedUser); 类似的还有anyInt()，anyObject()， isNull() ， same(), startsWith()等诸多实现。具体细节请参考本教 程中的"参数匹配"一文。
第 20 / 65 页
![]() http://skydream.javaeye.com
1.6 easymock教程-class mocking
1.6 easymock教程-class mocking
发表时间: 2010-10-26 前面的例子中，mock的对象都是基于interface，虽然说我们总是强调要面对接口编程，而不要面对实现， 但是实际开发中不提取interface而直接使用class的场景非常之多。尤其是一些当前只有一个明确实现而看不到 未来扩展的类，是否应该提取interface或者说是否应该现在就提取interface，总是存在争论。
这种情况下，我们就会面临主要测试对象依赖到一个具体类而不是interface的情况，easymock中通过class extension 来提供对class mocking的支持。
1. class mocking的使用
easymock class extension的使用方式和普通的interface mock完全一致，基本上easymock中有的功能 easymock class extension都同样提供，而且所有的类名和方法名都保持一致。
ClassA impl = Easymock.createMock(ClassA.class); Easymock.expect(impl.getById("1001")).andReturn(...); Easymock.replay(impl); 唯一的差异在于，easymock class extension的java package和easymock不同，easymock是 org.easymock.*, 而 easymock class extension是org.easymock.classextension.*，典型如 org.easymock.classextension.Easymock 对应 org.easymock.Easymock。另外在发布时，两者是分开发布 的，easymock.jar 和 easymockclassextension.jar，需要根据需要分别导入，或者必要时同时导入。
2. 3.0新版本和向后兼容
我们来回顾一下easymock的历史版本: easymock 1.* 非常久远了，已经没有人在用。2.0版本在 2005-12-24发布，基于jdk1.5，之后陆续发布的2.1/2.2/2.3/2.4/2.5等几个版本中，都提供了对应版本的 easymock class extension。easymock 3.0 版本是最新版本，2010-05-08 发布，主要改进就是将easymock
第 21 / 65 页
![]() http://skydream.javaeye.com
1.6 easymock教程-class mocking
class extension的功能合并到easymock中，以后只要使用easymock就可以提供class mocking的功能。当然 为了兼容2.*下的旧代码，依然提供了EasyMock 3.0 Class Extension代理到easymock 3.0。
3.0版本之后，easymock class extension的class mocking功能已经无缝集成到easymock中，因此代码的 编写简洁了很多，强烈建议新用户直接使用3.0版本。对于使用2.*版本的旧有代码，easymock提供了 easymock class extension的3.0版本，兼容2.*的代码，底层实现实际是代理给easymock3.0。因此2.*版本 easymock class extension的用户可以通过简单的升级easymock class extension到3.0即可平滑升级，之后再 逐渐替换掉easymock class extension的代码。
3. class mocking的限制
class mocking是有一些限制的， 1) 不能mock类的 final方法 如果final方法被调用，则只能执行原有的正常代码。
2) 不能mock类的static 方法。 同样如果private方法被调用，只能执行原有的正常代码。
3) 不能mock类的一些特殊方法: equals(), toString()和hashCode(). 原因是easymock在实现是为每个class mock对象提供了内建的以上三个方法。需要强调的是，对于基于 interface的mock，这个限制也是同样存在的，即使以上三个方式是interface定义的一部分。
在使用时需要避开这种场景，或者组合使用其他的mock 框架比如jmockit来mock private方法和final方法。
第 22 / 65 页
![]() http://skydream.javaeye.com
1.6 easymock教程-class mocking
第 23 / 65 页
![]() http://skydream.javaeye.com
1.7 easymock教程-mock的限制
1.7 easymock教程-mock的限制
发表时间: 2010-11-25 easymock并不是万能的，在使用easymock时有一些限制需要注意。
(1) Object方法的限制 我们都知道java是一个单根继承体系，Object是所有类的基类。在Object类上有几个基本的方法， easymock是不能改变其行为的：equals(), hashCode()和toString()。
即对于easymock创建的mock对象，其equals(), hashCode()和toString()三个方法的行为时已经固定了点， 不能通过Easymock.expect()来指定这三个方法的行为，即使这三个方法是接口定义的一部分。
我们来先看一个例子：
public class Business { private Service service;
public void execute() { System.out.println("service.toString() = " + service.toString()); System.out.println("service.hashCode() = " + service.hashCode()); }
public void setService(Service service) { this.service = service; } }
private interface Service {
public String toString();
public int hashCode();
第 24 / 65 页
![]() http://skydream.javaeye.com
}
1.7 easymock教程-mock的限制
execute()方法将为我们打印出toString()和hashCode()方法的结果。
public class LimitationTest {
private Business
business;
private IMocksControl mocksControl;
private Service
service;
@Before public void init() { business = new Business(); mocksControl = EasyMock.createStrictControl(); service = mocksControl.createMock(Service.class); business.setService(service); }
@Test public void testDefaultBehavior() { business.execute(); }
@Test public void testCustomizedBehavior() {
EasyMock.expect(service.toString()).andReturn("Customized toString"); EasyMock.expect(service.hashCode()).andReturn(100000); mocksControl.replay();
business.execute(); mocksControl.verify(); } }
第 25 / 65 页
![]() http://skydream.javaeye.com
1.7 easymock教程-mock的限制
测试案例testDefaultBehavior()将为我们打印出mock对象默认的行为，输出如下： service.toString() = EasyMock for interface net.sourcesky.study.easymock.tutorial.LimitationTest$Service service.hashCode() = 26208195
可见easymock内部已经做好了toString()和hashCode()实现。
在测试案例testCustomizedBehavior()中，我们试图通过EasyMock.expect()来指定toString()和 hashCode()的行为，但是运行时遭遇错误：
java.lang.IllegalStateException: no last call on a mock available at org.easymock.EasyMock.getControlForLastCall(EasyMock.java:521) at org.easymock.EasyMock.expect(EasyMock.java:499) at net.sourcesky.study.easymock.tutorial.LimitationTest.testCustomizedBehavior(LimitationTest.java:51) ...
从"no last call on a mock available"的描述上看，easymock根本没有把对toString()方法的调用记录 (record)下来作为一个对mock对象的调用。
因此，在使用mock对象时，请注意equals(), hashCode()和toString()三个方法无法更改其行为。
(2) class mock的限制
相对于interface mock，class mock下easymock限制更多，除了上面谈到的equals(), hashCode()和 toString()三个方法外，还有以下限制：
第 26 / 65 页
![]() http://skydream.javaeye.com
1.7 easymock教程-mock的限制
1. final 方法不能被mock 2. private 方法不能对mock
(3) 静态方法 对于静态方法，easymock也无法mock其行为。
由于这个限制，当被测试类中有静态方法调用时，典型如单例方法调用，lookup方式的依赖查找， easymock就会力不从心。从这个角度上，推荐尽量使用IOC 控制反转/ DI依赖注入的方式来实现依赖的获取， 而不要使用lookup的主动查找方式。
实际开发中，当发现有因为静态方法的限制从而导致easymock无法mock我们期望的行为，造成测试案 例"不好写"，“写不下去”时，请换个角度思考：为什么要用静态方法？可不可以改成注入？
(4) 解决的方法
如果由于某些原因必须使用静态方法或者定制final, private方法的行为，则可以考虑搭配其他mock框架来完 成功能。
以静态方法方法为例，一个典型的使用范例是：使用jmockit来定制静态方法的行为，指定其返回easymock 创建的mock对象，然后使用easymock的标准方式定制这个mock对象的行为。
第 27 / 65 页
![]() http://skydream.javaeye.com
1.8 easymock教程-strict和nice
1.8 easymock教程-strict和nice
发表时间: 2010-11-19 在easymock的使用过程中，当创建mock对象时，我们会遇到 strict mock和nice mock的概念。
比如创建mock对象我们通常使用EasyMock.createMock()，但是我们会发现easymock同时提供了两个类似 的方法：
EasyMock.createNiceMock() EasyMock.createStrictMock()
类似的在创建MocksControl时，除了通常的EasyMock.createControl() 外，easymock也同时提供两个类 似的方法：
EasyMock.createNiceControl() EasyMock.createStrictControl()
我们来看看strict和nice有什么作用。参考easymock的javadoc，我们对比createMock()和 createStrictMock()：
EasyMock.createMock()： checking is disabled by default.
Creates a mock object that implements the given interface, order
EasyMock.createNiceMock() ： Creates a mock object that implements the given interface, order checking is enabled by default.
发现strict mock方式下默认是开启调用顺序检测的，而普通的mock方式则默认不开启调用顺序检测。
再看一下createNiceMock()：
第 28 / 65 页
![]() http://skydream.javaeye.com
1.8 easymock教程-strict和nice
Creates a mock object that implements the given interface, order checking is disabled by default, and the mock object will return 0, null or false for unexpected invocations.
和createMock()相同的是默认不开启调用顺序检测，另外有一个非常有用的功能就是对于意料之外的调用将 返回0,null 或者false.之所以说有用，是因为在我们的实际开发过程中，有时候会有这样的需求：对于某个 mock对象的调用(可以是部分，也可以是全部)，我们完全不介意调用细节，包括是否调用和调用顺序，参数， 返回值，我们只要求mock对象容许程序可以继续而不是抛出异常报告说 unexpected invocations 。nice mock在这种情况下可以为我们节省大量的工作量，非常方便。
我们来看一个简单的实际使用的例子，假设我们有一个Business类，依赖于两个service 接口：
先看只调用一个依赖的情况，注意在record阶段service1.method2()和service1.method1()的顺序和 business.executeService1()方法中的实际调用顺序是故意设置为不同的。
public class Business { private Service1 service1;
private Service2 service2;
public void executeService1() { service1.method1(); service1.method2(); }
public void executeService1And2() { service1.method1(); service1.method2();
service2.method3(); service2.method4(); }
public void setService1(Service1 service1) {
第 29 / 65 页
![]() http://skydream.javaeye.com
this.service1 = service1; }
1.8 easymock教程-strict和nice
public void setService2(Service2 service2) { this.service2 = service2; } }
private interface Service1 {
public void method1();
public void method2(); }
private interface Service2 {
public void method3();
public void method4(); }
1. 普通mock
@Test public void testMock() { Business business = new Business(); Service1 service1 = EasyMock.createMock("service1", Service1.class); business.setService1(service1);
service1.method2(); EasyMock.expectLastCall(); service1.method1(); EasyMock.expectLastCall();
EasyMock.replay(service1);
第 30 / 65 页
![]() http://skydream.javaeye.com
business.executeService1(); EasyMock.verify(service1); }
1.8 easymock教程-strict和nice
测试案例可以通过，说明EasyMock.createMock()的确是不检测方法的调用顺序。 2. strict mock
@Test public void testStrictMock() { Business business = new Business(); Service1 service1 = EasyMock.createStrictMock("service1", Service1.class); ... }
案例失败，错误信息如下 java.lang.AssertionError: Unexpected method call service1.method1(): service1.method2(): expected: 1, actual: 0 at org.easymock.internal.MockInvocationHandler.invoke(MockInvocationHandler.java:45) at org.easymock.internal.ObjectMethodsFilter.invoke(ObjectMethodsFilter.java:73) at net.sourcesky.study.easymock.tutorial.$Proxy4.method1(Unknown Source) at net.sourcesky.study.easymock.tutorial.OrderTest$Business.executeService1(OrderTest.java:14) at net.sourcesky.study.easymock.tutorial.OrderTest.testStrictMock(OrderTest.java:79) ...... 说明strict mock下，easymock检测到了实际调用时的顺序和预期的不同。 3. nick mock
@Test public void testNiceMock() { Business business = new Business(); Service1 service1 = EasyMock.createNiceMock("service1", Service1.class);
第 31 / 65 页
![]() http://skydream.javaeye.com
... }
1.8 easymock教程-strict和nice
测试案例可以通过，而且如果是nick mock的话，record阶段可以简化：
@Test public void testNiceMockSimplify() { Business business = new Business(); Service1 service1 = EasyMock.createNiceMock("service1", Service1.class); business.setService1(service1);
EasyMock.replay(service1); business.executeService1(); EasyMock.verify(service1); }
这个简化版本的测试案例也是可以通过的。
上述的测试案例验证了strict mock和nice mock的基本使用，对于同一个mock对象，strict模式下多个方法 之间的调用顺序在record阶段和replay阶段下是需要保持一致的。但是故事并不是到此结束，更有意思的内容 在后面：如果出现多个mock对象，那么这些不同mock对象的方法之间，他们的调用顺序是否检测？普通mock 和nice mock模式下自然是不会检测顺序，但是strict模式下呢？
我们来看需要测试的方法executeService1And2()，这个方法会依次调用service1和service2的方法。使用 easymock测试这个方法，注意我们在record阶段依然故意将方法的调用顺序设置为和实际不同。
1. 不使用control，直接创建两个strict mock对象
@Test public void testWithoutControlInWrongOrder() { Business business = new Business(); Service1 service1 = EasyMock.createStrictMock("service1", Service1.class);
第 32 / 65 页
![]() http://skydream.javaeye.com
Service2 service2 = EasyMock.createStrictMock("service2", Service2.class); business.setService1(service1); business.setService2(service2);
1.8 easymock教程-strict和nice
service2.method3(); EasyMock.expectLastCall(); service1.method1(); EasyMock.expectLastCall();
EasyMock.replay(service1, service2); business.executeService1And2(); EasyMock.verify(service1, service2); }
这个测试案例，出于意外的，通过了。easymock并没有检测service1.method1()和service2.method3()这 两个方法的调用顺序。
2. 使用strict control创建两个strict mock对象
@Test public void testWithStrictControlInWrongOrder() { Business business = new Business(); IMocksControl mocksControl = EasyMock.createStrictControl(); ... }
案例失败，错误信息为： java.lang.AssertionError: Unexpected method call service1.method1(): service2.method3(): expected: 1, actual: 0 at org.easymock.internal.MockInvocationHandler.invoke(MockInvocationHandler.java:45) at org.easymock.internal.ObjectMethodsFilter.invoke(ObjectMethodsFilter.java:73) at net.sourcesky.study.easymock.tutorial.$Proxy4.method1(Unknown Source) at net.sourcesky.study.easymock.tutorial.OrderTest$Business.executeService1And2(OrderTest.java:19) at net.sourcesky.study.easymock.tutorial.OrderTest.testWithStrictControlInWrongOrder(OrderTest.java:218) ......
第 33 / 65 页
![]() http://skydream.javaeye.com
1.8 easymock教程-strict和nice
OK，easymock终于检测到service1.method1()和service2.method3()这两个方法的调用顺序和期望的不一 致了。
解释一下，EasyMock.createStrictMock()方法实际上内部是生成一个新的strict control，然后再创建mock 对象。 Service1 service1 = EasyMock.createStrictMock("service1", Service1.class); Service2 service2 = EasyMock.createStrictMock("service2", Service2.class);
这里实际是创建了两个strict control，而easymock是不会跨control进行顺序检测的。在实际使用过程中， 我们会有大量的场景需要检测多个mock之间的调用顺序(按说如果没有特殊要求，一般的测试场景默认都应该 如此),这种情况下就必须使用control, 而且必须是同一个strict control才能满足要求。
教程后面的最佳实践中有一条就是推荐使用mock control，可以跨mock对象检测方法调用顺序是一个重要 原因。
第 34 / 65 页
![]() http://skydream.javaeye.com
1.9 easymock教程-创建stub对象
1.9 easymock教程-创建stub对象
发表时间: 2010-11-23
前面教程中有个章节讨论到mock和stub的概念差别，一般来说easymock如其名所示，主要是用来做mock 用的，但是easymock中也提供有对stub的支持, 主要体现在 andStubAnswer(),andStubDelegateTo(),andStubReturn(),andStubThrow()和asStub()等方法的使用上。
我们来看一个实际使用的例子：
public class Business { private Service service;
private StubService stubService;
public void execute1() { if (service.execute()) { stubService.method1(); } }
public void execute2() { if (service.execute()) { stubService.method2(); } }
public void execute3() { if (service.execute()) { stubService.method1(); stubService.method2(); } }
第 35 / 65 页
![]() http://skydream.javaeye.com
public void setStubService(StubService stubService) { this.stubService = stubService; }
1.9 easymock教程-创建stub对象
public void setService(Service service) { this.service = service; } }
private interface Service { public boolean execute(); }
private interface StubService {
public String method1();
public String method2(); }
这里的Business类依赖到Service和StubService，execute1() / execute2() / execute3() 是我们需要测试的 三个方法，相同点都是必须调用service.execute()，不同点在于其后对stubService的调用各不相同。而我们假 设在这里我们只关心Business类对Service的调用是否如预期，不关心对于StubService的调用，只要程序可以 继续运行就可以了。
一个正统的做法是手工写一个StubService的stub 实现，例如：
private class StubServiceImpl { public String method1() { return ""; }
public String method2() { return ""; }
第 36 / 65 页
![]() http://skydream.javaeye.com
}
1.9 easymock教程-创建stub对象
但是如果这个接口复杂方法众多，则这个stub类不得不实现所有的其他方法，即使完全用不到，因为java的 语法限制。
可以这样使用easymock来实现stub：
public class BusinessTest {
private Business
business;
private IMocksControl mocksControl;
private Service
service;
@Before public void init() { business = new Business(); business.setStubService(prepareStubService()); mocksControl = EasyMock.createStrictControl(); service = mocksControl.createMock("service", Service.class); business.setService(service); }
private StubService prepareStubService() { StubService service = EasyMock.createMock("stubService", StubService.class); service.method1(); EasyMock.expectLastCall().andStubReturn(""); service.method2(); EasyMock.expectLastCall().andStubReturn("");
第 37 / 65 页
![]() http://skydream.javaeye.com
EasyMock.replay(service); return service; }
1.9 easymock教程-创建stub对象
@Test public void testExecute1() { EasyMock.expect(service.execute()).andReturn(true); mocksControl.replay(); business.execute1(); mocksControl.verify(); }
@Test public void testExecute2() { EasyMock.expect(service.execute()).andReturn(false); mocksControl.replay(); business.execute2(); mocksControl.verify(); }
@Test public void testExecute3() { EasyMock.expect(service.execute()).andReturn(true); mocksControl.replay(); business.execute3(); mocksControl.verify(); } }
在方法prepareStubService()中，我们通过easymock创建了一个mock对象，然后和普通mock对象一样记 录了期望的行为。不同的是用andStubReturn()替代了andReturn().
然后我们直接调用EasyMock.replay(service)，注意在这个测试案例中，我们另外创建了一个mocksControl 并通过这个mocksControl创建了我们关注的Service接口的mock对象，它的record/replay/和verify()是和 StubService完全分离的。这样做的好处是在execute1() / execute2() / execute3()的测试案例中，我们完全不
第 38 / 65 页
![]() http://skydream.javaeye.com
1.9 easymock教程-创建stub对象
必额外关心这个stub，所有的事情在init()函数中就已经准备好了。这样做的好处显而易见，execute1() / execute2() / execute3()的测试案例中，代码和测试逻辑都简单了。
最后总结，在适当的时候使用easymock来创建stub对象，对于简化测试还是能有所帮助的。
第 39 / 65 页
![]() http://skydream.javaeye.com
1.10 easymock教程-放宽调用次数
1.10 easymock教程-放宽调用次数
发表时间: 2010-11-29 对于mock对象上的mock方法的调用,easymock支持指定次数，默认为1，例如
Easymock.expect(mock.method1()).andReturn(...);
这里没有显式的指定调用次数，因此效果等同于
Easymock.expect(mock.method1()).andReturn(...).once();
同时easymock提供了其他的方法，用于指定具体调用次数或者放宽调用次数检验。
1. once() 如果明确调用次数为1，则可以使用这个方法显式指定，也可以省略，easymock默认为1。
2. atLeastOnce() 指定调用为1次或者多次，即 count >= 1.
3. anyTimes() 容许调用次数为任意次，即 count >= 0.
4. times(int count) 直接指定调用次数
5. times(int min, int max) 这个方法比较灵活，可以指定最小次数和最大次数。
第 40 / 65 页
![]() http://skydream.javaeye.com
1.10 easymock教程-放宽调用次数
其他的方法都可以视为这个方法的变体，比如 once()等价于times(1,1) atLeastOnce()等价于times(1,Integer.MAX_VALUE) anyTimes()等价于times(0,Integer.MAX_VALUE) times(int count)等价于times(count,count)
实际使用中根据具体要求可以灵活选用上述方法来指定我们期望的调用数次。
第 41 / 65 页
![]() http://skydream.javaeye.com
1.11 easymock教程-参数匹配
1.11 easymock教程-参数匹配
发表时间: 2010-11-29 easymock中提供了非常多的方法来实现参数匹配，基本能满足一般参数匹配的要求。 我们来具体看一下到底有哪些方法：
(1) 基于基本类型的比较
1. eq(X value)方法, X 可以是boolean,byte,char, double,float,int,long,short,T 有多个重载方法，支持基本类型如boolean, byte,char, double,float,int, long,short，后面会介绍它也 支持Object比较。 这个eq()方法的用法直接了当，基本数值直接比较数值，对于非整型的double和float，由于存在精度 的问题，因此增加了以下两个方法来指定比较精度。
eq(double value, double delta) eq(float value, float delta)
2. aryEq(X[] values) X 可以是boolean,byte,char, double,float,int,long,short,T 这个是eq(X value)方法的数组版本，要求比较的两个数组拥有相同的长度，然后每个元素都"相同"，即 都可以满足eq(X value)方法。 注意到double和float并没有像eq(X value)方法那样提供可以设置精度的重载版本，不知道在数组比较 时如何去设置容许精度。
3. gt(X value), lt(X value), X 可以是byte,double,float,int,long,short 这两个方法用于参数的大小匹配，适用于数值型的基本类型如byte,double,float,int,long,short。
4. geq(X value), leq(X value)
第 42 / 65 页
![]() http://skydream.javaeye.com
1.11 easymock教程-参数匹配
类似gt()和lt()，无非是将">"改为">=", "<"改为"<="。
5. anyX(), X可以是Boolean, Byte, Char, Double, Float, Int, Long, Short 这是一个宽松的匹配方法，任何数值都被视为匹配OK。这个方法在我们不介意参数值时特别有用。
(2) 基于对象的比较
1. eq(T value)方法 和基本类型类似，不过对于Object，是通过调用equals()方法来进行比较。
2. same(T value) 方法 和eq()不同，same()是通过比较对象引用来进行比较的。类似java代码中, a.equals(b)和a == b的差 别。
3. anyObject() 和 anyObject(Class<T> clazz) 类似基本类型的any***()方法，非常宽松，在我们不介意参数值时使用。 使用方式有三种: (T)EasyMock.anyObject() // 强制类型转换 EasyMock.<T> anyObject() // 固定返回的泛型 EasyMock.anyObject(T.class) // 在参数中指定返回的泛型
4. isA(Class<T> clazz) 和anyObject(Class<T> clazz) 非常，唯一一个差别在于当输入参数为null时，anyObject(Class<T> clazz)返回true而isA(Class<T> clazz) 返回false。
第 43 / 65 页
![]() http://skydream.javaeye.com
1.11 easymock教程-参数匹配
(3) 逻辑计算 easymock支持在参数匹配时进行一些简单的逻辑计算, 如and(), or (), not()。
not()容易理解，取反而已。or()也容易理解，两个匹配方法匹配一个即可。而and()匹配方法通常用于设置取 值区间，典型如and(gt(0), lt(5))) 的写法可以设置期望值大于0而小于5，即(0,5)区间。
此外在参数匹配中，有几个特殊角色，享受的待遇与众不同，easymock为它们提供了专有方法。
1. Comparable 对于实现了Comparable接口的对象，easymock提供了一系列的专用方法来处理，包括eq, gt, lt, geq, leq：
cmpEq(Comparable<T> value) gt(Comparable<T> value) lt(Comparable<T> value) geq(Comparable<T> value) leq(Comparable<T> value) 这个特殊处理非常合理，本来Comparable接口就提供了比较的功能，在参数匹配时应该容许直接使用。
2. string 由于字符串匹配使用的场景非常多，因此easymock为此也提供了几个常见的参数匹配方法：
contains(String substring) startsWith(String prefix) endsWith(String suffix) find(String regex) 其中contains/startsWith/endsWith是简单的字符串查找，而find()则通过支持正则表达式来提供复杂匹配。
第 44 / 65 页
![]() http://skydream.javaeye.com
1.11 easymock教程-参数匹配
3. null 对于Object匹配，很常见的一个场景就是输入的参数为null，easymock中提供isNull() 和 notNull() 两个 方法来完成对null值的匹配。
开发中，经常会遇到下面这种场景，期望输入的参数满足isA()或者容许为null。而直接使用isA()，是不能 支持null的，即如果参数为null时isA()会报不匹配。这个不是easymock的bug，而是刻意而为，解决的方法是 使用 or(isA(...), isNull(...))或者anyObject()。
service.execute((ClassA) EasyMock.or(EasyMock.isA(ClassA.class), EasyMock.isNull())); service.execute(EasyMock.anyObject(ClassA.class));
第 45 / 65 页
![]() http://skydream.javaeye.com
1.12 easymock教程-partial class mocking
1.12 easymock教程-partial class mocking
发表时间: 2010-11-30 easymock中提供对于类的mock功能，我们可以方便的mock这个类的某些方法，指定预期的行为以便测试 这个类的调用者。这种场景下被mock的类在测试案例中扮演的是次要测试对象或者说依赖的角色，主要测试对 象是这个mock类的调用者。但是有时候我们需要将这个测试类作为主要测试对象，我们希望这个类中的部分 (通常是大部分)方法保持原有的正常行为，只有个别方法被我们mock掉以便测试。
1. 使用方法
我们先来看看这个partial class mocking 是如何工作的：
public class Service {
public void execute() { actualMethod(); needMockMethod(); }
void actualMethod() { System.out.println("call actualMethod()"); }
public void needMockMethod() { System.out.println("call needMockMethod()"); }
}
我们给出了一个非常简单的类，我们将要测试execute()方法，期望能测试到actualMethod()这个方法的正常 行为，然后需要mock掉needMockMethod().
public class PartialClassMockTest extends Assert {
第 46 / 65 页
![]() http://skydream.javaeye.com
@Test public void testPartialMock() {
1.12 easymock教程-partial class mocking
Service service = EasyMock.createMockBuilder(Service.class).addMockedMethod("needMockMethod").createMock(); service.needMockMethod(); EasyMock.expectLastCall();
EasyMock.replay(service); service.execute(); EasyMock.verify(service); } }
上面的测试案例运行通过，输出为"call actualMethod()"，没有"call needMockMethod()"，说明我们设置 的mock生效了。我们创建的mock类的确是只有部分我们制定的方法是mock的，其他都是正常行为。
再来看看为什么我们要需要partial class mocking 这个功能？为什么需要mock掉其中的一个方法？
我们来看看下面这个更加真实的例子：
public class Service {
public String execute2() { return getConfiguration(); }
public String getConfiguration() { return Configuration.getUsername(); } }
public class Configuration { public static String getUsername() { //ignore the code to get configuration from file or database return "username";
第 47 / 65 页
![]() http://skydream.javaeye.com
} }
1.12 easymock教程-partial class mocking
这里例子中，需要测试的 execute2()方法需要调用getConfiguration()方法，而getConfiguration()方法则 调用了Configuration的静态方法来获取配置信息。我们假设读取配置的代码比较复杂不能直接在单元测试环境 下运行，因此通过情况下这里的execute2()方法就会因为这个getConfiguration()而造成无法测试。因此我们可 以考虑通过partial class mocking的功能来mock掉getConfiguration()方法从而使得我们的测试案例可以覆盖 到execute2()方法
@Test public void testStaticMethod() {
Service service = EasyMock.createMockBuilder(Service.class).addMockedMethod("getConfiguration").createMock(); EasyMock.expect(service.getConfiguration()).andReturn("abc");
EasyMock.replay(service); assertEquals("abc", service.execute2()); EasyMock.verify(service); }
这个测试案例可以正常通过，我们通过partial class mocking成功的避开了getConfiguration()这个绊脚 石。
当然这里的实例代码本身就有点问题，应该采用DI的方法将configuration注入进来，而不是在内部通过静态 方法来获取。因此一个建议是在使用partial class mocking功能前，先看看是不是可以通过重构来显改进测试 类。只有当我们有足够充分的不得已的理由时，才使用partial class mocking这种变通(或者说取巧)的方式来解 决问题。
2. 限制
上面两个例子中，我们仔细看看会发现，被mock的方法都是public的。我们试着将方法修改为protected和 default，partial class mocking依然生效。但是修改为private之后，则抛出异常：
第 48 / 65 页
![]() http://skydream.javaeye.com
1.12 easymock教程-partial class mocking
java.lang.IllegalArgumentException: Method not found (or private): needMockMethod at org.easymock.internal.MockBuilder.addMockedMethod(MockBuilder.java:75) at net.sourcesky.study.easymock.tutorial.PartialClassMockTest.testPartialMock(PartialClassMockTest.java:52)
或者将mock的方法继续保持public，但是加上final，则抛出以下异常：
java.lang.IllegalStateException: no last call on a mock available at org.easymock.EasyMock.getControlForLastCall(EasyMock.java:521) at org.easymock.EasyMock.expectLastCall(EasyMock.java:512) at net.sourcesky.study.easymock.tutorial.PartialClassMockTest.testPartialMock(PartialClassMockTest.java:54)
我们回到之前的章节，class mocking里面讲述了class mocking的一些限制：private方法和final方法是不能 mock的。partial class mocking下这些限制依然存在。因此，为了开启partial class mocking，我们不得不稍 微破坏一下类的封装原则，对于原本应该是private的方法，修改为protected或者default。 不得不再次申明，partial class mocking不是一个足够好的解决方案，它只适合在不得已的情况下使用，不 要太依赖这个特性。重构代码改善代码才是王道。 3. 疑问 另外class mocking中还讲到，对于类的equals(), toString()和hashCode()这三个方法，class mocking下是 easymock为这三个方法内建了easymock的实现，因此也不能mock。而partial class mocking，这三个方法 同样不能mock，但是easymock不再为它们内建实现，而是使用它们正常的功能。
关于这点还是有一点疑问，我在easymock的官方文档中看到以下描述 Remark: EasyMock provides a default behavior for Object's methods (equals, hashCode, toString). However, for a partial mock, if these methods are not mocked explicitly, they will have their normal behavior instead of EasyMock default's one.
第 49 / 65 页
![]() http://skydream.javaeye.com
1.12 easymock教程-partial class mocking
言下之意，似乎equals, hashCode, toString这三个方法还是可以显式mock的。但是我测试了一下：
public class Service {
public String execute3() { actualMethod(); return toString(); }
@Override public String toString() { return "defaultToString()"; } }
@Test public void testToStringMethod() { Service service = EasyMock.createMockBuilder(Service.class).addMockedMethod("toString").createMock(); EasyMock.expect(service.toString()).andReturn("abc"); EasyMock.replay(service); assertEquals("abc", service.execute3()); EasyMock.verify(service); }
toString()方法的mock没能生效，抛出异常:
java.lang.IllegalStateException: no last call on a mock available at org.easymock.EasyMock.getControlForLastCall(EasyMock.java:521) at org.easymock.EasyMock.expect(EasyMock.java:499) at
net.sourcesky.study.easymock.tutorial.PartialClassMockTest.testToStringMethod(PartialClassMockTest.java:
可以看到明显是EasyMock.expect(service.toString()).andReturn("abc"); 这里的record没有成功。
第 50 / 65 页
![]() http://skydream.javaeye.com
1.13 easymock教程-运行时返回值或者异常
1.13 easymock教程-运行时返回值或者异常
发表时间: 2010-11-30 前面的教程中，我们看到easymock可以通过expect方法来设定mock方法的返回值或者异常，但是注意这 些案例中设置的返回值都是在调用被测试的类的方法前就已经确定下来的，即我们其实在测试类的代码运行前 (实际是在EasyMock.replay()方法调用前)就已经"预知"了返回结果。
但是在某些情况下，我们可能无法预知返回值，比如我们需要根据输入的参数值来决定返回什么，而这个参 数可能无法在record阶段获得。因此在mock方法中我们无法在record阶段就决定应该返回什么。
对于这种场景，easymock提供了IAnswer接口和andAnswer()方法来提供运行时决定返回值或者异常的机 制。
我们来看一个简单的例子：
public class Business { private Service service;
public void execute() { int count = ramdonInt(); int result = service.execute(count); }
public void setService(Service service) { this.service = service; }
private int ramdonInt() { Random random = new Random(); return random.nextInt() / 10000; } }
第 51 / 65 页
![]() http://skydream.javaeye.com
public interface Service { public int execute(int count); }
1.13 easymock教程-运行时返回值或者异常
在Business的execute()方法中，需要调用service.execute(int count)方法，而传入的参数count是需要运行 时才能确定的，这里为了简单我们random了一个int来模拟这种情况。
然后看测试案例
@Test public void testRuntimeReturn() { Business business = new Business(); Service service = EasyMock.createMock(Service.class); business.setService(service); EasyMock.expect(service.execute(EasyMock.anyInt())).andAnswer(new IAnswer<Integer>() { public Integer answer() throws Throwable { Integer count = (Integer) EasyMock.getCurrentArguments()[0]; return count * 2; } });
EasyMock.replay(service); business.execute(); EasyMock.verify(service); }
这里我们通过EasyMock.expect(service.execute(EasyMock.anyInt()))来接受任意值的count参数输入， andAnswer(new IAnswer<Integer>() {}) 让我们可以指定一个IAnswer的实现类来给出返回值。在这个 IAnswer的实现类中，我们通过EasyMock.getCurrentArguments()[0]获取到service.execute()方法的第一个 参数，然后简单的运用count*2规则给出返回值。这里的EasyMock.getCurrentArguments()方法可以获取到 运行时的参数列表，不过注意这个方法对重构不够友好，如果参数列表发生变化则必须手工修改对象的获取参 数的代码。
下面是一个运行时抛出异常的例子，简单起见我们通过设置exception的message来在错误信息中传递运行 时的count值。
第 52 / 65 页
![]() http://skydream.javaeye.com
@Test public void testRuntimeException() { Business business = new Business(); Service service = EasyMock.createMock(Service.class); business.setService(service);
1.13 easymock教程-运行时返回值或者异常
EasyMock.expect(service.execute(EasyMock.anyInt())).andAnswer(new IAnswer<Integer>() { public Integer answer() throws Throwable { Integer count = (Integer) EasyMock.getCurrentArguments()[0]; throw new RuntimeException("count=" + count); } });
EasyMock.replay(service); try { business.execute(); fail("should throw RuntimeException"); } catch (RuntimeException e) { assertTrue(e.getMessage().indexOf("count=") != -1); //get count from message EasyMock.verify(service); } }
除了IAnswer接口外，easymock中还有另外一个方式可以完成类似的功能，就是使用andDelegate()方法，
public class ServiceStub implements Service { public int execute(int count) { return count * 2; } }
@Test public void testRuntimeReturn() { Business business = new Business(); Service service = EasyMock.createMock(Service.class); business.setService(service); EasyMock.expect(service.execute(EasyMock.anyInt())).andDelegateTo(new ServiceStub());
第 53 / 65 页
![]() http://skydream.javaeye.com
EasyMock.replay(service); business.execute(); EasyMock.verify(service); }
1.13 easymock教程-运行时返回值或者异常
这里需要先创建一个Service类的实现类和一个实例，然后通过andDelegateTo()将这个stub的实例传进去， 注意这里delegate进去的实例必须是mock对象接口相同。
delegateTo方式实际上是我们手工创建了stub(mock和stub的概念及差别请参考本教程的"mock和stub"一 文)，这和我们使用easymock的初衷有所违背。而且当这个接口有众多方法时，创建这样一个stub会显得很痛 苦，不如使用IAnswer方便直接。
第 54 / 65 页
![]() http://skydream.javaeye.com
1.14 easymock教程-改变同一个方法调用的行为
1.14 easymock教程-改变同一个方法调用的行为
发表时间: 2010-11-30 在easymock中，对于mock对象的同一个方法，可以为每一次的调用定制不同的行为。在record阶段 easymock会精确的记录我们录入的行为，基于每一次的方法调用。
这里有一个官网文档中的例子：
expect(mock.voteForRemoval("Document")) .andReturn((byte) 42).times(3) .andThrow(new RuntimeException()).times(4) .andReturn((byte) -42);
对于mock.voteForRemoval("Document")方法的调用，.andReturn((byte) 42).times(3) 表明前3次调用将 返回42，.andThrow(new RuntimeException()).times(4)表示随后的4次调用(第4,5,6,7次)都将抛出异常， andReturn((byte) -42)表示第8次调用时将返回-42。
我们来验证一下:
public class ChangeBehehaviorTest extends Assert { private static interface Service { public byte voteForRemoval(String name); }
@Test public void testExecute() { final String name = "Document";
Service mock = EasyMock.createMock("service", Service.class);
EasyMock.expect(mock.voteForRemoval("Document")).andReturn((byte) 42).times(3).andThrow(new RuntimeException .times(4).andReturn((byte) -42);
第 55 / 65 页
![]() http://skydream.javaeye.com
EasyMock.replay(mock);
1.14 easymock教程-改变同一个方法调用的行为
for (int i = 0; i < 3; i++) { assertEquals(42, mock.voteForRemoval(name)); } for (int i = 0; i < 4; i++) { try { mock.voteForRemoval(name); fail("should throw RuntimeException"); } catch (RuntimeException e) {
} } assertEquals(-42, mock.voteForRemoval(name)); EasyMock.verify(mock); } }
测试案例顺利通过。
第 56 / 65 页
![]() http://skydream.javaeye.com
1.15 easymock教程-自定义参数匹配器
1.15 easymock教程-自定义参数匹配器
发表时间: 2010-11-30 虽然easymock中提供了大量的方法来进行参数匹配，但是对于一些特殊场合比如参数是复杂对象而又不能 简单的通过equals()方法来比较，这些现有的参数匹配器就无能为力了。easymock为此提供了 IArgumentMatcher 接口来让我们实现自定义的参数匹配器。
我们还是用例子来说话：
public interface Service { public void execute(Request request); }
service类的execute()方法接收一个Request实例作为参数, Request是一个javabean:
public static class Request {
private boolean condition;
private String value1;
private String value2;
//ignore getter and setter method }
假设在我们的这个单独的测试案例中，我们有以下参数匹配逻辑: 如果condition为true，则只需要比较 value1；如果condition为false，则只需要比较value2. 由于这个逻辑和默认的equals方法不一致，因此我们不 能直接使用equals方法，只能实现自己的参数匹配器。
public class RequestMatcher implements IArgumentMatcher {
private boolean condition;
private String expectedValue;
第 57 / 65 页
![]() http://skydream.javaeye.com
private RequestMatcher(boolean condition, String expectedValue) { this.condition = condition; this.expectedValue = expectedValue; }
1.15 easymock教程-自定义参数匹配器
@Override public void appendTo(StringBuffer buffer) { buffer.append("RequestMatcher expect(condition="); buffer.append(condition); buffer.append(" expectedValue="); buffer.append(expectedValue); buffer.append(")"); }
@Override public boolean matches(Object argument) { if (!(argument instanceof Request)) { return false; }
Request request = (Request) argument; if (condition) { return expectedValue.equals(request.getValue1()); } else { return expectedValue.equals(request.getValue2()); } }
public static Request requestEquals(boolean condition, String expectedValue) { EasyMock.reportMatcher(new RequestMatcher(condition, expectedValue)); return null; } }
RequestMatcher 是我们定义的参数匹配器，matches()方法中是参数匹配逻辑的代码实现，appendTo()方 法用于在匹配失败时打印错误信息，后面我们会演示这个方法的使用。然后是最重要的方法requestEquals()， 在这里我们通过调用EasyMock.reportMatcher()告诉easymock我们要用的参数匹配器。
第 58 / 65 页
![]() http://skydream.javaeye.com
1.15 easymock教程-自定义参数匹配器
在测试案例中，我们和以往一样，先创建了mock对象，然后准备request对象作为测试数据。不同的是，我 们没有使用easymock提供的参数匹配方法，而是通过 service.execute(RequestMatcher.requestEquals(expectedCondition, expectedValue)); 来调用 EasyMock.reportMatcher()，以创建我们自定义的参数匹配器并为它传入了两个必备的参数 expectedCondition和expectedValue。
上面的测试案例可以顺利通过，我们的参数匹配器可以正常工作。然后我们来试试参数匹配不成功的情况
@Test public void testConditionTrueFailure() { final boolean expectedCondition = true; final String expectedValue = "aaa";
Service service = EasyMock.createMock("service", Service.class); Request request = prepareRequest(expectedCondition, "bbb", "ccc"); service.execute(RequestMatcher.requestEquals(expectedCondition, expectedValue)); EasyMock.expectLastCall();
EasyMock.replay(service); service.execute(request); EasyMock.verify(service); }
注意在Request request = prepareRequest(expectedCondition, "bbb", "ccc")中，我们故意设置value为 和期望的不同，当然这样测试案例就通不过了:
java.lang.AssertionError: Unexpected method call service.execute(net.sourcesky.study.easymock.tutorial.IArgumentMatcherTest$Request@10ef90c): service.execute(RequestMatcher expect(condition=true expectedValue=aaa)): expected: 1, actual: 0 at org.easymock.internal.MockInvocationHandler.invoke(MockInvocationHandler.java:45) at org.easymock.internal.ObjectMethodsFilter.invoke(ObjectMethodsFilter.java:73)
第 59 / 65 页
![]() http://skydream.javaeye.com
1.15 easymock教程-自定义参数匹配器
at $Proxy4.execute(Unknown Source) at
net.sourcesky.study.easymock.tutorial.IArgumentMatcherTest.testConditionTrueFailure(IArgumentMatcherT
注意"service.execute(RequestMatcher expect(condition=true expectedValue=aaa)): expected: 1, actual: 0"这行，其中的"RequestMatcher expect(condition=true expectedValue=aaa)"是我们在 appendTo()方法中构建出来的错误信息。appendTo()方法只在这个时候才被调用，用于生成可读性强的错误 信息以便我们在失败时检查，因此不要疏忽了这个方法的实现。
第 60 / 65 页
![]() http://skydream.javaeye.com
1.16 easymock教程-命名mock对象
1.16 easymock教程-命名mock对象
发表时间: 2010-11-29 在创建mock对象的时候，我们可以命名mock对象。
前面我们谈到easymock中有三种mock对象，分别用下面三个方法创建:
createMock(Class<T> toMock) createStrictMock(Class<T> toMock) createNiceMock(Class<T> toMock)
带有命名功能的方法也有对应的三种:
createMock(String name, Class<T> toMock) createStrictMock(String name, Class<T> toMock) createNiceMock(String name, Class<T> toMock)
命名mock对象有什么好处呢？其实就是一点，即在当测试案例因为某个mock对象的状态或行为不符合要求 而失败的时候，在异常信息里面可以输出这个mock对象的名称。
我们用实际的例子来看，同样是创建一个mock对象，然后调用一个没有record的方法，easymock报错退出 的测试案例。
1. 不命名
Service service = EasyMock.createMock(Service.class);
异常信息如下： java.lang.AssertionError: Unexpected method call execute(): at org.easymock.internal.MockInvocationHandler.invoke(MockInvocationHandler.java:45) at org.easymock.internal.ObjectMethodsFilter.invoke(ObjectMethodsFilter.java:73)
第 61 / 65 页
![]() http://skydream.javaeye.com
1.16 easymock教程-命名mock对象
at net.sourcesky.study.easymock.tutorial.$Proxy4.execute(Unknown Source) at net.sourcesky.study.easymock.tutorial.NamingMockTest$Business.execute(NamingMockTest.java:11) at net.sourcesky.study.easymock.tutorial.NamingMockTest.testExecute(NamingMockTest.java:31) ...... 这里的错误信息"Unexpected method call execute()"，只指出了发生错误的是execute()方法，并未指出具 体是哪个mock对象。 当测试案例简单时，比如就一两个mock对象，我们可以直接从方法名上就看出是哪个mock对象。但是当 mock对象多了之后，尤其是方法很多的时候，就会浪费时间。
2. 命名后
Service service = EasyMock.createMock("service", Service.class);
异常信息如下： java.lang.AssertionError: Unexpected method call service.execute(): at org.easymock.internal.MockInvocationHandler.invoke(MockInvocationHandler.java:45) at org.easymock.internal.ObjectMethodsFilter.invoke(ObjectMethodsFilter.java:73) at net.sourcesky.study.easymock.tutorial.$Proxy4.execute(Unknown Source) at net.sourcesky.study.easymock.tutorial.NamingMockTest$Business.execute(NamingMockTest.java:11) at net.sourcesky.study.easymock.tutorial.NamingMockTest.testExecute(NamingMockTest.java:30)
差别只在错误信息中"Unexpected method call serviceMock.execute()" 包含serviceMock的字样，因此我 们可以快递定位到具体的mock对象。
命名mock对象在初次编写测试案例时用处还不是很明显，因为编写者当时肯定对代码很熟悉。但是当另外一 个开发人员因为某此修改代码造成测试案例不能通过时，由于这个开发人员对代码的熟悉程度不够，因此命名 mock对象可以对他有所帮助，方便维护。
第 62 / 65 页
![]() http://skydream.javaeye.com
1.16 easymock教程-命名mock对象
命名之后的mock对象，还有一个特殊的地方就是toString()方法将返回该mock对象的名称，以代码为例:
Service service = EasyMock.createMock("service", Service.class); System.out.println(service.toString());
输出就是"service"，对比不命名mock对象的情况：
Service service = EasyMock.createMock(Service.class); System.out.println(service.toString());
得到的输出是"EasyMock for interface net.sourcesky.study.easymock.tutorial.NamingMockTest$Service".
第 63 / 65 页
![]() http://skydream.javaeye.com
1.17 easymock教程-使用MockControl
1.17 easymock教程-使用MockControl
发表时间: 2010-10-26 在easymock中，通常我们使用一下的代码来创建mock对象
IMyInterface mock = createStrictMock(IMyInterface.class); replay(mock); verify(mock); reset(mock);
如果需要mock多个对象，则需要如此:
IMyInterface1 mock1 = createStrictMock(IMyInterface1.class); IMyInterface2 mock2 = createStrictMock(IMyInterface2.class); IMyInterface3 mock3 = createStrictMock(IMyInterface2.class); ... replay(mock1, mock2, mock3, ...); verify(mock1, mock2, mock3, ...); reset(mock1, mock2, mock3, ...);
不仅需要为每个mock对象增加create语句，而且需要为这个新增的mock对象更新replay()/verify()/reset() 方法，比较啰嗦，而且容易出错。
这种情况下可以考虑使用MocksControl来简化代码：
IMocksControl mocksControl = createControl(); IMyInterface1 mock1 = mocksControl.createMock(IMyInterface1.class); IMyInterface2 mock2 = mocksControl.createMock(IMyInterface2.class); IMyInterface3 mock3 = mocksControl.createMock(IMyInterface3.class); ... mocksControl.replay(); mocksControl.verify(); mocksControl.reset();
IMocksControl接口容许创建多个mock对象，这些创建的对象自动关联到这个mocksControl实例上，以后
第 64 / 65 页
![]() http://skydream.javaeye.com
1.17 easymock教程-使用MockControl
再调用replay()/verify()/reset()时就不需要逐个列举出每个mock对象。当mock对象比较多，尤其是原有代码 上新增mock 对象时非常方便。 事实上，Easymock.createMock()方法内部实现也是使用IMocksControl的:
public static <T> T createMock(final Class<T> toMock) { return createControl().createMock(toMock); }
public static IMocksControl createControl() { return new MocksControl(MocksControl.MockType.DEFAULT); }
除了使用方便外，使用IMocksControl还有另外一个重要的好处，就是如果使用strict control，则可以跨多 个mock对象检测方法的调用顺序，具体的内容请参考本教程中的"strict和nice"一文。
第 65 / 65 页
