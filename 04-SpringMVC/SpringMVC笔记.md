# 第**1**章 **SpringMVC** 概述

## **1.1 SpringMVC** 简介

SpringMVC 也叫 Spring web mvc。是 Spring 框架的一部分，是ResponseBody在 Spring3.0 后发布的。

注意tomcat10无法用下面的方法，要改到tomcat9才行

## **1.2 SpringMVC** 优点

1. 基于 MVC 架构，功能分工明确。解耦合，

2. 容易理解，上手快；使用简单。就可以开发一个注解的 SpringMVC 项目，SpringMVC 也是轻量级的，jar 很小。不依赖的特定的接口和类。

3. 作 为 **Spring** 框 架 一 部 分 ， 能 够 使 用 **Spring** 的 **IoC** 和**Aop** 。 方 便 整 合**Strtus,MyBatis,Hiberate,JPA** 等其他框架。

4. SpringMVC** 强化注解的使用，在控制器，**Service**，**Dao**都可以使用注解。方便灵活。使用@Controller 创建处理器对象,@Service 创建业务对象，@Autowired 或者@Resource在控制器类中注入 Service, Service 类中注入 Dao。

5. 使用@Controller注解创建的是一个普通类的对象， 不是Servlet。 springmvc赋予了控制器对象一些额外的功能。

6. web开发底层是servlet， springmvc中有一个对象是Servlet ： DispatherServlet(中央调度器)

   DispatherServlet: 负责接收用户的所有请求， 用户把请求给了DispatherServlet， 之后DispatherServlet把请求转发给我们的@Controller对象， 最后是@Controller对象处理请求。

   

## **1.3** 第一个注解的 **SpringMVC** 程序

所谓 SpringMVC 的注解式开发是指，在代码中通过对类与方法的注解，便可完成处理器

在 springmvc 容器的注册。注解式开发是重点。



### **1.3.1** 新建 **maven web** 项目



### 1.3.2 pom.xml

在创建好 web 项目后，加入 Servlet 依赖，SpringMVC 依赖

依赖：

```xml
    <!--测试依赖-->
  <dependency>
	<groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <!--servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <!--springmvc依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```



### **1.3.3** 注册中央调度器

（**1**） 全限定性类名

该中央调度器为一个 Servlet，名称为 DispatcherServlet。中央调度器的全限定性类名在导入的 Jar 文件 spring-webmvc-5.2.5.RELEASE.jar 的第一个包org.springframework.web.servlet

下可找到。

（**2**） web.xml的**\<load-on-startup/\>**和配置文件位置与名称

```xml
 <!--声明，注册springmvc的核心对象DispatcherServlet，需要在tomcat服务器启动后，创建DispatcherServlet对象的实例。
        为什么要创建DispatcherServlet对象的实例呢？
        因为DispatcherServlet在他的创建过程中， 会同时创建springmvc容器对象，读取springmvc的配置文件，把这个配置文件中的对象都创建好， 当用户发起请求时就可以直接使用对象了。

        servlet的初始化会执行init（）方法。 DispatcherServlet在init（）中{
           //创建容器，读取配置文件
           WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc.xml");
           //把容器对象放入到ServletContext中
           getServletContext().setAttribute(key, ctx);
        }

        启动tomcat报错，DispatcherServlet默认读取这个文件 /WEB-INF/springmvc-servlet.xml（<servlet-name>里的名字+“-servlet”）
        springmvc创建容器对象时，读取的配置文件默认是/WEB-INF/<servlet-name>-servlet.xml .
    -->    
<servlet>
        <servlet-name>myweb</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--自定义springmvc读取的配置文件的位置-->
        <init-param>
            <!--springmvc的配置文件的位置的属性-->
            <param-name>contextConfigLocation</param-name>	
            <!--指定自定义文件的位置-->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>

        <!--在tomcat启动后，创建Servlet对象
            load-on-startup:表示tomcat启动后创建对象的顺序。
						它的值是整数，<0 默认,  >=0 启动时就加载并初始化，
                           数值越小，该Servlet的优先级就越高，其被创建的也就越早，数值相等时，容器会自己选择创建顺序
        -->
        <load-on-startup>1</load-on-startup>
</servlet>
```

（**3**）web.xml的 **\<url-pattern/\>**

对于\<url-pattern/\>，可以写为 / ，建议写为\*.do 的形式。

```xml
    <servlet-mapping>
        <servlet-name>myweb</servlet-name>
        <!--
            使用框架的时候， url-pattern可以使用两种值
            1. 使用扩展名方式， 语法 *.xxxx , xxxx是自定义的扩展名。 常用的方式 *.do, *.action, *.mvc等等
               不能使用 *.jsp
               http://localhost:8080/myweb/some.do
               http://localhost:8080/myweb/other.do
            2.使用斜杠 "/"，写在2.4
        -->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
```



### **1.3.4** 创建 **SpringMVC** 配置文件

在工程的类路径即 src 目录下创建 SpringMVC 的配置文件 springmvc.xml。该文件名可以任意命名。

![image-20220324151112060](.\SpringMVC笔记.assets\image-20220324151112060.png)

### **1.3.5** 创建处理器

在类上与方法上添加相应注解即可。

@Controller：放在类上，表示当前类为处理器

@RequestMapping：表示当前方法为处理器方法。该方法要对 value 属性所指定的 URI 进行处理与响应。被注解的方法的方法名可以随意。

```java
/**
 *  @Controller:创建处理器对象，对象放在springmvc容器中。
 *  位置：在类的上面
 *  和Spring中讲的@Service ,@Component用法一样
 *
 *  能处理请求的都是控制器（处理器）： MyController能处理请求，叫做后端控制器（back controller）
 *  没有注解之前，需要实现各种不同的接口才能做控制器使用
 */

/**
 * @RequestMapping:
 *    value ： 所有请求地址的公共部分，叫做模块名称，可以自动在方法的@RequestMapping的value里加上前缀
 *    位置： 放在类的上面
 */
@Controller
@RequestMapping("/user")
public class MyController {
    /*
       处理用户提交的请求，springmvc中是使用方法来处理的。
       方法是自定义的， 可以有多种返回值， 多种参数，方法名称自定义
     */
    /**
     * 准备使用doSome方法处理some.do请求。
     * @RequestMapping: 请求映射，作用是把一个请求地址和一个方法绑定在一起。
     *                  一个请求指定一个方法处理。
     *       属性： 1. value 是一个String，表示请求的uri地址的（some.do）。
     *                value的值必须是唯一的， 不能重复。 在使用时，推荐地址以“/”
     *				可以有多个，用","分隔
     *			  2.method， 表示请求的方式。 它的值RequestMethod.xxx,xxx是枚举值。
     *			  常用： get方式， RequestMethod.GET,post方式,RequestMethod.POST
     *			  不写就表示，没有限制
     *       位置：1.在方法的上面，常用的。
     *            2.在类的上面
     *  说明： 使用RequestMapping修饰的方法叫做处理器方法或者控制器方法。
     *  使用@RequestMapping修饰的方法可以处理请求的，类似Servlet中的doGet, doPost
     *
     *  返回值：ModelAndView 表示本次请求的处理结果
     *   Model: 数据，请求处理完成后，要显示给用户的数据
     *   View: 视图， 比如jsp等等。
     */
  
    @RequestMapping(value = {"/some.do","/first.do"},method = RequestMethod.GET)
    public ModelAndView doSome(){  // doGet()--service请求处理
        //处理some.do请求了。 相当于service调用处理完成了。
        ModelAndView mv  = new ModelAndView();
        //添加数据， 框架在请求的最后把数据放入到request作用域。
        mv.addObject("msg","欢迎使用springmvc做web开发");
        mv.addObject("fun","执行的是doSome方法");

        //1.指定视图, 指定视图的完整路径
        //框架对视图执行的forward操作， request.getRequestDispather("/show.jsp).forward(...)
        //mv.setViewName("/show.jsp");
        //mv.setViewName("/WEB-INF/view/show.jsp");
        //mv.setViewName("/WEB-INF/view/other.jsp");


        //2.当配置了视图解析器后，可以使用逻辑名称（文件名），指定视图
        //框架会使用视图解析器的前缀 + 逻辑名称 + 后缀 组成完成路径， 这里就是字符连接操作
        ///WEB-INF/view/ + show + .jsp
        mv.setViewName("show");
        //mv.setView( new RedirectView("/a.jsp"));

        //返回mv
        return mv;
    }
}
```

若有多个请求路径均可匹配该处理器方法的执行，则@RequestMapping 的 value 属性中可以写上一个数组。

ModelAndView 类中的 addObject()方法用于向其 Model 中添加数据。Model 的底层为一个 HashMap。

Model 中的数据存储在 request 作用域中，SringMVC 默认采用转发的方式跳转到视图，本次请求结束，模型中的数据被销毁。

### **1.3.6** 声明组件扫描器和修改视图解析器的注册

在 springmvc.xml 中注册组件扫描器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">

    <!--声明组件扫描器-->
    <context:component-scan base-package="com.bjpowernode.controller" />

    <!--声明springmvc框架中的视图解析器，在跳转路径时，会自动加上前缀和后缀-->
    <!--SpringMVC 框架为了避免对于请求资源路径与扩展名上的冗余，在视图解析器InternalResouceViewResolver 中引入了请求的前辍与后辍。而 ModelAndView中只需给出要跳转页面的文件名即可，对于具体的文件路径与文件扩展名，视图解析器会自动完成拼接。
把 show.jsp 文件放到 /WEB-INF/jsp/路径中-->
	   <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀：视图文件的路径-->
        <property name="prefix" value="/WEB-INF/view/" />
		<!--后缀：视图文件的扩展名-->
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```



### **1.3.7** 定义目标页面

在 webapp 目录下新建一个子目录 jsp。

注意：webapp/WEB-INF 目录下的外部（url）无法访问，只能通过tomcat内部跳转进入

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<p>第一个springmvc项目</p>
    <!--这里的用some.do-->
<p><a href="some.do">发起some.do的请求</a></p>
<p><a href="other.do">发起other.do的请求</a></p>
</body>
</html>
```



### **1.3.10** 使用 **SpringMVC** 框架 **web** 请求处理顺序

![image-20220324154809125](.\SpringMVC笔记.assets\image-20220324154809125.png)

### **1.4 SpringMVC** 的 **MVC** 组件**1.5 SpringMVC** 执行流程（理解）

![image-20220324154912320](.\SpringMVC笔记.assets\image-20220324154912320.png)

### 1.5 SpringMVC 执行流程（理解）

#### **1.5.1** 流程图

![image-20220324155057969](.\SpringMVC笔记.assets\image-20220324155057969.png)

#### **1.5.2** 执行流程简单分析

（1）浏览器提交请求到中央调度器

（2）中央调度器直接将请求转给处理器映射器。

（3）处理器映射器会根据请求，找到处理该请求的处理器，并将其封装为处理器执行链后返回给中央调度器。

（4）中央调度器根据处理器执行链中的处理器，找到能够执行该处理器的处理器适配器。

（5）处理器适配器调用执行处理器。

（6）处理器将处理结果及要跳转的视图封装到一个对象 ModelAndView 中，并将其返回给处理器适配器。

（7）处理器适配器直接将结果返回给中央调度器。

（8）中央调度器调用视图解析器，将 ModelAndView 中的视图名称封装为视图对象。

（9）视图解析器将封装了的视图对象返回给中央调度器

（10）中央调度器调用视图对象，让其自己进行渲染，即进行数据填充，形成响应对象。

（11）中央调度器响应浏览器。

# 第**2**章 **SpringMVC** 注解式开发

## **2.1 @RequestMapping** 定义请求规则

​	在1.3.5的代码里

## **2.2** 处理器方法的参数

处理器方法可以包含以下四类参数，这些参数会在系统调用时由系统自动赋值，即程序
员可在方法内直接使用。
➢ HttpServletRequest 类
➢ HttpServletResponse 类
➢ HttpSession 类
➢ 请求中所携带的请求参数

```java
//使用request对象接收请求参数，用纯servle的语法就行了
String strName = request.getParameter("name");
String strAge = request.getParameter("age");
```

###  **2.2.1** 参数接收



```jsp
     <form action="receiveproperty.do" method="post">
          姓名：<input type="text" name="name"> <br/>
          年龄：<input type="text" name="age"> <br/>
          <input type="submit" value="提交参数">
     </form>
```



```java
@Controller
public class MyController {
    /**
     * 逐个接收请求参数：
     *   要求： 处理器（控制器）方法的形参名和请求中参数名必须一致。
     *          同名的请求参数赋值给同名的形参
      框架接收请求参数的原理
     *      springmvc框架通过 DispatcherServlet 的request.getParameter（）方法把参数得到，之后调用 MyController的doSome()方法时，按名称对应，把接收的参数转发给形参
     *      doSome（strName，Integer.valueOf(strAge)）
     *      框架会提供类型转换的功能，能把String转为 int ，long ， float， double等类型。
     *		如果传进来的是 空字符串 ，因为 "" 无法转成数字 所以会报400错误
     *
     *  400状态码是客户端错误， 表示提交请求参数过程中，发生了问题。
     */
    @RequestMapping(value = "/receiveproperty.do")
    public ModelAndView doSome(String name, Integer age){
        System.out.println("doSome, name="+name+"   age="+age);
        //可以在方法中直接使用 name ， age
        //处理some.do请求了。 相当于service调用处理完成了。
        ModelAndView mv  = new ModelAndView();
        mv.addObject("myname",name);
        mv.addObject("myage",Integer.valueOf(age));
        //show是视图文件（.jsp）的逻辑名称（文件名称）
        mv.setViewName("show");
        return mv;
    }

    /**
     * 请求中参数名和处理器方法的形参名不一样
     * @RequestParam: 逐个接收请求参数中， 解决请求中参数名形参名不一样的问题
     *      属性： 1. value 请求中的参数名称
     *            2. required 是一个boolean类型，默认是true
     *                true：表示请求中必须包含此参数，不传参的话就400错误。
     *      位置： 在处理器方法的形参定义的前面
     */
    @RequestMapping(value = "/receiveparam.do")
    public ModelAndView receiveParam(@RequestParam(value = "rname",required = false) String name,
                                     @RequestParam(value = "rage",required = false) Integer age){
        System.out.println("doSome, name="+name+"   age="+age);
        //可以在方法中直接使用 name ， age
        //处理some.do请求了。 相当于service调用处理完成了。
        ModelAndView mv  = new ModelAndView();
        mv.addObject("myname",name);
        mv.addObject("myage",age);
        //show是视图文件的逻辑名称（文件名称）
        mv.setViewName("show");
        return mv;
    } 
    
    /**
     * 处理器方法形参是java对象， 这个对象的属性名和请求中参数名一样的
     * 框架会创建形参的java对象， 给属性赋值。 请求中的参数是name，框架会调用setName()
     * @RequestParam在这里无法使用。
     */
    @RequestMapping(value = "/receiveobject.do")
    public ModelAndView receiveParam(Student myStudent){
        System.out.println("receiveParam, name="+myStudent.getName()+"   age="+myStudent.getAge());
        //可以在方法中直接使用 name ， age
        //处理some.do请求了。 相当于service调用处理完成了。
        ModelAndView mv  = new ModelAndView();
        mv.addObject("myname",myStudent.getName());
        mv.addObject("myage",myStudent.getAge());
        mv.addObject("mystudent",myStudent);
        //show是视图文件的逻辑名称（文件名称）
        mv.setViewName("show");
        return mv;
    }
}


```

### **2.2.2** 请求参数中文乱码问题

jsp向控制器方法发送中文（post式）的时候，会有乱码。

Spring 对于请求参数中的中文乱码问题，给出了专门的字符集过滤器： spring-web-5.2.5.RELEASE.jar 的org.springframework.web.filter 包下的 CharacterEncodingFilter 类。（放在web.xml里）

```xml
 <!--多个过滤器同时执行的时候，就好把这个放在最前面，因为过滤器的执行是按照其注册顺序进行的
	注册声明过滤器，解决post请求乱码的问题
	参数：<filter-name> 名字自定义-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--设置项目中使用的字符编码-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <!--强制请求对象（HttpServletRequest）使用encoding编码的值-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!--强制应答对象（HttpServletResponse）使用encoding编码的值-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
         <!--失效过滤器目标的名字-->
        <filter-name>characterEncodingFilter</filter-name>
        <!--
           /*:表示强制所有的请求先通过过滤器处理。
        -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 

## **2.3** 处理器方法的返回值

使用@Controller 注解的处理器的处理器方法，其返回值常用的有四种类型：
➢第一种：ModelAndView
➢第二种：String
➢第三种：无返回值 void
➢第四种：返回自定义类型对象
根据不同的情况，使用不同的返回值。

### **2.3.1** 返回 **ModelAndView**

```xml
     <form action="receiveproperty.do" method="post">
          姓名：<input type="text" name="name"> <br/>
          年龄：<input type="text" name="age"> <br/>
          <input type="submit" value="提交参数">
     </form>
```



```java
   /**
     * @ModelAndView
     * 携带数据并转发到其他地方
     */
    @RequestMapping(value = "/receiveobject.do")
    public ModelAndView receiveParam( Student myStudent){
        ModelAndView mv  = new ModelAndView();
        //基本数据类型和应用类型都可以用
        mv.addObject("myname",myStudent.getName());
        mv.addObject("myage",myStudent.getAge());
        mv.addObject("mystudent",myStudent);
        //show是视图文件的逻辑名称（文件名称），要加视图解析器才可以这么写
        mv.setViewName("show");
        return mv;
    }
```



```jsp
<!--如果跳转到jsp页面的话,花括号里写mv.addObject(key,value)里的key键-->
    <h3>myname数据：${myname}</h3><br/>
    <h3>myage数据：${myage}</h3>
    <h3>student数据：${mystudent}</h3>
```



### **2.3.2** 返回 **String**（视图）



```java
	/**
     * 处理器方法返回String--表示逻辑视图名称，需要配置视图解析器
     */
    @RequestMapping(value = "/returnString-view.do")
    public String doReturnView(HttpServletRequest request,String name, Integer age){
        System.out.println("doReturnView, name="+name+"   age="+age);
        //可以自己手工添加数据到request作用域
        request.setAttribute("myname",name);
        request.setAttribute("myage",age);
        // show : 逻辑视图名称，注意项目中要配置视图解析器，不然要写全名
        // 框架对视图执行forward转发操作
        return "show";
    }

    //处理器方法返回String,表示完整视图路径， 此时不能配置视图解析器
    @RequestMapping(value = "/returnString-view2.do")
    public String doReturnView2(HttpServletRequest request,String name, Integer age){
        System.out.println("===doReturnView2====, name="+name+"   age="+age);
        //可以自己手工添加数据到request作用域
        request.setAttribute("myname",name);
        request.setAttribute("myage",age);
        // 完整视图路径，项目中不能配置视图解析器
        return "/WEB-INF/view/show.jsp";
    }
```



### **2.3.3** 返回 **void**（了解）

对于处理器方法返回 void 的应用场景，AJAX 响应.
若处理器对请求处理后，无需跳转到其它任何资源（局部刷新），此时可以让处理器方法返回 void。

可以被（ 返回对象 **Object**）代替

```java
	//处理器方法返回void， 响应ajax请求
    //手工实现ajax，json数据： 代码有重复的 1. java对象转为json； 2. 通过HttpServletResponse输出json数据
    @RequestMapping(value = "/returnVoid-ajax.do")
    public void doReturnVoidAjax(HttpServletResponse response, String name, Integer age) throws IOException {
       //处理ajax， 使用json做数据的格式
       //service调用完成了， 使用Student表示处理结果
        Student student  = new Student("张飞同学"，20);

        String json = "";
        //把结果的对象转为json格式的数据
        if( student != null){
            ObjectMapper om  = new ObjectMapper();
            json  = om.writeValueAsString(student);
            System.out.println("student转换的json===="+json);
        }

        //输出数据，响应ajax的请求
        response.setContentType("application/json;charset=utf-8");
        PrintWriter pw  = response.getWriter();
        pw.println(json);
        pw.flush();
        pw.close();
		
        //返回Object内容里可以包装，不用写这么多
    }
```



```jsp
<!--ajax要导入jquery包才可以使用-->     
<script type="text/javascript" src="js/jquery-3.4.1.js"></script>
     <script type="text/javascript">
         <!--接收参数时-->
         $(function(){
             $("button").click(function(){
                 $.ajax({
                     url:"returnVoid-ajax.do",
                     <!--传出的数据-->
                     data:{
                         name:"zhangsan",
                         age:20
                     },
                     type:"post",
                     <!--请求格式-->
                     dataType:"json",
                     //dataType:"json",
                     <!--接收参数，在服务端把Object对象转为json的格式发过来-->
                     success:function(resp){
                         //resp从服务器端返回的是json格式（要设置）的字符串 {"name":"zhangsan","age":20}
                         //jquery会把字符串转为json对象， 赋值给resp形参。
                         alert(resp.name + "    "+resp.age);
                     }
                 })
             })
         })
     </script>

<!--提交参数时和以前一样-->
<button id="btn">发起ajax请求</button>
</form>
```



### **2.3.4** 返回对象 **Object**

处理器方法也可以返回 Object 对象。这个 Object 可以是
Integer，String，自定义对象，Map，List 等。但返回的对象不是作为逻辑视图出现的，而是作为直接在页面显示的数据出现的。

返回对象，需要使用@ResponseBody 注解，将转换后的 JSON 数据放入到响应体中。



> ```java
> ch04-return: 处理器方法的返回值表示请求的处理结果
> 1.ModelAndView: 有数据和视图，对视图执行forward。
> 2.String:表示视图，可以逻辑名称，也可以是完整视图路径
> 3.void: 不能表示数据，也不能表示视图。
>   在处理ajax的时候，可以使用void返回值。 通过HttpServletResponse输出数据。响应ajax请求。
>   ajax请求服务器端返回的就是数据， 和视图无关。
> 4.Object： 例如String ， Integer ， Map,List, Student等等都是对象，
>   对象有属性， 属性就是数据。 所以返回Object表示数据， 和视图无关。
>   可以使用对象表示的数据，响应ajax请求。
> 
>   现在做ajax， 主要使用json的数据格式。 实现步骤：
>    1.加入处理json的工具库的依赖， springmvc默认使用的jackson。
>    2.在sprigmvc配置文件之间加入 <mvc:annotation-driven> 注解驱动。
>      json  = om.writeValueAsString(student);
>    3.在处理器方法的上面加入@ResponseBody注解
>        response.setContentType("application/json;charset=utf-8");
>        PrintWriter pw  = response.getWriter();
>        pw.println(json);
> 
>   springmvc处理器方法返回Object， 可以转为json输出到浏览器，响应ajax的内部原理
>   1. <mvc:annotation-driven> 注解驱动。
>      注解驱动实现的功能是 完成java对象到json，xml， text，二进制等数据格式的转换。
>      <mvc:annotation-driven>在加入到springmvc配置文件后， 会自动创建HttpMessageConverter接口
>      的7个实现类对象， 包括 MappingJackson2HttpMessageConverter （使用jackson工具库中的ObjectMapper实现java对象转为json字符串）
> 
>      HttpMessageConverter接口：消息转换器。
>      功能：定义了java转为json，xml等数据格式的方法。 这个接口有很多的实现类。
>            这些实现类完成 java对象到json， java对象到xml，java对象到二进制数据的转换
> 
>      下面的两个方法是控制器类把结果输出给浏览器时使用的：
>      boolean canWrite(Class<?> var1, @Nullable MediaType var2);
>      void write(T var1, @Nullable MediaType var2, HttpOutputMessage var3)
> 
> 
>      例如处理器方法
>      @RequestMapping(value = "/returnString.do")
>      public Student doReturnView2(HttpServletRequest request,String name, Integer age){
>              Student student = new Student();
>              student.setName("lisi");
>              student.setAge(20);
>              return student;
>      }
> 
>      1）canWrite作用检查处理器方法的返回值，能不能转为var2表示的数据格式。
>        检查student(lisi，20)能不能转为var2表示的数据格式。如果检查能转为json，canWrite返回true
>        MediaType：表示数格式的， 例如json， xml等等
> 
>      2）write：把处理器方法的返回值对象，调用jackson中的ObjectMapper转为json字符串。
>         json  = om.writeValueAsString(student);
> 
> 
>  2. @ResponseBody注解
>    放在处理器方法的上面， 通过HttpServletResponse输出数据，响应ajax请求的。
>            PrintWriter pw  = response.getWriter();
>            pw.println(json);
>            pw.flush();
>            pw.close();
> ```

#### （**1**） 环境搭建

**A**、 **maven pom.xml**

```xml
   <!--Jackson依赖-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>

```

**B**、 声明注解驱动

```xml
    <!--注解驱动,放在springMVC的核心xml里
        有多个名字相同的，选择结尾是“mvc”的就行-->
    <mvc:annotation-driven/>
```

下面有详细解释，总之，作用是 创建几个实体类对象，作用是重对象转化成其他数据类型（json，二进制等等）



1. 将 Object 数据转化为 JSON 数据，需要由消息转换器 HttpMessageConverter 完成。而转换器的开启，需要由\<mvc:annotation-driven/\>来完成。
2. SpringMVC 使用消息转换器实现请求数据和对象，处理器方法返回对象和响应输出之间的自动转换
3. 当 Spring容器进行初始化过程中，在\<mvc:annotation-driven/\>处创建注解驱动时，默认创建了七个 HttpMessageConverter对象。也就是说，我们注册\<mvc:annotation-driven/\>，就是为了让容器为我们创建 HttpMessageConverter 对象。
4. HttpMessageConverter 接口 : **HttpMessageConverter\<T\>**是 Spring3.0新添加的一个接口，负责将请求信息转换为一个对象（类型为 **T**），将对象（类型为**T**）输出为响应信息
5. HttpMessageConverter\<T\>接口定义的方法：

6. boolean canRead(Class\<?\> clazz,MediaType mediaType):指定转换器可以读取的对象类型，即转 换 器 是 否 可 将 请 求 信 息 转 换 为clazz 类 型 的 对 象 ， 同 时 指 定 支持 MIME 类 型(text/html,applaiction/json 等)
7. boolean canWrite(Class\<?\> clazz,MediaType mediaType):指定转换器是否可将 clazz类型的对象写到响应流中，响应流支持的媒体类型在 MediaType 中定义。LIst\<MediaType\> getSupportMediaTypes()：该转换器支持的媒体类型。

8. T read(Class\<? extends T\> clazz,HttpInputMessageinputMessage)：将请求信息流转换为 T 类型的对象。

9. void write(T t,MediaType contnetType,HttpOutputMessgae outputMessage):将 T
   类型的对象写到响应流中，同时指定相应的媒体类型为 contentType
10. 加入注解驱动\<mvc:annotation-driven/\>后适配器类的 messageConverters 属性值


![image-20220325092725588](.\SpringMVC笔记.assets\image-20220325092725588.png)

版本不同，增加的数量也不一样

![image-20220325093023872](.\SpringMVC笔记.assets\image-20220325093023872.png)

#### （**2**）返回自定义类型对象

返回自定义类型对象时，不能以对象的形式直接返回给客户端浏览器，而是将对象转换为 JSON 格式的数据发送给浏览器的。

由于转换器底层使用了 Jackson 转换方式将对象转换为 JSON 数据，所以需要导入Jackson的相关 Jar 包。



```java
    /**
     * 处理器方法返回一个Student，通过框架转为json，响应ajax请求
     * @ResponseBody:
     *    作用：把处理器方法返回对象转为json后，通过HttpServletResponse输出给浏览器。
     *    位置：方法的定义上面。 和其它注解没有顺序的关系。
     * 返回对象框架的处理流程：
     *  1. 框架会把返回Student类型，调用框架的中ArrayList<HttpMessageConverter>中每个类的canWrite()方法
     *     检查那个HttpMessageConverter接口的实现类能处理Student类型的数据--MappingJackson2HttpMessageConverter
     *
     *  2.框架会调用实现类的write（）， MappingJackson2HttpMessageConverter的write()方法
     *    把李四同学的student对象转为json， 调用Jackson的ObjectMapper实现转为json
     *    contentType: application/json;charset=utf-8
     *
     *  3.框架会调用@ResponseBody把2的结果数据输出到浏览器， ajax请求处理完成
     */
    @ResponseBody
    @RequestMapping(value = "/returnStudentJson.do")
    public Student doStudentJsonObject(String name, Integer age) {
        //调用service，获取请求结果数据 ， Student对象表示结果数据
        Student student = new Student();
        student.setName("李四同学");
        student.setAge(20);
        return student; // 会被框架自动转为json，不用手动写

    }
```



```jsp
<!--ajax要导入jquery包才可以使用-->     
<script type="text/javascript" src="js/jquery-3.4.1.js"></script>
     <script type="text/javascript">
         <!--接收参数时-->
         $(function(){
             $("button").click(function(){
                 //alert("button click");
                 $.ajax({
                     url:"returnStudentJsonArray.do",
                     <!--传出的数据-->
                     data:{
                         name:"zhangsan",
                         age:20
                     },
                     type:"post",
                     <!--请求格式-->
                     dataType:"json",
                     <!--接收参数，在服务端把Object对象转为json的格式发过来-->
                     success:function(resp){
                         //resp从服务器端返回的是json格式（要设置）的字符串 {"name":"zhangsan","age":20}
                         //jquery会把字符串转为json对象， 赋值给resp形参。

                         // [{"name":"李四同学","age":20},{"name":"张三","age":28}]
                         //alert(resp.name + "    "+resp.age);

                         /*$.each(resp,function(i,n){
                             alert(n.name+"   "+n.age)
                         })*/
                         alert("返回的是文本数据："+resp);

                     }
                 })
             })
         })
     </script>

    <button id="btn">发起ajax请求</button>
</form>
```

#### （**3**） 返回 **List** 集合



```java
 /**
     *  处理器方法返回List<Student>
     * 返回对象框架的处理流程：
     *  1. 框架会把返回List<Student>类型，调用框架的中ArrayList<HttpMessageConverter>中每个类的canWrite()方法
     *     检查那个HttpMessageConverter接口的实现类能处理Student类型的数据--MappingJackson2HttpMessageConverter
     *
     *  2.框架会调用实现类的write（）， MappingJackson2HttpMessageConverter的write()方法
     *    把李四同学的student对象转为json， 调用Jackson的ObjectMapper实现转为json array
     *    contentType: application/json;charset=utf-8
     *
     *  3.框架会调用@ResponseBody把2的结果数据输出到浏览器， ajax请求处理完成
     */
    @RequestMapping(value = "/returnStudentJsonArray.do")
    @ResponseBody
    public List<Student> doStudentJsonObjectArray(String name, Integer age) {
        List<Student> list = new ArrayList<>();
        //调用service，获取请求结果数据 ， Student对象表示结果数据
        Student student1 = new Student("李四同学",20);
        Student student2 = new Student("张三",28);
        list.add(student1);
        list.add(student2);

        return list;
    }
```



```jsp
<!--ajax要导入jquery包才可以使用-->     
<script type="text/javascript" src="js/jquery-3.4.1.js"></script>
     <script type="text/javascript">
         <!--接收参数时-->
         $(function(){
             $("button").click(function(){
                 $.ajax({
                     url:"returnStudentJsonArray.do",
                     <!--传出的数据-->
                     data:{
                         name:"zhangsan",
                         age:20
                     },
                     type:"post",
                     <!--请求格式-->
                     dataType:"json",
                     <!--接收参数，在服务端把Object对象转为json的格式发过来-->
                     success:function(resp){
                         //jquery会把字符串转为json对象， 赋值给resp形参。

                         // [{"name":"李四同学","age":20},{"name":"张三","age":28}]
                         // jquer语法，循环读集合，resp是集合，i是下标，n是子元素
                         $.each(resp,function(i,n){
                             alert(n.name+"   "+n.age)
                         })
                     }
                 })
             })
         })
     </script>

    <button id="btn">发起ajax请求</button>
</form>
```



#### （**4**） 返回字符串对象

```java
    /**
     * 处理器方法返回的是String ， String表示数据的，不是视图。
     * 区分返回值String是数据，还是视图，看有没有@ResponseBody注解
     * 如果有@ResponseBody注解，返回String就是数据，反之就是视图
     *
     * 默认使用“text/plain;charset=ISO-8859-1”作为contentType,导致中文有乱码，
     * 解决方案：给RequestMapping增加一个属性 produces, 使用这个属性指定新的contentType.
     * 返回对象框架的处理流程：
     *  1. 框架会把返回String类型，调用框架的中ArrayList<HttpMessageConverter>中每个类的canWrite()方法
     *     检查那个HttpMessageConverter接口的实现类能处理String类型的数据--StringHttpMessageConverter
     *
     *  2.框架会调用实现类的write（）， StringHttpMessageConverter的write()方法
     *    把字符按照指定的编码处理 text/plain;charset=ISO-8859-1
     *
     *  3.框架会调用@ResponseBody把2的结果数据输出到浏览器， ajax请求处理完成
     */
    @ResponseBody
    @RequestMapping(value = "/returnStringData.do",produces = "text/plain;charset=utf-8")
    public String doStringData(String name,Integer age){
        return "Hello SpringMVC 返回对象，表示数据";
    }
```



```jsp
<!--ajax要导入jquery包才可以使用-->     
<script type="text/javascript" src="js/jquery-3.4.1.js"></script>
     <script type="text/javascript">
         <!--接收参数时-->
         $(function(){
             $("button").click(function(){
                 $.ajax({
                     url:"returnStringData.do",
                     <!--传出的数据-->
                     data:{
                         name:"zhangsan",
                         age:20
                     },
                     type:"post",
                     <!--请求格式-->
                     dataType:"text",
                     <!--接收参数，在服务端把Object对象转为json的格式发过来-->
                     success:function(resp){
                         alert("返回的是文本数据："+resp);
                     }
                 })
             })
         })
     </script>


```



## **2.4** 解读**\<url-pattern/\>**

```xml
<servlet-mapping>
    <servlet-name>myweb</servlet-name>
    <!--
        使用框架的时候， url-pattern可以使用两种值
        1. 使用扩展名方式， 语法 *.xxxx , xxxx是自定义的扩展名。 常用的方式 *.do, *.action, *.mvc等等
           不能使用 *.jsp
           http://localhost:8080/myweb/some.do
           http://localhost:8080/myweb/other.do
        2.使用斜杠 "/"，写在2.4
    -->
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

**2.4.1** 配置详解

（**1**） **\*.do**

在没有特殊要求的情况下，SpringMVC 的中央调度器 DispatcherServlet的\<url-pattern/\>

常使用后辍匹配方式，如写为\*.do 或者 \*.action, \*.mvc 等。

（**2**） **/**

可以写为/，因为 DispatcherServlet会将向静态资源的获取请求，例如.css、.js、.jpg、.png等资源的获取请求，当作是一个普通的 Controller请求。中央调度器会调用处理器映射器为其查找相应的处理器。当然也是找不到的，所以在这种情况下，所有的静态资源获取请求也均会报 404 错误。



一般来说所有的请求都是由tomcat的default来负责转发的。

这时候<url-pattern>*.do</url-pattern>就表示先由SoringMVC来处理 *.do格式的请求（tomcat无法参与）

<url-pattern> / </url-pattern> 改用斜杠的话，它会替代 tomcat中的default的功能 , 也就是本应该通过tomcat处理的内容都被"拦截"了，而SpringMVC的中央调度器没能力处理静态资源文件，所以就报错了。同时动态资源some.do（或jsp等）是可以访问，的因为我们程序中有MyController控制器对象，能处理some.do请求。



**2.4.2** 静态资源访问

<url-pattern/\>的值并不是说写为/后，静态资源就无法访问了。经过一些配置后，该问题也是可以解决的。

#### （**1**） 使用**\<mvc:default-servlet-handler/\>**

在SpringMVC核心文件配置

```xml
    <!--声明组件扫描器-->
    <context:component-scan base-package="com.bjpowernode.controller" />

    <!--第一种处理静态资源的方式：
        需要在springmvc配置文件加入 <mvc:default-servlet-handler>
        原理是： 加入这个标签后，框架会创健控制器对象DefaultServletHttpRequestHandler（类似我们自己创建的MyController）.
        DefaultServletHttpRequestHandler这个对象可以把接收的所有请求转发给 tomcat的default这个servlet。
    -->
    <mvc:default-servlet-handler />

    <!-- default-servlet-handler 和 @RequestMapping注解（声明组件扫描器）有冲突， 需要加入annotation-driven 解决问题-->
    <mvc:annotation-driven />
```

#### （**2**） 使用**\<mvc:resources/\>**（掌握）

在SpringMVC核心文件配置

```xml
<!--第二种处理静态资源的方式
        mvc:resources 加入后框架会创建 ResourceHttpRequestHandler这个处理器对象。
        让这个对象处理静态资源的访问，不依赖tomcat服务器。
        mapping:网页访问静态资源的uri地址， 使用通配符 **
        location：静态资源在你的项目中的目录位置。

        /images/**:表示uri是 images/p1.jpg  , images/user/logo.gif , images/order/history/list.png等等
    -->
    <mvc:resources mapping="/images/**" location="/images/" />
    <mvc:resources mapping="/html/**" location="/html/" />
    <mvc:resources mapping="/js/**" location="/js/" />

    <!--mvc:resources和@RequestMapping有一定的冲突-->
    <mvc:annotation-driven />

    <!--可以把静态资源文件放在同一个文件夹里，这样可以只使用一个配置语句，指定多种静态资源的访问-->
    <!--<mvc:resources mapping="/static/**" location="/static/" />-->
```



# 第**4**章 **SpringMVC** 核心技术

## **4.1** 请求重定向和转发

forward:表示转发，实现 request.getRequestDispatcher("xx.jsp").forward()
redirect:表示重定向，实现 response.sendRedirect("xxx.jsp")

SpringMVC做了简写，可以更方便的使用了

### 4.1.1 请求转发

​	注意，对于请求转发的页面，可以是WEB-INF中页面；而重定向的页面，是不能为WEB-INF中页的。因为重定向相当于用户再次发出一次请求，而用户是不能直接访问 WEB-INF 中资源的。

```java
   /**
     * 处理器方法返回ModelAndView,实现转发
     * 语法： .setViewName("forward:视图文件完整路径")
     * forward特点： 不和视图解析器一同使用，就当项目中没有视图解析器
     * 作用是为了不受视图解析器限制
     */
    @RequestMapping(value = "/doForward.do")
    public ModelAndView doSome(){
        ModelAndView mv  = new ModelAndView();
        //显示转发
        //mv.setViewName("forward:/WEB-INF/view/show.jsp");

        mv.setViewName("forward:/hello.jsp");
        return mv;
    }

	//if String作为返回值
    @RequestMapping(value = "/doForward1.do")
    public String doSome(){
        return "forward:/hello.jsp";
    }
```



### **4.1.2** 请求重定向

```java
    /**
     * 处理器方法返回ModelAndView,实现重定向redirect
     * 语法：setViewName("redirect:视图完整路径")
     * redirect特点： 不和视图解析器一同使用，就当项目中没有视图解析器
     *
     * 框架对重定向的操作：
     * 1.框架会把Model中的简单类型的数据，转为string使用，作为hello.jsp的get请求参数使用。
     *   目的是在 doRedirect.do 和 hello.jsp 两次请求之间传递数据
     *
     * 2.在目标hello.jsp页面可以使用参数集合对象 ${param.}获取请求参数值
     *    ${param.myname}
     *
     * 3.重定向不能访问/WEB-INF资源
     */
    @RequestMapping(value = "/doRedirect.do")
    public ModelAndView doWithRedirect(String name,Integer age){
        ModelAndView mv  = new ModelAndView();
        //数据放入到 request作用域
        mv.addObject("myname",name);
        //重定向
        mv.setViewName("redirect:/hello.jsp");
        //目标页面的uri：http://localhost:8080/ch08_forard_redirect/hello.jsp ?myname=lisi&myage=22

        //重定向不能访问/WEB-INF资源
        //mv.setViewName("redirect:/WEB-INF/view/show.jsp");
        return mv;
    }
```

hello.jsp页面

```jsp
<!--下面这个是接收不到数据的，因为下面这个相当于 request.getAttribute("myname")-->
<h3>myname数据：${myname}</h3><br/>
<!--能接收到数据，下面两个的效果是一样的-->
<h3>myname数据：${param.myname}</h3><br/>
<h3>取参数数据：<%=request.getParameter("myname")%></h3>
<!-- -->
```

## **4.2** 自定义异常处理，@ExceptionHandler注解

使用spring的aop机制（动态代理），目标类只需要抛出异常即可，让框架统一处理，可以解耦合

不用处理异常



目录

![image-20220327103456702](.\SpringMVC笔记.assets\image-20220327103456702.png)

### 4.2.1 依赖

```xml
    <!--springmvc依赖，集成在mvc里-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

### 4.2.2自定义异常类

为了方便统一抛出异常，先创建一个父类，再创建多个继承子类

```
新建一个自定义异常类 MyUserException , 再定义它的子类NameException ,AgeException
```

```java
package com.bjpowernode.exception;
//父类MyUserException，继承异常extends Exception。
//处理两个异常
public class MyUserException extends Exception {
    //无参构造函数和有参构造函数
    public MyUserException() {
        super();
    }

    public MyUserException(String message) {
        super(message);
    }
}
```

```java
//当年龄有问题时，抛出的异常
public class AgeException extends MyUserException {
    public AgeException() {
        super();
    }
    public AgeException(String message) {
        super(message);
    }
}

```

```java
//表示当用户的姓名有异常，抛出NameException
public class NameException extends MyUserException {
    public NameException() {
        super();
    }
    public NameException(String message) {
        super(message);
    }
}

```

### 4.2.3全局异常处理类

```java
/**
 * @ControllerAdvice : 控制器增强（也就是说给控制器类增加功能--异常处理功能）
 *           位置：在类的上面。
 *  特点：必须让框架知道这个注解所在的包名，需要在springmvc配置文件声明组件扫描器。
 *  指定@ControllerAdvice所在的包名
 *  当抛出异常后,根据@ExceptionHandler()里的value值，会跳转到特定的方法里面进行处理
 */
@ControllerAdvice
public class GlobalExceptionHandler {
    //定义方法，处理发生的异常
    /*
        处理异常的方法和控制器方法的定义一样， 可以有多个参数，可以有ModelAndView,
        String, void,对象类型的返回值

        形参：Exception，表示Controller中抛出的异常对象,被这个方法接收。
        通过形参可以获取发生的异常信息。

        @ExceptionHandler(异常的class)：表示异常的类型，当发生此类型异常时，
        由当前方法处理
     */

    @ExceptionHandler(value = NameException.class)
    public ModelAndView doNameException(Exception exception){
        //处理NameException的异常。
        /*
           应该在这里写的东西：
           1.需要把异常记录下来， 记录到数据库，日志文件。
             记录日志发生的时间，哪个方法发生的，异常错误内容。
           2.发送通知，把异常的信息通过邮件，短信，微信发送给相关人员。
           3.给用户友好的提示。
           4.跳转到jsp等页面，向用户发送提示
         */
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","姓名必须是zs，其它用户不能访问");
        mv.addObject("ex",exception);
        mv.setViewName("nameError");
        return mv;
    }


    //处理AgeException
    @ExceptionHandler(value = AgeException.class)
    public ModelAndView doAgeException(Exception exception){
	...
    }

    //如果发送其它异常， NameException, AgeException以外，不知类型的异常，都会自动被这里接收
    //@ExceptionHandler里不写参数
    @ExceptionHandler
    public ModelAndView doOtherException(Exception exception){
        //处理其它异常
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","你的年龄不能大于80");
        mv.addObject("ex",exception);
        mv.setViewName("defaultError");
        return mv;
    }
}
```

### 4.2.4 创建springmvc的配置文件

```xml
    <!--处理需要的两步-->
	<!---->
	<!--扫描 全局异常处理类 的包，找到@ControllerAdvice和@ExceptionHandler两个注解-->
    <context:component-scan base-package="com.bjpowernode.handler" />
	<!--mvc:resources和@RequestMapping有一定的冲突-->
    <mvc:annotation-driven />
```

### 4.2.5 创建处理异常的视图页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
   defaultError.jsp <br/>
   提示信息：${msg} <br/>
   系统异常消息：${ex.message}
</body>
</html>
```

## **4.3** 拦截器

SpringMVC 中的 Interceptor 拦截器是非常重要和相当有用的，它的主要作用是拦截指定的用户请求，并进行相应的预处理与后处理。其拦截的时间点在“处理器映射器根据用户提交的请求映射出了所要执行的处理器类，并且也找到了要执行该处理器类的处理器适配器，在处理器适配器执行处理器之前”。当然，在处理器映射器映射出所要执行的处理器类时，已经将拦截器与处理器组合为了一个处理器执行链，并返回给了中央调度器

离开了SpringMVC框架，无法使用

### **4.3.1** 一个拦截器的执行

（**1**） 注册拦截器

在SpringMVC的核心文件

```xml
    <!--声明拦截器： 拦截器可以有0或多个-->
    <mvc:interceptors>
        <!--声明第一个拦截器-->
        <mvc:interceptor>
            <!--指定拦截的请求uri地址
                path：就是uri地址，可以使用通配符 **
                      ** ： 表示任意的字符，文件或者多级目录和目录中的文件
			   比如：
                http://localhost:8080/myweb/user/listUser.do
                http://localhost:8080/myweb/student/addStudent.do
            -->
            <mvc:mapping path="/**"/>
            <!--声明拦截器所在的类-->
            <bean class="com.bjpowernode.handler.MyInterceptor" />
        </mvc:interceptor>
    </mvc:interceptors>

```

（**2**） 拦截器类

有3个方法

```Java
//拦截器类：拦截用户的请求。
/*	执行顺序
拦截器的MyInterceptor的preHandle()
=====执行MyController中的doSome方法=====
拦截器的MyInterceptor的postHandle()
拦截器的MyInterceptor的afterCompletion()
*/
//添加implements HandlerInterceptor接口实现即可
public class MyInterceptor implements HandlerInterceptor {
    private long btime = 0;
    /*
     * preHandle叫做预处理方法。
     *   重要：是整个项目的入口，门户。 当preHandle返回true 请求可以被处理。
     *        preHandle返回false，请求到此方法就截止。
     *
     * 参数：
     *  Object handler ： 被拦截的控制器对象
     * 返回值boolean
     *   true：请求是通过了拦截器的验证，可以执行处理器方法。
     *   false：请求没有通过拦截器的验证，请求到达拦截器就截止了。 请求没有被处理
     *      拦截器的MyInterceptor的preHandle()
     *
     *  特点：
     *   1.方法在控制器方法（MyController的doSome）之前先执行的。
     *     用户的请求首先到达此方法
     *
     *   2.在这个方法中可以获取请求的信息， 验证请求是否符合要求。
     *     可以验证用户是否登录， 验证用户是否有权限访问某个连接地址（url）。
     *      如果验证失败，可以截断请求，请求不能被处理。
     *      如果验证成功，可以放行请求，此时控制器方法才能执行。
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        btime = System.currentTimeMillis();
        System.out.println("拦截器的MyInterceptor的preHandle()");
        //计算的业务逻辑，根据计算结果，返回true或者false
        //可以写个返回结果转发到浏览器
        //request.getRequestDispatcher("/tips.jsp").forward(request,response);
        return true;
    }

    /*
       postHandle:后处理方法。
       参数：
        Object handler：被拦截的处理器对象MyController
        ModelAndView mv:处理器方法的返回值

        特点：
         1.在处理器方法之后执行的（MyController.doSome()）
         2.能够获取到处理器方法的返回值ModelAndView,可以修改ModelAndView中的数据和视图，可以影响到最后的执行结果。
         3.主要是对原来的执行结果做二次修正，
		
		d
         ModelAndView mv = MyController.doSome();
         postHandle(request,response,handler,mv);
     */
    public void postHandle(HttpServletRequest request,HttpServletResponse response,
                           Object handler, ModelAndView mv) throws Exception {
        System.out.println("拦截器的MyInterceptor的postHandle()");
        //对原来的doSome执行结果，需要调整。
        if( mv != null){
            //修改数据
            mv.addObject("mydate",new Date());
            //修改视图
            mv.setViewName("other");
        }
    }

    /*
      afterCompletion:最后执行的方法
      参数
        Object handler:被拦截器的处理器对象
        Exception ex：程序中发生的异常
      特点:
       1.在请求处理完成后执行的。框架中规定是当你的视图处理完成后，对视图执行了forward。就认为请求处理完成。
       2.一般做资源回收工作的， 程序请求过程中创建了一些对象，在这里可以删除，把占用的内存回收。
     */
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                Object handler, Exception ex) throws Exception {
        System.out.println("拦截器的MyInterceptor的afterCompletion()");

        long etime = System.currentTimeMillis();
        System.out.println("计算从preHandle到请求处理完成的时间："+(etime - btime ));
    }
}
```

### **4.3.2** 多个拦截器的执行

```xml
    <!--声明拦截器： 拦截器可以有0或多个
        在框架中保存多个拦截器是ArrayList，
        按照声明的先后顺序放入到ArrayList
    -->
    <mvc:interceptors>
        <!--声明第一个拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <!--声明拦截器对象-->
            <bean class="com.bjpowernode.handler.MyInterceptor" />
        </mvc:interceptor>
        <!--声明第二个拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.bjpowernode.handler.MyInterceptor2" />
        </mvc:interceptor>
    </mvc:interceptors>
```



> 假如多个拦截器拦截一个请求的话，
>
> 第一个拦截器return true , 第二个拦截器return true
>
> 111111-拦截器的MyInterceptor的preHandle()
>
> 22222-拦截器的MyInterceptor的preHandle()
>
> =====执行MyController中的doSome方法=====
>
> 22222-拦截器的MyInterceptor的postHandle()
>
> 111111-拦截器的MyInterceptor的postHandle()
>
> 22222-拦截器的MyInterceptor的afterCompletion()
>
> 111111-拦截器的MyInterceptor的afterCompletion()
>
> \---------------------------------------------------
>
> 第一个拦截器preHandle=true , 第二个拦截器preHandle=false
>
> 111111-拦截器的MyInterceptor的preHandle()
>
> 22222-拦截器的MyInterceptor的preHandle()
>
> 111111-拦截器的MyInterceptor的afterCompletion()
>
> \----------------------------------------------------------
>
> 第一个拦截器preHandle=false , 第二个拦截器preHandle=true|false
>
> 111111-拦截器的MyInterceptor的preHandle()

下面是图像化流程

![image-20220327124124170](.\SpringMVC笔记.assets\image-20220327124124170.png)



一般使用方法，定义多个拦截器：登录验证-》权限验证-》日志记录

### 4.3.3 过滤器和拦截器的区别

1. 过滤器是servlet中的对象， 拦截器是框架中的对象
2. 过滤器实现Filter接口的对象， 拦截器是实现HandlerInterceptor
3. 过滤器是用来设置request，response的参数，属性的，侧重对数据过滤的。拦截器是用来验证请求的，能截断请求。
4. 过滤器是在拦截器之前先执行的。
5. 过滤器是tomcat服务器创建的对象拦截器是springmvc容器中创建的对象
6. 过滤器是一个执行时间点。拦截器有三个执行时间点
7. 过滤器可以处理jsp，js，html等等拦截器是侧重拦截对Controller的对象。 如果你的请求不能被DispatcherServlet接收， 这个请求不会执行拦截器内容
8. 拦截器拦截普通类方法执行，过滤器过滤servlet请求响应

# SpringMVC适配器

ApplicationContext ctx = new ClassPathxmlApplication ( "beans.xml");

studentservice service = (studentservice) ctx.getBean( "service") ;

springmvc内部请求的处理流程:也就是springmvc接收请求，到处理完成的过程

1.用户发起请求some . do
2.Dispatcherservlet接收请求some . do,把请求转交给处理器映射器
处理器映射器: springave框架中的一种对象，框架把实现了HandlerMapping接口的类都叫做映射器（多个)
处理器映射器作用:根据请求，从springnvc容器对象中获取处理器对象（MyController controller =ctx.getBean（“Some.do”）
框架把找到的处理器对象放到一个叫做处理器执行链(HandlerExecutionChain）的类保存

HandlerExecutionChain:类中保存君1．处理器对象（(MyController);2．项目中的所有的拦截器List<HandlerInterceptc

3.DispatcherServlet把2中的HandlerExecutionchain中的处理器对象交给了处理器适配器对象（多个)
处理器适配器: springmvc框架中的对象，需要实现HandlerAdapter接口。
处理器适配器作用:执行处理器方法（调用MyController.doSome () 得到返回值ModelAndView )

4.Dispatcherservlet把3中获取的ModelAndview交给了视图解析器对象。
视图解析器:springmvc中的对象，需要实现viewResoler接口(可以有多个)

视图解析器作用:组成视图完整路径，使用前缀，后缀。并创view对象。
		view是一个接口，表示视图的，在框架中jsp，html不是string表示，而是使用view和他的实现类表示视图。

# 路径注意事项：

1.路径

业务方法上面要写 斜杠

```java
@RequestMapping(value = {"/some.do","/first.do"})
```

jsp文件的her路径写的时候要去掉斜杠，

```jsp
<p><a href="some.do">发起some.do的请求</a> </p>
```

jsp里引入jquery包，以当前目录为相对目录

```jsp
     <script type="text/javascript" src="js/jquery-3.4.1.js"></script>
```

jsp跳转到后端寻找资源

```jsp
绝对地址：http://localhost:8080
<!--绝对地址，会跳转到http://localhost:8080/项目名 + /user/some.do -->
<a href="/user/some.do"></a>
```



```jsp
<!--相对地址：当前页面的地址去掉自己网页名字部分-->
<!--比如当前页面jsp地址是http://localhost:8080/项目名/user/a.jsp-->
<!--在a.jsp点击下面的地址会跳转到：http://localhost:8080/user/ + user/some.do -->
<a href="user/some.do"></a>

<!--相对路径并不稳定，在移动页面的时候，容易丢失链接 -->
<a href="/项目名/user/some.do"></a>
<!---->
```

```jsp
<!--1.动态获取项目名-->
<a href="${pageContext.request.contextPath}/user/some.do"></a>

<!--2.用<base>标签动态获取 “http://localhost:8080/项目名/” -->
<%
    String basePath = request.getScheme() + "://" +
            request.getServerName() + ":" + request.getServerPort() +
            request.getContextPath() + "/";
%>
    <base href="<%=basePath%>" />
	<!--自动添加前缀-->
    <a href="user/some.do"></a>

```

