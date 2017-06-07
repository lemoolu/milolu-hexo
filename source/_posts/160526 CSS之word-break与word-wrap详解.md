---
title: CSS之word-break与word-wrap详解
date: 2016-05-26 14:44:56
tags: css
---


## word-break
### 定义
css的 word-break 属性用来标明怎么样进行单词内的断句。
提示：通过使用 word-break 属性，可以让浏览器实现在任意位置的换行。

### 语法
word-break: normal|break-all|keep-all;

* normal:使用浏览器默认的换行规则。
* break-all: 允许在单词内换行。
* keep-all:     只能在半角空格或连字符处换行。




## word-wrap
### 定义
css的 word-wrap 属性用来标明是否允许浏览器在单词内进行断句，这是为了防止当一个字符串太长而找不到它的自然断句点时产生溢出现象
### 语法
word-wrap: normal|break-word;
* normal: 只在允许的断字点换行（浏览器保持默认处理）。
* break-word: 在长单词或 URL 地址内部进行换行。

**word-wrap 强调的是是否允许单词内断句，而word-break强调的则是怎么样来进行单词内的断句。**

## 演示
<style>
.test{
width:200px;
text-align: left;
padding: 0;
background: #7ECDF4;
}
</style>

word-wrap或word-break的时候，就是浏览器默认的时候，如果有一个单词很长，导致一行中剩下的空间已经放不下它时，则浏览器会把这个单词挪到下一行去


```javascript
<div class="test">
测试文本,this an is sadsdddddddddddddddddddddddddddddddd
</div>
```
<div class="test">
测试文本,this an is sadsdddddddddddddddddddddddddddddddd
</div>


为了防止长单词溢出，就在它的内部断句了。这就是 word-wrap:break-word 的功能
```javascript
<div class="test" style="word-wrap: break-word">
测试文本,this an is sadsdddddddddddddddddddddddddddddddd
</div>
```
<div class="test" style="word-wrap: break-word">
测试文本,this an is sadsdddddddddddddddddddddddddddddddd
</div>


word-break:break-all它不会尝试把长单词挪到下一行，而是直接进行单词内的断句
```javascript
<div class="test" style="word-wrap: break-word; word-break: break-all">
测试文本,this an is sadsdddddddddddddddddddddddddddddddd
</div>
```
<div class="test" style="word-wrap: break-word; word-break: break-all">
测试文本,this an is sadsdddddddddddddddddddddddddddddddd
</div>


