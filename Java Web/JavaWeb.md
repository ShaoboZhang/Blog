# 一、引言

## 1.1 C/S架构和B/S架构

> C/S架构和B/S架构是应用软件发展过程中出现的两种架构方式。

### 1.1.1 C/S架构

> C/S: Client/Server 客户端/服务器

- 特点：软件需要在客户端安装，通过运行安装后的软件进行使用。
- 优点：
  - 非必需可不必连网运行。
  - 即使连网，因为可将不需连网传输的数据放置在本地，所以通过网络传输的数据量较小，因此对网速要求较低。
  - 也因为无需将大型数据(如图像、音频、视频等)通过网络传输，所以可以使图像显示效果更好。
- 缺点：
  - 服务端软件功能改动后，客户端也需要升级软件。
  - 因此这一模式不利于软件维护。

### 1.1.2 B/S架构

> B/S: Browser/Server 浏览器/服务器

- 特点：软件无需在客户端安装，通过浏览器进行访问运行。

- 优点：
  - 服务端软件功能改动后，客户无需做任何改动仍可使用新功能。
  - 因此这一模式非常利于软件维护。
- 缺点：
  - 数据通信依赖网络传输，因此程序的运行必须连网使用。
  - 因为所有数据通过网络传输，因此一旦有大型数据传输，则对网速要求较高。
  - 图像显示效果不如C/S架构的软件。

## 1.2 服务器

### 1.2.1 概念

#### (1) Web

> Web (World Wide Web): 简单理解就是网站，用来表示Internet主机上供外界访问的资源。

Web资源分为两类：

- 静态资源：指Web页面中，供人们浏览的数据是始终不变的，如：HTML、CSS。
- 动态资源：指Web页面中，供人们浏览的数据是由程序产生，不同情况下产生的结果可能发生变化，如：JSP/Servlet。

**说明：在Java中，动态Web资源开发技术统称为Java Web。**

#### (2) Web服务器

> Web服务器是指运行及发布Web应用的容器。

**说明：只有将开发的Web项目放置到该容器中，才能使网络中的用户通过浏览器进行访问。**

### 1.2.2 常见服务器

#### (1) 开源服务器

- Tomcat: 主流服务器之一，适合初学者。
- Jetty: 淘宝，运行效率比Tomcat高。
- Resin: 新浪，所有开源服务器软件中运行效率最高的。

说明：三者的用法从代码角度完全相同，只有开启、关闭服务器软件时对应的指令稍有区别，**因此掌握其中一个技术也即掌握三者全部**。

#### (2) 收费服务器

- WebLogic: Oracle
- WebSphere: IBM

说明：以上两者由供应商提供相应的服务与支持，但软件大、资源消耗高。

## 1.3 Tomcat服务器

> Tomcat是Apache软件基金会Jakarta项目中的一个核心项目，免费开源、支持Servlet和Jsp规范。

官网地址：https://tomcat.apache.org/index.html

### 1.3.1 版本说明 (重要)

> 自Tomcat 10起，Jakarta项目从Java EE迁移至Jakarta EE，因此基于其API的命名由`javax.*`变更为`jakarta.*`。

注意：

- 由于API变化，若使用Tomcat 10，则Servlet、JSP所依赖的包也发生改变。因此，在使用Maven导入相关依赖时需导入的仓库地址需发生改变。
- 但SpringMvc中有关Servlet的依赖仍在使用javax包，所以若项目为Spring项目，则目前(2022.1)还不能使用Tomcat 10。

### 1.3.2 下载与安装

- 软件版本：Tomcat 9
- 下载地址：https://tomcat.apache.org/download-90.cgi
- 安装路径：建议为纯英文、无特殊符号的路径下。

### 1.3.3 目录结构

| 文件夹  | 说明                          | 备注                                                         |
| ------- | ----------------------------- | ------------------------------------------------------------ |
| bin     | 存放二进制可执行脚本文件      | 服务器启动、关闭等脚本                                       |
| conf    | 存放配置文件                  | server.xml: 配置整个服务器信息，例如端口号等<br/>web.xml: 项目部署描述符文件 |
| lib     | 存放Tomcat运行所需要的jar文件 | 例如: servlet-api.jar、jsp-api.jar                           |
| logs    | 存放日志文件                  | 记录tomcat从启动到关闭整个运行时信息                         |
| temp    | 存放运行时的临时文件          | tomcat停止后文件会删除                                       |
| webapps | 存放web项目                   | 每个文件夹对应一个独立的项目                                 |
| work    | 存放运行时生成的文件          | 当用户访问JSP文件时，生成的.java文件和.class文件会存放在此   |

## 1.4 Java Web程序目录结构

- WEB-INF: 存放项目核心内容的目录。**该目录必须存在！**
  - classes: 存放.class的目录
  - lib: 存放外部依赖jar包的目录
  - web.xml: 项目配置文件，配置url路径到servlet的映射
- index.jsp: 欢迎页面。**该文件非必需文件。**
- static: 存放静态资源(html/css/img等)的目录。**该目录非必需存在。**

## 1.5 Servlet

> Servlet: Server Applet的简称，是服务器端的程序，可交互式地处理客户端发送到服务端的请求，并完成操作响应。

### 1.4.1 Servlet的作用

- 接收客户端的请求，完成操作。
- 生成**动态网页资源**，使页面数据可变。
- 将包含操作结果的动态网页资源响应给客户端。

### 1.4.2 Servlet开发步骤

#### (1) 编写Servlet程序

- 创建自定义类、实现`javax.servlet.Servlet`接口
- 重写接口方法
- 在`service`方法中编写核心输出语句

```java
package servlet;

import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class HelloServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) {}

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) {
        System.out.println("hello my first servlet");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {}
}
```

#### (2) 配置Servlet访问地址

- 编写`web.xml`文件，将该文件置于`WEB-INF`目录下。

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello_servlet</url-pattern>
    </servlet-mapping>

</web-app>
```

说明：

- `<servlet>`标签用于为Servlet的实现类起别名。
  - `<servlet-name>`: 指定其别名。
  - `<servlet-class>`: 引用该类的全限定名。
- `<servlet-mapping>`标签用于为Servlet的实现类映射访问URL 地址。
  - `<servlet-name>`: 引用该类的别名。
  - `<url-pattern>`: 指定访问URL地址，支持通配符。

#### (3) 添加外部依赖

- 将Servlet相关jar包(servlet-api.jar)置于`WEB-INF/lib/`目录下。

#### (4) 编译和部署

- 将编写好的类通过java指令生成`.class`文件。
- 将该文件置于`WEB-INF/classes`目录下。
- 注意：若该类存在包名，则.class文件也需置于相应包名的路径之下。

------

# 二、Servlet开发简化

> Servelet开发包括四个步骤：编写Servlet程序、配置Servlet访问地址、添加外部依赖、Servlet程序编译和部署。每一步都可以进行针对性简化，提高开发效率。

## 2.1 简化外部依赖添加

> 创建Maven项目，在pom.xml中添加相关依赖，则无需手动添加外部依赖并置于指定目录。

```xml
<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.3</version>
    </dependency>
</dependencies>
```

**注意：由于此处使用的是tomcat 9，所以有关servlet和jsp的依赖来自javax。**

## 2.2 简化程序编译和部署

> 添加Web框架支持、并指定打包方式为war，则无需手动编译程序并部署在指定目录。

说明：

- 添加框架支持后，idea会自动生成于`src`同级的`web`目录及相关文件。

![image-20220108224033891](../../../../Library/Application Support/typora-user-images/image-20220108224033891.png)

- 只有web目录出现蓝灯标记，才说明该项目被识别为web项目。
- 否则，右击`工程目录`->点击 `Open Module Settings`->选中`Facets`->选中`相关工程`->修改`Web Resource Directory`为指定web目录。

![image-20220108225059668](../../../../Library/Application Support/typora-user-images/image-20220108225059668.png)

## 2.3 简化Servlet程序编写

> 可通过继承`Servlet`的实现类，减少非核心方法的重写。

### 2.3.1 继承GernericServlet

```java
package com.shaobo.servlet;

import javax.servlet.GenericServlet;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class SecondServlet extends GenericServlet {
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) {
        System.out.println("hello my second servlet");
    }
}
```

说明：

- `GenericServlet`是`Servlet`的一个实现类，已经对其所有方法进行了实现。
- 因此，只需重写`service`方法即可。

### 2.3.2 继承HttpServlet

```java
package com.shaobo.servlet;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ThirdServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        System.out.println("hello my third servlet by GET");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) {
        System.out.println("hello my third servlet by POST");
    }
}
```

说明：

- `HttpServlet`是`Servlet`的一个实现类，已经对其所有方法进行了实现。
- 同时，`HttpServlet`根据访问的请求类型对`service`方法进行了精细划分。
- 因此，只需根据需求，重写相应请求类型的方法即可。例如，此处对`doGet`和`doPost`进行了重写。

## 2.4 简化访问映射

> 在Servlet的实现类上添加`@WebServlet`注解并指定映射路径，则无需在web.xml中配置相关映射。

```java
package com.shaobo.servlet;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(value = {"/basic_servlet", "/bs"})
public class BasicServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        System.out.println("hello my servlet by annotation");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) {
        doGet(req, resp);
    }
}
```

说明：

- 在`value`中配置访问路径。
- 配置的访问路径可以有多个，并且支持通配符。

## 2.5 添加服务器配置

> 选择`add configuration`，为工程添加服务器配置

![image-20220108235109561](../../../../Library/Application Support/typora-user-images/image-20220108235109561.png)

- 通过`configure`指定tomcat本地所在路径。
- 可自定义访问端口号。
- 可自定义配置名称。

> 选择`Deployment`标签，将工程部署到服务器启动项

![image-20220109001600758](../../../../Library/Application Support/typora-user-images/image-20220109001600758.png)

- 点击`+` -> `artifacts`，添加带有`war exploded`的web工程。
- 修改`Application context`。

------

# 三、请求与响应

## 3.1 HTTP协议

> HTTP(HyperText Transfer Protocol): 是一个基于请求与响应模式的、无状态的、应用层的协议，运行于TCP协议基础之上。

### 3.1.1 协议的特点

- 支持B/S架构模式。
- 简单快速: 客户端(浏览器)只向服务器发送请求方法和路径，服务器即可响应数据回传给客户端。因此通信方式简单，速度很快。
- 灵活: HTTP允许传输人意类型的数据，数据类型由`Contenti-Type`标识。

- 无连接: 每次TCP连接只处理客户的请求，服务器处理完后即断开连接。因此可以节省传输时间。
  - HTTP1.0的版本采用*短连接*模式，即一个请求和响应之后，服务器立刻断开连接。
  - HTTP1.1之后的版本采用*长连接*模式，即一个请求和响应之后，服务器会等待几秒。这段时间内若用户有新的请求，则还是会采用之前的连接通道收发消息；这段时间内没有新的请求再断开连接。
- 无状态: 对事务处理没有记忆能力，把每个请求作为与之前任何请求都无关的独立事务。

### 3.1.2 协议的通信流程

- 浏览器与服务器建立连接（三次握手）。
- 客户向服务器发送请求。
- 服务器接收请求，并根据请求进行处理，然后返回响应。
- 客户与服务器关闭连接（四次挥手）。

说明：Web程序步骤即：**解析请求内容**、**根据请求内容进行处理**、**根据处理结果生成响应内容**、**返回响应**。

## 3.2 请求 (Request)

> 浏览器向Web服务器发出请求时，它向服务器传递的数据块，就是`请求信息`，也称为`请求报文`。

### 3.2.1 请求报文的成分

- 请求行
  - 格式：`请求类型/请求地址 URL协议/协议版本`
  - 例子：`GET/http:www.baidu.com:80 HTTP/1.1`
- 请求头(Request Header)，用以指定请求的各项属性，如连接方式、支持的语言等。
  - 格式：以键值对的形式
- 空行
- 请求正文

<img src="https://s2.loli.net/2022/01/10/aw48npKxFyb5oEH.png" alt="image-20220110113758240" style="zoom:150%;" />

### 3.2.2 请求的类型

> 请求的类型包括：get、post、put、delete，其中最重要的是`get`和`post`。

#### (1) Get请求

- 提交的数据会放在URL之后，以`?`分割、参数之间以`&`相连，通过明文传递。
- 数据传输量小、效率高，但不安全。
- 也是浏览器默认的请求方式。

#### (2) Post请求

- 提交的数据放http包的`<body>`标签中，通过密文传递。
- 数据传输量大、效率比get低，但更安全。

## 3.3 响应 (Response)

> Web服务器向浏览器返回响应时，它向浏览器传递的数据块，就是`响应信息`，也称为`响应报文`。

### 3.3.1 响应报文的成分：

- 状态行
  - 格式：`URL协议/协议版本 状态码 状态描述`
  - 例子：`HTTP/1.1 200 OK`
- 响应头(Response Header)，用以指定响应的各项属性，如连接方式、支持的语言等。
  - 格式：以键值对的形式
- 空行
- 响应正文

![image-20220110140601613](https://s2.loli.net/2022/01/10/9P8Uxa5jVsAMDFZ.png)

## 3.4 案例

> 项目地址：https://gitee.com/shaobo1103/study/tree/master/JavaWeb/03-req-resp

### 3.4.1 设置首页

> 在index.jsp中编写欢迎页面

```jsp
<%@ page contentType="text/html;charset=UTF-8"%>
<html>
<head>
    <title>Request&Response Servlet</title>
</head>
<body>
<h1>通过Get提交</h1>
<form action="${pageContext.request.contextPath}/req" method="get">
    <label>
        用户名<input type="text" name="username"><br/>
        密码<input type="password" name="password"><br/>
        <input type="submit" name="登录">
    </label>
</form>
<hr/>
<h1>通过Post提交</h1>
<form action="${pageContext.request.contextPath}/req" method="post">
    <label>
        用户名<input type="text" name="username"><br/>
        密码<input type="password" name="password"><br/>
        <input type="submit" name="登录">
    </label>
</form>
</body>
</html>

```

### 3.4.2 创建接收请求的Servlet

> 在指定包下创建接收请求的Servlet类

```java
package com.shaobo.servlet;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(value = {"/req"})
public class ReqServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        req.setCharacterEncoding("utf-8");
        System.out.println(req.getParameter("username"));
        System.out.println(req.getParameter("password"));

        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        writer.println("登录成功！");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        req.setCharacterEncoding("utf-8");
        System.out.println(req.getParameter("username"));
        System.out.println(req.getParameter("password"));

        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        writer.println("登录成功！");
    }
}
```

说明：

- 通过`@WebServlet`为该类绑定访问地址。

- 根据请求类型，`get`和`post`请求会分别通过`doGet`和`doPost`方法接收。
- 请求提交的数据通过`req.getParameter`方法获取。
- 通过`resp`获取`PringWriter`对象，通过`println`方法编写响应内容。

注意：

- 通过`req.setCharacterEncoding`方法设置请求报文的编码格式。
- 通过`resp.setContentType`方法设置响应的内容内类型，包括其编码格式。
- `doGet`方法可通过`doPost`方法实现，降低代码冗余。

简化之后的代码：

```java
package com.shaobo.servlet;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(value = {"/req"})
public class ReqServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        req.setCharacterEncoding("utf-8");
        System.out.println(req.getParameter("username"));
        System.out.println(req.getParameter("password"));

        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        writer.println("登录成功！");
    }
}
```

------

# 四、转发与重定向

## 4.1 背景

> 为符合单一职能原则，在处理请求时应将`业务处理`和`结果显示`分成两个独立的模块单独实现。
>
> 因此，需要"业务处理"模块的结果可传输给"结果显示"模块。
>
> 这一功能可通过`转发`和`重定向`两种方式实现。

## 4.2 转发

> 作用在服务器端，服务器将请求发给其上的其他资源，并将其收到的响应结果传递给用户进行显示。

### 4.2.1 业务逻辑

![image-20220110185943398](https://s2.loli.net/2022/01/10/IBeq5SgNJO2laFV.png)

说明：

- Servlet1可以对接收的请求进行处理，发送新的请求给其他Servlet。因此"请求1"和"请求2"可以不相同。
- 因为已经做了业务分离，所以Servlet1不应对接收的响应做额外处理，而是直接转发给客户端。

### 4.2.2 代码实现

> 负责业务处理的Servlet

```java
package com.shaobo.servlet;

import com.shaobo.entity.User;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;

@WebServlet(value = {"/a"})
public class ServletA extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
        req.setAttribute("user1", new User("张三", 18, "男", Arrays.asList("唱跳", "rap", "篮球")));
        req.getRequestDispatcher("/b").forward(req, resp);
    }
}
```

- 通过`setAttribute`方法，以键值对的形式将参数保存到`Request`对象中。
- 通过`getRequestDispatcher`指定请求转发的URL。
- 通过`forward`方法，将`Request`和`Response`对象作为参数传递给接收对象。

> 负责页面显示的Servlet

```java
package com.shaobo.servlet;

import com.shaobo.entity.User;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(value = {"/b"})
public class ServletB extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        User user1 = (User) req.getAttribute("user1");
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        writer.println(user1);
    }
}
```

- 通过`getAttribute`方法，从接收的`Request`对象中获取数据。
- 通过此处的`Response`对象进行响应页面的展示。

### 4.2.3 数据传递

- forward: 表示一次请求，是在服务器内部跳转，可共享同一次`request作用域`中的数据。
- request作用域: 指拥有存储数据的空间，作用范围是一次请求有效（可经过多次转发）。
  - 可以将数据存入request后，在一次请求过程中的任何位置获取。
  - 可以传递任何数据（基本数据类型、数组、对象、集合等）。

### 4.2.4 转发的特点

- 转发是服务器行为。
- 对浏览器而言，只做了一次访问请求，且浏览器访问的地址不变。
- 两次转发跳转之间传输的信息不会丢失，所以可以通过request进行数据传递。
- 转发只能将请求转发给同一Web应用中的组件。

## 4.3 重定向

> 重定向作用在客户端，客户端将请求发送给服务器后，服务器响应给客户端一个新的请求地址，客户端向新地址重新发起请求。

### 4.3.1 业务逻辑

![image-20220111103536082](https://s2.loli.net/2022/01/11/9paDlPRTWKrYq3Z.png)

说明：

- Servlet1将重定向的地址通过响应1传给用户，用户再向新地址发起请求。
- 请求1和请求2的作用域不互通，所以两次请求是不同对象。

### 4.2.2 代码实现

> 负责业务处理的Servlet

```java
package com.shaobo.servlet;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(value = {"/c"})
public class ServletC extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.sendRedirect("/04/b");
    }
}

```

- 通过`sendRedirect`方法，指定重定向地址。
- 重定向的地址需要从根路径开始。

### 4.3.3 数据传递

- sendRedirect跳转时，地址栏改变，代表客户端重新发送的请求，属于两次请求。
- response没有作用域，所以客户端发起的两次请求无法共享数据。
  - 若servlet1希望向servlet2传递数据，需要通过URL的拼接实现。
  - 通过这种方式只能传递字符串对象，且必须以明文形式传递。

### 4.3.4 重定向的特点

- 重定向是客户端行为。
- 对浏览器而言，做了两次访问请求，浏览器访问的地址改变。
- 两次跳转之间传输的信息会丢失，所以无法通过request进行数据传递。
- 重定向可以指向任何资源，包括当前Web应用中的其他资源、或其他站点的资源。

## 4.4 总结

> **当两个Servlet需要传递数据时，通过转发实现，而不要用重定向。**

------

# 五、状态管理

## 5.1 现有问题

- HTTP协议是无状态的，不能保存每次提交的信息。
- 如果用户发来新的请求，服务器无法知道它是否与之前的请求有联系。
- 无法减少多次提交相同数据的操作（如: 登录）。

## 5.2 状态管理的概念

> 将浏览器与web服务器之间的多次交互当作整体处理，并将涉及的数据(状态)保存下来。

## 5.3 状态管理的分类

- 客户端状态管理技术：将状态保存在客户端。代表性的是Cookie技术。
- 服务器状态管理技术：将状态保存在服务器端。代表性的是Session技术。

------

# 六、Cookie

## 6.1 Cookie的概念

- Cookie是在浏览器访问服务器某个资源时，由服务器在`HTTP响应头`中附带传送给浏览器的一段数据。
- 这段数据被浏览器保存在本地，且在之后访问该服务器时，都会在`HTTP请求头`中携带上这段数据传给服务器。
- 一个Cookie主要由标识该信息的`name`和`value`组成。

## 6.2 创建和设置Cookie

```java
// 创建Cookie
Cookie = new Cookie("code", code);
// 设置Cookie路径
ck.setPath("/projectName/subDir");
// 设置Cookie有效时间
ck.setMaxAge(-1);
// 将Cookie添加到Response对象中，响应时发送给客户端
response.addCookie(ck);
```

说明：

- `setPath`设置被允许使用该Cookie的Servlet，默认为该工程下所有Servlet均可使用。
- `setMaxAge`设置Cookie的有效时间，默认值为-1。
  - 参数值大于0: 单位为秒。
  - 参数值等于0: 浏览器关闭前有效，关闭后失效。
  - 参数值小于0: 永久有效，会存储到本地内存中。
- 通过`chrome://settings/content/cookies`可查看浏览器cookie信息。   

## 6.3 获取Cookie

```java
// 通过request对象获取所有cookie
Cookie[] cookies = req.geCookies();
// 循环便利Cookie
if (cookies != null) {
    for(Cookie cookie: cookies) {
 	   System.out.println(cookie.getName+": "+cookie.getValue);
	}
}
```

## 6.4 修改Cookie

- cookie的名和路径都一致时，会将新的值替换掉原有值。
- 如果只有名一致但路径不一致， 则会创建新的cookie，原有cookie会被保留。

## 6.5 编码与解码

> Cookie默认不支持中文，所以传入中文参数时，需要对Unicode字符进行编码。

- 编码：使用`java.net.URLEncoder`类的`encode(String str, String encoding)`方法。
- 解码：使用`java.net.URLDecoder`类的`decode(String str, String encoding)`方法。

### 6.6.1 编码

```java
// 新建Cookie
Cookie cookie = new Cookie(URLEncoder.encode("姓名","utf-8"), URLEncoder.encode("张三","utf-8"));
// 发送到客户端
```

### 6.6.2 解码

```java
if (req.getCookies() != null) {
    for (Cookie cookie: req.getCookies()) {
        String name = URLDecoder.decode(cookie.getName(), "utf-8");
        String value = URLDecoder.decode(cookie.getValue(), "utf-8");
        System.out.println(name + ": " + value);
    }
}
```

## 6.6 Cookie的优缺点

> Cookie的优点

- 简单轻便，是一种基于文本的轻量(键值对)结构。
- 数据具有持久型，并且可以配置到期规则。

> Cookie的缺点

- 大小受到限制：大多数浏览器限制为4K或8K。
- 有被禁用风险：浏览器提供禁用Cookie功能，因此无法保证客户端可以保存Cookie。
- 潜在安全风险：Cookie可能会被篡改，会导致依赖于它的应用程序失败。

------

# 七、Session对象

## 7.1 Session概述

- Session用于记录用户的状态。
- Session指的是在一段时间内，单个客户端与服务器的一连串相关的交互过程。
- 在一个Session中，客户可能会多次访问同一资源，也可能访问不同服务器的资源。

## 7.2 Session原理

- 服务器会为每一次会话分配一个Session对象。
- 同一个浏览器发起的多次请求，同属于一个Session。
- 首次使用Session时，服务器会自动创建Session，并创建Cookie存储Session ID发送回客户端。
- Session是由服务器创建的

## 7.3 Session使用

> Session的作用域

- 一次会话是使用同一浏览器发送的多次请求。
  - 浏览器不关闭，则会话一直存在。
  - 浏览器关闭，则会话结束。
- 可以将数据存入Session中，在一次会话的任意位置进行获取。
- 可传递任意类型的数据(基础类型、对象、集合、数组)

### 7.3.1 获取Session

