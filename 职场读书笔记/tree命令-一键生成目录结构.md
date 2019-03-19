# 1.tree是什么

浏览博客的时候，我相信很多人都跟我一样，看到过很多文章里都有很漂亮的目录结构。比如这样子。
```
.
└── test
    ├── css
    ├── img
    │   └── head
    └── js

```
看到这样的目录结果，打心底里觉得舒服——简洁、优雅，在这个世界上只有格子衫能匹配这样的美了。

可是等到自己写博客想搞目录结构的时候就捉急了。这是怎么生成的呢？真是百思不得其姐。百度也不知道如何下手。当年为了在博客上搞个目录结构，我是硬生生用键盘敲上去的。后来也是偶然的机会才知道有tree这个工具。

今天就讲解下tree命令。

# 2. tree的使用方法

tree [OPTIONS] [directory] 

**tree 常见的命令如下：**

- -a 显示所有文件和目录。
- -A 使用ASNI绘图字符显示树状图而非以ASCII字符组合。
- -C 在文件和目录清单加上色彩，便于区分各种类型。
- -d 显示目录名称而非内容。
- -D 列出文件或目录的更改时间。
- -f 在每个文件或目录之前，显示完整的相对路径名称。
- -F 根据ls -F，为目录添加一个'/'，为套接字文件添加一个'='，为可执行文件添加一个' *'，为FIFO添加一个' |'
- -g 列出文件或目录的所属群组名称，没有对应的名称时，则显示群组识别码。
- -i 不以阶梯状列出文件或目录名称。
- -I 不显示符合范本样式的文件或目录名称。
- -l 跟随目录的符号链接，就像它们是目录一样。避免了导致递归循环的链接
- -n 不在文件和目录清单加上色彩。
- -N 按原样打印不可打印的字符。
- -p 列出权限标示。
- -P 只显示符合范本样式的文件或目录名称。
- -q 将文件名中的不可打印字符作为问号打印。
- -s 列出文件或目录大小。
- -t 用文件和目录的更改时间排序。
- -u 列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码。
- -x 将范围局限在现行的文件系统中，若指定目录下的某些子目录，其存放于另一个文件系统上，则将该子目录予以排除在寻找范围外。

# 3 tree命令的详细讲解

我有一个test文件夹，文件夹下有如下文件。（以下目录结构为手敲，本来想截图，可惜我的makedown不支持图片）

- css
    - main.css
    - jquery-ui.css
- img
    - head
        - b.png
    - a.png
- js
    index.js
- .bablelrc
- hide.txt
- index.html
- 说明.txt


我们以这个目录为例子，来说明tree的用法。有几点需要特殊说明。

- bable的配置文件babelrc是没有文件名的
- 说明.txt的文件名是中文
- hide.txt文件属性是隐藏

以下命令都是在test文件夹下打开命令行，然后执行命令的。

## 3.1 直接执行tree命令

直接在命令行(terminal)上执行tree命令，

```shell
tree
```
显示结果如下,显示了所有的文件和目录，**除了没有文件名的文件（bebel配置文件）**：
```
.
└── test
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt

```
## 3.2 tree a 

显示所有的文件和目录，**包裹不含文件名的babel文件**

```shell
tree a
```

```
.
└── test
    ├── .babelrc
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt

```
## 3.3 tree -A (注意这里是大A)

使用ASNI绘图字符显示树状图而非以ASCII字符组合。**不包含没有文件名的文件（这点很奇怪）**

```
.
└── test
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt


```
*补充一点：ANSI编码用0x00~0x7f范围的1个字节来表示1个英文字符，超出一个字节的0x80~0xFFFF范围来表示其他语言的其他字符。也就是说，ANSI码仅在前126个与ASCII码相同，之后的字符全是某个国家语言的所有字符。*

## 3.4 tree -C

```
.
└── test
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt

```
*补充：如果未设置LS_COLORS环境变量，则使用内置颜色默认值始终打开颜色。用于将输出着色到管道。*

## 3.5 tree -d 

显示目录名称而非内容

```shell
tree -d
```

```
.
└── test
    ├── css
    ├── img
    │   └── head
    └── js



```
## 3.6 tree -f

在每个文件或目录之前，显示完整的相对路径名称

```shell
tree -f
```

```
.
└── ./test
    ├── ./test/css
    │   ├── ./test/css/jquery-ui.css
    │   └── ./test/css/main.css
    ├── ./test/img
    │   ├── ./test/img/a.png
    │   └── ./test/img/head
    │       └── ./test/img/head/b.png
    ├── ./test/index.html
    └── ./test/js
        └── ./test/js/index.js
        
```

## 3.7 tree -F

根据ls -F，为目录添加一个' /'，为套接字文件添加一个' ='，为可执行文件添加一个' *'，为FIFO添加一个' |'

```
.
└── test/
    ├── css/
    │   ├── jquery-ui.css*
    │   └── main.css*
    ├── hide.txt*
    ├── img/
    │   ├── a.png*
    │   └── head/
    │       └── b.png*
    ├── index.html*
    ├── js/
    │   └── index.js*
    └── 说明.txt*

```

## 3.8 tree -g

列出文件或目录的所属群组名称(其实就是文件所属用户组名称)，没有对应的名称时，则显示群组识别码。

```
.
└── [root    ]  test
    ├── [root    ]  css
    │   ├── [root    ]  jquery-ui.css
    │   └── [root    ]  main.css
    ├── [root    ]  hide.txt
    ├── [root    ]  img
    │   ├── [root    ]  a.png
    │   └── [root    ]  head
    │       └── [root    ]  b.png
    ├── [root    ]  index.html
    ├── [root    ]  js
    │   └── [root    ]  index.js
    └── [root    ]  说明.txt

```
## 3.9 tree -i

不以阶梯状列出文件或目录名称

```
.
test
css
jquery-ui.css
main.css
hide.txt
img
a.png
head
b.png
index.html
js
index.js
说明.txt

```

## 3.10 tree -I pattern 

其中pattern为通配符，不显示符合的文件或目录名称。

比如我们不显示css文件

``` shell
tree -I *.css
```
```
.
└── test
    ├── css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt
```

## 3.11 tree -l

跟随目录的符号链接，就像它们是目录一样。 避免了导致递归循环的链接。(不知道这个是干嘛的，测试也没效果。如果有人知道请告诉我)

## 3.12 tree -n

不在文件和目录清单加上色彩。(默认控制台打印出来的目录结构中，文件夹是高亮的，使用-n后，文件夹就不高亮了)

## 3.13 tree -N

按原样打印不可打印的字符。（看起来也没啥效果，应该是我的测试案例太正常了）

```
.
└── test
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt
```

## 3.14 tree -p

列出权限标示

```
.
└── [drwxrwxrwx]  test
    ├── [drwxrwxrwx]  css
    │   ├── [-rwxrwxrwx]  jquery-ui.css
    │   └── [-rwxrwxrwx]  main.css
    ├── [-rwxrwxrwx]  hide.txt
    ├── [drwxrwxrwx]  img
    │   ├── [-rwxrwxrwx]  a.png
    │   └── [drwxrwxrwx]  head
    │       └── [-rwxrwxrwx]  b.png
    ├── [-rwxrwxrwx]  index.html
    ├── [drwxrwxrwx]  js
    │   └── [-rwxrwxrwx]  index.js
    └── [-rwxrwxrwx]  说明.txt

```
## 3.15 tree -P(大写) pattern

仅列出与通配符模式匹配的文件。

比如仅显示css文件

```shell
tree -P *.css
```
```
.
└── test
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── img
    │   └── head
    └── js

```

**注意：您必须使用-a选项来考虑以点“.”开头的那些文件。有效的通配符运算符是*匹配任何零个或多个字符,?匹配任何单个字符，[...]匹配括号内列出的任何单个字符，[^...]匹配非[]中的任意字符，(|)匹配|前的和后的表达式。**

## 3.16 tree -P 配合 -a 使用

**这样我们就可以打印出来所有的问题，包括隐藏的和没有文件名的文件**

```shell
tree -P *.* -a
```
```
.
└── test
    ├── .babelrc
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt
```

## 3.17 tree -q

将文件名中的不可打印字符作为问号打印。

因为我的文件名太正常了，所以测试不出来效果

```
.
└── test
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── hide.txt
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── index.html
    ├── js
    │   └── index.js
    └── 说明.txt
```

## 3.18 tree -s

列出文件或目录大小。

```
.
└── [       4096]  test
    ├── [       4096]  css
    │   ├── [      22175]  jquery-ui.css
    │   └── [       2874]  main.css
    ├── [          0]  hide.txt
    ├── [          0]  img
    │   ├── [     131037]  a.png
    │   └── [          0]  head
    │       └── [     131037]  b.png
    ├── [       4725]  index.html
    ├── [          0]  js
    │   └── [       3942]  index.js
    └── [          0]  说明.txt

```
## 3.19 tree -t

按照文件和目录的更改时间排序。**注意文件打印的顺序不一样了**

```
.
└── test
    ├── index.html
    ├── js
    │   └── index.js
    ├── css
    │   ├── jquery-ui.css
    │   └── main.css
    ├── img
    │   ├── a.png
    │   └── head
    │       └── b.png
    ├── 说明.txt
    └── hide.txt

```

## 3.20 tree -u

列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码。

```
.
└── [root    ]  test
    ├── [root    ]  css
    │   ├── [root    ]  jquery-ui.css
    │   └── [root    ]  main.css
    ├── [root    ]  hide.txt
    ├── [root    ]  img
    │   ├── [root    ]  a.png
    │   └── [root    ]  head
    │       └── [root    ]  b.png
    ├── [root    ]  index.html
    ├── [root    ]  js
    │   └── [root    ]  index.js
    └── [root    ]  说明.txt


```

## 3.21 tree -x

将范围局限在现行的文件系统中，若指定目录下的某些子目录，其存放于另一个文件系统上，则将该子目录予以排除在寻找范围外

因为我的目录太正常了，这个没测试出来。

# 4 安装tree

看到tree这么强大，是不是很心动？不过tree只在linux和mac下才有这样强大的效果。linux和mac下tree工具不是自带的，需要安装。

window是自带tree的，不过实现的简直不能再垃圾了，可以用脖子以下都截肢来形容。为此，我开发了一个npm包，来实现tree命令。

可以点击这里进行安装[ctree-cli](https://www.npmjs.com/package/ctree-cli)。如果有问题，欢迎提交issuses
