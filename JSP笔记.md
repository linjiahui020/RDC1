## JSP笔记
JSP：在HTML嵌套JAVA代码  
#### 常见状态码：  
    404：资源不存在  
    403：权限不足  
    500：服务器内部错误（代码错误）
#### JSP的页面元素：  

```    
    ①脚本：  
    
    <%
        局部变量、JAVA代码
    %>
    
    <%!
        全局变量、定义方法
    %>
    
    <%=
        输出表达式
    %>

 回车：<br / >  
    ②指令  
    language：jsp页面使用的脚本语言  
    import：导入类  
    pageEncoding：jsp文件自身编码  
    contentType：浏览器解析jsp的编码 
    
    ③注释  
    `<%--  
        --%>`  
 
```  

#### JSP九大内置对象（自带的，不需要new也可以使用的对象）

* <font size = 4>**1.out：输出对象，向客户端输出内容**</font>  
* <font size = 4>**2.request：请求对象，存储客户端向服务端发送的请求信息，只在同一次请求中有效**</font>
  > `String getParameter(String name)`:根据请求的字段名key，返回字段值value  
    `String[] getParameter(String name)`:根据请求的字段名key，返回字段值value  
    `void setCharacterEncoding("编码格式utf-8")`：设置请求编码  
    `request.getRequestDispatcher().forward(request,response)`:页面跳转，请求转发，可以获取到数据，并且地址栏没有发生改变（仍然保留转发时的页面）  
    `request.getCookies()`：客户端获取Cookie，不能直接获取某一个单独对象，只能一次性将所有Cookie拿到
* <font size = 4> **3.response：响应对象**</font>
  > `void addCookie(Cookie cookie)`:服务端向客户端增加cookie对象  
    `void sendRedirect(String location)throws IOException`:页面跳转的一种方式，使用重定向，会导致数据丢失  
    `void setContentType(String type)`：设置服务端响应的编码(设置服务端的contentType类型)  
    
     
* <font size = 4> **4.pageContext(JSP页面容器)** </font>    
* <font size = 4> **5.session**(<font color="red">存储在服务端，较安全，保存的内容的类型为String</font>)：session对象用来跟踪在各个客户端请求间的会话</font>  
  > session机制: 客户端第一次请求服务端时，服务端会产生一个session对象（用于保存该客户的信息），并且每个session对象都会有一个唯一的sessionId（用于区分其他session），服务端会产生一个cookie，cookie的name=JSEESSIONID，value=服务端sessionId的值，之后服务端会在响应客户端的同时将该cookie发送给客户端，至此客户端就拥有了一个cookie（JSESSIONID），并且和服务端的session一一对应  
    客户端之后再次请求服务端时，服务端会先用客户端的cookie中的JSESSIONID去服务端匹配sessionId，如果匹配成功，则不是第一次访问，无需再次登录

><font size = 5>**session方法：**</font>  
`String getId()`：获取sessionId  
`boolean isNes()`：判断是否为新用户（第一次访问）  
`void invalidate()`：使session失效（退出登录或者注销）  
`void setAttribute()`  
`Object getAttribute()`  
`void setMaxInactiveInterval(秒)`：设置最大有效非活动时间
<br>

Cookie（<font color="red">存储在客户端，不是内置对象需要new，较不安全，保存的内容的类型为Object</font>）  
Cookie是由服务端生成的，在发送给客户端保存，相当于本地缓存的作用：客户端->服务端
提高访问服务端的效率，但是安全性较差 
当客户端第一次请求服务端时，如果服务端发现此请求没有JSESSIONID，则会创建一个name
=JSESSIONID的cookie，并返回给客户端
使用setMaxAge方法可以实现cookie的保存时间  

* <font size = 4> **6.application(全局对象)**</font>
> String getContextPath()：虚拟路径
  String getRealPath()：绝对路径（相当于虚拟路径而言）
* <font size = 4> **7.config(配置对象，服务器配置信息)**</font>  
* <font size = 4> **8.page(当前JSP页面对象，相当于java中的this)**</font>  
* <font size = 4> **9.exception(异常对象)**</font>  

#### get和post请求方式的区别
get方式在地址栏显示请求信息（但是显示的信息有限，若存在大文件如图片，则会出现错误），post不会显示  
文件上传操作必须用post  
推荐使用post方式
#### 四种范围对象（pageContext、request、session、application）
共有的方法：  
Object getAttribute(String name)：根据属性名或者属性值获得   
void setAttribute(String name,Object obj)：设置属性值  
区别：  
* pageContext：当前页面有效，页面跳转后无效  
* request：同一次请求有效，请求转发后有效，重定向后无效  
* session：同一次会话有效，关闭或切换浏览器时无效  
* application：全局变量，整个项目运行期间都有效，切换浏览器仍然有效，关闭服务、其他项目无效


## MVC设计模式  
M：Model模型：一系列功能   
V：View视图：用于展示、以及与用户交互。使用HTML、js、css、jsp、jquery等前端技术实现  
C：Controller控制器：接受请求，将请求跳转到模型进行处理；模型处理完毕后，再将处理的结果返回给请求处。一般使用servlet实现控制器   

### servlet  
JAVA类必须符继承HttpServlet类，重写其中的doGet或doPost方法  
doGet()：接受并处理所有get提交方式的请求  
doPost()：接受并处理所有post提交方式的请求  
servlet的生命周期：加载->初始化->服务->销毁->卸载  

Servlet API：可以适用于任何通信协议
ServletConfig：接口  
    getServletContext()：获取Servlet上下文对象  
    getInitParameter(String name)：在当前Servlet范围内，获取名为name的参数值
## 正则表达式
* 用\字符来匹配字符本身
* \d：匹配数字0-9中的任意一个==如果是大写的D则匹配相反，即匹配非数字==
* \w：匹配字母、数字、下划线
* []：自定义字符集合，里面放自己自定义的匹配字符
* ^：取反，匹配除了该字符以外的其他字符
* {n}：重复n次
* {m,n}：至少重复m次，最多重复n次
* ?：代表0-1次，相当于{0,1}
* +：至少1次，相当于{1,}
* *：至少0次，相当于{0,}
* ^：匹配的字符是开头
* $：匹配的字符是结尾
* \b：匹配一个单词边界，前面的字符和后面的字符不全是\w
* |：或的关系
* (?=exp)：断言自身出现的位置的后面能匹配表达式exp
* (?<=exp)：断言自身出现的位置的前面能匹配表达式exp
 
### 三层架构

###### 表示层：<br/>前台：对应于MVC中的View，用于和用户交互、页面的显示<br/>后台：对应于MVC中的Controller，用于控制跳转、调用业务逻辑层

######  业务逻辑层：接收表示层的请求和调用，组装数据访问层，逻辑性的操作  
######  数据访问层：直接访问数据库的操作，原子性的操作（增删查改）

分页查询：从0开始计数  
第0页： select * from studentinformation limit 0,10;  
第1页： select * from studentinformation limit 10,10;  
第n页： select * from studentinformation limit 页数*页面大小,10;  
分页查询的实现：  
    1.数据总数：查数据库   
    2.页面大小  
    3.总页数： 程序自动计算  总页数=数据总数%页面大小==0？数据总数/页面大小：数据总数/页面大小+1  
    4.当前页：  
    5.当前页的对象集合：每页所显示的所有数据，List<StudentInformation>