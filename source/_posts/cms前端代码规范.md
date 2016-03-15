---
title: cms前端代码规范
date: 2016-02-29 21:45:06
categories: cms
tags: cms
author: 赵晔
toc: true
---
## 前言

本文档旨在协助cms后端人员良好使用无线前端组所提供的开发资源。

## 资源引用规则

### 通用

#### [强制]css文件须在html的head标签中引用
示例:

``` html
<html>
<head>
    <link rel="stylesheets" href="foo.css">
</head>
</html>
```
#### [建议]少量补充样式，直接以`<style>`标签形式插入`<head>`标签中

示例:
``` css
/*foo.css*/
/*只有两句话*/
.appshow {
    display: none;
}
.wapshow {
    display: none;
}
```

``` html
<!--bad-->
<html>
<head>
    <link rel="stylesheets" href="foo.css">
</head>
</html>
<!--good-->
<html>
<head>
    <style>
        /*只有两句话，直接嵌入head*/
        .appshow {
            display: none;
        }
        .wapshow {
            display: none;
        }
    </style>
</head>
</html>
```
#### [强制]js文件全部置于所有`DOM`之后`</body>`标签之前引用

示例:
``` html
<!--bad-->
<html>
    <head>
        <script src="foo.js"></script>
        <script src="bar.js"></script>
    </head>
    <body>
        <div></div>
    </body>
</html>
<!--good-->
<html>
    <head>
    </head>
    <body>
        <div></div>
        <script src="foo.js"></script>
        <script src="bar.js"></script>
    </body>
</html>
```
### 类库引用

#### [强制]css引用base.css及ui组建类库gomeui.css(尚未提交)
#### [强制]js引用gomeui.js(尚未提交)

## 内嵌脚本

### rem
#### [强制]公共头`<head>`中嵌入平滑rem脚本

代码如下
``` javascript
var width = document.querySelector('html').offsetWidth;
var delta =  (640-320) / (20-10);
var fontSize = (width - 320) / delta + 10;
if(width <= 320){
    fontSize = 10;
}else if(width >=640){
    fontSize = 20;
}
document.querySelector('html').style.fontSize = fontSize+'px';
window.addEventListener('resize',function(){
    var width = document.querySelector('html').offsetWidth;
    var delta =  (640-320) / (20-10);
    var fontSize = (width - 320) / delta + 10;
    if(width <= 320){
        fontSize = 10;
    }else if(width >=640){
        fontSize = 20;
    }
    document.querySelector('html').style.fontSize = fontSize+'px';
})
```

### 自定义模板

#### [强制]在自定义模板的末尾嵌入自定义模板加载完成事件

解释

* 特殊情况下，需要对自定义模板进行js操作的，通过侦听**CMSCustomLoaded**事件完成
* 基于观察者模式的消息机制，可以有效的避免直接调用方法造成的耦合
* 在页面中，直接发送消息，可以在自定义模板生成后立即进行操作显隐，避免页面闪烁现象

代码

``` html
<div class="cms_custom">
    <div class="wapshow"></div>
    <div class="appshow"></div>
    <!--发送cms自定义模板加载完成事件-->
    <script>
    (function(){
        var e = document.createEvent('HTMLEvents')
        e.initEvent('CMSCustomLoaded',true,false)
        window.dispatchEvent(e)
    })()
    </script>
<div>
```

#### [强制]公共头`<head>`中嵌入`style`标签控制自定义模板的默认显隐

代码
``` html
<!--默认都是隐藏的-->
<style rel='stylesheets'>
    .wapshow,
    .appshow {
        display: none;
    }
</style>
```
#### [强制]公共头`<head>`中嵌入`<script>`标签控制自定义模板的默认显隐

**依赖appView**
``` html
<script>
(function(v){
    //生成标签
    var style = document.createElement('style')
    style.setAttribute('rel','stylesheets')
    //根据宿主环境确定显示对象
    if(appView.wap){
        style.innerHTML = '.wapshow{display:block;}'
    }else if (appView.ios || appView.android) {
        //appversion=0的话就按默认处理
        if(v == 0){
            style.innerHTML = '.appshow{display:block;}'
            return
        }
        document.addEventListener('CMSCustomLoaded',function(){
            if(document.querySelectorAll('.appshow[class*=lte]').length == 0){
                //默认
                style.innerHTML = '.appshow{display:block;}'
            }else{
                //需要判断版本
                var list = document.querySelectorAll('.appshow[class*=lte]')
                var versions = [0]
                for(var i=0; i<list.length; i++){
                    if(list[i].className.match(/lte(\d+)/))
                        versions.push(Number(list[i].className.match(/lte(\d+)/)[1]))
                }
                versions.sort()
                if(versions[versions.length-1] < v){
                    var l = document.querySelectorAll('.appshow:not([class*=lte])')
                    for(var i=0; i<l.length; i++){
                        l[i].style.display = 'block'
                    }
                    return;
                }
                for(var j=0; j<versions.length-1; j++){
                    if(versions[j] < v && versions[j+1] >= v){
                        var l = document.querySelectorAll('.appshow.lte'+versions[j+1])
                        for(var i=0; i<l.length; i++){
                            l[i].style.display = 'block'
                        }
                        return;
                    }
                }
            }
        })
    //}
    //插入dom使之生效
    document.head.appendChild(style)
})(appView.version)
</script>
```



## 数据挂载规则


#### [强制]使用data-* 作为js hook进行数据处理，禁止使用无意义的css类

#### [强制]使用<span style="color:red;">data-cms-[模块类型]='模块id'</span>表示cms模块

#### [强制]使用<span style="color:red;">data-cms-params=[json格式字符串]</span>表示模块数据

#### [建议]在DOM树的上层定义模块名，靠近叶子节点的dom，直接挂在数据即可，如商品列表中的商品，可直接挂载data-sku-id="xxx"

示例

``` html
<div class="appshow">
    <!--商品详情-->
    <a data-cms-jump="goods" data-cms-params="{'type':0,'params':'1122450299&No=9133681941&skipType=2'}" href="javascript:;">...</a>
    <!--金融-->
    <a data-cms-jump="finance" data-cms-params="{type:'meiyingbaohome'}" href="javascript:;">...</a>
</div>
<script>
//基于不同的宿主环境，对相同的数据结构，可以进行不同的处理，
$(function(){
    //商品的跳转
    $('[data-cms-jump="goods"]').click(function(){
        var data = JSON.parse($(this).data('cms-href'))
        //操作bridge跳转
    })
    //金融产品的跳转
    $('[data-cms-jump="finance"]').click(function(){
        var data = JSON.parse($(this).data('cms-href'))
        //操作bridge跳转
    })
})
</script>
```
