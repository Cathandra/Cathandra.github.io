---
title: "Vue的计算属性和侦听属性"
date: 2019-11-08 15:43:42
tags: 
    - vue
categories: [Front-End]
---
有些问题本不是问题，不看文档，于是也就成了问题。  
还是要把那句老生常谈的话甩自己脸上：**有问题查文档**。  

然后试着总结下二者的区别。  
直觉上来说，`computed`用于处理依赖的值产生变化而变化的情况；而`watch`用于监听属性值变化并完成后续的逻辑。  
（从这种说法可以看出来，这两者有时候被混淆，就是因为`watch`可以完成所有`computed`可以完成的功能。）  

此外`computed`不能完成异步操作，但是`watch`可以。  

<!-- More -->
## 计算属性

初接触`computed`时会和最早学vue时看到的双向绑定案例想到一起去，但是出于简化`template`的结构和计算逻辑的考虑，使用`computed`去操作数据是有必要的。  

一个简单的例子：

```vue
<template>
    <p>
      Who are you?
    </p>
    <br>
    first name:<input v-model="firstName"/>
    last name:<input v-model="lastName"/>
    <p>Hello, {{fullName}}</p>
</template>

<script>
export default {
  data() {
    return {
      firstName:'',
      lastName:'',
    };
  },
  computed: {
    fullName: {
        get:function() {
            return this.firstName + ' ' + this.lastName;
        }
    }
  }
};
</script>
```

比如这里我在first name的`input`框中输入'Cathy'，在last name框中输入'Tao'，那么下面会随着我输入出现'Hello, Cathy Tao'。  

### computed:getter

本质上，刚才的computed相当于：

```vue
computed: {
    fullName: {
        get:function() {
            return this.firstName + ' ' + this.lastName;
        }
    }
}
```

### computed:setter

有`getter`当然就也有`setter`。但是，set设置属性，并不是修改计算属性，而是**修改它的依赖**。  
对上述例子进行改造：

```vue
<template>
    <p>
      Who are you?
    </p>
    <br>
    first name:<input v-model="firstName"/>
    last name:<input v-model="lastName"/>
    <br>
    <!-- Add Anonymous -->
    <button @click="setAnonymous">Anonymous</button>
    <p>Hello, {{fullName}}</p>
</template>

<script>
export default {
  data() {
    return {
      firstName:'',
      lastName:'',
    };
  },
  methods: {
    //add setAnonymous
    setAnonymous() {
      this.fullName = "A Nobody"
    }
  },
  computed: {
      fullName:{
        get: function() {
          return this.firstName + ' ' + this.lastName;
        },
        // add setter
        set: function(originFullName) {
          let names = originFullName.split(' ');
          this.firstName = names[0];
          this.lastName = names[1];
        }
    }
  }
};
</script>
```

这个例子主要是加入了一个Anonymous按钮，点击这个按钮会调用`setAnonymous`方法，`fullName`会被修改为'A Nobody'。  
computed的`setter`就将`fullName`拆开，把`firstName`和`lastName`改为分别的值。  
这样，一点击Anonymous按钮，输入框中的值就会变为'A'和'Nobody'。

## 侦听属性

### 常规用法

回到最开始的例子：

```vue
<template>
    <p>
      Who are you?
    </p>
    <br>
    first name:<input v-model="firstName"/>
    last name:<input v-model="lastName"/>
    <p>Hello, {{fullName}}</p>
</template>

<script>
export default {
  data() {
    return {
      firstName:'',
      lastName:'',
      fullName:''
    };
  },
  watch: {
    firstName() {
      this.fullName = this.firstName + ' ' + this.lastName;
    },
    lastName() {
      this.fullName = this.firstName + ' ' + this.lastName;
    }
  }
};
</script>
```

这里使用watch监听了`firstName`和`lastName`，并在这两者变化时立即改变`fullName`。  
可以看出，watch监听的值是主动改变而非被动改变的值，而computed是计算被动改变的值。

### 回调用法

把刚才的例子稍作修改：

```vue
<template>
    <p>
      Who are you?
    </p>
    <br>
    first name:<input v-model="firstName"/>
    last name:<input v-model="lastName"/>
    <p>Hello, {{fullName}}</p>
</template>

<script>
export default {
  data() {
    return {
      firstName:'',
      lastName:'',
      fullName:''
    };
  },
  watch: {
    firstName() {
      this.fullName = this.firstName + " " + this.lastName;
    },
    lastName: {
      handler: function() {
        this.fullName = this.firstName + " " + this.lastName;
        this.fullName += "!";
      },
      deep: true
    }
  }
};
</script>
```

emmm这个例子其实不太明显，其实想要表达lastName的内部值可以被监听，一旦产生变化，会在最后插入标点。  
>该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深。

## 参考资料

- [计算属性和侦听器 - Vue.js](https://cn.vuejs.org/v2/guide/computed.html)  
- [API选项/数据:computed - Vue.js](https://cn.vuejs.org/v2/api/#computed)  
- [API选项/数据:watch - Vue.js](https://cn.vuejs.org/v2/api/#watch)