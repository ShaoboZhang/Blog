## 5.2 创建和设置Cookie

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

## 5.3 获取Cookie

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

## 5.4 修改Cookie

- cookie的名和路径都一致时，会将新的值替换掉原有值。
- 如果只有名一致但路径不一致， 则会创建新的cookie，原有cookie会被保留。

## 5.5 编码与解码

> Cookie默认不支持中文，所以传入中文参数时，需要对Unicode字符进行编码。

- 编码：使用`java.net.URLEncoder`类的`encode(String str, String encoding)`方法。
- 解码：使用`java.net.URLDecoder`类的`decode(String str, String encoding)`方法。

### 5.5.1 编码

```java
// 新建Cookie
Cookie cookie = new Cookie(URLEncoder.encode("姓名","utf-8"), URLEncoder.encode("张三","utf-8"));
// 发送到客户端
```

### 5.5.2 解码

```java
if (req.getCookies() != null) {
    for (Cookie cookie: req.getCookies()) {
        String name = URLDecoder.decode(cookie.getName(), "utf-8");
        String value = URLDecoder.decode(cookie.getValue(), "utf-8");
        System.out.println(name + ": " + value);
    }
}
```

## 5.6 Cookie的优缺点

> Cookie的优点

- 简单轻便，是一种基于文本的轻量(键值对)结构。
- 数据具有持久型，并且可以配置到期规则。

> Cookie的缺点

- 大小受到限制：大多数浏览器限制为4K或8K。
- 有被禁用风险：浏览器提供禁用Cookie功能，因此无法保证客户端可以保存Cookie。
- 潜在安全风险：Cookie可能会被篡改，会导致依赖于它的应用程序失败。

------

# 六、Session对象

## 6.1 Session概述

- Session用于记录用户的状态。
- Session指的是在一段时间内，单个客户端与服务器的一连串相关的交互过程。
- 在一个Session中，客户可能会多次访问同一资源，也可能访问不同服务器的资源。

## 6.2 Session原理

- 服务器会为每一次会话分配一个Session对象。
- 同一个浏览器发起的多次请求，同属于一个Session。
- 首次使用Session时，服务器会自动创建Session，并创建Cookie存储Session ID发送回客户端。
- Session是由服务器创建的

## 6.3 Session使用

> Session的作用域

- 一次会话是使用同一浏览器发送的多次请求。
  - 浏览器不关闭，则会话一直存在。
  - 浏览器关闭，则会话结束。
- 可以将数据存入Session中，在一次会话的任意位置进行获取。
- 可传递任意类型的数据(基础类型、对象、集合、数组)

### 6.3.1 获取Session

