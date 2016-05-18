---
title: angularjs指令中的compile与link函数
date: 2016-04-14 10:27:02
tags: AngularJs
---

在angularJs应用启动之前，它们是以HTML文本形式存在文本编辑器当中。应用启动会进行编译和链接，作用域会同HTML进行绑定。

　　在编译的阶段，angularJs会遍历整个的文档并根据JavaScript中指令定义来处理页面上什么的指令。通常情况下，如果设置了compile函数，说明我们希望在指令和实时数据被放到DOM中之前，进行DOM操作，在这个函数中进行诸如添加和删除节点等DOM操作是安全的。
本质上，当我们设置了link选项，实际上是创建了一个postLink() 链接函数，以便compile() 函数可以定义链接函数。编译函数compile负责对模板DOM进行转换。链接函数link负责将作用域和DOM进行链接。

![](http://upload-images.jianshu.io/upload_images/59687-053e8b8ee76b74a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```javascript
//demo1 最简单的指令创建
ui.directive('header', function () {
  return {
    restrict: 'AE',
    link: function (scope, element, attrs) {
    }
  }
});
```

```javascript
//demo2 等价于demo1
ui.directive('header', function () {
  return {
    restrict: 'AE',
    compile: function (element, attrs, transclude) {
      return : function(scope, element, attrs){

      }
    }
  }
});
```

```javascript
//demo3 等价于demo1
ui.directive('header', function () {
  return {
    restrict: 'AE',
    compile: function (element, attrs, transclude) {
      return {             
        pre: function(scope, element, attrs){},             
        post: function(scope, element, attrs){}
      }
    }
  }
});
```

如果同时设置了compile与link，那么会把compile所返回的函数当作链接函数，而link选项本身则会被忽略。
