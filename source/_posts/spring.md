---
title: Spring 初学习
date: 2017-10-22 21:41:52 #文章生成时间，一般不改，当然也可以任意修改
tags: 
    - spring
categories: [Back-End]
---
Spring学习笔记。

## Spring基本特征

- 控制反转（ioc）：
  - 即不需要像原生JAVA一样，在创建类对象时通过类去new一个方法，而是交给Spring配置对象
- 面向切面（aop）：
  - 将业务逻辑与系统结构分离
- 容器：
  - 包含于管理对象的生命周期
- 一站式框架,即在三层结构中有各自解决方法：
  - web层：Spring MVC
  - service层：Spring的ioc
  - dao层：Spring的异常层次结构

<!-- more -->

## ioc底层原理使用技术

- 工厂设计模式
  - 原理
    - 以抽象类提供公共的实现
    - xml配置文件控制注入
  - 分类
    - 简单工厂模式
    - 抽象工厂模式
  - 步骤
    - 1、配置xml文件，创建对象类
    - 2、创建工厂类，使用dom4j解析配置文件+反射
      - 使用dom4j解析xml文件
      - 使用反射创建类对象
    - 解决配置文件不显示问题：
      - spring引入schema约束，将约束文件引入eclipse
        - 具体方法：将约束路径复制入xml cataloge中，选择key type为schema location
        - 在src下面新建file，名为log4j.properties，配置好内容

## 基于xml文件的配置方式

### bean实例化的方式

#### 使用类的无参数构造创建

```xml
    <!-- bean.xml -->
    <bean id="user" class="iocLearning.User"></bean>
```

```Java
    <!-- User.java -->
    public class User {
	    private String username;
	    public User(String username) {
		    this.username = username;
    	}
	    public User() {
		    super();
    	}
    }
```

```java
    <!-- UserIoc.java -->
    public class UserIoc {
	    @Test//表示单元测试
	    public void testUser() {
		    //1、加载spring配置文件，创建对象
    		ApplicationContext context = new ClassPathXmlApplicationContext ("bean.xml");
		    //2、得到配置创建的对象
		    User user = (User) context.getBean("user");
		    System.out.println(user);
		    user.add();
    	}
    }
```

#### 使用静态工厂创建：创建静态方法，返回类对象

```xml
	<!-- bean2.xml -->
	<bean id="bean2" class="beanLearning.Bean2Factory" factory-method = "getBean2"></bean>
```
```java
    <!-- Bean2.java -->
    public class Bean2 {
	    public void add() {
		    System.out.println("Bean2 has been added successfully too!!2333";
    	}
    }
```
```java
    <!-- Bean2Factory.java -->
    public class Bean2Factory {
	//使用静态方法，返回Bean2对象
	    public static Bean2 getBean2() {
		    return new Bean2();
    	}
    }
```
```java
    <!-- UserIoc2.java -->
    public class UserIoc2 {
	@Test
	    public void testUser() {
		    //1、加载spring配置文件，创建对象
    		ApplicationContext context = new ClassPathXmlApplicationContext("bean2.xml");
		    //2、得到配置创建的对象
		    Bean2 bean2 =(Bean2) context.getBean("bean2");
		    System.out.println(bean2);
    	}
    }
```

#### 使用实例工厂创建

```xml
	<!-- bean3.xml -->
	<bean id="Bean3Factory" class="beanLearning.Bean3Factory"></bean>
	<bean id="bean3" factory-bean="Bean3Factory" factory-method="getBean3"></bean>
```
```java
    <!-- Bean3.java -->
    public class Bean3 {
	    public void add() {
		    System.out.println("Bean3 has been added successfully too!!2333");
	    }
    }
```
```java
    <!-- Bean3Factory.java -->
    public class Bean3Factory {
	    //普通方法，返回Bean3对象
	    public Bean3 getBean3() {
		    return new Bean3();
    	}
    }
```
```java
    <!-- UserIoc3.java -->
    public class UserIoc3 {
	    @Test
	    public void testUser() {
		    //1、加载spring配置文件，创建对象
		    ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml");
    		//2、得到配置创建的对象
	    	Bean3 bean3 =(Bean3) context.getBean("bean3");
		    System.out.println(bean3);
	    }
    }
```

### bean标签常用属性

- `id`：由id值得到配置对象
- `class`：创建对象所在类的全路径
- `name`：与id功能相同，但是id不能含特殊符号而name可以
- `scope`：界定bean的作用范围
- `singleton`：默认值，单例的；
- `prototype`：多例的；
- `request`：Spring创建的一个Bean对象会被存入request域中；
- `session`：Spring创建的一个Bean对象会被存入session域中；
- `globalSession`：应用在Porlet环境，否则相当于session；

## 基于注解(annotation)的配置方式

- 属性注入方式
  - spring注入属性：有参构造和set方法
  - 注入对象类型属性
  - p名称空间注入
  - 注入复杂数据