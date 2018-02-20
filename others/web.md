## Web开发

### 工具

- **Chrome Dev Tools** :

[Chrome Dev Tools 浅析：成为更高效的开发人员](http://blog.jobbole.com/22065/)


- 从这里查看各种浏览器对于某些CSS、JS、SVG、HTML5特性的支持情况：http://caniuse.com
- Firebug调试IE调试

IE 8没有内置好用的前端调试工具，可以通过在待调试的页面中引入Firebug Lite来调试。

```html
<script type="text/javascript" src="https://getfirebug.com/firebug-lite.js"></script>
```

http://getfirebug.com/firebuglite

- [浏览器开发工具的秘密](http://jinlong.github.io/blog/2013/08/29/devtoolsecrets/)
- [构建初级前端页面重构开发环境](http://blog.wpjam.com/article/build-frontend-development-environment/)
- [网页前端开发工具推荐！超实用的CSS库、框架大全](http://www.uisdc.com/css-and-framework-tool)


### 浏览器兼容问题

1.
对于页面隐藏元素，应该使用

```html
<input type="hidden" name="..." value="..." />
```

来实现，这种方式所有浏览器应该都是兼容的。

对于非隐藏输入域的隐藏元素，IE并不支持。

------

2.
IE 8以及更早的版本不支持JavaScript字符串的trim()方法。可如下解决

```javascript
var browser = navigator.appName;
if(browser === 'Microsoft Internet Explorer'){
    String.prototype.trim = function() {
        return this.replace(/(^\s*)|(\s*$)/g, "");
    }
}
```

另外，还有ltrim()和rtrim()方法。

也可以使用jQuery提供的工具方法 `$.trim(str)` 。

http://api.jquery.com/jQuery.trim/

3.
IE 8以及更早也不支持JavaScript数组的forEach方法，有两种解决方案：1）以for循环
替代；2）为Array.prototype添加forEach方法。

```javascript
if ( !Array.prototype.forEach ) {
    Array.prototype.forEach = function(fn, scope) {
        for(var i = 0, len = this.length; i < len; ++i) {
            fn.call(scope, this[i], i, this);
        }
    }
}
```

https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Array/forEach

4.
对于JavaScript数组的map方法，IE 8也不支持，需自己实现。

```javascript
if (!Array.prototype.map) {
    Array.prototype.map = function(callback, thisArg) {
        var T, A, k;

        if (this == null) {
            throw new TypeError(" this is null or not defined");
        }
        // 1. 将O赋值为调用map方法的数组.
        var O = Object(this);
        // 2.将len赋值为数组O的长度.
        var len = O.length >>> 0;
        // 4.如果callback不是函数,则抛出TypeError异常.
        if ({}.toString.call(callback) != "[object Function]") {
            throw new TypeError(callback + " is not a function");
        }
        // 5. 如果参数thisArg有值,则将T赋值为thisArg;否则T为undefined.
        if (thisArg) {
            T = thisArg;
        }
        // 6. 创建新数组A,长度为原数组O长度len
        A = new Array(len);
        // 7. 将k赋值为0
        k = 0;
        // 8. 当 k < len 时,执行循环.
        while(k < len) {
            var kValue, mappedValue;
            //遍历O,k为原数组索引
            if (k in O) {
                //kValue为索引k对应的值.
                kValue = O[ k ];
                // 执行callback,this指向T,参数有三个.分别是kValue:值,k:索引,O:原数组.
                mappedValue = callback.call(T, kValue, k, O);
                // 返回值添加到新书组A中.
                A[ k ] = mappedValue;
            }
            // k自增1
            k++;
        }
        // 9. 返回新数组A
        return A;
    };
}
```

https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Global_Objects/Array/map

5.
IE 8(在iframe中无法正常使用json)以及更早版本对于JSON没有原生支持，可使用Douglas Crockford写的json2.js，但要
考虑如何根据条件加载该文件。若仅需要解析JSON字符串返回JavaScript对象，也可以使用
jQuery的jQuery.parseJSON方法，但jQuery没有stringify方法。

6.
IE下，button元素内如果加超链接，点击该button，不会发生通常的超链接跳转。如：

```html
<button type="button" class="btn btn-info"><a href="/curl/view_log_list?id=2">查看日志</a></button>
```

点击该按钮并不会跳转到 ``/curl/view_log_list?id=2`` 所指向的页面。

可修改为：

```html
<button type="button" class="btn btn-info" onClick="javascript:location.href='/curl/view_log_list?id=2'">查看日志</button>
```

来实现。

### 最佳实践

1.
外部CSS文件在`<head>`中引入，外部JS文件在`<body>`的最后位置引入。

### 推荐阅读

- [浏览器的渲染原理简介](http://coolshell.cn/articles/9666.html)
- [Introduction to Layout in Mozilla](https://developer.mozilla.org/en-US/docs/Introduction_to_Layout_in_Mozilla)
- [前端知识体系](http://fe.adbeginner.com/)
- [favicon-cheat-sheet](https://github.com/audreyr/favicon-cheat-sheet)
- [Fontello - icon fonts generator](http://fontello.com/)
- [Pears（学习CSS的好资源）](http://pea.rs/)
- [浏览器的工作原理：新式网络浏览器幕后揭秘](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)
- [WebPlatform.org](http://www.webplatform.org/)(赞！强烈推荐！)
- [关于URL编码](http://www.ruanyifeng.com/blog/2010/02/url_encoding.html)
- [Chrome扩展及应用开发](http://www.ituring.com.cn/book/1421)

- [说说JSON和JSONP，也许你会豁然开朗](http://kb.cnblogs.com/page/139725/)
- [Javascript中几种实用的跨域方法](http://gejiawen.github.io/2014/09/19/methods-for-cross-region/)
- [Cookie-based Authentication in AngularJS](http://blog.ionic.io/angularjs-authentication/)
- [关于跨域的ajax——Cross-Origin Resource Sharing (CORS)](http://mutongwu.iteye.com/blog/1637183)
- PDF和HTML互转神器：[wkhtmltopdf](http://wkhtmltopdf.org/)、[pdf2htmlEX](http://coolwanglu.github.io/pdf2htmlEX/)

