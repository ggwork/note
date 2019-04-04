## gulp的使用方法

### 1、安装gulp

```powershell
npm install --save-dev gulp
```

### 2、创建gulpfile.js文件

```javascript
var gulp = require("gulp");
gulp.task("default", function() {
    console.log("hello");
})
```

### 3、gulp命令

#### 3.1 gulp.src(globs,[options])

作用：将匹配的文件转化为数据流然后通过pipe方法，将数据流传递给对应的插件进行处理。

globs为路径匹配模式，类似正则表达式，但是跟正则表达式是不同的。可以直接写真实的页面路径。

比如需要对a.txt这个文件进行某种处理，可以这样引入a.txt文件

```javascript
gulp.src("a.txt")
```

#### 3.2 gulp.dest(path[,options])

作用：将pipe进来的数据流转化为文件。注意文件名是不会变的。如果要改文件名就需要相应的gulp插件

```javascript
gulp.task("default", function() {
    gulp.src("aa.txt")
        .pipe(gulp.dest("test"));
})
```

path为文件将被写入的路径（输出目录）。也可传入一个函数，在函数中返回相应的路径。

#### 3.3 gulp.task(name[, deps], fn)

作用：定义一个使用gulp任务

- name类型为字符串，含义为任务的名字。
- deps一个包含任务列表的数组，这些任务会在你当前任务运行之前完成（如果任务是异步任务的话，还需要做进一步的特殊处理）。**数组中的任务并不是按照他们在数组中的先后顺序执行，而是是并发执行的**
- fn 该函数定义任务所要执行的一些操作

创建一个任务a，打印一个hello world。

```javascript
gulp.task("a",function(){
  console.log("hello world");
})
```



#### 3.4 gulp.watch(glob [, opts], tasks) 或 gulp.watch(glob [, opts, cb])

监视文件，并且在文件变动的时候执行某些任务。

##### 3.4.1 gulp.watch(glob [, opts], tasks)

比如监视js文件，当js文件变动的时候，压缩js文件到dest文件夹。文件变动包括：1、新增文件，2、改变文件内容，3、删除文件，4、重命名文件

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
var uglify = require("gulp-uglify");
gulp.task("compassJs", function() {
    gulp.src("*.js")
        .pipe(uglify())
        .pipe(gulp.dest("test"));
})
gulp.task("default", function() {
    gulp.watch("*.js", ["compassJs"]);
})
```

##### 3.4.2 gulp.watch(glob [, opts, cb])

比如监视js文件，当文件变动的时候，执行某些操作。1、新增文件，2、改变文件内容，3、删除文件，4、重命名文件

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
var uglify = require("gulp-uglify");
gulp.task("default", function() {
    gulp.watch("*.js", function(event) {
        console.log(event.path, event.type);
    });
})
```













## 进阶篇

### 1、gulp.src(globs,[options])

gulp引入需要处理的文件，采用的方法是glob匹配文件路径。glob匹配具体规则如下

#### 1.1、glob 模式匹配

[点击在github上查看完整文档](https://github.com/isaacs/node-glob) 

- `*` 匹配0个或者多个任意字符
- `?` 匹配任意一个字符
- `[...]`范围匹配，如果字符在中括号范围内，则匹配。使用方法类似正则表达式的中括号。如果中括号中以`!`或者`^`开头，则当字符不在中括号中时才匹配
- `!(pattern|pattern|pattern)` 不满足括号中的所有模式则匹配
- `?(pattern|pattern|pattern)`匹配括号中的0个或者多个模式
- `+(pattern|pattern|pattern)` 匹配括号中 1 或 更多模式，或者模式之间的组合
- `*(pattern|pattern|pattern)`匹配括号中的0个或者多个模式，或者任意模式的组合。
- `@(pattern|pattern|patter)`满足 1 个括号中的模式则匹配
- `**` 单独使用时匹配n级目录或者n级目录+文件，如果跟其他模式混用则匹配某个目录或者某个文件

glob的使用方法：

```javascript
//其中pattern 为glob模式，options为glob配置选项，如果没有可以省略。function为匹配的回调函数，参数er为错误对象，files为匹配成功的文件名数组。
var glob = require("glob")
glob(pattern, options, function (er, files) {
  // files is an array of filenames.
  // If the `nonull` option is set, and nothing
  // was found, then files is [pattern]
  // er is an error object or null.
})
```

glob的使用案例：

目录结构：

```
--glob.js
--a.txt
--b.txt
--node_modules
```

假设有一个glob.js文件，代码如下，在glob同级目录下有a.txt,b.txt两个文件。命令行执行node glob.js。则可以匹配到a.txt,b.txt 两个文件：

```javascript

glob("*.txt", {}, function(err, files) {
    console.log("files：", files); // files:["a.txt","b.txt"]
});
```

##### 1.1.1 点的特殊处理

目录名或者文件名如果以 `.` 开头，只有模式中对应部分以 `.` 开头才能匹配。 

如果在option中设置`dot:true`，glob就会把`.`当成普通的字符来处理，这时候就可以使用*或者？来匹配.了。

##### 1.1.2 matchBase匹配

如果option中设置了matchBase为true，则glob会遍历同级目录及其子目录下任意符合匹配模式的文件

##### 1.1.3 空输出

如果没有文件或者目录符合模式，则返回一个空数组

#### 1.2 完整的测试案例

目录结构

```
+--b
| +--b.txt
+--c
| +--c2
| | +--c.txt
+--..git
+--a.txt
+--a1.txt
+--aa.txt
+--ab.txt
+--ac.txt
+--b.txt
+--ba.txt
+--c.txt
+--glob.js
```

glob.js代码及输入结果如下：

```javascript
var glob = require("glob");
// *测试
glob("*.txt", {}, function(err, files) {
    console.log("*符号测试", files);   
});

//?测试
glob("?.txt", {}, function(err, files) {
    console.log("?符号测试", files);
});
//[]测试
glob("a[a-z].txt", {}, function(err, files) {
    console.log("[]符号测试", files);
});
// !()测试
glob("!(a|b).txt", {}, function(err, files) {
    console.log("!()符号测试", files);
});
// ?()测试
glob("?(a|b).txt", {}, function(err, files) {
    console.log("?()符号测试", files);
});
// +()测试
glob("+(a|b).txt", {}, function(err, files) {
    console.log("+()符号测试", files);
});
// *()测试
glob("*(a|b).txt", {}, function(err, files) {
    console.log("*()符号测试", files);
});

// @()测试
glob("@(a|b).txt", {}, function(err, files) {
    console.log("@()符号测试", files);
});

// **测试
glob("**", {}, function(err, files) {
    console.log("**符号测试", files);
});
// .测试
glob("?.git", {}, function(err, files) {
    console.log(".符号测试", files);
});
// .测试
glob("?.git", { dot: true }, function(err, files) {
    console.log(".符号测试dot为true", files);
});

// matchBase测试外部终端配置
glob("*.txt", { matchBase: true }, function(err, files) {
    console.log("matchBase测试", files);
});

// 不匹配测试
glob("*.png", { matchBase: true }, function(err, files) {
    console.log("不匹配测试", files);
});

以下为代码的输出结果:
/*
*符号测试 [ 'a.txt',
  'a1.txt',
  'aa.txt',
  'ab.txt',
  'ac.txt',
  'b.txt',
  'ba.txt',
  'c.txt' ]
?符号测试 [ 'a.txt', 'b.txt', 'c.txt' ]
[]符号测试 [ 'aa.txt', 'ab.txt', 'ac.txt' ]
!()符号测试 [ 'a1.txt', 'aa.txt', 'ab.txt', 'ac.txt', 'ba.txt', 'c.txt' ]
?()符号测试 [ 'a.txt', 'b.txt' ]
+()符号测试 [ 'a.txt', 'aa.txt', 'ab.txt', 'b.txt', 'ba.txt' ]
*()符号测试 [ 'a.txt', 'aa.txt', 'ab.txt', 'b.txt', 'ba.txt' ]
@()符号测试 [ 'a.txt', 'b.txt' ]
.符号测试 []
.符号测试dot为true [ '..git' ]
**符号测试 [ 'a.txt',
  'a1.txt',
  'aa.txt',
  'ab.txt',
  'ac.txt',
  'b',
  'b.txt',
  'b/b.txt',
  'ba.txt',
  'c',
  'c.txt',
  'c/c2',
  'c/c2/c.txt',
  'glob.js' ]
matchBase测试 [ 'a.txt',
  'a1.txt',
  'aa.txt',
  'ab.txt',
  'ac.txt',
  'b.txt',
  'b/b.txt',
  'ba.txt',
  'c.txt',
  'c/c2/c.txt' ]
不匹配测试 []
*/
```

#### 1.3 options选项

glob的options是通过gulp.src(globs,[options])中的options传入的。gulp的option处理包含glob的所有配置选项外还扩展了一些自己的内容。

##### 1.3.1 options.buffer

默认options.buffer为true。gulp.src将文件转为vinyl类型的数据流的时候，默认是以buffer存储数据内容的。gulp的插件默认也是处理buffer类型的数据。buffer类型的数据流有个缺点就是需要全部读取完文件才能处理。这样处理速度就会相对较慢。

可以将options.buffer设置为false，这样gulp.src将文件转为vinyl类型的数据流的时候，就会采用stream的形式存储数据内容。这样可以提高处理大文件的速度。缺点支持stream类型的插件较少。

##### 1.3.2 options.read

默认options.read为true。如果设置为false。gulp.src就不会去读取文件

##### 1.3.3 options.base

字符串类型，默认返回路径中glob模式之前的字符串或者路径中匹配成功之前的字符串

**以上为glob常用的使用方法，完整的使用方法需要在github上查看。glob的options是通过gulp.src(globs,[options])的options传入的。**

### 2、 vinyl-fs

gulp.src会将匹配的文件转化为vinyl类型的数据流，然后通过pipe方法将流传递给对应的插件进行处理。 gulp只支持vinyl类型的数据流，不支持常规的nodejs数据流。

#### 2.1 vinyl是什么

vinyl是一个文件格式，通过它可以很方面的描述一个文件，包括文件的路径、文件名和内容，它也可以很方便的设置或者获取文件的内容。优点就是跨平台，可以将不同操作系统的文件转化为统一的形式。

创建一个vinyl类型的文件

```javascript
//创建vinyl方法 new Vinyl(options)
var Vinyl = require("vinyl");
var jsFile = new Vinyl({
    cwd: "/",
    base: "/test",
    path: "/test/file.js",
    contents: new Buffer("var x=123")
})
```

其中各个选项的含义如下

- cwd——The current working directory of the file 文件当前工作目录，默认为process.cwd()

页面目录结果如下

```
+--b
| +--aa.txt
+--aa.txt
```

程序如下：

```javascript
gulp.task("default", function() {
    gulp.src("aa.txt", { cwd: "b" })
        .pipe(gulp.dest("test"))
})
```

如果没有给gulp.src传{cwd:"b"} gulp选择的文件为跟b文件夹同目录的aa.txt  。现在因为程序传了{cwd:"b"}，所以现在选择的文件为b文件夹下的aa.txt

- base——相对路径，默认为process.cwd()
- path——文件的完整路径
- contents——buffer形式（二进制）的文件内容

参考来源：

[vinyl的npm地址](https://www.npmjs.com/package/vinyl)

### 3、gulp.dest(path[,options])

#### 3.1 options 选项

- cwd 输出目录的cwd参数，只在所给的输出目录是相对路径时有效

程序1：

```javascript
gulp.task("default", function() {
    gulp.src("aa.txt")
        .pipe(gulp.dest("test"));
})
```

程序2：

```javascript
gulp.task("default", function() {
    gulp.src("aa.txt")
        .pipe(gulp.dest("test", { cwd: "hello" }));
})
```

程序1的执行结果

```
+--test
| +--aa.txt
```

程序2的执行结果

```
+--hello
| +--test
| | +--aa.txt

```

程序2会先在目录中生成hello文件夹，然后再输出gulp.dest结果。

- mode 八进制权限字符，用以定义所有在输出目录中所创建的目录的权限。

### 4、gulp.task(name[, deps], fn)

异步任务按顺序执行

```javascript
gulp.task("a", function() {
    console.log("a");
})
gulp.task("b", function() {
    setTimeout(() => {
        console.log("b");
    }, 100);
})
gulp.task("default", ["a", "b"], function() {
    console.log("default");
})
//执行结果控制台输出 a default b
```

如果要确保依赖任务完全执行完毕，再执行当前任务，也就是需要序列化的执行任务，需要做两件事情。

1. 需要给一个提示，来告知task什么时候执行完毕
2. 并且需要再给一个提示，来告知任务之间的依赖关系

方法有3个：

#### 4.1 使用回调函数

有两个任务a、b，b任务里有一个异步函数。如果不做特殊处理，输出结果如下。

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
gulp.task("a", function() {
    console.log("a");
})
gulp.task("b", function() {
    setTimeout(() => {
        console.log("b");
    }, 100);
})
gulp.task("default", ["a", "b"], function() {
    console.log("default");
})
//输出结果 a default b
```

这并不是我们想要的，我们希望a、b执行完了再执行default。我们可以在b任务中调用一个callback函数，来实现我们的要求

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
gulp.task("a", function() {
    console.log("a");
})
gulp.task("b", function(cb) {
    setTimeout(() => {
        console.log("b");
        cb()
    }, 100);
})
gulp.task("default", ["a", "b"], function() {
    console.log("default");
})
// 执行结果 a b default
```

#### 4.2 返回一个stream

有a、b两个任务，b任务需要去执行一个异步任务——读取一张图片。

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
gulp.task("a", function() {
    console.log("a");
})
gulp.task("b", function() {
    gulp.src("b.png")
        .pipe(map(function(data, callback) {
            console.log("b");
            callback(null, data)
        }))
        .pipe(gulp.dest("test"));
})
gulp.task("default", ["a", "b"], function() {
    console.log("default");
})
// 执行结果 a default b
```

default任务在b任务执行完成之前执行了，这不符合我们的要求，我们可以在b任务中返回一个stream，来达到序列化的要求。

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
gulp.task("a", function() {
    console.log("a");
})
gulp.task("b", function() {
    return gulp.src("b.png")
        .pipe(map(function(data, callback) {
            console.log("b");
            callback(null, data)
        }))
        .pipe(gulp.dest("test"));
})
gulp.task("default", ["a", "b"], function() {
    console.log("default");
})
//执行结果 a b default
```

#### 4.3 返回一个promise

我们修改4.1中第一个程序，把它由callback实现序列化改成通过promise来实现序列化

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
gulp.task("a", function() {
    console.log("a");
})
gulp.task("b", function() {
    var p = new Promise(function(resolve, reject) {
        setTimeout(() => {
            console.log("b");
            resolve();
        }, 100);
    })
    return p;

})
gulp.task("default", ["a", "b"], function() {
    console.log("default");
})
//输出结果 a、b、default
```

### 5、gulp的报错处理

执行gulp任务的时候经常会遇到gulp报错的情况，gulp对于错误的处理非常糟糕，一旦报错gulp就会退出工作流，而gulp给出的错误信息，几乎没啥参考价值的。

比如有如下一个程序，监视a.js文件，当a.js文件发生改变的时候就压缩a.js。压缩后的文件放在dest文件夹下。

gulpfile.js代码如下

```javascript
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
var uglify = require("gulp-uglify");
var pump = require("pump");
gulp.task("compassJs", function() {
    gulp.src("a.js")
        .pipe(uglify())
        .pipe(gulp.dest("dest"));
})
gulp.task("default", function() {
    gulp.watch("a.js", ["compassJs"]);
})
```

当我们在a.js文件中不小心输入这么一段代码，gulp就会退出工作流。

```javascript
import {} from "module";
```

这不是我们想要的，我们希望gulp能继续工作，这样当我们删掉这段错误代码的时候，gulp可以继续压缩a.js文件，不需要我们重新再命令行输入gulp命令

gulp有很多插件可以处理错误，这里抛砖引玉介绍两种，一种是用来处理js报错的，一种是用来处理sass报错的

#### 5.1 js报错处理

推荐插件：[pump](https://www.npmjs.com/package/pump) 

```
var gulp = require("gulp");
var glob = require("glob");
var fs = require("fs");
var map = require("map-stream");
var uglify = require("gulp-uglify");
var pump = require("pump");
gulp.task("compassJs", function() {
    pump([
        gulp.src("a.js"),
        uglify(),
        gulp.dest("test")
    ])
})
gulp.task("default", function() {
    gulp.watch("a.js", ["compassJs"]);
})
```

#### 5.2 sass报错处理

推荐插件：plumber

我们的任务是监视c.scss文件，当文件变动的时候就时时把它编译为css文件。

c.scss文件内容如下

```scss
html,
body {
    width: 100px;
    height: 100px;
}
```

gulpfile.js代码如下

```javascript
var gulp = require("gulp");
var sass = require("gulp-sass");
var plumber = require("gulp-plumber");
gulp.task("compilecss", function() {
    gulp.src("c.scss")
        .pipe(sass())
        .pipe(gulp.dest("dest"));
})
gulp.task("default", function() {
    gulp.watch("c.scss", ["compilecss"]);
})
```

当我们在c.scss文件中不小心输入一段错误代码，并保存的时候，gulp就会报错，并退出工作流

比如c.scss文件改成如下(多了一个img标签)。gulp就会退出工作流

```
html,
body {
    width: 100px;
    height: 100px;
}
img
```

gulp报错：

```powershell
[10:41:26] Using gulpfile ~\Desktop\博客内容\gulp的使用\gulp-test\gulpfile.js
[10:41:26] Starting 'default'...
[10:41:26] Finished 'default' after 11 ms
[10:41:33] Starting 'compilecss'...
[10:41:33] Finished 'compilecss' after 8.41 ms

events.js:160
      throw er; // Unhandled 'error' event
      ^
 Error: c.scss
Error: Invalid CSS after "img": expected "{", was ""
        on line 7 of c.scss
>> img
   ---^

    at options.error (C:\Users\寄居蟹黄粥\Desktop\博客内容\gulp的使用\gulp-test\node_modules\_node-sass@4.7.2@node-sass\lib\index.js:291:26)
```

我们可以使用plumber来避免这种情况，修改后的gulpfile.js代码如下

```javascript
var gulp = require("gulp");
var sass = require("gulp-sass");
var plumber = require("gulp-plumber");
gulp.task("compilecss", function() {
    gulp.src("c.scss")
        .pipe(plumber({
            errorHandler: function(event) {
                console.log(event.message);
                this.emit('end');
            }
        }))
        .pipe(sass())
        .pipe(gulp.dest("dest"));
})
gulp.task("default", function() {
    gulp.watch("c.scss", ["compilecss"]);
})
```

这样当scss文件出错的时候，gulp只会在命令行打印错误，并不会退出工作流，监视工作还可以继续。

**关于gulp网上还有其他插件和方法，刚兴趣可以自行google**







