---
title:      "Less知识点复习"
date:       2019-04-21
tags: 
    - less
categories: [Front-End]
---

`Less`复习。  
`Less`是一种`CSS`预处理语言，规避了`CSS`的逻辑性短板。个人认为，`Less`主要是对`CSS`添加了变量、混合与函数三个板块的功能，藉此简化开发工作。  
<del>那下面我就又要开始罗列知识点了。<del>

<!--more-->

## 变量

Less中定义的变量使用`@`开头，引用时直接使用`@variable_name`。

### 值变量

与其称之为变量，不如把变量理解为一个常量，因为其值本身就是不可以重新定义的。  
这样，在编写样式文件时，对于常用变量，如主色调、固定的长度等等，可以直接进行通过变量名引用，不必重新写出值。

```less
@lightPrimaryColor : #fff;

.nav__item--active{
    color: @lightPrimaryColor;
}
```

其他还有属性变量、选择器变量等。为了上下思路连贯，这里就不多写了。

### 变量运算

变量虽然不可修改，但是less支持对变量进行运算。在加减法中，单位自动与第一个变量统一。

```less
@width:300px;

#wrap{
    width:@width-20;
    height:@width-20*5;
    margin:(@width-20)*5;
}
```

### 混合

混合实质上可以理解为对方法的使用，所以归在下一节中。

## 方法

方法直接可以和`声明的集合`一样定义，但是为了作区分，最好定义时在方法后写 `()`。

```less
.item { // 等价于.item()
    width: 330px;
    box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
```

方法可以设置默认参数，然后在调用时直接使用或修改。

```less
.border(@color:#000){
    border:solid 1px @color;
}
```

### @arguments——全部参数

`@arguments`和很多语言中一样，指代的是全部参数。

```less
.border(@a:10px,@b:50px,@c:30px,@color:#000){
    border:solid 1px @color;
    box-shadow: @arguments;
}
```

### 匹配模式

可以和`多态`概念一样理解。

### extend关键字——继承

使用`extend`可以继承所匹配声明中的全部样式。

```less
.animation {
    transition: all .3s ease-out;
    .hide {// 嵌套
        transform:scale(0);
    }
}
#main {
    &:extend(.animation);// 继承
}
```

### 混合模式

混合模式指，在需要使用该方法的元素样式中直接引用该方法。

```less
.item() { 
    width: 330px;
    box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
#card {
    .item();
}
```

## &关键字——嵌套模式

使用`&`代表上一层选择器的名字。  
嵌套与继承、与混合的区别在于，继承指继承所匹配声明中的全部样式、混合是引用了匹配样式，嵌套只是代表上一层选择器的名字。

```less
li {
    &: hover {
        color: #aaa;
    }
}
```

## @media——媒体查询

less的媒体查询可以直接在元素内编写，每一个元素都会编译出自己 `@media` 声明。

```less
.nav {
    width: 960px;
    @media screen{
        @media (max-width:768px) {
            width: 660px;
        }
    }
}
```

## !important关键字

`!important`关键字指对于重复规则强制使用。在less中直接在方法名后加关键字即可。

```less
.border {
    border: solid 1px red;
    margin: 50px;
}

#box {
    .border() !important;
}
```

## 其他

虽然不属于语言特性但好像还是应该提一下。

### 在Node.js环境中使用Less

```
npm install -g less
> lessc styles.less styles.css
```

编译后，`.less`文件的内容会以符合css语法的形式出现在同名的`.css`文件中。

## 注释和避免编译

`/* */`是CSS原生注释，会被编译在 CSS 文件中；  
`//` 是Less提供的注释，不会被编译在 CSS 文件中。
