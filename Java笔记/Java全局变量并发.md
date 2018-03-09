# Java多线程共享全局变量问题

## 全局私有变量

### 问题

从下面的例子，可以看到：开了10个线程，没个线程都对countNum这个私有变量进行了操作，导致数量一直在不断叠加。实际上，想要的结果是线程的此变量是彼此隔离的。（当然此处也与Count count = new Count();声明的位置有关，实际上是对同一个声明的变量进行10个线程代码的来回调用）

Count:

```java
public class Count {
    private Integer countNum = 0;

    public void count() {
        for (int i = 1; i <= 10; i++) {
            countNum = countNum + i;
        }
        System.out.println(Thread.currentThread().getName() + "-" + countNum);
    }
}
```

main:

```java
public class Test {
    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            Count count = new Count();

            public void run() {
                count.count();
            }
        };
        for (int i = 0; i < 10; i++) {
            new Thread(runnable).start();
        }
    }
}
```

result:

```java
Thread-5-330
Thread-9-550
Thread-1-110
Thread-4-275
Thread-7-440
Thread-6-385
Thread-8-495
Thread-3-220
Thread-2-165
Thread-0-55
```

### 解决办法一

ThreadLocal的作用是提供线程内的局部变量，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。

Count：

```java
public class Count {
    ThreadLocal<Integer> th = new ThreadLocal<Integer>() {
        protected Integer initialValue() {

            return 0;
        }
    };

    public void count() {
        for (int i = 1; i <= 10; i++) {
            th.set(th.get() + i);
        }
        System.out.println(Thread.currentThread().getName() + "-" + th.get());
    }
}
```

控制台：

```java
Thread-7-55
Thread-3-55
Thread-4-55
Thread-1-55
Thread-2-55
Thread-6-55
Thread-9-55
Thread-0-55
Thread-5-55
Thread-8-55
```

### 解决方法二

全局变量是每个方法共同持有的，因此很容易产生线程问题，在多线程环境中建议多使用局部变量，因为局部变量，顾名思义就是局部有效，每个线程都有自己的一份，因此不会产生线程问题。

Count:

```java
public class Count {
    public void count() {
        int num = 0;
        for (int i = 1; i <= 10; i++) {
            num = num + i;
        }
        System.out.println(Thread.currentThread().getName() + "-" + num);
    }
}
```

控制台：

```java
Thread-5-55
Thread-8-55
Thread-0-55
Thread-3-55
Thread-2-55
Thread-9-55
Thread-4-55
Thread-1-55
Thread-7-55
Thread-6-55
```

### 解决办法三

对象局部化：**对象局部化了，每个对象都有自己的全局变量**

main:

```java
public class Test {
    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            public void run() {
                Count count = new Count();
                count.count();
            }
        };
        for (int i = 0; i < 10; i++) {
            new Thread(runnable).start();
        }
    }
}
```

控制台：

```java
Thread-3-55
Thread-4-55
Thread-8-55
Thread-2-55
Thread-1-55
Thread-0-55
Thread-9-55
Thread-6-55
Thread-5-55
Thread-7-55
```

## 全局静态变量

RollDice

```java
import java.util.ArrayList;
import java.util.List;

public class RollDice {
    //ThreadLocal封装静态变量
    public static ThreadLocal<List<Object>> threadRollList = new ThreadLocal<List<Object>>() {
        //这里加同步是因为ThreadRollList是静态全局变量，防止ThreadLocal本身被并发。
        @Override
        protected synchronized List<Object> initialValue() {
            return new ArrayList<Object>(0);
        }
    };
    //用此种方式定义全局变量，遭遇多线程并发时，会出现bug.
    public static List<Object> rollList = new ArrayList<Object>(0);
    //私有全局变量
    public ThreadLocal<List<Object>> priTreadRollList = new ThreadLocal<List<Object>>() {
        //因为是私有变量，ThreadLocal本身会被放进线程，所以不用担心并发，因此也不需要synchronized。
        @Override
        protected List<Object> initialValue() {
            return new ArrayList<Object>(0);
        }
    };
    //调用方式
    public static void main(String[] args) {
        //ThreadLocal调用方式
        threadRollList.get().add(new Object());
        //普通定义调用方式
        rollList.add(new Object());
    }
}

```

