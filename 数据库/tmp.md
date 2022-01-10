### 三、请求与响应

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

> 服务器将请求发给服务器上的其他资源并获取其响应，将响应的结果传递给用户进行显示。

### 4.2.1 业务逻辑

![image-20220110185943398](https://s2.loli.net/2022/01/10/IBeq5SgNJO2laFV.png)

说明：

- Servlet1可以对接收的请求进行处理，发送新的请求给其他Servlet。因此"请求1"和"请求2"可以不相同。
- 因为已经做了业务分离，所以Servlet1不应对接收的响应做额外处理，而是直接转发给客户端。

### 4.2.2 代码实现



### 4.2.3 数据传递



