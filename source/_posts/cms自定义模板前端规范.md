---
title: cms自定义模板规范
date: 2016-02-29 12:14:50
categories: cms
tags: cms
toc: true
author: 赵晔
---
# cms自定义模板规范
<!--### Author: [赵晔](https://github.com/JALBAA)-->

## 前言
自定义模板，由于其灵活度过高，在此对其作出一定的限制。

**核心思想** 是将页面中业务逻辑方法分离，防止cms的自定义模板同cms静态页系统耦合过高，导致无法适配其他宿主环境，即 **自定义模板数据化** 。



## 显隐控制
### 解释

#### [强制] 在公共头模板，通过调用appView方法，UA判断宿主环境，根据不同宿主环境，设置css样式并插入dom

#### [强制] 原则上不再增加新的对应关系

### 对应关系

| 宿主环境                | 对应css类     |
| :-------------         | :------------- |
| 普通浏览器              | wapshow       |
| 国美在线app客户端       | appshow       |
| app版本控制             | gte[版本号]\(*解释：小于某版本则显示*)   |

### 代码示例
``` html
<div class="wapshow">
普通浏览器显示的部分
</div>
<div class="appshow">
国美在线app客户端显示的部分
默认显示
或有版本控制时，用来匹配最新版本
</div>
<div class="appshow lte30">
国美在线app客户端显示的部分
版本小于30大于20
</div>
<div class="appshow lte20">
国美在线app客户端显示的部分
版本小于20大于10
</div>
<div class="appshow lte10">
国美在线app客户端显示的部分
版本小于10大于1
</div>
```

详情参照[cms前端代码规范#自定义模板](/2016/02/29/cms前端代码规范/#自定义模板)


## cms数据挂载规则

详情参照[cms前端代码规范#数据挂载规则](/2016/02/29/cms前端代码规范/#数据挂载规则)

## 跳转规则

### 解释

#### [强制]新版本须使用scheme方案

#### [强制]旧版本，需使用data-hook代替方法调用，对页面和宿主环境进行分离

### 代码示例

标签中挂载数据，标识跳转模块和对应参数
``` html
<div class="appshow">
    <!--商品详情-->
    <a data-cms-jump="goods" data-cms-params="{'type':0,'params':'1122450299&No=9133681941&skipType=2'}" href="javascript:;">...</a>
    <!--金融-->
    <a data-cms-jump="finance" data-cms-params="{type:'meiyingbaohome'}" href="javascript:;">...</a>
</div>
```

基于不同的宿主环境，对相同的数据结构，可以进行不同的处理，
比如

``` javascript
//cms静态页
//xxx.js
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
```

混合app会预解析模板,拼接成scheme

## 领券

### 解释

#### [强制]需使用data-hook代替方法调用，对页面和宿主环境进行分离

### 代码示例
标签中挂载数据，标识模块名称和对应参数
``` html
<div data-cms-coupon data-cms-params="{'key':'afal3d5f52jwo7ef9ij1woe99if5jweofijwoefwmfeowfmw'}">...</div>
```
cms静态页脚本
``` javascript
$(function(){
    $('[data-cms-coupon]').click(function(){
        //领券
    })
})
```
混合app已经做另行处理
