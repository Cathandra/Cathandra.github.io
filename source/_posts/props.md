---
layout: post
title: Vue的Prop属性
date: 2020-01-16 19:50:11
tags:
    - vue
categories: [Front-End]
---
复读：有些问题本不是问题，不看文档，于是也就成了问题。  
文档真是常看常新啊: )  

>组件实例的作用域是孤立的。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。父组件的数据需要通过 prop 才能下发到子组件中。
> ——官方文档

也就是说，`Prop`是子组件访问父组件数据的唯一接口。  

<!-- More -->

## Prop 用法

### 动态传递

父组件：

```HTML
<template>
    <child :prop_content='data_for_child' />
</template>

<script>
export default {
    components: { Child },
    data: function() {
        return {
            data_for_child: "Dynamic props msg for child",
        };
    }
}
</script>
```

子组件：

```HTML
<script>
export default {
    props: ["prop_content"],
    data: function() {
        // 通过this.prop_content即可获取传值
        console.log(this.prop_content)
    }
}
</script>
```

### 静态传递

所谓静态传递，只要将传递的内容放在`{% raw %}{{}}{% endraw %} `中就可以了。  
上述例子中的子组件改作：  

```HTML
<template>
    {{prop_content}}
</template>

<script>
export default {
    props: ["prop_content"],
    data() {
        return {};
    }
}
</script>
```

## Prop 类型

>字符串数组形式列出的 prop：  

```JS
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

>如果希望每个 prop 都有指定的值类型，可以以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型：

```JS
props: {
    title: String,
    likes: Number,
    isPublished: Boolean,
    commentIds: Array,
    author: Object,
    callback: Function,
    contactsPromise: Promise // or any other constructor
}
```

## Prop 验证

在指定值类型的基础上，prop可以对数据的类型进行直接验证。包括：  
基础的类型检查，多个可能的类型，必填，带有默认值的数字，带有默认值的对象以及自定义验证函数等。  

```JS
props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
}
```

## 单向数据流

>所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。
>
>额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你**不应该**在一个子组件内部改变 prop。
>
>这个 prop 以一种原始的值传入且需要进行转换。在这种情况下，最好使用这个 prop 的值来定义一个计算属性。

简言之，  

1. 数据流是**单向下行**绑定的；
2. 父组件更新会影响子组件，但不允许反之改变子组件的prop；  
3. 可使用计算属性对prop传入的值进行加工，这种方法不会改变prop。  

## Props down, events up

>另外，父子组件的关系可以总结为：  
>props down, events up  
>父组件通过 props 向下传递数据给子组件；子组件通过 events 给父组件发送消息。  
> ——brave-sailor

## 参考资料

- [通过 Prop 向子组件传递数据 - Vue.js](https://cn.vuejs.org/v2/guide/components.html#%E9%80%9A%E8%BF%87-Prop-%E5%90%91%E5%AD%90%E7%BB%84%E4%BB%B6%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE)
- [Prop - Vue.js](https://cn.vuejs.org/v2/guide/components-props.html)
- [关于Vue中props的详解 - jingtian678](https://blog.csdn.net/jingtian678/article/details/81160995)（有少量错误）
- [Vue props用法详解 - brave-sailor](https://www.cnblogs.com/Free-Thinker/p/11658783.html)