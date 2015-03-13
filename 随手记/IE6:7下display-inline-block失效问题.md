### 问题：

我给原来是block元素的li标签设置了display为inline-block，在chrome等的其他浏览器上都是ok的，ie6/7下失效。

### 原因：

- 1.inline元素的display属性设置为inline-block时,所有的浏览器都支持；
- 2.block元素的display属性设置为inline－block时，ie6/7不支持；

### 解决方法：

	div { display:inline-block; _zoom:1;_display:inline;} /*IE6*/
	div { display:inline-block; *zoom:1;*display:inline;} /*IE7*/