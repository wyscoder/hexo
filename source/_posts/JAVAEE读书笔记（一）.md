---
title: JAVAEE读书笔记（一）
date: 2019-08-10 15:21:10
categories: JAVAEE读书笔记
tags: 学习
---
## Spring简介

#### 1. Spring由来

- 目的是为了解决企业级应用开发的业务逻辑和其他各层的耦合问题

#### 2. Spring体系结构

- 核心容器（Core Container）
- 数据访问/ 集成（Data Access/Integration）层
- Web层
- AOP（Aspect Oriented Programming，面向切面的编程）模块
- 植入（Instrumentation）模块
- 消息传输（Messaging）
- 测试（Test）
- Spring体系结构

![Spring结构](/upload/JAVAEE读书笔记/Spring结构.jpg)



## SpringIOC

### 1. Spring IOC基本概念

控制反转是一个比较抽象的概念，是Spring框架的核心，用来削减计算机程序的耦合问题。

依赖注入是IOC的另外一种说法。

**解释**

​	当某个java对象需要调用另一个对象时，在传统编程模式下，调用者通常会采用 "new 被调用者" 的方式来创建对象，这种方式会增加调用者和被调用者之间的耦合性，不利于后期代码的维护。

​	当Spring框架出现后，对象的实例不再由调用者来创建，而是由Spring容器来创建。Spring容器会负责控制程序之间的关系，这样，控制权由调用者转移到Spring容器，控制权发生了反转，这就是Spring的控制反转。

​	从Spring容器角度来看，Spring容器负责将被依赖对象赋值给调用者的成员变量，相当于为调用者注入他所依赖的实例，这就是Spring依赖注入。

​	综上所述，控制反转是一种通过描述（在Spring中可以是XML或注解）并通过第三方产生或获取特定对象的方式。在Spring中实现控制反转的是IOC容器，其实现方式是依赖注入。

### 2. SpringIOC容器

SpringIOC容器的设计主要是基于BeanFactory和ApplicationContext两个接口。

#### **BeanFactory**

##### (1).  **概念**

​	BeanFactory由org.springframework.beans.factory.BeanFactory接口定义，它提供了完整的Ioc服务支持，是一个管理BeanFactory的工厂，主要负责初始化各种Bean。BeanFactory接口有许多实现类，比较常用的就是org.springframework.beans.factory.xml.XmlBeanFactory.这个类会根据XML来装配Bean，创建时候需要提供XML文件的绝对路径。

```java
public static void main(String[] args){
    //初始化Spring容器，加载配置文件
    BeanFactory beanFactory = new XmlBeanFactory(new FileSystemResource("绝对路径"));
    TestDao testDao = (TestDao)beanFactory.getBean("testDao");
    testDao.sayHello();
}
```

这种写法开发中一般不常见，了解即可。

#### **ApplicationContext**

​	ApplicationContext是BeanFactory的子接口，也称为应用上下文，org.springwork.context.ApplicationContext接口定义，ApplicationContext接口除了包含BeanFactory的所有功能外，还添加了国际化，资源访问，事件传播等内容的支持。

​	创建ApplicationContext接口实例通常有以下三种方式:

##### (1). **通过ClassPathXmlApplicationContext创建 **：

​	ClassPathXmlApplicationContext将类路径目录（src根目录）中寻找指定的XML配置文件，代码如下：

```java
public static void main(String[] args){
    //初始化Spring容器ApplicationContext,加载配置文件
    ApplicationContext ac = ClassPathXmlApplicationContext("spring-config.xml");
    //通过容器获取实例对象
    TestDao testDao = (TestDao)ac.getBean("testDao");
    testDao.sayHello();
}
```

##### (2). 通过FileSystemXmlApplicationContext创建：

​	FileSystemXmlApplicationContext将从指定文件的绝对路径中寻找XML配置文件，找到并装载完成ApplicationContext的实例化工作。代码如下：

```java
public stsatic void main(String[] args){
    //初始化Spring容器ApplicationContext，加载配置文件
    ApplicationContext ac = new FileSystemXmlApplication("C:\Users\hp\IdeaProjects\my_pratices_account_day_01\src\main\resources\spring-config.xml");
    //获取实例对象
    TestDao testDao = (TestDao)ac.getBean("testDao");
    testDao.sayHello();
}
```

采用绝对路径的加载方式将导致程序的灵活性变差，一般不推荐使用。

##### (3). 通过Web服务器实例化ApplicationContext容器

​	在Web服务器实例化ApplicationContext容器时，一般使用org.springframework.web.context.ContextLoaderListener的实现方式（需要导入spring-web.5.0.2.RELEASE.jar包），此方法只需在web.xml中添加如下代码：

```xml
<context-param>
	<!--加载src目录下的spring-config.xml-->
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-config.xml</param-value>
</context-param>
<!--指定以ContextLoaderListener方式启动Spring容器-->
<listener>
    <listener-class>
   		org.springframework.web.context.ContextLoaderListener
    </listener-class>
</listener>
```

### 3. 依赖注入的类型

#### 构造方法注入

Spring框架可以采用java的反射机制，通过构造方法完成依赖注入

首先创建持久层接口

```java
package com.test.dao;

/**
 * 创建持久层
 */
public interface TestDao {
    /**
     * Hello方法
     */
    void sayHello();
}

```

然后创建持久层实现类

```java
package com.test.dao.impl;

import com.test.dao.TestDao;

public class TestDaoImpl implements TestDao {

    @Override
    public void sayHello(){
        System.out.println("Hello World");
    }
}

```

创建业务层接口

```java
package com.test.service;

/**
 * 业务层
 */
public interface TestService {
    void sayHello();
}

```

创建业务层实现类

```java
package com.test.service.impl;

import com.test.dao.TestDao;
import com.test.service.TestService;

/**
 * 业务层接口
 */
public class TestServiceImpl implements TestService {
    private TestDao testDao;
    //使用构造方法注入
    public TestServiceImpl(TestDao testDao){
        this.testDao = testDao;
    }
    @Override
    public void sayHello() {
        testDao.sayHello();
    }
}

```

配置spring-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="testService" class="com.test.service.impl.TestServiceImpl">
        <constructor-arg ref="testDao"></constructor-arg>
    </bean>
    <bean id="testDao" class="com.test.dao.impl.TestDaoImpl"></bean>
</beans>
```

最后在测试类中测试一下

```java
package com.test;


import com.test.service.TestService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("spring-config.xml");
        TestService testService = (TestService)ac.getBean("testService");
        testService.sayHello();
    }
}

```

然后看一下结果

![1564378188976](/upload/JAVAEE读书笔记/1564378188976.png)

以上就是构造注入

#### 属性的setter注入

和上面创建一样，只需要更改一下TestServiceImpl中的代码和spring-config.xml就行

```java
package com.test.service.impl;

import com.test.dao.TestDao;
import com.test.service.TestService;

/**
 * 业务层接口
 */
public class TestServiceImpl implements TestService {
    private TestDao testDao;
    //使用构造方法注入

    public void setTestDao(TestDao testDao) {
        this.testDao = testDao;
    }

    @Override
    public void sayHello() {
        testDao.sayHello();
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="testService" class="com.test.service.impl.TestServiceImpl">
        <property name="testDao" ref="testDao"></property>
    </bean>
    <bean id="testDao" class="com.test.dao.impl.TestDaoImpl"></bean>
</beans>
```

### 4. 课后习题

1. 举例说明IOC容器的实现方式有哪些？

   控制反转和依赖注入

2. spring中什么是控制反转？什么是依赖注入？使用控制反转与依赖注入的优点？

   控制反转：是一种通过描述并通过第三方去产生去获取特定对象的方式

   依赖注入：使用spring框架创建对象时动态的将其所依赖的对象注入到Bean组件中。

   控制反转优点：

   ​	1.获取对象可以通过注解等方式获取对象，打破传统的获取方式

   ​	2.对象不再由程序本身进行创建，而是交给spring容器创建，降低了程序的耦合性

   ​	3.控制反转能做到更多的事情，例如事务控制

   ​	4.后期维护方便

   依赖注入优点：

   ​	1.项目开发讲究高内聚，低耦合

   ​	2.使用依赖注入可以避免使用new关键字创建对象，从而降低类与类之间的耦合度

3. spring框架采用java的**<u>反射机制</u>**进行依赖注入.


