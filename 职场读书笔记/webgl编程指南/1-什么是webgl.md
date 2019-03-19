# 1-什么是webgl

# 1.webgl概述

webgl是一项用来在网页上绘制和渲染复杂三维图形（3D图形）,并允许用户与之进行交互的技术。

传统意义上，为了显示三维图形，开发者需要使用c或者c++语言，辅以专门的计算机图形库，如OpenGL或者Direct3D，来开发一个独立的应用程序。现在用了webgl，我们只需要向已经熟悉的html和javascript中添加一些额外的三维图形学代码，就可以在网页上显示三维图形了。

webgl是内嵌在浏览器中的，你不必安装插件和库就可以使用它，而且它是基于浏览器的，你可以在多个平台上运行webgl程序。

## webgl起源

在个人计算机上使用最广泛的两种三维图形渲染技术是Direct3D和OpenGL，Direct3D是微软DirectX技术的一部分，是一套由微软控制的编程接口，主要用户windows平台。而OpenGL由于其开放性和免费性，在多个平台上被广泛的应用。

webgl根植与OpenGL，但它实际是从OpenGL的一个特殊的版本OpenGL ES中派生出来。OpenGL ES 于2003-2004年被手提出来，并且在2007年（ES2.0）和2012年（ES3.0）进行了两次升级。webgl是基于OpenGL ES 2.0的


![webgl历史](https://images.vrm.cn/2018/12/18/history.png)


## 着色器
 
从2.0版本开始，OpenGL支持一项非常重要的特性，既可编程着色器方法，该特性被OpenGL ES 2.0继承，并成为了WebGL1.0的标准核心部分。
 
 着色器方法，或成为着色器。使用的是一种类似于C的编程语言实现了精美的视觉效果。编写着色器的语言又成为着色器语言。OpenGL ES 2.0 基于OpenGL着色器语言（GLSL），因此后者又被陈伟OpenGL ES 着色器语言（GLSL ES），webgl也使用GLSL ES编写着色器。
 
 ## webgl程序的结构
 
 在html中，动态网页包括html和javascript，现在引入了webgl后，还需要加入着色器语言GLSE ES,也就是说，WegGL页面包含三种语言：HTML5 Javascript和GLSL ES

![网页结构](https://images.vrm.cn/2018/12/19/construct.png)