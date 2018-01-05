# Spring

## BeanFactory

spring的IOC容器能够帮我们自动new对象，对象交给spring管之后我们不用自己手动去new对象了。那么它的原理是什么呢？是怎么实现的呢？下面我来简单的模拟一下spring的机制，相信看完之后就会对spring的原理有一定的了解。

spring使用BeanFactory来实例化、配置和管理对象，但是它只是一个接口，里面有一个getBean()方法。我们一般都不直接用BeanFactory，而是用它的实现类ApplicationContext，这个类会自动解析我们配置的applicationContext.xml，然后根据我们配置的bean来new对象，将new好的对象放进一个Map中，键就是我们bean的id,值就是new的对象。

示例模拟代码：

### 模拟Spring BeanFactory 实现代码

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
2 <beans>
3     <bean id="c" class="com.spring.Car"></bean>
4      <bean id="p" class="com.spring.Plane"></bean>
5 </beans>
```

BeanFactory(interface)

```java
package com.spring;

public interface BeanFactory {
    Object getBean(String id);
}
```

ClassPathXmlApplicationContext(implements BeanFactory)

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;


public class ClassPathXmlApplicationContext implements BeanFactory {
    private Map<String, Object> beans = new HashMap<String, Object>();
    public ClassPathXmlApplicationContext(String fileName) throws Exception{
        SAXReader reader = new SAXReader();
        Document document = reader.read(this.getClass().getClassLoader().getResourceAsStream(fileName));
        List<Element> elements = document.selectNodes("/beans/bean");
      	//遍历xml文件中，节点/beans/bean，下的id和class类名。根据类名，使用newInstance()方法，创建类的实例
        for (Element e : elements) {
            String id = e.attributeValue("id");
            String value = e.attributeValue("class");
            Object o = Class.forName(value).newInstance();
            beans.put(id, o);
        }
    }
    
    public Object getBean(String id) {
        return beans.get(id);
    }

}
```

### 模拟使用
定义使用类

```java
public interface Moveable {
    void run();
}

public class Car implements Moveable{
    
    public void run(){
        System.out.println("拖着四个轮子满街跑car·····");
    }
}

public class Plane implements Moveable{

    public void run() {
        System.out.println("拖着翅膀天空飞plane......");
    }
    
}
```

main方法

```java
package com.spring;

import org.dom4j.DocumentException;

public class Test {

    /**
     * @param args
     * @throws DocumentException 
     */
    public static void main(String[] args) throws Exception {
        BeanFactory factory = new ClassPathXmlApplicationContext("applicationContext.xml");
        Object o = factory.getBean("c");
        Moveable m = (Moveable)o;
        m.run();
    }

}
```

## ApplicationContext

ApplicationContext的初始化和BeanFactory有一个重大的区别：BeanFactory在初始化容器时，并未实例化Bean，直到第一次访问某个Bean时才实例目标Bean；而ApplicationContext则在初始化应用上下文时就实例化所有单实例的Bean。因此ApplicationContext的初始化时间会比BeanFactory稍长一些，不过稍后的调用则没有"第一次惩罚"的问题。

## Annotion(注解)

### @PostConstruct

被@PostConstruct修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器调用一次，类似于Serclet的inti()方法。

被@PostConstruct修饰的方法会在构造函数之后，init()方法之前运行。

用来修饰一个非静态的void()方法。

写法有如下两种方式：

```java
@PostConstruct
Public void someMethod() {}
```

```java
public @PostConstruct void someMethod(){}
```

### @PreDestroy

被@PreConstruct修饰的方法会在服务器卸载Servlet的时候运行，并且只会被服务器调用一次，类似于Servlet的destroy()方法。

被@PreConstruct修饰的方法会在destroy()方法之后运行，在Servlet被彻底卸载之前。

### @Autowired 

自动装配

### @Component

通过@Component将切面定义为Spring管理Bean

### @Lazy

spring配置文件中的beans默认情况下是及时加载的，有时候一些类加载耗时很厉害，我们可以通过配置将其设置为延迟加载。

ApplicationContext实现的默认行为就是在启动时将所有singleton bean提前进行实例化。提前实例化意味着作为初始化过程的一部分，ApplicationContext实例会创建并配置所有的singleton bean。通常情况下这是件好事，因为这样在配置中的任何错误就会即刻被发现（否则的话可能要花几个小时甚至几天）。

有时候这种默认处理可能并不是你想要的。如果你不想让一个singleton bean在ApplicationContext实现在初始化时被提前实例化，那么可以将bean设置为延迟实例化。一个延迟初始化bean将告诉IoC 容器是在启动时还是在第一次被用到时实例化。