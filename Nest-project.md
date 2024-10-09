# 新建项目
*Nest CLI 创建*
```bash
npm i -g @nestjs/cli
nest new project-name
```
创建 `project` 木、安装 node 模块和其它一些模板文件，同时还将创建 `src/` 目录，并填充几个核心文件。
src
app.controller.ts 带有单个路由的基本控制器。
app.controller.spec.ts 针对控制器的单元测试。
app.module.ts
app.service.ts
main.ts

|`app.module.ts`|T应用程序的根模块（root module）。|
|`app.service.ts`|具有单一方法的基本服务（service）。 method.|
|`main.ts`|应用程序的入口文件，它使用核心函数 `NestFactory` 来创建 Nest 应用程序的实例。|

`main.ts` 文件中包含了一个异步函数，此函数将 **引导（bootstrap）** 应用程序的启动过程：
