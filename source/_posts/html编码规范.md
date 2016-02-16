---
title: HTML编码规范
date: 2016-02-16 18:25:36
tags: 编码规范
---

*本文内容基于[ecomfe(百度EFE团队)/html](https://github.com/ecomfe/spec/blob/master/html-style-guide.md)规范修改得来*

## 目录

## 1 前言


本文档的目标是使HTML代码风格保持一致，并且提供一套html标签语义化指导意见，期望html页面可以很容易被理解和被维护。




## 2 代码风格


### 2.1 缩进与换行


#### [强制] 使用 `4` 个空格做为一个缩进层级，不允许使用 `2` 个空格 或 `tab` 字符。


示例：

```html
<ul>
    <li>first</li>
    <li>second</li>
</ul>
```

#### [建议] 行内标签不换行，块级元素依块内复杂度灵活判断

示例：

```html
<div>
    <h4>title</h4>
    <p>
        <span><em>foo</em>bar<i>icon<i/></span>
    </p>
</div>
```

解释：

行内元素换行，可能造成位置的空格出现。


#### [建议] 每行不得超过 `120` 个字符。

解释：

过长的代码不容易阅读与维护。但是考虑到 HTML 的特殊性，不做硬性要求。


### 2.2 命名



#### [强制] `class` 必须单词全字母小写，单词间以 `_` 分隔。

#### [强制] `class` 必须代表相应模块或部件的内容或功能，不得以样式信息进行命名。

示例：

```html
<!-- good -->
<div class="sidebar"></div>

<!-- bad -->
<div class="left"></div>
```

#### [强制] 元素 `id` 必须保证页面唯一。

解释：

同一个页面中，不同的元素包含相同的 id，不符合 id 的属性含义。并且使用 document.getElementById 时可能导致难以追查的问题。


#### [建议] `id` 建议单词全字母小写，单词间以 `_` 分隔。同项目必须保持风格一致。


#### [建议] `id`、`class` 命名，在避免冲突并描述清楚的前提下尽可能短。

示例：

```html
<!-- good -->
<div id="nav"></div>
<!-- bad -->
<div id="navigation"></div>

<!-- good -->
<p class="comment"></p>
<!-- bad -->
<p class="com"></p>

<!-- good -->
<span class="author"></span>
<!-- bad -->
<span class="red"></span>
```

#### [强制] 禁止为了 `hook 脚本`，创建无样式信息的 `class`。

解释：

不允许 class 只用于让 JavaScript 选择某些元素，class 应该具有明确的语义和样式。否则容易导致 css class 泛滥。

使用 id、属性选择作为 hook 是更好的方式。



### 2.3 标签


#### [强制] 标签名必须使用小写字母。

示例：

```html
<!-- good -->
<p>Hello StyleGuide!</p>

<!-- bad -->
<P>Hello StyleGuide!</P>
```

#### [强制] 对于无需自闭合的标签，不允许自闭合。

解释：

常见无需自闭合标签有input、br、img、hr等。


示例：

```html
<!-- good -->
<input type="text" name="title">

<!-- bad -->
<input type="text" name="title" />
```

#### [强制] 对 `HTML5` 中规定允许省略的闭合标签，不允许省略闭合标签。

解释：

对代码体积要求非常严苛的场景，可以例外。比如：第三方页面使用的投放系统。


示例：

```html
<!-- good -->
<ul>
    <li>first</li>
    <li>second</li>
</ul>

<!-- bad -->
<ul>
    <li>first
    <li>second
</ul>
```


#### [强制] 标签使用必须符合标签嵌套规则。

解释：

比如 div 不得置于 p 中，tbody 必须置于 table 中。

详细的标签嵌套规则参见[HTML DTD](http://www.cs.tut.fi/~jkorpela/html5.dtd)中的 `Elements` 定义部分。


#### [建议] 在 `CSS` 可以实现相同需求的情况下不得使用表格进行布局。

解释：

在兼容性允许的情况下应尽量保持语义正确性。对网格对齐和拉伸性有严格要求的场景允许例外，如多列复杂表单。


#### [建议] 标签的使用应尽量简洁，减少不必要的标签。

示例：

```html
<!-- good -->
<img class="avatar" src="image.png">

<!-- bad -->
<span class="avatar">
    <img src="image.png">
</span>
```


### 2.4 属性


#### [强制] 属性名必须使用小写字母。

示例：

```html
<!-- good -->
<table cellspacing="0">...</table>

<!-- bad -->
<table cellSpacing="0">...</table>
```


#### [强制] 属性值必须用双引号包围。

解释：

不允许使用单引号，不允许不使用引号。


示例：

```html
<!-- good -->
<script src="esl.js"></script>

<!-- bad -->
<script src='esl.js'></script>
<script src=esl.js></script>
```

#### [强制] 除href和onerror外，不允许直接使用事件属性

示例：

```html
<!-- good -->
<p src="esl.js"></p>
<script>
    $(document).on("p","click",function(e){
        //dosth
    });
</script>

<!-- bad -->
<p onclick="dosth()"></p>
```

#### [建议] 布尔类型的属性，建议不添加属性值。

示例：

```html
<input type="text" disabled>
<input type="checkbox" value="1" checked>
```


#### [建议] 自定义属性建议以 `xxx-` 为前缀，推荐使用 `data-`。

解释：

使用前缀有助于区分自定义属性和标准定义的属性。


示例：

```html
<ol data-ui-type="Select"></ol>
```
#### [建议] 标签中的属性顺序，按此规则排列，id>class>style>checked等自有属性>data-*等自定义属性

示例：
```html
<span id="foo" class="bar" style="foobar" data-test="baz"></span>
```


## 3 通用

### 3.1 编码


#### [建议] `HTML` 文件使用无 `BOM` 的 `UTF-8` 编码。

解释：

UTF-8 编码具有更广泛的适应性。BOM 在使用程序或工具处理文件时可能造成不必要的干扰。



### 3.2 CSS和JavaScript引入


#### [强制] 引入 `CSS` 时必须指明 `rel="stylesheet"`。

示例：

```html
<link rel="stylesheet" href="page.css">
```

#### [建议] 无特殊情况，不允许直接在页面中写script和style

解释：
不允许`<style>`标签出现
`<script>`标签，只允许用来引入js


#### [建议] 引入 `CSS` 和 `JavaScript` 时无须指明 `type` 属性。

解释：

`text/css` 和 `text/javascript` 是 type 的默认值。


#### [建议] 展现定义放置于外部 `CSS` 中，行为定义放置于外部 `JavaScript` 中。

解释：

结构-样式-行为的代码分离，对于提高代码的可阅读性和维护性都有好处。


#### [建议] 在 `head` 中引入页面需要的所有 `CSS` 资源。

解释：

在页面渲染的过程中，新的CSS可能导致元素的样式重新计算和绘制，页面闪烁。


#### [建议] `JavaScript` 应当放在页面末尾，或采用异步加载。

解释：

将 script 放在页面中间将阻断页面的渲染。出于性能方面的考虑，如非必要，请遵守此条建议。


示例：

```html
<body>
    <!-- a lot of elements -->
    <script src="init-behavior.js"></script>
</body>
```

### 3.3 图片


#### [强制] 禁止 `img` 的 `src` 取值为空。延迟加载的图片也要增加默认的 `src`。

解释：

src 取值为空，会导致部分浏览器重新加载一次当前页面，参考：<https://developer.yahoo.com/performance/rules.html#emptysrc>

## 4 国美在线wap前端html语义化标签使用建议

本规范将html标签大致归类为：

- 布局类标签
- 有明确功能用途元素类标签
- 文本类标签
- 特殊标签/万能标签

### 4.1 布局类标签

此类标签一般只具有布局和划分大的功能模块的含义，不代表过于具体的功能

|标签|语义|备注|
|----|----|----|
|nav|导航|除公共导航外，其他具备导航性质的区块都可以使用|
|header|头部|根据产品设计判断特别明显的可以作为头部的区块，使用头部，一般头部只允许有一个|
|footer|底部|已经被公共模板底部占用|
|article|功能模块|代表一个大的功能模块，他是最高级的，不能被其他article和section包含|
|section|子模块|只出现在article中，不能被其他section包含|
|aside|侧边栏|用于初始时不在页面中显示的区块，比如能滑入滑出的侧边栏|
|div|区块|一般用在上述标签中，用于划分布局区块，若是不知道使用什么标签布局时，也可以使用div|
|ul,li|无序列表|大多数使用列表时用ul，li|
|ol,li|有序列表|有明确的1234序列时使用|
|dl,dt,dd|定义列表|列表有明显的标题时使用|
|table系列|表格|一般情况下禁用|
示例：
```html
<header>
    <nav></nav>
    <ul class="list">
        <li></li>
        <li></li>
    </ul>
</header>
<article class="first_module">
    <section class="sub_module"></section>
</article>
<article class="second_module">
    <section class="sub_module"></section>
</article>
<article class="third_module">
    <section class="sub_module"></section>
</article>
<footer>
</footer>
```

**布局类标签使用时，需要基于性能优先的原则，比如当前页只有一个模块，不必使用aritcle，section等顶级布局标签**

示例：
```html
<!--这个页面只是一个显示列表的页面，则直接使用ul布局即可-->
<body>
    <nav>...</nav>
    <ul class="list">
        <li></li>
        <li></li>
    </ul>
</body>
```



### 5.2 功能元素标签

这类标签一般具有更加具体的功能性

|标签|语义|备注|
|----|----|----|
|img|图片||
|label,input|输入框|input的描述必须用label，label只能跟input一起使用|
|textarea|多行输入框||
|button|按钮|一般很明显的按钮，推荐使用button标签，按钮有跳转功能时，推荐用a表示标签|
|select系列|选择框||
|canvas|画布|逐像素渲染区块，一般用不到|

### 5.3 文本类标签

|标签|语义|备注|
|----|----|----|
|h1|明确的标题|禁止使用|
|h2|明确的标题|已经被顶部导航占用|
|h3-h6|明确的标题|按模块层级使用h3-h6|
|p|一个文本段落|一行，一段，都可以使用，但一般只能用于文本，不能出现类似用p表示button的情况|
|sup|上标|用于出现在左上、右上角的小图标、小文本等|
|sub|下标|用于出现在左下、右下角的小图标、小文本等|
|i|字体图标|i标签只能用于字体图标，搭配font_icon类或data-icon属性使用|
|del|带有删除线的文本||
|em|强调|用于染色类的强调|
|strong|更强的强调|用于染色或加粗或放大的强调|
|small|用于一段文本中的缩小文本|
|q|引用||
|blockquete|大段的引用||
|ins|插入的文本||
|abbr|缩写的文本||

### 5.4 特殊标签/万能标签

**span**

span标签一般用于表示行内元素，也可用于表示block，inline-block。
当上述3类标签都不适合使用时，可以使用span标签。具体使用，根据情况灵活运用。

**a**

需要点击跳转的区块，用a标签包裹。

#### [建议]当a标签用于跳转时，将a标签置于触发区域之下，并设置为block，<span style="color:red">不要用a标签包裹触发区域</span>

示例:
```html
<style>
a.react {
    display: block;
    height: 100%;
    color: inherit;
}
</style>
<ul>
    <li>
        <a class="react">
            <h3>title</h3>
            <p>paragraph</p>
        </a>
    </li>
</ul>
<section>
    <a class="react">
        <span>text</span>
    </a>
</section>
```

####[建议]在base_new.css中加入了a.react样式，其内容如下，建议当使用a标签做包裹时，使用此样式代替自定义样式

### 5.5 其他标签

以上标签已能涵盖大部分情况，其他标签，一般情况下不建议使用。

## 6 模板中的 HTML


#### 待补充
