# 1. 背景

​		在进行项目开发的时候，我们往往不止创建一个模块。并且，我们希望不同模块对应解决不同类型的问题、同一模块内的各组件，均是为了解决同一类型的问题。因此，我们尽量采用**高内聚、低耦合**的方式设计各个模块。

​		假设，在我们的项目中，有一个已经写好的工具类A，在类B中我们需要一个A的实例化。

```java
public class A {
    private String name;
    private int age;
    
    public A(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public String toString() {
        return "A{" + "name='" + name + '\'' + ", age=" + age + '}';
    }
}
```
（1）实现方式一

```java
public class B {
	private A a = new A("张三", 18);
}
```

​		最基本的实现方式，即在类B中使用```new```实现A的实例化。该方式的弊端在于类A和类B高度耦合。

​		具体来说，若对类A的构造方法发生改变时，比如增添了一个新的属性。那么必须在本项目所有对其实例化的地方都进行改动。

（2）实现方式二

```java
public class Factory {
    public static A getA() {
        return new A("张三", 18);
    }
}
```

```java
public class B {
	private A a = Factory.getA();
}
```

​		采用工厂模式实现，此时类A仅与类Factory高度耦合。因此，当类A的构造方法发生改变时，只需修改Factory中的代码，而项目中其他获取类A的实例化对象的程序，完全不需要进行任何改变。

（3）实现方式三

​		假设能有某种方式，可以让类A的构造方法写在某种特定格式的配置文件中（例如xml文件）。而在Factory的getA方法中，通过读取配置文件的方式对其进行实例化。

​		那么此时，此时类A仅与该配置文件高度耦合。因此，当类A的构造方法发生改变时，只需修改这一配置文件，而项目中其他所有部分，完全不需要做任何改变。

​		**这一思想，便是Spring Framework的理念雏形！**



# 2. 简介

​		Spring Framework是Java EE编程领域的一个轻量级开源框架。目的是为了解决企业级编程开发中的复杂性、实现敏捷开发。它集成各类型的工具，通过核心的 Bean factory 实现了底层的类的实例化和生命周期的管理。在整个框架中，各类型的功能被抽象成一个个的 Bean，这样就可以实现各种功能的管理，包括动态加载和切面编程。



# 3. 优点

+ Spring是一个**开源的、免费使用**的框架（容器）
+ Spring具有**轻量级、非入侵式**的特点，即在使用它的同时，无需改变已有程序（反而可能简化）
+ Spring整合了现有的技术框架，使现有的技术更加容易使用



# 4. 模块组成

![img](https://img2018.cnblogs.com/blog/1010726/201909/1010726-20190908042152777-1895820426.png)

​		Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式，组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。

每个模块的功能如下: 

+ **Spring Core**: Spring Core 提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用控制反转（IOC）模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。

+ **Spring Context**: Spring Context 是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。

+ **Spring AOP**: 通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。

+ **Spring DAO**: JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。

+ **Spring ORM**: Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。

+ **Spring Web**: Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。

+ **Spring MVC**: Spring MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的。并且容纳了大量视图技术，包括JSP、Velocity、Tiles、iText 和 POI。



# 5. 使用

​		这里，利用Spring框架实现在类B中使用类A的实例化对象这一功能。

## 1. 导入jar包

创建一个Maven项目，并在项目的``pom.xml``中，添加Spring框架的依赖项。

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.2.5.RELEASE</version>
</dependency>
```



## 2. 编写Spring配置文件

在工程的``resources``目录下，创建``beans.xml``配置文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--bean就是java对象 , 由Spring创建和管理-->
    <bean id="a" class="utils.A">
        <constructor-arg name="name" value="张三"/>
        <constructor-arg name="age" value="18"/>
    </bean>

</beans>
```



## 3. 实例化调用

在类B的程序中，调用类A的实例化对象。

```java
public static void main(String[] args) {
    // 解析beans.xml文件 , 生成管理相应的Bean对象
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    // getBean参数为Spring配置文件中指向类A的bean的id
    A a = context.getBean("a", A.class);
    System.out.println(a.toString());
}
```

> 输出结果为:  A{name='张三', age=18}

说明类B中的程序成功完成了类A实例化对象的调用。