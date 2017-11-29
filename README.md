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
5. [H5声明下height设置百分比无效](#5-h5声明下height设置百分比无效)
6. [禁止点击事件](#6-禁止点击事件)
7. [n个元素内\<button>点击事件](#7-n个元素内button点击事件)
8. [jQuery效果函数](#8-jquery效果函数)
9. [取整函数](#9-取整函数)
10. [\<div>内部元素垂直居中](#10-div内部元素垂直居中)
11. [页面底部固定元素](#11-页面底部固定元素)
12. [background-size属性](#12-background-size属性)

****

## 1. 创建HTML模板

```html
<!DOCTYPE html> 
<html lang="ZH-CN"> 
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width,initial-scale=1,
        maximum-scale=1,minimum-scale=1,user-scalable=no">
        <title>****</title> 
        <meta name="apple-mobile-web-app-capable" content="yes">
        <meta name="apple-mobile-web-app-status-bar-style" content="default">
        <meta name="apple-touch-fullscreen" content="yes">
        <meta name="format-detection" content="telephone=no,email=no">
        <link rel="shortcut icon" href="${pageContext.request.contextPath}/resource/image/****.ico" />
        <link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath}/resource/css/****.css " />
        <script type="text/javascript" src="${pageContext.request.contextPath}/resource/js/****.js"></script>
        <script type="text/javascript">	
            <!-- 页面JS写在这里 --> 			
        </script>
        <style type="text/css">
            <!-- 页面CSS写在这里 -->
        </style>
    </head> 
    <body> 
        <!-- 页面结构写在这里 --> 
    </body>
</html>
```

## 2. 通过vw布局适配移动端页面

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
.title{
    display: inline-block;
    width: 30%;
}
```
这样就可以达到我们要的效果，如下图：

![alt](/image/2.png)

* 同行中，`<span>`之间会有间隙。例如：
```html
<div>
    <span>甲</span>
    <span>乙</span>
    <span>丙</span>	
</div>
```
此时，页面表现如下图：

![alt](/image/3.png)

可以看到，三个`<span>`之间出现了间隙，其原因是标签段之间有换行符，在浏览器加载页面时，这个换行符就会占用一个空格的空间。最简单的解决办法就是改变`HTML`结构，即：
```html
<div>
    <span>甲</span
    ><span>乙</span
    ><span>丙</span>	
</div>
```
这样既不用将所有`<span>`写在同一行中，又去掉了换行符，效果如下图：

![alt](/image/4.png)

## 4. float后外部容器无法撑开

有时我们需要对元素设置`float`属性，此时其父元素就会无法包裹这些元素，如下图：

![alt](/image/5.png)

这是因为内部的元素因为使用`float:left`之后，就丢失了`clear:both`的样式，所以外部的容器不会被撑开。  
有两个较好的解决办法：  
* 对父元素设置`overflow`属性，可以是`overflow:auto`或是`overflow:hidden`；
* 利用伪元素，直接在父元素的最后增加一个`clear: both`的动作：
```CSS
.container::after{
    content: "";
    display: block;
    clear: both;
    visibility: hidden;
}
```
这样就可以解决外部容器无法撑开的问题了，如下图：

![alt](/image/6.png)

## 5. H5声明下height设置百分比无效
## 6. 禁止点击事件
## 7. n个元素内\<button>点击事件
## 8. jQuery效果函数
## 9. 取整函数
## 10. \<div>内部元素垂直居中
## 11. 页面底部固定元素
## 12. background-size属性

****

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
