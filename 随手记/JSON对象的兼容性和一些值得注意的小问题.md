### JSON对象的一些方法

+ JSON.parse()解析JSON字符串返回解析后的JSON对象;
+ JSON.stringify()返回指定JSON对象的字符串形式;

### JSON对象的兼容性问题

IE8+才会支持JSON对象。我们可以通过代码自己模拟原生的JSON对象：[模拟原生JSON对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON)

*eval和new Function*

看模拟原生JSON对象的实现我们可以看到实现思路：

+ 1.eval(str)
+ 2.(new Function('return'+str))()

当我们实际的操作一下，还是会有些小问题的.

### 一些小问题：

+ 1.利用eval()函数解析JSON对象：

```javascript
eval('{"name":"skylar"}') // SyntaxError: Unexpected token :
```

这是为什么呢？其实是{}做的怪。{}的用处无非两个，（1）用于创建对象；（2）创建代码块，并且当{}位于语句的开头时，代码就会被当成代码块来解析。这里eval()中明显就是把对象当成代码块来解析了，所以就报错了。

- 解决方法：

```javascript
eval("(" + str + ")");  // {"name": "skylar"}并且注意JSON中的key和value要使用双引号，避免出错
```
或
```javascript
eval('0,' + str);
```

+ 2. JSON.parse()不允许逗号结尾：

```javascript
// both will throw a SyntaxError
JSON.parse("[1, 2, 3, 4, ]");
JSON.parse("{ \"foo\" : 1, }");
```

### 参考资料：

[JSON.stringify](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

[JSON.parse](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

[JSON对象](http://javascript.ruanyifeng.com/stdlib/json.html)


