# 什么是Nodejs？
[Node](https://nodejs.org/zh-cn/)（正式名称 Node.js）是一个开源的、跨平台的运行时环境，开发人员可以使用 [JavaScript](https://developer.mozilla.org/zh-CN/docs/Glossary/JavaScript) 创建各种服务器端工具和应用程序。此运行时主要用于浏览器上下文之外（即可以直接运行于计算机或服务器操作系统上）。据此，该环境省略了一些浏览器专用的 JavaScript API，同时添加了对更传统的 OS API（比如 HTTP 库和文件系统库）的支持。

Node.js 是一个基于 Google JS 引擎 V8 的 JS 运行时环境。在这个定义里面有两个关键词，第一个就是 JS 引擎，第二个是 JS 的运行时。那什么叫 JS 引擎呢？

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e51a29c264744dd49292716c48bb6dec~tplv-k3u1fbpfcp-zoom-1.image)

JS 引擎就是把我们写的一些 JS 的代码进行解析执行，最后得到一个结果。比如我们看上图的左边用 JS 的语法定义一个变量 a 等于 1，b 等于 1，然后把 a 加 b 的值赋值给新的变量 c，接着把这个字符串传入 JS 引擎里，JS 引擎就会进行解析执行，执行完之后就可以得到对应的结果。

  


那么 JS 运行时又是什么呢 ？它和 JS 本身有什么区别 ？JS 是一门语言，有独立的语法规范，提供一些内置对象和 API（如数组、对象、函数等）。但和其他语言（C、C++等）不一样的是，JS 不提供网络、文件、进程等功能，这些额外的功能是由运行时环境实现和提供的，比如浏览器或 Node.js。所以，**JS 运行时可以理解为 JS 本身** **（下图左侧）** **+ 一些拓展的能力** **（下图右侧）** **所组成的一个运行环境**，具体我们可以看看下面这张图：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/082a5415b36e40c0a05c017158a8285d~tplv-k3u1fbpfcp-zoom-1.image)

可以看到，这些运行时都不同程度地拓展了 JS 本身的功能。JS 运行时封装底层复杂的逻辑，对上层暴露 JS API，开发者只需要了解 JS 的语法，就可以使用这些 JS 运行时做很多 JS 本身无法做到的事情。

### [Hello Node.js](https://developer.mozilla.org/zh-CN/docs/Learn/Server-side/Express_Nodejs/Introduction#hello_node.js)

以下示例将创建一个 web 服务器，它将监听对 URL `http://127.0.0.1:8000/` 所有种类的 HTTP 请求，当接收到一个请求时，脚本将做出响应：返回一个字符串“Hello World”。如果已经安装了 Node，可以按照下面的步骤尝试一下：

1. 打开终端（Windows 中打开命令行工具）
2. 创建一个空文件夹用来存放项目，比如 `"test-node"`，然后在终端输入以下命令进入这个文件夹：
    
    
    ```
    cd test-node
    ```
    
3. 用你最喜欢的文本编辑器创建一个名为 `"hello.js"` 的文件，把以下代码粘贴进来。
    
    
    ```js
    // 调用 HTTP 模块
    const http = require("http");
    
    // 创建 HTTP 服务器并监听 8000 端口的所有请求
    http
      .createServer((request, response) => {
        // 用 HTTP 状态码和内容类型来设定 HTTP 响应头
        response.writeHead(200, { "Content-Type": "text/plain" });
    
        // 发送响应体 "Hello World"
        response.end("Hello World\n");
      })
      .listen(8000);
    
    // 在控制台打印访问服务器的 URL
    console.log("服务器运行于 http://127.0.0.1:8000/");
    ```
    
4. 将其保存在刚才创建的文件夹。
5. 返回终端并输入以下命令：
    
    ```
    node "hello.js"
    ```
    

最后，在浏览器地址栏中输入 `"http://localhost:8000"` 并按回车，可以看到一个大面积空白的网页，左上角有“Hello World" 字样。

## [Web 框架](https://developer.mozilla.org/zh-CN/docs/Learn/Server-side/Express_Nodejs/Introduction#web_%E6%A1%86%E6%9E%B6)

Node 本身并不支持其他常见的 web 开发任务。如果需要进行一些具体的处理，比如运行其他 HTTP 动词（比如 `GET`、`POST`、`DELETE` 等）、分别处理不同 URL 路径的请求（“路由”）、托管静态文件，或用模板来动态创建响应，那么可能就要自己编写代码了，亦或使用 web 框架，以避免重新发明轮子。

## [什么是 Express?](https://developer.mozilla.org/zh-CN/docs/Learn/Server-side/Express_Nodejs/Introduction#%E4%BB%80%E4%B9%88%E6%98%AF_express)
[Express](https://www.expressjs.com.cn/) 是最流行的 Node 框架，是许多其他流行 [Node 框架](https://www.expressjs.com.cn/resources/frameworks.html) 的底层库。它提供了以下机制：

- 为不同 URL 路径中使用不同 HTTP 动词的请求（路由）编写处理程序。
- 集成了“视图”渲染引擎，以便通过将数据插入模板来生成响应。
- 设置常见 web 应用设置，比如用于连接的端口，以及渲染响应模板的位置。
- 在请求处理管道的任何位置添加额外的请求处理“中间件”。

虽然 Express 本身是极简风格的，但是开发人员通过创建各类兼容的中间件包解决了几乎所有的 web 开发问题。这些库可以实现 cookie、会话、用户登录、URL 参数、`POST` 数据、安全头等功能。可在 [Express 中间件](https://www.expressjs.com.cn/resources/middleware.html) 网页中找到由 Express 团队维护的中间件软件包列表（还有一张流行的第三方软件包列表）。

# 什么是Nest？
Nest (NestJS) 是一个用于构建高效、可扩展的 [Node.js](https://nodejs.org/) 服务器端应用程序的开发框架。它利用 JavaScript 的渐进增强的能力，使用并完全支持 [TypeScript](http://www.typescriptlang.org/) （仍然允许开发者使用纯 JavaScript 进行开发），并结合了 OOP （面向对象编程）、FP （函数式编程）和 FRP （函数响应式编程）。

在底层，Nest 构建在强大的 HTTP 服务器框架上，例如 [Express](https://expressjs.com/) （默认），并且还可以通过配置从而使用 [Fastify](https://github.com/fastify/fastify) ！

Nest 在这些常见的 Node.js 框架 (Express/Fastify) 之上提高了一个抽象级别，但仍然向开发者直接暴露了底层框架的 API。这使得开发者可以自由地使用适用于底层平台的无数的第三方模块。
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38a8aa18ae1a40e1ab83767b2558d84f~tplv-k3u1fbpfcp-watermark.image?)
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20b41feed8d54e8bb264e508cd55c9c3~tplv-k3u1fbpfcp-watermark.image?)

*Express跟着mdn文档学，nest跟着官网学*
