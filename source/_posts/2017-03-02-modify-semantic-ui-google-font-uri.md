---
title: 修改Semantic-UI中的google字体源
date: 2017-03-02 11:04:18
categories:
tags:
- semantic-ui
- nginx
- nginx_substitutions_filter
---

## 原因

在配置Semantic-UI的过程中，如果使用国内某些开放的CDN源的话（例如cdn.bootcss.com)，会发现虽然css和js文件享受到了国内的CDN服务，但是在css文件中通过import引入的文件还是原来的地址
```css
 /*
 * # Semantic UI - 2.2.9
 * https://github.com/Semantic-Org/Semantic-UI
 * http://www.semantic-ui.com/
 *
 * Copyright 2014 Contributors
 * Released under the MIT license
 * http://opensource.org/licenses/MIT
 *
 */
@import url(https://fonts.googleapis.com/css?family=Lato:400,700,400italic,700italic&subset=latin);/*!
 * # Semantic UI 2.2.9 - Reset
 * http://github.com/semantic-org/semantic-ui/
 *
 *
 * Released under the MIT license
 * http://opensource.org/licenses/MIT
 *
 */
```
如上所示，返回的``semantic.min.css``中使用的还是google的源，怎么办呢？

经过一番查阅，找到几种可行的方法，主要思路是替换google源为国内的源。
> 国内的中科大提供了Google Fonts加速服务（https://lug.ustc.edu.cn/wiki/lug/services/googlefonts）

## 解决方法

### 方法一

1. 先下载目标的``semantic.min.css``文件
2. 手动修改css文件中的url，将``fonts.googleapis.com``替换成``fonts.lug.ustc.edu.cn``，将``./themes/default/assets``替换成``//cdn.bootcss.com/semantic-ui/2.2.9/themes/default/assets``
3. 将``semantic.min.css``传到自己服务器上或者其他CDN服务器上使用

### 方法二

1. 先下载目标的semantic.min.css文件
2. 使用nginx的nginx_substitutions_filter模块替换css文件中的url，如下
```text
subs_filter_types text/css;
subs_filter fonts.googleapis.com fonts.proxy.ustclug.org;
subs_filter themes/default/assets //cdn.bootcss.com/semantic-ui/2.2.9/themes/default/assets;
```
3. 将``semantic.min.css``传到自己服务器上使用
