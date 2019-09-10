---
title: JAVAEE读书笔记（二）
date: 2019-09-04 07:24:34
categories: JAVAEE读书笔记
tags: 学习
---

## SpringBean

### Bean的配置

​		Spring用于生产和管理Spring容器中的Bean。如果要使用这个工厂生产和管理Bean，需要开发者将Bean配置在Spring的配置文件中。Spring框架支持Xml和Properties两种格式的配置文件，在实际开发中常用XML格式的配置文件。

​                                                               **\<bean>元素的常用属性及其子元素**



| 描述               | 属性或子元素名称                                             |
| ------------------ | ------------------------------------------------------------ |
| id                 | Bean在BeanFactory中的唯一标识，在代码中通过BeanFactory获取Bean实例时需要以此作为索引名称 |
| class              | Bean的具体实现类，使用类的名（例如dao.TestDIDaoImpl）        |
| scope              | 指定Bean实例的作用域                                         |
| \<constructor-arg> | \<bean>元素的子元素，使用构造方法注入，指定构造方法的参数。该元素的index属性指定参数的序号，ref属性指定对BeanFactory中其他Bean的引用关系，type属性指定参数类型，value属性指定参数的常量值 |
| \<property>        | \<bean>元素的子元素用于设置一个属性，该元素的name属性指定Bean实例中相应的属性名称，value属性指定Bean的属性值，ref属性指定属性对BeanFactory中其他Bean的引用关系 |
| \<list>            | \<property>元素的子元素，用于封装List或数组类型的依赖注入    |
| \<map>             | \<property>元素的子元素，用于封装Map类型的依赖注入           |
| \<set>             | \<property>元素的子元素，用于封装Set类型的依赖注入           |
| \<entry>           | \<map>元素的子元素，用于设置一个键值对                       |

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="accountService" class="com.service.impl.AccountServiceImpl">
        <constructor-arg ref="accountDao"></constructor-arg>
    </bean>
    <bean id="accountDao" class="com.dao.impl.AccountDaoImpl"></bean>
</beans>
```

### Bean的实例化

​		在Spring框架中，如果想使用Spring容器中的Bean，也需要实例化Bean，Spring框架实例化Bean有三种方式

即，构造方法实例化、静态工厂实例化和实例工厂实例化（最常用就是构造方法实例化）。

#### 1. 构造方法实例化

![1567505009778](/upload/JAVAEE读书笔记/结构.png)

spring-config.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="accountService" class="com.service.impl.AccountServiceImpl">
        <constructor-arg ref="accountDao"></constructor-arg>
    </bean>
    <bean id="accountDao" class="com.dao.impl.AccountDaoImpl"></bean>
</beans>
```

测试类

```java
package com.test;

import com.service.AccountService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-config.xml");
        AccountService accountService = (AccountService)applicationContext.getBean("accountService");
        accountService.addAccountMoney("张三",1000f);
    }
}
```

#### 2. 静态工厂实例化对象（开发中不使用，再次不列举使用方法）

#### 3. 实例工厂实例化对象（这个也不列举）

### Bean的作用域

| 作用域名称  | 描述                                                         |
| :---------- | ------------------------------------------------------------ |
| singleton   | 默认的作用域，使用singleton定义的Bean在Spring容器中只有一个Bean实例 |
| prototype   | Spring容器每次获取protopyte定义的Bean，容器都将创建一个新的Bean实例 |
| request     | 在一次Http请求中容器将返回一个Bean实例，不同的Http请求返回不同的Bean实例。仅在Web Spring应用程序上下文中使用 |
| session     | 在一个HTTP Session 中，容器将返回同一个Bean实例。尽在Web Spring应用程序上下文中使用 |
| application | 为每个ServletContext对象创建一个实例，即同一个应用共享一个Bean实例，尽在Web Spring应用程序上下文中使用 |
| websocket   | 为每个WebSocket对象创建一个Bean实例。仅在Web Spring应用程序上下文中使用 |

#### 1. singleton作用域

​		由于singleton是bean的scope默认设置，所以写好bean之后就可以进行测试

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="accountService" class="com.test.AccountServiceImpl" scope="singleton"></bean>
</beans>
```

代码如下

```java
package com.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-config.xml");
        //第一个对象
        AccountServiceImpl a1 = (AccountServiceImpl)applicationContext.getBean("accountService");
        //第二个对象
        AccountServiceImpl a2 = (AccountServiceImpl)applicationContext.getBean("accountService");
        System.out.println(a1 == a2);
    }
}

```

结果展示：

![1567506824579](/upload/JAVAEE读书笔记/singleton结果展示.png)

#### 2. prototype作用域

代码如上所述，然后只设置scope作用域即可

结果展示:

![1567506916588](/upload/JAVAEE读书笔记/prototype结果展示.png)



### Bean的生命周期

​		一个对象的生命周期包括创建（实例化与初始化）、使用以及销毁等阶段，在Spring中，Bean对象周期也遵循这一过程，但是Spring提供了许多对外接口，允许开发者对3个过程（实例化、初始化、销毁）的前后做一些操作。在Spring Bean中，实例化是为Bean对象开辟空间，初始化则是对属性的初始化。

​		Bean生命周期整个过程如下：

​		（1）根据Bean的配置情况实例化一个Bean。

​		（2）根据Spring上下文对实例化的Bean进行依赖注入，即对Bean的属性进行初始化。

​		（3）如果Bean实现了BeanNameAware 接口，将调用它实现的，setBeanName（String beanId）方法，

此处参数传递的是Spring配置文件中Bean的id。

​		（4）如果Bean实现BeanFactoryAware接口，将调用它实现的setBeanFactory方法，此处参数传递的是当前Spring工厂实例的引用。

​		（5）如果Bean实现了ApplicationContextAware接口，将调用它实现的setApplicationContext（ApplicationContext）方法，此处参数传递的是Spring上下文实例的引用。

​		（6）如果Bean关联了BeanPostProcessor接口，将调用初始化方法postProcessBeforeInitialization（Object obj， String s）对Bean进行操作。

​		（7）如果Bean实现了InitializingBean接口， 将调用afterPropertiesSet方法。

​		（8）如果Bean在Spring配置文件中配置了  init-method 属性，将自动调用其配置的初始化方法。

​		（9）如果Bean关联了BeanPostProcessor接口，将调用postProcessAfterInitialization（Object obj，String s）方法，由于是在Bean初始化结束时调用After方法，也可用于内存或缓存技术。

​		（10）当Bean不再需要时将进入销毁阶段，如果Bean实现了DisposableBean接口，则调用其实现的destroy方法将Spring中的Bean销毁。

​		（11）如果在配置文件中通过destory-method属性指定了Bean的销毁方法，将调用其配置的销毁方法进行销毁。

​		在Spring中，通过特定的接口或通过\<bean>元素的属性设置可以对Bean的生命周期过程产生影响。

​		例子：

​		**创建Bean的实现类**

```java
package com.test;

/**
 * @author wys
 */
public class BeanLife {
    public void initMyself(){
        System.out.println(this.getClass().getName()+"执行了自定义的初始化方法");
    }
    public void destroyMyself() {
        System.out.println(this.getClass().getName()+"执行了自定义的销毁方法");
    }
}

```

​		**配置Bean**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--配置bean，使用init-method指定初始化方法，使用destroy-method指定销毁方法-->
    <bean id="beanLife" class="com.test.BeanLife" init-method="initMyself" destroy-method="destroyMyself"></bean>
</beans>
```

​		**测试生命周期**

```java
package com.test;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        //初始化Spring容器，加载配置文件
        //为了方便演示，用的是ClassPathXmlApplicationContext
        //实现声明类容器
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("spring-config.xml");
        System.out.println("获得对象前");
        BeanLife beanLife = (BeanLife)ctx.getBean("beanLife");
        System.out.println("获得对象后"+beanLife);
        ctx.close();
    }
}

```

结果如下:

![Bean的生命周期结果展示](/upload/JAVAEE读书笔记/Bean的生命周期结果展示.png)