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
 
熟悉Android系统和HTML编程的人可能都听说过Android提供的一个重要类android.webkit.WebView，它继承于View类，这是它同其它很多控件的相似之处。不同之处在于，它能够用来渲染网页。当前，WebView的实现是基于现有的缺省WebKit内核（Android缺省浏览器是基于WebView构建），它不同于chromium所使用的WebKit内核，虽然它们都叫WebKit.

由此可见，表现不一致的根本原因是内核对页面的渲染机制不同，那么它们都叫WebKit，为什么还会有不一样的地方呢？

读了这篇文章：[开发者需要了解的WebKit](http://www.infoq.com/cn/articles/webkit-for-developers) 你就会了解到了，同是WebKit它也有不同的Port,它们专注于不同的部分，每个WebKit port中有共享的部分，但是也有很大一部分功能是不会共享的，其中就包括JS引擎。

所以我停止了说：「为什么ios都行了安卓怎么就不行呢？」，而是埋头开始修bug...虽然有时候这些bug不是那么好修...

### 什么是WebView

作为一个前端儿，你肯定听过WebView这个词儿。我也听过，也好奇过，把我的网页放到手机上看就叫WebView啦？WebView到底是什么呢？

其实WebView是类名：

- 在Android中，这是个继承于View类的类android.webkit.WebView，用来布局和渲染网页；
- 在iOS中，这个类叫UIWebView,同样是应用程序的UI接口。

我们前端怎么就会对客户端的类名这么熟悉了呢？

如果你是个有见识的前端那么你肯定听过Hybrid App（混合模式移动应用）.这是指介于web-app、native-app这两者之间的app，兼具“Native App良好用户交互体验的优势”和“Web App跨平台开发的优势”。
在我看来，这就是前端跟iOS开发和安卓开发一起做的事情嘛。它的实现就离不开WebView了，因为客户端需要渲染我提供的H5页面，那具体怎么实现呢？我也不知道了...去问客户端开发吧...

### Touch事件





### 参考资料

[开发者需要了解的WebKit](http://www.infoq.com/cn/articles/webkit-for-developers)  

[理解Webkit和Chromium：基于Chromium内核的Android WebView](http://blog.csdn.net/milado_nju/article/details/8926720)  

[webkit百度百科](http://baike.baidu.com/view/1510583.htm)  

[Chrom for Android 你必须知道的N件事](http://archives.guao.hk/posts/google-chrome-for-android-must-knows.html)  

[指尖下的JS（1）](http://www.cnblogs.com/pifoo/archive/2011/05/23/webkit-touch-event-1.html)  

[指尖下的JS (2)](http://www.cnblogs.com/pifoo/archive/2011/05/22/webkit-touch-event-2.html)  

[指尖下的JS (3)](http://www.cnblogs.com/pifoo/archive/2011/05/22/webkit-touch-event-3.html)

[android vs iPhone-touch Events](http://mattiacci.com/2010/11/18/android-vs-iphone-touch-events/)

[Touching and Gesturing on iPhone,Android and more](https://www.sitepen.com/blog/2011/12/07/touching-and-gesturing-on-iphone-android-and-more/)
