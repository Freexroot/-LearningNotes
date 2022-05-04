Spring4 框架技术

讲师 王鹤

# 第**1**章 **Spring** 概述

## **1.1 Spring** 框架是什么

> Spring 是于 2003 年兴起的一个轻量级的 Java 开发框架，它是为了解决企业应用开发

的复杂性而创建的。Spring 的核心是控制反转（IoC）和面向切面编程（AOP）。Spring 是可

以在 Java SE/EE 中使用的轻量级开源框架。

> Spring 的主要作用就是为代码"解耦"，降低代码间的耦合度。就是让对象和对象（模

块和模块）之间关系不是使用代码关联，而是通过配置来说明。即在 Spring 中说明对象（模

块）的关系。

> Spring 根据代码的功能特点，使用 Ioc 降低业务对象之间耦合度。IoC 使得主业务在相互

调用过程中，不用再自己维护关系了，即不用再自己创建要使用的对象了。而是由 Spring

容器统一管理，自动"注入",注入即赋值。 而 AOP 使得系统级服务得到了最大复用，且

不用再由程序员手工将系统级服务"混杂"到主业务逻辑中了，而是由 Spring 容器统一完成

"织入"。

[官网：**https://spring.io/**](https://spring.io/)

## **1.2 Spring** 优点？

> Spring 是一个框架，是一个半成品的软件。有 20 个模块组成。它是一个容器管理对象，

容器是装东西的，Spring 容器不装文本，数字。装的是对象。Spring 是存储对象的容器。

（**1**） 轻量

> Spring 框架使用的 jar 都比较小，一般在 1M 以下或者几百 kb。Spring 核心功能的所需

的 jar 总共在 3M 左右。

Spring 框架运行占用的资源少，运行效率高。不依赖其他 jar

（**2**） 针对接口编程，解耦合

Spring 提供了 Ioc 控制反转，由容器管理对象，对象的依赖关系。原来在程序代码中的

对象创建方式，现在由容器完成。对象之间的依赖解耦合。



2

（**3**） **AOP** 编程的支持

> 通过 Spring 提供的 AOP 功能，方便进行面向切面的编程，许多不容易用传统 OOP 实现

的功能可以通过 AOP 轻松应付

> 在 Spring 中，开发人员可以从繁杂的事务管理代码中解脱出来，通过声明式方式灵活地

进行事务的管理，提高开发效率和质量。

（**4**） 方便集成各种优秀框架

> Spring 不排斥各种优秀的开源框架，相反 Spring 可以降低各种框架的使用难度，Spring

提供了对各种优秀框架（如 Struts,Hibernate、MyBatis）等的直接支持。简化框架的使用。

Spring 像插线板一样，其他框架是插头，可以容易的组合到一起。需要使用哪个框架，就把

这个插头放入插线板。不需要可以轻易的移除。

## **1.3 Spring** 体系结构

> Spring 由 20 多个模块组成，它们可以分为数据访问/集成（Data Access/Integration）、

Web、面向切面编程（AOP, Aspects）、提 供 JVM 的代理（Instrumentation）、消息发送（Messaging）、

核心容器（Core Container）和测试（Test）。



3

# 第**2**章 **IoC** 控制反转

> 控制反转（IoC，Inversion of Control），是一个概念，是一种思想。指将传统上由程序代

码直接操控的对象调用权交给容器，通过容器来实现对象的装配和管理。控制反转就是对对

象控制权的转移，从程序代码本身反转到了外部容器。通过容器实现对象的创建，属性赋值，

依赖的管理。

> IoC 是一个概念，是一种思想，其实现方式多种多样。当前比较流行的实现方式是依赖

注入。应用广泛。

> 依赖：classA 类中含有 classB 的实例，在 classA 中调用 classB 的方法完成功能，即 classA

对 classB 有依赖。

Ioc 的实现：

​	依赖注入：DI(Dependency Injection)，程序代码不做定位查询，这些工作由容器自行

完成。

> 依赖注入 DI 是指程序运行过程中，若需要调用另一个对象协助时，无须在代码中创建

被调用者，而是依赖于外部容器，由外部容器创建后传递给程序。

> Spring 的依赖注入对调用者与被调用者几乎没有任何要求，完全支持对象之间依赖关系

的管理。

**Spring** 框架使用依赖注入（**DI**）实现 **IoC**。

> Spring 容器是一个超级大工厂，负责创建、管理所有的 Java 对象，这些 Java 对象被称

为 Bean。Spring 容器管理着容器中 Bean 之间的依赖关系，Spring 使用"依赖注入"的方式

来管理 Bean 之间的依赖关系。使用 IoC 实现对象之间的解耦和。

**2.1** 开发工具准备

开发工具：idea2017 以上

依赖管理：maven3 以上

jdk:1.8 以上

需要设置 **maven** 本机仓库：



![image-20220320212842576](.\srping笔记.assets\image-20220320212842576.png)

## **2.2 Spring** 的第一个程序

举例：01-primay

**2.2.1** 创建 **maven** 项目

![image-20220320212906881](.\srping笔记.assets\image-20220320212906881.png)

## **2.2.2** 引入 **maven** 依赖 **pom.xml**

```xml
    <!--spring依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

**2.2.3** 定义接口与实体类

**public interface** SomeService {

**void** doSome();

}

**public class** SomeServiceImpl **implements** SomeService {

**public** SomeServiceImpl() {

**super**();

System.***out***.println(\"SomeServiceImpl无参数构造方法\");

}

\@Override

**public void** doSome() {

System.***out***.println(\"====业务方法doSome()===\");

}

}

**2.2.4** 创建 **Spring** 配置文件

在 src/main/resources/目录现创建一个 xml 文件，文件名可以随意，但 Spring 建议的名

> 

6

称为 applicationContext.xml。

spring 配置中需要加入约束文件才能正常使用，约束文件是 xsd 扩展名。

> \<bean />：用于定义一个实例对象。一个实例对应一个 bean 元素。
>
> id：该属性是 Bean 实例的唯一标识，程序通过 id 属性访问 Bean，Bean 与 Bean 间的依

赖关系也是通过 id 属性关联的。

class：指定该 Bean 所属的类，注意这里只能是类，不能是接口。

**2.2.5** 定义测试类

**2.2.6** 使用 **spring** 创建非自定义类对象

spring 配置文件加入 java.util.Date 定义：

\<bean id=\"myDate\" class=\"java.util.Date\" />

MyTest 测试类中：

调用 getBean("myDate"); 获取日期类对象。



7

**2.2.7** 容器接口和实现类

**ApplicationContext** 接口（容器）

ApplicationContext 用于加载 Spring 的配置文件，在程序中充当"容器"的角色。其实现

类有两个。

**A**、 配置文件在类路径下

若 Spring 配置文件存放在项目的类路径下，则使用 ClassPathXmlApplicationContext 实现

类进行加载。

**B**、 **ApplicationContext** 容器中对象的装配时机

ApplicationContext 容器，会在容器对象初始化时，将其中的所有对象一次性全部装配好。

以后代码中若要使用到这些对象，只需从内存中直接获取即可。执行效率较高。但占用内存。



8

**C**、 使用 **spring** 容器创建的 **java** 对象

## **2.3** 基于 **XML** 的 **DI**

举例：项目 di-xml

### **2.3.1** 注入分类

​	bean 实例在调用无参构造器创建对象后，就要对 bean 对象的属性进行初始化。初始化是由容器自动完成的，称为注入。

​	根据注入方式的不同，常用的有两类：set 注入、构造注入。

（**1**） **set** 注入**(**掌握)

​	set 注入也叫设值注入是指，通过 setter 方法传入被调用者的实例。这种注入方式简单、直观，因而在 Spring 的依赖注入中大量使用。

**A**、 简单类型

![image-20220320213841949](.\srping笔记.assets\image-20220320213841949.png)

![image-20220320213847367](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20220320213847367.png)

![image-20220320213854721](.\srping笔记.assets\image-20220320213854721.png)

创建 java.util.Date 并设置初始的日期时间：

**Spring** 配置文件：

![image-20220320213904253](.\srping笔记.assets\image-20220320213904253.png)

测试方法：

![image-20220320213909018](.\srping笔记.assets\image-20220320213909018.png)

**B**、 引用类型

当指定 bean 的某属性值为另一 bean 的实例时，通过 ref 指定它们间的引用关系。ref的值必须为某 bean 的 id 值。

![image-20220320214037083](.\srping笔记.assets\image-20220320214037083.png)

![image-20220320214042161](.\srping笔记.assets\image-20220320214042161.png)

对于其它 Bean 对象的引用，使用\<bean/>标签的 ref 属性，内容是其他<bean>里的id值

![image-20220320214048066](.\srping笔记.assets\image-20220320214048066.png)

测试方法：

![image-20220320214053042](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20220320214053042.png)

（**2**） 构造注入**(**理解**)**

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--声明student对象
        注入：就是赋值的意思
        简单类型： spring中规定java的基本数据类型和String都是简单类型。
        di:给属性赋值
        1. set注入（设值注入） ：spring调用类的set方法， 你可以在set方法中完成属性赋值
         1）简单类型的set注入
            <bean id="xx" class="yyy">
               <property name="属性名字" value="此属性的值"/>
               一个property只能给一个属性赋值
               <property....>
            </bean>

         2) 引用类型的set注入 ： spring调用类的set方法
           <bean id="xxx" class="yyy">
              <property name="属性名称" ref="bean的id(对象的名称)" />
           </bean>

        2.构造注入：spring调用类有参数构造方法，在创建对象的同时，在构造方法中给属性赋值。
          构造注入使用 <constructor-arg> 标签
          <constructor-arg> 标签：一个<constructor-arg>表示构造方法一个参数。
          <constructor-arg> 标签属性：
             name:表示构造方法的形参名
             index:表示构造方法的参数的位置，参数从左往右位置是 0 ， 1 ，2的顺序
             value：构造方法的形参类型是简单类型的，使用value
             ref：构造方法的形参类型是引用类型的，使用ref
    -->


    <!--使用name属性实现构造注入-->
    <bean id="myStudent" class="com.bjpowernode.ba03.Student" >
        <constructor-arg name="myage" value="20" />
        <constructor-arg name="mySchool" ref="myXueXiao" />
        <constructor-arg name="myname" value="周良"/>
    </bean>

    <!--使用index属性-->
    <bean id="myStudent2" class="com.bjpowernode.ba03.Student">
        <constructor-arg index="1" value="22" />
        <constructor-arg index="0" value="李四" />
        <constructor-arg index="2" ref="myXueXiao" />
    </bean>

    <!--省略index-->
    <bean id="myStudent3" class="com.bjpowernode.ba03.Student">
        <constructor-arg  value="张强强" />
        <constructor-arg  value="22" />
        <constructor-arg  ref="myXueXiao" />
    </bean>
    <!--声明School对象-->
    <bean id="myXueXiao" class="com.bjpowernode.ba03.School">
        <property name="name" value="清华大学"/>
        <property name="address" value="北京的海淀区" />
    </bean>

    <!--创建File,使用构造注入-->
    <bean id="myfile" class="java.io.File">
        <constructor-arg name="parent" value="D:\course\JavaProjects\spring-course\ch01-hello-spring" />
        <constructor-arg name="child" value="readme.txt" />
    </bean>
</beans>
```

### **2.3.2** 引用类型属性自动注入

​	通过为\<bean/>标签设置 autowire 属性值，为引用类型属性进行隐式自动注入（默认是不自动注入引用类型属性）。根据自动注入判断标准的不同，可以分为两种：

​	注意只能对引用类型使用

（**1**） **byName** 方式根据名称自动注入

```xml
<!--
        引用类型的自动注入： spring框架根据某些规则可以给引用类型赋值。不用你在给引用类型赋值了
   1.byName(按名称注入) ： java类中引用类型的属性名和spring容器中（配置文件）<bean>的id名称一样，且数据类型是一致的，这样的容器中的bean，spring能够赋值给引用类型。
	2.是set注入，记得set后的名字要对上
     语法：
     <bean id="xx" class="yyy" autowire="byName">
        简单类型属性赋值
     </bean>
-->
<!--byName-->
<bean id="myStudent" class="com.bjpowernode.ba04.Student"  autowire="byName">
    <property name="name" value="李四" />
    <property name="age" value="26" />
    <!--引用类型-->
    <!--<property name="school" ref="mySchool" />-->
</bean>

<!--声明School对象,注意id名要和上面的类的set后的一致-->
<bean id="school" class="com.bjpowernode.ba04.School">
    <property name="name" value="清华大学"/>
    <property name="address" value="北京的海淀区" />
</bean>
```



（**2**） **byType** 方式类型自动注入

```xml
<!--   
2.byType(按类型注入) ： java类中引用类型的数据类型和spring容器中（配置文件）<bean>的class属性是同源关系的，这样的bean能够赋值给引用类型
     同源就是一类的意思：
      1.java类中引用类型的数据类型和bean的class的值是一样的。
      2.java类中引用类型的数据类型和bean的class的值父子类关系的。
      3.java类中引用类型的数据类型和bean的class的值接口和实现类关系的
     语法：
     <bean id="xx" class="yyy" autowire="byType">
        简单类型属性赋值
     </bean>

     注意：在byType中， 在xml配置文件中声明bean只能有一个符合条件的，
          多余一个是错误的(会报错)
-->
<!--byType-->
<bean id="myStudent" class="com.bjpowernode.ba05.Student" autowire="byType">
    <property name="name" value="张飒"/>
    <property name="age" value="26"/>
    <!--引用类型-->
    <!--<property name="school" ref="mySchool" />-->
</bean>

<!--声明School对象，和class的值是-->
<bean id="mySchool" class="com.bjpowernode.ba05.School">
    <property name="name" value="人民大学"/>
    <property name="address" value="北京的海淀区"/>
</bean>

<!--声明School的子类，和class的值接口和实现类关系-->
<bean id="primarySchool" class="com.bjpowernode.ba05.PrimarySchool">
    <property name="name" value="北京小学"/>
    <property name="address" value="北京的大兴区"/>
</bean>
```

### **2.3.3** 为应用指定多个 **Spring** 配置文件

 多个配置优势

​	 1. 每个文件的大小比一个文件要小很多。效率高

​	 2. 避免多人竞争带来的冲突。

 

 多文件的分配方式：

1. 按功能模块，一个模块一个配置文件

2. 按类的功能，数据库相关的配置一个文件配置文件， 做事务的功能一个配置文件， 做service功能的一个配置文件等

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
         包含关系的配置文件：
         total(当前xml文件名)表示主配置文件 ： 包含其他的配置文件的，主配置文件一般是不定义对象的。
         语法：<import resource="其他配置文件的路径" />
         关键字："classpath:" 表示类路径（class文件所在的目录），
               在spring的配置文件中要指定其他文件的位置， 需要使用classpath，告诉spring到哪去加载读取文件。
    -->

    <!--加载的是文件列表-->
    <!--
    <import resource="classpath:ba06/spring-school.xml" />
    <import resource="classpath:ba06/spring-student.xml" />
    -->

    <!--
       在包含关系的配置文件中，可以通配符（*：表示任意字符）
       注意： 主的配置文件名称不能包含在通配符的范围内（不能叫做spring-total.xml，会循环）
    -->
    <import resource="classpath:ba06/spring-*.xml" />
</beans>
```



## **2.4** 基于注解的 **DI**

```
实现步骤：
1.加入依赖
2.创建类，在类中加入注解
3.创建spring的配置文件
  声明组件扫描器的标签，指名注解在你的项目的中的位置。
4.使用注解创建对象， 创建容器ApplicationContext
```

需要在 Spring 配置文件中配置组件扫描器，用于在指定的基本包(类包)中扫描注解。

![image-20220321085206640](.\srping笔记.assets\image-20220321085206640.png)

```xml
    <context:component-scan base-package="" />
```

指定多个包的三种方式：

**1)**使用多个 **context:component-scan** 指定不同的包路径

![image-20220321085214185](.\srping笔记.assets\image-20220321085214185.png)

**2)**指定 **base-package** 的值使用分隔符

分隔符可以使用逗号（，）分号（；）还可以使用空格，不建议使用空格。

逗号分隔：

![image-20220321085220168](.\srping笔记.assets\image-20220321085220168.png)

分号分隔：

![image-20220321085223970](.\srping笔记.assets\image-20220321085223970.png)

**3)base-package** 是指定到父包名

> base-package 的值表是基本包，容器启动会扫描包及其子包中的注解，当然也会扫描到子包下级的子包。所以 base-package 可以指定一个父包就可以。

![image-20220321085231814](.\srping笔记.assets\image-20220321085231814.png)

或者最顶级的父包

![image-20220321085241026](.\srping笔记.assets\image-20220321085241026.png)

但不建议使用顶级的父包，扫描的路径比较多，导致容器启动时间变慢。指定到目标包和合适的。也就是注解所在包全路径。例如注解的类在 com.bjpowernode.beans 包中



### **2.4.1** 定义 **Bean** 的注解**@Component**（@Repository，@Service，@Controller）

```java
/**
 * @Component: 创建对象的， 等同于<bean>的功能
 *     属性：value 就是对象的名称，也就是bean的id值，
 *          value的值是唯一的，创建的对象在整个spring容器中就一个
 *     位置：在类的上面
 *
 *  @Component(value = "myStudent")等同于
 *   <bean id="myStudent" class="com.bjpowernode.ba01.Student" />
 *
 *  spring中和@Component功能一致，创建对象的注解还有：
 *  1.@Repository（用在持久层类的上面） : 放在dao的实现类上面，
 *               表示创建dao对象，dao对象是能访问数据库的。
 *  2.@Service(用在业务层类的上面)：放在service的实现类上面，
 *              创建service对象，service对象是做业务处理，可以有事务等功能的。
 *  3.@Controller(用在控制器的上面)：放在控制器（处理器）类的上面，创建控制器对象的，
 *              控制器对象，能够接受用户提交的参数，显示请求的处理结果。
 *  以上三个注解的使用语法和@Component一样的。 都能创建对象，但是这三个注解还有额外的功能。
 *  @Repository，@Service，@Controller是给项目的对象分层的。
 */

//使用value属性，指定对象名称
@Component(value = "myStudent")
//省略value
@Component("myStudent")
//不指定对象名称，由spring提供默认名称,格式是: 类名的首字母小写
@Component

public class Student {
...
}
```

### **2.4.2** 简单类型属性注入**@Value(**掌握**)**

属性上使用注解，给属性初始化

```java
@Component("myStudent")
public class Student {
    /**
     * @Value: 简单类型的属性赋值
     *   属性： value 是String类型的，表示简单类型的属性值
     *   位置： 1.在属性定义的上面，无需set方法，推荐使用。
     *         2.在set方法的上面
     */
    @Value("李四" )
    private String name;
    private Integer age;
    
    @Value("30")
    public void setAge(Integer age) {this.age = age;}
 }
```



### **2.4.3@Autowired** 的byType 自动注入**(**引用赋值**)**

需要在引用属性上使用注解\@Autowired，该注解默认使用按类型自动装配 Bean 的方式。

使用该注解完成属性注入时，类中无需 setter。当然，若属性有 setter，则也可将其加

到 setter 上。

```java
@Component("myStudent")
public class Student {
    /**
     * 引用类型
     * @Autowired: spring框架提供的注解，实现引用类型的赋值。
     * spring中通过注解给引用类型赋值，使用的是自动注入原理 ，支持byName, byType
     * @Autowired:默认使用的是byType自动注入。
     *
     *  位置：1）在属性定义的上面，无需set方法， 推荐使用
     *       2）在set方法的上面
     */
    @Autowired
    private School school;
    private School school1;
    
    @Autowired
	public setSchool(School school1){this.school1=school1};
 }
```

### **2.4.4 @Autowired** 与**@Qualifier**的byName自动注入**(**引用赋值**)**

注意要用byName自动注入的话，两个注解要一起用 

```java
@Component("myStudent")
public class Student {
    /**
     * 引用类型
     *  如果要使用byName方式，需要做的是：
     *  1.在属性上面加入@Autowired
     *  2.在属性上面再加入@Qualifier(value="bean的id") ：表示使用指定名称的bean完成赋值。
     *	3.两个注解的顺序不要求 
     */
    @Autowired
    @Qualifier("mySchool")
    private School school;
    private School school1;
    
    @Autowired
    @Qualifier("mySchool1")	
    public setSchool(School school1){this.school1=school1};
 }
```



```java
/**
*   属性：required ，是一个boolean类型的，默认true
*       @Autowired(required=true)：表示引用类型赋值失败，程序报错，并终止执行。
*       @Autowired(required=false)：引用类型如果赋值失败， 程序正常执行，引用类型是null
*/

```

行。若将其值设置为 false，则匹配失败，将被忽略，未匹配的属性值为 null。

### **2.4.5 JDK** 注解**@Resource** 自动注入（byName和byType引用赋值)

```java
@Component("myStudent")
public class Student {

    @Resource
    private School school;
    private School school1;
    
    /**
     * 引用类型
     * @Resource: 来自jdk中的注解，spring框架提供了对这个注解的功能支持，可以使用它给引用类型赋值
     *            使用的也是自动注入原理，支持byName， byType .默认是byName
     *	注意：默认是byName： 先使用byName自动注入，如果byName赋值失败，再使用byType
     *  位置： 1.在属性定义的上面，无需set方法，推荐使用。
     *        2.在set方法的上面
     *
     * @Resource的byName方式失败后不自动使用使用byType的方法
     * 需要增加一个属性 name，name的值是bean的id（名称）
     */
    
    @Resource(name = "mySchool")
    public setSchool(School school1){this.school1=school1};
 }
```

关于@Resource无法使用的问题

1. 把tomcat路径构建进去就有了 tomcat自带Resource注解

2. jdk1.6以上，jdk11以下才有

3. Resource注解变红的，需要在pom文件中加入依赖：

```xml
<!--Resource依赖，防止@Resource无法使用-->
<dependency>
    <groupId>javax.annotation</groupId>
    <artifactId>javax.annotation-api</artifactId>
    <version>1.2</version>
</dependency>
```

### **2.4.6** 和properties配合使用

```properties
#文件名test.properties
#避免乱码 
myname=\u5F20\u4E09\u975E
myage=20
```

```xml
<!--再在beans标签下添加（和扫描器同级）， 加载属性配置文件-->
<!--file-encoding 解决中文乱码问题，根据idea选择utf-8或者gbk-->
    <context:property-placeholder file-encoding="utf-8" location="classpath:test.properties" />
```

```java
@Component("myStudent")
public class Student {
    @Value("${myname}")
    private String name;
    @Value("${myage}")
    private Integer age;
 }
```



### **2.4.7 注解与 **XML** 的对比

注解特点是：

1. 方便，直观，高效（代码少，没有配置文件的书写那么复杂）。
2. 排版乱一点
3. 修改麻烦，要重写编译源文件，重写发布

XML 方式特点是：

1. 配置和代码是分离的，在 xml 中做修改，无需编译代码，只需重启服务器即可将新的配置加载。
2. 编写麻烦，效率低，大型项目过于复杂

# 第**3**章**AOP** 面向切面编程



## **3.1**为什么使用**AOP** 的开发方式（理解）

​	先定义好接口与一个实现类，该实现类中除了要实现接口中的方法外，还要再写两个非业务方法。非业务方法也称为交叉业务逻辑：

也就是如下格式

```
非业务方法1
业务方法
非业务方法2
```

但是为了解耦合，不在原代码的基础上增加非业务方法，就要用动态代理。也就是不改变业务方法的提前下，添加两个非业务方法。

##  **3.2 AOP** 概述

1. AOP（Aspect Orient Programming），面向切面编程。面向切面编程是从动态角度考虑程序运行过程,一种动态代理的规范化。
2. AOP 底层，就是采用动态代理模式实现的。采用了两种代理：
   1. **JDK** 的动态代理：使用jdk中的Proxy，Method，InvocaitonHanderl创建代理对象。jdk动态代理要求目标类必须实现接口
   2. 与 **CGLIB**的动态代理：第三方的工具库，创建代理对象，原理是继承。 通过继承目
3. AOP 为 Aspect Oriented Programming 的缩写，意为：面向切面编程，可通过运行期动态代理实现程序功能的统一维护的一种技术。AOP 是 Spring 框架中的一个重要内容。利用 AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
4. 面向切面编程，就是将交叉业务逻辑封装成切面，利用 AOP 容器的功能将切面织入到主业务逻辑中。所谓交叉业务逻辑是指，通用的、与主业务逻辑无关的代码，如安全检查、事务、日志、缓存等。
5. 例如，转账，在真正转账业务逻辑前后，需要权限控制、日志记录、加载事务、结束事务等交叉业务逻辑，而这些业务逻辑与主业务逻辑间并无直接关系。但，它们的代码量所占比重能达到总代码量的一半甚至还多。它们的存在，不仅产生了大量的“冗余”代码，还大大干扰了主业务逻辑---转账。

 

 怎么理解面向切面编程 ？ 

​	1）需要在分析项目功能时，找出切面。

​	2）合理的安排切面的执行时间（在目标方法前， 还是目标方法后）

​	3）合理的安全切面执行的位置，在哪个类，哪个方法增加增强功能

## 3.3面向切面编程对有什么好处？

​	1.减少重复；

​	2.专注业务；

​	注意：面向切面编程只是面向对象编程的一种补充。

​	使用 **AOP** 减少重复代码，专注业务实现：



## **3.4 AOP** 编程术语**(**掌握**)**

（**1**） 切面（**Aspect**）

​	切面泛指交叉业务逻辑。上例中的事务处理、日志处理就可以理解为切面。常用的切面是通知（Advice）。实际就是对主业务逻辑的一种增强，也就是额外功能。

（**2**） 连接点（**JoinPoint**）

​	连接点指可以被切面织入的具体方法。通常业务接口中的方法均为连接点。

（**3**） 切入点（**Pointcut**）

> 切入点指声明的一个或多个连接点的集合。通过切入点指定一组方法。
>
> 被标记为 final 的方法是不能作为连接点与切入点的。因为最终的是不能被修改的，不

能被增强的。

（**4**） 目标对象（**Target**）

> 目 标 对 象 指 将 要 被 增 强 的 对 象 。 即 包 含 主 业 务 逻 辑 的 类 的 对 象 。 上 例 中 的StudentServiceImpl 的对象若被增强，则该类称为目标类，该类对象称为目标对象。当然，不被增强，也就无所谓目标不目标了。

（**5**） 通知（**Advice**）

> 通知表示切面的执行时间，Advice 也叫增强。上例中的 MyInvocationHandler 就可以理解为是一种通知。换个角度来说，通知定义了增强代码切入到目标代码的时间点，是目标方法执行之前执行，还是之后执行等。通知类型不同，切入时间不同。

> 切入点定义切入的位置，通知定义切入的时间。

## **3.5AspectJ** 对 **AOP** 的实现**(**掌握**)**



1. spring：spring在内部实现了aop规范，能做aop的工作。spring主要在事务处理时使用aop。我们项目开发中很少使用spring的aop实现。 因为spring的aop比较笨重。

2. aspectJ: 一个开源的专门做aop的框架(主要用这个)。spring框架中集成了aspectj框架，通过spring就能使用aspectj的功能。

 aspectJ框架实现aop有两种方式：

+ 使用xml的配置文件 ： 配置全局事务

+ 使用注解，我们在项目中要做aop功能，一般都使用注解， aspectj有5个注解。



总结：所以，在 Spring 中使用 AOP 开发时，一般使用 AspectJ 的实现方式，而不用String自带的。



AspectJ 简介

AspectJ 是一个优秀面向切面的框架，它扩展了 Java 语言，提供了强大的切面实现。

官网[地址：**<span class="underline">http://www.eclipse.org/aspectj/</span>**](http://www.eclipse.org/aspectj/)

**AspetJ** 是 **Eclipse** 的开源项目，官网介绍如下：

​	a seamless aspect-oriented extension to the Javatm programming language（一种基于 Java 平台的面向切面编程的语言）

​	Java platform compatible（兼容 Java 平台，可以无缝扩展）

​	easy to learn and use（易学易用）

### **3.5.1 AspectJ** 的通知类型**(**理解**)**

AspectJ 中常用的通知有五种类型：

​	（1）前置通知

​	（2）后置通知

​	（3）环绕通知

​	（4）异常通知

​	（5）最终通知

### **3.5.2 AspectJ** 的切入点表达式**(**掌握**)**

AspectJ 定义了专门的表达式用于指定切入点。表达式的原型是：



> execution(modifiers-pattern? ret-type-pattern
>
> declaring-type-pattern?name-pattern(param-pattern)
>
> throws-pattern?)

解释：

> modifiers-pattern 访问权限类型
>
> ret-type-pattern 返回值类型
>
> declaring-type-pattern 包名类名
>
> name-pattern(param-pattern) 方法名(参数类型和参数个数)
>
> throws-pattern 抛出异常类型
>
> ？表示可选的部分
>



以上表达式共 4 个部分。

execution(访问权限 <u>方法返回值</u> <u>方法声明(参数)</u> 异常类型)

> 切入点表达式要匹配的对象就是目标方法的方法名。所以，execution 表达式中明显就是方法的签名。注意，表达式中带下划线表示一定要写的部分，各部分间用空格分开。在其中可以使用以下符号：

举例：

execution(public \* \*(..))

指定切入点为：任意公共方法，任意参数。

execution(\* set\*(..))

指定切入点为：任何一个以"set"开始的方法。

execution(\* com.xyz.service.\*.\*(..))

指定切入点为：定义在 com.xyz.service 包里的任意类的任意方法。

execution(\* com.xyz.service..\*.\*(..))

指定切入点为：定义在 service 包或者子包里的任意类的任意方法。".."出现在类名中时，后面必须跟"\*"，表示包、子包下的所有类。

execution(\* \*..service.\*.\*(..))

指定所有包（不管多少级）下的 serivce 子包下所有类（或接口）中所有方法为切入点

execution(\* \*.service.\*.\*(..))

指定只有一级包下的 serivce 子包下所有类（接口）中所有方法为切入点

execution(\* \*.ISomeService.\*(..))

指定只有一级包下的 ISomeSerivce 接口中所有方法为切入点

execution(\* \*..ISomeService.\*(..))

指定所有包下的 ISomeSerivce 接口中所有方法为切入点

execution(\* com.xyz.service.IAccountService.\*(..))

指定切入点为：IAccountService 接口中的任意方法。

execution(\* com.xyz.service.IAccountService+.\*(..))

指定切入点为：IAccountService 若为接口，则为接口中的任意方法及其所有实现类中的任意

方法；若为类，则为该类及其子类中的任意方法。

execution(\* joke(String,int)))

指定切入点为：所有的 joke(String,int)方法，且 joke()方法的第一个参数是 String，第二个参

数是 int。如果方法中的参数类型是 java.lang 包下的类，可以直接使用类名，否则必须使用

全限定类名，如 joke( java.util.List, int)。

execution(\* joke(String,\*)))

指定切入点为：所有的 joke()方法，该方法第一个参数为 String，第二个参数可以是任意类型 ，如 joke(String s1,String s2)和 joke(String s1,double d2)都是，但 joke(String s1,double d2,String s3)不是。

execution(\* joke(String,..)))

指定切入点为：所有的 joke()方法，该方法第一个参数为 String，后面可以有任意个参数且参数类型不限，如 joke(String s1)、joke(String s1,String s2)和 joke(String s1,double d2,String s3)都是。

execution(\* joke(Object))

指定切入点为：所有的 joke()方法，方法拥有一个参数，且参数是 Object 类型。joke(Object ob)是，但，joke(String s)与 joke(User u)不是。

execution(\* joke(Object+)))

指定切入点为：所有的 joke()方法，方法拥有一个参数，且参数是 Object 类型或该类的子类。不仅 joke(Object ob)是，joke(String s)和 joke(User u)也是。

### **3.5.3 AspectJ** 的开发环境**(**掌握**)**

（**1**） **maven** 依赖

```xml
        <!--aspectj依赖，aop-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
```



（**2**） 引入 **AOP** 约束

> 在 AspectJ 实现 AOP 时，要引入 AOP 的约束。配置文件中使用的 AOP 约束中的标签，
>
> 均是 AspectJ 框架使用的，而非 Spring 框架本身在实现 AOP 时使用的。
>
> AspectJ 对于 AOP 的实现有注解和配置文件两种方式，常用是注解方式。

####    

### **3.5.4 AspectJ** 基于注解的 **AOP** 实现**(**掌握**)**

#### （**1**） 实现步骤

- 使用aop：目的是给已经存在的一些类和方法，增加额外的功能。 前提是不改变原来的类的代码。

- 使用aspectj实现aop的基本步骤：
  1.新建maven项目
  2.加入依赖
    1）spring依赖
    2）aspectj依赖
    3）junit单元测试
  3.创建目标类：接口和他的实现类。
    要做的是给类中的方法增加功能
- 4.创建切面类：普通类
    1）在类的上面加入 @Aspect
    2）在类中定义方法， 方法就是切面要执行的功能代码
      在方法的上面加入aspectj中的通知注解，例如@Before
      有需要指定切入点表达式execution()
- 5.创建spring的配置文件：声明对象，把对象交给容器统一管理
    声明对象你可以使用注解或者xml配置文件<bean>
    1)声明目标对象
    2）声明切面类对象
    3）声明aspectj框架中的自动代理生成器标签。
       自动代理生成器：用来完成代理对象的自动创建功能的。
- 6.创建测试类，从spring容器中获取目标对象（实际就是代理对象）。
    通过代理执行方法，实现aop的功能增强。



#### （**2**） **\[**掌握**]@Before** 前置通知**-**方法有 **JoinPoint** 参数

```java
//目标接口
public interface SomeService {
    void doSome(String name,Integer age);
}
```

```java
//目标类（业余部分）
public class SomeServiceImpl implements SomeService {
    public void doSome(String name,Integer age) {
        //给doSome方法增加一个功能，在doSome()执行之前， 输出方法的执行时间
        System.out.println("====目标方法doSome()====");
    }
}
```

切口类（增加部分/额外功能部分）

```java
/**
 *  @Aspect : 是aspectj框架中的注解。
 *     作用：表示当前类是切面类。
 *     切面类：是用来给业务方法增加功能的类，在这个类中有切面的功能代码
 *     位置：在类定义的上面
 */
@Aspect
public class MyAspect {
    /**
     * 定义方法，方法是实现切面功能的。
     * 方法的定义要求：
     * 1.公共方法 public
     * 2.方法没有返回值
     * 3.方法名称自定义
     * 4.方法可以有参数，也可以没有参数。
     *   如果有参数，参数不是自定义的，有几个参数类型可以使用。
     */


    /**
     * @Before: 前置通知注解
     *   属性：value ，是切入点表达式，表示切面的功能执行的位置。
     *   位置：在方法的上面
     * 特点：
     *  1.在目标方法之前先执行的
     *  2.不会改变目标方法的执行结果
     *  3.不会影响目标方法的执行。
     */
        @Before(value = "execution(public void com.bjpowernode.ba01.SomeServiceImpl.doSome(String,Integer))")
    	/*
    	同样的可以写成
        /*@Before(value = "execution(void *..SomeServiceImpl.doSome(String,Integer))") 省略public
          @Before(value = "execution(* com.bjpowernode.ba01.*ServiceImpl.*(..))")	类名模糊查询
          @Before(value = "execution(* *..SomeServiceImpl.*(..))")					
          @Before(value = "execution(* do*(..))")
		如果找不到，不会报错出异常
		*/
        public void myBefore(){
            //就是你切面要执行的功能代码
            System.out.println("前置通知， 切面功能：在目标方法之前输出执行时间："+ new Date());
        }
}
```



```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--把对象交给spring容器，由spring容器统一创建，管理对象-->
    <!--声明目标对象-->
    <bean id="someService" class="com.bjpowernode.ba01.SomeServiceImpl" />

    <!--声明切面类对象-->
    <bean id="myAspect" class="com.bjpowernode.ba01.MyAspect" />

    <!--声明自动代理生成器：使用aspectj框架内部的功能，创建代理对象。
        创建代理对象是在内存中实现的， 修改目标对象的内存中的结构。
        所以目标对象就是被修改后的代理对象.

        aspectj-autoproxy:会把spring容器中的所有的目标对象，一次性都生成代理对象(让切口类生效)。
    -->
    <aop:aspectj-autoproxy />


    <!--
       如果你的目标类有接口，要主动使用cglib代理
       proxy-target-class="true":告诉框架，要使用cglib动态代理

  	  <aop:aspectj-autoproxy proxy-target-class="true"/>
    -->
</beans>
```

测试类

```java
public void test01(){
    String config="applicationContext.xml";
    ApplicationContext ctx = new ClassPathXmlApplicationContext(config);
    //从容器中获取目标对象
    SomeService proxy = (SomeService) ctx.getBean("someService");

    //通过代理的对象执行方法，实现目标方法执行时，增强了功能
    proxy.doSome();
}
```



JoinPoint

```java
/**
 * 指定通知方法中的参数 ： JoinPoint
 * JoinPoint:业务方法，要加入切面功能的业务方法
 *    作用是：可以在业务方法中获取方法执行时的信息， 例如方法名称，方法的实参。
 *    如果你的切面功能中需要用到方法的信息，就加入JoinPoint.
 *    这个JoinPoint参数的值是由框架赋予， 必须是第一个位置的参数
 */
@Before(value = "execution(void *..SomeServiceImpl.doSome(String,Integer))")
public void myBefore(JoinPoint jp){
    //获取方法的完整定义
    System.out.println("方法的签名（定义）="+jp.getSignature());
    System.out.println("方法的名称="+jp.getSignature().getName());
    //获取方法的实参
    Object args [] = jp.getArgs();
    for (Object arg:args){
        System.out.println("参数="+arg);
    }
    //就是你切面要执行的功能代码
    System.out.println("2=====前置通知， 切面功能：在目标方法之前输出执行时间："+ new Date());
}
```



#### （**3**） **\[**掌握**]@AfterReturning** 后置通知**-**注解有 **returning** 属性

```java
public class MyAspect {
    /**
     * 后置通知定义方法，方法是实现切面功能的。
     * 方法的定义要求：
     * 1.公共方法 public
     * 2.方法没有返回值
     * 3.方法名称自定义
     * 4.方法有参数的,推荐是Object ，参数名自定义
     */

    /**
     * @AfterReturning:后置通知
     *    属性：1.value 切入点表达式
     *         2.returning 自定义的变量，表示目标方法的返回值的。
     *          自定义变量名必须和通知方法的形参名一样。
     *    位置：在方法定义的上面
     * 特点：
     *  1。在目标方法之后执行的。
     *  2. 能够获取到目标方法的返回值，可以根据这个返回值做不同的处理功能
     *      Object res = doOther();
     *  3. 可以修改这个返回值，是引用传递
     *
     *  后置通知的执行
     *	  目标方法的返回值传递给切面方法
     *    Object res = doOther();
     *    参数传递： 传值， 传引用
     *    myAfterReturing(res);
     *
     	  在后置通知使用JoinPoint，注意，一定要把JoinPoint jp放在第一个，否则报错
     	  jp里面反的是业务方法里的信息
     */
    @AfterReturning(value = "execution(* *..SomeServiceImpl.doOther(..))",
                    returning = "res")
    public void myAfterReturing(JoinPoint jp,  ,Object res ){
        // Object res:是目标方法执行后的返回值，根据返回值做你的切面的功能处理
        System.out.println("后置通知：方法的定义"+ jp.getSignature());
        System.out.println("后置通知：在目标方法之后执行的，获取的返回值是："+res);
        
        //根据res值不同，做不同的处理
        if(res.equals("abcd")){
            //做一些功能
        } else{
            //做其它功能
        }

    }
    
```

#### （**4**） **\[**掌握**]@Around** 环绕通知**-**增强方法有 **ProceedingJoinPoint**



```java
@Aspect
public class MyAspect {
    /**
     * 环绕通知方法的定义格式
     *  1.public
     *  2.必须有一个返回值，推荐使用Object
     *  3.方法名称自定义
     *  4.方法有参数，固定的参数 ProceedingJoinPoint
     */

    /**
     * @Around: 环绕通知
     *    属性：value 切入点表达式
     *    位置：在方法的定义什么
     * 特点：
     *   1.它是功能最强的通知
     *   2.在目标方法的前和后都能增强功能。
     *   3.控制目标方法是否被调用执行
     *   4.修改原来的目标方法的执行结果。 影响最后的调用结果
     *
     *  环绕通知，等同于jdk动态代理的，InvocationHandler接口
     *
     *  参数：  ProceedingJoinPoint 就等同于 Method
     *         作用：执行目标方法的
     *  返回值： 就是目标方法的执行结果，可以被修改。
     *
     *  环绕通知： 经常做事务， 在目标方法之前开启事务，执行目标方法， 在目标方法之后提交事务
     */
    @Around(value = "execution(* *..SomeServiceImpl.doFirst(..))")
    public Object myAround(ProceedingJoinPoint pjp) throws Throwable {
        //1.在目标方法的前加入功能
        System.out.println("环绕通知：在目标方法之前，输出时间："+ new Date());
        
        String name = "";
        //获取第一个参数值
        Object args [] = pjp.getArgs();
        if( args!= null && args.length > 1){
              Object arg=  args[0];
              name =(String)arg;
        }
        
        Object result = null;
        //1.目标方法调用（筛选性调用）
        if( "zhangsan".equals(name)){
            //符合条件，调用目标方法
            result = pjp.proceed(); //相当于method.invoke(); 或 Object result = doFirst();
        }
        
        //修改目标方法的执行结果， 影响方法最后的调用结果
        if( result != null){
              result = "Hello AspectJ AOP";
        }
        
		//2.在目标方法的后加入功能
        System.out.println("环绕通知：在目标方法之后，提交事务");
        
        //返回目标方法的执行结果
        return result;
    }
}
```

#### （**5**） **\[**了解**\]@AfterThrowing** 异常通知**-**注解中有 **throwing** 属性

```java
@Aspect
public class MyAspect {
    /**
     * 异常通知方法的定义格式
     *  1.public
     *  2.没有返回值
     *  3.方法名称自定义
     *  4.方法有个一个Exception， 如果还有是JoinPoint,
     */

    /**
     * @AfterThrowing:异常通知
     *     属性：1. value 切入点表达式
     *          2. throwinng 自定义的变量，表示目标方法抛出的异常对象。
     *             变量名必须和方法的参数名一样
     * 特点：
     *   1. 在目标方法抛出异常时执行的
     *   2. 可以做异常的监控程序， 监控目标方法执行时是不是有异常。
     *      如果有异常，可以发送邮件，短信进行通知
     *
     *  相当于就是：
     *   try{
     *       SomeServiceImpl.doSecond(..)
     *   }catch(Exception e){
     *       myAfterThrowing(e);
     *   }
     */
    @AfterThrowing(value = "execution(* *..SomeServiceImpl.doSecond(..))",
            throwing = "ex")
    public void myAfterThrowing(Exception ex) {
        System.out.println("异常通知：方法发生异常时，执行："+ex.getMessage());
        //发送邮件，短信，通知开发人员
    }

}
```

#### （**6**） **\[**了解**]@After** 最终通知

```java
@Aspect
public class MyAspect {
    /**
     * 最终通知方法的定义格式
     *  1.public
     *  2.没有返回值
     *  3.方法名称自定义
     *  4.方法没有参数，  如果还有是JoinPoint,
     */

    /**
     * @After :最终通知
     *    属性： value 切入点表达式
     *    位置： 在方法的上面
     * 特点：
     *  1.总是会执行，不会因为报错异常而不执行
     *  2.在目标方法之后执行的
     *	3.相当于下面的finally
     *
     *  try{
     *      SomeServiceImpl.doThird(..)
     *  }catch(Exception e){
     *
     *  }finally{
     *      myAfter()
     *  }
     *
     */
    @After(value = "execution(* *..SomeServiceImpl.doThird(..))")
    public  void  myAfter(){
        System.out.println("执行最终通知，总是会被执行的代码");
        //一般做资源清除工作的。
     }

}
```

#### （**7**） **@Pointcut** 定义切入点

```java
@Aspect
public class MyAspect {
    @After(value = "mypt()")
    public  void  myAfter(){
        System.out.println("执行最终通知，总是会被执行的代码");
     }

    @Before(value = "mypt()")
    public  void  myBefore(){
        System.out.println("前置通知，在目标方法之前先执行的");
    }

    /**
     * @Pointcut: 定义和管理切入点， 如果你的项目中有多个切入点表达式是重复的，可以复用的。
     *            可以使用@Pointcut
     *    属性：value 切入点表达式
     *    位置：在自定义的方法上面
     * 特点：
     *   当使用@Pointcut定义在一个方法的上面 ，此时这个方法的名称就是切入点表达式的别名。
     *   其它的通知中，value属性就可以使用这个方法名称，代替切入点表达式了
     */
    @Pointcut(value = "execution(* *..SomeServiceImpl.doThird(..))" )
    private void mypt(){
        //无需代码
    }
}
```



#### 

### 3.5.5切面类获得连接点（目标类方法）的数据

在切面功能里增加（JoinPoint jp）参数，这个jp就有连接点的各种信息

```java
/**
* 指定通知方法中的参数 ： JoinPoint
* JoinPoint（连接点）:业务方法，要加入切面功能的业务方法
*    作用是：可以在通知方法中获取方法执行时的信息， 例如方法名称，方法的实参。
*    如果你的切面功能中需要用到方法的信息，就加入JoinPoint.
*	 这个JoinPoint参数的值是由框架赋予， 必须是第一个位置的参数
*/
@Before(value = "execution(void *..SomeServiceImpl.doSome(String,Integer))")
public void myBefore(JoinPoint jp){
    //获取方法的完整定义
    System.out.println("方法的签名（定义）="+jp.getSignature());
    System.out.println("方法的名称="+jp.getSignature().getName());
    //获取方法的实参
    Object args [] = jp.getArgs();
    for (Object arg:args){
        System.out.println("参数="+arg);
    }
    //就是你切面要执行的功能代码
    System.out.println("2=====前置通知， 切面功能：在目标方法之前输出执行时间："+ new Date());
}
```

# 第**4**章**Spring** 集成 **MyBatis**

> 将 MyBatis 与 Spring 进行整合，主要解决的问题就是将 SqlSessionFactory 对象交由 Spring来管理。所以，该整合，只需要将 SqlSessionFactory 的对象生成器 SqlSessionFactoryBean 注册在 Spring 容器中，再将其注入给 Dao 的实现类即可完成整合。实现 Spring 与 MyBatis 的整合常用的方式：扫描的 Mapper 动态代理Spring 像插线板一样，mybatis 框架是插头，可以容易的组合到一起。插线板 spring 插上 mybatis，两个框架就是一个整体。
>
> 换了个连接池



## 4.1.1 MySQL 创建数据库 springdb,新建表 Student

![image-20220322084653791](.\srping笔记.assets\image-20220322084653791.png)



## 4.1.2 maven 依赖 pom.xml

```xml
<dependencies>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

		<!--spring核心ioc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.9.RELEASE</version>
        </dependency>
        <!--aspectj依赖，aop-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <!--做spring事务用到的-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <!--Resource依赖，防止@Resource无法使用-->
        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>javax.annotation-api</artifactId>
            <version>1.2</version>
        </dependency>

        <!--myBatis依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.1</version>
        </dependency>
        <!--mybatis和spring集成的依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.1</version>
        </dependency>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.9</version>
        </dependency>
        <!--阿里公司的数据库连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.12</version>
        </dependency>
    </dependencies>

    <!--插件-->
    <build>
        <!--目的是把src/main/java目录中的xml文件包含到输出结果中。输出到classes目录中-->
        <resources>
            <resource>
                <directory>src/main/java</directory><!--所在的目录-->
                <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```



## **4.1.3** 定义实体类 **Student**



```java
public class Student {
    //属性名和列名一样。
    private Integer id;
    private String name;
    private String email;
    private Integer age;
}
```

## **4.1.4** 定义 **StudentDao** 接口

```java
public interface StudentDao {
    int insertStudent(Student student);
    List<Student> selectStudents();
}
```

## **4.1.5** 定义映射文件 **mapper**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.bjpowernode.dao.StudentDao">

    <insert id="insertStudent">
        insert into student values(#{id},#{name},#{email},#{age})
    </insert>

    <select id="selectStudents" resultType="Student">
        select id,name,email,age from student order by id desc
    </select>
</mapper>
```

## **4.1.6** 定义 **Service** 接口和实现类

```JAVA
public interface StudentService {
    int addStudent(Student student);
    List<Student> queryStudents();
}

public class StudentServiceImpl implements StudentService {

    //引用类型,调用sql语句
    private StudentDao studentDao;

    //使用set注入，赋值
    public void setStudentDao(StudentDao studentDao) {
        this.studentDao = studentDao;
    }

    @Override
    public int addStudent(Student student) {
        int nums = studentDao.insertStudent(student);
        return nums;
    }

    @Override
    public List<Student> queryStudents() {
        List<Student> students = studentDao.selectStudents();
        return students;
    }
}
```

## **4.1.7** 定义 **MyBatis** 主配置文件

在 src 下定义 MyBatis 的主配置文件，命名为 mybatis.xml。
这里有两点需要注意：
（1）主配置文件中不再需要数据源的配置了。因为数据源要交给 Spring 容器来管理了。
（2）这里对 mapper 映射文件的注册，使用<package/>标签，即只需给出 mapper 映射文件所在的包即可。因为 mapper 的名称与 Dao 接口名相同，可以使用这种简单注册方式。这种方式的好处是，若有多个映射文件，这里的配置也是不用改变的。当然，也可使用原来的<resource/>标签方式。

```XML
<configuration>

    <!--settings：控制mybatis全局行为-->
    <settings>
        <!--设置mybatis输出日志-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <!--设置别名-->
    <typeAliases>
        <!--name:实体类所在的包名
            比如使用Student表示com.bjpowenrode.domain.Student
        -->
        <package name="com.bjpowernode.domain"/>
    </typeAliases>

    <!-- sql mapper(sql映射文件)的位置-->
    <mappers>
        <!--name：是包名， 这个包中的所有mapper.xml一次都能加载-->
        <package name="com.bjpowernode.dao"/>
    </mappers>
</configuration>
```

## **4.1.8** 修改 **Spring** 配置文件

（1） 数据源的配置(掌握)

> ​	使用 JDBC 模板，首先需要配置好数据源，数据源直接以 Bean 的形式配置在 Spring 配置文件中。根据数据源的不同，其配置方式不同：
>
> ​	Druid 数据源 DruidDataSource
> ​	Druid 是阿里的开源数据库连接池。是 Java 语言中最好的数据库连接池。Druid 能够提供强大的监控和扩展功能。Druid 与其他数据库连接池的最大区别是提供数据库的
> ​	官网：https://github.com/alibaba/druid
> ​	使用地址：https://github.com/alibaba/druid/wiki/常见问题

用Spring 的方法配置myBatis的数据库连接池

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--
       把数据库的配置信息，写在一个独立的文件，编译修改数据库的配置内容
       spring知道jdbc.properties文件的位置
    -->
    <context:property-placeholder location="classpath:jdbc.properties" />

    <!--声明数据源DataSource, 作用是连接数据库的-->
    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource"
          init-method="init" destroy-method="close">
        <!--set注入给DruidDataSource提供连接数据库信息 -->
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.passwd}" />
        <!--链接池数量-->
        <property name="maxActive" value="${jdbc.max}" />
    </bean>

    <!--声明的是mybatis中提供的SqlSessionFactoryBean类，这个类内部创建SqlSessionFactory的
        SqlSessionFactory  sqlSessionFactory = new ..  -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--set注入，把数据库连接池付给了dataSource属性-->
        <property name="dataSource" ref="myDataSource" />
        <!--mybatis主配置文件的位置
           configLocation属性是Resource类型，读取配置文件
           它的赋值，使用value，指定文件的路径，使用classpath:表示文件的位置
        -->
        <property name="configLocation" value="classpath:mybatis.xml" />
    </bean>

    <!--创建dao对象，使用SqlSession的getMapper（StudentDao.class）
        MapperScannerConfigurer:在内部调用getMapper()生成每个dao接口的代理对象。
	    Mapper 扫描配置器
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--指定SqlSessionFactory对象的id-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <!--指定包名， 包名是dao接口所在的包名。
            MapperScannerConfigurer会扫描这个包中的所有接口，把每个接口都执行
            一次getMapper()方法，得到每个接口的dao对象。
            创建好的dao对象放入到spring的容器（map）中的。 dao对象的默认名称是:接口名首字母小写
        -->
        <property name="basePackage" value="com.bjpowernode.dao"/>
    </bean>

    <!--声明service-->
    <bean id="studentService" class="com.bjpowernode.service.impl.StudentServiceImpl">
        <property name="studentDao" ref="studentDao" />
    </bean>
</beans>
```

（2） 从属性文件读取数据库连接信息

jdbc.properties文件

```properties
jdbc.url=jdbc:mysql://localhost:3306/springdb
jdbc.username=root
jdbc.passwd=222
jdbc.max=30
```

## 

## spring和mybatis整合在一起使用，事务是自动提交的。 无需执行SqlSession.commit();



# 第**5**章**Spring** 事务

## **5.1Spring** 的事务管理

1.什么是事务：多条sql语句一起执行

2.不同的数据库访问技术，处理事务的对象，方法不同，spring提供一种处理事务的统一模型， 能使用统一步骤，方式完成多种不同数据库（兼容）访问技术的事务处理。可以兼容jdbc，mybatis，hibernate等多种数据库方式。

> 事务原本是数据库中的概念，在 Dao 层。但一般情况下，需要将事务提升到业务层，
>
> 即 Service 层。这样做是为了能够使用事务的特性来管理具体的业务。
>
> 在 Spring 中通常可以通过以下两种方式来实现对事务的管理：
>
> （1）使用 Spring 的事务注解管理事务
>
> （2）使用 AspectJ 的 AOP 配置管理事务 

## **5.2Spring** 事务管理 **API**

Spring 的事务管理，主要用到两个事务相关的接口。

（**1**） 事务管理器接口**(**重点**)**

事务管理器是 PlatformTransactionManager 接口对象。其主要用于完成事务的提交、回滚，及获取事务的状态信息。

![image-20220322151140596](.\srping笔记.assets\image-20220322151140596.png)

**A**、 常用的两个实现类

​	PlatformTransactionManager 接口有两个常用的实现类：

​		DataSourceTransactionManager：使用 JDBC 或 MyBatis 进行数据库操作时使用。

​		HibernateTransactionManager：使用 Hibernate 进行持久化数据时使用。



**B**、 **Spring** 的回滚方式**(**理解**)**

​	Spring 事务的默认回滚方式是：发生运行时异常和 error 时回滚，发生受查(编译)异常时提交。不过，对于受查异常，程序员也可以手工设置其回滚方式。



**C**、 回顾错误与异常**(**理解**)**

> Throwable 类是 Java 语言中所有错误或异常的超类。只有当对象是此类(或其子类之一)的实例时，才能通过 Java 虚拟机或者 Java 的 throw 语句抛出。

> Error 是程 序在运 行过程 中出现 的无法 处理的错 误，比如 OutOfMemoryError 、ThreadDeath、NoSuchMethodError 等。当这些错误发生时，程序是无法处理（捕获或抛出）的，JVM 一般会终止线程。

> 程序在编译和运行时出现的另一类错误称之为异常，它是 JVM 通知程序员的一种方式。通过这种方式，让程序员知道已经或可能出现错误，要求程序员对其进行处理。

> 异常分为运行时异常与受查异常。
>
> 运行时异常，是 RuntimeException 类或其子类，即只有在运行时才出现的异常。如，

NullPointerException、ArrayIndexOutOfBoundsException、IllegalArgumentException 等均属于运行时异常。这些异常由 JVM 抛出，在编译时不要求必须处理（捕获或抛出）。但，只要代码编写足够仔细，程序足够健壮，运行时异常是可以避免的。

> 受查异常，也叫编译时异常，即在代码编写时要求必须捕获或抛出的异常，若不处理，则无法通过编译。如 SQLException，ClassNotFoundException，IOException 等都属于受查异常。

> RuntimeException 及其子类以外的异常，均属于受查异常。当然，用户自定义的 Exception的子类，即用户自定义的异常也属受查异常。程序员在定义异常时，只要未明确声明定义的为 RuntimeException 的子类，那么定义的就是受查异常。

（**2**） 事务定义接口

事务定义接口 TransactionDefinition 中定义了事务描述相关的三类常量：事务隔离级别、事务传播行为、事务默认超时时限，及对它们的操作。



**A**、定义了五个事务隔离级别常量**(**掌握**)**

这些常量均是以 ISOLATION_开头。即形如 ISOLATION_XXX。
DEFAULT：采用 DB 默认的事务隔离级别。MySql 的默认为 REPEATABLE_READ； Oracle
默认为 READ_COMMITTED。
READ_UNCOMMITTED：读未提交。未解决任何并发问题。
READ_COMMITTED：读已提交。解决脏读，存在不可重复读与幻读。
REPEATABLE_READ：可重复读。解决脏读、不可重复读，存在幻读
SERIALIZABLE：串行化。不存在并发问题。

**B**、 定义了七个事务传播行为常量**(**掌握**)**

> 所谓事务传播行为是指，处于不同事务中的方法在相互调用时，执行期间事务的维护情况。如，A 事务中的方法 doSome()调用 B 事务中的方法 doOther()，在调用执行期间事务的维护情况，就称为事务传播行为。事务传播行为是加在方法上的。事务传播行为常量都是以 PROPAGATION\_ 开头，形如 PROPAGATION_XXX。



**PROPAGATION_REQUIRED**

**PROPAGATION_REQUIRES_NEW**

**PROPAGATION_SUPPORTS**

上面3个比较重要

PROPAGATION_MANDATORY

PROPAGATION_NESTED

PROPAGATION_NEVER

PROPAGATION_NOT_SUPPORTED

**a**、 **PROPAGATION_REQUIRED**：

指定的方法必须在事务内执行。若当前存在事务，就加入到当前事务中；若当前没有事务，则创建一个新事务。这种传播行为是最常见的选择，也是 Spring 默认的事务传播行为。

> 如该传播行为加在 doOther()方法上。若 doSome()方法在调用 doOther()方法时就是在事务内运行的，则 doOther()方法的执行也加入到该事务内执行。若 doSome()方法在调用doOther()方法时没有在事务内执行，则 doOther()方法会创建一个事务，并在其中执行。

![image-20220323202453679](.\srping笔记.assets\image-20220323202453679.png)



**b**、 **PROPAGATION_SUPPORTS**

指定的方法支持当前事务，但若当前没有事务，也可以以非事务方式执行 。

![image-20220323202502556](.\srping笔记.assets\image-20220323202502556.png)

**c**、 **PROPAGATION_REQUIRES_NEW**

总是新建一个事务，若当前存在事务，就将当前事务挂起，直到新事务执行完毕。

![image-20220323202507763](.\srping笔记.assets\image-20220323202507763.png)



**C**、 定义了默认事务超时时限

> 常量 TIMEOUT_DEFAULT 定义了事务底层默认的超时时限，sql 语句的执行时长。
>
> 注意，事务的超时时限起作用的条件比较多，且超时的时间计算点较复杂。所以，该值一般就使用默认值即可。

## **5.3**程序举例环境搭建

依赖文件

```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
```



## **5.4**使用 **Spring** 的事务管理事务**(**掌握**)**

### 注解法

适合中小型项目

1. 声明事务管理器 和 开启注解驱动

   ```xml
       <!--使用spring的事务处理-->
       <!--1. 声明事务管理器-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!--连接的数据库， 指定数据源
   			ref="myDataSource"是之前定义的数据库的id-->
           <property name="dataSource" ref="myDataSource" />
       </bean>
   
       <!--2. 开启事务注解驱动，告诉spring使用注解管理事务，创建代理对象
              transaction-manager:事务管理器对象的id
   		tx:annotation-driven引入的时候有多个包可选择，记得选择带tx的
       -->
       <tx:annotation-driven transaction-manager="transactionManager" />
   ```
   
   2.业务层 public 方法加入事务属性。类也能加，表示类下的全部方法都开启事务，不过没必要
   
   通过@Transactional 注解方式，可将事务织入到相应 public 方法中，实现事务管理。
   @Transactional 的所有可选属性如下所示：
   
   propagation：用于设置事务传播属性。该属性类型为 Propagation 枚举，默认值为Propagation.REQUIRED。
   isolation：用于设置事务的隔离级别。该属性类型为 Isolation 枚举，默认值为Isolation.DEFAULT。
   readOnly：用于设置该方法对数据库的操作是否是只读的。该属性为 boolean，默认值为 false。
   timeout：用于设置本操作与数据库连接的超时时限。单位为秒，类型为 int，默认值为-1，即没有时限。
   rollbackFor：指定需要回滚的异常类。类型为 Class[]，默认值为空数组。当然，若只有一个异常类时，可以不使用数组。
   rollbackForClassName：指定需要回滚的异常类类名。类型为 String[]，默认值为空数组。当然，若只有一个异常类时，可以不使用数组。
   noRollbackFor：指定不需要回滚的异常类。类型为 Class[]，默认值为空数组。当然，若只有一个异常类时，可以不使用数组。
   noRollbackForClassName：指定不需要回滚的异常类类名。类型为 String[]，默认值为空数组。当然，若只有一个异常类时，可以不使用数组。
   需要注意的是，@Transactional 若用在方法上，只能用于 public 方法上。对于其他非 public方法，如果加上了注解@Transactional，虽然 Spring 不会报错，但不会将指定事务织入到该方法中。因为 Spring 会忽略掉所有非 public 方法上的@Transaction 注解。若@Transaction 注解在类上，则表示该类上所有的方法均将在执行时织入事务。
   
   
   
   ```java
   public class BuyGoodsServiceImpl implements BuyGoodsService {
       /**
        * rollbackFor:表示发生指定的异常一定回滚.
        *   处理逻辑是：
        *     1) spring框架会首先检查方法抛出的异常是不是在rollbackFor的属性值中
        *         如果异常在rollbackFor列表中，不管是什么类型的异常，一定回滚。
        *     2) 如果你的抛出的异常不在rollbackFor列表中，spring会判断异常是不是RuntimeException,
        *         如果是一定回滚。
        *
        */
      /* @Transactional(
               propagation = Propagation.REQUIRED,
               isolation = Isolation.DEFAULT,
               readOnly = false,
               rollbackFor = {
                       NullPointerException.class,  NotEnoughException.class
               }
       )*/
   
       //这里是事务控制的默认值， 默认的传播行为是REQUIRED，默认的隔离级别DEFAULT
       //默认抛出运行时异常，回滚事务。
       //只能用在public
       @Transactional
       @Override
       public void buy(Integer goodsId, Integer nums) {
       ...
       }
   ```



### xml式

适合大型项目，有很多的类，方法，需要大量的配置事务，使用aspectj框架功能，在spring配置文件中
  声明类，方法需要的事务。这种方式业务方法和事务配置完全分离。

  实现步骤： 都是在xml配置文件中实现。 
   1)要使用的是aspectj框架，需要加入依赖

```xml
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-aspects</artifactId>
		<version>5.2.5.RELEASE</version>
	</dependency>
```

2）声明事务管理器对象

```xml
    <!--使用spring的事务处理-->
    <!--1. 声明事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--连接的数据库， 指定数据源-->
        <property name="dataSource" ref="myDataSource" />
    </bean>
```

3）声明方法需要的事务类型（配置方法的事务属性【隔离级别，传播行为，超时】）

```xml
<!--2.声明业务方法它的事务属性（隔离级别，传播行为，超时时间）
      id:自定义名称，表示 <tx:advice> 和 </tx:advice>之间的配置内容的
      transaction-manager:事务管理器对象的id
-->

<!--这里只能配对方法名，无法对类/包名配对-->
<tx:advice id="myAdvice" transaction-manager="transactionManager">
    <!--tx:attributes：配置事务属性-->
    <tx:attributes>
        <!--tx:method：给具体的方法配置事务属性，method可以有多个，分别给不同的方法设置事务属性
            name:方法名称，1）业务方法的完整的方法名称，不带有包和类。
                          2）方法可以使用通配符,* 表示任意字符
            propagation：传播行为，枚举值
            isolation：隔离级别
            rollback-for：你指定的异常类名，全限定类名。 发生异常一定回滚，否则运行时异常回滚，什么都不写也行
            以下不同name根据通配符的不同有查询优先级
        -->
        <!--第1次匹配，找不到就到下一次匹配-->
        <tx:method name="buy" propagation="REQUIRED" isolation="DEFAULT"
                   rollback-for="java.lang.NullPointerException,com.bjpowernode.excep.NotEnoughException"/>
        <!--第2次匹配，找不到就到下一次匹配-->
        <!--使用通配符，指定很多的方法-->
        <tx:method name="add*" propagation="REQUIRES_NEW" />
        
        <!--第3次匹配-->
        <!--表示所有方法-->
        <tx:method name="*" propagation="SUPPORTS" read-only="true" />
    </tx:attributes>
    </tx:advice>

 
```
```xml
<!--在上面的基础上配置aop,就可以指定类/包名了-->
<aop:config>
    <!--配置切入点表达式：指定哪些包中类，要使用事务
        id:切入点表达式的名称，唯一值
        expression：切入点表达式，指定哪些类要使用事务，aspectj会创建代理对象		
    -->
    <aop:pointcut id="servicePt" expression="execution(* *..service..*.*(..))"/>

    <!--配置增强器：关联adivce和pointcut
       advice-ref:通知，是上面<tx:advice>那里的id值
       pointcut-ref：切入点表达式<aop:pointcut>的id值
    -->
    <aop:advisor advice-ref="myAdvice" pointcut-ref="servicePt" />
</aop:config>
```

# 第**6**章**Spring** 与 **Web**

> 在 Web 项目中使用 Spring 框架，首先要解决在 web 层（这里指 Servlet）中获取到 Spring容器的问题。只要在 web 层获取到了 Spring 容器，便可从容器中获取到 Service 对象。

## **6.1 Web** 项目使用 **Spring** 的问题**(**了解**)**

一般来说，在Servlet里使用Spring 的容器时，先要new一个ApplicationContext对象后，才能从Spring 容器里拿出对象。但是在web里会有多位用户反复调用这个Servlet方法，这样的话，ApplicationContext对象过多创建会大量消耗内存。

为了解决这个问题，只能提前创建一个ApplicationContext对象，然后调用它

```java
        //创建spring的容器对象
        String config= "spring.xml";
        ApplicationContext ctx = new ClassPathXmlApplicationContext(config);
        StudentService service  = (StudentService) ctx.getBean("studentService");
	   ....
```

## 6.2获取 Spring 容器对象

这里是把放ApplicationContext到应用域ServletContext里

```xml
    <!--pom里加，监听器依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

web.xml

```xml
    <!--注册监听器ContextLoaderListener
        监听器被创建对象后，会读取/WEB-INF/spring.xml
        为什么要读取文件：因为在监听器中要创建ApplicationContext对象，需要加载配置文件。
        /WEB-INF/applicationContext.xml就是监听器默认读取的spring配置文件路径

        可以修改默认的文件位置，使用context-param重新指定文件的位置

        配置监听器：目的是创建容器对象，创建了容器对象， 就能把spring.xml配置文件中的所有对象都创建好。
        用户发起请求就可以直接使用对象了。
    -->
    <context-param>
        <!-- contextConfigLocation:表示配置文件的路径  -->
        <param-name>contextConfigLocation</param-name>
        <!--自定义配置文件的路径-->
        <param-value>classpath:spring.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
```

在Select里使用时

```java
 public class Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
/*
        创建spring的容器对象
        WebApplicationContext ctx = null;
        //获取ServletContext中的容器对象，创建好的容器对象，拿来就用
        String key =  WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE;
        Object attr  = getServletContext().getAttribute(key);
        if( attr != null){
            ctx = (WebApplicationContext)attr;
        }
*/
//下面是简化写法
WebApplicationContext ctx = WebApplicationContextUtils.getRequiredWebApplicationContext(getServletContext());
StudentService service  = (StudentService) ctx.getBean("studentService");
//获得对象 ctx 了
....
    
}
```

```
org.springframework.web.context.ContextLoaderListener
```





# 归纳：

## 路径

因为路径格式不一样，所以这里总结一下

```xml
<!--创建File,使用构造注入-->    
	<bean id="myfile" class="java.io.File">
         <!--绝对路径-->
        <constructor-arg name="parent" value="D:\course\JavaProjects\spring-course\ch01-hello-spring" />
        <!--相对于工程目录下-->
        <constructor-arg name="child" value="readme.txt" />
    </bean>
```



```java
//根目录从resources下开始
String config="ba04/applicationContext.xml";
ApplicationContext ac = new ClassPathXmlApplicationContext(config);
```



```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--只用于引用其他配置文件，路径从target/classpath（或者resources）开始-->
<import resource="classpath:ba06/spring-*.xml" />
    
</beans>
```



xml配置文件里所有值（等号右边的部分）都要加双引号

```xml
   <!--xml的<beans></beans>会二次扫描，所以不用担心ref因为位置不同而找不到-->
	<bean id="myStudent" class="com.bjpowernode.ba02.Student" >
        <property name="school" ref="mySchool" /><!--setSchool(mySchool)-->
	</bean>

    <!--声明School对象-->
    <bean id="mySchool" class="com.bjpowernode.ba02.School">
    </bean>
```

