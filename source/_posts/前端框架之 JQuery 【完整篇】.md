---
title: 吃透 JQuery 之【完整篇】
categories: 
- 前端
- JQuery 
tags: 
- 前端
- JQuery 
---



一篇 JQuery 核心内容文章！

<!--more-->

[TOC]



## 一、选择器

### 1、基本选择器

#### 1.1 按 ID 查找

> 返回 jQuery 对象，jQuery 对象类似数组，每个元素都是引用了 DOM 节点的对象。`[<div id="abc">...</div>]`，如果找不到该对象，返回 `[] ` 空对象。

```javascript
// 按照 ID 查找
<div id="abc"></div>
var div = $('#abc');
```

> jQuery 对象与 DOM 对象之间的转化：

```javascript
var div = $('#abc');    // 获取 jQuery 对象 ‘#abc’ 执行了 document.getElementById('#abc');
var divDOM = div.get(0);// 获取第一个 DOM 对象
var anthor = $(divDOM); // 重新把 DOM 封装为 jQuery 对象
```



#### 1.2 按 tag 查找

```javascript
var ps = $('p');//获取所有的 <p> 节点
ps.length;      //统计 <p> 节点的个数
```



#### 1.3 按 class 查找

> 查找之前要加一个 '.'；

```javascript
var ps = $('.red');// 所有节点包含`class="red"`都将返回
```



#### 1.4 按属性查找

> DOM 节点很多属性，可以根据属性来快速定位节点。

```javascript
// 找出属性为 [name=email] 的节点
var email = $('[name^=email]');

// 按属性查找使用前缀或后缀查找
var icons = $('name^= icon')       // 找出所有 name 属性值以 icon 开头的 DOM
var names = $('[name$=with]');     // 找出所有 name 属性值以 with 结尾的 DOM
var icons = $('[class^="icon-"]'); // class 经常使用到
```



#### 1.5 组合查找

> 通常选择会选择所有的属性，但是有时我们只选择某标签里的属性进行使用。

```javascript
// 标签和属性组合
var emilInput = $('input[name = email]');
// 标签和 Class 组合
var tr = $('tr.red');
```



#### 1.6 多项选择器

> 多个选择器用 `，` 组合起来一块选。选出来的元素是按照它们在 HTML 中出现的顺序排列的，而且不会有重复元素。 

```javascript
// 把 <p> 和 <div> 都选出来
$('p,div')
// 把 <p class="red"> 和 <p class="green"> 都选出来
$('p.red,p.green')
```



### 2、层级选择器

> 由于 DOM 的结构是层级结构，经常需要根据层级来进行选择，所以 jQuery 层级选择器更加灵活。
>
> **优点： **层级选择器的好处就是在于缩小了选择的范围，定位父元素后再定位子元素，避免了不相干页面的干扰。



#### 2.1 层级选择器

> 如果两个 DOM 元素有层级结构，可以使用 `$('ancestor descendant') ` 来选择，层级之间需要用空格隔开。

```javascript
$('form[name=upload] input');
```



#### 2.2 子选择器（Child Selector）

> 类似层级选择器，但是限定了只能选择父元素的子元素。

```javascript
$('ul > li') //选择 <ul> 父节点下的子节点 <li>
```



#### 2.3 过滤器（Filter）

> 过滤器通常不单独使用，附加到选择器中使用，更精确的定位元素。

```javascript
$('ul.lang li'); // 选出 JavaScript、Python 和 Lua 3个节点

// 选择节点的第一个节点
$('ul.lang li:first-child');     // 仅选出 JavaScript
// 选择最后一个元素
$('ul.lang li:last-child');      // 仅选出 Lua
// 选出第 n 的元素，n 从 1 开始
$('ul.lang li:nth-child(2)');  
$('ul.lang li:nth-child(even)'); // 选出序号为偶数的元素
$('ul.lang li:nth-child(odd)');  // 选出序号为奇数的元素
```



#### 2.4 表单相关

> 针对表单，jQuery 有一组特殊的选择器。

- `:input`：可以选择 `<input>`，`<textarea>`，`<select> ` 和 `<button>`；
- `:file`：可以选择 `<input type="file"> ` 和 `input[type=file]` 一样；
- `:checkbox`：可以选择复选框，和 `input[type=checkbox]` 一样；
- `:radio`：可以选择单选框和 `input[type=radio]` 一样；
- `:focus`：可以选择当前输入焦点的元素，例如把光标放到一个 `<input>` 上，用 `$('input:focus')` 就可以选出；
- `:checked`：选择当前勾上的单选框和复选框，用这个选择器可以立刻获得用户选择的项目，如`$('input[type=radio]:checked')`；
- `:enabled`：可以选择可以正常输入的 `<input>`、`<select>`  等，也就是没有灰掉的输入；
- `:disabled`：和 `:enabled` 正好相反，选择那些不能输入的。

```javascript
$('div:visible'); // 所有可见的div
$('div:hidden'); // 所有隐藏的div
```



### 3、查找和过滤

> 最常用的是在某个节点的所有子节点中查找，使用 find 方法，接受一个选择器进行选择。

```html
<ul class="lang">
    <li class="js dy">JavaScript</li>
    <li class="dy">Python</li>
    <li id="swift">Swift</li>
    <li class="dy">Scheme</li>
    <li name="haskell">Haskell</li>
</ul>
```

```javascript
var ul = $('ul.lang');               // 获得 <ul>
var dy = ul.find('.dy');             // 获得 JavaScript, Python, Scheme
var swf = ul.find('#swift');         // 获得 Swift
var hsk = ul.find('[name=haskell]'); // 获得 Haskell
```

向上查找使用 `parent()` 方法：

```javascript
var swf = $('#swift'); // 获得Swift
var parent = swf.parent(); // 获得 Swift 的上层节点 <ul>
var a = swf.parent('.red'); // 获得 Swift 的上层节点 <ul>，同时传入过滤条件。如果ul不符合条件，返回空 jQuery 对象
```

同级元素使用 `next（）` 和 `prev（） ` 方法：

```javascript
var swift = $('#swift');

swift.next(); // Scheme
swift.next('[name=haskell]'); // 空的jQuery对象，因为Swift的下一个元素Scheme不符合条件[name=haskell]

swift.prev(); // Python
swift.prev('.dy'); // Python，因为 Python 同时符合过滤器条件.dy
```



#### 3.1 过滤

> 函数式编程中的 map、filter类似，jQuery 也有自己类似的方法。

filter() 方法：

```javascript
var langs = $('ul.lang li');// 拿到 JavaScript, Python, Swift, Scheme 和 Haskell
var a = langs.filter('.dy') // 拿到 JavaScript, Python, Scheme 
```

```javascript
//传入一个函数
var langs = $('ul.lang li'); // 拿到 JavaScript, Python, Swift, Scheme 和 Haskell
langs.filter(
    function () {
        // 函数内部的 this 被绑定为 DOM 对象，不是 jQuery 对象
    	return this.innerHTML.indexOf('S') === 0; // 检查每个子节点，返回 S 开头的节点
	}
); // 拿到Swift, Scheme
```

map() 方法：把一个  jQuery 对象包含的若干 DOM 节点转化为其他对象 。

```javascript
var langs = $('ul.lang li'); // 拿到 JavaScript, Python, Swift, Scheme 和 Haskell
var arr = langs.map(function () {
    return this.innerHTML;
}).get(); 
// 用 get() 拿到包含 string 的 Array：['JavaScript', 'Python', 'Swift', 'Scheme', 'Haskell']
```

一个jQuery对象如果包含了不止一个DOM节点，`first()`、`last()`和`slice()`方法可以返回一个新的 jQuery对象，把不需要的 DOM 节点去掉： 

```javascript
var langs = $('ul.lang li');  // 拿到 JavaScript, Python, Swift, Scheme 和 Haskell
var js = langs.first();       // JavaScript，相当于 $('ul.lang li:first-child')
var haskell = langs.last();   // Haskell, 相当于 $('ul.lang li:last-child')
var sub = langs.slice(2, 4);  // Swift, Scheme 参数和数组的 slice() 方法一致
```

```
var inputs = $('#test-form :input').not('button');
var obj = {};
inputs.filter(function(){
 if(this.type !== "radio" || this.checked);
 obj[this.name] = this.value;
})
json = JSON.stringify(obj);
```



## 二、操作 DOM

### 1. 操作 DOM

#### 1.1 修改 Text 和 HTML

> jQuery 对象的 text() 和 html() 方法分别获取节点文本和原始的 HTML 文本。
>
> 1）jQuery 可以获取一组数据进行统一设置文本。
>
> 2）jQuery 如果不存在结点对象，将不会报错。

```html
<ul id="test-ul">
    <li class="js">JavaScript</li>
    <li name="book">Java &amp; JavaScript</li>
</ul>
```

```javascript
// 获取文本
$('#test-ul li[name=book]').text(); // 'Java & JavaScript'
$('#test-ul li[name=book]').html(); // 'Java &amp; JavaScript'

// 设置文本
$('#test-ul li[name=book]').text('JavaScript & ECMAScript'); 
$('#test-ul li[name=book]').html('<span style="color: red">JavaScript</span>');
```



#### 1.2 修改 CSS

> 调用 jQuery 对象的 `css("name","value")` 方法。
>
> 1）CSS 属性可以使用 `'background-color' ` 和  `'backgroundColor'`两种格式。
>
> 2）css() 方法将作用于 DOM 节点的 style 属性，具有最高的优先级。

```javascript
// 注意：jQuery 的所有方法返回的是对象，可以链式调用。
$('#test-css li.dy>span').css('background-color', '#ffd351').css('color', 'red');
```

> 修改 `class` 属性，jQuery 提供一下方法：

```javascript
var div = $('#test');
div.hasClass('container');// 判断该结点 class 是否包含 container 属性
div.addClass('container');// 添加 container 这个 Class
div.removeClass('container'); // 删除 container 这个 Class
```



#### 1.3 显示和隐藏 DOM

> 显示或隐藏 DOM 需要设置 CSS 属性的 display 属性。
>
> 1）隐藏 DOM 需要设置 CSS 的 display 属性为 none。
>
> 2）显示 DOM 需要知道 display 之前的属性（block,inline）。
>
> 3）jQuery 对象提供的 show() 和 hide() 方法。

```javascript
var a = $('div');
a.hide(); // 隐藏
a.show(); // 显示(并没有删除 DOM 结点，影响了 DOM 结点的显示)
```



#### 1.4 获取 DOM 信息

> 无序针对特定的浏览器编写特定的代码，jQuery 对象的方法直接获取。
>
> 1）操作 DOM 节点的属性：`attr()` 和 `removeAttr()` 方法。
>
> 2）操作 H5 中无值属性：`prop()` 但会 `boolean` 值，也可以用 `is` 判断。  

```javascript
// 浏览器可视窗口大小:
$(window).width(); // 800
$(window).height(); // 600

// HTML文档大小:
$(document).width(); // 800
$(document).height(); // 3500

// 某个div的大小:
var div = $('#test-div');
div.width(); // 600
div.height(); // 300
div.width(400); // 设置CSS属性 width: 400px，是否生效要看CSS是否有效
div.height('200px'); // 设置CSS属性 height: 200px，是否生效要看CSS是否有效
```

```javascript
var div = $('#test-div');
div.attr('data');// undefined 属性值不存在
div.attr('name');// test 获取属性值
div.attr('name','Hello'); // 设置属性值
div.removeAttr('name'); //删除 name 属性
div.attr('name');  // undefined 
```

```javascript
<input id="test-radio" type="radio" name="test" checked value="1">

var radio = $('#test-radio');
radio.prop('checked'); // true
radio.is(':checked'); // true
is(':selected');// 下拉属性判断
```



#### 1.5 操作表单

> 对于 jQuery 操作表单，统一使用 `val()` 方法获取和设置对应的 `value` 属性：

```javascript
/*
    <input id="test-input" name="email" value="">
    <select id="test-select" name="city">
        <option value="BJ" selected>Beijing</option>
        <option value="SH">Shanghai</option>
        <option value="SZ">Shenzhen</option>
    </select>
    <textarea id="test-textarea">Hello</textarea>
*/
var
    input = $('#test-input'),
    select = $('#test-select'),
    textarea = $('#test-textarea');

input.val(); // 'test'
input.val('abc@example.com'); // 文本框的内容已变为abc@example.com

select.val(); // 'BJ'
select.val('SH'); // 选择框已变为Shanghai

textarea.val(); // 'Hello'
textarea.val('Hi'); // 文本区域已更新为'Hi'
```



### 2、修改 DOM 

> 原生的浏览器 API 修改 DOM 需要根据不同的浏览器进行写不同的代码。



#### 2.1 添加 DOM

> 添加 DOM 结点除了使用 `html()` 方法外还可以使用 `append()` 方法。
>
> 1) append() 方法可以接收一下几个参数：
>
> - DOM 对象
> - jQuery 对象
> - 函数对象(该函数要返回一个字符串、DOM 对象、jQuery对象)
>
> 2）`append()` 把DOM添加到最后，`prepend()` 则把 DOM 添加到最前。 
>
> 3）`after()` 方法和 `before()` 方法将 DOM 插入指定位置。

```javascript
/*<div id="test-div">
    <ul>
        <li><span>JavaScript</span></li>
        <li><span>Python</span></li>
        <li><span>Swift</span></li>
    </ul>
</div>*/
var ul = $('#test-div>ul');
ul.append('<li><span>Haskell</span></li>');
```

```javascript
// 创建DOM对象:
var ps = document.createElement('li');
ps.innerHTML = '<span>Pascal</span>';
// 添加DOM对象:
ul.append(ps);

// 添加jQuery对象:
ul.append($('#scheme'));

// 添加函数对象:
ul.append(function (index, html) {
    return '<li><span>Language - ' + index + '</span></li>';
});
```

> 注意：jQuery的 `append()` 可能作用于一组 DOM 节点，只有传入函数才能针对每个 DOM 生成不同的子节点。 
>

```javascript
var js = $('#test-div>ul>li:first-child');
js.after('<li><span>Lua</span></li>');
```



#### 2.2 删除节点

> 拿到 jQuery 对象之后，直接执行 `remove()` 方法可以删除一组或单个节点。



### 3、事件

> 1) 浏览器获取到鼠标事件，自动在对应的 DOM 结点上触发响应的时间，如果该结点绑定了函数，函数就触发响应的事件，然后调用绑定了对应的 javascript 处理函数，该函数就会自动调用。
>
> 2) 不同浏览器的代码是不一样的，jQuery 屏蔽了不同浏览器的差异。

```javascript
a.on('click', function () {
    alert('Hello!');
});
```



#### 3.1 鼠标事件

- click : 单击事件
- dbclick: 双击事件
- mouseenter: 鼠标进入时触发
- mouseleace: 鼠标离开时触发
- mousemove: 鼠标在 DOM 内部移动时触发
- hover: 鼠标进入和退出时触发两个函数，相当于 mouseenter 加上 mouseleave。



#### 3.2 键盘事件

> 键盘事件仅作用在当前焦点 DOM 上，通常是 `<imput>` 和 `<textarea>`。

- keydown：键盘按下时触发
- keyup：键盘松开时触发
- keypress：按一次键后触发



#### 3.3 其他事件

- focus：当 DOM 获取焦点时触发；
- blur：当 DOM 失去焦点时触发；
- change：当 `<input>`、`<select>` 或 `<textarea>` 的内容改变时触发；
- submit：当 `<form>` 提交时触发；
- ready：当页面被载入并且 DOM 树完成初始化后触发（仅作用于 document 对象）。

```javascript

```



#### 3.4 事件参数

> 有些事件需要传入参数，获取到按键的值和鼠标的位置。所有事件都传入 Event 对象作为参数。

```javascript
$(function () {
    $('#testMouseMoveDiv').mousemove(function (e) {
        $('#testMouseMoveSpan').text('pageX = ' + e.pageX + ', pageY = ' + e.pageY);
    });
});
```



#### 3.5  取消绑定

> 一个绑定的事件可以被解除，通过 `off（‘click’, function）` 实现：
>
> 1) off 无参会一次性移除已绑定的所有类型的事件处理函数。

```javascript
function hello() {
    alert('hello!');
}

a.click(hello); // 绑定事件

// 10秒钟后解除绑定:
setTimeout(function () {
    a.off('click', hello);
}, 10000);
```



#### 3.6 事件触发条件

> 事件的触发是由用户操作引发的。
>
> 1）change 事件的触发是由用户改变文本框内容触发的，在 js 修改文本框内容则不会触发。
>
> 2）用代码触发 change 事件，可以直接调用无参数的 change() 方法来触发事件。

```javascript
var input = $('#test-input');
input.change(function () {
    console.log('changed...');
});
```

```javascript
var input = $('#test-input');
input.val('change it!');
input.change(); // 触发change事件,相当于 input.trigger('change')
```



#### 3.7 浏览器安全限制

> 有些 javascript 代码只有用户触发才能执行，如 `window.open()` 函数。

```javascript
// 无法弹出新窗口，将被浏览器屏蔽:
$(function () {
    window.open('/');
});
```



### 4、AJAX

> jQuery 的 AJAX 不用针对不同浏览器写不同代码，代码也得到很大的简化。



#### 4.1 ajax

> jQuery 在全局对象中（$）绑定了 ajax() 函数。
>
> 1）`ajax(url,settings)` 函数需要接收一个 URL 和一个可选的 settings 对象

- **async **：默认 true

- **method： **发送的 Method ，默认为 `GET`。

- **contentType： **发送的 POST 格式，默认值为 `'application/x-www-form-urlencoded; charset=UTF-8' `

  其余格式为 `text/plain `、`application/json `；

- **data： **发送的数据，可以是字符串、数组或 object。如果是 GET 请求，data 转换为 query 附加到 URL 上。如果是 POST 根据 contentType 将数据序列化成合适的格式；

- **headers： **发送额外的 HTTP 头，必须是一个 object;

- **dataType：**接受的数据格，`html` 、`xml`、`json` 、`text` 等，没有设置的情况下根据 contentType 来定义。

```javascript
var jqxhr = $.ajax('/api/categories',{
    dataType: 'json'
})
```

> 响应方式：

```javascript
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
}).done(function (data) {
    ajaxLog('成功, 收到的数据: ' + JSON.stringify(data));
}).fail(function (xhr, status) {
    ajaxLog('失败: ' + xhr.status + ', 原因: ' + status);
}).always(function () {
    ajaxLog('请求完成: 无论成功或失败都会调用');
});
```



#### 4.2 get 方法

> 最常见的写法；第二个参数会被拼接到 url 后边

```javascript
var jqxhr = $.get(url, {
    name: 'Bob Lee',
    check: 1
});
```

```javascript
url?name=Bob%20Lee&check=1
```



#### 4.3 post 方法

> 虽然写法和 GET 类似，但是第二个参数被序列化了为 `application/x-www-form-urlencoded `。

```javascript
var jqxhr = $.post('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});
```



#### 4.4 getJSON 对象

> 通过 `getJSON()` 方法来快速通过 GET 获取一个 JSON 对象：

```javascript
var jqxhr = $.getJSON('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
}).done(function (data) {
    // data已经被解析为JSON对象了
});
```



#### 4.5 安全限制

> 关于跨域，jQuery 也是有限制的，和 JavaScript 一样。如果跨域加载数据，设置 `jsonp：‘callback’` ，这样就可以实现 jQuery 加载跨域数据了。



### 5、动画

> jQuery 封装的动画非常简单，只需要一行代码就可以搞定。



#### 5.1 show/hide

> 无参的`show()` `hide()` 方法用于隐藏、显示元素，传入一个时间参数就会变成动画。
>
> 1）参数可以是`'slow'`，`'fast'` 。
>
> 2）`toggle() ` 根据当前的状态来决定显示还是隐藏。

```javascript
var div = $('#id');
div.hide(3000); // 在 3 秒钟内逐渐消失
```



#### 5.2 slideUp / slideDown

> 这两个方法是垂直方向消失和隐藏的。
>
> 1) `slideToggle() ` 根据状态来决定。

```javascript
var div = $('#id');
div.slideUp(3000); // 在3秒钟内逐渐向上消失
```



#### 5.3 fadeIn/fadeOut

> 这两个方法的动画就是淡入淡出，通过设置 `opacity` 来实现的。
>
> 1）`fadeToggle()` 决定是否状态是否改变。

```javascript
var div = $('#id');
div.fadeOut('slow'); // 在 0.6 秒内淡出
```



#### 5.4 自定义动画

> `animate()` 可以实现自定义动画，传入的参数就是 DOM 元素最终的 CSS 状态和时间。

```javascript
var div = $('#id');
div.animate({
    opacity: 0.25,
    width: '256px',
    height: '256px'
}, 3000); // 在 3 秒钟内 CSS 过渡到设定值
```

> 第三个参数就是传入一个函数。

```javascript
var div = $('#test-animate');
div.animate({
    opacity: 0.25,
    width: '256px',
    height: '256px'
}, 3000, function () {
    console.log('动画已结束');
    // 恢复至初始状态:
    $(this).css('opacity', '1.0').css('width', '128px').css('height', '128px');
});
```



#### 5.5 串行动画

> 动画可以串行执行，通过 `delay（）`  方法可以实现暂停，可以实现更复杂的动画效果。

```javascript
var div = $('#id');
// 动画效果：slideDown - 暂停 - 放大 - 暂停 - 缩小
div.slideDown(2000)
   .delay(1000)
   .animate({
       width: '256px',
       height: '256px'
   }, 2000)
   .delay(1000)
   .animate({
       width: '128px',
       height: '128px'
   }, 2000);
}
```



#### 5.6 动画设置失败

> 1）有的动画没有效果，jQuery 动画的原理是逐渐改变 CSS 的值。很多元素不是 block 性质的 DOM 元素，对它们设置有的属性不起作用，所以没有动画效果。
>
> 2）jQuery也没有实现对`background-color`的动画效果，用`animate()`设置`background-color`也没有效果 。只能借助 CSS3 动画。

































































