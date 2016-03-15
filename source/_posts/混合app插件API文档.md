---
title: 混合app插件API文档
date: 2016-02-29 13:19:58
tags: 混合app
categories: 混合app
---
## 前言
此文档旨在描述国美在线混合app中，app端和h5端的插件调用接口

## plugins

### gome-app-nativeLogin

| 方法 | 参数 | 回调参数 |
| ------- | ------- | --- |
|navigator.gome.app.nativeLogin.jumpToNativeLogin(success, fail)|success{function}; fail{function}|json字符串如:'{"jumpToNativeLogin":"Y"}'|
|navigator.gome.app.nativeLogin.jumpToNativeLogout(success, fail)|success{function}; fail{function}|json字符串如:'{"jumpToNativeLogout":"Y"}'|
<br>
### gome-util-nativeRequest

| 方法 | 参数 | 回调参数 |
| ------- | ------- | --- |
|navigator.gome.util.nativeRequest.sendNativeRequest(success, fail, args)|success{function}; fail{function}; args{object}|html字符串|
|navigator.gome.util.nativeRequest.sendNativeLayoutRequest(success, fail, args)|success{function}; fail{function}; args{object}|json字符串|
<br>
### gome-util-nativeUtils

| 方法 | 参数 | 回调参数 |
| ------- | ------- | --- |
|navigator.gome.util.nativeUtils.isLogin(success)|success{function}|json字符串如:'{"isLogin":"Y"}'|
|navigator.gome.util.nativeUtils.showToast(args)|args{object}|--|
|navigator.gome.util.nativeUtils.jumpExternalLink(args)|args{object}|--|
|navigator.gome.util.nativeUtils.showTitle(args)|args{object}|--|
|navigator.gome.util.nativeUtils.getMeasure(args)|args{object}|--|
|navigator.gome.util.nativeUtils.getAppEnvironment(success)|success{function}|json字符串如:'{"environment":"uat"}'|
|navigator.gome.util.nativeUtils.getAppVersion(success)|success{function}|json字符串如:'{"dev_version": 55,"user_version":"4.1.2"}'|
