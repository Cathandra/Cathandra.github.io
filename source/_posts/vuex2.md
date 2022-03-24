---
title:      "vuex详解"
date:       2019-05-12
tags: 
    - vue
categories: [Front-End]
---

上周跟着慕课网的视频复习了vuex，虽然大致过了一遍vuex的模式，但其实对这个组件的细节理解并不到位。  
这次就参考vuex官方文档，结合上次的demo，仔细对相关功能做理解。

首先引用别人的总结。简言之：  

> `state`属性存储状态值、`getter`属性获取状态值、`mutation`提供改变状态的方法、`action`属性接收对应的指令，去`mutation`中执行。

<!--more-->

这是上周demo的store.js文件。

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

## state

> state属性存储状态值。

在根组件注册store之后，可以在组件中通过`store.state.count`访问state中的参数。  
在本案例中，state非常简单，就是点击数`count`。

```js
//store.js
...
export default new Vuex.Store({
  state: {
    count: 0
  }
  ...
})
```

这个值被放在About页面的`{{ msg }}`中展示：

```html
<!--about.vue-->
<template>
    ...
    <p>Info page has been clicked by {{msg}} times in this session.</p>
    ...
</template>
<script>
    ...
  data () {
    return {
      msg: store.state.count // count
    }
  }
</script>
```

## getter

> getter属性获取状态值。

在需要对参数进行计算时，可以通过在store中定义getter，使state中派生出其他状态。  
案例中没有涉及。

## mutation

> mutation提供改变状态的方法。

mutation用于修改store的状态。提交mutation后，vue监控到数据的变化，随后动态更新节点。
在本案例中，通过`this.state.count`调用state，并在mutation中每次增加1。

```js
//store.js
...
export default new Vuex.Store({
    ...
    mutations: {
        increase () {
            this.state.count += 1
        }
    }
})
```

## action

> action属性接收对应的指令，去mutations中执行。

注意Action提交的是mutation，而不是直接变更状态。  
案例中没有涉及，也举个例子吧。

```js
...
export default new Vuex.Store({
  ...
    actions: {
        increment (context) {
            context.commit('increment')
        }
    }
})
```

## 参考资料

- [Vuex官方文档](https://vuex.vuejs.org/zh/)
