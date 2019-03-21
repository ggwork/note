## 前言

其实我已经用过vuex一段时间了，最近又有一段时间没有用，结果今天又生疏了。为此，我决定再入门下。

首先vuex是啥？vuex是vue的一个状态管理器。何为状态管理器？状态管理器可以理解为一个小型的前端数据库。用来实现各个页面之间的数据共享。

那为啥不用sessionStorge和localStorage共享数据呢。因为这里涉及到状态更新。当你在a页面更新了数据后，使用vuex的话，b页面数据就会自动更新。而使用sessionStorage或者localStorage就没有这种效果。

## 1.使用vuex

### 1.1 安装vuex

vuex跟vue是分开的，使用vuex还需要安装，可以使用npm也可以使用yarn。代码如下：

```shell
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

假设我们要在组件helloworld中使用store（vuex）中的数据。就可以使用`this.$store.state.字段名`来获取store中存放的数据。使用store中的数据也是有要求的——“要放vue的computed属性中”，你放在其他地方比如created也行，或者直接赋值给data对象对应的属性。但是这样谁有个问题，就是当store中的数据更新的时候，页面数据不能自动更新。

```html

<template>
  <div class="contain">
    <div class="title">{{name}}</div>
    <p>
      动物园里单身狗的数量为:
      <span>{{count}}</span>只
    </p>
    <button @click="addCount">拆散一对情侣</button>
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
      name: 'hello world'
    }
  },
  computed: {
    count() {
      return this.$store.state.count
    }
  },
  methods: {
    addCount() {
      this.$store.commit('increment')
      this.$store.commit('increment')
      console.log('this.$store.state.count:', this.$store.state.count);
    }
  },
}
</script>

<style scoped>
.title {
  font-size: 28px;
  text-align: center;
  margin: 10px;
}
</style>


```



上面的程序初始的时候，页面显示“动物园里单身狗的数量为:0”。当我们点击“拆散一对情侣”按钮时，“动物园里单身狗的数量为:”就变成了2.

如果我们把count放在data对象中，比如把js改成下面这样子。点击“拆散一对情侣”按钮时，store中的count值变了，但是页面上“动物园里单身狗的数量”不会变。

```javascript
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
  },
  methods: {
    addCount() {
      this.$store.commit('increment')
      this.$store.commit('increment')
      console.log('this.$store.state.count:', this.$store.state.count);
    }
  },
}
```




### 1.5 插播一个广告
使用vue-cli3开发的时候，有时候因为eslint的限制很严格，所以导致编译不通过。可以修改package.json中eslint的rules。

比如在我项目中，eslint限制我不能写console，这导致我开发的时候很不方便。我在package.json中把no-console改为0，就去掉了这种限制

```json

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

vuex其实很简单，我以前学习vuex的时候，感觉好高深，现在看来还是蛮简单的。vuex只由4部分组成，这四部分分别负责不同的功能，就相当于游戏中的4个不同的角色吧。4个分别为：

- state 用来存储数据的
- mutation 以同步的方式来修改数据
- getters 用来对数据进行过滤和计算，返回计算后的数据
- action 以异步的方式来修改数据

vuex是单一状态树的，就是在整个vue程序中只有一个vuex实例（store）。单一状态树可以让我们很容易的定位代码的执行情况。

### 2.1 state

我们一般获取store对象中的数据都是通过`this.$store.state.key`获取的。比如我们获取count属性，我们是这样获取的。

```js
this.$store.state.count
```

#### 2.1.1 mapState函数

我们获取state上的值，一般都是放在computed属性中的，如果state中有很多值，那么我们就要在computed中写很多计算属性。为了介绍代码量，vuex提供了一个mapState函数。

我们可以把程序改成如下这样子。注意这里引入了mapState，在computed中我们也使用了mapState

```javascript
import { mapState } from 'vuex'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      name: 'hello world'
    }
  },
  computed: mapState([
    'count'
  ]),
  methods: {
    addCount() {
      this.$store.commit('increment')
      this.$store.commit('increment')
      console.log('this.$store.state.count:', this.$store.state.count);
    }
  },
}
```

实现的功能跟之前是一样的。

```javascript
computed: mapState([
   'count'
])
// 相当于我们之前的
computed: {
  count() {
      return this.$store.state.count
  }
}
```

使用mapState更简便的方法是使用对象展开符....  

```javascript
  computed: {
    ...mapState([
      'count'
    ])
  },
```

如果想重命名，可以把mapState的参数改成对象格式

```javascript
  computed: {
    ...mapState({
      count2: 'count'
    })
  },
```



### 2.2 Getters

我们有时候需要store中state的值进行计算，如果一个组件或者两个组件的话，我们可以直接把属性放在computed中。但是如果多个组件都需要进行相同的计算的话。我们代码就会很冗余。解决办法就是在store就对state的值进行计算。因此就要用到store的getters属性。

在我们的案例中count代表单身狗的数量，初始值为0，这个世界是没有单生狗的。但是某天忽然我们要在vue里增加一个程序员组件（It.vue）。程序员这个群体，大家都懂的，单生狗特别多，假设比平均值count多10个，那么我们可以在store中定义一个itDogs的属性，这个属性比count值多10。

修改store代码如下：

```javascript
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex)
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++
    }
  },
  getters: {
    itDogs: state => {
      return state.count + 10
    }
  }
})

export default store
```



那么我们该怎么获取呢？

获取getters中数据可以通过属性访问，也可以通过方法访问

#### 2.2.1 通过属性访问

通过属性访问代码如下：this.$store.getters.健名，在这个需求中就是：

```javascript
this.$store.getters.itDogs
```

It.vue 中获取itDogs代码如下：

```javascript
computed: {
    itDogs: function () {
      return this.$store.getters.itDogs
    }
 }
```

#### 2.2.2 通过方法访问

要想通过方法访问，那么getters中的属性，在定义的时候必须返回一个函数，而不是直接返回值

假设我们的需求改变了，在程序员的世界里，使用不同语言的单身狗的数量是不一样的。比如使用js的就是在平均单身狗的数量上加10，使用java的就是平均的基础上加100

那么可以把getters里写itDogs写成一个函数，根据不同的语言传入不同的值，然后返回对应的结果。（当然也可以用其他方法）

store修改如下：

```javascript
getters: {
    itDogs: state => (num) => {
        return state.count + num
    }
}
```

获取方法如下：

```javascript
  computed: {
    itDogs: function () {
      return this.$store.getters.itDogs(100)
    }
  },
```



***通过属性方法访问getters中的值，这个值会被缓存起来。下次需要时可以直接从缓存中拿。通过方法访问的话，每次都要调用函数重新计算*

#### 2.2.3 访问其他getter

getter也可以接受其他getter作为第二个参数。比如男程序员单身狗的数量在一般的程序员单身狗的数量上又加100.

```javascript
  getters: {
    itDogs: state => (num) => {
      return state.count
    },
    manItDogs: (state, getters) => {
      return getters.itDogs + 100
    }
  }
```

#### 2.2.4  mapGetters函数

mapGetters函数将store中getter映射到局部计算属性，比如getter是如下：

```javascript
  getters: {
    itDogs: state => {
      return state.count
    },
    manItDogs: (state, getters) => {
      return getters.itDogs + 100
    }
  }
```

获取的代码我们修改如下:

```javascript
  computed: {
    ...mapGetters([
      'itDogs',
      'manItDogs'
    ])
  },
```

如果要重命名，可以把mapGetters的参数改成对象

```javascript
  computed: {
    ...mapGetters({
      itDogs2: 'itDogs',
      manItDogs2: 'manItDogs'
    })
  },
```

### 2.3 Mutation

更改 Vuex 的 store 中的数据的唯一方法是提交 mutation。需要现在store中注册事件，后面才能调用。mutation中定义的函数必须是同步的。

比如我们之前定义的

```javascript
  mutations: {
    increment(state) {
      state.count++
    }
  },
```

我们想要count的数量加1，我们就调用

```javascript
this.$store.commit('increment')
```

#### 2.3.1 传参

我们之前定义了一个按钮叫“拆散一对情侣”。点击这个按钮我们就把count的数量加2，我们之前是这样调用的。

```javascript
  methods: {
    addCount() {
      this.$store.commit('increment')
      this.$store.commit('increment')
    }
  },
```

我们调用了两次commit，这显得很奇怪，其实mutations中的函数是可以传参的。我们修改increment函数。

```javascript
  mutations: {
    increment(state, num) {
      state.count += num
    }
  },
```

state.count不再是加1了，而是加上num。这样好了，无论是3p的还是百人斩的，我们都可以拆散了。

修改调用函数如下：

```javascript
  methods: {
    addCount() {
      this.$store.commit('increment', 2)
    }
  },
```

#### 2.3.2 对象参数

有时候我们参数很多怎么办？我们可以把第二个参数改成对象。

修改increment

```javascript
  mutations: {
    increment(state, obj) {
      state.count += obj.num
    }
  },
```

修改调用

```javascript
  methods: {
    addCount() {
      this.$store.commit('increment', {
        num: 2
      })
    }
  },
```

#### 2.3.3 `mapMutations` 

mapMutations可以将store中mutation中的方法映射为vue组件中methods的方法

```javascript
  methods: {
    ...mapMutations([
      'increment'
    ])
  },
```

这样我们调用组件的increment方法，就相当于调用stroe中mutation的increment方法

调用如下

```html
<button @click="increment({num:2})">拆散一对情侣</button>
```

### 2.4 Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作

一个简单的action如下

```javascript
 actions: {
    increment: (context, obj) => {
      setTimeout(() => {
        context.commit('increment', obj)
      }, 5000);

    }
  }
```

action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters

如何调用action呢？我们可以使用dispatch来调用

```javascript
this.$store.dispatch('increment')
```

完整代码：

```javascript
  methods: {
    increment: function () {
      this.$store.dispatch('increment', { num: 2 })
    }
  },
```

actions函数跟mutation函数一样都是可以传参的。

#### 2.4.1 `mapActions` 

`mapActions`将action中的方法，映射到vue实例methods方法上。

```javascript
  methods: {
    ...mapActions([
      'increment'
    ])
  },
```

调用

```html
<button @click="increment({num:2})">5秒后拆散一对情侣</button>
```

#### 2.4.2 组合多个action

store.dispatch可以处理被触发的 action 的处理函数返回的 Promise，并且 `store.dispatch` 仍旧返回 Promise

假设我们的action如下

```javascript
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

那么我们在组件里可以这样处理：

```javascript
store.dispatch('actionA').then(() => {
  // ...
})
```

在另一个action里的话，可以这样调用

```javascript
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```



总结：以上我们vuex的基本用法都讲完了，以上知识点基本上可以解决我们工作中面临的问题了。但是vuex还有些更加高深的用法，我们下一节再讲

