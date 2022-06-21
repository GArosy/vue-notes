# 初识VueX
`VueX`是适用于在`Vue`项目开发时使用的**状态管理工具**。 在项目中，组件之间的通讯需要利用props或自定义事件完成。如果父组件下有多个子组件, 子组件之间通讯的路径就会变的很繁琐, 父组件需要监听大量的事件, 还需要负责分发给不同的子组件, 很显然这并不是我们想要的组件化的开发体验。

试想一下，如果在一个项目开发中频繁的使用组件传参的方式来同步`data`中的值，一旦项目变得很庞大，管理和维护这些值将是相当棘手的工作。为此，`Vue`为这些被多个组件频繁使用的值提供了一个统一管理的工具——`VueX`。在具有`VueX`的Vue项目中，我们只需要把这些值定义在VueX中，即可在整个Vue项目的组件中使用。

# 引入

```
yarn add vuex
// 或
npm install vuex --save
```

为方便管理，可以在项目目录下新建一个store文件夹保存vuex文件，并添加index.js文件作为vuex的统一接口。

> 由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。 

在index.js中必须显式地通过 `Vue.use()` 来挂载 Vuex： 

```
// index.js
import Vue from 'vue'
import Vuex from 'vuex'
import xxx from './xxx';

Vue.use(Vuex)

// 创建vuex对象并导入各个状态管理模块
export default new Vuex.Store({
	// 使用modules属性引入各模块
    modules: {
        // 状态管理文件
        xxx
    }
})
```

一个状态管理模块示例：

```js
// xxx.js
import store from ".";

export default {
    state: {
    	//存放的键值对就是所要管理的状态
        a: 'this is a'
    },
}
```

最后，将store挂载到当前项目的Vue实例当中去：

```js
// main.js
import Vue from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';

new Vue({
    el: '#app',
    router,
    store,	// store:store,和 router 一样，将我们创建的Vuex实例挂载到这个vue实例中
    render: h => h(App)
})
```



# 概念

## vuex对象

Vuex 和单纯的全局对象有以下两点不同： 

1. Vuex 的状态存储是**响应式**的。若 store 中的状态发生变化，相应组件也会得到更新。
2. **不能直接改变 store 中的状态**。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。

## store

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。

## state

`state`就是Vuex中的**公共状态**, 可以将`state`看作是所有组件的`data`，用于保存所有组件的公共数据。

## getters

可以将`getters`属性理解为所有组件的`computed`属性, 也就是**计算属性**。vuex的官方文档解释可以将getter理解为store的计算属性, getters的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

## mutations

可以将`mutaions`理解为`store`中的`methods`，`mutations`对象中保存着更改数据的回调函数,该函数名官方规定叫`type`, 第一个参数是`state`, 第二参数是`payload`, 也就是自定义的参数.

## actions

`actions` 类似于 `mutations`，不同在于：

- `actions`提交的是`mutations`而不是直接变更状态；
- `actions`中可以包含异步操作, `mutations`中绝对不允许出现异步；
- `actions`中的回调函数的第一个参数是`context`, 是一个与`store`实例具有相同属性和方法的对象；

 ![img](https://upload-images.jianshu.io/upload_images/16550832-20d0ad3c60a99111.png?imageMogr2/auto-orient/strip|imageView2/2/w/701/format/webp) 

首先，`Vue`组件如果调用某个 `VueX` 的方法过程中需要向后端请求时或者说出现异步操作时，需要`dispatch`  VueX中 `actions` 的方法，以保证数据的同步。可以说，`action` 的存在就是为了让`mutations` 中的方法能在异步操作中起作用。

如果没有异步操作，那么我们就可以直接在组件内提交状态中的 `Mutations` 中自己编写的方法来达成对`state`成员的操作。注意，不建议在组件中直接对 `state` 中的成员进行操作，这是因为直接修改(例如：`this.$store.state.name = 'hello'` )的话不能被 `VueDevtools` 所监控到。

最后被修改后的state成员会被渲染到组件的原位置当中去。