# JavaWeb
Project exercises for javaweb

****

|Author|YaphetS911|
|---|---
|E-mail|shengchensb@gmail.com

****

## 目录
1. [创建HTML模板](#1-创建html模板)
2. [通过vw布局适配移动端页面](#2-通过vw布局适配移动端页面)
3. [正确区分块级元素和内联元素](#3-正确区分块级元素和内联元素)
4. [float后外部容器无法撑开](#4-float后外部容器无法撑开)
5. [HTML5声明下height设置百分比无效](#5-html5声明下height设置百分比无效)
6. [禁止点击事件](#6-禁止点击事件)
7. [n个元素内\<button>点击事件](#7-n个元素内button点击事件)
8. [jQuery效果函数](#8-jquery效果函数)
9. [取整函数](#9-取整函数)
10. [\<div>内部元素垂直居中](#10-div内部元素垂直居中)
11. [页面底部固定元素](#11-页面底部固定元素)
12. [background-size属性](#12-background-size属性)

****

## 1. 创建`HTML`模板

```HTML
<!DOCTYPE html> 
<html lang="ZH-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    <meta name="viewport" content="width=device-width,initial-scale=1,
    maximum-scale=1,minimum-scale=1,user-scalable=no">
    <title>****</title> 
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="apple-touch-fullscreen" content="yes">
    <meta name="format-detection" content="telephone=no,email=no">
    <link rel="shortcut icon" href="${pageContext.request.contextPath}/resource/image/****.ico" />
    <link rel="stylesheet" href="${pageContext.request.contextPath}/resource/css/****.css " />
    <script src="${pageContext.request.contextPath}/resource/js/****.js">
    </script>
    <script>	
      <!-- 页面JS写在这里 --> 			
    </script>
    <style>
      <!-- 页面CSS写在这里 -->
    </style>
  </head>
  <body>
    <!-- 页面结构写在这里 --> 
  </body>
</html>
```

## 2. 通过`vw`布局适配移动端页面

随着越来越多的浏览器支持了`vw`特性，在设计移动端页面时，推荐使用`vw`作为长度单位，这样可以解决大部分主流终端下的适配问题，而不用花费太多精力去修改页面样式。根据相关的测试，在以下这些地方可以使用`vw`来适配我们的页面：  
* 容器适配，可以使用`vw`；   
* 文本的适配，可以使用`vw`；  
* 大于1px的边框、圆角、阴影都可以使用`vw`；  
* 内距和外距，可以使用`vw`。 

有关`vw`的详细介绍，可以参考以下这篇文章：  
[再聊移动端页面的适配](https://www.w3cplus.com/css/vw-for-layout.html "再聊移动端页面的适配")

## 3. 正确区分块级元素和内联元素

常见的块级元素：`div`、`p`、`form`、`li`等；  
常见的内联元素：`span`、`input`、`label`、`select`等。  
其最明显的区别室块级元素会占一行，而内联元素则不会换行，宽度随内容而定。  
目前遇到过两个问题：  
* 多行`<span>`对齐，如下图：

![alt](/image/1.png)

由于两行中标题宽度不一致，导致后面的内容无法对齐。为此，我们希望为标题`<span>`设定一个宽度，但前面说到，内联元素宽度随内容而定，对其设定`width`是无效的。这里就要用到`display:inline-block`这个属性了。`inline-block`是`CSS2.1`新增的值，简单来说就是使其既具有`block`的宽度高度特性又具有`inline`的同行特性。我们对标题的`CSS`做如下设定：
```CSS
.title {
  display: inline-block;
  width: 30%;
}
```
这样就可以达到我们要的效果，如下图：

![alt](/image/2.png)

* 同行中，`<span>`之间会有间隙。例如：
```HTML
<div>
  <span>甲</span>
  <span>乙</span>
  <span>丙</span>	
</div>
```
此时，页面表现如下图：

![alt](/image/3.png)

可以看到，三个`<span>`之间出现了间隙，其原因是标签段之间有换行符，在浏览器加载页面时，这个换行符就会占用一个空格的空间。最简单的解决办法就是改变`HTML`结构，即：
```HTML
<div>
  <span>甲</span
  ><span>乙</span
  ><span>丙</span>	
</div>
```
这样既不用将所有`<span>`写在同一行中，又去掉了换行符，效果如下图：

![alt](/image/4.png)

## 4. `float`后外部容器无法撑开

有时我们需要对元素设置`float`属性，此时其父元素就会无法包裹这些元素，如下图：

![alt](/image/5.png)

这是因为内部的元素因为使用`float: left`之后，就丢失了`clear: both`的样式，所以外部的容器不会被撑开。  
有两个较好的解决办法：  
* 对父元素设置`overflow`属性，可以是`overflow: auto`或是`overflow: hidden`；
* 利用伪元素，直接在父元素的最后增加一个`clear: both`的动作：
```CSS
.container::after {
  content: "";
  display: block;
  clear: both;
  visibility: hidden;
}
```
这样就可以解决外部容器无法撑开的问题了，如下图：

![alt](/image/6.png)

## 5. `HTML5`声明下`height`设置百分比无效

当页面声明为`HTML4`时，如：
```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
```
对元素设置`height：10%`，元素高度会被设置为浏览器高度的10%；
但当页面声明为`HTML5`时，如：
```HTML
<!DOCTYPE html>
```
对元素设置`height：10%`，元素高度会等于0。这是由于其父元素`height`属性值为0，因此需要对`html`和`body`设定`height`属性，`CSS`代码如下：
```CSS
body,html {
  height: 100%;		
}
```

## 6. 禁止点击事件

当我们希望禁止某个元素与用户发生交互效果，可以对其做如下设置：
```CSS
.container::after {
  pointer-events:none;
}
```

## 7. n个元素内`<button>`点击事件

假设页面上有n个`<div>`（其id分别为tab1，tab2……tabn），每个`<div>`中都包含一个`<button>`（其id分别为btn1，btn2……btnn，class为a）。当点击某个`<button>`时，希望对该`<div>`执行一些列操作（例如隐藏该`<div>`），可以对`JS`做如下设置：
```JS
$(".a").click(function() {
  var n = this.id.substring(3);
  $("#div"+n).hide();
});
```

## 8. `jQuery`效果函数

目前接触过以下几个效果函数：
* `slideDown()`/`slideUp()`函数  
`slideDown()`/`slideUp()`与`show()`/`hide()`不同的是，前者是向下展开/向上收缩，而后者是向右下展开/向左上收缩。实际应用中，`slideDown()`/`slideUp()`往往更常用，其使用方法如下：
```JS
$(selector).slideUp(speed,callback)
```
* `animate()`函数  
`animate()`方法执行`CSS`属性集的自定义动画。该方法通过`CSS`样式将元素从一个状态改变为另一个状态。`CSS`属性值是逐渐改变的，这样就可以创建动画效果, 其使用方法如下：
```JS
$(selector).animate(styles,speed,easing,callback)
```
一个点击`<button>`后页面滚动到指定`<div>`顶部的案例：
```JS
$("#btn ").click(function() {
  $("html, body").animate({scrollTop: $("#div").offset().top}, 300); 
});
```

## 9. 取整函数

在上个案例中，点击`<button>`页面虽然滚动到了指定`<div>`的顶部，但实际应用中去发现，经常发生`<div>`顶部距离页面底部还有一点距离。如下图：

![alt](/image/10.png)

这是由于浏览器在处理非整数值（如20.6666px）时造成的。为了使滚动效果一致，我们可以根据具体情况在滚动距离外添加向上取整、向下取整或四舍五入函数，具体使用方法如下：
```JS
Math.ceil(5.21)         //向上取整，结果为6 
Math.floor(5.55)        //向下取整 结果为5 
Math.round(5.78)        //四舍五入 结果为6 
```
在案例中，对`<button>`点击事件做如下修改，即可去除`<div>`顶部的距离：
```JS
$("#btn").click(function() {
  $("html, body").animate({scrollTop: Math.ceil($("#div").offset().top)}, 300); 
});
```

## 10. `<div>`内部元素垂直居中

当需要在一个`<div>`中，使其某个子元素水平居中，我们可以对父元素设置`text-align: center`或者对子元素设置`margin: 0 auto`（子元素要求必须有宽度，且未设置`float`属性）。但如果要使子元素垂直居中，且其高度尚未确定时，我们就需要使用`transform`这个属性了。`transform`属性向元素应用2D或3D转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。我们对内部子元素做如下设置：
```CSS
.child {
  position: relative;
  top: 50%;
  transform: translateY(-50%);	
}
```
这样就可以使`<div>`内部的元素垂直居中，效果如下图：

![alt](/image/7.png)

## 11. 页面底部固定元素

当我们希望在页面的底部固定一些元素（如备注、特别说明等）的时候，我们可以采用如下结构：
`CSS`结构：
```CSS
.html, body {
  height: 100%;
}
.wrapper {
  min-height: 100%;
  height: auto !important;
  height: 100%;
  margin: 0 auto -xpx;        //x为底部元素的高度+margin，下同
}
.footer {
  height: xpx;
}
.push {
  height: xpx;
  pointer-events: none;
}
```
`HTML`结构：
```HTML
<body>
  <div class="wrapper">
    <!-- 页面主体内容写在这里 --> 
    <div class="push"></div>
  </div>
  <div class="footer">
    <!-- 页面底部内容写在这里 --> 
  </div>
</body>
```
这样，无论页面主体部分内容有多少，底部内容将永远固定在页面底部，不会影响页面整体布局。

## 12. `background-size`属性
在设置`background-image`属性时，往往需要同时设置`background-size`来确保图片完整显示。但是由于图片大小有时会因为布局的关系，出现非整数值长宽（如10.23px），此时如果`background-size`设置为`100%`或者`cover`，在部分浏览器下（如safari）会出现被截掉1px的现象，如下图：

![alt](/image/8.png)

因此，设置`background-image`属性时，推荐将`background-size`值设为`contain`，即可避免此现象。下图为设置`background-size: contain`后的效果：

![alt](/image/9.png)

****

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
