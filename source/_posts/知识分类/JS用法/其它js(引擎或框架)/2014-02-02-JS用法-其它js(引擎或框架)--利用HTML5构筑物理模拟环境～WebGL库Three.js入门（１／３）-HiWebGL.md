---
layout: post
title: "利用HTML5构筑物理模拟环境～ WebGL库Three.js入门（１／３） - HiWebGL"
categories: JS用法
tags: 
 - JS用法
 - 其它js(引擎或框架)
--- 

# 利用HTML5构筑物理模拟环境～ WebGL库Three.js入门（１／３） | HiWebGL

[![]()](http://www.hiwebgl.com/ "HiWebGL")

最好的HTML5 WebGL中文资讯站！

* [**首页**Homepage](http://www.hiwebgl.com/ "HiWebGL")
* [**教程** ](http://www.hiwebgl.com/?p=42)
* [**Demo** ](http://www.hiwebgl.com/?cat=16)
* [**有问必答** ](http://www.hiwebgl.com/ask)
* [**论坛** ](http://hiwebgl.com/forum/)

You are here: [Home](http://www.hiwebgl.com/) » [WebGL教程](http://www.hiwebgl.com/?cat=17 "查看 WebGL教程 中的全部文章") » 利用HTML5构筑物理模拟环境～ WebGL库Three.js入门（１／３）
# 利用HTML5构筑物理模拟环境～ WebGL库Three.js入门（１／３）

By admin On 2012 年 5 月 7 日 31 条评论 25,719 次浏览

**作者：遠藤 理平
翻译：瀡风(2012年5月6日)
如需转载本中文译文，请联系翻译者瀡风！
点击这里查看[原文](http://www.natural-science.or.jp/article/20120220155529.php)**
目录

* [1 HTML5和WebGL]()

* [1.1 HTML5的新成员「canvas元素」]()
* [1.2 OpenGL 的浏览器版 WebGL 登场]()
* [1.3 WebGL例]()
* [1.4 WebGL的利用方法]()
* [2 三维物理模拟：2重单摆模拟器的作成]()

* [2.1 环境的准备]()
* [2.2 Three.js下载]()
* [3 Three.js的动作确认]()

* [3.1 例１：tutorial1.html（设置视点和光源、绘制立方体）]()
* [3.2 例１的解说]()
* [3.3 ０．画布(Canvas)的准备]()

* [3.3.1 body 元素中追加 div 标签]()
* [3.3.2 style 元素中追加 CSS样式]()
* [3.4 １．设置渲染器]()
* [3.5 ２．设置相机]()

* [3.5.1 透视投影模式下相机的设置]()
* [3.5.2 设置相机的位置坐标]()
* [3.5.3 设置相机的上方向]()
* [3.5.4 设置相机的视野中心]()
* [3.5.5 动作确认]()
* [3.5.6 其他投影方式]()
* [3.6 ３．声明场景]()
* [3.7 ４．设置光源并追加到场景]()

* [3.7.1 动作确认]()
* [3.7.2 其他光源]()
* [3.8 ５．声明立方体并追加到场景]()

* [3.8.1 动作确认]()
* [3.8.2 其他形状类]()
* [3.9 ６．执行渲染（threeStart() ）]()
* [3.10 稍微再试一下（追加平面和球体）]()

* [3.10.1 执行结果]()
* [4 Three.js 实现动画]()

* [4.1 使用「window.requestAnimationFrame」函数实现动画]()
* [4.2 例２：tutorial2.html（实现动画）]()
* [4.3 例２的解说]()

* [4.3.1 创建多个对象]()
* [4.3.2 「loop()」函数的定义和执行]()
* [4.3.3 指定物体的回转角度的方法]()
* [4.4 例２－２：tutorial2_2.html（相机的移动）]()
* [4.5 例２－３：tutorial2_3.html（设置环境光）]()
* [4.6 例２－４：tutorial2_4.html（影子的设置）]()
* [4.7 例２－５：tutorial2_5.html（纹理）]()
* [5 鼠标事件]()

* [5.1 鼠标事件例]()
* [5.2 例３：tutorial3.html（鼠标事件）]()

## HTML5和WebGL

简单认识一下HTML5吧。HTML5是目前构成WEB页的主流的HTML4和XHTML的后续语言、 2008年制定了草案、约定各大浏览器提供商一起力争在2014年前形成正式的版本。 HTML5在2012年1月还处于「草案」阶段、虽然还处于规范在变动的准备阶段、但是无论是开发者还是用户都非常的关注。 一个主要的理由就是、**随着iPhone和Android等智能手机的崛起**，人们期待HTML5能够对各种各样的WEB内容**跨平台化**做 出重要的贡献。 在智能手机登上舞台之前、Adobe公司的 FLASH 在动画和音频等多媒体内容领域已成为了事实上的标准、通过导入FLASH插件在各种操作系统或者浏览器中都能够支持FLASH格式、事实上通过FLASH 应该是可以构建一个跨平台的执行环境、但是因为Apple公司的iPhone（和iPad）上拒绝支持FLASH使得这一构想成为了泡影。 于是、一个崭新的跨平台执行环境闪耀登场、这就是WEB标准化团体的作为下一代WEB页开发语言的HTML5。 HTML从此由单纯的支持静态文本内容的语言开始， 进化成支持动画和音频，而且还可以直接操作画像数据的语言。 并且、因为不再需要导入插件了、当然可以实现包括智能手机在内的大多数环境中的跨平台。出于对WEB内容跨平台化迫切的需求、虽然还只是在【草案】阶段的 HTML5已经被各浏览器所支持。2012年2月为止、HTML5智能手机中的浏览器基本上100%支持HTML5了（电脑平台的各浏览器也基本都支 持）。 另外，HTML5不是一个单体，而是需要和控制视觉体裁的CSS3及浏览器支持的Javascript语言联合起来才能发挥出真实的价值。

### HTML5的新成员「canvas元素」

在HTML5中、新登场的 canvas 元素所表现的领域里、可以以像素为单位指定颜色。 这意味着不依靠插件也能在HTML中直接绘制二维和三维图像。

### OpenGL 的浏览器版 WebGL 登场

关于2维3维计算机图形技术、Khronos Group对各种技术进行了标准化并制定了[OpenGL]、作为Windows、Mac、Linux上的一种跨平台技术并一直活跃着。 OpenGL 能够从任意视点出发，对三维空间中的物体进行二维投影的自动计算。 而且因为可以直接操作计算机的图形卡、OpenGL能够非常高速和高精度地描绘三维图像。2009年，Khronos Group把这样的OpenGL技术运用到WEB浏览器并制定了WebGL。WebGL 是 OpenGL 和 JavaScript 之间的结晶、**HTML5 的 canvas 元素里、利用和OpenGL同样的API、可以绘制高精度的三维图像**。 但问题也出来了，占有最大浏览器份额的InternetExplore反对采用WebGL。 因为一直以来 OpenGL 和 开发InternetExplore 的 Microsoft公司独自开发的三维图形API「DirectX」处于竞争者关系。 可因为 DirectX 只是 Microsoft 公司的产品、不能成为跨平台的选择、 而且在浏览器中描绘性能也没有超过WebGL的迹象、所以胜败已经是很明显的事情了。 关于WebGL的中需要注意的是、浏览器要支持WebGL、电脑中安装的图形显示卡中的OpenGL的版本必须要是2.0以上。

### WebGL例

关于WebGL非常有名的例子是、Human Engines和Gregg Tavares 2010年末开发的「[Aquarium](http://webglsamples.googlecode.com/hg/aquarium/aquarium.html)」。下图是画面截图、请使用支持WebGL的浏览器进行浏览。

[![]( "Aquarium")](http://webglsamples.googlecode.com/hg/aquarium/aquarium.html)

在浏览器中能够实现如此精致的三维图形，而且还有动画效果真是让人吃惊。 最近、侧重学习的实用性、[化学构造的三维绘制库「ChemDoodle」（下图左）](http://web.chemdoodle.com/) 和 [三维人体解剖图「BIODIGITAL HUMAN」（下图右）](http://www.biodigitalhuman.com/)等已经陆续出现了。近期教育界正在议论的教材数字化，也在考虑是不是可以直接推进到类似这样的层次。

[![]( "ChemDoodle")](http://web.chemdoodle.com/)    [![]( "BIODIGITAL HUMAN")](http://www.biodigitalhuman.com/)

### WebGL的利用方法

在做WebGL开发的时候，可以利用「glMatrix.js」库矩阵运算的功能。 2009年末、Giles发表了使用WebGL的入门向导文章「[Learning WebGL](http://learningwebgl.com/blog/?page_id=1217)」、WebGL的使用方法变得更加容易理解了。虽说通过WebGL的绘制效果甚至可能达到本地的高性能游戏专用机的绘制效果，但如果没有非常精通OpenGL的人在还是不太现实的。 因此、2010年末为了能够更加简单的利用WebGL，库「[Three.js（Mr.doob）](http://mrdoob.github.com/three.js/)」被发表了。因为 Three.js 非常容易使用，所以很快变得非常有人气。 发表的初期每周一回的频率、到2012年2月还保持在1.5个月１回的频率进行版本更新、期待它在未来有更远的发展。 本文的三维物理模拟的绘制也使用了Three.js。 WebGL的javascript框架除了Three.js之外，还有「[J3D](http://creativejs.com/)」和 「[SceneJS](http://scenejs.com/)」和、日本开发的「[gl.enchant.js β版（株式会社ユビキタスエンターテインメント）](http://9leap.net/games/1109)」等。

## 三维物理模拟：2重单摆模拟器的作成

本文中、利用 WebGL 的 Three.js 库作成了三维物理模拟实例「[２重振り子シミュレータ](http://www.natural-science.or.jp/WebGL/DoublePendulum_ver1.0.html)」。 支持WebGL的浏览的话[查看这边](http://www.natural-science.or.jp/WebGL/DoublePendulum_ver1.0.html)。下图是「[２重振り子シミュレータ](http://www.natural-science.or.jp/WebGL/DoublePendulum_ver1.0.html)」的截图。

[![]( "２重振り子シミュレータ")](http://www.natural-science.or.jp/WebGL/DoublePendulum_ver1.0.html)

### 环境的准备

１．文本编辑器（什么都没有也没关系）
２．运行WebGL的浏览器（推荐：Google Chrome ver.17）
３．调试器（推荐：Google Chrome）
４．WebGL 库 Three.js

本文的内容按照下面的环境进行过测试。
OS：Windows7 Professional
浏览器：Google Chrome（ver.17）
GPU：NVIDIA 550i（OpenGL 4.2） Three.js 的版本：r47

### Three.js下载

Three.js 的下载、先打开「[mrdoob/three.js – GitHub](https://github.com/mrdoob/three.js/)」、点击下图的 「ZIP」就可以下载所有的文件。使用 Three.js 的时候只要有「build」目录下的「Three.js」就足够了。

![]( "mrdoob/three.js - GitHub")

拷贝「build」目录下的「Three.js」到工作目录下。这样准备工作就做好了。

## Three.js的动作确认

绘制一个长方体，顺便确认下 Three.js 的运行。 启动文本编辑器、把下面的源代码拷贝&粘贴、并保存到工作目录下。

### 例１：tutorial1.html（设置视点和光源、绘制立方体）

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72<!DOCTYPE html> <html> <head> <meta charset="UTF-8"> <title>Three.js チュートリアル１</title> <script src="Three.js"></script> <style type="text/css"> div#canvas-frame{ border: none; cursor: pointer; width: 600px; height: 600px; background-color: #EEEEEE; } </style> <script> var renderer; function initThree() { width = document.getElementById('canvas-frame').clientWidth; height = document.getElementById('canvas-frame').clientHeight; renderer = new THREE.WebGLRenderer({antialias: true}); renderer.setSize(width, height ); document.getElementById('canvas-frame').appendChild(renderer.domElement); renderer.setClearColorHex(0xFFFFFF, 1.0); }   var camera; function initCamera() { camera = new THREE.PerspectiveCamera( 45 , width / height , 1 , 10000 ); camera.position.x = 100; camera.position.y = 20; camera.position.z = 50; camera.up.x = 0; camera.up.y = 0; camera.up.z = 1; camera.lookAt( {x:0, y:0, z:0 } ); } var scene; function initScene() { scene = new THREE.Scene(); } var light; function initLight() { light = new THREE.DirectionalLight(0xFF0000, 1.0, 0); light.position.set( 100, 100, 200 ); scene.add(light); } var cube; function initObject(){ cube = new THREE.Mesh( new THREE.CubeGeometry(50,50,50), //形状の設定 new THREE.MeshLambertMaterial({color: 0xff0000}) //材質の設定 ); scene.add(cube); cube.position.set(0,0,0); } function threeStart() { initThree(); initCamera(); initScene(); initLight(); initObject(); renderer.clear(); renderer.render(scene, camera); } </script> </head>   <body onload="threeStart();"> <div id="canvas-frame"></div> </body> </html>

执行的结果就象下面这样。 使用支持WebGL的浏览器就可以看到一个红色的长方体被绘制出来了。

### 例１的解说

使用Three.js 绘制的步骤参照如下

* ０．画布的准备
* １．Three.js 的初期化（initThree()）
* ２．相机的准备（initCamera()）
* ３．场景的准备（initScene()）
* ４．光源的准备（initLight()）
* ５．物体的装备（initObject()）
* ６．执行绘制（threeStart()）

### ０．画布(Canvas)的准备

准备和画布框大小一致的领域用于WebGL绘制。 具体来说、
(1) body 标签中添加 id 为「canvas-frame」的 div 元素。
(2) style 标签中指定 「canvas-frame」的CSS样式。

### body 元素中追加 div 标签

1<[div](http://december.com/html/4/element/div.html) id="canvas-frame"></[div](http://december.com/html/4/element/div.html)>

### style 元素中追加 CSS样式

通过【元素#ID名】的CSS选择器方式，为【canvas-frame】元素指定如下属性。
(1) 隐藏框线
(2) 鼠标光标设置为【pointer】
(3) 宽度设置为600px
(4) 高度设置为600px
(5) 背景设置为「#EEEEEE」
1 2 3 4 5 6 7 8 9<[style](http://december.com/html/4/element/style.html) type="text/css"> div#canvas-frame{ border: none; //(1) cursor: pointer; //(2) width: 600px; //(3) height: 600px; //(4) background-color: #EEEEEE; //(5) } </[style](http://december.com/html/4/element/style.html)>

### １．设置渲染器

三维空间里的物体映射到二维平面的过程被称为三维渲染。 一般来说我们都把进行渲染操作的软件叫做渲染器。 具体来说要进行下面这些处理。

(0) 声明全局变量（对象）
(1) 获取画布「canvas-frame」的高宽
(2) 生成渲染器对象（属性：抗锯齿效果为设置有效）
(3) 指定渲染器的高宽（和画布框大小一致）
(4) 追加 【canvas】 元素到 【canvas-frame】 元素中。
(5) 设置渲染器的清除色(clearColor)
1 2 3 4 5 6 7 8 9 10var width, height; //(0) var renderer; //(0) function initThree() { width = document.getElementById('canvas-frame').clientWidth; //(1) height = document.getElementById('canvas-frame').clientHeight; //(1) renderer = new THREE.WebGLRenderer({antialias: true}); //(2) renderer.setSize(width, height); //(3) document.getElementById('canvas-frame').appendChild(renderer.domElement); //(4) renderer.setClearColorHex(0xFFFFFF, 1.0); //(5) }

### ２．设置相机

OpenGL（WebGL）中、三维空间中的物体投影到二维空间的方式中，存在*透视投影*和*正投影*两种相机。 透视投影就是、从视点开始越近的物体越大、远处的物体绘制的较小的一种方式、和日常生活中我们看物体的方式是一致的。 正投影就是不管物体和视点距离，都按照统一的大小进行绘制、在建筑和设计等领域需要从各个角度来绘制物体，因此这种投影被广泛应用。在 Three.js 也能够指定透视投影和正投影两种方式的相机。 本文按照以下的步骤设置透视投影方式。

(0) 声明全局的变量（对象）
(1) 设置透视投影的相机
(2) 设置相机的位置坐标
(3) 设置相机的上为「z」轴方向
(4) 设置视野的中心坐标
1 2 3 4 5 6 7 8 9 10 11var camera; //(0) function initCamera() { var camera = new THREE.PerspectiveCamera( 45 , width / height , 1 , 10000 ); //(1) camera.position.x = 100; //(2) camera.position.y = 20; //(2) camera.position.z = 50; //(2) camera.up.x = 0; //(3) camera.up.y = 0; //(3) camera.up.z = 1; //(3) camera.lookAt( {x:0, y:0, z:0 } ); //(4) }

### 透视投影模式下相机的设置

透视投影中，会把称为视体积领域中的物体作成投影图。 视体积是通过以下4个参数来指定。
视野角：fov
纵横比：aspect
相机离视体积最近的距离：near
相机离视体积最远的距离：far

![]( "设置相机")

通过上面4个参数确定的透视投影设置相机。 默认情况下相机的上方向为Y轴，右方向为X轴，沿着Z轴朝里。
1var camera = new THREE.PerspectiveCamera( fov , aspect , near , far );

通过 Three.js 的 「PerspectiveCamera」类声明对象[camera]，同时我们可以修改对象[camera]的各种属性值来设置相机的详细属性。

### 设置相机的位置坐标

相机的位置坐标和视野的中心坐标，按照(2)那样的方式进行设置。 和上面(2)的方式一样，下面这样的方法也可以。
1camera.position.set(50,50,100); //(2)

### 设置相机的上方向

相机的上方向按照(3)这样的方式设置对象的属性。

### 设置相机的视野中心

利用[lookAt]方法来设置相机的视野中心。 「lookAt()」的参数是一个属性包含中心坐标「x」「y」「z」的对象。 「lookAt()」方法不仅是用来设置视点的中心坐标、 在此之前设置的相机属性要发生实际作用，也需要调用 [lookAt] 方法。

### 动作确认

请尝试修改(1),(2),(3),(4),(5)、来确认相机各个设定的实际作用。

### 其他投影方式

在 Three.js 中、有各种各样的类，用来来实现透视投影、正投影或者复合投影（透视投影和正投影）这样的相机。
1 2var camera = THREE.OrthographicCamera = function ( left, right, top, bottom, near, far ) //正投影 var camera = THREE.CombinedCamera = function ( width, height, fov, near, far, orthonear, orthofar ) //複合投影

### ３．声明场景

场景就是一个三维空间。 用 [Scene] 类声明一个叫 [scene] 的对象。
1 2 3 4var scene; function initScene() { scene = new THREE.Scene(); }

### ４．设置光源并追加到场景

OpenGL（WebGL）的三维空间中，存在*点光源*和*聚光灯*两种类型。 而且，作为点光源的一种特例还存在*平行光源(无线远光源)*。另外，作为光源的参数还可以进行 [环境光] 等设置。 作为对应， Three.js中可以设置 [点光源(Point Light)] [聚光灯(Spot Light)] [平行光源(Direction Light)]，和 [环境光(Ambient Light)]。 和OpenGL一样、在一个场景中可以设置多个光源。 基本上，都是环境光和其他几种光源进行组合。 如果不设置环境光，那么光线照射不到的面会变得过于黑暗。 本文中首先按照下面的步骤设置平行光源，在这之后还会追加环境光。
(0) 声明全局变量(对象)
(1) 设置平行光源
(2) 设置光源向量
(3) 追加光源到场景
1 2 3 4 5 6var light; //(0) function initLight() { light = new THREE.DirectionalLight(0xFF0000, 1.0, 0); //(1) light.position.set( 100, 100, 200 ); //(2) scene.add(light); //(3) }

用「DirectionalLight」类声明一个叫 [light] 的对象来代表平行光源，之后在构造函数中进行属性设置

1var light = new THREE.DirectionalLight( hex, intensity)

「hex」是用来以16进制指定光源的颜色。默认是白色「0xFFFFFF」。 「intensity」是光源的强度。默认为1。 另外，还需要通过对象的属性来设置平行光源的方向。

1light.position.set( x, y, z );

光源方向向量是从通过[set()]方法指定的点(x, y, z)开始到 点(0, 0, 0) 的方向，和向量的大小没有关系。 和相机的位置坐标设置一样，直接修改「position.x, position.y, position.z」属性也可以。 最后通过 [add] 方法追加光源到场景中去。

### 动作确认

请试着修改(1),(2)来明确平行光源的方向以及光线的颜色设置原理。

※注意：三维世界中物体的颜色不只是由光源的颜色来决定，而且还和物体本身的颜色有关。 之后会再次说明， 因为这个立方体的颜色为红色(0xFF0000),光源中即使存在绿色或者蓝色，立方体看起来的颜色也不会改变。也就是说， 就这个立方体来说，光源的颜色不论是「0xFFFF00」还是「0xFF0000」看起来颜色都没有变化。

### 其他光源

1 2 3var light = new THREE.AmbientLight( hex ); //(1)環境光源 var light = new THREE.PointLight( hex, intensity, distance ); //(2)点光源 var light = new THREE.SpotLight( hex, intensity, distance, castShadow ); //(3)スポットライト光源

### ５．声明立方体并追加到场景

终于要向三维空间中追加物体了。
1 2 3 4 5 6 7 8 9var cube; //(0) function initObject(){ cube = new THREE.Mesh( //(1) new THREE.CubeGeometry(50,50,50), //(2) new THREE.MeshLambertMaterial({color: 0xff0000}) //(3) ); scene.add(cube); //(4) cube.position.set(0,0,0); //(5) }

(1) 声明 [Mesh] 类的对象 [cube]。 设置构造函数的第一个参数「形状对象（geometory）」、第二个参数[材质对象（material）]。 第一参数中设置的代表立方体的形状对象，通过「CubeGeometry」类可以创建。

1var geometry = THREE.CubeGeometry ( width, height, depth, segmentsWidth, segmentsHeight, segmentsDepth, materials, sides );

构造函数中的「width」「height」「depth」代表立方体的宽高及深度，其他的参数可以省略。 第二个参数中代入的材质对象， 本文中使用的是反射来自光源的光线的材质 「MeshLambertMaterial」类创建的对象。

1var geometry = THREE.MeshLambertMaterial( parameters );

构造函数中的参数是联想队列。 本例中、物体的颜色指定为「color: 0xff0000」。另外，通过联想队列参数还可以指定环境光的颜色。关于这些之后我们再介绍。 通过步骤(4),(5)追加立方体到场景，并指定位置坐标。

### 动作确认

请试着修改(2),(3),(5)来明确怎么设置立方体的大小， 颜色和位置。

### 其他形状类

1 2 3 4 5 6THREE.CubeGeometry ( width, height, depth, segmentsWidth, segmentsHeight, segmentsDepth, materials, sides );//立方体 THREE.CylinderGeometry ( radiusTop, radiusBottom, height, segmentsRadius, segmentsHeight, openEnded );//円錐型 THREE.OctahedronGeometry ( radius, detail ) //八面体 THREE.PlaneGeometry ( width, height, segmentsWidth, segmentsHeight ); //平面型 THREE.SphereGeometry ( radius, segmentsWidth, segmentsHeight, phiStart, phiLength, thetaStart, thetaLength );//球型 THREE.TorusGeometry ( radius, tube, segmentsR, segmentsT, arc )//トーラス型

### ６．执行渲染（threeStart() ）

最后，按照已设定的相机来观察场景中的物体的方式来进行实际绘制。
1 2 3 4 5 6 7 8 9function threeStart() { initThree(); //(0) initCamera(); //(0) initScene(); //(0) initLight(); //(0) initObject(); //(0) renderer.clear(); //(1) renderer.render(scene, camera); //(2) }

### 稍微再试一下（追加平面和球体）

在上面的例子中再追加平面和球体。
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23var cube, sphere, plane; function initObject(){ cube = new THREE.Mesh( new THREE.CubeGeometry(50,50,50), new THREE.MeshLambertMaterial({color: 0xff0000}) ); scene.add(cube); cube.position.set(0,-50,0);   sphere = new THREE.Mesh( new THREE.SphereGeometry(20,20,20), new THREE.MeshLambertMaterial({color: 0x00ff00}) ); scene.add(sphere); sphere.position.set(0,0,0);   plane = new THREE.Mesh( new THREE.PlaneGeometry(50, 50), new THREE.MeshLambertMaterial({color: 0x0000ff}) ); scene.add(plane); plane.position.set(0,50,0); }

### 执行结果

## Three.js 实现动画

通过 WebGL，也能够像电视机一样连续绘制静态画像(frame)来实现动画效果。 WEB浏览器中能够按照一定时间间隔调用Javascript函数。最大可以支持60fps(fps就是一秒中的帧数）（译者注：这里是特定指用window.requestAnimationFrame这样的方式）。 当然根据所调用函数的执行时间，帧数可能会下降。接下来说一下具体如何实现动画效果。

### 使用「window.requestAnimationFrame」函数实现动画

「window.requestAnimationFrame」函数能够在一定时间间隔后调用指定函数。这个函数只有在所在浏览器页正在被浏览的时候才会执行，因此更加节省资源。
1 2 3 4function loop() { （処理） window.requestAnimationFrame(loop); }

看下上面的例子就能明白，「window.requestAnimationFrame」和 loop 函数配合形成一个无限循环的调用。
1 2 3 4function threeStart() { （処理） loop(); }

按照这样连续绘制就实现了动画效果。

### 例２：tutorial2.html（实现动画）

拷贝&粘贴下面这段javascript代码，看一看效果吧。
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61var width, height; var renderer; function initThree() { width = document.getElementById('canvas-frame').clientWidth; height = document.getElementById('canvas-frame').clientHeight; renderer = new THREE.WebGLRenderer({antialias: true}); renderer.setSize(width, height ); document.getElementById('canvas-frame').appendChild(renderer.domElement); renderer.setClearColorHex(0xFFFFFF, 1.0); }   var camera; function initCamera() { camera = new THREE.PerspectiveCamera( 45 , width / height , 1 , 10000 ); camera.position.x = 400; camera.position.y = 20; camera.position.z = 50; camera.up.x = 0; camera.up.y = 0; camera.up.z = 1; camera.lookAt( {x:0, y:0, z:0 } ); } var scene; function initScene() { scene = new THREE.Scene(); } var light; function initLight() { light = new THREE.DirectionalLight(0xFF0000, 1.0, 0); light.position.set( 100, 100, 200 ); scene.add(light); } var cube = Array(); function initObject(){ for(var i=0; i<3; i++){ cube[i] = new THREE.Mesh( new THREE.CubeGeometry(50,50,50), //形状の設定 new THREE.MeshLambertMaterial({color: 0xff0000}) //材質の設定 ); scene.add(cube[i]); cube[i].position.set(0,-100+100*i,0); } } var t=0; function loop() { t++; cube[0].rotation.set( t/100, 0, 0 ); cube[1].rotation.set( 0, t/100, 0 ); cube[2].rotation.set( 0, 0, t/100 ); renderer.clear(); renderer.render(scene, camera); window.requestAnimationFrame(loop); } function threeStart() { initThree(); initCamera(); initScene(); initLight(); initObject(); loop(); }

例2的执行结果： 三个立方体分别按照「x」「y」「z」轴进行回转。

### 例２的解说

和例1相比有三处新追加的地方

(1) 创建多个对象
(2) 「loop()」函数的定义和执行
(3) 追加指定物体回转角度的方法

### 创建多个对象

例２中通过数组来保存立方体。 Javascript 中数组可以按照
1var cube = Array();

这样的方式进行声明。 和C语言不一样，Javascript中定义数组对象不用指定长度。

1配列名[識別子].position.set(x, y, z);

### 「loop()」函数的定义和执行

「loop()」函数中通过「window.requestAnimationFrame(loop);」实现无限循环调用。 在本节开始的地方已接触过这种方式。 如果不让物体动起来，即使不断重复绘制，必然还是同样的静态的画面。 让例2中的物体回转起来。

### 指定物体的回转角度的方法

一般来说通过指定物体的位置和回转角度，三维空间中的物体就能展示任意的姿势。 例１中、接触过指定物体位置坐标的方法「position.set(x,y,z)」。 例２中、会同时指定物体的位置坐标和回转角度。 利用 [Mesh]类的对象的方法，让物体回转起来。
1オブジェクト名.rotation.set( theta_x, theta_y, theta_z );

而且、通过「theta_x」「theta_y」「theta_z」可以实现「x軸」「y軸」「z軸」各种角度的回转。 单位是弧度。

1 2 3オブジェクト名.rotation.x = theta_x; オブジェクト名.rotation.y = theta_y; オブジェクト名.rotation.z = theta_z;

实现动画的时候，值的变更一定要在渲染之前进行。 另外，渲染之前不要忘记了 「renderer.clear();」。如果不执行「renderer.clear();」 ， 上一帧绘制的内容就会遗留下来。

### 例２－２：tutorial2_2.html（相机的移动）

在上一节、实现了物体的移动动画效果、按照同样的步骤也可以来移动相机（视点）。 在例2的基础上，追加了相机(视点）的移动。
1 2 3 4 5 6 7 8 9 10 11 12var t=0; function loop() { t++; renderer.clear(); cube[0].rotation.set( t/100, 0, 0 ); cube[1].rotation.set( 0, t/100, 0 ); cube[2].rotation.set( 0, 0, t/100 ); camera.position.set( 400*Math.cos(t/100), 400*Math.sin(t/200), 50*Math.cos(t/50)); camera.lookAt( {x:0, y:0, z:0 } ); renderer.render(scene, camera); window.requestAnimationFrame(loop); }

可以看到相机（视点）发生了移动。 关于代码中，调用了Javascript中[Math]类的一些方法， 这些方法应该曾经在学校数学中学习过。（一览[点击这里](http://www.scollabo.com/banban/java/ref_19.html)） 比如

1 2 3 4 5 6Math.PI //\pi Math.cos(theta) //\cos(\theta) Math.asin(x) //\arcsin(x) Math.pow(x,2) //x^2 Math.sqrt(x) //\sqrt{x} Math.exp(x) //\exp{x}

>等。 更改相机参数的时候、一定要在「camera.lookAt」函数之前进行。 因为「camera.lookAt」函数、不仅仅是设置视点的中心、还能够让设置的相机参数发生实际的效果。

### 例２－３：tutorial2_3.html（设置环境光）

在Three.js里，光源有直接照射到物体的光源（平行光源，聚灯光源，点光源）和间接照射到物体的环境光源。 在此之前设置了直接光源， 本节开始设置环境光。 首先，确认一下加上环境光后物体的照射例子。

和例２－２相比、可以看出直接光照射不到的地方变得明亮一些了。 现实中，关于环境光不是光源直接照射形成的，而是因为各种物体漫反射形成的。 在计算机图形处理世界中，这样间接照射在物体上的光线被称为环境光。 在OpenGL中，环境光是通过直接光源的参数添加进去的，而Three.js中把环境光定义为光源的一种。 具体来说像下面这样向平行光源添加环境光。
1 2 3 4 5 6 7 8 9var light, light2; function initLight() { light = new THREE.DirectionalLight(0xFF0000, 1.0, 0); light.position.set( 50, 50, 200 ); scene.add(light);   light2 = new THREE.AmbientLight(0x555555); scene.add(light2); }

环境光和直接光都一样，利用相关构造函数声明对象然后添加到场景。 另外，环境光的强度可以通过构造函数的参数来指定（上面的例子中：0×555555）。 同时要让环境光照射到物体，只是设置光源是不够的，还需要向物体的材质中设置 [ambient] 属性。

1 2 3 4 5 6 7 8 9 10 11var cube = Array(); function initObject(){ for(var i=0; i<3; i++){ cube[i] = new THREE.Mesh( new THREE.CubeGeometry(50,50,50), new THREE.MeshLambertMaterial({color: 0xff0000, ambient:0xFF0000}) ); scene.add(cube[i]); cube[i].position.set(0,-100+100*i,0); } }

声明物体材质的构造函数中的参数里，设置 [ambient] 属性。 这个属性，通过16进制指定了对于环境光反射的强度。简单说，实际物体绘制的颜色，是根据环境光 和这个属性的乘积决定的， 如果环境光和ambient属性都是「0xFFFFFF」的话，那么物体将把环境光都反射出去而看起来为白色。

### 例２－４：tutorial2_4.html（影子的设置）

WebGL（OpenGL）中，没有绘制相对于某个光源的物体阴影的API, 需要使用其它途径来计算。 利用Three.js, 只要简单设置一下就可以实现。

设置物体的阴影， 需要设置下面这4个对象属性。
(1) 渲染器
(2) 光源
(3) 生成阴影的原物体
(4) 绘制阴影的物体
模式图就象下面这样。

![]( "设置物体的阴影")

具体就是在各函数中追加一些对象的属性。
1 2 3 4function initThree() { （省略） renderer.shadowMapEnabled = true;//影をつける(1) }

1 2 3 4 5function initLight() { （省略） light.castShadow = true;//影をつける（2） （省略） }
1 2 3 4 5 6 7function initObject(){ （省略） cube[i].castShadow = true;//影をつける（3）  （省略） plane.receiveShadow = true; //影をつける（4） （省略） }

使用阴影的时候需要注意的是，阴影的计算成本非常的高， 过于使用阴影功能会影响处理速度。

### 例２－５：tutorial2_5.html（纹理）

利用Three.js 能够非常简单就实现纹理的粘贴。

到这里指定颜色物体的材质中，按照选择的纹理就能够自动进行纹理粘贴。 具体的代码如下。
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16var sphere, sphere2 ; function initObject(){ var texture1 = new THREE.ImageUtils.loadTexture('earthmap1k.jpg'); sphere1 = new THREE.Mesh( new THREE.SphereGeometry(50,50,50), new THREE.MeshLambertMaterial({map: texture1}) ); scene.add(sphere1); sphere1.position.set(0,0,0); var texture2 = new THREE.ImageUtils.loadTexture('moonmap1k.jpg'); sphere2 = new THREE.Mesh( new THREE.SphereGeometry(5,20,20), new THREE.MeshLambertMaterial({map: texture2}) ); scene.add(sphere2); }

但是，因为WebGL禁止使用跨域纹理资源，所以也不能读取本地的纹理资源。

## 鼠标事件

### 鼠标事件例

事件名 意思 onmousedown 鼠标被按下的时候 onmouseup 鼠标弹上的时候 onmousemove 鼠标移动的时候 onmouseover 鼠标移动到物体的上面的时候 onmouseout 鼠标从物体上面移出物体范围的时候

利用鼠标事件，在浏览器中进行特定的动作时， 就能够调用Javascript功能。 而且，事件发生时鼠标的位置也能够获取到。 关于鼠标事件的处理，具体参照下面。

1 2 3 4 5 6window.onmousedown = function (ev){ var x1 = ev.clientX; //マウスのx座標（ブラウザ上の座標）の取得 var y1 = ev.clientY; //マウスのy座標（ブラウザ上の座標）の取得 var x2 = ev.screenX; //マウスのx座標（ディスプレイ上の座標）の取得 var y2 = ev.screenY; //マウスのy座標（ディスプレイ上の座標）の取得 }

### 例３：tutorial3.html（鼠标事件）

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28var down = false; var sy = 0, sz = 0; window.onmousedown = function (ev){ //マウスダウン if (ev.target == renderer.domElement) { down = true; sy = ev.clientX; sz = ev.clientY; } }; window.onmouseup = function(){ //マウスアップ down = false; }; window.onmousemove = function(ev) { //マウスムーブ var speed = 2; if (down) { if (ev.target == renderer.domElement) { var dy = -(ev.clientX - sy); var dz = -(ev.clientY - sz); camera.position.y += dy*speed; camera.position.z -= dz*speed; sy -= dy; sz -= dz; } } } window.onmousewheel = function(ev){ //マウスホイール var speed = 0.2; camera.position.x += ev.wheelDelta * speed ; }

到这里为止，对如何使用 Three.js 进行了简单概要性的介绍。 下一节就进入如何实现物理模拟的环节了。

分享到： [QQ空间]( "分享到QQ空间") [新浪微博]( "分享到新浪微博") [腾讯微博]( "分享到腾讯微博") [微信]( "分享到微信") [更多](http://www.jiathis.com/share) [5]()
Posted in [WebGL教程](http://www.hiwebgl.com/?cat=17 "查看 WebGL教程 中的全部文章"). Bookmark the [permalink](http://www.hiwebgl.com/?p=1058 "Permalink to 利用HTML5构筑物理模拟环境～ WebGL库Three.js入门（１／３）").

[← BigWorld 将在 4.0 版本支持 HTML5 全球首家](http://www.hiwebgl.com/?p=1055)

[Firefox 和 WebKit 开始提供纹理压缩扩展支持 →](http://www.hiwebgl.com/?p=1095)
### 3 trackbacks

[利用HTML5构筑物理模拟环境～ WebGL库Three.js入门（１／３） | 移动开发者大会](http://www.himdc.com/html5/63.html) 2012 年 5 月 8 日 下午 2:24 [Canvas 3D 学习笔记 | He Peng's Blog](http://hepeng.net/canvas-3d-%e5%ad%a6%e4%b9%a0%e7%ac%94%e8%ae%b0-%e4%b8%80/) 2013 年 3 月 17 日 下午 9:06 [Three.js学习笔记 - “我和小伙伴都惊呆了”的特效和Three.js初探 - Tony的工作站](http://tonythegod.eu5.org/three-js-study-notes-study-on-three-js/) 2013 年 7 月 9 日 下午 10:40

### 28 comments

1. ![]() yzx 2012 年 5 月 16 日 下午 10:17

期待2/3/翻译的完成

[回复](http://www.hiwebgl.com/?p=1058&replytocom=868#respond)
1. ![]() echoff 2012 年 5 月 22 日 上午 9:17

不错 谢谢楼主的辛勤劳动

[回复](http://www.hiwebgl.com/?p=1058&replytocom=891#respond)
1. ![]() bin381 2012 年 6 月 13 日 上午 9:53

期待完成

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1112#respond)
1. ![]() AHLONG 2012 年 6 月 18 日 上午 10:40

three.js 看起来比 Oak3D_v_0_5.js 简单许多，没有vs、PS，程序段看起来也很简短。初学者用哪个好呢？为什么hi,webgl的webgl教程要采用 OAK3D_V_0_5.JS 呢？

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1133#respond)

* ![]() admin 2012 年 6 月 21 日 下午 3:09

HIWEBGL网站上的教程用的都是WebGL原生API，没有用OAK3D API，只是数学运算的部分用了OAK3D的数学库。你可以去OAK3D的官网看看他们的官方DEMO，对比一下源代码就知道了。

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1139#respond)

* ![]() yonger 2013 年 3 月 20 日 下午 5:50

我看了一下，OAK3d的DEMO，感觉还是THree.js的结构更清晰，如果单纯从代码行数来看，貌似Three.js更少吧

[回复](http://www.hiwebgl.com/?p=1058&replytocom=7183#respond)
* ![]() 倩倩 2012 年 7 月 16 日 上午 9:01

楼主你用的什么平台？three.js的代码提示是怎么弄的？求指点

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1238#respond)
* ![]() FlyHorse 2012 年 7 月 31 日 下午 2:33

感谢楼主的辛勤劳动，期待下一节！！！

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1359#respond)
* ![]() hava 2012 年 8 月 3 日 下午 1:44

感谢楼主劳动，期待下面的内容，感谢给我开阔视野 Three.js是好东西

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1379#respond)
* ![]() gj 2012 年 8 月 13 日 下午 2:48

感谢楼主的辛勤劳动。
下面发布我找到的原文，为那些对three。js下2部分（2/3） （3/3）饥渴的同学提供的。 虽然在日语。 但是你可以通过goole 翻译来翻译整个页面。 翻译的不错。
[http://www.field-and-network.jp/rihei/20120302143134.php](http://www.field-and-network.jp/rihei/20120302143134.php)

对菜鸟们 讲解下 goole 是怎么翻译网页的。
翻译网站是 translate.google.com， 在语言里选择 日文 –> 中文，然后把你想翻译的网站的网站输入到输入框里就行了。
祝大家学习愉快 o(∩_∩)o

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1499#respond)
* ![]() 研究webgl的甘苦人 2012 年 9 月 25 日 下午 1:03

請問版主
WebGL原生API文件在哪可以看得到（網頁）
我只能找到 Oak3D 的 API 文件 與 THREE 的 API

另外請教一下
如果 .html 網頁中有使用到 OAK3D 的方法的話
(例如：程式碼中使用到 “okMat4Proj” 與 “okMat4Trans” )
是不是就必須得在程式碼的前面加上

其中 “SRC=”OAK3D_V_0_5.JS” 這個檔案應該要放在跟要執行的 .html 檔的同一層目錄底下

我說的對嗎? @@
P.S. 剛接觸 WebGL 這一塊, 有些關於環境的設定或函式庫的種類都還不太清楚, 還請見諒

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1705#respond)

* ![]() hjlld 2012 年 10 月 9 日 下午 5:51

你可以在Khronos官网上找到WEBGL的标准，如果是具体API的用法和教程，可以参考OPENGL ES 2.0的教程，因为几乎都是一样的。

第二个是对的，因为你要调用OAK3D的接口就必须先置入oak3d.js的库文件。

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1766#respond)
* ![]() abcdefg 2012 年 10 月 30 日 下午 9:43

楼主幸苦，Three.JS资源本开就少，看了楼主的介绍受益匪浅。

[回复](http://www.hiwebgl.com/?p=1058&replytocom=1928#respond)
* ![]() 研究webgl的甘苦人 2012 年 12 月 3 日 下午 10:25

請問版主在 HiwebGL 中文教程的程式碼中使用到的 “okMat4Proj” 與 “okMat4Trans”
有沒有 WebGL 官方的寫法? 還是只能仰賴 Oak3D 所提供的這兩個方法呢?

另外，three.js 有沒有方法能做到跟 “okMat4Proj” 與 “okMat4Trans” 一樣的效果？

您辛苦了！感激不盡～

[回复](http://www.hiwebgl.com/?p=1058&replytocom=2152#respond)
* ![]() [3d打印](http://www.513dp.com/) 2012 年 12 月 27 日 上午 10:06

感谢楼主，我正需要学习这方面的知识。我需要在我的3d打印网站上用three.js来展示3d模型。我的网址是：http://www.513dp.com。有兴趣可以到我的网站看一看。

[回复](http://www.hiwebgl.com/?p=1058&replytocom=2356#respond)
* ![]() THREE菜鸟 2013 年 1 月 17 日 上午 10:43

楼主，请问一下，3d模型的贴图是怎么生成的呀，比如说哪个地球模型的jpg贴图，如下
function initObject(){
var texture1 = new THREE.ImageUtils.loadTexture(‘earthmap1k.jpg’);
sphere1 = new THREE.Mesh(
new THREE.SphereGeometry(50,50,50),
new THREE.MeshLambertMaterial({map: texture1})
);
如果，我做一个一个机器人，已经在建模工具上贴好了图，并且导出了obj与mtl,如何导入带图的模型到three.js啊。。我现在通过github上的python脚本，导出了json，并导入了three.js，但是不知道怎么导入贴图。。另外three.js的动画，比如说模型的动作，大概如何实现呀（目前，只会js的loop函数，操纵坐标移动）

[回复](http://www.hiwebgl.com/?p=1058&replytocom=2745#respond)
* ![]() [structured settlements calculator](http://charlesdon541.inube.com/blog/) 2013 年 2 月 3 日 上午 12:28

It’s not my first time to pay a visit this web page, i am browsing this site dailly and get nice data from here all the time.

[回复](http://www.hiwebgl.com/?p=1058&replytocom=3268#respond)
* ![]() YURI 2013 年 3 月 12 日 下午 5:21

等啊等啊。。。。怒求。。。。。

[回复](http://www.hiwebgl.com/?p=1058&replytocom=6837#respond)
* ![]() WILL 2013 年 5 月 6 日 上午 5:19

期待下一节，谢谢！

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8027#respond)
* ![]() [Sheldon](http://knowtips.ca/user/view.php?id=4927&course=1) 2013 年 5 月 10 日 下午 11:43

Hey! I know this is kinda off topic however I’d figured I’d ask.
Would you be interested in trading links or maybe guest authoring a blog post or vice-versa?
My site goes over a lot of the same topics as yours and I believe we could greatly benefit from each other.
If you might be interested feel free to send me an e-mail.
I look forward to hearing from you! Terrific blog by the way!

My personal website on modern technology: [Sheldon](http://knowtips.ca/user/view.php?id=4927&course=1)

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8268#respond)
* ![]() [Zak](http://farmrich.com/content/quick-look-multichannel-loudspeakers) 2013 年 5 月 11 日 上午 4:21

Hello, i think that i saw you visited my web site thus
i came to “return the favor”.I am trying to find things
to enhance my web site!I suppose its ok to use
a few of your ideas!!

Our blog site regarding technological know-how: [Zak](http://farmrich.com/content/quick-look-multichannel-loudspeakers)

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8270#respond)
* ![]() [金地自在城社区](http://www.myjindy.net/) 2013 年 5 月 15 日 下午 5:20

楼主翻译的不错，请问第二章什么什么有啊？？？？？
还有楼主有空帮我看看我的网站([http://www.myjindy.net](http://www.myjindy.net/))怎么美化好不？

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8317#respond)
* ![]() 小巨蛋 2013 年 5 月 29 日 上午 10:32

楼主请问一下 我看了你的，有几个地发不理解，我是刚刚接触WEBGL的，
在 tutorial2_4中 plane.receiveShadow = true; 这个为什么没用，下面报错：找不到plane,
tutorial2_5中，报这个错，DEPRECATED: .setClearColorHex() is being removed. Use .setClearColor() instead. 纯菜鸟球帮助

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8633#respond)

* ![]() 蓝明乐 2013 年 6 月 1 日 下午 3:15

那个plane你可以使用的PlaneGeometry
var plane = new three.PlaneGeometry(width, height, widthSegments, heightSegments);
来实现！你自己看下文档，真希望能翻译中文的API,想OAK3D那样多好啊！要不看着three简单好用才学的，

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8705#respond)
* ![]() 蓝明乐 2013 年 6 月 1 日 下午 3:11

感谢分享先！不知道为什么我纹理就是不出效果！真的搞不懂！哭！其他的都能实现，就是纹理没有显示！连基本的图形都没有。

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8704#respond)
* ![]() [金地自在城社区](http://www.myjindy.net/) 2013 年 6 月 4 日 上午 11:02

webgl的缺点就是很多用户的浏览器不支持

[回复](http://www.hiwebgl.com/?p=1058&replytocom=8768#respond)
* ![]() [windows 8 activator](http://www.youtube.com/watch?v=AZQRgskqj58) 2013 年 7 月 3 日 上午 1:11

I’ve been surfing online more than 3 hours today, yet I never found any interesting article like yours. It’s pretty
worth enough for me. Personally, if all site owners and bloggers made good content as you did, the net
will be a lot more useful than ever before.

[回复](http://www.hiwebgl.com/?p=1058&replytocom=9329#respond)
* ![]() [want to use whatsapp](http://whatsappforpcfreedownload.weebly.com/) 2013 年 7 月 3 日 上午 6:05

Excellent website you have here but I was wanting to know
if you knew of any community forums that cover the same topics talked about in this article?
I’d really like to be a part of community where I can get opinions from other experienced individuals that share the same interest. If you have any recommendations, please let me know. Kudos!

[回复](http://www.hiwebgl.com/?p=1058&replytocom=9335#respond)
### Post a Comment [点击这里取消回复。]()

电子邮件地址不会被公开。 必填项已用 * 标注

姓名 *

电子邮件 *

站点

评论

您可以使用这些 HTML 标签和属性：

<a href="" title=""> <abbr title=""> <acronym title=""> <b> <blockquote cite=""> <cite> <code> <del datetime=""> <em> <i> <q cite=""> <strike> <strong>

*
* 
### 加入我们的QQ群

**HiWebGL技术讨论群**
①群群号174563471（已满）
②群群号133703893（已满）
③群群号196778533（已满）
④群群号217873851（已满）
⑤群群号225923498（已满）
500人群群号219549633
欢迎各位程序员和工程师加入！
[![点击这里加入此群]( "点击这里加入此群")](http://qun.qq.com/#jointhegroup/gid/219549633)
**HiWebGL产业讨论群**
①群群号176289597（已满）
②群群号216936388
欢迎关注Web3D发展的从业者与富有理想与激情的创业者朋友加入！
[![点击这里加入此群]( "点击这里加入此群")](http://qun.qq.com/#jointhegroup/gid/216936388)
* 
### 近期评论

* Larryzhi 发表在《[Lesson 14 镜面高光和载入JSON模型](http://www.hiwebgl.com/?p=384#comment-10156)》
* admin 发表在《[Oak3D 教程 – 第一课: Oak3D Engine 绘制流程](http://www.hiwebgl.com/?p=973#comment-9939)》
* J.C 发表在《[用WebGL实现光线追踪！](http://www.hiwebgl.com/?p=27#comment-9840)》
* J.C 发表在《[用WebGL实现光线追踪！](http://www.hiwebgl.com/?p=27#comment-9839)》
* 小婷 发表在《[Oak3D 教程 – 第一课: Oak3D Engine 绘制流程](http://www.hiwebgl.com/?p=973#comment-9711)》
* 
### 分类目录

* [Demo大赏](http://www.hiwebgl.com/?cat=16 "查看 Demo大赏 下的所有文章")
* [HTML5新闻](http://www.hiwebgl.com/?cat=29 "查看 HTML5新闻 下的所有文章")
* [Oak3D教程](http://www.hiwebgl.com/?cat=57 "Oak3D中文教程")
* [WebGL教程](http://www.hiwebgl.com/?cat=17 "查看 WebGL教程 下的所有文章")
* [WebGL新闻](http://www.hiwebgl.com/?cat=3 "WebGL新闻")
* [博文精选](http://www.hiwebgl.com/?cat=10 "查看 博文精选 下的所有文章")
* [推荐](http://www.hiwebgl.com/?cat=4 "推荐阅读")
* [未分类](http://www.hiwebgl.com/?cat=1 "查看 未分类 下的所有文章")
* 
### 文章归档

* [2013 年七月](http://www.hiwebgl.com/?m=201307 "2013 年七月")
* [2013 年五月](http://www.hiwebgl.com/?m=201305 "2013 年五月")
* [2013 年四月](http://www.hiwebgl.com/?m=201304 "2013 年四月")
* [2013 年三月](http://www.hiwebgl.com/?m=201303 "2013 年三月")
* [2013 年一月](http://www.hiwebgl.com/?m=201301 "2013 年一月")
* [2012 年十一月](http://www.hiwebgl.com/?m=201211 "2012 年十一月")
* [2012 年九月](http://www.hiwebgl.com/?m=201209 "2012 年九月")
* [2012 年六月](http://www.hiwebgl.com/?m=201206 "2012 年六月")
* [2012 年五月](http://www.hiwebgl.com/?m=201205 "2012 年五月")
* [2012 年四月](http://www.hiwebgl.com/?m=201204 "2012 年四月")
* [2012 年三月](http://www.hiwebgl.com/?m=201203 "2012 年三月")
* [2012 年二月](http://www.hiwebgl.com/?m=201202 "2012 年二月")
* [2012 年一月](http://www.hiwebgl.com/?m=201201 "2012 年一月")
* [2011 年十二月](http://www.hiwebgl.com/?m=201112 "2011 年十二月")
* [2011 年十一月](http://www.hiwebgl.com/?m=201111 "2011 年十一月")
* [2011 年十月](http://www.hiwebgl.com/?m=201110 "2011 年十月")
* [2011 年九月](http://www.hiwebgl.com/?m=201109 "2011 年九月")
* [2011 年八月](http://www.hiwebgl.com/?m=201108 "2011 年八月")

Powered by [Wordpress](http://wordpress.org/ "Semantic Personal Publishing Platform") and [WPCrunchy](http://wpcrunchy.com/ "Free Premium Quality Wordpress Themes")

Designed by [best web hosting](http://www.hostreviewgeeks.com/ "best web hosting"). In collaboration with [VPS Hosting](http://www.hostv.com/ "VPS Hosting"), [Broadway Tickets](http://broadwaytickets.co/ "Broadway Tickets"), [Free Wordpress Themes](http://wpcorner.com/ "Free Wordpress Themes").
