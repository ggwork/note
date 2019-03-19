# 前言

说来惭愧，yarn已经出来很久了，没想到我现在才打算入门。能看到这篇文章的人，大概跟我都一样，知道yarn很久了但是从来没有用过。大概心里一直有个声音在呼喊，学yarn有毛用，npm扫天下。曾经我也一直这么认为，现在也时时这么想。但是我最终还是决定学下下。因为学这个花不了多少时间，那些用来纠结的时间，足够入门了。而且确实很多项目都用用yarn的而不是npm。总之学习下不吃亏。

顺便吐槽下现在的前端，现在前端基本稳定下来，不会像以前发展那么快。但是还是有个问题很让人纠结，就是要经常选边站。是npm还是yarn、是学习vue还是学习react、是用sass还是用less，是用webpack还是rollup。当然大神都是全学的。像我这等渣渣就经常要考虑这些问题。

# 1.yarn与npm使用的区别

我虽然用npm用了很多年，但是我对npm不是很熟，这么多年来，我常用的npm命令也就4、5个而已。yarn也是我新学，让我深刻比较yarn和npm的区别，我也暂时做不到。我只能提供一下自己粗浅的认识。

## 1.1 yarn比npm更快
    
yarn的介绍中说，yarn会缓存每一个它下载的包，这样当下次请求相同的包的时候就不要再次下载了。而且yarn会并行下载依赖的包。所以yarn比npm更快，
    
npm确实存在这些问题。一个项目依赖经常几百m,把项目挪一个位置重新install下，都要重新下载一遍npm包，而且下载速度真是出奇的慢。当然也跟我们社会主义制度有点问题，所以也才有了cnpm的诞生。其实cnpm也不是什么好东西，这东西下载包真是经常报错，尤其是跟npm混用的时候。还是能不用就别用cnpm。

*测试*

以下是本人的一些测试。本电脑npm版本6.2.0  yarn版本1.12.3

文件夹t3下有package.json和yarn.lock文件：

```
.
|-t3
  |-package.json
  |-yarn.lock
  
```
文件夹t4下有package.json和package-lock.json文件：

```
.
|-t4
  |-package-lock.json
  |-package.json
```

package.json代码如下

```
{
  "name": "t1",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-env": "^1.7.0",
    "loadsh": "^0.0.3"
  }
}

```

要安装的dependencies，我电脑上之前安装过了，现在测试下使用yarn和npm的快慢

*使用yarn 10.33s，占用空间15.5m*

```
yarn install v1.12.3
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@1.2.7: The platform "win32" is incompatible with this module.
info "fsevents@1.2.7" is an optional dependency and failed compatibility check.
Excluding it from installation.
[3/4] Linking dependencies...
[4/4] Building fresh packages...
Done in 10.33s.
```

*使用npm 14.71s，占用空间16.0m*

```
npm WARN t1@1.0.0 No description
npm WARN t1@1.0.0 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.7 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.7: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

added 290 packages from 127 contributors and audited 3297 packages in 14.71s
found 1 low severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
```

**结论：对于已经安装过的包，yarn确实比npm快，而且节省空间。对于从来没有安装过的包，yarn会节省空间，但是会比npm安装速度慢一些（yarn要40多s，而npm时间10多s，我就不贴数据了）。**

## 1.2 yarn比npm更可靠

这个问题其实已经不存在，在npm5.0之前，因为没有package-lock.json文件，安装都是依赖package.json文件的。而package.json中对于模块的版本配置都是不确定的。比如如下：

```
{
  "name": "t1",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-env": "^1.7.0",
    "loadsh": "^0.0.3"
  }
}

```
以babel-cli为例。^6.26.0 代表版本范围 >=6.26.0 && < 7.0.0  这样当我同事根据我的package.json安装包的时候 此时babel-cli更新到了6.26.1。这样我跟同事安装的babel-cli就不同了。这就会导致很多问题。

## 1.3 yarn比npm更安全

yarn比npm检查更加严格，所以也更加安全。


# 2.yarn和npm的命令

这章我倒很想自己总结，本来我心里有个提纲的，但是刚刚我不忍心，搜索了下其他帖子，发现有个帖子总结的真好。大概没有更好的方式了，大丈夫不拘小节，我就抄一抄了，顺带根据自己的思路改一改。

原贴地址如下[：Npm vs Yarn 之备忘详单](https://www.jeffjade.com/2017/12/30/135-npm-vs-yarn-detial-memo/)

## 2.1有区别的命令 





Npm | Yarn | 功能描述
---|---|---
npm install(npm i)	| yarn install(yarn)	| 根据 package.json 安装所有依赖
npm i –save [package] |	yarn add [package]	| 添加依赖包
npm i –save-dev [package] |	yarn add [package] –dev |	添加依赖包至 devDependencies
npm i -g [package]	| yarn global add [package] |	进行全局安装依赖包
npm update –save |yarn upgrade [package]	| 升级依赖包
npm uninstall [package] |yarn remove [package]  |移除依赖包

## 2.2 相同操作的命令

Npm	| Yarn	| 功能描述
---|---|---
npm run	| yarn run |运行 package.json 中预定义的脚本
npm config list | yarn config list |	查看配置信息
npm config set registry 仓库地址 |	yarn config set registry 仓库地址 |	更换仓库地址
npm init |	yarn init |互动式创建/更新 package.json 文件
npm list |	yarn list |	查看当前目录下已安装的node包
npm login |	yarn login |	保存你的用户名、邮箱
npm logout |	yarn logout |	删除你的用户名、邮箱
npm outdated |	yarn outdated |	检查过时的依赖包
npm link |	yarn link |	开发时链接依赖包，以便在其他项目中使用
npm unlink |	yarn unlink |	取消链接依赖包
npm publish |	yarn publish |	将包发布到 npm
npm test |	yarn test |	测试 = yarn run test
npm bin	 | yarn bin | 显示 bin 文件所在的安装目录
yarn info |	yarn info |	显示一个包的信息

## 2.3 yarn或者是npm安装或者升级包的时候，都可以指定对应的版本信息

npm:

```shell
npm install [package]@[version]
npm install [package]@[tag]
```
yarn:

```shell
yarn add [package]@[version]
yarn add [package]@[tag]
```

## 2.4 yarn和npm独有的命令

npm:
- `npm rebuild pacakgename`: 用于更改包内容后进行重建；比如常见的`npm rebuild node-sass`；当使用Sass（Scss）来作样式表预处理器，再打包的时候，你可能会遇见如下错误；而解决此问题，最为简单的方式即使用 rebuild 命令，对 node-sass 进行重建即可

```shell
Module build failed: ModuleBuildError: Module build failed: Error: Node Sass does not yet support your current environment: 
This usually happens because your environment has changed since running npm install. Run npm rebuild node-sass to build the binding for your current environment.
```
yarn:

- `yarn import`：依据原npm安装后的node_modules目录生成一份yarn.lock文件；
- `yarn licenses`：列出已安装包的许可证信息
- `yarn pack`：创建一个压缩的包依赖 gzip 档案
- `yarn why`：显示有关一个包为何被安装的信息
- `yarn autoclean`：从包依赖里清除并移除不需要的文件


*总结：yarn的命令跟npm的命令大同小异，对于使用过npm的人来说，从npm过渡到yarn只是半个小时以内的事情。所以学习成本很低，值得学习一下。*

# 3. yarn的安装和使用

## 3.1 安装yarn，[yarn下载地址](https://yarnpkg.com/zh-Hans/docs/install#windows-stable)

## 3.2 设置yarn淘宝镜像

```shell
yarn config set registry http://registry.npm.taobao.org
```

设置完yarn的淘宝镜像以后下载包就会快很多了。

自此使用yarn应该就没啥问题了，贴下yarn的官网，[官网](https://yarnpkg.com/zh-Hans/)。欢迎大家入门yarn。以后要是我再想到什么要点，我再补充。

