HTTP协议（HyperText Transfer Protocol，超文本传输协议）

工作于CS架构的请求/响应协议

HTTP默认端口号为80



特性：

* 无状态：不保存之前的状态来加快响应，否则会导致请求数据越来越大。
* 无连接：处理完请求后旧断开，方便处理其他用户的请求
* 媒体独立：只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送

可以Keep-Alive保持连接(超过Keep-Alive规定的时间，意外断电等情况除外)



HTTP消息的组成：

​	请求：请求头，请求行，请求空行，请求体

​	响应：状态行，响应头，响应空行，响应体



***请求头***

| 头消息              | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| Accept              | 浏览器可以处理的MIME 类型，常见的text/json等                 |
| Accept-Charset      | 浏览器可以用来显示信息的字符集                               |
| Accept-Encoding     | 浏览器知道如何处理的编码类型                                 |
| Accept-Language     | 客户端使用的语言                                             |
| Authorization       | 授权信息                                                     |
| Connection          | 是无连接状态还是Keep-Alive                                   |
| Content-Length      | 给出 POST 数据的大小                                         |
| Cookie              | 把之前发送到浏览器的 cookies 返回到服务器。                  |
| Host                | 原始的 URL 中的主机和端口                                    |
| If-Modified-Since   | 当页面在指定的日期后已更改时，客户端想要的页面，没有时返回304 |
| If-Unmodified-Since | 指定只有当文档早于指定日期时，操作才会成功                   |
| Referer             | 从哪个url来的                                                |
| User-Agent          | 发出请求的浏览器版本号，操作信息等                           |

通过***HttpServletRequest***对象获得请求信息

| 头消息                                | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| Cookie[] getCookies()                 |                                                              |
| Enumeration getAttributeNames()       | 返回一个枚举，包含提供给该请求可用的属性名称。               |
| Enumeration getHeaderNames()          | 返回一个枚举，包含在该请求中包含的所有的头名。               |
| Enumeration getParameterNames()       | 返回一个 String 对象的枚举，包含在该请求中包含的参数的名称   |
| HttpSession getSession(boolean create | 返回与该请求关联的当前 session 会话，没有是否创建            |
| Locale getLocale()                    | 基于 Accept-Language 头，返回客户端接受内容的首选的区域      |
| Object getAttribute(String name)      | 不存在的属性设置为null                                       |
| ServletInputStream getInputStream()   | 以二进制数据形式检索请求的主体                               |
| String getAuthType()                  | 获得身份验证方案的名称，不受保护返回null                     |
| String getCharacterEncoding()         |                                                              |
| String getContentType()               |                                                              |
| String getContextPath()               | 返回URI                                                      |
| String getHeader(String name)         |                                                              |
| String getMethod()                    |                                                              |
| String getParameter(String name)      |                                                              |
| String getPathInfo()                  | 返回与客户端发送的 URL 相关的任何额外的路径信息              |
| String getProtocol()                  | 返回请求协议的名称和版本。                                   |
| int getContentLength()                | 以字节为单位返回请求主体的长度，并提供输入流，或者如果长度未知则返回 -1。 |
| boolean isSecure()                    | 请求是否使用安全通道，如 HTTPS                               |
|                                       |                                                              |

状态行：HTTP 版本+状态码+状态码短消息

响应头：

状态码



# 过滤器



***作用***：

- 在客户端的请求访问后端资源之前，拦截这些请求。
- 在服务器的响应发送回客户端之前，处理这些响应。



***使用场景***：

- 身份验证过滤器（Authentication Filters）。
- 数据压缩过滤器（Data compression Filters）。
- 加密过滤器（Encryption Filters）。
- 触发资源访问事件过滤器。
- 图像转换过滤器（Image Conversion Filters）。
- 日志记录和审核过滤器（Logging and Auditing Filters）。
- MIME-TYPE 链过滤器（MIME-TYPE Chain Filters）。
- 标记化过滤器（Tokenizing Filters）。
- XSL/T 过滤器（XSL/T Filters），转换 XML 内容。

与Service一样，将一个拦截器和它的映射路径声明到web中





HttpEntity

* 请求头
* 请求体



HttpHeaders，实现MultiValueMap<K, V>(Map<K, List\<V>>)

***ContentType***

[参考](https://blog.csdn.net/qq_37651267/article/details/91380351)

没有MIME头，就默认它为HTML格式

text类型

image类型

application类型

application/xml会根据xml头指定的编码格式来编码































## 分块传输协议



`org.apache.http.MalformedChunkCodingException: Bad chunk header`





### 内容编码与传输编码

传输编码：

分块传输









分块传输编码允许服务器在最后发送消息头字段



早起通过Content-Length判断传输是否完成

现在通过Transfer-Encoding判断

只有一种情况Transfer-Encoding:chunked，表示分块传输





错误码

| 错误码 | 描述         |
| ------ | ------------ |
| 400    | 读取参数失败 |
|        |              |
|        |              |









