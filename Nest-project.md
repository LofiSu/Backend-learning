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

## 第一个接口
前面我们已经启动了服务， 那我们怎么查看呢， 首先就是找到入口文件`main.ts`

```ts
import { NestFactory } from '@nestjs/core'; 
import { AppModule } from './app.module';
 async function bootstrap() { 
   const app = await NestFactory.create(AppModule);
   await app.listen(3000); 
} 
bootstrap();
```

内容比较简单， 使用`Nest.js`的工厂函数`NestFactory`来创建了一个`AppModule`实例，启动了 HTTP 侦听器，以侦听`main.ts` 中所定义的端口。

> 监听的端口号可以自定义， 如果3000端口被其他项目使用，可以更改为其他的端口号 因为我的3000端口有别的项目在用， 所以修改成:9080,重新启动项目

我们打开浏览器访问`http://localhost:9080`地址：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7c4f276af3a24609815635d2792b70e5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

这里看到的`Hello World`就是接口地址`http://localhost:9080`返回的内容， 不信我们也可以使用常用 Postman看看：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dd9d93bea0a44890883a65175c4557ad~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

说明`Nest.js`创建项目默认就给写了一个接口例子，那就通过这个接口例子来看，我们应该怎么实现一个接口。

前边看到`mian.ts`中也没有别的文件引入， 只有`AppModule`, 打开`src/app.module.ts`:

```ts
import { Module } from '@nestjs/common'; 
import { AppController } from './app.controller'; 
import { AppService } from './app.service';
@Module({
 imports: [], 
 controllers: [AppController], 
 providers: [AppService],
}) 
export class AppModule {}
```

`AppModule`是应用程序的根模块，根模块提供了用来启动应用的引导机制，可以包含很多功能模块。

`.mudule`文件需要使用一个`@Module()` 装饰器的类，装饰器可以理解成一个封装好的函数，其实是一个语法糖（对装饰器不了解的，可以看[走近MidwayJS：初识TS装饰器与IoC机制](https://juejin.cn/post/6859314697204662279#heading-2 "https://juejin.cn/post/6859314697204662279#heading-2"))。`@Module()` 装饰器接收四个属性：`providers`、`controllers`、`imports`、`exports`。

- providers：`Nest.js`注入器实例化的提供者（服务提供者），处理具体的业务逻辑，各个模块之间可以共享（_注入器的概念后面依赖注入部分会讲解_）；
- controllers：处理http请求，包括路由控制，向客户端返回响应，将具体业务逻辑委托给providers处理；
- imports：导入模块的列表，如果需要使用其他模块的服务，需要通过这里导入；
- exports：导出服务的列表，供其他模块导入使用。如果希望当前模块下的服务可以被其他模块共享，需要在这里配置导出；

如果你是Vue或者React技术栈，初次接触`Nest.js`，可能会觉得很面生啊， 其实很正常，`Nest.js`的思维方式一开始确实不容易理解，但假如你接触过`AngularJS`，就会感到熟悉，如果你用过 Java 和 Spring 的话，就可能会想，这不是抄的 Spring boot嘛！

确实`AngularJS`、`Spring`和`Nest.js`都是基于`控制反转`原则设计的,而且都使用了依赖注入的方式来解决解耦问题。如果你觉得一头雾水， 别急，这些问题后面深入学习都会一一讲解的。这里我们还是照葫芦画瓢，学一下Nest究竟怎么使用的。

在`app.module.ts`中，看到它引入了`app.controller.ts`和`app.service.ts`，分别看一下这两个文件：
```ts
// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller() 
export class AppController { 
   constructor(private readonly appService: AppService) {} 
   
   @Get()
   getHello(): string {     
       return this.appService.getHello();
  }
}
```

使用`@Controller`装饰器来定义控制器, `@Get`是请求方法的装饰器，对`getHello`方法进行修饰， 表示这个方法会被GET请求调用。

```ts
// app.service.ts
import { Injectable } from '@nestjs/common';
@Injectable()
export class AppService {
   getHello(): string {
      return 'Hello World!'; 
   }
 }
```

从上面，我们可以看出使用`@Injectable`修饰后的 `AppService`, 在`AppModule`中注册之后，在`app.controller.ts`中使用，我们就不需要使用`new AppService()`去实例化，直接引入过来就可以用。

至此，对于`http://localhost:9080/`接口返回的`Hello World`逻辑就算理清楚了， 在这基础上我们再详细的学习一下`Nest.js`中的路由使用。

## 路由装饰器

`Nest.js`中没有单独配置路由的地方，而是使用装饰器。`Nest.js`中定义了若干的装饰器用于处理路由。

### @Controller

如每一个要成为控制器的类，都需要借助`@Controller`装饰器的装饰，该装饰器可以传入一个路径参数，作为访问这个控制器的主路径：

对`app.controller.ts`文件进行修改

```ts
// 主路径改为 /app
@Controller("app")
export class AppController {
   constructor(private readonly appService: AppService) {}
      @Get()
         getHello(): string {
              return this.appService.getHello();
            }
 }
 ```

然后**重新启一下服务**，此时再去访问`http://localhost:9080/`会发现404了。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/996c9a91029f46fba0e8b0202a34d4e8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

就是由于通过`@Controller("app")`修改这个控制器的路由前缀为`app`, 此时可以通过`http://localhost:9080/app`来访问。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aee15b2b0f0544e7b85a24639b12988e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### HTTP方法处理装饰器

`@Get`、`@Post`、`@Put`等众多用于HTTP方法处理装饰器，经过它们装饰的方法，可以对相应的HTTP请求进行响应。同时它们可以接受一个字符串或一个字符串数组作为参数，这里的**字符串**可以是固定的路径，也可以是通配符。

继续修改`app.controller.ts`，看下面的例子：

```ts
// 主路径为 app
@Controller("app")
export class AppController { 
  constructor(private readonly appService: AppService) {}
  // 1. 固定路径：
  // 可以匹配到 get请求，http://localhost:9080/app/list
  @Get("list")
    getHello(): string {...}
  // 可以匹配到 post请求，http://localhost:9080/app/list
  @Post("list") 
    create():string{...}
 // 2.通配符路径(?+* 三种通配符 )
 // 可以匹配到 get请求, http://localhost:9080/app/user_xxx  
  @Get("user_*") 
    getUser(){
       return "getUser"
}      
  // 3.带参数路径
  // 可以匹配到put请求，http://localhost:9080/app/list/xxxx  
  @Put("list/:id") 
    update(){ return "update"}
 }
```
  
由于修改了文件， 需要重启才能看到路由， 每次都重启简直就是噩梦，本来打算配置一个实时监听文件变化，发现`Nest.js`非常贴心的配置好了， 我们只要运行命令即可：

```
npm run start:dev
```

这样再修改什么内容， 保存后都会自动重启服务了。

这里要提一个关于路由匹配时的注意点， 当我们有一个put请求，路径为`/app/list/user`,此时，我们在`app.controller.ts`控制器文件中增加一个方法：

 ```ts
 @Put("list/user")
   updateUser(){
          return {userId:1}
   }
```

你觉得这个路由会被匹配到吗？我们测试一下：

![postman11.09.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/68786c83748c4fe18196d18d7e64cd27~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

发现`/app/list/user`匹配到的并不是`updateUser`方法， 而是`update`方法。这就是我要说的注意点。

> 如果因为在匹配过程中， 发现`@Put("list/:id")`已经满足了,就不会继续往下匹配了，所以 `@Put("list/user")`装饰的方法应该写在它之前。

### 全局路由前缀

除了上面这些装饰器可以设置路由外， 我们还可以设置全局路由前缀， 比如给所以路由都加上`/api`前缀。此时需要修改`main.ts`

```ts
async function bootstrap() { 
const app = await NestFactory.create(AppModule);   app.setGlobalPrefix('api'); // 设置全局路由前缀 
await app.listen(9080); 
} 
bootstrap();
```

此时之前的路由，都要变更为：
```ts
http://localhost/api/xxxx
```

到此我们认识了`Controller`、`Service`、`Module`、路由以及一些常用的装饰器， 那接下来就实战一下，我们以开发文章（Post）模块作为案例, 实现文章简单的CRUD,带大家熟悉一下这个过程。

## 编写代码

写代码之前首先介绍几个`nest-cli`提供的几个有用的命令：

`//语法 nest g [文件类型] [文件名] [文件目录]`

- 创建模块

> nest g mo posts 创建一个 posts模块，文件目录不写，默认创建和文件名一样的`posts`目录，在`posts`目录下创建一个`posts.module.ts`

```ts
// src/posts/posts.module.ts 
import { Module } from '@nestjs/common';
@Module({})
export class PostsModule {}
```

执行完命令后，我们还可以发现同时在根模块`app.module.ts`中引入`PostsModule`这个模块，也在`@Model`装饰器的`inports`中引入了`PostsModule`


```ts
import { Module } from '@nestjs/common'; 
import { AppController } from './app.controller';
import { AppService } from './app.service'; 
import { PostsModule } from './posts/posts.module'; 
@Module({  
   controllers: [AppController], 
   providers: [AppService],  
   imports: [PostsModule],
 }) 
export class AppModule {}
```

- 创建控制器

> nest g co posts

此时创建了一个posts控制器，命名为`posts.controller.ts`以及一个该控制器的单元测试文件.


```ts
// src/posts/posts.controller.ts
import { Controller } from '@nestjs/common';
@Controller('posts')
export class PostsController {}
```

执行完命令， 文件`posts.module.ts`中会自动引入`PostsController`,并且在`@Module`装饰器的`controllers`中注入。

- 创建服务类

> nest g service posts



```ts
// src/posts/posts.service.ts 
import { Injectable } from '@nestjs/common';
@Injectable() 
export class PostsService {}
```

创建`app.service.ts`文件，并且在`app.module.ts`文件下，`@Module`装饰器的`providers`中注入注入。

其实`nest-cli`提供的创建命令还有很多， 比如创建过滤器、拦截器和中间件等，由于这里暂时用不到，就不过多的介绍，后面章节用到了再介绍。

> **注意创建顺序**： 先创建`Module`, 再创建`Controller`和`Service`, 这样创建出来的文件在`Module`中自动注册，反之，后创建Module, `Controller`和`Service`,会被注册到外层的`app.module.ts`

看一下现在的目录结构：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64b704b67f394971ac2e3efb231e9bca~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

  


