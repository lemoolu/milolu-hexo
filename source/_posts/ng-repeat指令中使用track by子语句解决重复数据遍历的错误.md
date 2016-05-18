---
title: ng-repeat指令中使用track by子语句解决重复数据遍历的错误
date: 2016-04-08 10:41:02
tags: AngularJs
---

我们可以使用ng-repeat指令遍历一个javascript数组，当数组中有重复元素的时候,angularjs会报错:

Error: [ngRepeat:dupes] Duplicates in a repeater are not allowed. Use 'track by' expression to specify unique keys. Repeater: user in users, Duplicate key: number:1。下面的代码就会报错：

```javascript
function rootController($scope,$rootScope,$injector)  
{  
  $scope.dataList = [1,2,1];  
}  
<div ng-repeat="data in dataList">  
  {{data}}      
</div>   
```

这是因为ng-Repeat不允许collection中存在两个相同Id的对象。
For example: item in items is equivalent to item in items track by $id(item). This implies that the DOM elements will be associated by item identity in the array.
对于数字或者字符串等基本数据类型来说，它的id就是它自身的值。因此数组中是不允许存在两个相同的数字的。为了规避这个错误，需要定义自己的track by表达式。
// 业务上自己生成唯一的id
item in items track by item.id
//或者直接拿循环的索引变量$index来用
item in items track by $index

如果是javascript对象类型数据，那么就算内容一摸一样，ng-repeat也不会认为这是相同的对象。如果将上面的代码中dataList，那么是不会报错的。比如$scope.dataList = [{"age":10},{"age":10}];