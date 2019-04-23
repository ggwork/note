# vue3.0从熟悉到入门

说起来有点丢人，我已经使用vue好久了，但是怎么从0开始新建一个vue项目，每次还是要百度。所以这次我必须自己写个博客了。再记不住就直播自己的女朋友洗澡。

以下以新建一个图书管理项目为例。以下我使用vue3新建项目，对于创建一个项目来说，vue3真的比vue2简单很多。

### 1.初始化项目

#### 1.1全局安装vue-cli

创建vue项目，首先要确保全局安装了vue命令行工具。

我这边使用yarn而不用npm，因为yarn要比npm好用点多。

```powershell
yarn add global @vue/cli
```

#### 1.2 新建项目

使用vue-cli3开发项目，可以使用图形化界面，也可以使用命令行，还可以基于原型进行开发。我这里以常见的基于命令行的开发为例。

我想在我D盘的test文件夹下新建一个图书管理项目，项目名叫book。执行如下命令即可。

```powershell
D:\test>vue create book
```

这里可以选择我们需要安装的预处理器preset。我们可以直接选下图中的第一个选项，这样可以省去很多麻烦。不过这里为了讲解需要，我们选择默认的（bable+eslint）。往后我们再讨论怎么手动安装其他preset。



![预处理器](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vue/createBook.png)



程序执行完后，项目结构如下：

```shell
.
|-book
  |-babel.config.js
  |-package.json
  |-public
  |  |-favicon.ico
  |  |-index.html
  |-README.md
  |-src
  |  |-App.vue
  |  |-assets
  |  |  |-logo.png
  |  |-components
  |  |  |-HelloWorld.vue
  |  |-main.js
  |-yarn.lock
```

将命令行路径设置为book项目所在的路径

```shell
D:\test>cd book
```

启动项目

```js
yarn serve
```

没有报错，就说明安装没有问题。

### 2.项目结构

vue已经给我们新建了一个初始的项目结构，但是这个项目结构还不完善。我们需要新建一下几个目录。新建目录都新建在src目录下。

- views  用户存放我们的页面
- store  放置vuex程序
- api  放置所有的接口程序
- utils 放置工具函数（可有可无）
- router 放置路由信息
- styles 放置全局样式（可有可无）
- components 这个已经有了，用来存放我们页面中的组件。我们可以直接把组件新建在components目录下，不过这样不太清晰，我喜欢在components目录下，再为每个页面新建一个文件夹，把每个页面的组件放在对应的文件下
- assets  这个也已经有了，用来存放我们的资源文件，视频、音频、图片等。

此时的目录结构如下：

```shell
|-book
  |-babel.config.js
  |-package.json
  |-public
  |  |-favicon.ico
  |  |-index.html
  |-README.md
  |-src
  |  |-api
  |  |-App.vue
  |  |-assets
  |  |  |-logo.png
  |  |-components
  |  |  |-HelloWorld.vue
  |  |-main.js
  |  |-router
  |  |-store
  |  |  |-index.js
  |  |-utils
  |  |-views
  |-yarn.lock
```



### 3.项目介绍

我们要讲解vue的使用，总的拿个项目练手。我就做一回产品经理，虚拟一个项目吧。我们有3个页面。分别如下

- 登录页 ，用户输入账号admin和密码admin，就跳转到我们的首页
- 首页，显示各类图书排行榜，用户点击图片，进入图书详情页。
- 图书详情页，显示图书的详细信息

这个项目会涉及到那些操作呢：

1. 点击跳转
2. 请求
3. 展示

因此处理，vue提供给我们的组件外，我们还需要手动添加一下这些组件

- 路由组件：vue-router
- 请求组件：axios mock
- ui组件：element-ui  sass

我们根据我们的需求，一步步开发我们的页面。

### 4. 开发项目

#### 4.1 登录页

1. 在views文件夹下新建Login.vue、Index.vue、Detail.vue 。这三个页面的代码大同小异，目前只是基本的代码，稍后还要修改。

```html
<template>
  <!-- 这是login页面 -->
  <div>这里是login 页面</div>
  <!-- 这是detail页面 -->
  <div>这里是Detail 页面</div>
  <!-- 这是index页面 -->
  <div>这里是index 页面</div>
</template>
<style>
</style>
<script>
export default {
  name: 'Login',
  data() {
    return {

    }
  },
}
</script>

```

2. 修改src目录下的App.vue 文件。删掉#app文件里的内容，改为\<router-view\>，这里就是以为显示其他组件的地方。删除script中的内容。修改后的App.vue代码如下：

```html
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>


<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

### 5.路由组件

使用vue开发认识一个项目都涉及都路由。这里需要安装的组件是vue-router

```shell
D:\test\book>yarn add vue-router
```

在我们刚刚新建的router文件夹下新建index.js文件，程序如下

```js
import Vue from 'vue'
import Router from 'vue-router'
import Login from '../views/Login.vue'
import Index from '../views/Index.vue'
import Detail from '../views/Detail.vue'

Vue.use(Router)
const router = new Router({
  routes: [{
    path: '/',
    redirect: '/index'
  }, {
    path: '/login',
    name: 'Login',
    components: Login
  }, {
    path: 'index',
    name: 'Index',
    components: Index
  }, {
    path: 'detail',
    name: 'Detail',
    components: Detail
  }]
})

export default router
```

修改main.js，引入我们的路由。第3行我们引入了路由，在new Vue的时候我们还需要把router加进去了

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App),
}).$mount('#app')
```

好了现在我们可以访问我们的页面了，忽略链接上的端口号，vue会根据你电脑端口的使用情况，自动选择一个合适的端口号，所以我的端口号可能跟你的不同。

- http://localhost:8081/#/  index页面，这是因为路由里使用redirect重定向
- <http://localhost:8081/#/index>  还是index页面
- <http://localhost:8081/#/login> login页面
- <http://localhost:8081/#/detail> detail页面

ok 自此我们的路由配置成功。

### 6.ui组件

下面就进入我们的页面开发模式，首先我们要开发的是login页。可以使用原生的html开发，但是，效率低下，还是用vue组件吧。这里我element为例

安装

```shell
D:\test\book>yarn add  element-ui
```

配置：在main.js中引入element

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI)

Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App),
}).$mount('#app')
```

修改Login.vue

### 5.ui组件

