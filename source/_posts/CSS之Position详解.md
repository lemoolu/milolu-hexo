---
title: CSS之Position详解
date: 2015-02-10 09:52:56
tags: css
---

转自:http://www.cnblogs.com/Zigzag/archive/2009/02/19/position.html

最近又开始写css了，发现只是会用position，没好好研究过

CSS的很多其他属性大多容易理解，比如字体，文本，背景等。有些CSS书籍也会对这些简单的属性进行大张旗鼓的介绍，而偏偏忽略了对一些难缠的属性讲解，有避重就轻的嫌疑。CSS中主要难以理解的属性包括盒型结构，以及定位。正如positioniseverything，本文将主要讲述关于position的理解，力求让您看完本文后对position有着最全面的认识。

position的四个属性值：

relative

absolute

fixed

static

下面分别讲述这四个属性。

sub1

sub2

1. relative

relative属性相对比较简单，我们要搞清它是相对哪个对象来进行偏移的。答案是它本身的位置。在上面的代码中，sub1和sub2是同级关系，如果设定sub1一个relative属性，比如设置如下CSS代码：

#sub1

{

position: relative;

padding: 5px;

top: 5px;

left: 5px;

}

我们可以这样理解，如果不设置relative属性，sub1的位置按照正常的文档流，它应该处于某个位置。但当设置sub1为的position为relative后，将根据top，right，bottom，left的值按照它理应所在的位置进行偏移，relative的“相对的”意思也正体现于此。

对于此，您只需要记住，sub1如果不设置relative时它应该在哪里，一旦设置后就按照它理应在的位置进行偏移。

随后的问题是，sub2的位置又在哪里呢？答案是它原来在哪里，现在就在哪里，它的位置不会因为sub1增加了position的属性而发生改变。

如果此时把sub2的position也设置为relative，会发生什么现象？此时依然和sub1一样，按照它原来应有的位置进行偏移。

注意relative的偏移是基于对象的margin的左上侧的。

2. absolute

这个属性总是有人给出误导。说当position属性设为absolute后，总是按照浏览器窗口来进行定位的，这其实是错误的。实际上，这是fixed属性的特点。

当sub1的position设置为absolute后，其到底以谁为对象进行偏移呢？这里分为两种情况：

（1）当sub1的父对象(或曾祖父，只要是父级对象)parent也设置了position属性，且position的属性值为absolute或者relative时，也就是说，不是默认值的情况，此时sub1按照这个parent来进行定位。

注意，对象虽然确定好了，但有些细节需要您的注意，那就是我们到底以parent的哪个定位点来进行定位呢？如果parent设定了margin，border，padding等属性，那么这个定位点将忽略padding，将会从padding开始的地方(即只从padding的左上角开始)进行定位，这与我们会想当然的以为会以margin的左上端开始定位的想法是不同的。

接下来的问题是，sub2的位置到哪里去了呢？由于当position设置为absolute后，会导致sub1溢出正常的文档流，就像它不属于 parent一样，它漂浮了起来，在DreamWeaver中把它称为“层”，其实意思是一样的。此时sub2将获得sub1的位置，它的文档流不再基于 sub1，而是直接从parent开始。

（2）如果sub1不存在一个有着position属性的父对象，那么那就会以body为定位对象，按照浏览器的窗口进行定位，这个比较容易理解。

3. fixed

fixed是特殊的absolute，即fixed总是以body为定位对象的，按照浏览器的窗口进行定位。

4. static

position的默认值，一般不设置position属性时，会按照正常的文档流进行排列。