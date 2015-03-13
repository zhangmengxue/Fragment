### 报错:

我在chrome和其他浏览器上都正常的页面，放在ie6/ie7下报错：Expected identifier string or number，嗯，但是ie8下是好的。

### 问题：

我在某个对象的结尾不小心加了个逗号，像这样（33后面的逗号）：

  var p = {
    name:"Jack",
    age:33,
  };

当我把结尾的逗号去掉，这个世界都美好了.....

### 原因：

浏览器的解析的不同：

－ 1.对象最后面的逗号：

  var p = {
    name:"Jack",
    age:33,
  };

这样不严谨的写法，除了ie6，7会报错，别的浏览器都会忽略最后那个逗号，正确执行（包括ie8）。

－ 2.数组最后面的逗号：

  var ary = [1,2,];
  console.log(ary.length);

这种写法IE6，7，8下输出length都会是3，它们会在最后填补一个数组元素undefined，但是在ie9+和其它浏览器中都是2；

曾经有人利用这个特性来判断ie浏览器：

  var ie = !-[1,];
  alert(ie);

可见在ie9+它已经不起作用了。

－ 3.数组中间的逗号：

  var arr = [1, 2,, 3];
  for (var i = 0; i < arr.length; i++) {
    alert(arr[i]);
  }

这时所有的浏览器都会输出1，2，undefined，3.可见当多余的逗号出现在数组中间而不是末尾的时候，各个浏览器的处理方式都是相同的，也就是在多余的逗号前填充undefined。


