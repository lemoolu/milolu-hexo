---
title: '$parse,$eval,$observe,$watch如何区分'
date: 2016-04-07 16:21:43
tags: AngularJs
---

http://www.ngnice.com/posts/2314014da4eea8

自定义select指令时需要根据ng-disabled字段来设置指令的状态

$parse是作为一个单独的服务存在的。$eval是作为scope的方法来使用的。

$parse典型的使用是放在设置字符串表达式映射在真实对象上的值。也可以从$parse上直接获取到表达式对应的值。

$eval 即scope.$eval，是执行当前作用域下的表达式，如：scope.$eval('a+b'); 而这个里的a,b是来自 scope = {a: 2, b:3};

$observe是属性对象上的方法，因此它是用来监控DOM属性上的值的变化，它仅用在指令内部，当你需要在指令内部监控包含有插值表达式的DOM属性的时候，就要用到这个方法

$watch更复杂一点，它可以监视表达式，这个表达式可以是函数或者字符串，假如表达式是字符串的话，会被封装成一个函数，然后在digest循环的时候被调用。 这个字符串表达式不能包含{ { } },$watch是一个scope对象上的方法，所以它可以在任何你可以访问到作用域的地方被调用。比如，控制器中或者link函数中。因为字符串是被当做angular的表达式解析的，所以$watch经常被用在当你想要监控一个模型或者作用域对象的时候
