---
layout: post
title: "WordPress - 3.6.1 PHP 对象注入漏洞 - WooYun知识库"
categories: 安全
tags: 
 - 安全
 - PHP-WordPress
--- 

# WordPress < 3.6.1 PHP 对象注入漏洞 | WooYun知识库

### 分享到

* [一键分享]()
* [QQ空间]()
* [新浪微博]()
* [百度云收藏]()
* [人人网]()
* [腾讯微博]()
* [百度相册]()
* [开心网]()
* [腾讯朋友]()
* [百度贴吧]()
* [豆瓣网]()
* [搜狐微博]()
* [百度新首页]()
* [QQ好友]()
* [和讯微博]()
* [更多...]()

[百度分享]()

# [WooYun知识库](http://drops.wooyun.org/ "WooYun知识库")

[登录](http://drops.wooyun.org/login "登录")

像一朵乌云一样成长

* [首页](http://drops.wooyun.org/ "WooYun知识库")
* [WooYun](http://www.wooyun.org/)
* [Zone](http://zone.wooyun.org/)
* [投稿](http://drops.wooyun.org/newsend)
[首页](http://drops.wooyun.org/ "首页") » [漏洞分析](http://drops.wooyun.org/category/papers "查看 漏洞分析 中的全部文章") » WordPress < 3.6.1 PHP 对象注入漏洞

## [WordPress < 3.6.1 PHP 对象注入漏洞](http://drops.wooyun.org/papers/596 "Permanent Link to WordPress < 3.6.1 PHP 对象注入漏洞")

9人收藏 [收藏]()

2013/09/13 11:36  | [五道口杀气](http://drops.wooyun.org/author/五道口杀气 "由 五道口杀气 发布")  | [漏洞分析](http://drops.wooyun.org/category/papers "查看 漏洞分析 中的全部文章")  | [占个座先]()

From:[WordPress < 3.6.1 PHP Object Injection](http://vagosec.org/2013/09/wordpress-php-object-injection/)

## 0x00 背景

当我读到一篇关于Joomla的“PHP对象注射”的漏洞blog后，我挖深了一点就发现Stefan Esser大神在2010年黑帽大会的文章：

[http://media.blackhat.com/bh-us-10/presentations/Esser/BlackHat-USA-2010-Esser-Utilizing-Code-Reuse-Or-Return-Oriented-Programming-In-PHP-Application-Exploits-slides.pdf](http://media.blackhat.com/bh-us-10/presentations/Esser/BlackHat-USA-2010-Esser-Utilizing-Code-Reuse-Or-Return-Oriented-Programming-In-PHP-Application-Exploits-slides.pdf)

这篇文章提到了PHP中的unserialize函数当操作用户生成的数据的时候会产生安全漏洞。

所以呢，基本来说，unserialize函数拿到表现为序列化的数据，然后就解序列化它（unserialize嘛，当然就干这个啊~）为php的值。这个值它可以是resource之外的任何类型，可为（integer, double, string, array, boolean, object, NULL），当这个函数操作一个用户生成的字符串的时候，在低版本的PHP中可能会产生内存泄露的漏洞，当然这也不是这篇blog要关注的问题。如果你对这个问题感兴趣，你可以再去看看我上面说的大神的文章。

另外一种攻击方式发生在当攻击者的输入被unserialize函数操作的时候，这种就是我说到的“PHP对象注入”。在这种方式中，对象类型的被unserialize的话，允许攻击者设置他选择对象的属性。当这些对象中的方法被调用的时候，会出现一些效果（例如：删除一些文件），当攻击者可以去选择对象里的一些属性的时候，他就能够删除一个他所提交的文件。

让我们举个例子吧，想象以下的代码中的class是用户自己生成的内容被unserialize时载入的：

（ps:老外贴出的代码语法有问题，改了一下测试成功……）
1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17<?php

class

Foo {
    

private

$bar

;

    

public
  

$file

;
 

    

public

function

__construct(

$fileName

) {
        

$this

->bar =

'foobar'

;

        

$this

->file =

$fileName

;
    

}

 
    

// 一些其他的代码……

 
    

public

function

__toString() {

        

return

file_get_contents

(

$this

->file);
    

}

}
?>

如果受害的缺陷代码同时还存在以下的代码：

1echo

unserialize(

$_GET

[

'in'

]);

这攻击者就可以读取任意文件，攻击者可以如下去构造它的对象。

1

2
3

4
5

6
7

8
<?php

class

Foo {
    

public

$file

;

}
$foo

=

new

Foo();

$foo

->file =

'/etc/passwd'

;
echo

serialize(

$foo

);

?>

上面这段代码的结果是：O:3:"Foo":1:{s:4:"file";s:11:"/etc/passwd";} ，攻击者现在要做的事情就事通过提交get请求到存在漏洞的页面触发他的攻击代码。这个页面会吐出/etc/passwd的内容来。能读到这些文件的内容怎么看都不是一个好事情，你就想象一下，万一缺陷代码中的函数不是file_get_contents而是eval呢？

我相信上面这部分已经能让人明白允许用户输入进入unserialize这个函数危害有多大了。就连PHP手册里也说了不要把用户生成的内容交给unserialize函数。

### 警告：

不要把不可信的用户输入交给unserialize，使用该函数解序列化内容能导致访问且自动载入对象，恶意用户可以利用这一点，从安全的角度，如果你想让用户可以标准的传递数据，可以使用json （json_encode json_decode）。

好，让我们继续说这些问题怎么影响到Wordpress。

## 0x01 wordpress的安全问题

Stefan Esser's的黑帽演讲中，他提到Wordpress是一款使用了serialize和unserialize函数的知名应用。在他的幻灯片中，unserialize用来接收来自Wordpress站点上的数据。所以攻击者可以在受害站点上发起一次中间人攻击。他可以修改来自Wordpress站点的返回数据，把他的代码加进去。有趣的是就在我编写这篇文章的时候，Wordpress最新的版本也包含这个问题（距离那演讲似乎过去三年了），想象一下，如果有黑客可以劫持WordPress.org的DNS会发生什么事情吧。

然而，这也不是Wordpress使用这个unserialize的唯一地方，它还用于用于在数据库中数据。举例来说，用户的metadata就被序列化后存储在数据库中，metadata的取回方式在wp-includes/meta.php的272行的get_metadata()，我在这里引用一下该函数的部分代码（292-297行）
1

2
3

4
5

6
if

( isset(

$meta_cache

[

$meta_key

]) ) {

    

if

(

$single

)
        

return

maybe_unserialize(

$meta_cache

[

$meta_key

][0] );

    

else
        

return

array_map

(

'maybe_unserialize'

,

$meta_cache

[

$meta_key

]);

}

基本上，这个函数所干的事情就是取回数据库里的metadata（它来自每篇文章或用户输入），数据在数据库中的wp_postmeta和wp_usermeta表中，有些数据是被序列化的而有些没有被序列化，所以maybe_unserialize()函数替代了unserialize()在这里操作，这个函数在wp-includes/functions.php的230到234行之间被定义。

1

2
3

4
5function

maybe_unserialize(

$original

) {

    

if

( is_serialized(

$original

) )

//序列化的数据才会走到这里面
        

return

@unserialize(

$original

);

    

return

$original

;
}

所以，这个函数干的事情是检查给予它的值是不是一个序列化的数据，如果是的话，就解序列化。这里用来判断是否是序列化所使用的函数是is_serialized()，它的定义在同文件的247到276行之间。

1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23

24
25

26
27

28
29

30
function

is_serialized(

$data

) {

    

// 如果连字符串都不是，那就不是序列化的数据了
    

if

( !

is_string

(

$data

) )

        

return

false;
    

$data

= trim(

$data

);

     

if

(

'N;'

==

$data

)
        

return

true;

    

$length

=

strlen

(

$data

);
    

if

(

$length

< 4 )

        

return

false;
    

if

(

':'

!==

$data

[1] )

        

return

false;
    

$lastc

=

$data

[

$length

-1];

    

if

(

';'

!==

$lastc

&&

'}'

!==

$lastc

)
        

return

false;

    

$token

=

$data

[0];
    

switch

(

$token

) {

        

case

's'

:
            

if

(

'"'

!==

$data

[

$length

-2] )

                

return

false;
        

case

'a'

:

        

case

'O'

:
            

return

(bool) preg_match(

"/^{$token}:[0-9]+:/s"

,

$data

);

        

case

'b'

:
        

case

'i'

:

        

case

'd'

:
            

return

(bool) preg_match(

"/^{$token}:[0-9.E-]+;\$/"

,

$data

);

    

}
    

return

false;

}

WordPress检查一个值是否是序列化的字符串为什么那么重要的原因马上要变得清晰了。首先，我们看一下一个攻击者如何把他的内容最终加入到metadata表中的。每个用户的姓名，雅虎IM都存储在wp_usermeta表里。所以我们把自己的恶意代码加在那我们就可以搞掂掉WordPress，对不对？你可以试试在你该写名字的地方写个i:1试试，如果这个没有被解序列化那这里只会返回一个我们输入的i:1。

麻痹的，看来要发几个大招才可以搞掂WordPress啊。让我们挖得再深一点，看看为什么这个东西就没有给解序列化。在 wp-includes/meta.php 中，这个update_metadata() 函数定义在101-164行，这里有这个函数的部分代码。
1

2
3

4
5

6
7

8
9

10
// …

    

$meta_value

= wp_unslash(

$meta_value

);
    

$meta_value

= sanitize_meta(

$meta_key

,

$meta_value

,

$meta_type

);

// …
    

$meta_value

= maybe_serialize(

$meta_value

);

 
    

$data
  

= compact(

'meta_value'

);

// …
    

$wpdb

->update(

$table

,

$data

,

$where

);

// …

这里maybe_serialize函数可能能解释为什么我们刚才的操作没成功，我们再跟进去看看这个函数，它定义在wp-includes/functions.php的314-324行。

1

2
3

4
5

6
7

8
9

10
function

maybe_serialize(

$data

) {

    

if

(

is_array

(

$data

) ||

is_object

(

$data

) )
        

return

serialize(

$data

);

    

// 二次序列化是为了向下支持
    

// 详见 http://core.trac.wordpress.org/ticket/12930

    

if

( is_serialized(

$data

) )
        

return

serialize(

$data

);

 
    

return

$data

;

}

所以当我们传入一个序列化的值的话，它就会再序列化一下，这就是现在发生的情况，你看，数据库里的东西不是i:1;而是s:4:"i:1;";，当解序列化的时候它就显示为一个字符串，那现在该怎么办呢？

你懂的，这帖子的内容也存在数据库里，上面这就说明了为什么我们失败了。如果我们现在想往数据库插一个序列化后的东西，我们就需要在我们插入数据的时候让is_serialized()这个函数返回一个false，而当我们再从数据里取它的时候，它就应该返回个true了。

你懂的，Mysql数据库，表和字段都有他们自己的charset和collation(字符集和定序）。WordPress呢，默认的字符集是UTF-8。从这个名字就看的出来，这个字符集它不支持全部的Unicode字符，你要是对这个感兴趣，你可以看看Mathias Bynens的这篇文章：http://mathiasbynens.be/notes/mysql-utf8mb4，这文章教了我UTF-8的表储存不了Unicode编码区间是U+010000到U+10FFFF的字符。所以当我们在这个情况下尝试保存这些字符呢？显而易见，包括这个字符和这个字符之后的内容都会被忽略掉。所以在我们尝试插入

foo𝌆bar
的时候，Mysql会忽略

𝌆bar
而保存为foo。

这个迷题的最后一部分就是我们需要插入一个用以一会儿解序列化的内容，为了测试这个，你可以插入

1:i𝌆
为你的名字。正如所见到的，结果是1，意味着你的输入被解序列化了，如果你还不相信我，你试着输入一个空数组的序列化并且以该字符结尾：

a:0:{}𝌆
。这个结果是Array。

让我们继续

maybe_serialized('i:1;𝌆')
插入了数据库。WordPress不认为这是一个已序列化的数据，因为它不是;或者}结尾的。它会返回

i:1;𝌆
，当插入数据库的时候，它的值是i:1，当它从数据库取回的时候，它有了;最为最后一个字符，所以它可以解序列化成功。碉堡了。漏洞。

## 0x02 WordPress 利用

现在我们展示了WordPress存在PHP对象注入漏洞。让我们尝试利用它。所以为了利用该漏洞（通过注入对象的方法），我们需要找到一个符合以下条件的class：

1，内有“有用”的方法可被调用。
2，存在该对象的类已经被包含了。

当一个对象被解序列化的时候，__wakeup函数会被调用，这被称作PHP的魔术方法，这也是我们确定会被调用的方法，实际上这些函数会更多写些，我写了一个以下的class来获取被调用的class到/tmp/fumc.log。
1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23<?php

class

Foo {
    

public

static

function

logFuncCall(

$funcName

) {

        

$fh

=

fopen

(

'/tmp/func.log'

,

'a'

);
        

fwrite(

$fh

,

$funcName

.

"\n"

);

        

fclose(

$fh

);
    

}

    

public

function

__construct() { Foo::logFuncCall(

'__construct('

.json_encode(func_get_args()).

')'

);}
    

public

function

__destruct() { Foo::logFuncCall(

'__destruct()'

);}

    

public

function

__get(

$name

) { Foo::logFuncCall(

"__get($name)"

);

return

"Foo"

;}
    

public

function

__set(

$name

,

$value

) { Foo::logFuncCall(

"__set($name, value)"

);}

    

public

function

__isset(

$name

) { Foo::logFuncCall(

"__isset($name)"

);

return

true;}
    

public

function

__unset(

$name

) { Foo::logFuncCall(

"__unset($name)"

);}

    

public

function

__sleep() { Foo::logFuncCall(

"__sleep()"

);

return

array

();}
    

public

function

__wakeup() { Foo::logFuncCall(

"__wakeup()"

);}

    

public

function

__toString() { Foo::logFuncCall(

"__toString()"

);

return

"Foo"

;}
    

public

function

__invoke(

$a

) { Foo::logFuncCall(

"__invoke("

. json_encode(func_get_args()).

")"

);}

    

public

function

__call(

$a

,

$b

) { Foo::logFuncCall(

"__call("

. json_encode(func_get_args()).

")"

);}
    

public

static

function

__callStatic(

$a

,

$b

) { Foo::logFuncCall(

"__callStatic("

. json_encode(func_get_args()).

")"

);}

    

public

static

function

__set_state(

$a

) { Foo::logFuncCall(

"__set_state("

. json_encode(func_get_args()).

")"

);

return

null;}
    

public

function

__clone() { Foo::logFuncCall(

"__clone()"

);}

}
?>

为了列出这些被调用的函数，首先要确认这个函数在解序列化发生的时候是被引入被包含过的（php中的include require等）。你可以把require_once('foo.php')加到functions.php的顶端。接下来，把名字改为O:3:"Foo":0:{}𝌆来尝试利用这个PHP对象注入漏洞，当刷新页面后，你回看到你的名字变成了Foo，这也就是意味着这是上面那class中__toString()函数的返回，然后让我们看看都有哪些函数被调用了。

$ sort -u /tmp/func.log __destruct() __toString() __wakeup()

给出了我们三个函数：__wakeup(), __destruct() 和 __toString()

很不幸的是我不能再WordPress中找到一个载入了并且解序列化时能被利用造成影响的类。所以不是一个WordPress的安全问题，而是一个可能利用的地方。

所以是不是WordPress是有安全隐患的，但是无法被利用？不一定，如果你熟悉WordPress，你可能会觉察到可能有一堆插件存在漏洞。这些插件有他们自己的类并且可能暴露出可被利用的安全漏洞。我想到这个后，已经发现了一款著名的插件存在漏洞并且可以导致远程任意代码执行。

由于道德考虑，这个时候我不会发布PoC的，有太多存在安全漏洞的WordPress了。

## 0x03 修复WordPress

这个修复方式是修改is_serialized函数，我简单的说说：
1

2
3

4
5

6
7

8
9

10
11

12
13

14
15

16
17

18
19

20
21

22
23

24
25

26
27

28
29

30
31

32
33

34
35function

is_serialized(

$data

,

$strict

= true ) {

     

// 如果不是字符串就不会是序列化后的数据
     

if

( !

is_string

(

$data

) )

         

return

false;
     

if

(

':'

!==

$data

[1] )

         

return

false;
    

if

(

$strict

) {

        

$lastc

=

$data

[

$length

- 1 ];
        

if

(

';'

!==

$lastc

&&

'}'

!==

$lastc

)

            

return

false;
    

}

else

{

         

//确认存在;或}但不是在第一个字符
        

if

(

strpos

(

$data

,

';'

) < 3 &&

strpos

(

$data

,

'}'

) < 4 )

            

return

false;
    

}

     

$token

=

$data

[0];
     

switch

(

$token

) {

         

case

's'

:
            

if

(

$strict

) {

                

if

(

'"'

!==

$data

[

$length

- 2 ] )
                    

return

false;

            

}

elseif

( false ===

strpos

(

$data

,

'"'

) ) {
                 

return

false;

            

}
         

case

'a'

:

         

case

'O'

:
             

return

(bool) preg_match(

"/^{$token}:[0-9]+:/s"

,

$data

);

         

case

'b'

:
         

case

'i'

:

         

case

'd'

:
            

$end

=

$strict

?

'$'

:

''

;

            

return

(bool) preg_match(

"/^{$token}:[0-9.E-]+;$end/"

,

$data

);
     

}

     

return

false;
 

}

这主要的区别是当$strict参数设置为false的时候，会有一些强制操作导致一个字符串被标记为已序列化。举例说明，最后一个字符不需要必须是;或者{（译者注：作者此处应该笔误了，应该是;或者}),修复了我所提交的漏洞。现在大家有没有相似的内容可以拿出来做个讨论的？

WordPress依旧使用着不安全的unserialize()而非安全的json_decode。它的安全性全在判断规则或者Mysql的规则实现上。我在上面揭露的漏洞实际上是使用Mysql的规则去掉我跟在特殊符号后的所有字符。

有一个很简洁的修复方案，修改一下数据库编码不被截断就好：
ALTER TABLE wp_commentmeta CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci; ALTER TABLE wp_postmeta CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci; ALTER TABLE wp_usermeta CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
版权声明：未经授权禁止转载 [五道口杀气](http://drops.wooyun.org/author/五道口杀气 "由 五道口杀气 发布")@[乌云知识库](http://drops.wooyun.org/)
分享到： []( "分享到QQ空间") []( "分享到新浪微博") []( "分享到腾讯微博") []( "分享到人人网") []( "分享到网易微博") [2]( "累计分享2次")

### 相关日志

* [分析下难得一见的ROR的RCE（CVE－2013－0156）](http://drops.wooyun.org/papers/61)
* [攻击JavaWeb应用[5]-MVC安全](http://drops.wooyun.org/tips/347)
* [攻击JavaWeb应用[3]-SQL注入[1]](http://drops.wooyun.org/tips/236)
* [Flash CSRF](http://drops.wooyun.org/tips/688)
* [浅谈互联网中劫持的一些事情](http://drops.wooyun.org/tips/127)
* [PHP安全编码](http://drops.wooyun.org/tips/135)
上一篇:[老外的一份渗透测试报告](http://drops.wooyun.org/papers/576)

下一篇:[对某创新路由的安全测试](http://drops.wooyun.org/papers/384)

### 楼被抢了 18 层了... [抢座]()、[Rss 2.0](http://drops.wooyun.org/papers/596/feed)或者 [Trackback](http://drops.wooyun.org/papers/596/trackback)

* ![]() 园长 | 2013/09/13 11:36 | [#]()

沙发？

[登录以回复](http://drops.wooyun.org/login)
* ![]() ccSec | 2013/09/13 11:51 | [#]()

太漂亮了！

[登录以回复](http://drops.wooyun.org/login)
* ![]() erevus | 2013/09/13 11:51 | [#]()

那我这是地板

[登录以回复](http://drops.wooyun.org/login)
* ![]() RuIg | 2013/09/13 11:57 | [#]()

good job man!

[登录以回复](http://drops.wooyun.org/login)
* ![]() 0x334 | 2013/09/13 11:59 | [#]()

顶，节操。

[登录以回复](http://drops.wooyun.org/login)
* ![]() BlackWidow7 | 2013/09/13 13:31 | [#]()

杀气。日

[登录以回复](http://drops.wooyun.org/login)
* ![]() insight-labs | 2013/09/13 14:08 | [#]()

杀气太重了……
又要倒下一片了

[登录以回复](http://drops.wooyun.org/login)
* ![]() horseluke | 2013/09/13 16:39 | [#]()

unserialize一直都是大坑，但悲剧的是某些场景还是得要用它，比如文件缓存......

[登录以回复](http://drops.wooyun.org/login)
* ![]() MEng | 2013/09/13 20:15 | [#]()

高富帅!

[登录以回复](http://drops.wooyun.org/login)
* ![]() Ivan | 2013/09/13 22:57 | [#]()

poc啊poc

[登录以回复](http://drops.wooyun.org/login)
* ![]() s3cur1ty | 2013/09/16 10:23 | [#]()

s:4:"i:1;";压根就没再数据库里出现过。。。。

[登录以回复](http://drops.wooyun.org/login)
* ![]() Sct7p | 2013/09/16 13:06 | [#]()

good job

[登录以回复](http://drops.wooyun.org/login)
* ![]() s3cur1ty | 2013/09/16 13:14 | [#]()

function is_serialized( $data ) {
// if it isn't a string, it isn't serialized
if ( ! is_string( $data ) )
return false;
$data = trim( $data );
if ( 'N;' == $data )
return true;
$length = strlen( $data );
if ( $length < 4 )
return false;
if ( ':' !== $data[1] )
return false;
$lastc = $data[$length-1];
if ( ';' !== $lastc && '}' !== $lastc )
return false;
$token = $data[0];
switch ( $token ) {
case 's' :
if ( '"' !== $data[$length-2] )
return false;
case 'a' :
case 'O' :
return (bool) preg_match( "/^{$token}:[0-9]+:/s", $data );
case 'b' :
case 'i' :
case 'd' :
return (bool) preg_match( "/^{$token}:[0-9.E-]+;\$/", $data );
}
return false;
}

难道楼主没有看到if ( $length < 4 ) return false;
[登录以回复](http://drops.wooyun.org/login)

* ![]() Ray | 2013/09/24 17:12 | [#]()

我估计啊，是作者翻译的时候翻着翻着把i:1;写成了i:1，而i:1;正好是4个字节。

[登录以回复](http://drops.wooyun.org/login)
* ![]() 龙臣 | 2013/09/16 16:21 | [#]()

holy shit

[登录以回复](http://drops.wooyun.org/login)
* ![]() 齐迹 | 2013/09/17 17:57 | [#]()

弱弱的问一下 只有在用了__toString 这个魔术方法的时候才能够触发吗？

[登录以回复](http://drops.wooyun.org/login)
* ![]() lhshaoren | 2013/09/24 17:05 | [#]()

膜拜

[登录以回复](http://drops.wooyun.org/login)
* ![]() Forever80s | 2013/10/08 22:01 | [#]()

wordpress框架要是能找出来漏洞，你就发了。。。

[登录以回复](http://drops.wooyun.org/login)
### 发表评论

[点击这里取消回复。]()

您必须 [登陆](http://drops.wooyun.org/login) 后才能发表评论。

### 订阅更新

* [![]()](http://drops.wooyun.org/feed "点击订阅")

### 分类

* [漏洞分析](http://drops.wooyun.org/category/papers "漏洞分析") (40)
* [技术分享](http://drops.wooyun.org/category/tips "技术分享") (57)
* [工具收集](http://drops.wooyun.org/category/tools "工具收集") (5)
* [业界资讯](http://drops.wooyun.org/category/news "业界资讯") (3)
### 最新日志

* [Flash CSRF](http://drops.wooyun.org/tips/688 "Flash CSRF")
* [XSS与字符编码的那些事儿 —科普文](http://drops.wooyun.org/tips/689 "XSS与字符编码的那些事儿 —科普文")
* [Zabbix SQL Injection/RCE – CVE-2013-5743](http://drops.wooyun.org/papers/680 "Zabbix SQL Injection/RCE – CVE-2013-5743")
* [CDN流量放大攻击思路](http://drops.wooyun.org/papers/679 "CDN流量放大攻击思路")
* [从丝绸之路到安全运维（Operational Security）与风险控制（Risk Management） 上集](http://drops.wooyun.org/news/674 "从丝绸之路到安全运维（Operational Security）与风险控制（Risk Management） 上集")
* [攻击JavaWeb应用[8]-后门篇](http://drops.wooyun.org/tips/662 "攻击JavaWeb应用[8]-后门篇")
* [php4fun.sinaapp.com PHP挑战通关攻略](http://drops.wooyun.org/papers/660 "php4fun.sinaapp.com PHP挑战通关攻略")
* [搭建基于Suricata+Barnyard2+Base的IDS前端Snorby](http://drops.wooyun.org/papers/653 "搭建基于Suricata+Barnyard2+Base的IDS前端Snorby")
* [GPU破解神器Hashcat使用简介](http://drops.wooyun.org/tools/655 "GPU破解神器Hashcat使用简介")
* [内网渗透应用 跨vlan渗透的一种思路](http://drops.wooyun.org/tips/649 "内网渗透应用 跨vlan渗透的一种思路")

### 最新评论

* ![]()

c2y2 在 [XSS与字符编码的那些事儿 ---科普文](http://drops.wooyun.org/tips/689#comment-1883)
谢谢
* ![]()

Evi1c0de 在 [如何用意念获取附近美女的手机号码](http://drops.wooyun.org/tips/573#comment-1877)
连接CMCC跳转到登陆页面 右键源码 修改即可
* ![]()

牛牛 在 [Flash CSRF](http://drops.wooyun.org/tips/688#comment-1871)
支持一下呵呵
* ![]()

luwikes 在 [JBoss安全问题总结](http://drops.wooyun.org/papers/178#comment-1870)
mark
* ![]()

NgInx 在 [Flash CSRF](http://drops.wooyun.org/tips/688#comment-1869)
好吧，基佬，顶你一个，不错。
* [下一页 »]()
Powered by [WordPress](http://wordpress.org/) | GZai Theme by [鬼仔](http://huaidan.org/ "GZai Theme produced by 鬼仔")
[![]()](http://tongji.baidu.com/hm-web/welcome/ico?s=9fc41da6a2322bdd80563c9d5a4bdb1d)
