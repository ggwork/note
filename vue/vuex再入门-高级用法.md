# vuex再入门-高级用法

## 1 模块module

vuex由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割。

一个简单的案例如下：

在store文件夹下新建modules文件夹，在modules文件夹下新建moduleA.js和moduleB.js文件

```shell
|-store
  |-index.js
  |-modules
  |  |-moduleA.js
  |  |-moduleB.js
```

moduleA..js代码如下：

```javascript
const moduleA = {
  state: {
    name: 'people',
    age: 30
  },
  mutations: {},
  getters: {},
  actions: {}
}
export default moduleA
```

moduleB.js代码如下：

```javascript
const moduleB = {
  state: {
    name: 'dog',
    age: 4
  },
  mutations: {},
  getters: {},
  actions: {}
}
export default moduleB
```

store下index.js文件代码如下：

```js
import Vue from 'vue';
import Vuex from 'vuex';
import moduleA from './modules/moduleA';
import moduleB from './modules/moduleB';
Vue.use(Vuex)
const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }

})

export default store
```

在组件中的调用方法如下：

```js
  computed: {
    name: function () {
      return this.$store.state.a.name
    }
  },
```



