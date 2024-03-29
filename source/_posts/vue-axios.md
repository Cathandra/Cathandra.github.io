---
layout: post
title: "Vue中使用axios异步加载数据"
date: 2020-01-16 19:44:22
tags: 
    - vue
categories: [Front-End]
---

主要归纳2种用法：  

1. 直接使用原生axios；

2. 使用vue-axios组件；

<!-- More -->

## 直接使用原生axios

### 安装axios

使用`npm`安装：

```shell
npm install axios --save
```

或使用`yarn`：

```shell
yarn add axios
```

### 引用axios

首先，axios是不能够像普通的[vue-resource](https://cn.vuejs.org/v2/guide/plugins.html)插件一样，通过在主入口文件引入`import VueResource from vue-resource`后就使用`Vue.use(VueResource)`全局引用。

#### 在页面引用

所以在使用页面不多的情况下，可以直接在相应页面使用。
在相应页面的`<script></script>`中引入：

```html
<script>
import http from 'axios'
</script>
```

#### 改写为Vue的原型属性

在主入口文件`main.js`中引用，然后挂在vue的原型链上：

```html
<script>
import axios from 'axios'
Vue.prototype.http= axios
</script>
```

使用这种方法，在相应页面使用`this.http`就可以发起请求。但是这种方法不太合适。

### 创建实例

#### GET方法

写DEMO的时候，我在`/public`文件夹下放了一个`data.json`文件。所以请求路径就是`'/data.json'`。  
读取数据时，使用`response.data`。

```html
<script>
export default {
  mounted() {
    http.get('/data.json')
    .then(res => {
      console.log(res.data);
    })
    .catch(err => {
      console.log(err);
    })
  }
}
</script>
```

如需要设置参数，可在`get()`方法中设置`params`：

```html
<script>
export default {
  mounted() {
    http.get('/data', {
        params: {
            title: 'test'
        }
    })
    .then(res => {})
    .catch(err => {});
```

#### POST方法

```html
<script>
http.post('/data', {
        params: {
            title: 'test'
        }
  })
    .then(res => {})
    .catch(err => {});
</script>
```

## 使用vue-axios组件

### 安装vue-axios

使用`npm`安装：

```shell
npm i vue-axios
```

或使用`yarn`：

```shell
yarn add vue-axios
```

### 引用vue-axios

在主入口文件`main.js`中引用：

```JavaScript
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios);
```

### 创建实例

#### GET方法

仍旧是在`/public`文件夹下放了一个`data.json`文件。

```html
<script>
export default {
  mounted() {
    this.axios
    .get('/data.json')
    .then(res => {
      console.log(res.data);
    })
    .catch(err => {
      console.log(err);
    })
  }
}
</script>
```

## 参考资料

1. [axios中文网](http://www.axios-js.com/zh-cn/docs/index.html#%E4%BB%80%E4%B9%88%E6%98%AF-axios%EF%BC%9F)

2. [vue-axios](https://github.com/imcvampire/vue-axios)

3. [axios在vue中的使用 - 慕课网](https://www.imooc.com/learn/1152)