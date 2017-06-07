---
title: 常用meta整理
date: 2016-06-03 15:08:06
tags: html
---


> 标签提供关于HTML文档的元数据。元数据不会显示在页面上，但是对于机器是可读的。它可用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他
> web 服务。 —— W3School 

 - **申明编码**
```
<meta charset='utf-8' />
```

- **浏览器内核控制**
```
<meta name="renderer" content="webkit|ie-comp|ie-stand">
<meta name="renderer" content="webkit">
```

- **设置缓存** 

	1. 值为private或must-revalidate则只有第一次访问时会访问服务器，以后就不再访问
	2. 值为no-cache，那么每次都会访问
	3. 如果指定了max-age值，那么在此值内的时间里就不会重新访问服务器，例如： Cache-control: max-age=5(表示当访问此网页后的5秒内再次访问不会去服务器)
 
 ```
 <meta http-equiv="cache-control" content="no-cache">
 ```


- **优先使用 IE 最新版本和 Chrome**
```
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<!-- 关于X-UA-Compatible -->
<meta http-equiv="X-UA-Compatible" content="IE=6" ><!-- 使用IE6 -->
<meta http-equiv="X-UA-Compatible" content="IE=7" ><!-- 使用IE7 -->
<meta http-equiv="X-UA-Compatible" content="IE=8" ><!-- 使用IE8 -->
```

- **转码申明**，用百度打开网页可能会对其进行转码（比如贴广告），避免转码可添加如下meta
```
<meta http-equiv="Cache-Control" content="no-siteapp" />
```

- **页面关键词**，每个网页应具有描述该网页内容的一组唯一的关键字。
使用人们可能会搜索，并准确描述网页上所提供信息的描述性和代表性关键字及短语。标记内容太短，则搜索引擎可能不会认为这些内容相关。另外标记不应超过 874 个字符。
```
<meta name="keywords" content="xxx,xxx,xxx,xxx" />
```

- **页面描述**，每个网页都应有一个不超过 150 个字符且能准确反映网页内容的描述标签。
```
<meta name="description" content="150 words" />
```

- **搜索引擎索引方式**
```
<meta name="robots" content="index,follow" />
<!--
    all：文件将被检索，且页面上的链接可以被查询；
    none：文件将不被检索，且页面上的链接不可以被查询；
    index：文件将被检索；
    follow：页面上的链接可以被查询；
    noindex：文件将不被检索；
    nofollow：页面上的链接不可以被查询。
 -->
```

- **页面重定向和刷新**，content内的数字代表时间（秒），既多少时间后刷新。如果加url,则会重定向到指定网页（搜索引擎能够自动检测，也很容易被引擎视作误导而受到惩罚）。

	```
	<meta http-equiv="refresh" content="0;url=" />
	```

- **移动设备viewport**，能优化移动浏览器的显示。如果不是响应式网站，不要使用initial-scale或者禁用缩放。
 1. width：宽度（数值 / device-width）（范围从200 到10,000，默认为980 像素）
 2. height：高度（数值 / device-height）（范围从223 到10,000）
 3. initial-scale：初始的缩放比例 （范围从>0 到10）
 4. minimum-scale：允许用户缩放到的最小比例
 5. maximum-scale：允许用户缩放到的最大比例
 6. user-scalable：用户是否可以手动缩 (no,yes)
 7. minimal-ui：可以在页面加载时最小化上下状态栏。（已弃用）
	
 大部分4.7-5寸设备的viewport宽设为360px；5.5寸设备设为400px；iphone6设为375px；ipone6 plus设为414px。
	
 注意，很多人使用initial-scale=1到非响应式网站上，这会让网站以100%宽度渲染，用户需要手动移动页面或者缩放。如果和initial-scale=1同时使用user-scalable=no或maximum-scale=1，则用户将不能放大/缩小网页来看到全部的内容。
```
<meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>
<!-- `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边  -->
```
