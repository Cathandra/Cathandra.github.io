---
title:      "vuex"
date:       2019-05-05
tags: 
    - vue
categories: [Front-End]
---

> `vuex`是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。  
> vuex适用于中大型单页应用，考虑如何把组件的共享状态抽取出来，以一个全局单例模式管理，不管在哪个组件，都能获取状态/触发行为。

<!--more-->

## vuex安装

由于vue-cli搭建脚手架时安装了vuex（上星期的vue-router同理），所以不多介绍。  

```node
npm install vuex --save
```

安装完毕后，需要引入并使用vuex：

```js
import Vuex from 'vuex'

Vue.use(Vuex)
```

## vuex使用

### 搭建store实例

注册store:

```js
import store from './store'
```

创建`Vuex.Store`实例，注册`state`、`mutations`、`actions`、`getters`:

```js
export default new Vuex.Store ({
      state: ,
      mutations: ,
      actions: ,
      getters: ,
    })
```

### 引入store

在入口文件main.js中引入store并注册实例中

```js
import store from './store'

new Vue({
    router,
    store,
}).$mount('#app')
```

### 组件中使用

引入各属性对应的辅助函数，然后就可以使用store中的状态数据、方法。

```js
import {mapActions, mapState,mapGetters} from 'vuex'
```

### 举个例子

一个简单的点击info页面按钮，about页中计数的小例子：

```js
//store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increase () {
      this.state.count += 1
    }
  }
})
```

```html
<!--info.vue-->
<template>
  <div>
      info component
      <button type="button" @click="add()">CLICK ME!</button>
  </div>
</template>

<script>
import store from '@/store'
export default {
  name: 'Info',
  store,
  methods: {
      add () {
          console.log('Click number added from info')
          store.commit('increase')
      }
  }
}
</script>
```

```html
<!--about.vue-->
<template>
  <div class="about">
    <h1>Vue is accelent</h1>
    <p>Info page has been clicked by {{msg}} times in this session.</p>
  </div>
</template>
<script>
import store from '@/store'
export default {
  name: 'about',
  store,
  data () {
    return {
      msg: store.state.count
    }
  }
}
</script>
```

## 参考资料

- [Vuex官方文档](https://vuex.vuejs.org/zh/)
