title: "移动端开发概览【webview和touch事件】"
date: 2015-03-21 14:10:00
tags:
- 博客
- 跨终端
---

作为一个前端，而且作为一个做移动端开发的前端，那意味着你要有三头六臂，跟iOS开发哥哥一起打酱油，跟Android开发哥哥一起修bug...

### Android vs Ios

我在webkit内核的chrome中进行开发的页面，拿着iPhone和安卓机来进行测试，传说中它们的浏览器内核也是WebKit，那么问题来了，同样的页面为什么在ios中和安卓中表现不同，出现了各种稀奇古怪的bug...

我尝试找下两者的根本区别：

- iOS
 
随着2007年6月29日iPhone的上市，WebKit进入iPhone OS平台，经过Apple的定制，成为iPhone OS平台独一无二的排版引擎；

- Android 

在旧版本的安卓中：

熟悉Android系统和HTML编程的人可能都听说过Android提供的一个重要类android.webkit.WebView，它继承于View类，这是它同其它很多控件的相似之处。不同之处在于，它能够用来渲染网页。当前，WebView的实现是基于现有的缺省WebKit内核（Android缺省浏览器是基于WebView构建），它不同于chromium所使用的WebKit内核，虽然它们都叫WebKit.

但是，在最新的Android 4.4 Kitkat版本中，原本基于Android WebKit的WebView实现被换成基于Chromium的WebView实现.

- 结论

由此可见，虽然它们都叫WebKit，但是WebKit和WebKit也是不同的：

读了这篇文章：[开发者需要了解的WebKit](http://www.infoq.com/cn/articles/webkit-for-developers) 你就会了解到了，同是WebKit它也有不同的Port,它们专注于不同的部分，每个WebKit port中有共享的部分，但是也有很大一部分功能是不会共享的，其中就包括JS引擎。

所以我停止了说：「为什么ios都行了安卓怎么就不行呢？」，而是埋头开始修bug...虽然有时候这些bug不是那么好修...

### 什么是WebView

作为一个前端儿，你肯定听过WebView这个词儿。我也听过，也好奇过，把我的网页放到手机上看就叫WebView啦？WebView到底是什么呢？

其实WebView是类名或者说它是一个API(在我看来)：

- 在Android中，这是个继承于View类的类android.webkit.WebView，用来布局和渲染网页；
- 在iOS中，这个类叫UIWebView,同样是应用程序的UI接口。

我们前端怎么就会对客户端的类名这么熟悉了呢？

如果你是个有见识的前端那么你肯定听过Hybrid App（混合模式移动应用）.这是指介于web-app、native-app这两者之间的app，兼具“Native App良好用户交互体验的优势”和“Web App跨平台开发的优势”。
在我看来，这就是前端跟iOS开发和安卓开发一起做的事情嘛。它的实现就离不开WebView了，因为客户端需要渲染我提供的H5页面，那具体怎么实现呢？我也不知道了...去问客户端开发吧...

### Touch事件

现在我们知道了手机跟手机不同，WebKit与WebKit的不同，先知道着，我先从头看一下touch事件，再看看他在不同的手机和手机中会有什么问题。

Touch事件：
touchstart:当一个手指放在屏幕上时触发；
touchend:当一个手指从屏幕上移走的时候触发；
touchmove:当一个手指已经在屏幕上并且在屏幕上移动时触发；
touchcancel:如果太多个手指在屏幕上或一个其他动作发生时触发。

Touch事件中事件对象的属性：
touches: 它包含了每个手指当前touch屏幕的一系列信息；
targetTouches: 像touches一样，但是提供当前手指的touch信息；
changedTouches: 包含每一个手指的touch改变的信息。

下面详细介绍下这三个属性:
1.当你放下一个手指的时候，上面三个属性会提供相同的信息；
2.当你放下第二个手指的时候,touches对象会包含两条信息，每个手指都有一个；targetTouches只有在第二根手指放在了同第一根手指相同的节点上的时候会包含两条信息（否则的话它只包含第二根手指的信息）；changedTouches只会包含跟第二根手指相关的信息.
3.如果几乎同是两个手指触碰屏幕，在changedTouches中会有两个手指的信息。
4.如果我们在屏幕上移动手指，唯一会改变的对象是changedTouches，它会包含我们移动的一个或两个手指的信息；
5.如果抬起一个手指，它的信息会从touches和targetTouches对象中移除，但是在changedTouches会找到它的信息；
6.当我们把最后一个手指从屏幕上移走，touches和targetTouches对象会为空了，但是changedTouches将会保留最后这跟手指的信息。

touches对象中包含的每个手指的信息列表中也有下面这些我们在鼠标事件中熟悉的属性：

identifier - 一个标识符，对于每个touch点（手指）是独一无二的；
target -当前手指touch的dom节点；
clientX/clientY -touch事件发生时相对视口(viewport)的坐标(包含滚动)
screenX/screenY -相对于屏幕的坐标
pageX/pageY -相对于整个文档的坐标

[简单的touch事件实现的拖拽小demo]()

### Touch事件相关tip

- 检测设备是否为触摸屏设备：
通常我们检测设备是否为触摸屏有两个方法，一个是通过UA还有一个就是特征检测：
    var isTouch = !!ua.match(/AppleWebkit.*Mobile.*/) || 'ontouchstart' in document.documentElement;
但是我前两天读了一篇非常好的文章。这样进行判断已经不再准确了。当笔记本为触摸屏的笔记本时.

当前，我们可以通过W3C Interaction Media Features给出的方案来判断。

W3C在Media Query Levle 4中增加了pointer类别的特征查询，pointer有三个条件选项：none、fine 和 coarse

None，当前设备没有任何除键盘或触控板之外的输入方式
Fine，当前设备使用了鼠标来操作
Coarse, 表示当前设备至少支持触屏操作，也可能同时支持鼠标

    // 既有触屏也有鼠标
    matchMedia("(pointer:coarse)").matches &&  matchMedia("(pointer:fine)").matches
    // 只是触屏
    matchMedia("(pointer:coarse)").matches &&  !matchMedia("(pointer:fine)").matches
    // 只是鼠标
    !matchMedia("(pointer:coarse)").matches &&  matchMedia("(pointer:fine)").matches
    // 没有任何一种
    matchMedia("(pointer:none)").matches

所以，比较稳妥的判断触摸屏的方法应该是这样的：

    function isTouchScreen(){
       if(window.matchMedia){
          // W3C way
          if(!matchMedia("(pointer:coarse)").matches && matchMedia("(pointer:fine)").matches){
              return false
          }
          // Firefox way: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries#-moz-touch-enabled
          if(matchMedia("-moz-touch-enabled: 0").matches){
              return false
          }
       }
       if(window.ontouchstart != null){
          return true
       }
    }

- zepto中的tap事件

[zepto touch事件源码](https://github.com/madrobby/zepto/blob/master/src/touch.js)

地球人都知道zepto封装的Touch事件都绑在了document上。于是问题接踵而至。

tap点击穿透发生的条件：
1.两个DOM节点一个在上一个在下；
2.两个节点是父与子的关系；
3.同时绑定了tap事件；
4.如果下层元素是input元素的话,会触发input元素获得焦点(这时阻止冒泡，阻止浏览器的默认行为也有可能不同)。

解决办法：
1.使用github上有一个叫做fastclick的库；

2.监听touchend事件，并在事件中使用preventDefault()阻止冒泡；

3.使用css3的pointer-events=true,pointer-events=none切换来实现；

4.延迟一定的时间来处理事件。

5.合理的改善dom结构，下层是否可以用a标签作为链接处理，同是尝试改善父子层的关系，合理的避免点击穿透；

6.如果还不奏效，终极解决方案是用click提代tap;







### 参考资料

[开发者需要了解的WebKit](http://www.infoq.com/cn/articles/webkit-for-developers)  

[理解Webkit和Chromium：基于Chromium内核的Android WebView](http://blog.csdn.net/milado_nju/article/details/8926720)  

[理解WebKit和Chromium：Android 4.4 上的Chromium WebView](http://blog.csdn.net/milado_nju/article/details/17098399)

[webkit百度百科](http://baike.baidu.com/view/1510583.htm)  

[Chrom for Android 你必须知道的N件事](http://archives.guao.hk/posts/google-chrome-for-android-must-knows.html)  

[指尖下的JS（1）](http://www.cnblogs.com/pifoo/archive/2011/05/23/webkit-touch-event-1.html)  

[指尖下的JS (2)](http://www.cnblogs.com/pifoo/archive/2011/05/22/webkit-touch-event-2.html)  

[指尖下的JS (3)](http://www.cnblogs.com/pifoo/archive/2011/05/22/webkit-touch-event-3.html)

[android vs iPhone-touch Events](http://mattiacci.com/2010/11/18/android-vs-iphone-touch-events/)

[Touching and Gesturing on iPhone,Android and more](https://www.sitepen.com/blog/2011/12/07/touching-and-gesturing-on-iphone-android-and-more/)

[关于交互模式的媒体查询](http://dev.w3.org/csswg/mediaqueries-4/#mf-interaction)
