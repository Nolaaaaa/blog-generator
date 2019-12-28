---
title: Vue相关知识整理
date: 2018-07-18 14:33:55
tags: Vue
categories: 前端
---

整理的在使用vue的过程中遇到的一些零碎的容易忘记的知识点
<escape><!-- more --></escape>

## Vue
### vue的computed属性和methods区别
1. computed是响应式的，methods并非响应式。
2. 调用方式不一样，computed定义的成员像属性一样访问，methods定义的成员必须以函数形式调用。
3. computed是带缓存的，只有其引用的响应式属性发生改变时才会重新计算，而methods里的函数在每次调用时都要执行。
4. computed中的成员可以只定义一个函数作为只读属性，也可以定义get/set变成可读写属性，这点是methods中的成员做不到的

### Vue 的双向绑定是如何实现的？如何追踪变化？
把一个普通的 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 把这些属性全部转为 getter/setter。
这些 getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知变化。
[vue响应式原理](https://cn.vuejs.org/images/data.png)

### 父子通信
组件关系可分为父子组件通信、兄弟组件通信（new Vue() 作为 eventBus）、跨级组件通信。
父子组件通信的方法为：子组件通过`props方法`接受父组件的data数据，子组件通过`$emit`触发父组件的自定义事件，如：
```java
// 父组件将数据（currentUser）和方法（logout）传递出去
<template>
  <Todo v-else @logout="logout" :currentUser="currentUser"/>
</template>

// 子组件接收数据和方法
props : ['currentUser'],
methods: {
  logout() {   //注册 _user
    this.$emit("logout")
  }
}
```
on和emit的事件必须是在一个公共的实例上，才能触发
子组件用`$emit()`触发事件`this.$emit(‘logout’)`
父组件用`$on()`监昕子组件的事件`@logout =“logout”`
[父子通信](https://images2018.cnblogs.com/blog/1047894/201804/1047894-20180408144143328-388340893.png)

### v-show和v-if指令的共同点和不同点
**v-show**指令是通过修改元素的display的CSS属性让其显示或者隐藏
**v-if**指令是直接销毁和重建DOM达到让元素显示和隐藏的效果

### 生命周期
生命周期，即Vue 实例从创建到销毁的过程，一共分为8个阶段。生命周期可以让我们在控制整个Vue实例时形成好的逻辑。第一次页面加载时会触发 beforeCreate, created, beforeMount, mounted 这几个钩子。DOM 在 mounted时渲染完成。[Vue生命周期举例](http://www.cnblogs.com/gagag/p/6246493.html)[API — Vue.js](https://cn.vuejs.org/v2/api/%23%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)
**创建前/后：** 
**beforeCreated**阶段，vue实例的挂载元素$el和数据对象data都为undefined，还未初始化。这个时候可以加loading事件。
**created**阶段，vue实例的数据对象data有了，$el还没有。这个时候可以结束loading事件，也可以调用异步请求。
**载入前/后：**
**beforeMount**阶段，vue实例的$el和data都初始化了，但还是挂载之前为虚拟的dom节点，data.message还未替换。
**mounted**阶段，vue实例挂载完成，data.message成功渲染。
**更新前/后：**
当data变化时，会触发**beforeUpdate**和**updated**方法。
**销毁前/后：**
**beforeDestroy**时可以做一个确认停止事件的确认框。执行 **destroy** 方法后，对data的改变不会再触发周期函数，说明此时vue实例已经解除了事件监听以及和dom的绑定，但是dom结构依然存在
![生命周期](https://i.loli.net/2018/08/18/5b77c66918abe.png)

### css在Vue.cli中怎么安装使用？
**Scss：**
可以用变量，例如（$变量名称=值）
可以用混合器，例如（）
可以嵌套
**Vue.cli中安装使用：**
```javascript
//安装
npm i -D sass-loader
npm i -D node-sass
//页面引用
<style lang="scss">
```

### 什么是MVVM
ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作是自动的，无需人为操作DOM。
MVVM主要解决了MVC中大量的DOM 操作使页面渲染性能降低，加载速度变慢的缺陷

### 一些报错及解决方法
1. Vue空格等报错解决方法
```javascript
//注释掉webpack.base.config.js文件中的如下代码
const createLintingRule = () => ({
  /*test: /\.(js|vue)$/,
  loader: 'eslint-loader',
  enforce: 'pre',
  include: [resolve('src'), resolve('test')],
  options: {
    formatter: require('eslint-friendly-formatter'),
    emitWarning: !config.dev.showEslintErrorsInOverlay
  }*/
}) 
```

2. Vue去掉警告 You are running Vue in development mode
在js中加一句：`Vue.config.productionTip=false`

### 使用Vue时的注意事项及一些小技巧
1. vue实例挂载点，不能是div和html
2. 单vue组件，template必需且只能有一个根结点
3. 监听一个vue组件的input时加.native`@keyup.enter.native="addTodo"`
4. 使文本框自动获取焦点的方法：
```java
// html文件
<input ref="input">

// js文件
new Vue({
  ...
  mounted() {
    this.$refs['input'].focus()
    // this.$refs['input'].value = ''
  }，
	...
})
```
5. 让CSS只在当前组件中起作用，可以将当前组件的`<style>`修改为`<style scoped>`
6. 渲染时莫名其妙报错可以用`:key="xxx.id"`解决。原因：key的值是需要是唯一识别的
7. 避免` v-if `和` v-for` 用在一起。为什么？因为v-for 比 v-if 具有更高的优先级，通过v-if 移动到容器元素，不会再重复遍历列表中的每个值。只检查它一次，且不会在 v-if 为否的时候运算 v-for。

### 图片资源懒加载
```JS
// 下载
$ npm install vue-lazyload --save-dev
// 引入
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload)
// 使用
<img v-lazy="/static/img/1.png">
```

### 性能优化
1. 频繁切换条件场景用`v-show`，不频繁的切换使用`v-if`。（`v-show`元素一直被渲染； `v-if`条件为真时才渲染）
2. 依赖的属性值进行数值计算时使用`computed`，监听数据变化后执行异步或开销较大的操作时使用`watch`
3. `v-for`比`v-if`优先级高，如使用在同一个标签，会意味着每个循环都会执行`v-if`，如果不是每个循环都需要判断，最好将`v-if`放在循环外面
4. 如果内容不需要改动，可将数据`Object.freeze(data)`冻结起来，防止Vue通过 `Object.defineProperty`对数据进行劫持
5. 由于在Vue组件内使用`addEventListene`等方式是不会自动销毁的，所以需要在组件销毁时手动`removeEventListener`移除这些事件的监听，以免造成内存泄露
6. 图片路由资源懒加载
7. 第三方插件按需引入，可借用`babel-plugin-component`插件
8. 服务端渲染SSR，直接在服务端完成渲染，所以可以被搜索引擎爬取工具抓取（和异步获取的SPA页面相比有更好的SEO，搜索引擎爬取工具抓取不到异步返回的内容），且无需等待所有js加载完再去渲染，能加快首屏加载速度。缺点：需要Node.js环境、只支持`beforCreate`和`created`两个钩子函数、服务器负载


## Vuex 
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式，它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化，并没有任何永久存储的功能。每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)。
[十分钟入门Vuex](https://www.jianshu.com/p/a804606ad8e9)
### 如何在 Vue 组件中展示状态？
一个store实例：
```javascript
import Vue from "vue"
import Vuex from "vuex"
Vue.use(Vuex)
const store = new Vuex.Store({
  state，   
  getters，    
  mutations， 
  actions， 
})
```
在Vue的实例中，把 store 对象提供给 “store” 选项`即store:store`，这可以把 store 的实例注入所有的子组件，子组件能通过`this.$store`访问到，如：
```javascript
const app = new Vue({
  el: '#app',
  store,
})
```

### Vuex的几种属性介绍
vuex一共有5种属性，分别是State、 Getter、Mutation 、Action、 Module
#### State
1. Vuex就是一个仓库，仓库里面放了很多对象，state是唯一的数据源，**相当于VUE中的data**
2. State里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
3. 通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中
4. 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation
5. **mapState**辅助函数

#### Getter
1. Getters 可以对State进行计算操作，相当于**VUE中的computed**，getters 可以在多组件之间复用，如果一个状态只在一个组件内使用，是可以不用getters
2. 可以通过属性访问，如
```javascript
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```
3. 可以通过方法访问（让 getter 返回一个函数，来实现给 getter 传参，如`store.getters.getTodoById(2)的形式 `），但是需要注意的是，getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。
4. **mapGetters**辅助函数

#### Mutation
1. **同步**，**相当于VUE中的methods**，但是只能是同步。每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。它的回调函数(handler)接受state作为第一个参数，如`handler(state,其他参数){}`
2. 不能直接调用一个 mutation handler,需要以相应的 type 调用 **store.commit** 方法，如：
```javascript
mutations: {
  increment (state, n) {
    state.count += n
  }
}
//调用
store.commit('increment'，额外的参数即载荷payload)
```
3. 对象风格的提交方式，即直接使用包含 type 属性的对象
```javascript
store.commit({
  type: 'increment',
  amount: 10
})
```
4. 在对象上添加新属性
`Vue.set(obj, 'newProp', 123)`
`state.obj = { ...state.obj, newProp: 123 }`

#### Action
**异步**,但是actions不能对数据，只能用commit调用mutations。参数是store,可以用{commit}，然后commit('mutations中的方法', 参数)，  `this.$store.dispatch`用于分发action

#### Module
在其他地方用引入
```javascript
//在其他地方用引入
import {mapState,mapGetters,mapMuations, mapAtions} from 'vuex'
export default {
  computed:{
    ...mapState(['increment'])//相当于将 `this.increment()` 映射为 `this.$store.commit('increment')`
    ...mapGetters([''])
  }
  methods:{
    ...mapMuations([''])
    ...mapAtions([''])
  }
}
```

### 3  热重载Hot Reload
把getters统一为数据的获取出口，actions统一为数据的操作入口
热重载：主要在开发环境中使用；当模块内容修改时，保留Vuex数据，重载修改模块的业务逻辑；如果不用热重载，修改模块时整体刷新，数据不再保留
```javascript
//Vuex 支持在开发过程中热重载 mutation、module、action 和 getter、
//对于 mutation 和模块，需要使用 store.hotUpdate() 方法
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations.js'
import getters from './getters.js'
import actions from './actions.js'
import modulesA from './modules/a'
Vue.use(Vuex)
const state = { ... }
const store = new Vuex.Store({
  state,
  getters,
  mutations,
  actions,
  modules: {
      a: moduleA
  }
}
})
//热重载
if (module.hot) {
  // 使 action 和 mutation 成为可热重载模块
  module.hot.accept(['./getters.js','./mutations.js', './actions.js','./modules/a'], () => {
    // 获取更新后的模块
    // 因为 babel 6 的模块编译格式问题，这里需要加上 `.default`
    // 加载新模块
    store.hotUpdate({
      getters: require('./getters.js').default
      mutations: require('./mutations.js').default
      actions: equire('./actions.js').default
      modules: {
        a: require('./modules/a').default
      }
    })
  })
}
```


## Vue-router
[vue-router入门学习文章](http://www.cnblogs.com/keepfool/p/5690366.html)
路由执行过程：用户点击 `router-link` 标签时，寻找 `to` 属性，`to` 属性和 `js` 中配置的路径`{ path: '/home', component: Home}`  `path` 对应，找到匹配的组件，最后把组件渲染到 `router-view` 标签，OK。
### 1  $router和$route的区别是什么？都在什么时候用？
#### this.$router：
表示全局路由器对象，项目中通过router路由参数注入路由之后，在任何一个页面都可以通过此方法获取到路由器对象，并调用其`push()`, `go()`等方法；
#### this.$route:
表示当前正在用于跳转的路由器对象，可以调用其name、path、query、params等方法；
```javascript
//两条路由
const routes = [
  { path: '/home', component: Home },
  { path: '/about', component: About }
]
//创建路由实例
const router = new VueRouter({
      routes,
})
//配置完成后把路由实例注入到vue根实例
const app = new Vue({
  router
})
```


