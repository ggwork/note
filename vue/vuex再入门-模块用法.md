# vuex再入门-模块用法

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
  mutations: {
    addAge: state => {
      state.age++
    }
  },
  getters: {
    manAge: state => {
      return state.age + 10
    }
  },
  actions: {
    addAge: context => {
      setTimeout(() => {
        context.commit('addAge')
      }, 3000);
    }
  }
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
  mutations: {
    addAge: state => {
      state.age++
    }
  },
  getters: {
    dogManAge: state => {
      return state.age + 10
    }
  },
  actions: {
    addAge: context => {
      setTimeout(() => {
        context.commit('addAge')
      }, 3000);
    }
  }
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

### 1.2 调用模块

在以上的代码中，我们通过模块的方法把store按照业务拆分成多个部分，这样使我们的代码结构更加清晰。那么问题来了，我们这样定义以后，我们该怎样使用模块中的变量呢？

**以下全是没有设置命名空间时的调用方法。**

#### 1.2.1 state

 获取sate需要带上模块的key才能获取。比如我们要获取moduleA中的name值，我们可以这样获取

```js
  computed: {
    name: function () {
      return this.$store.state.a.name
    }
  },
```

#### 1.2.2 getters

我们在A模块的getters中定义了manAge，在B模块的getters中定义了dogManAge。刚开始我以为获取getters跟获取state是一样的。因此我获取manAge的时候是这样写的。`this.$store.getters.a.manAge`，结果发现报错了。原来获取getters是直接获取的，并不用带模块名。

```js
manAge: function () {
    return this.$store.getters.manAge
},
```

***虽然我们把stroe分成了模块A和B，但是getter是不分模块的。所以不能再A模块的getters定义了manAge，B模块的getters也定义manAge。这样子会命名重复的错误*

#### 1.2.3 mutations和actions

调用mutations和actions中定义的方法也是不分模块的，直接按全局调用就可以了。比如我在A模块定义为了addAge方法，在B模块也定义了addAge方法。

调用的时候直接这样调用就可以了。

```javascript
  methods: {
    increment: function () {
      this.$store.commit('addAge')
    },
    increment2: function () {
      this.$store.dispatch('addAge')
    }
  },
```



**这里其实有个问题的，我在A和B模块都定义了addAge方法，当我在组件里调用this.$store.commit('addAge')的时候，A模块和B模块的方法都会被调用。这样是很危险的，可能你不想调用B模块的addAge方法，结果也被调用了。这里自己要检查情况。**

其实官方文档有这样一句话，我直接竟然没看到。*默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。*

#### 1.2.4 局部状态

没有使用命名空间的时候，对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**。

```javascript
  mutations: {
    addAge: state => {
      // 这里的state就是moduleA中的state。跟其他模块中的state没有关系
      state.age++
    }
  },
  getters: {
    // 这里的state就是moduleA中的state。跟其他模块中的state没有关系
    manAge: state => {
      return state.age + 10
    }
  },
```

同样，对于模块内部的 actions，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`

```js
  actions: {
    addAge: context => {
      // context.state 就是moduleA中的state，context.rootState就是全局state
      console.log('context.state:', context.state) // {age:32,name:'people'}
      console.log('context.rootState:', context.rootState) //{a:{age:32,name:'people'},b:{age:6,name='dog'}}
      setTimeout(() => {
        context.commit('addAge')
      }, 3000);
    }
  }
```



### 1.3 命名空间

首先命名空间是啥？这个我还在自己年幼的时候也不懂，而且这个名字看起来就能奇怪。命名空间就是给各个变量或者方法的调用添加前缀。比如我们有个方法xxx。我们不加命名空间的时候调用就是xxx()。我们如果给它加一个命名空间a,那么我们调用的时候就是a.xxx()。我们给她加个命名空间b，我们调用就会b.xxx()

在store中添加命名空间很简单。我们只需要增加一个nameSpaced为true的字段就可以。命名空间名就是我们在index.js中注册时的key值。比如我们的A模块，添加命名空间后代码如下。

```js
const moduleA = {
  namespaced: true,
  state: {
    name: 'people',
    age: 30
  },
  mutations: {
    addAge: state => {
      // 这里的state就是moduleA中的state。跟其他模块中的state没有关系
      state.age++
    }
  },
  getters: {
    // 这里的state就是moduleA中的state。跟其他模块中的state没有关系
    manAge: state => {
      return state.age + 10
    }
  },
  actions: {
    addAge: context => {
      // context.state 就是moduleA中的state，context.rootState就是全局state
      console.log('context.state:', context.state) // {age:32,name:'people'}
      console.log('context.rootState:', context.rootState) //{a:{age:32,name:'people'},b:{age:6,name='dog'}}
      setTimeout(() => {
        context.commit('addAge')
      }, 3000);
    }
  }
}
export default moduleA
```

#### 1.3.1 state

添加了命名空间后我们获取state跟之前的方法一样

```js
age: function () {
    return this.$store.state.a.age
},
```

#### 1.3.2 getters，mutations和actions

调用getters,mutations和actions就要加命名空间了。

```js
// getters
manAge: function () {
    return this.$store.getters['a/manAge']
}
```

mutaions和actions

```javascript
  methods: {
    increment: function () {
      this.$store.commit('a/addAge')
    },
    increment2: function () {
      this.$store.dispatch('a/addAge')
    }
  },
```

#### 1.3.3 局部状态

添加上命名空间后，`rootState` 和 `rootGetter` 会作为第三和第四参数传入 getter，也会通过 `context` 对象的属性传入 action

```js
  getters: {
    // 这里的state就是moduleA中的state。跟其他模块中的state没有关系
    manAge: (state, getters, rootState, rootGetters) => {
      console.log('getters:', getters) // {manAge:40}
      console.log('rootState:', rootState) // {a:{age:30,name:'people'},b:{age:4,name:'dog'}}
      console.log('rootGetters:', rootGetters) // {'a/manAge':40,dogManAge:14}
      return state.age + 10
    }
  },
```

#### 1.3.4 带命名空间注册全局action

我们的action有时候要全局注册，可以把方法的root设置为true，并且改变方法的定义方式

A模块的addAge之前是这个样的

```js
  actions: {
    addAge: context => {
      setTimeout(() => {
        context.commit('addAge')
      }, 3000);
    }
  }
```

我们改成这个样

```javascript
  actions: {
    addAge: {
      root: true,
      handler: (context) => {
        setTimeout(() => {
          context.commit('addAge')
        }, 3000);
      }
    }
  }
```



### 1.4 map函数

模块不加命名空间的时候，我们是不能使用store提供的map函数的，比如mapState，mapGetters，mapMutations和mapActions。加上命名空间后我们就可以使用了。

调用的时候第一个参数传命名空间，第一个传数组或者对象，就是我们要获取的属性

```js
    ...mapState('a', [
      'name',
      'age'
    ]),
    ...mapGetters('a', [
      'manAge'
    ]),
    ...mapState('b', {
      dogAge: 'age'
    }),
    ...mapGetters('b', [
      'dogManAge'
    ])
```

#### 1.5 createNamespacedHelpers` 

我们也可以通过使用 `createNamespacedHelpers` 创建基于某个命名空间辅助函数。它返回一个对象，对象里有新的绑定在给定命名空间值上的组件绑定辅助函数。

使用方法如下：

```js
import { createNamespacedHelpers } from 'vuex'
const { mapState: AmapState, mapGetters: AmapGetters } = createNamespacedHelpers('a')
const { mapState: BmapState, mapGetters: BmapGetters } = createNamespacedHelpers('b')
export default {
  name: 'HelloWorld',
  data() {
    return {
      count: 10
    }
  },
  computed: {
    ...AmapState([
      'name',
      'age'
    ]),
    ...AmapGetters([
      'manAge'
    ]),
    ...BmapState({
      dogAge: 'age'
    }),
    ...BmapGetters([
      'dogManAge'
    ])
  }
}
```



### 1.5 其他

#### 1.5.1 嵌套

store中模块是可以多层嵌套的。比如我们A模块嵌套一个C模块

```js
const moduleA = {
  namespaced: true,
  state: {
    name: 'people',
    age: 30
  },
  mutations: {
    addAge: state => {
      state.age++
    }
  },
  getters: {
    manAge: (state) => {
      return state.age + 10
    }
  },
  actions: {
    addAge: {
      root: true,
      handler: (context) => {
        setTimeout(() => {
          context.commit('addAge')
        }, 3000);
      }
    }
  },
  modules: {
    c: {
      state: 60,
      mutations: {

      },
      getters: {

      },
      actions: {}

    }
  }
}
export default moduleA
```

#### 1.5.2 动态注册

模块是可以动态注册和删除的。注册的方法是`store.registerModule(moduleName,module)` 。删除的方法是store.unregisterModule(moduleName)

```js
this.$store.registerModule('d',{
    
})
```



***以上是store中关于模块的部分，还是蛮重要的。*



