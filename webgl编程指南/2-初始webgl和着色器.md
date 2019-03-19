# 2-初始webgl和着色器

# 前言

*原书中第2章非常长，如果整理成一个文档的话，得看好多天。为了浏览方便，我将其拆分成若干小节，方便大家学习。*


webgl采用HTML5中引入的canvas元素来定义页面的绘图区域。如果没有WegGL，js只能在canvas上绘制二维图形。

在这一章中，我们将通过创建若干个示例程序，一步步的介绍一些核心的webgl函数。

## 1.最短的webgl程序：清空绘图区

我们要开始编写最短的webgl程序，这个程序的主要功能是使用背景色清空canvas标签的绘图区。（这里的清空，是指用背景色填充的意思）

html代码如下；

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>webgl学习</title>
</head>

<body>
  <canvas id="canvas" width="400" height="400"></canvas>
  <script src="./index.js"></script>
</body>

</html>
```
js代码如下:
```javascript

// 获取canvas
var canvas = document.getElementById('canvas')
// 获取webgl绘图上下文
var gl = canvas.getContext('webgl')

if (!gl) {
  console.log('Failed to get the rendering context')
}
// 指定清空canvas的颜色
gl.clearColor(0.0, 0.0, 0.0, 1.0)

// 清空canvas
gl.clear(gl.COLOR_BUFFER_BIT)

```
示例程序的执行步骤如下：
1. 获取canvas元素
2. 获取webgl的绘图上下文
3. 设置背景色
4. 清空canvas

[点击查看效果](https://codepen.io/guoxianqiang/pen/GPjjYZ/)

## 2. 程序解析

### 2.1 获取canvas元素

我们通过getElementById获取canvas元素

```javascript
var canvas = document.getElementById('canvas')
```

### 2.2 为webgl获取绘图上下文

这里传入的参数是‘webgl’而不是‘3d’。我们以前使用canvas画2d图形的时候传入的是‘2d’。所以这里要注意下，不要传错了。

```javascript
var gl = canvas.getContext('webgl')
```

### 2.3 设置canvas的背景色

```javascript
gl.clearColor(0.0, 0.0, 0.0, 1.0)
```

gl.clearColor采用RGBA的方式设置背景色：

```javascript
gl.clearColor(red,green,blue,alpha)
```

参数 | 含义
---|---
red | 指定红色值（从0.0到1.0）
green | 指定绿色值（从0.0到1.0）
blue | 指定蓝色值（从0.0到1.0）
alpha | 指定透明度（从0.0到1.0）
返回值 | 无
错误 | 无

**如果任何值小于0或者大于1，那么就会被截断为0.0或者1.0**

示例程序执行`gl.clearColor(0.0, 0.0, 0.0, 1.0)`，背景色就被指定为了黑色。

我们一般指定颜色时，颜色的值是0到255之间的，但是由于webgl是继承自OpenGL，所以它遵循传统OpenGL颜色分量的取值范围。

一旦指定的背景色，背景色就会驻存在WegGL系统中，在下一次调用gl.clearColor之前都不会改变。如果你将来还想用同一个颜色清空绘图区，就没必须要再指定一次背景色。

### 2.4 清空canvas

```
gl.clear(gl.COLOR_BUFFER_BIT);
```
函数参数是`gl.COLOR_BUFFER_BIT`,这是因为WebGL中，gl.clear()方法实际上继承自OpenGL，它基于多基本缓冲区模型。这比二维绘图上下文复杂的多。清空绘图区域，实际上在清空颜色缓冲区，传递参数gl.COLOR_BUFFER_BIT就是告诉WebGL清空颜色缓冲区。除了颜色缓冲区，WebGL还有其他种类的缓冲区，比如深度缓冲区和模板缓冲区。

`gl.clear(buffer)`的用法

buffer是指定待清空的颜色缓冲区，位操作符OR(|)可用于指定多个缓冲区
参数 | 含义
---|---
gl.COLOR_BUFFER_BIT | 颜色缓冲区
gl.DEPTH_BUFFER_BIT | 深度缓冲区
gl.STENCIL_BUFFER_BIT | 指定模板缓冲区
返回值 | 无
错误 | INVALID_VALUE 缓冲区不是以上三种类型


## 3. 绘制一个点

之前的章节，我们学习如何建立一个wegl程序，以及如何使用一些简单的webgl函数。这一节我们将进一步在一个示例程序中绘制一个最简单的图形，一个点。

注意这里的html比上一次的html多了一些script。这些script是一些工具函数，避免重复的写大量的代码。代码很简单，可以直接打开查看。这里就不解释了。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>webgl学习</title>
</head>

<body>
  <canvas id="canvas" width="400" height="400"></canvas>
  <!-- 引入几个webgl帮助函数，这里的代码很简单，你可以直接点击查看 -->
  <script src="https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/webgl-lib/lib/webgl-utils.js"></script>
  <script src="https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/webgl-lib/lib/webgl-debug.js"></script>
  <script src="https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/webgl-lib/lib/cuon-utils.js"></script>
  <script src="./index.js"></script>
</body>

</html>
```
javascript代码：

```javascript
//  顶点着色器 注意这里的\n不能省略。否则不符合语法
var VSHADER_SOURCE = `
  void main(){\n
    gl_Position = vec4(0.0,0.0,0.0,1.0);\n
    gl_PointSize = 10.0;\n
  }\n
`
// 片段（片元）着色器
var FSHADER_SOURCE = `
  void main(){\n
    gl_FragColor = vec4(1.0,0.0,0.0,1.0);\n
  }\n
`

function main() {
  // 获取canvas
  var canvas = document.getElementById('canvas')
  // 获取webgl绘图上下文
  var gl = canvas.getContext('webgl')

  if (!gl) {
    console.log('Failed to get the rendering context')
    retun
  }

  // 初始化着色器函数
  if (!initShaders(gl, VSHADER_SOURCE, FSHADER_SOURCE)) {
    console.log("设置着色器失败")
    return
  }

  // 指定清空canvas的颜色
  gl.clearColor(0.0, 0.0, 0.0, 1.0)

  // 清空canvas
  gl.clear(gl.COLOR_BUFFER_BIT)
  // 画一个点
  gl.drawArrays(gl.POINTS, 0, 1)
}

window.onload = function () {
  main()
}
```

[点击查看效果](https://codepen.io/guoxianqiang/pen/vvgGog)

### 3.1 着色器是什么？

要使用webgl进行绘图必须使用着色器，在代码中，着色器是以字符串的形式“嵌入”到js中的。

webgl的着色器分两种

- **顶点着色器**:顶点着色器用来描述顶点的特性（如位置、颜色等）的程序，顶点是指二维或者三维空间的一个点，比如二维或者三维图形的端点和交点。
- **片元着色器**：也叫片段着色器。负责进行逐片元处理过程如光照的程序。片元是webgl的一个术语。你可以理解它为像素。

在三维场景中，仅仅用线条和颜色把图形画出来是远远不够的，必须要考虑光照上去或者观察者视角发生变化时，对场景有什么影响。着色器可以高度灵活的完成这些工作。提供各种渲染效果。

上述程序执行流程：

- 先运行javascript程序
- 调用webgl的相关方法
- 执行顶点着色器
- 执行片元着色器
- 在颜色缓冲区进行绘制
- 清空绘图区
- 颜色缓冲区的内容自动在浏览器的canvas上显示出来

### 3.2 initShaders

`initShaders()`,该函数定义在cuon.util.js中，是专为本书编写的一个工具函数。其用法说明如下：

`initShaders(gl,vshader,fshader)`

类型 | 参数 | 含义
---|---|---
gl | vshader | 指定顶点着色器程序代码
 | | fshader |  指定片元着色器程序代码
返回值 | true | 初始化着色器成功
 | | false | 初始化着色器失败


### 3.3 顶点着色器

```javascript
var VSHADER_SOURCE = `
  void main(){\n
    gl_Position = vec4(0.0,0.0,0.0,1.0);\n
    gl_PointSize = 10.0;\n
  }\n
`
```

顶点着色器必须和c语言一样，必须包含一个main函数。main前面的void表示这个函数不会有返回值。

首先将顶点位置赋值给gl_Position变量。

在将顶点尺寸赋值给gl_PointSize。

这两个变量内置在顶点着色器中，有特殊的含义。如下：

类型和变量名 | 含义
---|---
vec4 gl_Position | 表示顶点的位置
float gl_PointSize | 表示顶点的尺寸

vec4 表示由4个浮点数组成的矢量。

**注意：GLSL ES是一种强类型编程语言，比10是整型，10.0就是浮点数，传错类型就会报错。**

gl_Position其类型为vec4，但是我们只有3个浮点数（0.0,0.0,0.0）,既x、y、z坐标值。需要用某种方式把它转为vec4类型的变量。好在着色器为我们提供了内置函数vec4(),帮助你创建vec4类型的变量。（原书中这一段有错误，已纠正）

在赋值gl_Position的矢量中，我们添加了1.0作为第4个分量。由4个分量组成的矢量称为齐次坐标。，因为它能够提高处理三维数据的效率。所以被大量使用。虽然齐次坐标是四维的，但是如果最后一个分量为1.0，那么齐次坐标就可以表示“前三个分量为坐标值”的那个点。



### 3.4 片元着色器

片元着色器是用来设置颜色的，这里设置为红色。

```javascript
var FSHADER_SOURCE = `
  void main(){\n
    gl_FragColor = vec4(1.0,0.0,0.0,1.0);\n
  }\n
`
```
gl_FragColor的用法如下：

类型和变量名 | 含义
---|---
vec4 gl_FragColor | 指定片元颜色（RGBA格式）

### 3.5 绘制操作

我们使用drawArrays()进行绘制。

```javascript
gl.drawArrays(gl.POINTS, 0, 1)
```

`gl.drawArrays(mode,first,count)`


参数 | 含义
---|---
mode | 指定绘制的方式，可以接受以下常量符号: gl.POINTS ,gl.LINES ,gl.LINE_STRIP ,gl.LINE_LOOP ,gl.TRIANGLES ,gl.TRIANGLE_STRIP ,gl.TRIANGLE_FAN
first | 指定从哪个点开始绘制
count | 指定绘制多少个点
错误 INVALID_ENUM | 传入的mode参数不是前述参数之一
错误 INVALID_VALUE | 参数first或者count为负数

*总结：以上我们简单介绍了两个webgl程序，并初步认识了着色器。后续的章节我们将学习webgl的坐标系统和更多着色器的相关知识。欲知后事如何，请动动你的手指，关注下我，我会继续带来webgl的分享，哈哈*
