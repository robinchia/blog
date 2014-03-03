---
layout: post
title: "项目经理、系统架构师或技术骨干应该具备的水平 - shirlly - JavaEye技术网站"
categories: zhiya
tags: 
 - zhiya
 - 技术之路
--- 

# 项目经理、系统架构师或技术骨干应该具备的水平 - shirlly - JavaEye技术网站

[首页](http://www.javaeye.com/) [新闻](http://www.javaeye.com/news) [论坛](http://www.javaeye.com/forums) [问答](http://www.javaeye.com/ask) [博客](http://www.javaeye.com/blogs) [招聘](http://www.javaeye.com/job) [更多 ▼](http://shirlly.javaeye.com/blog/235061#)

[专栏](http://www.javaeye.com/wiki) [文摘](http://www.javaeye.com/articles) [圈子](http://www.javaeye.com/groups) [搜索](http://www.javaeye.com/google_search)

[您还未登录 !](http://shirlly.javaeye.com/login "登录") [我的应用](http://www.javaeye.com/all) [登录](http://shirlly.javaeye.com/login) [注册](http://shirlly.javaeye.com/signup)

# [shirlly](http://shirlly.javaeye.com/)

永久域名 [http://shirlly.javaeye.com/](http://shirlly.javaeye.com/)

[nvarchar和varchar的区别](http://shirlly.javaeye.com/blog/236725 "nvarchar和varchar的区别") | [用例建模指南](http://shirlly.javaeye.com/blog/234788 "用例建模指南")

2008-08-31

### [项目经理、系统架构师或技术骨干应该具备的水平](http://shirlly.javaeye.com/blog/235061)
版权所有，欢迎转载，转载请注明转自http://www.suneca.com
      一直想写一篇这样的总结性文章，但不是没有时间就是没有勇气写下去，因为怕别人丢臭鸡蛋。这两天有时间，终于鼓起勇气，将这篇文章写来下！也希望对一些正在寻找更好发展的朋友能有点帮助，也希望对于一些技术跟管理方面的牛人，能给予一些建议。
      作为一名项目经理、系统架构师或技术骨干，其水平如何，关系到公司的项目管理、软件质量管理等方面的问题。项目经理或技术骨干应该要起带头作用，使整个团队的开发及管理能达到一种更高的水平。
      那作为一名项目经理或公司技术骨干应该学会那些工具及知识点呢？涉及到这一块的工具及技术点非常多，如何去选择，是摆在项目经理、系统架构师跟技术骨干面前的问题。根据公司及团队的情况，选择合适的工具或技术框架，这一点非常重要。在项目的不同阶段，需要有不同的工具来支持。
      按照软件系统的生命周期的六个阶段，一般分为需求分析阶段、系统设计阶段、系统开发阶段、软件测试阶段、系统发布阶段、系统维护阶段，这几个阶段都需要有不同工具的支持。
一、需求分析阶段：
第一、项目管理及需求管理工具
    项目管理工具很多公司都在使用，为什么要使用这些工具？假如没有使用这些工具，而是使用Excel或Word进行记录，那当需求变更？需求实现情况的跟踪？软件是否能按时交付？将是一件非常烦锁且容易出错的事情。一个软件项目、开发团队能否获得成功，管理非常关键。比较有名的商业化工具有：MicroSoft Project Server及Project 2003、IBM Rational RequisitePro、JIRA、PowerDesinger。比较有名的开源需求管理工具包括：OSRMT(Open Source Requirements Management Tools)、Xplanner、Openworkbench等等。
    很多软件公司都会使用SharePoint，在SharePoint平台上，只要你想得到，基本上都可以通过配置方式来满足你的业务需求。在SharePoint上，可以跟MicroSoft Project Server很好的结合，再配置Project 2003为客户端，进行公司的项目管理。也许对Project操作习惯的问题，在Web界面进行项目管理的时候，总觉得很不方便。
    IBM Rational RequisitePro（http://www.ibm.com ）可以算是最骨灰级的一个软件了，假如你公司整个软件生命周期管理都是采用IBM的解决方案，那使用RequisitePro是一个非常好的解决方案。需要这些软件可以到IBM官方网站上去下载一个最新版本，或者在电驴上面下载一些“特别”版本。设计工具、管理工具的完美结合，这个正是IBM Rational RequisitePro的强项。RequisitePro跟Offce结合得也是非常完美。
    JIRA（http://www.atlassian.com ）原来只是一个缺陷跟踪系统，你可以在JIRA上面创建新的ISSUE，当ISSUE分配给某个程序员时，系统会自动发送一封邮件给该程序员，提示有新的BUG。JIRA也有提供一个Eclipse插件，你可以在Eclipse上面，查到属于自己的ISSUE，并快速解决。现在JIRA也可以用来做项目管理，在操作方面非常人性化，个人一直非常喜欢使用JIRA来进行项目管理、缺陷管理，再结合Eclipse，简直就是完美！但作为商业的软件，价格也非常贵，互联网上也有很多Crack，大家有兴趣也可以搜一下。
    OSRMT（http://sourceforge.net/projects/osrmt ）是一个开源的需求管理工具，分为客户端跟服务器，也提供了一个安装界面供用户安装，做开源的已经算是做得非常完美了。当前最新版本是V1.5，有兴趣的朋友可以下载一个最新版本玩一下，操作还算是挺人性化的。
    Xplanner（http://www.xplanner.org ）是一个开源的，基于XP编程的项目管理软件，它可以帮我们生成一些统计图表。这个软件从06年底发布0.7b7版后，就再也没有更新过了，我对开源工具的看法就是：版本号没有超过1.0版，我都不会应用于生产！对于Xplanner，也是停留在试用的阶段。
    Openworkbench（http://www.openworkbench.org ）也是一个开源的项目管理软件，其功能跟Project 2003相似，是一个值得大家去使用的一个工具，但对于中国很多软件公司，都是使用特别版的Project 2003。假如你很尊重版权，又不想使用Project 2003，那Openworkbench是一个非常好的选择。
第二、需求分析工具
    需求分析工具用得比较多可能就是Rational Rose、MicroSoft Visio或MindManager，一般我们使用Rational Rose来进行用例分析，画用例图，画状态图；使用MicroSoft Visio来画出应用系统的结构图、流程图等。当然，对于MicroSoft Visio能画出来的东西，其实Rose也一样可以实现，只是，大家都是这么干，我们也没有必要专门去做一些特例的东西，特别是对于一些比较特殊的公司及行业。
    Ration Rose 2003是一个值得怀念的工具，至今还是有很多公司跟个人都是使用，个人觉得这个软件版本算是最经典的一个，但对于现在所见即所得的要求下，使用Rose 2003，可能没有办法满足你，因为它需要经过一些小操作才能满足你的要求。但不可否认，它是一个非常优秀的软件。现在对于一些喜欢使用新工具新技术的程序员，也许现在他们正在使用RSA。
    MicroSoft Visio（http://www.microsoft.com ）是每个搞设计的人都会用的一个工具，我们一般使用Visio来画系统结构图、关键流程图、系统部署结构图等。MicroSoft Visio也提供了UML的功能，可以用它来画用例图、类图、状态图，时序图等，但一般这个功能很少使用。至少我基本上不用。
    MindManager（http://www.mindjet.com ）是一个非常好用的工具，我们用来描述我们的思维，很多人都不喜欢通过软件来描述，而是通过一张纸，然后在上面进行涂鸦，接着跟客户或团队进行思维沟通。MindManager很好地解决了这个问题。MindManager跟Office结合得非常完美，可以生成Word、Excel、PDF等文件。这个工具是我一直在使用的一个软件，非常好用。最新版本为7，大家有兴趣可以下载一个试用一下，也可以在网搜搜索一些“特别”版本。
二、系统设计阶段：
第一、系统设计工具
    主流的系统设计工具有大家非常熟悉的Rose2003，不过，现在已经不叫Rose了，现在IBM最新的设计工具是RSA（Ration Software Architect），Borland Together，SyBase PowerDesinger，MicroSoft Visio，对于开源的系统设计工具也有很多，比如ArgoUML、DBDesigner等等。
    RSA（http://www.ibm.com ）：IBM最新的设计工具，它是一个基于Eclipse平台的一个工具，对于你使用RSA，那也许你会将你的整个团队的工具都采用IBM的整套解决方案，使用RequisitePro来进行需求管理、使用RSA来进行建模、使用ClearCase来进行配置管理、使用ClearQuest来进行缺陷跟踪、使用RFT(Rational Functional Tester)来进行测试……RSA有一个最大的优点，那就是跟Word结合得非常好。这一点可以肯定。
    Together（http://www.borland.com ）：Borland公司的NB的设计工具，Together 2006版本也是一个基于Eclipse平台的软件，功能也是非常强大，其所见所得的功能，是我非常喜欢它的一个原因。还有一个原因就是基于Eclipse平台，这个可以跟我的开发工具很完美地整合在一起。不过，整合要注意一个问题，那就是Eclipse兼容性问题，这一点是非常烦人的。
    PowerDesigner（http://www.sybase.com ）： PowerDesigner是“一站式”建模与设计解决方案，物理数据模型的数据库平台无关性，所见即所得，反向工程，报表生成等等功能，使得它成为数据库设计人员心目中最好的产品，它的易用性深深地吸引了我！特别它的Repository模型库的功能，更让我们实现了模型设计的版本控制。最新的PowerDesigner，使得我觉得它是一件艺术品。做设计的人员一般会使用PowerDesigner来进行数据库物理模型设计，它是我心目中的首选工具。之前曾经对比过RSA、Together、ERWin的数据库模型设置工具，最终我还是更加喜欢使用PowerDesigner，也许，我的操作习惯已经被PowerDesigner腐蚀。
第二、开发的技术框架
    技术框架的选择是非常关键，一个好的技术框架，可以让我们的开发更加快速、团队的分工更加合理、系统能够支持多种数据库平台、我们的维护更加方便。
    Web前端MVC框架是Struts 2。Struts 2可以说是Struts穿上了WebWork的外衣，其内核大部分都是采用了WebWork的技术，并且基于AOP的设计思想，让我们在软件设计上的能够更加多地体现“高内聚，低耦合”的设计思想。
    J2EE框架是Spring，作为一个开源的J2EE框架，虽然它没有太多的新技术点，但它的整合性，拿得我们的开发更加简单，IOC、AOP、事务处理、开源框架的整合支持等等，使得作为一个J2EE框架的首选。
    持久层框架是Hibernate，作为一个开源的项目，我想，没有一个开源项目的社区能够你Hibernate一样，丰富的文档，活跃的社区，基于Hibernate的开发团队的庞大，使得它作为持久层框架的首先。基于 Hibernate，我们可以开发出数据库平台无关性的产品。但是，Hibernate也有自身的问题，假如使用不当，也许会有所失控，一旦失控，它所带来的，就是性能问题。对于最新的Hibernate3，存储过程的支持，外部SQL的定制，很好地解决了这个问题。但在关联关系上，使用还是要小心为好。
    页面框架，可以多考虑使用DIV技术、JSTL标签库、Struts 2标签库、DWR、AJAX、XML+XSLT等技术来让我们页面更好维护，使用OSCache缓存技术来提高我们页面的访问速度。
第三、开发规范的定制
    文件命名规范、数据库设计规范、编码规范、团队协作规定等等一些规范性的东西，需要在系统开发前就规定好，并且做相应的培训。QA也要做好监督的作用，定期做评审工作，对已发生的问题及可能出现的问题，及早发现，及早处理。
第四、开发工具的选择
    团队一定要选择同样的开发工具，开发工具相同，软件版本相同。为什么要这样子做，其实假如你作为一个Team Leader，你会在管理你的团队的时候发现很多问题，而解决这个问题，那在项目编码前，就把什么东西都规定好，以免其中发生问题，影响整个团队的开发速度。开发工具的选择也是非常重要的，目前企业用得比较多的开发工具有：Eclipse、Jbuilder、NetBeans、IDEA。
    Jbuilder：最新的Jbuilder版本是2007，2007版基本上可以算是重新开发的版本，因为它是基于Eclipse之上的。我算是Borland公司最为忠实的Fans啦，从Jbuilder6，到Jbuilder7，再到Jbuilder8，再到Jbuilder9、Jbuilder X，Jbuilder 2005，Jbuilder 2006，我经常跟我学生说，对于Jbuilder，相信没有人比我更熟悉他了，做Java开发接近6年时间，超过4年的时间，每天都都在使用的工具，Jbuilder见证了我的长成。使用过Jbuilder的人很多人知道一点，就是Jbuilder的盗版问题，安装完Jbuilder之后，假如你一个不小心，没有安装防火墙，那Jbuilder会不时通过8888端口向Borland总部发送一些你的计算机信息，这个是一种非常可怕的“木马”，什么是“木马”？这个就是！这种情况自从Jbuilder X以后就一直有。假如你不怕Borland公司的人跟工商局过来查你公司的软件的话，那选择Jbuilder是一个不错的选择。作为Java IDE开发平台的老大，Jbuilder在企业应用开发是非常有优势的，特别是开发EJB跟WebService，偶只能用一个句来形容，那就是牛。Jbuilder 2007，王者归来，相信对于很多Borland的Fans，还是非常喜欢并乐意去尝试的，不过，价格还是会让很多公司都受不了、速度会让很多程序员也受不了。我的Jbuilder的缘分到2006就基本上已经结束了。现在我的开发环境基本上都是Eclipse。
    Eclipse：IBM捐出来的好东西，发展挺快的，现在已经到了Eclipse3.3，非常好用的一个工具。但Eclipse只是一个基础平台，假如你需要其他的功能，那你需要下载相关的插件进行扩展，下载的插件要注意一下跟Eclipse平台的兼容性问题。Eclipse+MyEclipse（http://www.myeclipseide.com ）是个是很多WEB开发人员都是在采用的一个整合工具，但MyEclipse要钱，如果公司愿意为此支付29.9美元的话，那它是一个非常好的选择；比MyEclipse更上一个档次的还有Exadel（http://www.exadel.com/web/portal/home ），不过，价格贵得离谱，因为它本身就是一家咨询服务公司做出来，主要还是靠咨询服务，培训挣钱，并且，运行时的不稳定，也让我放弃了选择这个插件作为我的开发工具，虽然这个工具真的是很强大。Eclipse+WTP（http://www.eclipse.org ）也是一个非常好的免费的开发工具，从eclipse官方网站上可以下载WTP跟Eclipse整合在一起的工具，现在教学基本上用这个。Lomboz（http://lomboz.objectweb.org/ ）也是一个非常好用的免费J2EE插件，学生用的很多，因为好像有不少书都是用这个进行教学的。通过插件来的扩展本来是一件好事，但当它的版本问题？兼容性问题？安全性问题？语言问题？出现的时候，你就会骂着，为什么不提供一站式开发平台呢？如果你下载了语言包，你会发现，有些地方是中文的，有些地方是英文的，极其丑陋！也许，Eclipse作为一个基础平台，它确实是太基础了。但现在，我们也可以下载一些All-In-One版本的Eclipse，但个人感觉还是不够，很多功能，我们还需要去找插件来进行扩展。也许，Eclipse的决策者认为，作为基础平台，肯定是越简单越好，需要什么就加什么，这样，资源占用会更少。正如东方标准最咨深的平面老师曾宇飞讲过一句话：你会去麦当劳点酸菜鱼吃吗？
    NetBeans：作为Sun公司出品的开发工具，功能一样也是非常强大，不管你是做应用程序开发还是做应用系统开发，NetBeans都是一个不错的选择。NetBeans也跟Eclispe一样，也是一个基础平台，但这个基础平台做得比Eclipse强大很多，基本上你下载一个NetBeans就可以开发应用程序或J2EE应用系统了。并且，NetBeans的中文支持非常好，基本上一个新版本出来，就已经有中文版、英文版跟日语版了。看来，NetBeans的决策者还是比较看好这些人群的。NetBeans的Mobile插件开发J2ME是最快最好用的，至少我个人这么认为。开发J2ME应用产品，我首选的就是NetBeans。目前NetBeans已经发展到6.0的版本了，界面非常华丽，有兴趣的朋友可以下载一个玩一下。NetBeans的下载地址是：http://www.netbeans.org 。
    IDEA：对于IDEA的评价，我只能用六个字来形容，那就是：实用的艺术品。它非常好用，界面非常华丽，相当如果你是一个女性的项目经理或技术牛人，你会喜欢上这件艺术器的。IDEA开发应用程序非常强大，这一点绝对可以肯定。官方提供的插件也非常丰富，当你需要那一方面的功能，基本上都可以找得到，找插件，你只需要在官方插件库里面去找就可以了，并且自动安装，自动更新。作为2003年拿到JavaWorld大将的一个作品，相信，它可以带来很多IDEA的创新。它是属于商业化的工具，价格也只有499美元，而个人买也就249美元，如果你愿意牺牲某些功能，那你完全可以下载一个免费的版本。价格方面，个人觉得完全对得起这件艺术品价值。有兴趣的话可以下载一个试用版玩一下：http://www.jetbrains.com/idea ，小声地说，上一下baidu，插件一下，其实你可以找到很多注册号。
    Ant是apache的一个开源项目，可以从Ant官方网站上下载一个最新的版本：http://ant.apache.com 。虽然该项目虽然现在发展变得非常缓慢，但可以非常肯定地讲，它是一个好东西。我们可以使用ant来对我们整个工程进行编译，打包，单元测试，部署等等，基本上你想得到的东西，Ant可以帮你做得到。Maven（http://maven.apache.com ）是一人比Ant还要强大的工具，现在大有Maven将会代替Ant的趋势，Maven也是项目经理要关注的一个技术点。基本上现在主流的开发工具都提供共了对Ant的支持，有些甚至是依赖，比如：NetBeans，你在NetBeans当中创建一个新的工程，那系统会自动地创建一个ant的运行脚本程序。对于你进行编译、打包、发布，那完全都是依赖于这个ant脚本。我们可以使用Ant来开发一个DailyBuild（微软叫每日产品生成，XP叫持久集成）的流程，来提高我们整个团队的软件开发质量。Ant的使用非常简单，多看手册，多花点心思，那你会做得更好。
三、开发阶段
第一、配置管理工具
代码管理工具有很多，现在公司用得比较多的代码管理工具有CVS、VSS、SVN。
对于一个开发团队只有2-5个人，并且这两三个人是同一间办公室里，那使用VSS是一个非常不错的选择，个人觉得他小团队的管理方面非常好用。个人觉得VSS唯一的缺点就是一个文件当被一个人锁定，那其他人就没有办法进行修改了，当一个文件为多个人所共用且开发团队人数较多时，这种问题将会显示非常严重。VSS客户端跟服务器你都可以从Visio Studio里面找到。
Eclipse的VSS客户端插件：http://vssplugin.sourceforge.net/
    对于一个开发团队有超过5个人，那此如选择CVS或SVN将是一个更好的选择，并且，假如你的团队是分散的，可能不在一间办公室或者根本不在同一个城市，那使用CVS或SVN是一个非常更想的选择。CVS的服务器一般是使用CVSNT或CVSServer。
CVSServer：
Linux for X86：http://ftp.gnu.org/non-gnu/cvs/binary/stable/x86-linux/RPMS/i386/
Window for X86：http://ftp.gnu.org/non-gnu/cvs/binary/stable/x86-woe/
CVSNT：http://www.cvsnt.org/
CVSClient：
    WinCVS：http://www.wincvs.org
    TortoiseCVS：http://www.tortoisecvs.org/
    JBuilder、Eclipse、NetBeans、IDEA集成的CVS客户端
    作为版本管理工具，CVS出现至今，已经有二十个年头，可以说他已经走到了尽头，但可以肯定，它将继续存在着。SVN是作为CVS的代替产品而出现的。现在很多开源组织，都慢慢地转到SVN上，比如Apache跟SourceForge。SVN有着比CVS更强大的功能，比如，它可记录目录的更改，它的性能比CVS会快很多等等。目前SVN慢慢地被企业所接受，但个人觉得其Eclipse的客户端的稳定性还有待提高，也许这个跟Eclipse的版本兼容性有一定关系。但这些不稳定性，让我现在对这个产品的使用还继续停留在试用的阶段。
SVNServer：http://subversion.tigris.org/
SVNClient：
    TortoiseSVN ：http://tortoisesvn.net/
    Eclipse插件：http://subclipse.tigris.org/
目前SVN插件支持包括Eclipse、Jdeveloper、NetBeans等开发工具。
第二、知识库管理工具
团队每一个人在开发的时候都会发现一些问题，最终，有些问题可能没有办法解决，有些问题可以解决。一般情况，大部分问题经过团队成员的共同努力，都是可以解决的，那解决问题的方法，解决问题的步骤，这些都应该形成知识。作为一个团队的Leader，我们必须重视这些知识，因为，这些知识非常有用，它对于一些新手或没有遇到此类问题的同事，能够提供相应的帮助。
    Confluence（http://www.atlassian.com/software/confluence ），跟JIRA来自同一家公司的产品，它跟JIRA可以整合得非常好。我们可以通过JIRA的ISSUE，将该ISSUE上升为一个知识。假如你是使用JIRA来进行项目管理跟缺陷管理，那使用Confluence是一个最佳选择。
    PHPBB（http://www.phpbb.com ），论坛其实也是一个非常好的知识库管理工具，当某一个工程师遇到一些疑难杂症的时候，最终，通过自己的努力或团队其他同事的努力，终于解决问题了。那作为Leader的你，应该鼓励他们将这些知识，写一些文章，然后发布在公司自己的BBS上。供大家参考及讨论。这个是一种很好的方法。记得我以前，我在网上看到一些有用的信息，我就把它保存在我本机的PHPBB上。只可怜，后来电脑被人偷了。贼郁闷。
四、软件测试阶段
第一、缺陷管理工具
    软件你不能保证它永远不会错，只是，有些错误你暂时还没有发现而已；有些错误需要在某些特定的环境下它才会发生。就像Windows，时不时会有一些系统更新文件要求更新。可能这些更新不是错误，只是一些系统安全方面的隐患。这些都可以算是软件系统的缺陷。那这些缺陷我们应该怎么进行管理？怎么进行跟踪呢？现在缺陷管理用得比较多的有两个：第一个是开源的bugzilla，另一个是商业的JIRA。
    Bugzilla（http://www.bugzilla.org ），作为开源界缺陷管理系统的鼻祖，它发展到现在已从98年到现在经有10的时间了。它的开发语言是Perl，这使得它的安装变得很麻烦，Bugzilla可以安装在Windows、Linux、Unix等操作系统上。现在的Eclipse也提供了对它的支持，我们可以在Eclispe平台上，找到应用系统的BUG，功能做得非常强大。如果安装能更加轻松一点，或者提供一个All-In-One版本，那会更好！
    JIRA，作为商为上化的缺陷管理系统，JIRA的价格对得起它的功能。JIRA不只是一个缺陷管理系统，它更是一个集项目管理、缺陷管理、统计分析为一身的工具。这个工具我一直在使用，只是使用一些“特别”版本而已。
第二、软件性能监测工具
    Jprofiler（http://www.ej-technologies.com ）是一个非常好的性能监测工具，使用这个工具，你可以快速发现系统那些模块出现性能瓶颈或算法导致的性能问题；它还可以分析内存泄漏的问题。这个工具也提供了相应的Eclipse插件，让你开发更加快速方便。它支持主流的服务器。
    Borland Optimizeit Suite（http://www.borland.com ）也是一个非常好的性能监测工具，它跟Borland产品整合得非常好。不过，运行这个工具，你最好准备一下，最好有2G内存，否则，本来系统好好的，一运行起来，你机器反而死掉了。
第三、软件性能测试工具
    Ant+Windows计划任务创建公司的DailyBuild自动化测试流程，这个是以前做的一个测试流程。使用这种测试流程，无非一个目的，就是提高公司的软件质量。
    Jmeter（http://jakarta.apache.org/jmeter ），这个工具是apache出品的，作为apache忠实的Fans，我对Jmeter也是很喜欢，使用Jmeter，你可以摸似多用户环境，对应用系统进行测试，测试整个应用系统能够承受的最大并发量。
    LoadRunner（http://www.hp.com ），假如你不知道这个软件，那你肯定做不了测试工程师，这个是最专业的一个软件性能测试工具，它可以模似上千万个用户量来进行压力测试，检测系统能够承受的最大并发量。这个软件我只用过几次，编写脚本，进行测试，使用来讲其实算是比较简单。
五、软件发布
    软件的发布我们会怎么去做呢？我们一般做法就是，将数据库脚本化，包括建表语句、初始化数据等，还有制作WAR文件或EAR文件。然后到客户那边，我们需要将数据库表及数据进行初始化，接着，将WAR或EAR文件发布到应用服务器上。这个也许是我们到客户现在发布经常在做的一件事情。那能不能做得更加简单呢？做法一般有两个，第一个就是使用Ant，编写一个初始化数据库跟发布应用程序模块的Ant脚本，然后到生产机上直接运行该脚本即可；第二个就是制作安装文件，一般用来制作安装文件的有IzPack，这个是用得最多的一个免费工具，你可以使用这个免费工具来制作安装程序。也许客户都习惯了安装程序的安装方式了，制作一个可执的安装程序，有助于提高软件产品化的档次。
六、软件维护阶段
第一、客户CASE跟踪管理工具
    客户CASE跟踪系统相信很多做CISCO公司金牌代理的人都会用过。我们必须在公司内部建立相应的CASE跟踪制度。当用户使用系统的时候，发现一些问题，那我们需要对这些问题进行录入并进行跟踪。像客户呼叫服务系统等等一些商业化的软件外面还是很多的，这些系统其实公司自己开发一个也是很快的。但必须要有。这个也是提高整个公司整体服务形象的一种态度。

[nvarchar和varchar的区别](http://shirlly.javaeye.com/blog/236725 "nvarchar和varchar的区别") | [用例建模指南](http://shirlly.javaeye.com/blog/234788 "用例建模指南")
* 17:00
* 浏览 (579)
* [评论](http://shirlly.javaeye.com/blog/235061#comments) (1)
* 分类: [项目管理](http://shirlly.javaeye.com/category/39981)
* [相关推荐](http://www.javaeye.com/wiki/topic/235061)

### 评论

[]()

1 楼 [javanxedu](http://javanxedu.javaeye.com/) 2009-10-21   [引用](http://shirlly.javaeye.com/blog/235061#)

写得不错，怎么没人评论呢![]()
### 发表评论

您还没有登录，请[登录](http://shirlly.javaeye.com/login)后发表评论(快捷键 Alt+S / Ctrl+Enter)

[![shirlly的博客]( "shirlly的博客: shirlly")](http://shirlly.javaeye.com/)

shirlly

* 浏览: 139131 次
* 性别: ![Icon_minigender_2]( "女")
* 来自: 福州
* ![]()
* [详细资料](http://shirlly.javaeye.com/blog/profile) [留言簿](http://shirlly.javaeye.com/blog/guest_book)

### 搜索本博客
### 最近访客 [>>更多访客](http://shirlly.javaeye.com/blog/user_visits)

[![zyh2002my的博客]( "zyh2002my的博客: ")](http://zyh2002my.javaeye.com/)

[zyh2002my](http://zyh2002my.javaeye.com/)

[![saint1107的博客]( "saint1107的博客: ")](http://saint1107.javaeye.com/)

[saint1107](http://saint1107.javaeye.com/)
[![oaaccc的博客]( "oaaccc的博客: ")](http://oaaccc.javaeye.com/)

[oaaccc](http://oaaccc.javaeye.com/)

[![海阔天空love的博客]( "海阔天空love的博客: it仰望天空")](http://jinjiangyuanlove-126-com.javaeye.com/)

[海阔天空love](http://jinjiangyuanlove-126-com.javaeye.com/)

### 博客分类

* [全部博客 (437)](http://shirlly.javaeye.com/)
* [strust2.0 (13)](http://shirlly.javaeye.com/category/33982)
* [spring3.0 (0)](http://shirlly.javaeye.com/category/33983)
* [hibernate2.0 (5)](http://shirlly.javaeye.com/category/33984)
* [java (12)](http://shirlly.javaeye.com/category/33985)
* [javaScript (51)](http://shirlly.javaeye.com/category/34222)
* [AJAX (8)](http://shirlly.javaeye.com/category/35688)
* [版本控制软件 (4)](http://shirlly.javaeye.com/category/35689)
* [其它 (18)](http://shirlly.javaeye.com/category/35864)
* [CSS (13)](http://shirlly.javaeye.com/category/36352)
* [asp (18)](http://shirlly.javaeye.com/category/37627)
* [Dojo (5)](http://shirlly.javaeye.com/category/39206)
* [项目管理 (2)](http://shirlly.javaeye.com/category/39981)
* [SQLServer (24)](http://shirlly.javaeye.com/category/40305)
* [oracle (24)](http://shirlly.javaeye.com/category/40482)
* [.NET (193)](http://shirlly.javaeye.com/category/41294)
* [openCms (1)](http://shirlly.javaeye.com/category/51134)
* [常识 (36)](http://shirlly.javaeye.com/category/53715)
* [软件架构 (1)](http://shirlly.javaeye.com/category/56674)
* [My SQL (1)](http://shirlly.javaeye.com/category/56773)
### 其他分类

* [我的收藏](http://shirlly.javaeye.com/blog/favorite) (27)
* [我的论坛主题贴](http://shirlly.javaeye.com/blog/topic) (437)
* [我的所有论坛贴](http://shirlly.javaeye.com/blog/post) (7)
* [我的精华良好贴](http://shirlly.javaeye.com/blog/article) (0)

### 最近加入圈子
### 存档

* [2010-02](http://shirlly.javaeye.com/blog/monthblog/2010-02) (3)
* [2010-01](http://shirlly.javaeye.com/blog/monthblog/2010-01) (24)
* [2009-12](http://shirlly.javaeye.com/blog/monthblog/2009-12) (21)
* [更多存档...](http://shirlly.javaeye.com/blog/monthblog_more)

### 最新评论

* [解决FCK插入图片中的上传 ...](http://shirlly.javaeye.com/blog/575317#comments "解决FCK插入图片中的上传中文名图片的时候，名字变乱码")
这个文件在那啊
-- by [sumaolin](http://sumaolin.javaeye.com/)
* [项目经理、系统架构师或技 ...](http://shirlly.javaeye.com/blog/235061#comments "项目经理、系统架构师或技术骨干应该具备的水平")
写得不错，怎么没人评论呢
-- by [javanxedu](http://javanxedu.javaeye.com/)
* [输入框中按回车触发提交事 ...](http://shirlly.javaeye.com/blog/211416#comments "输入框中按回车触发提交事件")
代码不错，一直在寻找这个代码哎~写一个完整版的呗~
-- by [chenglu](http://javazn.javaeye.com/)
* [struts2页面传递list对象 ...](http://shirlly.javaeye.com/blog/227586#comments "struts2页面传递list对象到action[尚未解决的问题]")
可以使用session来传递list
-- by [jirabotuo](http://jirabotuo.javaeye.com/)
* [由编码的不一致造成乱码的 ...](http://shirlly.javaeye.com/blog/366400#comments "由编码的不一致造成乱码的参考解决方案")
很明显呀, utf-8 和gbk 吧. 默认字符集不一样面已
-- by [iwlk](http://iwlk.javaeye.com/)
### 评论排行榜

* [控制按钮中的字在垂直方向上的位置](http://shirlly.javaeye.com/blog/344588 "控制按钮中的字在垂直方向上的位置")
* [由编码的不一致造成乱码的参考解决方案](http://shirlly.javaeye.com/blog/366400 "由编码的不一致造成乱码的参考解决方案")
* [解决FCK插入图片中的上传中文名图片的时候 ...](http://shirlly.javaeye.com/blog/575317 "解决FCK插入图片中的上传中文名图片的时候，名字变乱码")
* [sql 2000 附加数据库 错误602](http://shirlly.javaeye.com/blog/327936 "sql 2000 附加数据库 错误602 ")
* [FCK插入图片的时候提示无权限解决方法有两 ...](http://shirlly.javaeye.com/blog/580352 "FCK插入图片的时候提示无权限解决方法有两种")

* [![Rss]()](http://shirlly.javaeye.com/rss)
* [![Rss_google]()](http://fusion.google.com/add?feedurl=http://shirlly.javaeye.com/rss)
* [![Rss_xianguo]()](http://www.xianguo.com/subscribe.php?url=http://shirlly.javaeye.com/rss)
* [[什么是RSS?]](http://www.google.com/search?hl=zh-CN&q=RSS)
声明：JavaEye文章版权属于作者，受法律保护。没有作者书面许可不得转载。若作者同意转载，必须以超链接形式标明文章原始出处和作者。
© 2003-2009 JavaEye.com. All rights reserved. 上海炯耐计算机软件有限公司 [ 沪ICP备05023328号 ]
