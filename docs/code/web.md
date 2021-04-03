# 如何进行 Web 开发 <!-- {docsify-ignore} -->
总结一点对 Web 开发的理解，这样对项目整体有更深的认识。     
Web 开发通常是指开发服务器端的 Web 应用程序。浏览器访问某个网站时，会发送 url 请求到服务器器，服务器再将响应数据返回并解析为 HTML 给浏览器。浏览器和服务器之间的传输协议是 HTTP。

## Web 开发过程
### 请求处理
使用封装好的读取 HTTP 协议的 Web 服务器，进行 get\post 等请求。浏览器发出的请求由服务器端进行路径映射到对应的路径处理，或者服务器处理完返回一个重定向指令，告诉浏览器使用新的 URL 再发送新的请求。
- 获取请求参数      
通过封装好的框架注解获取参数、request 请求传递 json 参数、格式化 Date\Number 等传递的参数、自定义参数转换（源类型转换为目标类型）、自动转换为集合类型
- 数据验证      
获取参数后需要验证参数的合法性。在 JavaBean 里通过注解的方式对其属性进行规范。如不能为空，需要一个将来的日期、最小值、最大值为固定值、限定范围、邮箱格式错误、字符串长度范围、自定义参数验证。最后在控制器的参数(调用控制器前先执行)添加校验机制进行验证

- 数据模型      
数据模型的作用是绑定数据，为后面的视图渲染做准备。控制器自定义模型和视图 ModelAndView，模型则是存放数据的地方，视图则是展示给用户。

### Session 和 Cookie 记录
因为 HTTP 协议是一个无状态协议。无法区分收到的两个 HTTP 请求是否是同一个浏览器发出的。      
所以为了追踪用户状态，服务器向浏览器分配一个唯一 ID，并以 Cookie 的形式发送到浏览器，浏览器在后续访问时总是附带此 Cookie ，这样就可以识别用户身份。       
Session 是记录在服务器端的，由于服务器把所有用户的Session都存储在内存中，如果遇到内存不足的情况，就需要把部分不活动的 Session 序列化到磁盘上，这会大大降低服务器的运行效率，对于大型服务使用 Session 会存在性能问题，需要将 Session 数据单独存储在数据库中。Session 需要持久化存储。
Cookie 存储在浏览器，存在生效的路径范围，有效期。

## MVC 设计模式
Model-View-Controller，控制器（Controller）→ 模型（Model）→ 视图（View）。      
Controller 专注于业务处理，它的处理结果就是 Model。Model 可以是一个 JavaBean，也可以是一个包含多个对象的 Map，Controller 只负责把 Model 传递给 View，View 只负责把Model 给“渲染”出来，这样，三者职责明确，且开发更简单，因为开发Controller 时无需关注页面，开发 View 时无需关心如何创建 Model。

## 过滤器 Filter
为了把一些公用逻辑从各个控制器中抽离出来，使用过滤器，它的作用是，在 HTTP 请求到达 Servlet 之前，可以被一个或多个 Filter 预处理，类似打印日志、登录检查等逻辑，完全可以放到 Filter 中。用在请求的数据传输情况验证如：文件上传，和请求响应比较久的地方如：缓存处理。

## 监听器 Listener      
通过 Listener 我们可以监听 Web 应用程序的生命周期，获取 `HttpSession` 等创建和销毁的事件；
`ServletContext` 是一个 WebApp 运行期的全局唯一实例，可用于设置和共享配置信息。

## 拦截器：Interceptor
拦截 Controller，基于 AOP 的方法拦截。通常在认证或者安全检查失败时直接返回错误响应，处理异常。

## 跨域
浏览器同源：domain 域名、协议、端口相同。指定 Cookie 的所属域名为一级域名       
- 什么是跨域：允许其他域名的网站可以访问获取某个网站的 cookie。    

解决跨源问题的方法：JSONP、WebSocket、CORS    

- CORS：跨源资源分享（Cross-Origin Resource Sharing）的缩写。它是 W3C 标准，是跨源 AJAX 请求的根本解决方法。相比 JSONP 只能发 GET 请求，CORS 允许任何类型的请求。   

字段 Origin，表示该请求的请求源（origin），即发自哪个域名。                

CORS 它允许浏览器向跨源服务器，发出 XMLHttpRequest 请求，从而克服了 AJAX 只能同源使用的限制。实现 CORS 通信的关键是服务器。只要服务器实现了 CORS 接口，就可以跨源通信。     

web 开发中允许指定的网站通过页面 JavaScript 访问这些 REST API，就必须正确地设置 CORS。使用 @CrossOrigin，CorsRegistry（在WebMvcConfigurer 中定义一个全局 CORS 配置）注解。


## 异步处理
回调的方式处理线程，或者增加线程：在方法内部又 new 了一个线程或者扔到线程池里，由这个新的线程去处理耗时的部分，方法本身不去等待线程执行完毕。

## WebSocket
服务器主动给浏览器推送数据，将请求变成长连接的 WebSocket。


## Mybatis 持久层声明式事务

事务的隔离性：多个应用程序的线程同时访问同一个数据，数据库同样的数据会被各自不同的事务中被访问。如：多个信用卡同时进行还款

高并发的情况，抢购某项商品，多个事务同时访问商品库存的场景
## REST
输入输出都是JSON，便于第三方调用或者使用页面 JavaScript 与之交互。

## Spring 框架
如果一个系统有大量的组件，其生命周期和相互之间的依赖关系如果由组件自身来维护，不但大大增加了系统的复杂度，而且会导致组件之间极为紧密的耦合，继而给测试和维护带来了极大的困难。很多组件需要销毁以便释放资源，例如访问数据库资源，但如果该组件被多个组件共享，如何确保它的使用方都已经全部被销毁？        
在 IoC 模式下，控制权发生了反转，即从应用程序转移到了IoC容器，所有组件不再由应用程序自己创建和配置，而是由IoC容器负责。这样，应用程序只需要直接使用已经创建好并且配置好的组件。为了能让组件在IoC容器中被“装配”出来，需要某种“注入”机制，组件不用通过 new 创建，而是注入其他组件。