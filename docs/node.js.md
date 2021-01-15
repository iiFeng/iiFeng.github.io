## Node.js
[TOC]
### Node.js 是什么？
- 因为 javaScript 只能在浏览器中执行，而 Node.js 提供了 js 在后端运行的环境。将 js 模块化，函数式编程。
### Node.js 的特性
- 单线程，process 进程。只能等待下一次事件响应中执行代码。所以可以使用异步编程。
- Node.js 开发的目的就是为了用 JavaScript 编写 Web 服务器程序。
### npm 有什么关系
- node 提供了很多模块包，使用 npm 包管理器进行管理。
### Node.js 模块
- 一个.js文件就称之为一个模块（module）。
- fs 文件系统模块
    - 读写文件。异步读写文件，如：文本文件、二进制文件（图片），用 Buffer 表示。
- stream 流模块
    - “流”这种数据结构的特点是数据是有序的，而且必须依次读取。如：从文件流读取文本，从文件读取数据时，可以打开一个文件流，然后从文件流中不断地读取数据。
- http 模块
    - 应用程序并不直接和HTTP协议打交道，而是操作http模块提供的request和response对象。
    - https 安全问题，建议用反向代理服务器如Nginx等Web服务器去处理证书。
### Node.js 工程目录
- 执行文件：执行 node xx.js。工程文件：package.json <-- 项目描述文件，node_modules/ <-- npm安装的所有依赖包
### Node.js 的 web 框架
- koa 框架
    - 引入了新的关键字async和await，可以轻松地把一个function变为异步模式。
    - web 开发过程中需统一管理处理 url 。引入 middleware（中间件）解析原始request请求。post 请求会发送表单或 json，它作为 request 的 body 请求实体发送。（get 和 post 请求的区别）
    - Nunjucks 模版引擎。输出HTML时需要考虑：特殊字符转义、不同类型的变量格式化模板、继承。
    - mvc。MVC：Model-View-Controller，中文名“模型-视图-控制器”。
- mysql
    - 通过 mysql 驱动程序访问数据库。驱动程序通过网络发送SQL命令，然后，MySQL服务器执行后返回结果。
    - ORM 技术：Object-Relational Mapping，把关系数据库的表结构映射到对象上。配置文件，设计好的结构，才能提升工程能力。
- mocha 单元测试
    - 测试异步代码，测试异步函数需要在函数内部手动调用done()表示测试成功，done(err)表示测试出错。也可以直接把 async 函数当成同步函数来测试。
- WebSocket 协议
    - 目的是在浏览器和服务器之间建立一个不受限的双向通信的通道，让浏览器和服务器之间可以建立无限制的全双工通信。WebSocket并不是全新的协议，而是利用了HTTP协议来建立连接。如，服务器可以在任意时刻发送消息给浏览器。
    - HTTP 协议是一个请求－响应协议，请求必须先由浏览器发给服务器，服务器才能响应这个请求，再把数据发送给浏览器。为什么 HTTP 协议无法进行全双工通信。因为TCP协议本身就实现了全双工通信，但是HTTP协议的请求－应答机制限制了全双工通信。
    - WebSocket 发起请求时，请求头 Upgrade: WebSocket 和 Connection: Upgrade 表示这个连接将要被转换为WebSocket 连接；还需要对 Node.js 提供的 HTTPServer 做额外的开发，以支持 WebSocket 协议。
    - 配置反向代理：HTTP 和 WebSocket 都必须通过反向代理 Nginx 连接Node服务器。
- REST
    - 一个 URL 返回的不是 HTML，而是机器能直接解析的数据，这个 URL 就可以看成是一个Web API。
    - REST 请求只是一种请求类型和响应类型均为 JSON 的 HTTP 请求。REST 请求仍然是标准的HTTP请求，但是，除了 GET 请求外，POST、PUT 等请求的 body 是 JSON 数据格式，请求的 Content-Type 为 application/json；返回的结果也是。
    - 处理错误。HTTP 请求时发生错误，客户端除了提示用户“出现了网络错误，稍后重试”以外，并无法获得具体的错误信息。另一类错误是业务逻辑的错误，这种类型的错误完全可以通过JSON返回给客户端。
- MVVM
    - 使用 MVVM 框架来实现 JavaScript 在前端修改服务器渲染后的数据。
    - MVVM 的设计思想：关注 Model 的变化，让 MVVM 框架去自动更新 DOM 的状态。js  修改 HTML 的 DOM 结构和 CSS 来实现一些动画效果。使用 MVVM 框架可直接修改 JavaScript 对象，来更新 DOM。
    - 常用的MVVM框架有：Angular.js、Vue.js。
    - 使用 Vue.js：在该节点以及该节点内部，就是 Vue 可以操作的 View，初始化 Model 的两个属性 name 和 age。Vue 会自动监听 Model 的任何变化。单向绑定：把 Model 绑定到 View，Model 更新，view 就会自动更新。双向绑定：如果用户更新了View，Model的数据也自动被更新了，在 html 属性上绑定双向绑定。
    - 处理事件：当用户提交表单时，响应 onsubmit 事件也可以放到 VM 中。
    - 让 Model 和 DOM 的结构保持同步。当我们更新 Model 时，DOM 结构会随着 Model 的变化而自动更新。在 MVVM 内部更新 Model 的同时，通过 API 把数据更新反映到服务器端，这样，用户数据就保存到了服务器端。
