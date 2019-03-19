## 前言

其实我已经用过vuex一段时间了，最近又有一段时间没有用，结果今天又生疏了。为此，我决定再入门下。

首先vuex是啥？vuex是vue的一个状态管理器。何为状态管理器？状态管理器可以理解为一个小型的前端数据库。用来实现各个页面之间的数据共享。

那为啥不用sessionStorge和localStorage共享数据呢。因为这里涉及到状态更新。当你在a页面更新了数据后，使用vuex的话，b页面数据就会自动更新。而使用sessionStorage或者localStorage就没有这种效果。

## 1.使用vuex

### 1.1 安装vuex

vuex跟vue是分开的，使用vuex还需要安装，可以使用npm也可以使用yarn。代码如下：

```powershell
npm install vuex -S
或者
yarn add vuex
```

### 1.2 引用vuex

经过上面一个步骤已经安装了vuex,现在就是我们使用vuex的时候。使用很简单，vue的src目录下新建store文件夹，在store文件夹下新建index.js文件。其实在其他地方建也可以，不过这不太符合规范，如果有人改你的代码的时候就可以找不到你的store放哪里。目录结构如下：

```shell
|-src
  |- App.vue
  |- assets
  |  |-logo.png
  |-components
  |  |-HelloWorld.vue
  |-main.js
  |-store
  |  |-index.js

```

### 1.3 引入vuex

在我们新建的index.js中引入vue和vuex，并且使用vue调用vuex组件，这样我们就可以在index中写vuex的程序了。

```shell

import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex)

```
写一个简单vuex程序。

```javascript

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }})
  
export default store

```
在这个程序中，我们在store中定义了一个字段count，初始值为0。我们还定义了一个方法increment，每次调用increament，count值都会加1.

### 1.4 使用vuex中的数据

#### 1.4.1 注入store

为了方便我们使用vuex中的数据，我们将store加入vue中，这样以后无论在那里都可以通过this.$store访问vuex了。

修改main.js，在new Vue的构造函数中加入store

```javascript

import Vue from 'vue'
import App from './App.vue'

// 引入store
import store from './store'

Vue.config.productionTip = false

new Vue({
  store,
  render: h => h(App),
}).$mount('#app')

```

#### 1,4,2 使用stroe

假设我们要在组件helloworld中使用store（vuex）中的数据。就可以使用`this.$store.state.字段名`来获取store中存放的数据。比如我们在created中获取获取store中的count

```html

<template>
  <div class="contain">
    <div class="title">name:{{name}}</div>
    <div class="count">count:{{count}}</div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      name: 'hello world',
      count: this.$store.state.count
    }
  }
}
</script>

<style scoped>
</style>


```

如果要是count加1，就需要调用`this.$store.commit('increament')`方法（之前在stroe/index.js中定义过这个方法）来是count加1


### 1.5 插播一个广告
使用vue-cli3开发的时候，有时候因为eslint的限制很严格，所以导致编译不通过。可以修改package.json中eslint的rules。

比如在我项目中，eslint限制我不能写console，这导致我开发的时候很不方便。我在package.json中把no-console改为0，就去掉了这种限制

```javascript

"eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "rules": {
      "no-console": 0
    },
    "parserOptions": {
      "parser": "babel-eslint"
    }
  }

```

以上就是vuex的简单入门，以下我们将更加深入的讲解下vuex。

## 2.vuex的组成

vuex其实很简单，vuex只由4部分组成，分别为：

- state 用来存储数据的
- mutation 以同步的方式来修改数据
- getters 用来对数据进行过滤和计算，返回计算后的数据
- action 以异步的方式来修改数据

一个简单的vuex如下:


```html

<template>
<div class="contain">
<div class="title">name:{{name}}</div>
<div class="count">count:{{count}}</div>
</div>
</template>
<script>
export default {
name: 'HelloWorld',
props: {
msg: String
},
data() {
return {
name: 'hello world',
count: this.$store.state.count
}
}
}
</script>
<style scoped>
</style>


```