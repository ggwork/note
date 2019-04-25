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

修改Login.vue。

把代码修改成如下的样式后，打开页面。发现我们页面功能是有了，但是太丑了，表单占据了整个页面的宽度，因此我们还要修改下样式。这里我们就要用到sass

```html
<template>
  <div class="login">
    <div class="l-form">
      <div class="l-tip">图书馆管理系统</div>
      <el-form ref="form" :model="form">
        <el-form-item>
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input v-model="form.password" type="password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      form: {
        name: '',
        password: ''
      }
    }
  },
  methods: {
    onSubmit() {

    },
  },
}
</script>
<style>
</style>

```

### 7.sass

我们可以直接用css的，但是我不推荐这样用，因为原生太丑了。我们使用css预处理器写css样式。css预处理器有sass和less，当然还有其他的但是我没用过。less我不推荐使用，因为功能不强、像自定义函数功能就没有，写代码就别扭，我推荐sass。

安装sass

```shell
D:\test\book>yarn add node-sass sass-loader
```

vue-cli3  对sass简直是0配置，上面我们安装好处理器后，直接在style标签上加上`lang=“scss”`就可以使用sass了。

修改Login.vue中的style

```html
<style lang="scss">
.login {
  width: 100%;
  height: 100%;
  background: #000;
  .l-form {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translateY(-50%) translateX(-50%);
    width: 300px;
    margin: auto;
    border-radius: 8px;
    background: #fff;
    padding: 20px;
    .l-tip {
      text-align: center;
      font-size: 24px;
      font-weight: bold;
    }
    .el-form {
      width: 100%;
      margin: auto;
      margin-top: 20px;
      .el-form-item {
        button {
          width: 100%;
        }
      }
    }
  }
}
</style>

```

样式跟我们想要的还有点差别，是因为父元素的样式问题引起的，修改App.vue中的style。主要是给html，body还有#app元素设置了宽和高和边距。

```html
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>


<style lang="scss">
html,
body,
#app{
  width: 100%;
  height: 100%;
  margin: 0px;
  padding: 0px;
}
</style>

```

再打开浏览器，看我们的页面，已经很好看了。

### 8.normalize.css

安装normalize.css 其实这个安不安装都可以，我是有强迫症就安装了。

```shell
yarn add normalize.css
```

修改main.js引入normalize.css。

```shell
import 'normalize.css'
```

### 9.安装axios

```shell
D:\test\book>yarn add axios
```

引入axios

在api文件夹下新建config.js文件，配置一些请求的通用选项，同时还对gei和post请求进一步封装。其实封装不封装都无所谓，我个人感觉封装后会省事一点。代码如下：

```js
import axios from "axios"
import { Message } from "element-ui"

//  这个baseUrl要根据实际情况进行改变
axios.defaults.baseURL = "/"
axios.defaults.headers.common["Content-Type"] =
  "application/json; charset=UTF-8"
axios.defaults.headers.common["Access-Control-Allow-Origin"] = "*"

// 请求拦截器 添加token
axios.interceptors.request.use(
  config => {
    return config
  },
  error => {
    return Promise.reject(error)
  }
)

// 响应拦截器即异常处理
axios.interceptors.response.use(
  response => {
    return response
  },
  error => {
    Message({
      message: error.message,
      type: "error",
      duration: 5000,
    })
    return Promise.resolve(error)
  }
)

export default {
  // get请求
  get(url, param) {
    return new Promise((resolve, reject) => {
      axios({
        method: "get",
        url,
        params: param,
      })
        .then(res => {
          resolve(res)
        })
        .catch(error => {
          Message({
            message: error,
            type: "error",
            duration: 5000,
          })
          reject(error)
        })
    })
  },
  // post请求
  post(url, param) {
    return new Promise((resolve, reject) => {
      axios({
        method: "post",
        url,
        data: param,
      })
        .then(res => {
          resolve(res)
        })
        .catch(error => {
          Message({
            message: error,
            type: "error",
            duration: 5000,
          })
          reject(error)
        })
    })
  },
  // all get
  allGet(fnArr) {
    return axios.all(fnArr)
  },
}

```

在api的文件夹下新建login.js。这个函数的作用就是请求后台的login接口。

代码如下:

```javascript
import service from './config'
class Login {
  login(params) {
    return service.post('login', params)
  }
}
export default new Login()
```

现在问题来了，有了接口了，但是我们没有后台啊。我们没法做登录的功能。不过不用担心，现在的我们的前端已经很强大了。没有后端，我们可以使用mock来模拟后端

### 10.设置eslint

在安装mock前，我们还有一个小问题要解决，就是设置eslint的规则，默认的eslint的规则很严格的，我们在页面甚至不能使用console.log()  这就会给我们的调试带来困难。因此我们要禁用一些eslint规则。

打开package.json，找到`eslintConfig`项，在找到其下的rules。配置`"no-console": "off"`：

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
      "no-console": "off"
    },
    "parserOptions": {
      "parser": "babel-eslint"
    }
  },
```

好了，现在我们就可以愉快的在vue写console了。

### 11. mock

mock是啥呢？mock是一个测试工具。mock会拦截ajax请求，以前需要后台返回给我们的数据，现在我们可以使用mock返回。mock的功能很强大，可以模拟出后端的增删改查等功能。非常方便我们前端进行测试.

#### 11.1 安装

```shell
D:\test\book>yarn add mockjs
```

#### 11.2 使用

在main.js的同级目录下新建mock.js。我们在mock.js里定义刚刚我们需要的login接口。代码如下：

```javascript
import Mock from 'mockjs'

Mock.mock('/login', 'post', (options) => {
  // console.log('options:', options)
  let data = JSON.parse(options.body)
  let name = data.name
  let password = data.password
  if (name === 'admin' && password === 'admin') {
    return {
      status: 1,
      message: '登录成功'
    }
  } else {
    return {
      status: 0,
      message: '账号或者密码错误'
    }
  }
})
```

这段代码的作用即使拦截login接口请求，当账号和密码是admin的时候，就返回请求成功，否则返回‘账号密码错误’

现在问题来了？我们写好程序了，怎么使用mock呢，其实非常简单，简答到我都不敢相信。我们直接在main.js文件中引入mock.js既可。

main.js

```shell
// 引入mock
import './mock.js'
```



ok，现在再打开我们的login页。输入账号和密码。如果不是admin，则弹窗报错.

![账号密码错误](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vue/v-login-fail.png)

如果账号密码正确，则跳转到index页面。

![index页面](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vue/v-login-success.png)



ok。自此大体工作都完成了，接下来，我们继续完成项目（好累）。

### 12.vee-validate 表单验证

我们的登录表单还有个问题，就是怎么加验证。表单不验证，一来不容易发现问题，二来会频繁的骚扰后端。自己写验证也可以，但是每次都要重复写很多代码，键盘都受不了。so，还是用组件吧，我使用的是vue推荐的vee-validate。

**安装**：

```shell
yarn add vee-validate
```

**配置**：

在main.js中引入vee-validate

```shell
// 引入表单验证
import VeeValidate, {
  Validator
} from 'vee-validate'
Vue.use(VeeValidate, {
  fieldsBagName: 'vee-fields'
})

// 汉化表单验证
import zhCN from 'vee-validate/dist/locale/zh_CN'
Validator.localize('zh_CN',
  zhCN)
```

修改login.vue 添加表单验证，以用户名为例，我的要求是用户名不能为空，添加的规则如下：

```html
<el-input
v-model="form.name"
v-validate="{required:true}"
name="name"
:class="{'is-danger':errors.has('name')}"
data-vv-as="用户名"
placeholder="请输入用户名"
></el-input>
```

- v-validate 里配置的是验证规则
- name 是字段名称，这个名称可以自己定
- is-danger 是我为报错的字段配置的一个class名，如果erros.has(字段名)不为空，则说明验证没通过。

is-danger 样式如下，把错误表单的边框设置成红色，目的是为了突出显示错误信息。至于为什么加`/deep/`前缀，是因为`el-input`组件是element组件，我们再`style`中设置的样式是局部的，没法应用的到element子组件上，需要加上/deep/

```css
/deep/ .is-danger input {
  border-color: #ff3860;
}
```

接下来，我们要考虑错误信息怎么显示。我的做法是直接在表单下显示错误信息就可以，缺点是如果错误信息很多，每个输入框都有一个错误信息的话，表单就会变得很高。

因为每个输入框都要显示错误信息，所以我觉得把显示错误信息的功能做成组件，这样可以通用，省了很多重复代码。

在components文件夹下新建common文件夹，再在commen文件夹下新建`FormErrorMessage.vue`组件

代码如下：

```html
<template>
  <div class="help is-danger">
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: 'FormErrorMessage'
}
</script>
<style scoped>
.help {
  font-size: 0.75rem;
  margin-top: 0.25rem;
  line-height: 0.75rem;
}

.help.is-danger {
  color: #ff3860;
}
</style>

```

在Login.vue中引入

```js
import FormErrorMessage from '../components/common/FormErrorMessage.vue'
```

在components中配置

```json
  name: 'Login',
  components: {
    FormErrorMessage
  },
  data() {
    return {
      form: {
        name: '',
        password: ''
      }
    }
  },
```

使用

```html
<el-form-item>
    <el-input
      v-model="form.name"
      v-validate="{required:true}"
      name="name"
      :class="{'is-danger':errors.has('name')}"
      data-vv-as="用户名"
      placeholder="请输入用户名"
    ></el-input>
</el-form-item>
<el-form-item>
    <FormErrorMessage v-if="errors.has('name')">{{ errors.first('name') }}</FormErrorMessage>
</el-form-item>
```



表单验证添加了，错误显示的组件也添加了，现在只差怎么触发表单验证了。很简单，修改onSubmit函数，代码如下,如果result为true，则说明表单验证通过了，否则错误信息，就会自动显示在错误信息组件里。

```javascript
    onSubmit() {
      this.$validator.validate().then(result => {
        if (result) {
          login.submit(this.form).then(res => {
            // console.log('res:', res);
            if (res.data.status === 1) {
              // 如果登录成功则跳转我index页面
              this.$router.push('/index')
            } else {
              // 使用element-ui的message组件，显示登录报错信息
              this.$message({
                message: res.data.message,
                type: 'error',
                duration: 5000
              })
            }
          }).catch(error => {
            this.$message({
              message: error,
              type: 'error',
              duration: 5000
            })
          })
        }
      })
    },
```

添加错误提示后，表单样式还需要调整下，我就不调了，结果如下：

![验证错误](https://mycode04-1252305175.cos.ap-guangzhou.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87/vue/v-login-verror.png)

完整`Login.vue`代码如下:

```html
<template>
  <div class="login">
    <div class="l-form">
      <div class="l-tip">图书馆管理系统</div>
      <el-form ref="form" :model="form">
        <el-form-item>
          <el-input
            v-model="form.name"
            v-validate="{required:true}"
            name="name"
            :class="{'is-danger':errors.has('name')}"
            data-vv-as="用户名"
            placeholder="请输入用户名"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <FormErrorMessage v-if="errors.has('name')">{{ errors.first('name') }}</FormErrorMessage>
        </el-form-item>

        <el-form-item>
          <el-input
            v-model="form.password"
            type="password"
            v-validate="{required:true}"
            name="password"
            :class="{'is-danger':errors.has('password')}"
            data-vv-as="密码"
            placeholder="请输入密码"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <FormErrorMessage v-if="errors.has('password')">{{ errors.first('password') }}</FormErrorMessage>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="onSubmit">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import login from '../api/login.js'
import FormErrorMessage from '../components/common/FormErrorMessage.vue'
export default {
  name: 'Login',
  components: {
    FormErrorMessage
  },
  data() {
    return {
      form: {
        name: '',
        password: ''
      }
    }
  },
  methods: {
    onSubmit() {
      this.$validator.validate().then(result => {
        if (result) {
          login.submit(this.form).then(res => {
            // console.log('res:', res);
            if (res.data.status === 1) {
              // 如果登录成功则跳转我index页面
              this.$router.push('/index')
            } else {
              // 使用element-ui的message组件，显示登录报错信息
              this.$message({
                message: res.data.message,
                type: 'error',
                duration: 5000
              })
            }
          }).catch(error => {
            this.$message({
              message: error,
              type: 'error',
              duration: 5000
            })
          })
        }
      })
    },
  },
}
</script>
<style lang="scss" scoped>
.login {
  width: 100%;
  height: 100%;
  background: #000;
  .l-form {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translateY(-50%) translateX(-50%);
    width: 300px;
    margin: auto;
    border-radius: 8px;
    background: #fff;
    padding: 20px;
    .l-tip {
      text-align: center;
      font-size: 24px;
      font-weight: bold;
    }
    .el-form {
      width: 100%;
      margin: auto;
      margin-top: 20px;
      .el-form-item {
        button {
          width: 100%;
        }
      }
    }
  }
}
.is-danger input {
  border-color: #ff3860;
}
</style>

```

### 13. js-cookie

本来接下来该讲vuex了，这里插播一个组件`js-cookie`

地址：[js-cookie](https://www.npmjs.com/package/js-cookie)

**安装**

```
yarn add js-cookie
```

至于这个怎么用，我们稍后讲vuex的时候再讲解。

### 14.vuex

先在我们终于讲到最后一个知识点vuex了。为什么要用vuex,在本项目里，使用vuex是为了保持网站的登录状态。比如我们index页面要求用户必须登录才能够访问，这里就要用vuex了。

地址：[vuex](https://vuex.vuejs.org/zh/installation.html)

**安装**

```shell
D:\test\book>yarn add vuex
```

在store文件夹下，新建index.js文件

代码如下：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
// 引入js-cookie
import Cookies from 'js-cookie'
Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    name: ''
  },
  mutations: {
    loginIn(state, name) {
      state.name = name
      // 设置过期时间为1天
      Cookies.set('name', name, {
        expires: 1
      })
    },
    loginOut(state) {
      state.name = ''
      Cookies.remove('name')
    }
  }
})
export default store
```

我们定义了一个loginIn方法，调用这个方法，我们就可以把用户的用户名存在store中，同时也存在cookie里，cookie的有效期是1天。

修改`Login.vue`的`onSubmit`方法，用户登录成功后，就把用户信息存在store中



自此，一个我们常用vue项目基本上配置完成了。当然不同的项目，还有其他不同的组件需要安装，到时候你们再根据情况确定吧。

关于我项目里用到的组件，我都是简单的使用，入门而已。如果真的做项目还需要对每个组件都深入了解，比如element和mock和vee-validate。你们自己去研究吧。

项目地址如下：

book



