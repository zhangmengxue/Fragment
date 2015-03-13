### 需求：

一张小图片单独请求一次多郁闷，不如把它转成base64吧，于是就去这里：[编码base64](http://tool.css-js.com/base64.html)

### 问题：

我的图片是png－8的，于是它就有了锯齿。

### 原因：

gif图片或png8图片是存在杂边锯齿的问题的，具体可以看这里:[关于gif图片（或png8）杂边锯齿的问题](http://www.zhangxinxu.com/wordpress/2009/09/%E5%85%B3%E4%BA%8Egif%E5%9B%BE%E7%89%87%EF%BC%88%E6%88%96png8%EF%BC%89%E6%9D%82%E8%BE%B9%E9%94%AF%E9%BD%BF%E7%9A%84%E9%97%AE%E9%A2%98/#g1_3)

### 解决：

换成png24的图片，问题就解决了。
