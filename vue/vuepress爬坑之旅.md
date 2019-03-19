# 前言

前些天我突然想搞个个人博客，自己搞网站觉得太麻烦，百度来百度去好像hexo很不错。可是不知道为啥，我搞了一个星期都没搞出来，不是这错就是那错。怎么分类，怎么管理标签又是一塌糊涂，文档写起来又很麻烦。然后我就放弃了。没过多久我就了解到还有一个东西叫vuepress。虽然现在马上要9021年了，可是我vue还没用过几次。于是我决定拿这个项目练练手，顺便学习下vue。事实证明这又是一次踩坑，不过结果还好，踩坑很久之后搞出来网站了。

# 1.vuepress是什么

vuepress是基于vue的静态博客系统。何为静态博客？静态博客就是不用跟后台打交道，只有展示功能的博客。也就是不能评论，不能点赞，不能在线讨论等。哎，看到这些就憋屈，没有评论万一有人看上我怎么办？但是要交互，就需要数据库就需要有后台。这些搞起来就没完没了。还需要花大把银子。思来想去，想来思去，最后决定还是不要女朋友了。就搞个静态博客了，末尾我留下我的微信就好了。看上我的妹子可以加我（偷笑.jpg）。

# 2.安装vuepress

安装vuepress，官网推荐是使用yarn。事实证明官方推荐是正确的。我在我的window上使用npm结果报错不断。最后还是改用了yarn。

yarn是什么？这个1年前我就知道了，不过现在还是第一次用。你可以把yarn当成npm。它只是比npm优秀一丢丢。使用的命令又不太一样而已。作用都是一样的，也都是从npm库里下载模块。


安装步骤：

1. 新建一个文件夹，比如名为blog
2. **使用管理员身份**运行命令行工具，进入刚才新建的blog文件夹，执行`yarn init -y`生成package.json
3. 执行`yarn global add vuepress` 全局安装vuepress，再执行`yarn add -D vuepress`再本地安装yarn
4. 命令行执行`mkdir docs`,在blog文件夹下新建docs文件夹。
5. 命令行执行echo '# hello world' > docs/README.md 在docs文件夹下新建一个makedown文件
6. 执行`npx vuepress dev docs` 预览效果。

## 2.1 步骤详解：

第3步解释下，全局安装vuepress是为了方便在命令行调用vuepress命令。在本地也可以不用安装vuepress，在本地安装的目的是为了解耦。当全局vuepress升级到其他版本后，本地还可以使用之前版本的vuepress,避免因为升级而导致项目不可用。全局安装只是为了命令行使用命令，实际用到的代码还是本地的代码。

从第1步到第4步，执行完后，文件夹的目录结果如下：

blog文件下有package.json和docs两个文件(夹)，其实blog文件夹下还有node_modules文件夹，我这里忽略了，你知道有就行，我就不在命令行打印出来了

```shell
.
|-blog
  |-docs
    |-README.md
  |-package.json
```

执行`vuepress dev .`结果如下：

命令行：

![命令行效果](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-dev.png)

浏览器：

![浏览器效果](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-brower.png)

浏览器显示的格式不太对，应该显示成标题1的格式，结果显示成了'# Hello VuePress!'。这个是经常出现的事情。不用紧，稍后我们用自己的makedown工具，编辑下docs文件夹下README.md就可以了。

# 2.2 修改package.json 

在package.json中加入一些命令，简化我们的工作。

```js
"scripts": {
	"docs:dev": "vuepress dev docs",
	"docs:build": "vuepress build docs"
}
```

当我们运行`yarn docs:dev` 就相当于运行`vuepress dev docs`

当我们运行`yarn docs:build` 就相当于运行`vuepress build docs`

此时package.json完整代码如下:

```js
{
  "name": "blog",
  "version": "1.0.0",
  "main": "index.js",
	"scripts": {
		"docs:dev": "vuepress dev docs",
		"docs:build": "vuepress build docs"
	},
  "license": "MIT"
}

```

## 2.3 预览效果

命令行执行 `yarn docs:dev` 预览效果。这时候你可以用你的makedown工具修改docs下README.md的内容，修改完了浏览器会自动刷新，展现最新内容。

## 2.4 打包

命令行执行`yarn docs:build`,vuepress就会把我们的makedown文件打包生成html文件。打包会在docs文件夹下生成.vuepress文件夹，生成的html文件被存放在.vuepress文件夹下的dist文件夹下。

我们可以在.vuepress下新建一个config.js文件，通过修改dest可以把文件生成到不同的目录。我就不修改了。

生成的html文件可以部署到任何静态服务器上，我是上传到了github上。

此时目录结构如图所示:

```js
|-blog
  |-docs
  |  |-.vuepress
  |  |  |-dist
  |  |  |  |-404.html
  |  |  |  |-assets
  |  |  |  |  |-css
  |  |  |  |  |  |-0.styles.f164d2bb.css
  |  |  |  |  |-img
  |  |  |  |  |  |-search.83621669.svg
  |  |  |  |  |-js
  |  |  |  |  |  |-2.aae77c18.js
  |  |  |  |  |  |-3.48b292a8.js
  |  |  |  |  |  |-4.6947e3e9.js
  |  |  |  |  |  |-app.0ffe2193.js
  |  |  |  |-index.html
  |  |-README.md
  |-package.json
```
## 2.5 总结

以上是vuepress的基本用法，相信大多数人都可以执行成功。接下来就是进阶阶段了。

# 3. 配置favorite.icon

这是一个很小的知识点，但是折腾起来也很麻烦。favorite.icon就是这个东东。

![image](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-fav.png)

配置这个的步骤如下:

1.制作icon,尺寸可以是16\*16 或者 32\*32 或者64\*64。具体什么尺寸自己定就可以。可以ps制作一个或者百度下载一个

2.在.vuepress下新建public文件夹，把制作好的icon放进去。

3.在.vuepress下新建config.js配置文件。config.js中可以配置的项非常多，可以点击这里查看[配置](https://vuepress.vuejs.org/zh/guide/basic-config.html)，我这里只讲最基本的。

此时的目录结构如下：

``` shell
|-blog
  |-.gitignore
  |-docs
  |  |-.vuepress
  |  |  |-config.js
  |  |  |-public
  |  |  |  |-favicon.ico
  |  |  |  |-favorite.png
  |  |-README.md
  |-package.json
  
```
4.配置config.js

```js
module.exports = {
	title:"前端蜗牛路",
	description:"郭邯同学的个人博客",
	head:[
		['link',{rel:'icon',href:'/favicon.ico'}]
	]
}
```
注意这里的link的href是直接链接到根目录，我之前写的是`/public/favicon.ico`结果导致一直出不来。

好了，现在重新命令行重新运行下`yarn docs:dev` 就可以看到效果了(有时候刷新下就可以了，有时候不行)。

![添加了favorite](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-fav2.png)

# 4. 配置导航栏

## 4.1 目标是是什么

我想要这样一个导航栏，分别为首页、文章、掘金、GitHub、知乎、女朋友。

- 点击“首页”就回到博客的首页
- 点击“文章”显示下拉列表,下拉列表有两个项“软技能”和“webgl”。这是两类我想分享的博客内容
- 点击“掘金”、“GitHub”、“知乎”就跳到我相应的其他网站主页
- “女朋友”就是我一些无病呻吟的感慨（拿键盘捂脸）

## 4.2 配置config.js

有了以上目标我们配置config.js。我们修改config.js如下：

其中themeConfig下的nav就是我们配置的导航栏。

```js
module.exports = {
	title:"前端蜗牛路",
	description:"郭邯同学的个人博客",
	head:[
		['link',{rel:'icon',href:'/favicon.ico'}]
	],
  themeConfig: {
    nav: [
        { text: '首页', link: '/' },
        { 
          text: '文章', 
          items:[
            { text: '软技能' , link:'/softskill/'},
            { text: 'webgl' , link:'/webgl/'}
          ]
        },
        { text: '掘金', link: 'https://juejin.im/user/5b0f41de518825153a440dd9' },
        { text: 'GitHub', link: 'https://github.com/ggwork'},
        { text: '知乎', link: 'https://www.zhihu.com/people/yagb/activities'},
        { text: '女朋友', link: '/girlfriend/'}
      ],
  }
}
```
配置中text是显示在导航栏上的文字，link是链接，items是二级菜单。items是可以嵌套的，从而可以显示多级菜单。


配置前：

![nav配置前](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-nav-before.png)

配置后：

![nav配置后](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-nav-after.png)

## 4.3 新建目录

在导航栏上我们除了外链外，我们还配置了几个目前还不存在的链接'/softskill/'、'/webgl/'、'/girlfriend/'。这些链接对应我们服务器上的对应的目录。如果我们服务器上没有这些目录，点击这些菜单的话就会报404错误。

所以我们要新建这些目录（文件夹）

在.vuepress同级目录下新建softskil、webgl和girlfriend文件夹。**每个文件夹下都必须存在一个readme.md文件、否则就会报错**。

此时的目录结构如下：

```
|-blog
  |-docs
  |  |-.vuepress
  |  |  |-config.js
  |  |  |-public
  |  |  |  |-favicon.ico
  |  |  |  |-favorite.png
  |  |-girlfriend
  |  |  |-readme.md
  |  |-README.md
  |  |-softskill
  |  |  |-readme.md
  |  |-webgl
  |  |  |-readme.md
  |-package.json
```

回到浏览器，刷新下，点击下导航栏，你就可以发现，导航栏已经可以访问了。比如点击“女朋友”。结果如下:

![导航栏-女朋友](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-girl.png)


自此我们导航栏就配置完成了。关于配置选项的详情，请点击这里查看：[主题配置](https://vuepress.vuejs.org/zh/theme/default-theme-config.html)

# 5. 配置侧边栏

## 5.1 需求说明

下面我们开始配置侧边栏，我们以导航栏文章选项下的“软技能”为例。我需要点击“软技能”之后，左边出现侧边栏，显示我softskill文件夹下我需要分享的文章，这样方便网友通过点击侧边栏查看不同的文章。

## 5.2 配置config.js

配置侧边栏还需要配置config.js，配置如下：

```shell
module.exports = {
  title: '前端蜗牛路',
  description: '郭邯同学的个人博客',
  head: [['link', { rel: 'icon', href: '/favicon.ico' }]],
  themeConfig: {
    nav: [
      { text: '首页', link: '/' },
      {
        text: '文章',
        items: [
          { text: '软技能', link: '/softskill/' },
          { text: 'webgl', link: '/webgl/' }
        ]
      },
      { text: '掘金', link: 'https://juejin.im/user/5b0f41de518825153a440dd9' },
      { text: 'GitHub', link: 'https://github.com/ggwork' },
      { text: '知乎', link: 'https://www.zhihu.com/people/yagb/activities' },
      { text: '女朋友', link: '/girlfriend/' }
    ],
    sidebar: {
      '/softskill/': [
        '',
        '软技能-代码之外的生存指南1（职业篇）',
        '软技能-代码之外的生存指南2（自我营销篇）',
        '软技能-代码之外的生存指南3（自我学习）',
        '软技能-代码之外的生存指南4（生产力）',
        '软技能-代码之外的生存指南5（理财）',
        '软技能-代码之外的生存指南6（健身）',
        '软技能-代码之外的生存指南7（精神）'
      ],
      '/webgl/': ['']
    }
  }
}


```
sidebar就是侧边栏，softskill就是我们的软技能。softskill下的''的意思是，点击“软技能”显示其下的readme.md文件。其他的都是对应的makedown文件。

## 5.3 新建makedown文件

此时目录结构如下：

```
.
|-blog
  |-docs
  |  |-.vuepress
  |  |  |-config.js
  |  |  |-public
  |  |  |  |-favicon.ico
  |  |  |  |-favorite.png
  |  |-girlfriend
  |  |  |-readme.md
  |  |-README.md
  |  |-softskill
  |  |  |-readme.md
  |  |  |-软技能-代码之外的生存指南1（职业篇）.md
  |  |  |-软技能-代码之外的生存指南2（自我营销篇）.md
  |  |  |-软技能-代码之外的生存指南3（自我学习）.md
  |  |  |-软技能-代码之外的生存指南4（生产力）.md
  |  |  |-软技能-代码之外的生存指南5（理财）.md
  |  |  |-软技能-代码之外的生存指南6（健身）.md
  |  |  |-软技能-代码之外的生存指南7（精神）.md
  |  |-webgl
  |  |  |-readme.md
```

刷新下浏览器，已经成功了。
![侧边栏软技能](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vuepress/vue-nav-soft.png)

**这里有一点需要注意：在config.js中配置的侧边栏的文字跟浏览器侧边栏显示的文字可能是不一样的。在config中配置的文字是对应的makedown文章的文件名。在浏览器侧边栏显示的文字是makedown文章内容里第一个格式为标题1的文字**

# 6.静态资源

我们写makedown中，有很多时候都需要引入静态资源，比如图片和一些js。vuepress提供了对应的引入静态资源的方法。详情请点击这里，[静态资源](https://vuepress.vuejs.org/zh/guide/assets.html/)。

不过，我个人不喜欢这样，因为有时候我的博客经常需要复制粘贴到其他网站。这时候如果用静态资源就就比较麻烦了，每次都要上传排版。这里我推荐腾讯云的对象存储，为什么推荐它呢？因为它是免费的，而且有50G的空间，足够我用好多年了。

详细用法请点击这里。[腾讯对象存储](https://cloud.tencent.com/document/product/436/6225)

# 7. 部署

对于部署，官网提供了一个.sh文件，结果悲催啊，windows上不能运行.sh文件。反正按照官网给的教程，搞了很久都没成功。目前据我百度的结果vuepress没有什么自动部署的方法，最后我还是老老实实的每次打包后，手动使用git把dist文件夹上传到自己的github page上。

对于如何创建github page你可以百度，这个很简单，我就不分享了。如果你有好的部署方法请告诉我。

官网推荐的部署方法地址在这里：[部署](https://vuepress.vuejs.org/zh/guide/deploy.html)

# 8.其他高级用法

vuepress简单但是也蛮强大的，通过以上的步骤我们基本上可以建一个看起来丑丑的，但是却很实用的网站。vuepress是可以美化的，也有很多高级的用法，比如makedown语法的扩展简直牛逼了，还可以自己创建组件。这些我就不介绍了，我有个丑丑的网站就知足了。更详细的用法请点击这里：

[vuepress文档](https://vuepress.vuejs.org/zh/)

另外推荐下我自己的网站：[前端蜗牛路](https://ggwork.github.io/)

vuepress的demo我已经上传到github了，可以查看这里。[vuepress的demo](https://github.com/ggwork/vuepress-demo)

再啰嗦下，显示的目录结构你们有没有觉得很帅，这个工具是我自己开发的,[ctree-cli](https://www.npmjs.com/package/ctree-cli)，欢迎下载使用。