

# Spring框架

## 概述

### 依赖注入（DI）

Spring 最认同的技术是控制反转的依赖注入（DI）模式。

控制反转（IoC）是一个通用的概念，它可以用许多不同的方式去表达，依赖注入仅仅是控制反转的一个具体的例子。

应该尽可能的独立于其他的 Java 类来增加这些类可重性。

依赖注入（或者有时被称为配线）有助于将这些类粘合在一起，并且在同一时间让它们保持独立。

#### 到底什么是依赖注入？

依赖。类 A 依赖于类 B

注入。所有这一切都意味着类 B 将通过 IoC 被注入到类 A 中

依赖注入可以是向构造函数传递参数的方式发生，或者通过使用 setter 方法 post-construction

#### 面向方面的程序设计（AOP）：

在 面向对象（OOP） 中模块化的关键单元是类，而在 AOP 中模块化的关键单元是方面。

AOP 帮助你将横切关注点从它们所影响的对象中分离出来，然而依赖注入帮助你将你的应用程序对象从彼此中分离出来。

## Spring的体系结构

### 核心容器

核心容器由核心，Bean，上下文和表达式语言模块组成，它们的细节如下：

- **核心**模块提供了框架的基本组成部分，包括 IoC 和依赖注入功能。
- **Bean** 模块提供 BeanFactory，它是一个工厂模式的复杂实现。
- **上下文**模块建立在由核心和 Bean 模块提供的坚实基础上，它是访问定义和配置的任何对象的媒介。ApplicationContext 接口是上下文模块的重点。
- **表达式语言**模块在运行时提供了查询和操作一个对象图的强大的表达式语言。

### 数据访问/集成

数据访问/集成层包括 JDBC，ORM，OXM，JMS 和事务处理模块，它们的细节如下：

- **JDBC** 模块提供了删除冗余的 JDBC 相关编码的 JDBC 抽象层。
- **ORM** 模块为流行的对象关系映射 API，包括 JPA，JDO，Hibernate 和 iBatis，提供了集成层。
- **OXM** 模块提供了抽象层，它支持对 JAXB，Castor，XMLBeans，JiBX 和 XStream 的对象/XML 映射实现。
- Java 消息服务 **JMS** 模块包含生产和消费的信息的功能。
- **事务**模块为实现特殊接口的类及所有的 POJO 支持编程式和声明式事务管理。

### Web

Web 层由 Web，Web-MVC，Web-Socket 和 Web-Portlet 组成，它们的细节如下：

- **Web** 模块提供了基本的面向 web 的集成功能，例如多个文件上传的功能和使用 servlet 监听器和面向 web 应用程序的上下文来初始化 IoC 容器。
- **Web-MVC** 模块包含 Spring 的模型-视图-控制器（MVC），实现了 web 应用程序。
- **Web-Socket** 模块为 WebSocket-based 提供了支持，而且在 web 应用程序中提供了客户端和服务器端之间通信的两种方式。
- **Web-Portlet** 模块提供了在 portlet 环境中实现 MVC，并且反映了 Web-Servlet 模块的功能。

### 其他

还有其他一些重要的模块，像 AOP，Aspects，Instrumentation，Web 和测试模块，它们的细节如下：

- **AOP** 模块提供了面向方面的编程实现，允许你定义方法拦截器和切入点对代码进行干净地解耦，它实现了应该分离的功能。
- **Aspects** 模块提供了与 **AspectJ** 的集成，这是一个功能强大且成熟的面向切面编程（AOP）框架。
- **Instrumentation** 模块在一定的应用服务器中提供了类 instrumentation 的支持和类加载器的实现。
- **Messaging** 模块为 STOMP 提供了支持作为在应用程序中 WebSocket 子协议的使用。它也支持一个注解编程模型，它是为了选路和处理来自 WebSocket 客户端的 STOMP 信息。
- **测试**模块支持对具有 JUnit 或 TestNG 框架的 Spring 组件的测试。

## IOC容器

### BeanFactory 容器

### ApplicationContext 容器

## Bean

bean 是一个被实例化，组装，并通过 Spring IoC 容器所管理的对象。这些 bean 是由用容器提供的配置元数据创建的。如在XML 的表单中的 定义。

| 属性                       | 描述                                       |
| ------------------------ | ---------------------------------------- |
| class                    | 这个属性是强制性的，并且指定用来创建 bean 的 bean 类。        |
| name                     | 这个属性指定唯一的 bean 标识符。 可以使用 ID 和/或 name 属性  |
| scope                    | bean 定义创建的对象的作用域                         |
| constructor-arg          |                                          |
| properties               |                                          |
| autowiring mode          |                                          |
| lazy-initialization mode | 延迟初始化的 bean。 告诉 IoC 容器在它第一次被请求时，而不是在启动时去创建一个 bean 实例。 |
| initialization 方法        | 在 bean 的所有必需的属性被容器设置之后，调用回调方法。           |
| destruction 方法           | 当包含该 bean 的容器被销毁时，使用回调方法。                |

## Bean 的作用域

当在 Spring 中定义一个bean 时，你必须声明该 bean 的作用域。

### prototype

为了强制 Spring 在每次需要时都产生一个新的 bean 实例，你应该声明 bean 的作用域的属性为 prototype

### singleton

如果你想让 Spring 在每次需要时都返回同一个bean实例，你应该声明 bean 的作用域的属性为 singleton

```xml
<!-- A bean definition with singleton scope -->
<bean id="..." class="..." scope="singleton">
    <!-- collaborators and configuration for this bean go here -->
</bean>
```

## Bean 的生命周期

为了定义安装和拆卸一个 bean，我们只要声明带有 **init-method** 和/或 **destroy-method** 参数的实例 。

init-method 属性指定一个方法，在实例化 bean 时，立即调用该方法。

destroy-method 指定一个方法，只有从容器中移除 bean 之后，才能调用该方法。

### 第一种方法：

初始化工作可以在 afterPropertiesSet() 方法中执行

```java
impport org.springframework.beans.factory.InitializingBean;
public class ExampleBean implements InitializingBean {
   public void afterPropertiesSet() {
      // do some initialization work
   }
}
```

### 第二种方法

```xml
<bean id="exampleBean" 
         class="examples.ExampleBean" init-method="init"/>
```

```java
public class ExampleBean {
   public void init() {
      // do some initialization work
   }
}
```

## Bean 后置处理器

### BeanPostProcessor

```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.tutorialspoint.HelloWorld"
       init-method="init" destroy-method="destroy">
       <property name="message" value="Hello World!"/>
   </bean>

   <bean class="com.tutorialspoint.InitHelloWorld" />

</beans>
```

ApplicationContext.getBean()的执行顺序是这样的：

main方法的package为 package com.tutorialspoint;

监测到bean xml中有如下：

-   **<bean class="com.tutorialspoint.InitHelloWorld" />**
-   init-method="init"
-   destroy-method="destroy"

所以，执行package com.tutorialspoint 中的main方法时，顺序为：

- 继承BeanPostProcessor 的InitHelloWorld方法中的postProcessBeforeInitialization方法
- id="helloWorld"class实例中的init方法
- 继承BeanPostProcessor 的InitHelloWorld方法中的postProcessAfterInitialization方法
- 根据逻辑调用helloworld中的方法
- id="helloWorld"class实例中的destroy方法

### 代码模拟

xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.tutorialspoint.HelloWorld"
       init-method="init" destroy-method="destroy">
       <property name="message" value="Hello World!"/>
   </bean>

   <bean class="com.tutorialspoint.InitHelloWorld" />

</beans>
```

Main:

```java
package com.tutorialspoint;
import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      AbstractApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
      obj.getMessage();
     //在 AbstractApplicationContext 类中声明的关闭 hook 的 registerShutdownHook() 方法。它将确保正常关闭，并且调用相关的 destroy 方法
      context.registerShutdownHook();
   }
}
```

HelloWorld.java:

```java
package com.tutorialspoint;
public class HelloWorld {
   private String message;
   public void setMessage(String message){
      this.message  = message;
   }
   public void getMessage(){
      System.out.println("Your Message : " + message);
   }
   public void init(){
      System.out.println("Bean is going through init.");
   }
   public void destroy(){
      System.out.println("Bean will destroy now.");
   }
}
```

InitHelloWorld.java:

```java
package com.tutorialspoint;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.beans.BeansException;
public class InitHelloWorld implements BeanPostProcessor {
   public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
      System.out.println("BeforeInitialization : " + beanName);
      return bean;  // you can return any other object as well
   }
   public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
      System.out.println("AfterInitialization : " + beanName);
      return bean;  // you can return any other object as well
   }
}
```

result:

```java
BeforeInitialization : helloWorld
Bean is going through init.
AfterInitialization : helloWorld
Your Message : Hello World!
Bean will destroy now.
```

## Bean 定义继承

类似java里的继承一样，可以在xml文件中定义两个class文件的继承关系。

如下：

```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.tutorialspoint.HelloWorld">
      <property name="message1" value="Hello World!"/>
      <property name="message2" value="Hello Second World!"/>
   </bean>

   <bean id="helloIndia" class="com.tutorialspoint.HelloIndia" parent="helloWorld">
      <property name="message1" value="Hello India!"/>
      <property name="message3" value="Namaste India!"/>
   </bean>

</beans>
```

1. helloWorld两个属性：message1，message2
2. helloIndia，parent="helloWorld"，代表是helloworld的子类
3. helloIndia的message1将会重写helloWorld的message1，新增message3属性

## 依赖注入

标准代码：

```java
public class TextEditor {
   private SpellChecker spellChecker;  
   public TextEditor() {
      spellChecker = new SpellChecker();
   }
}
```
在这里，TextEditor 不应该担心 SpellChecker 的实现。

SpellChecker 将会独立实现，并且在 TextEditor 实例化的时候将提供给 TextEditor，整个过程是由 Spring 框架控制。

在这里，我们已经从 TextEditor 中删除了全面控制，并且把它保存到其他地方（即 XML 配置文件），且依赖关系（即 SpellChecker 类）通过**类构造函数**被注入到 TextEditor 类中。因此，控制流通过依赖注入（DI）已经“反转”，因为你已经有效地委托依赖关系到一些外部系统。

```java
public class TextEditor {
   private SpellChecker spellChecker;
   public TextEditor(SpellChecker spellChecker) {
      this.spellChecker = spellChecker;
   }
}
```

### 基于构造函数的依赖注入

