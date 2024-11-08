#  概述
Node.js 是一个基于 V8 JavaScript 引擎的 JavaScript 运行时环境

也就是说Node.js基于V8引擎来执行JavaScript的代码，但是不仅仅只有V8引擎：

+ V8 可以嵌入到任何C++应用程序中，无论是 Chrome 还是 Nodejs，事实上都是嵌入了V8 引擎来执行JavaScript代码
+ 在 Chrome 浏览器中，还需要解析、渲染 HTML、CSS 等相关渲染引擎，另外还需要提供支持浏览器操作的API、浏览器自己的事件循环等
+ 在 Nodejs 中也需要进行一些额外的操作，比如文件系统读/写、网络IO、加密、压缩解压文件等操作

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730018657141-badb1af4-4607-4ea4-9f5d-daf8a4cfea8d.png)

编写的JavaScript代码会经过V8引擎，再通过Node.js的Bindings，将任务放到Libuv的事件循环中，libuv是使用C语言编写的库

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730019404255-fbb87f59-3078-41d8-84b6-607744e802a6.png)



# Npm
npm（全称 Node Package Manager）是 Node.js 的包管理工具，它是一个基于命令行的工具，用于帮助开发者在自己的项目中安装、升级、移除和管理依赖项

## 功能
+ 包安装：可以通过命令行工具安装、更新、卸载包。
+ 依赖管理：在项目的package.json文件中自动记录项目所依赖的包及其版本，确保开发环境和生产环境的一致性。
+ 脚本运行：可以在package.json中定义脚本命令，如启动应用、运行测试等。
+ 版本控制：npm支持语义化版本控制，可以定义包的版本号以指示更新的性质（如主要更新、次要更新或补丁）。
+ 包发布：开发者可以将自己编写的模块发布到npm，供其他人使用。

## 常用命令
npm init：初始化一个新的 npm 项目，创建 package.json 文件

npm config get registry ：显示当前设置的npm源的URL

npm config set registry newUrl：切换npm源



npm install：安装包，install可以简写为i，当在项目目录中运行这个命令时，npm 会查看 package.json 文件，读取依赖项列表，并安装它们

+ npm install <package-name> --save：安装指定的包，并将其添加到 package.json 文件中的依赖列表中。
+ npm install <package-name> --save-dev：安装指定的包，并将其添加到 package.json 文件中的开发依赖列表中。
+ npm install -g <package-name>：全局安装指定的包
+ npm update <package-name>：更新指定的包
+ npm uninstall <package-name> ：卸载指定包

## package.json 的配置项
1. name: 项目的名称。这个名称必须是唯一的，因为如果你要将你的包发布到 npm，它将用作包的唯一标识符
2. version: 项目的当前版本。版本号应该遵循 语义化版本控制 的约定
3. description: 一个简短的描述，说明项目或模块是做什么的
4. main: 指定模块的入口文件，Node.js 将会加载这个文件当模块被 require 调用
5. scripts: 定义了一系列可以运行的脚本命令。常用的脚本命令包括 start或者dev（启动应用），test（运行测试），以及其他自定义脚本
6. repository: 指定项目的源代码仓库，通常是一个 Git 仓库
7. keywords: 一个由关键词组成的数组，有助于用户在 npm 搜索引擎中找到你的项目
8. author: 项目的作者的信息。这可以是一个人的名字也可以包括联系信息如电子邮件地址
9. license: 项目的许可证。这指定了其他人可以如何使用该项目的代码
10. dependencies: 项目运行所需的 npm 包列表。当通过 npm install <package_name> 安装依赖时，它们会自动被添加到这个列表中
11. devDependencies: 项目开发所需的 npm 包列表，这些依赖不会被包含在生产环境中
12. peerDependencies: 指定宿主项目需要遵循的依赖。当你的包被安装时，npm 会提示宿主项目的开发者也安装这些依赖
13. bundledDependencies: 这是一个数组，其中包含了项目依赖的包名称，这些依赖将一同打包发布
14. private: 如果设置为 true，将防止项目被意外发布到 npm
15. engines: 指定你的项目需要的 Node.js 版本

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730023104237-bbd4cd4e-18cb-4cbc-9bc3-8af6428c1f5a.png)

## npm install 原理
1. 解析 package.json 文件：
+ npm首先读取项目根目录下的 package.json 文件，解析该文件中的 dependencies 和 devDependencies 对象，获取项目所需的所有依赖包及其版本号。
2. 查询并解析依赖：
+ 对于每个依赖，npm会查询npm注册中心，找到与package.json中指定的版本号相匹配的依赖版本。如果依赖还包含自己的依赖（子依赖），npm也会递归地进行查询和解析(需要注意的是，这里采用的是广度优先算法)。
3. 检查本地缓存：
+ npm会检查本地缓存中是否已经存在所需依赖的副本。如果本地缓存中已有所需版本的依赖，则直接从缓存中获取，而不会从npm注册中心下载。
4. 下载依赖：
+ 对于本地缓存中不存在的依赖，npm会从npm注册中心下载依赖包。下载的依赖包包含了依赖的代码和它自己的 package.json 文件。
5. 安装依赖：
+ 下载后的依赖包被解压到 node_modules 目录中。如果依赖包含子依赖，npm也会递归地安装这些子依赖。
6. 执行生命周期脚本：
+ 在安装过程中，npm会检查依赖的 package.json 文件中是否定义了生命周期脚本（如 preinstall、install、postinstall 等）。如果有定义，npm会在相应的时机执行这些脚本。
7. 生成或更新 package-lock.json 或 npm-shrinkwrap.json 文件：
+ npm会生成或更新 package-lock.json 文件，以确保未来安装时能够得到相同版本的依赖。这有助于项目在不同环境和不同开发者之间保持依赖的一致性。
8. 整理 node_modules 目录结构：
+ 为了避免冗余和冲突，npm可能会对 node_modules 目录进行整理，优化依赖的存储结构。从npm v3开始，尽可能将依赖扁平化，以减少目录深度和重复的依赖副本。

## npm run serve & npm run dev
通俗解释

> npm run serve：你把餐厅的门打开，让顾客（在这里是浏览器）可以进来。但它可能只是最基本的服务，比如提供食物（网站文件），但不包括一些额外的装饰或服务（比如自动刷新你的浏览器，当你更改了食物配方后）。
>
> npm run dev：你不仅打开了门，还开启了一些特别的服务，比如现场音乐、特别的装饰，甚至还有服务员（开发工具）来帮助你更好地服务顾客。这可能包括自动刷新你的浏览器，当你更改了代码后，或者提供一些额外的功能，让你的开发过程更加顺畅。
>
> 简单来说：
>
> + npm run serve 就是让你的网站跑起来，让浏览器可以访问。
> + npm run dev 不仅让网站跑起来，还开启了一些帮助开发者的工具和特性，让开发过程更加方便。
>

专业解释

> npm run serve
>
> + 这个命令通常用于启动一个静态文件服务器
> + 在某些框架或构建工具中，serve 可能被配置为执行额外的任务，比如自动刷新浏览器（live reload）、提供代理服务（proxy）以便前端可以访问后端 API 等
> + serve 命令可能与 npm run build 命令配合使用，用于在开发过程中快速查看构建结果
>
> npm run dev
>
> + 这个命令通常用于启动一个开发环境，它可能包括启动一个开发服务器，并且开启额外的开发特性，如源代码映射（source maps）、更详细的错误报告等
> + dev 模式可能还包括开启某些只在开发环境中需要的插件或工具，比如 linters、formatters 或其他用于提高开发效率和代码质量的工具
> + 在某些情况下，dev 命令可能与特定的开发服务器配置相关联，比如 webpack-dev-server、Vue CLI 的 serve 命令等
> + 具体用哪个，可以在 package.json 的 scripts 中查看
>

![两者都可以使用](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730024896625-bae9a0f8-cc55-4f02-b71e-56875c8d9b9e.png)





## npx
npx 是一个 npm 包执行器，它随 npm 5.2.0 版本一起引入。npx 的主要用途是方便开发者执行安装在项目本地 node_modules/.bin 目录或全局安装的 npm 包的命令

### 特点
+ 临时安装并运行包：
    - 使用 npx 可以执行未全局安装的包，npx 会临时安装这些包并运行它们，执行完毕后不会保留这些包
+ 运行项目本地安装的包：
    - 项目中经常会安装一些命令行工具作为开发依赖，例如 eslint、webpack 等。通常情况下，如果想要运行这些工具，需要在项目的脚本中定义它们或者通过项目的 node_modules/.bin 路径来运行。npx 简化了这个过程，允许直接在命令行中输入命令来执行
+ 避免全局安装包：
    - 有时全局安装 npm 包可能会导致版本冲突或其他问题。npx 允许我们执行包而无需全局安装，减少了全局空间的污染
+ 执行 GitHub 上的包：
    - npx 还可以直接执行存储在 GitHub 等代码托管服务上的包，这对于试运行开源项目中的示例脚本或工具非常方便

### npx 和 npm 联系
1. npx 是 npm 包的一部分，是 npm 的一个功能，随 npm 一起安装
2. npx侧重于执行命令的，执行某个模块命令。虽然会自动安装模块，但是重在执行某个命令
    - 比如使用 npx create-react-app my-app 来创建一个新的 React 应用，而不需要事先安装，虽然执行时会安装，但是执行完就删除了
3. npm侧重于安装或者卸载某个模块的。重在安装，并不具备执行某个模块的功能。







## 发布 Npm 包
1. 检查镜像源`npm get register`，需要是 npm 官网的(https://registry.npmjs.org/)
2. 创建账号 `npm adduser`
3. 登录账号 `npm login`
4. 发布包 npm publish
5. 查询是否发布成功 [npm | Home](https://www.npmjs.com/)



**发布规则**

1. 包名是唯一的
2. 使用语义化版本控制，每次发布都需要更新版本号
3. 注意 package.json
    - name 和 version 字段是必须的
    - main 字段指定了包的入口文件
    - scripts 字段可以包含运行测试和构建等脚本
    - dependencies 和 devDependencies 字段定义了包所需的其他 npm 包
    - repository、keywords、author、license 等字段虽然不是必须的，但非常有助于包的发现和使用
4. 包应该指出许可证类型，确保别人知道如何合法使用
5. 包需要包含一个 README.md 文件，介绍包的功能、安装方法、使用实例、API 文档等
6. 包应该通过单元测试，符合基本的可读性要求
7. 如果想发布一个私有包，需要在 package.json 中设置 "private": true，或者使用付费的 npm 组织账户
8. 如果你不希望某些文件或目录包含在你的包中，可以使用 .npmignore 文件来排除它们

## npm 搭建私服
npm私服（私有仓库）指的是建立在企业或组织内部的、独立于公共 npm 注册中心的私有 Node 包管理器。私服允许团队在不公开源码的情况下共享和复用代码，也可以用来代理公共 npm 注册中心，缓存下载过的包，提高安装速度，减少对外部网络的依赖

### **常见仓库**
+ Nexus Repository OSS：一个广泛使用的仓库，支持多种包类型，包括 npm
+ JFrog Artifactory：一种企业级的解决方案，支持多种包管理和自动化工具
+ Verdaccio：一个轻量级的私有 npm 代理注册中心，容易部署和维护
+ GitHub Packages：GitHub 提供的包管理服务，支持 npm 私有包

### 示例
下面用 verdaccio 做示例

1. 安装`npm install verdaccio -g`
2. 启动：`verdaccio`
3. 配置：
    - 自定义端口号运行 `verdaccio --listen 9333`，默认是 4873
    - 指定配置文件运行 `verdaccio --config /xxx/xxx/xxx`
4. 创建用户： `npm adduser --registry http://localhost:4873`
5. 登录：`npm login --registry http://localhost:4873`
6. 发布：`npm publish --registry http://localhost:4873`
7. 使用托管：`registry=http://localhost:4873`，之后就可以使用 Npm install 安装了
8. 保持 Verdaccio 服务的运行

#### 取消后缀
每次都需要加上--registry http://localhost:4873/这样的一个后缀就会很麻烦

可以使用包管理器 xmzs 提供的 mmp add 命令

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730075903560-9c70b441-e99c-418a-a087-423b180ba5f8.png)

然后使用 mmp use 这个镜像

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730075934419-559ccb41-2b1d-4904-b0d6-79a8a148541b.png)

后面使用直接用指令即可

# 模块化
Nodejs 模块化规范遵循两套规范，一套CommonJS规范，另一套 ESM 规范

## CommonJS 
### 由来
CommonJS 规范诞生于服务器端 JavaScript 的早期，尤其是在 Node.js 出现之前。在那个时候，JavaScript 主要用于浏览器，没有模块化的概念，这意味着所有JavaScript代码和库都是通过 <script> 标签引入，并且全都在同一个全局作用域中。这种做法很容易造成命名冲突，而且代码组织和依赖管理也很混乱。

随着 JavaScript 开始被用于更复杂的服务器端开发，需要一种方法来组织和封装代码，以便易于管理和重用。因此，CommonJS 规范被提出，目的是为 JavaScript 创建一个模块生态系统。CommonJS 模块允许开发者在一个文件中导出一个或多个对象、函数或变量，并在另一个文件中通过 require 函数来同步导入这些模块。

Node.js 选择 CommonJS 作为其模块系统的基础，因为它适合服务器端编程，这种环境下，模块文件通常是本地可用的，因此可以同步加载模块，而不会对性能产生太大影响。



### 规范
在package.json文件里面的type属性就能看到当前使用的是哪套规范

![ESM](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730076299267-b9225c41-0dc2-4f45-82c8-7ebb59300456.png)

默认情况下为commonJS，可以选择不写type这个属性，那就会以默认值为准

#### 模块引入
1. 引入自己编写的模块：`require('./test.js')`
2. 引入第三方模块：比如 md5，安装之后`const md5 = require("md5");`
3. 引入内置模块：比如 fs，`const fs = require("node:fs");`
4. 引入 Json：`const data = require('./data.json')`

#### 模块导出
##### module.exports
是 CommonJS 规范中最常用的导出方式，本质上是一个对象

可以通过它导出一个对象、类、函数或任何其他有效的 JavaScript 值。被 module.exports 导出的内容可以通过 require 函数导入到另一个模块中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730076725812-01719d46-9d59-4d5d-8806-d18de3998800.png)

##### exports
exports 实际上是 module.exports 的一个别名(引用)

不能直接将 exports 重新赋值为一个新的对象，因为这样会断开 exports 和 module.exports 之间的引用关系，而 require 函数只会返回 module.exports 的内容

所以在使用时，只能用 `exports.xxx=xxx` 的方式添加属性到 exports 对象上

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730077953128-49393c54-84be-4afb-88c3-c1d5824aa65f.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730077957094-af9cf3d4-8b23-4b18-9a25-1ca429213cb3.png)

#### 注意事项
不可以混用 exports 和 module.exports

## ECMAScript Modules (ESM) 
### 由来
随着前端开发的日益复杂化和模块化需求的增长，以及 JavaScript 语言标准的不断发展，社区感到需要一个内置在语言层面的模块系统，这样的系统应当同时适用于服务器和浏览器环境。

这促使了 ECMAScript 标准（JavaScript的官方标准）的制定者引入了 ESM 规范。ESM 作为 ECMAScript 2015（ES6）的一部分被加入 JavaScript 语言标准，它采用 import 和 export 语句来导入和导出模块，这些操作是异步完成的，特别适合于网络加载模块。

ESM 的设计是为了支持静态分析和树摇（tree shaking），这意味着工具可以在构建时分析代码中的 import 语句，仅打包实际使用的模块代码，从而减少最终应用程序的体积。此外，它也支持循环依赖和动态导入。



### 规范
#### 模块导出
首先需要再 package.json文件中，将type类型改为"module"，才能运行ESM规范的引入导出

##### 默认导出 default exports
每个模块可以有一个默认导出(且仅能有一个)，下面两种方式都可以

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078225405-a1eb290e-1939-4324-85ec-94634df5e348.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078242654-96ce1f2d-2161-4a99-9e82-03616ac8376a.png)

##### 命名导出 named exports
直接在要导出的变量、常量、函数、对象、类等前面加上export关键字

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078295873-67af7d2f-0e63-49e6-b473-0ba70e6ddefc.png)

##### 特殊导出方式
![集中多个导出](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078348418-afcd0bc8-bf84-41d0-a88b-3fc79362c69c.png)

![混合导出](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078368875-0c17f8dc-69dd-4682-9ab8-56135f95e62e.png)

![重命名导出](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078382538-64b051e2-5f11-4816-b80c-2a8c782d781e.png)

![全部导出](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078401074-2eb964a8-96c2-4651-b490-d94cdda4c31e.png)![选择性导出](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078430768-868b12cb-9edc-4744-b9f0-fc4395942dfb.png)

#### 模块导入
##### 默认导入
当一个模块使用export default导出了一个默认成员时，可以使用import关键字配合任意名称来导入它

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078582081-31961c3b-9b3e-4893-afae-ad7095d20a07.png)

##### 命名导入
对于模块中使用export导出的命名成员，可以通过指定名称来导入，也就是解构

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078600303-e186991a-d425-4b8d-a7ac-3677a87f429a.png)

##### 特殊导入方式
![混合导入](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078644037-4b14d09e-2f72-4d27-924e-e2e96ec8c092.png)![重命名导入](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078623377-a2381de4-5c96-4597-8436-62838b955a36.png)![全部导入](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078633289-78681fa9-ea70-483c-8883-e310825789df.png)

![ES2020：动态导入 Import()](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730078670951-a7ffaf68-bf68-4c1a-8a60-30f7bcd29dd1.png)

#### 注意事项
浏览器中使用 ESM 时，<script> 标签需要有 type="module" 属性，或者把 package.json的type属性要调节为module

ESM规范是不支持引入JSON文件的。有些地方可以import进JSON文件，是因为vite和webpack的loader去处理了，所以才能使用，正常情况下是用不了的



## 两者的区别
1. 加载机制：CommonJS 模块是运行时加载，ESM 模块则是编译时输出接口
    - 因此 Cjs 允许一些动态编程技巧，比如基于条件的导入模块，或者在任意位置导入模块，甚至在函数内部
    - 由于 ESM 的静态性质，所有的 import 和 export 语句必须位于模块的顶层作用域，不能被条件语句包围，也不能动态生成；如果非要只用，那必须使用 ES2020 的动态导入 import()
2. 语法：Cjs 使用 require 和 module.exports，ESM 使用 import 和 export
3. 异步加载：Cjs 通常不支持异步加载和动态导入，而 ESM 是支持的
4. 执行环境：Cjs 主要用于 Node.js 环境，而 ESM 设计时考虑了跨环境，可以在现代浏览器和 Node.js 中使用
5. 摇树优化：Cjs不可以tree shaking，esm支持tree shaking
6. this 指向：Cjs中顶层的this指向这个模块本身，而ES6中顶层this指向undefined
7. 值修改：Cjs是可以修改值的，esm值并且不可修改（可读的）

## 内置模块
Node.js 提供了许多内置模块，这些模块不需要额外安装，可以直接使用

1. assert：提供了一个简单的断言测试，用于调试目的
2. buffer：提供了对缓冲区的操作，缓冲区是固定长度的字节序列
3. child_process：允许你创建子进程
4. cluster：允许你容易地创建共享服务器端口的子进程
5. console：提供了一个类似于 Web 浏览器的 console 对象
6. crypto：提供了加密功能，包括哈希、HMAC、加密和解密算法等
7. dns：提供了域名查询和操作功能
8. events：允许你使用事件驱动模式
9. fs（文件系统）：提供了文件操作功能，如读取和写入文件
10. http：提供了创建 HTTP 服务器和客户端的功能
11. https：提供了创建 HTTPS 服务器和客户端的功能
12. net：提供了网络操作功能，如 TCP 和 IPC 通信
13. os：提供了操作系统相关信息
14. path：提供了路径处理功能，如路径的解析、连接、规范化等
15. process：代表当前 Node.js 进程，允许你与之交互
16. punycode：提供了 Punycode 转换功能，用于国际化域名
17. querystring：提供了处理查询字符串的功能
18. readline：提供了从命令行读取数据的功能
19. repl：提供了一个交互式命令行接口
20. stream：提供了流操作功能，如可读流和可写流
21. string_decoder：用于解码字符串流
22. timers：提供了定时器功能，如 setTimeout 和 setInterval
23. tls：提供了创建 TLS 服务器和客户端的功能
24. tty：提供了终端操作功能
25. url：提供了 URL 操作功能，如解析和格式化 URL
26. util：提供了实用工具，如文本和二进制数据的转换
27. v8：提供了与 V8 引擎交互的功能
28. vm：提供了运行代码在虚拟机上下文中的功能
29. zlib：提供了压缩和解压缩功能

# 全局变量
平时定义全局变量是采用var关键字来进行定义的，而这里正常定义的全局变量是放在window全局对象之下的，但在Node中，是没有window这个对象的

## Global 对象
Node.js 中的 global 对象是一个全局命名空间对象。在任何模块中给 global 对象添加属性，都会使这些属性成为全局可访问的变量

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730080527437-a733ea06-699b-473d-a342-4c356209bd4c.png)

特殊情况

+ 全局变量应在使用之前被初始化，只有在创建语句执行之后才可用
+ 如果在模块 A 中 require 了模块 B，并且模块 B 定义了全局变量，那么在模块 A 的 require 调用之后（即模块 B 的代码执行完毕后）这些全局变量才可以在模块 A 中使用
+ 如果一个全局变量的初始化依赖于异步操作（如文件读取、数据库查询等），那么即使全局变量已经被定义，它的值也可能在模块的其余代码执行时还未准备好，对于这种情况，应该使用回调、Promise 或 async/await 来确保变量在使用前已经被正确初始化
+ 如果希望在一个模块中定义全局变量，并确保它在其他模块中也是可访问的，可以先将变量挂载到 global 对象，然后从模块中导出它![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730081039654-03919bd5-cc41-4ebe-b6f2-1e34c1fc5bca.png)



注意事项

+ 注意变量名，避免命名冲突
+ 尽量少使用全局变量，会难以维护和理解
+ 全局变量在整个应用的生命周期内都是存在的，可能会导致意外的副作用和内存泄漏
+ 所以优先考虑使用其他模式，如模块导出或使用类和构造函数，来共享和重用代码，而不是依赖全局变量

## GlobalThis 对象
window和global分别是浏览器和node的不同全局对象形式，这会导致一个比较麻烦的问题：如果要开发一个第三方库，要兼容node 和浏览器环境时就很麻烦，所以在ES 2020的时候，提供了globalThis这个API，能够同时作用在浏览器和node环境

globalThis 提供了一个统一的方式来引用全局对象，他会根据具体的环境自己判断是使用window还是global

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730081346684-808f6cdf-93b8-478d-a0e9-7f90ddf1ff26.png)

## 全局 API
由于nodejs中没有DOM和BOM，除了涉及到DOM和BOM的这些API用不了，其他的ECMAscriptAPI基本都能用，下面记录两个之前没见过的

+ __dirname：表示当前模块的所在目录的绝对路径
+ __filename：表示当前模块文件的绝对路径，包括文件名和文件扩展名

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730081515853-ac9fd16f-7c3a-4a2a-9502-423a843ad65a.png)

# SEO SSR CSR
## SEO
Search Engine Optimization，即搜索引擎优化

SEO讲究三个东西，分别是TDK，即 Title、Description、Keywords

1. 标题（Title）：这是网页的标题标签（<title>），在浏览器的标题栏和搜索引擎结果页（SERP）中显示
2. 描述（Description）：描述是元描述标签（<meta name="description" content="...">）中的文本，虽然它对排名影响不大，但在搜索引擎结果页中显示，为用户提供了页面内容的简短摘要
3. 关键词（Keywords）：关键词是指明网页内容主题的词或短语，它们应该反映用户可能用来搜索你的内容的词汇。过去，<meta name="keywords" content="...">标签被用来明确指出页面的关键词，但由于滥用和过度优化，现在这个标签对大多数搜索引擎的排名几乎没有影响。

## SSR
服务器端渲染（SSR）是一种传统的网页渲染方式，其中HTML内容在服务器上生成，然后发送到客户端进行展示



**流程**

1. 用户请求：用户请求一个网页。
2. 服务器处理：服务器接收请求，运行应用的服务器端代码来生成页面内容，包括HTML、CSS和初始状态的数据。
3. 发送HTML：服务器将生成的HTML和相关资源（如CSS、JS）发送给浏览器。
4. 浏览器展示：浏览器接收到完整的HTML，渲染页面并展示给用户。
5. 客户端激活：浏览器加载页面的JavaScript文件，JavaScript代码在浏览器中运行，页面变得交互式（这一步也称为水合 hydration）



**优点**

+ 更快的首屏时间：用户不需要等待所有JavaScript都加载和执行完毕才能看到完整的页面，因此首屏展示更快
+ 更好的SEO：搜索引擎更容易抓取和索引服务器渲染的页面，因为内容在HTML中直接提供，而不是通过JavaScript动态生成
+ 对低性能设备友好：服务器端已经完成了渲染工作，客户端设备的计算负担较轻

****

**缺点**

+ 服务器负载：每次页面请求都需要服务器生成HTML，对服务器的计算资源要求更高
+ 用户体验：页面在与服务器通信时可能会有短暂的空白期，特别是在数据更新或页面跳转时
+ 复杂度：服务器端渲染的应用通常比纯客户端渲染的应用更复杂，因为它们需要处理客户端和服务器两端的逻辑

****

**使用场景**

+ 服务器端渲染（SSR）：在 Node.js 环境中渲染初始的 HTML 页面，通常用于提高 SEO 和首屏加载性能
+ 测试：测试需要 DOM 环境的 JavaScript 代码，尤其是在不启动浏览器的情况下进行自动化测试
+ 网页抓取和自动化：加载和操作网页，模拟用户操作，用于爬虫或自动化脚本
+ 网页应用原型开发：允许开发人员在没有浏览器的环境中快速构建和测试网页应用的原型

### jsdom
前面提到在node环境中无法操作DOM 和 BOM，但是如果非要操作DOM 和 BOM 的话，通过第三方库jsdom也可以实现，它在 Node.js 环境中模拟了一个 web 浏览器的环境



**主要功能**

1. DOM 操作：提供一个类似于浏览器中的 document 对象，允许像在浏览器中一样创建、修改和查询 DOM 元素
2. CSS 解析：支持读取和应用 CSS 规则，虽然它不会渲染实际的布局，但可以理解样式信息
3. 事件模拟：可以在 DOM 元素上触发事件，模拟用户交互行为，如点击、输入等
4. Canvas 支持：通过第三方库可以模拟 <canvas> 元素的功能（jsdom 自身不包含绘图能力）
5. 资源加载：能够加载和执行外部脚本和资源，例如 JavaScript 文件或图片
6. 浏览器 API 模拟：提供浏览器全局对象如 window 和 navigator，以及其他 API 如 localStorage、XMLHttpRequest 等
7. 测试工具：常用于测试环境中，为在 Node.js 环境下运行的客户端 JavaScript 代码提供测试平台



jsdom 的使用在后面的项目中再具体学习

## CSR
客户端渲染，网页的内容主要是通过客户端JavaScript在用户的浏览器上动态生成的

在客户端渲染过程中，浏览器首先加载一个几乎为空的HTML文件，这个文件包含了启动应用所需的JavaScript脚本(故第一次加载会很慢)。

当JavaScript被下载并执行后，它将动态地在浏览器中构建和渲染页面的元素，通常是通过操作DOM来实现的。客户端渲染通常与**单页面应用（SPA）**相关联，如使用React、Vue、Angular构建的应用



**特点**

1. 初始载入：首次加载时，服务器只发送一个简单的HTML页面和必要的JavaScript代码。
2. 交互性：页面的后续动态变化都是通过客户端JavaScript直接在浏览器中进行的。
3. 数据请求：应用的数据通常通过异步请求（如使用Ajax或Fetch API）从服务器获取，并在客户端进行处理和展示。
4. 用户体验：在应用加载后，用户可以获得流畅的交互体验，因为页面无需重新加载即可更新内容
5. 资源利用：客户端渲染依赖用户设备的资源（CPU、内存）来处理渲染和数据交互
6. SEO优化：客户端渲染的SPA可能需要额外的SEO优化措施，因为搜索引擎爬虫在执行JavaScript时可能不如服务器渲染的页面高效(等下还会提到)

## 怎么选择
**CSR 对比 SSR**

1. 内容生成位置：CSR在客户端生成内容，SSR在服务器生成内容
2. 首屏加载速度：SSR通常提供更快的首屏加载，因为HTML是预先生成的
3. 资源利用：CSR主要依赖客户端资源，SSR增加了服务器的计算负担
4. SEO优化：SSR对搜索引擎抓取更友好
5. 开发复杂度：SSR涉及更复杂的配置和构建过程，需要处理客户端和服务器两端的逻辑



**CSR网站**

1. 单页面应用 SPA ：使用React、Vue、Angular等构建的应用，用户在应用内的导航无需重新加载整个页面，提供了流畅的用户体验
2. 富交互应用：需要大量动态交互和即时内容更新的应用，如在线协作工具、社交网络平台
3. 应用程序仪表板：后台管理界面、分析仪表板等，用户登录后进行大量的数据交云和操作
4. 对SEO要求不高的内部系统或应用：例如企业内部管理系统、SaaS应用等，这类应用可能只对特定用户开放，不依赖搜索引擎带来流量



**SSR网站**

1. 内容驱动的网站：如新闻、博客、文章发布平台等，这些网站内容为王，需要良好的SEO来吸引更多流量
2. 电商网站：对于加载速度和SEO都有高要求的电商网站，SSR可以提供更快的首屏加载时间，提高转化率
3. 营销和宣传网站：对于需要优化搜索引擎排名以吸引潜在客户的网站，SSR能够确保内容被搜索引擎有效抓取
4. 初次访问速度要求高的应用：对首屏加载速度有严格要求的应用，SSR可以减少白屏时间，改善用户体验

# Path 模块
Node.js 的 path 模块在不同的操作系统中表现有所不同

这些差异主要源于 Windows 和 POSIX（Linux、macOS等）系统使用不同的路径分隔符和不同的路径表示方式

## Windows 和 POSIX 路径的差异
Windows：

+ 路径分隔符是反斜杠（\），例如 C:\Users\Example\file.txt，这是一个历史原因导致不完全遵守POSIX标准
+ 文件路径可以是相对的也可以是绝对的，绝对路径通常以盘符开头
+ 不区分大小写

POSIX：

+ 路径分隔符是正斜杠（/），例如 /home/example/file.txt
+ 文件路径同样可以是相对的或绝对的，绝对路径以根目录 / 开头
+ 区分大小写

## Path 的用法
Node.js 中的 path 模块提供了一系列实用工具，用于处理文件和目录的路径。这个模块可以帮助我们构造、解析和转换路径字符串

由于不同操作系统中文件路径的表示方式可能不同，path模块封装了这些差异，使路径操作在不同平台上更加一致

### path.basename
path.basename(path[,ext])，返回 path(给定路径) 的最后一部分，可以通过 ext 参数去除文件扩展名

```javascript
const path = require('path');

console.log(path.basename('/foo/bar/baz/asdf/quux.html'));; 
// 返回: 'quux.html'

console.log(path.basename('\\foo\\bar\\baz\\asdf\\quux.html'));;
//返回: 'quux.html'
//其中\需要写两个是因为需要进行转义(Windows写法，POSIX处理不了)

console.log(path.basename('/foo/bar/baz/asdf/quux.html', '.html'));; 
// 返回: 'quux'
```

### path.dirname
path.dirname(path)，返回 path 的目录部分，类似于 Unix 中的 dirname 和node中的__dirname命令

```javascript
const path = require('path');

console.log(path.dirname('/foo/bar/baz/asdf/quux.html'));
// 返回: '/foo/bar/baz/asdf'

console.log(path.dirname('\\foo\\bar\\baz\\asdf\\quux.html'));
//返回'\foo\bar\baz\asdf'
```

### path.extname
path.extname(path)，返回 path 的扩展名，从最后一个.到字符串结束。如果没有.或path基本名称以.开始，则返回空字符串，通常可以用来判断当前文件是什么个东西(是图片还是个视频或者是一个js文件)

```javascript
const path = require('path');

console.log("1",path.extname('index.html'));; // 返回: '.html'
console.log("2",path.extname('index.coffee.md'));; // 返回: '.md'
console.log("3",path.extname('index.'));; // 返回: '.'
console.log("4",path.extname('index'));; // 返回: ''
console.log("5",path.extname('.index'));; // 返回: ''
```

### path.join
path.join([...paths])，将所有给定的 paths 片段连接在一起，然后规范化生成的路径(拼接路径)，常用来处理平台特定的分隔符，如POSIX上的/和Windows上的\

```javascript
const joinedPath = path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
// POSIX: '/foo/bar/baz/asdf'
// Windows: '\\foo\\bar\\baz\\asdf'
console.log(joinedPath);
//为什么结果会没有quux这个内容，那是因为最后还有一个..，这个join拼接是支持回退的
//如果是../，返回的结果就会是\foo\bar\baz\asdf\，最后面多一个斜杠
```

### path.resolve
path.resolve([...paths])，将路径或路径片段的序列解析为绝对路径。给定的路径序列从右至左被处理，直到构造出绝对路径为止

+ 如果在处理所有给定的路径片段后还未能生成绝对路径，则会使用当前工作目录
+ 如果都是绝对路径，就返回最后一个
+ 如果只有一个相对路径，返回当前工作的绝对路径
+ 相对路径+绝对路径(典型的__dirname和当前文件拼接)，返回的还是绝对路径

```javascript
const path = require('path');

// 示例 1: 基本用法
console.log(path.resolve('/foo/bar', './baz'));
// 输出: '/foo/bar/baz'

// 示例 2: 使用 '..' 导航到父目录
console.log(path.resolve('/foo/bar', './baz', '../qux'));
// 输出: '/foo/bar/qux'

// 示例 3: 当路径为空时，默认为当前工作目录
console.log(path.resolve());
// 输出: 当前工作目录的绝对路径
// 解释: 由于没有提供路径，path.resolve将返回当前工作目录的绝对路径

// 示例 4: 当最后一个参数是绝对路径时
console.log(path.resolve('/foo/bar', '/tmp/file/'));
// 输出: '/tmp/file'
// 解释: '/tmp/file/'是绝对路径，因此path.resolve立即返回它

// 示例 5: 当路径片段是相对路径时
console.log(path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif'));
//输出: 如果当前工作目录为'/home/myself/node'
//则返回'/home/myself/node/wwwroot/static_files/gif/image.gif'

// 示例 6: 结合Windows盘符使用（在Windows环境下）
console.log(path.resolve('C:\\path\\to\\file\\', '..\\img\\picture.png'));
// 输出: 'C:\path\to\img\picture.png'

// 示例 7: 使用多个相对路径片段
console.log(path.resolve('tmp', 'local', '..', 'uploads'));
// 输出: 如果当前工作目录为'/home/user/project'
//则返回'/home/user/project/tmp/uploads'

// 示例 8: 没有找到绝对路径时使用当前工作目录
console.log(path.resolve('myapp', 'views'));
// 输出: 如果当前工作目录为'/home/user/project'
//则返回'/home/user/project/myapp/views'
```

### path.parse
path.parse(path)，将 path 字符串解析成一个对象，包含 dir、root、base、name 和 ext 等属性

```javascript
const path = require('path');

path.parse('/home/user/dir/file.txt');
// 返回:
// { root: '/',
//   dir: '/home/user/dir',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file' }
```

### path.format
path.format(pathObject)，从一个对象返回路径字符串，是 path.parse 的逆操作

需要加上对应的操作系统，`path.win32.format() path.posix.format()`

```javascript
const path = require('path');

console.log(path.posix.format(
  {
    root: '/',//根目录
    dir: '/home/user/dir',//文件所在目录
    base: 'file.txt',//文件名+后缀名
    ext: '.txt',//文件扩展名(后缀名)
    name: 'file'//文件名
  }
))
// 返回/home/user/dir/file.txt
```

### path.sep
path.sep，用于确定当前系统的路径分隔符

```javascript
const path = require('path');

// 输出路径分隔符
console.log(path.sep); 
// 在 POSIX 系统上，输出：'/'
// 在 Windows 系统上，输出：'\\'
```

# OS 模块
Node.js 中的 os 模块提供了一系列操作系统相关的实用工具函数。能够直接从Node.js应用程序中查询底层操作系统的信息和使用操作系统相关的功能。由于它是Node.js的核心模块，因此无需安装，直接引入就能用，`const os  = require('os')`

## 功能
1. 获取操作系统的基本信息：如当前登录用户、操作系统类型、系统架构、主机名、系统运行时间等
2. 查询CPU信息：如CPU的型号、速度、核心数以及每个CPU核心的负载信息
3. 查询内存信息：如系统的总内存、空闲内存等
4. 网络接口信息：获取网络接口的详细信息，如IPv4和IPv6地址、MAC地址等

## 方法
+ os.platform：返回一个字符串，标识Node.js进程运行其上的操作系统平台（如'darwin'、'win32'、'linux'等）
+ os.release：获取操作系统版本
+ os.type：返回操作系统的名字（如 'Linux'、'Darwin'（macOS的内核名称）、'Windows_NT'等）
+ os.version：输出当前系统的版本，比如 windows 11
+ os.homedir：读取到用户名所在的目录，即 C盘到用户名的部分，C:\Users\3278
+ os.arch：返回一个字符串，表示Node.js二进制编译所用的架构
+ os.cpus：返回一个对象数组，包含安装的每个CPU/核的信息，包括型号、速度（以MHz为单位）和时间（一个包含用户、系统和空闲时间的对象）![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730098152179-80f59b67-5c52-4e28-903f-475d61f90b72.png)
+ os.networkInterfaces：网络接口，它的作用就是查看网络信息

# Process 模块
process 是Nodejs操作当前进程和控制当前进程的API，并且是挂载到globalThis下面的全局API(作为一个全局对象，process无需通过require来引入，可以在任何地方直接使用)

1. process.arch：返回操作系统 CPU 架构，同 os.arch，如 x64
2. process.platform：运行Node.js进程的操作系统平台，如'darwin', 'win32'
3. process.argv：包含启动Node.js进程时传入的命令行参数的数组
    - process.argv[0]：通常是 node 的路径
    - process.argv[1]：被执行的JavaScript文件的完整路径
    - process.argv[2]、process.argv[3] ...：任何额外的命令行参数
4. process.cwd：返回Node.js进程的当前工作目录(不包括当前文件)，同 __dirname，但__dirname是node的产物，在ESM模式下是用不了的，这时候就可以使用process的cwd代替一下
5. process.memoryUsage：返回Node.js进程的内存使用情况的对象![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730098441633-8df6a515-6b26-49ef-9b9d-0324fcad20b5.png)
6. process.exit([code])：使用指定的 code 终止当前进程。如果省略，将使用代码 0 或上一个设置的退出码，这个事件会退出当下的执行
7. process.kill(pid, [signal])：pid 参数是要发送信号的进程的ID，[signal] 参数是要发送的信号的名称或数字标识。如果省略了 [signal] 参数，将发送默认的 SIGTERM 信号
8. process.env：用于读取操作系统所有的环境变量，也可以修改和查询环境变量，但只会在当前进程生效，不会真正影响系统的环境变量





# Child_process 模块
## 概念
child_process 模块是 Node.js 的一个核心模块，可以从 Node.js 应用程序中创建和管理子进程。

+ 子进程是从父进程（你的 Node.js 应用）中派生出来的新进程，它可以执行系统命令、运行其他应用或执行 JavaScript 文件
+ 使用子进程，可以实现多任务处理，提高应用的性能和效率
+ 使用子进程时需要注意安全问题，特别是当执行的命令包含来自用户输入的部分时，需要防止注入攻击
+ 过多的子进程可能会耗尽系统资源，需要合理管理子进程的数量



**使用场景**

1. 执行系统命令：从 Node.js 应用中执行 Shell 命令时，如 ls, grep, mkdir 等
2. 运行其他应用程序：在 Node.js 应用中运行其他外部应用程序，比如 Python 脚本、Ruby 程序或其他可执行文件
3. 并行处理：需要并行执行多个任务以提高应用性能时，可以使用子进程，这在处理 CPU 密集型任务时尤其有用
4. 资源密集型操作：对于一些资源密集型或长时间运行的操作，使用子进程可以避免阻塞 Node.js 的事件循环

## 创建子进程--未掌握(需要 linux 知识)
共有7个常用的API，加Sync后缀的是同步API，没加的是异步API，异步一般会提供一个回调函数，返回一个buffer(Buffer相当于是一个字节的数组，数组中的每一项对应一个字节的大小)，可以执行shell命令，或者跟软件交互

+ spawn
+ exec
+ execFile
+ fork
+ execSync
+ execFileSync
+ spawnSync

### spawn
用于创建一个新的子进程来运行一个命令

它适用于需要与子进程交互的情况，例如从子进程获取输出或向其发送输入

spawn 是异步的，它会立即返回一个 ChildProcess 实例，而不会等待命令完成

接收三个参数

+ command：要执行的命令（字符串）
+ args：可选，命令行参数的数组
+ options：可选，配置对象，如 cwd（当前工作目录）、env（环境变量）、stdio（输入输出流）等

```javascript
const { spawn } = require('child_process');
const child = spawn(command, [args], options);

//实例
const { spawn } = require('child_process');
const ls = spawn('ls', ['-lh', '/usr'], { stdio: 'inherit' });

ls.on('close', (code) => {
  console.log(`child process exited with code ${code}`);
});
```

### exec
执行一个 shell 命令，并在命令完成时调用回调函数

参数：

+ command：要执行的命令（字符串）
+ options：可选，配置对象，如 cwd、env、timeout 等
+ callback：回调函数，接受三个参数：error、stdout 和 stderr
    - error 是失败的输出，stdout 使标准数据流，stderr 是失败的输出流

```javascript
const { exec } = require('child_process');
exec(command, [options], callback);

//实例
const { exec } = require('child_process');
exec('ls -lh /usr', (error, stdout, stderr) => {
  if (error) {
    console.error(`exec error: ${error}`);
    return;
  }
  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});
```

### execFile
执行一个可执行文件，并在命令完成时调用回调函数

参数：

+ file：要执行的可执行文件（字符串）
+ args：可选，命令行参数的数组
+ options：可选，配置对象
+ callback：回调函数，接受三个参数：error、stdout 和 stderr

```javascript
const { execFile } = require('child_process');
execFile(file, [args], [options], callback);


//实例
const { execFile } = require('child_process');
execFile('node', ['--version'], (error, stdout, stderr) => {
  if (error) {
    console.error(`execFile error: ${error}`);
    return;
  }
  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});
```

### fork
创建一个新的 Node.js 子进程并运行一个模块

参数：

+ modulePath：要执行的模块路径（字符串）
+ args：可选，传递给子进程的参数数组
+ options：可选，配置对象

回调函数：使用 message 事件来接收来自子进程的消息。

```javascript
const { fork } = require('child_process');
const child = fork(modulePath, [args], [options]);

const { fork } = require('child_process');
const child = fork('child.js');

child.on('message', (msg) => {
  console.log('parent got message:', msg);
});

child.send({ hello: 'child' });
```

### execSync
同步地执行一个命令，直到命令完成

参数：

+ command：要执行的命令（字符串）
+ options：可选，配置对象

返回值是 buffer

```javascript
const { execSync } = require('child_process');
const output = execSync(command, [options]);

const { execSync } = require('child_process');
const output = execSync('ls -lh /usr').toString();
console.log(output);
```

### execFileSync
同步地执行一个可执行文件

参数：

+ file：要执行的可执行文件（字符串）
+ args：可选，命令行参数的数组
+ options：可选，配置对象

返回值：buffer

```javascript
const { execFileSync } = require('child_process');
const output = execFileSync(file, [args], [options]);

const { execFileSync } = require('child_process');
const output = execFileSync('node', ['--version']).toString();
console.log(output);
```

### spawnSync
同步地创建一个新的子进程来执行一个命令

参数：

+ command：要执行的命令（字符串）
+ args：可选，命令行参数的数组
+ options：可选，配置对象

返回值：一个对象，包含 stdout、stderr 和 status 等属性

```javascript
const { spawnSync } = require('child_process');
const { stdout, stderr } = spawnSync(command, [args], [options]);

const { spawnSync } = require('child_process');
const { stdout } = spawnSync('ls', ['-lh', '/usr']);
console.log(stdout.toString());
```

# events 模块
在 Node.js 中，events 模块是一个允许不同部分的应用程序通过发射和监听事件来进行通信的核心模块。这个模块特别关键，因为它支持构建基于事件的架构，这对于处理异步操作尤其重要

events用的其实不多，但很多API的底层都有集成了这个events，所以才导致明面上用的不是很多，学习这部分的内容不在于追求使用，而是探索events的一个架构设计

## EventEmitter 类
events 模块的核心是 EventEmitter 类。这个类用于创建能够发射（emit）事件的对象，并且这些对象可以拥有多个监听（监听器是函数，当指定事件被触发时会被调用）

```javascript
//引入模块和创建实例
const EventEmitter = require('events');
const emitter = new EventEmitter();

//监听事件：使用 .on() 或 .addListener() 方法来添加事件监听器
emitter.on('event', function(message) {
  console.log(`Received message: ${message}`);
});

//发射事件：使用 .emit() 方法来触发事件。
//这个方法允许传递任意数量的参数给监听器函数
emitter.emit('event', 'Hello, world!');
```

### 事件模型
Nodejs事件模型采用了，发布订阅设计模式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730101994207-05310960-9462-4de4-ab64-1cf1020d8092.png)

在 Node.js 中，发布-订阅模式也是这样工作的：

+ 订阅者（Subscribers）：图中的“订阅 test1”和“订阅 test2”代表两个功能或组件，它们“订阅”或者说监听某些事件，等待事件被触发。在代码里，这是通过在 EventEmitter 实例上添加事件监听器实现的
+ 发布者（Publishers）：当特定事件发生时，如“事件中心”收到特定的信号，它就会“发布”事件。在 Node.js 中，这通常通过调用 emit 方法来实现
+ 事件中心（Event Center）：这是事件的调度中心，在 Node.js 中通常由 EventEmitter 类实例化的对象来充当。它负责接收订阅者的监听器，并在适当的时刻触发这些监听器

### API
+ emitter.on('data', callback)：为事件注册一个监听器，该监听器每次事件发生时都会被调用
    - on 默认情况下最多 10 个，可以调用 event.setMaxListeners(num) 传入新的上限数量
+ emitter.once('open', callback)：为事件注册一个一次性监听器，事件发生一次后监听器解除绑定
+ emitter.emit('data', 'This is a message')：触发事件，导致添加到该事件的所有监听器被调用
+ emitter.off('data', callback)：移除特定事件的一个监听器

# util 模块
util 是Node.js内部提供很多实用工具类型的API，能方便我们快速开发

由于API比较多，这里着重讲解一些常用的API

## util.promisify
util.promisify 将一个遵循常见 Node.js 回调风格的函数（即最后一个参数是回调函数 callback(err, value) 的函数）转换为返回 Promise 的函数，然后可以使用现代的异步编程特性，如 async/await，来处理异步操作

比如要获取 Node 的版本

```javascript
import { exec } from 'node:child_process'
exec('node -v', (err,stdout)=>{
   if(err){
      return err
   }
   console.log(stdout)
})
```

```javascript
import { exec } from 'node:child_process'
import util from 'node:util'

const execPromise = util.promisify(exec)

//如果返回多个参数，则res就是一个对象，就需要我们通过res.xxx获取对象里的内容
execPromise('node -v').then(res=>{
    console.log(res,'res')
}).catch(err=>{
    console.log(err,'err')
})
```

## util.callbackify
与 util.promisify 相反，util.callbackify 将一个返回 Promise 的函数转换为遵循 Node.js 回调风格的函数。这在需要将基于 Promise 的 API 集成到仅支持回调风格的旧代码库中时非常有用

```javascript
onst util = require('node:util')
//fn函数返回的结果是promise,我们要进行改写成回调风格
const fn = (type) =>{
  //自己设置条件,这个需求很宽松.比如在这里我就希望type为1的情况下才是我想要的,则成功,反之失败
  if(type == 1){
    return Promise.resolve('success')
  }else{
    return Promise.reject('err')
  }
}
//使用callbackify进行回调处理
const callback = util.callbackify(fn)

//Node.js 中，使用回调函数的约定是由 Node.js 的 API 设计哲学决定的.遵循了所谓的“错误优先回调”
//主要目的是确保在进行异步操作时，能够优先处理可能出现的错误
callback(1,(err,value)=> {
  console.log(err,value);
})
```

## util.format
util.format 是一个格式化字符串的函数，类似于 C 语言中的 printf 或 Python 中的字符串格式化。它可以将多个参数组合成一个字符串，支持多种占位符，如 %s（字符串）、%d（数字）等

+ %s - 字符串
+ %d - 整数（使用 %i 也可以得到相同结果）
+ %f - 浮点数
+ %j - JSON（如果转换失败，会替换为 '[Circular]'）
+ %o - 对象（带一些附加选项的详细信息）
+ %O - 对象（标准详细信息）
+ %% - 输出一个 % 符号

```javascript
const util = require('util');

// 基础字符串格式化
console.log(util.format('%s:%s', 'name', 'Alice')); 
// 输出: 'name:Alice'

// 混合不同类型
console.log(util.format('My name is %s and I am %d years old.', 'Bob', 25)); 
// 输出: 'My name is Bob and I am 25 years old.'

// JSON 对象
console.log(util.format('User details: %j', { name: 'Carol', age: 30 })); 
// 输出: 'User details: {"name":"Carol","age":30}'

// 多个对象
console.log(util.format('Data: %o', { foo: 'bar' })); 
// 输出对象的详细信息

// 浮点数
console.log(util.format('Value: %f', 3.14159265)); 
// 输出: 'Value: 3.14159265'

// 显示百分比
console.log(util.format('100%% Complete')); 
// 输出: '100% Complete'
```

# fs 模块
它提供了文件操作功能，允许你执行文件的读取、写入、删除、重命名、检查状态等操作，fs 模块包含了很多同步和异步的函数，以处理文件系统相关的任务

## 文件 API
### 读取文件
fs.readFile(path[, options], callback)：异步读取文件内容

fs.readFileSync(path[, options])：同步读取文件内容



options 配置项

+ encoding：string/null 格式，默认值 null，指定文件读取的编码，如 'utf8'。如果设为 null，文件内容将作为原始的缓冲区（Buffer）返回
+ flag：String 格式，默认值'r'，指定文件系统操作的标志。例如，'r' 代表读取文件，如果文件不存在则抛出异常![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730104678097-4b2ba85d-e1b5-4831-9299-f2efa5691145.png)

```javascript
const fs = require('fs');

//异步
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});

//同步
try {
  const data = fs.readFileSync('example.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error(err);
}
```

### fs 可读流
fs 模块提供了流（Stream）API，允许以流的方式读取或写入文件。这种方式对于处理大文件或连续的数据流非常有用，因为它不需要一次性将整个文件加载到内存中



**创建可读流**

使用 fs.createReadStream() 函数。这个函数返回一个可读流对象，你可以在这个对象上注册各种事件处理器，如 'data'、'end'、'error' 、'close'

```javascript
const fs = require('fs');
const readableStream = fs.createReadStream(path[, options]);

//实例
const fs = require('fs');
const path = 'example.txt';

const readableStream = fs.createReadStream(path, { encoding: 'utf8' });

readableStream.on('data', (chunk) => {
  console.log(`Received ${chunk.length} bytes of data.`);
  console.log(chunk);
});

readableStream.on('end', () => {
  console.log('There will be no more data.');
});

readableStream.on('error', (err) => {
  console.error('An error occurred:', err);
});
```

### fs 可写流
fs.createWriteStream 方法用于创建一个可写流（Writable Stream），这是处理文件写入操作的一种高效方式，特别适用于大量数据的写入



**创建可写流**

```javascript
const fs = require('node:fs');
let writeStream = fs.createWriteStream('index.txt');

//使用write()写入数据
let verse = [
    '待到秋来九月八',
    '我花开后百花杀',
    '冲天香阵透长安',
    '满城尽带黄金甲'
];

verse.forEach(item => {
    writeStream.write(item + '\n');
});

//结束写入
writeStream.end();

//监听事件
writeStream.on('finish', () => {
    console.log('写入完成');
});

//错误处理
writeStream.on('error', (error) => {
    console.error('写入失败:', error);
});
```

### 监听文件变化
使用 fs.watch 方法可以监听文件或文件夹的变化

+ 当文件或文件夹有变化时，回调函数会被调用。
+ eventType 可以是 'rename'(文件重命名) 或 'change'(文件内容更新)，并且如果操作系统支持，filename 参数会提供发生变化的文件的名称
+ 需要注意的是，fs.watch 的行为非常依赖于操作系统，而且在某些情况下可能不稳定（例如一些系统可能在文件发生变化时不提供 filename）
+ 对于需要更稳定和一致性的文件监视，建议使用 chokidar 这样的库，它提供了对 fs.watch 和 fs.watchFile 的封装，解决了很多原生 fs 监听功能的问题

```javascript
const fs = require('node:fs')
// filename 是变化的文件的名称
//event返回参数,判断是修改了内容还是修改了文件名
fs.watch('./xiaoyu01.txt', (eventType, filename) => {
  if (eventType === 'change') {
    console.log(`文件发生变化，发生变化的文件名是：${filename}`)
  }
  if (eventType === 'rename') {
    console.log('文件名发生变化')
  }
})
```

### 写入文件
fs.writeFile(file, data, [options], callback)：异步写入文件

fs.writeFileSync(file, data, [options])：同步写入文件

```javascript
const fs = require('fs');

//异步
fs.writeFile('example.txt', 'Hello World', 'utf8', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('File written successfully');
});

//同步
try {
  fs.writeFileSync('example.txt', 'Hello World', 'utf8');
  console.log('File written successfully');
} catch (err) {
  console.error(err);
}
```

### 追加文件
fs.appendFile(file, data, [options], callback)：异步追加内容到文件

fs.appendFileSync(file, data, [options])：同步追加内容到文件

```javascript
const fs = require('fs');

//异步
fs.appendFile('example.txt', '\nAdditional content', 'utf8', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('Content appended successfully');
});

//同步
try {
  fs.appendFileSync('example.txt', '\nAdditional content', 'utf8');
  console.log('Content appended successfully');
} catch (err) {
  console.error(err);
}
```

### 删除文件
fs.unlink(path, callback)：异步删除文件

fs.unlinkSync(path)：同步删除文件

```javascript
const fs = require('fs');

//异步
fs.unlink('example.txt', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('File deleted successfully');
});

//同步
try {
  fs.unlinkSync('example.txt');
  console.log('File deleted successfully');
} catch (err) {
  console.error(err);
}
```

### 重命名文件
fs.rename(oldPath, newPath, callback)：异步重命名文件

fs.renameSync(oldPath, newPath)：同步重命名文件

```javascript
const fs = require('fs');

//异步
fs.rename('oldName.txt', 'newName.txt', (err) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('File renamed successfully');
});

//同步
try {
  fs.renameSync('oldName.txt', 'newName.txt');
  console.log('File renamed successfully');
} catch (err) {
  console.error(err);
}
```

### 检查文件状态
fs.stat(path, callback)：异步获取文件状态

fs.statSync(path)：同步获取文件状态

```javascript
const fs = require('fs');

//异步
fs.stat('example.txt', (err, stats) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(`File size: ${stats.size}`);
});

//同步
try {
  const stats = fs.statSync('example.txt');
  console.log(`File size: ${stats.size}`);
} catch (err) {
  console.error(err);
}
```

## 文件夹 API
### 创建文件夹目录
使用 fs.mkdir 或 fs.mkdirSync 创建新的文件夹

+ 如果开启 recursive 可以递归创建多个文件夹
+ mkdir 是 "make directory" 的缩写，其中 "mk" 代表 "make"（创建），"dir" 代表 "directory"（目录）
+ 一般使用Sync同步的方式去创建，因为创建文件夹很快，基本上不存在堵塞的问题

```javascript
const fs = require('fs');

// 异步方式
fs.mkdir('./xiaoyu', { recursive: true }, (err) => {
  if (err) throw err;
  console.log('Folder created!');
});

// 同步方式
try {
  //开启 recursive ,路径从当下开始递归创建多个文件夹
  //分别是xiaoyu文件夹以及之内的xiaoman,xiaoman之内的test,嵌套了三层文件夹
  fs.mkdirSync('/xiaoyu/xiaoman/test', { recursive: true });
  console.log('Folder created!');
} catch (err) {
  console.error(err);
}
```

### 删除文件夹目录
使用 fs.rmdir 或 fs.rmdirSync 删除文件夹。

+ 在删除文件夹之前，该文件夹必须是空的
+ 如果开启recursive 递归删除全部文件夹
+ rmdir 是 "remove directory" 的缩写，其中 "rm" 代表 "remove"（删除），"dir" 代表 "directory"（目录）

```javascript
// 异步方式
fs.rmdir('./xiaoyu', (err) => {
  if (err) throw err;
  console.log('Folder deleted!');
});

// 同步方式
try {
  fs.rmdirSync('/xiaoyu/xiaoman/test');
  console.log('Folder deleted!');
} catch (err) {
  console.error(err);
}
```

### 重命名文件夹目录
使用 fs.rename 或 fs.renameSync 来重命名或移动文件夹

```javascript
// 异步方式
fs.rename('./xiaoyur', './index', (err) => {
  if (err) throw err;
  console.log('Folder renamed!');
});

// 同步方式
try {
  fs.renameSync('./xiaoyur', './index');
  console.log('Folder renamed!');
} catch (err) {
  console.error(err);
}
```

## 软链接和硬链接
### 硬链接
硬链接是指向相同文件inode（索引节点）的另一个文件名

+ 共享存储：
    - 硬链接允许多个文件名指向同一个文件，这样可以在不同的位置使用不同的文件名引用相同的内容。
    - 硬链接指向的文件共享相同的inode和数据块，因此它们实际上是同一个文件，对任何一个链接的更改都会影响文件内容
+ 文件备份：
    - 通过创建硬链接，可以在不复制文件的情况下创建文件的备份，如果原始文件发生更改，备份文件也会自动更新
    - 它们不需要复制文件内容，只是增加了一个指向相同数据的新入口
    - 硬链接不会因为源文件的删除而变成悬空链接，硬链接只有在最后一个链接被删除后，文件数据才会从磁盘上删除
+ 文件重命名：
    - 通过创建硬链接，可以为文件创建一个新的文件名，而无需复制或移动文件，这对于需要更改文件名但保持相同内容和属性的场景非常有用

```javascript
const fs = require('fs');
fs.link('originalFile.txt', 'hardLink.txt', (err) => {
  if (err) throw err;
  console.log('Hard link created!');
});
```

### 软链接
软链接是一种特殊类型的文件，它包含了另一个文件的路径

+ 独立存储：软链接文件本身有自己的inode和一个小的存储空间来保存目标文件的路径
+ 删除行为：软链接删除不会影响目标文件，即使目标文件被删除，软链接仍然存在，直到被显式删除或其所在的文件系统被卸载
+ 路径依赖：软链接包含目标的路径，如果目标文件移动或重命名，软链接将不再有效（称为“悬挂的链接”）

```javascript
//软链接需要管理员权限
const fs = require('fs');
fs.symlink('originalFile.txt', 'symlink.txt', (err) => {
  if (err) throw err;
  console.log('Symbolic link created!');
});
```

### 使用场景
+ 硬链接：适用于需要确保即使文件被删除，内容仍然保留的场景。它们经常被用于备份和恢复系统中
+ 软链接：适用于需要创建跨文件系统链接或链接到目录的场景。它们在动态链接库、应用程序快捷方式创建等场景中非常有用

# crypto 模块
密码学是计算机科学的一个关键领域，涵盖了加密、解密、哈希函数和数字签名等技术。Node.js提供了一个称为crypto的强大模块，使开发人员可以轻松地在其应用程序中实施这些密码学功能

crypto模块的目的是为了提供通用的加密和哈希算法。尽管用纯JavaScript实现这些算法是可行的，但执行速度通常很慢。因此，Node.js通过C/C++实现了这些算法，并通过crypto模块将其作为JavaScript接口提供，这不仅方便了使用，也大大提升了运行效率

## 对称加密
 对称加密是一种简单而快速的加密方式，它使用相同的密钥（称为对称密钥）来进行加密和解密。这意味着发送者和接收者在加密和解密过程中都使用相同的密钥。对称加密算法的加密速度很快，适合对大量数据进行加密和解密操作。然而，对称密钥的安全性是一个挑战，因为需要确保发送者和接收者都安全地共享密钥，否则有风险被未授权的人获取密钥并解密数据

### 方法
1. crypto.createCipheriv(algorithm, key, iv)：这个函数用于创建一个加密算法的实例
    - algorithm：加密算法，这里使用的是"AES-256-CBC"
    - key：用于加密的密钥，32 位
    - iv：初始化向量，用于算法的起始块加密，这个iv是可以随便取名的



2. cipher.update(data, inputEncoding, outputEncoding)：这个方法用于加密数据。它可以多次调用，用于加密长数据流。
    - data：要加密的数据
    - inputEncoding：输入数据的编码格式，这里为"utf-8"
    - outputEncoding：输出数据的编码格式，这里为"hex"



3. cipher.final(outputEncoding)：这个方法用于结束加密过程，输出最后的加密数据。它返回的数据需要与update方法的输出连接



4. crypto.createDecipheriv(algorithm, key, iv, options)：用于创建一个解密对象，这个对象使用指定的加密算法、密钥（key）和初始化向量（iv），返回一个 Decipher 对象，该对象可以用于解密数据



```javascript
 const crypto = require('node:crypto');
 
 // 生成一个随机的 16 字节的初始化向量 (IV)
 const iv = Buffer.from(crypto.randomBytes(16));
 
 // 生成一个随机的 32 字节的密钥
 const key = crypto.randomBytes(32);
 
 // 创建加密实例，使用 AES-256-CBC 算法，提供密钥和初始化向量
 const cipher = crypto.createCipheriv("aes-256-cbc", key, iv);
 
 // 对输入数据进行加密，并输出加密结果的十六进制表示(hex)
 cipher.update("小余", "utf-8", "hex");
 const result = cipher.final("hex");
 
 // 解密,de是解密的工具，包含了三位要素。然后把需要解密的内容丢进去就得到结果了
 const de = crypto.createDecipheriv("aes-256-cbc", key, iv);

 de.update(result, "hex");
 const decrypted = de.final("utf-8");
 
 console.log("Decrypted:", decrypted);
```

## 非对称加密
非对称加密使用一对密钥，分别是公钥和私钥。发送者使用接收者的公钥进行加密，而接收者使用自己的私钥进行解密。

公钥可以自由分享给任何人，而私钥必须保密。非对称加密算法提供了更高的安全性，因为即使公钥泄露，只有持有私钥的接收者才能解密数据。

然而，非对称加密算法的加密速度相对较慢，不适合加密大量数据。因此，在实际应用中，通常使用非对称加密来交换对称密钥，然后使用对称加密算法来加密实际的数据

### 方法
generateKeyPairSync：这个方法用于同步生成公钥和私钥对

+ algorithm：指定加密算法，例如'rsa'
+ options：一个对象，包含密钥对生成的选项
    - modulusLength：密钥的长度
    - publicKeyEncoding：公钥的编码方式，包含type和format两个属性
    - privateKeyEncoding：私钥的编码方式，同样包含type和format两个属性
+ 返回值：一个对象，包含publicKey和privateKey两个属性，分别代表公钥和私钥



publicEncrypt：这个方法用于使用公钥加密数据

+ publicKey：公钥对象，可以是Buffer、TypedArray或DataView
+ data：要加密的数据，通常是一个Buffer对象
+ 返回值：返回加密后的数据，通常是一个Buffer对象



privateDecrypt：这个方法用于使用私钥解密数据

+ privateKey：私钥对象，可以是Buffer、TypedArray或DataView
+ buffer：要解密的数据，通常是一个Buffer对象
+ 返回值：返回解密后的数据，通常是一个Buffer对象

```javascript
const { generateKeyPairSync, publicEncrypt, privateDecrypt } = require('crypto');

// 生成密钥对
const { publicKey, privateKey } = generateKeyPairSync('rsa', {
  modulusLength: 2048, // 密钥长度
  publicKeyEncoding: {
    type: 'pkcs1', // 公钥格式
    format: 'pem' // 编码格式
  },
  privateKeyEncoding: {
    type: 'pkcs1', // 私钥格式
    format: 'pem' // 编码格式
  }
});

// 要加密的数据
const data = 'Hello, this is a secret message!';

// 使用公钥加密数据
const encrypted = publicEncrypt(publicKey, Buffer.from(data));

// 使用私钥解密数据
const decrypted = privateDecrypt(privateKey, encrypted);

// 输出结果
console.log('Encrypted:', encrypted.toString('hex')); // 加密后的数据
console.log('Decrypted:', decrypted.toString()); // 解密后的数据
```

## 哈希函数
哈希函数，用通俗的语言来说，就像是一个特殊的“压缩机”。这个“压缩机”接收任何类型的输入（比如文件、文本、数字等），然后通过一系列复杂的计算，输出一个固定长度的字符串，称之为“哈希值”或者“哈希码”



**特点**

+ 唯一性：每个不同的输入通过哈希函数处理后，都会得到一个独一无二的哈希值
+ 确定性：对于同一个输入，无论何时何地使用同一个哈希函数，生成的哈希值总是相同的。
+ 不可逆性：哈希函数是单向的，也就是说，你不能从哈希值反推出原始的输入是什么，这就像不能从一个人的指纹反推出这个人的长相
+ 敏感性：输入的微小变化会导致哈希值的巨大变化。这被称为“雪崩效应”
+ 快速计算：哈希函数计算速度很快，即使是很大的数据，也能迅速生成哈希值
+ 碰撞：同的输入有可能产生相同的哈希值，这种情况称为“哈希碰撞”。但是，好的哈希函数会尽可能减少这种碰撞发生的概率



**哈希函数**

+ MD5、SHA-1、SHA-256等
+ 随着技术的发展，一些旧的哈希函数（如MD5和SHA-1）因为安全性问题不再推荐使用，而更安全的哈希函数（如SHA-256和SHA-3）被广泛采用

# zlib 模块
zlib 模块允许我们，通过Gzip、Deflate/Inflate 等数据压缩算法，压缩和解压数据，这个模块对于处理大量数据非常重要，尤其是在需要优化网络传输效率和减少数据存储空间时

## 方法
压缩和解压：

+ zlib.createGzip()：创建一个Gzip压缩流，用于压缩数据
+ zlib.createGunzip()：创建一个Gzip解压流，用于解压数据
+ zlib.createDeflate()：创建一个Deflate压缩流
+ zlib.createInflate()：创建一个Inflate解压流



同步和异步操作：

+ zlib.gzip(), zlib.gunzip()：异步压缩/解压数据。
+ zlib.gzipSync(), zlib.gunzipSync()：同步压缩/解压数据，当在处理非常大的数据集时，异步方法更适合使用，以避免阻塞事件循环。



实用函数：zlib.deflateRaw()、zlib.inflateRaw()等：提供更多底层的压缩解压功能，允许更精细控制压缩解压过程

## 压缩方式
### Gzip
#### 概念
+ Gzip（GNU zip）是一种基于 DEFLATE 压缩算法的文件格式，广泛用于 Unix 和类 Unix 系统
+ Gzip 压缩文件时，会在文件头部添加一些元数据，如文件的原始大小、时间戳等
+ Gzip 格式的文件通常以 .gz 扩展名结尾
+ 适合于单文件压缩，常用于备份和归档
+ 常用于网络传输，比如 HTTP 协议中的 Content-Encoding: gzip 就是指使用 gzip 压缩的内容



#### 实例
```javascript
//引入模块
const zlib = require("node:zlib")
const fs = require("node:fs")

//创建可读流
const readStream = fs.createReadStream('天才俱乐部.txt')

//创建可写流
const writeStream = fs.createWriteStream('天才俱乐部.txt.gz')

//.pipe() 方法用于将一个流（stream）的输出连接到另一个流的输入
readStream.pipe(zlib.createGzip()).pipe(writeStream)
```

readStream被连接到zlib.createGzip()创建的压缩流，然后进项压缩，压缩后的数据流被连接到writeStream，这意味着压缩后的数据会被写入到天才俱乐部.txt.gz文件中

### Deflate/Inflate
#### 概念
+ Deflate 是一种压缩算法，它结合了 LZ77 算法和哈夫曼编码
+ Inflate 是 Deflate 的逆过程，用于解压缩
+ Deflate 压缩的数据不包含任何文件元数据，只包含压缩后的数据本身
    - 因为不包含文件元数据，所以 Deflate 压缩的数据需要额外的信息来正确解压缩
+ 适合于数据流的压缩和解压缩，常用于网络传输和文件格式

#### 实例
```javascript
// 创建可读流，读取名为 index.txt 的文件
const readStream = fs.createReadStream('我的诡异人生模拟器.txt'); 

// 创建可写流，将压缩后的数据写入 index.txt.deflate 文件
const writeStream = fs.createWriteStream('我的诡异人生模拟器.txt.deflate');

//压缩
readStream.pipe(zlib.createDeflate()).pipe(writeStream)
```

```javascript
const readStream = fs.createReadStream('我的诡异人生模拟器.txt.deflate')
const writeStream = fs.createWriteStream('我的诡异人生模拟器-xiaoyu.txt')
//解压
readStream.pipe(zlib.createInflate()).pipe(writeStream)
```

### 方法选择
使用Gzip：

+ 压缩文件需要长期存储
+ 需要在客户端完整恢复文件的原始信息
+ 是压缩效率而不是速度
+ 压缩的文件较大（如日志文件、备份等）

使用Deflate：

+ 快速压缩和解压的场合
+ 网络数据传输，如Web页面、API通信等

# http 模块
用于构建HTTP服务器和客户端应用程序。它提供了一系列的功能，使得开发者可以轻松地处理HTTP请求和响应，而无需额外的库

## 方法
### 服务器
+ http.createServer(fn)：创建一个新的 HTTP 服务器实例，接受一个请求处理函数作为参数
    - 回调函数有两个参数 req 和 res
    - 请求对象（req）：包含了请求的所有信息，如 HTTP 方法、URL、请求头等
    - 响应对象（res）：允许发送响应头和响应体给客户端
+ server.listen(端口号，fn)：使服务器开始监听指定端口的请求，可以指定端口号、 hostname 和回调函数
+ server.close()：停止服务器监听，服务器将不再接收新的请求
+ server.address()：返回服务器的地址，如果是 TCP 服务器则返回 port 和 address
+ server.maxConnections：设置或获取服务器的最大连接数
+ server.ref(), server.unref()：控制服务器的引用计数，影响程序的退出行为

### **发送请求**
http.request(options,callback)：发送请求，返回一个可写流，可以用 On 监听事件



options 包含内容如下

+ hostname 或 host：请求的服务器域名或IP地址
+ port：服务器端口号
+ path：请求的路径
+ method：请求方法（如 'GET', 'POST', 'PUT', 'DELETE' 等）
+ headers：一个对象，包含了请求头的键值对

```javascript
const http = require('http');

// 请求的选项
const options = {
  hostname: 'example.com',
  port: 80,
  path: '/path/to/resource',
  method: 'GET'
};

// 发起请求
const req = http.request(options, (res) => {
  console.log(`状态码：${res.statusCode}`);
  console.log(`响应头：${JSON.stringify(res.headers)}`);

  // 处理响应体数据
  let data = '';
  res.on('data', (chunk) => {
    data += chunk;
  });
  res.on('end', () => {
    console.log('响应结束');
    console.log(data);
  });
});

req.on('error', (e) => {
  console.error(`请求遇到问题: ${e.message}`);
});

// 发送请求
req.end();
```



http.get(options[,callback])： http.request() 的简化版本，只用于 GET 请求,options 可以只写一个网址



client.request()（对于 HTTP 客户端请求）：在创建的 HTTP 客户端上发送请求

### **请求**
+ req.url：请求的 URL
+ req.method：请求的方法，如 GET 或 POST
+ req.headers：请求头对象
+ req.statusCode：响应的状态码（只读）
+ req.statusMessage：响应的状态消息（只读）
+ req.on：给请求对象绑定事件监听器，这些事件在请求的生命周期中不同的时间点被触发
    - data 事件：当请求的一部分数据被接收时触发
    - end 事件：当请求的所有数据都被接收完毕时触发
    - close 事件：当底层连接被关闭时触发，无论请求是否已完成
    - error 事件：如果请求过程中发生错误，这个事件会被触发
    - aborted 事件：如果客户端中止了请求（例如，客户端关闭了连接），这个事件会被触发

```javascript
req.on('data', (chunk) => {
  console.log(chunk);
});

req.on('end', () => {
  console.log('Request finished');
});

req.on('close', () => {
  console.log('Connection closed');
});

req.on('error', (err) => {
  console.error('Request error:', err);
});

req.on('aborted', () => {
  console.log('Request aborted');
});
```

### **响应**
res.writeHead()：

+ 发送一个 HTTP 响应头，可以指定状态码和响应头
+ 必须在调用 res.write() 或 res.end() 发送响应体之前调用，因为它标志着响应的开始
+ 参数
    - statusCode：一个数字，表示 HTTP 状态码（例如 200、404、500 等）
    - reasonPhrase：一个字符串，表示状态码的简短描述（例如 "OK"、"Not Found" 等）
    - headers：一个对象，表示响应头的键值对



res.write()：

+ 发送响应体的一部分数据给客户端。可以多次调用 res.write() 来发送多个数据片段
+ 参数：
    - chunk：要发送的数据片段，可以是字符串或 Buffer
    - encoding：数据的编码（如果 chunk 是字符串），默认为 'utf8'



res.end()：

+ 这个方法用于发送响应体的最后部分，并结束响应
+ 在调用 res.end() 后，不能再向客户端发送任何数据
+ 参数：
    - data：可选的，要发送的最后数据片段
    - encoding：如果 data 是字符串，这个参数指定其编码



res.setTimeout()：设置响应的超时时间

## 实例
用Nodejs 写一个get接口，允许前端通过get请求获取数据

```javascript
// 引入http模块
const http = require('http'); 

// 创建一个HTTP服务器
const server = http.createServer((req, res) => {
  // 检查请求是否为GET方法
  if (req.method === 'GET') {
    // 设置响应头，告诉客户端我们将发送的是JSON格式的数据
    res.writeHead(200, { 'Content-Type': 'application/json' });

    // 构造要返回的数据对象
    const data = {
      name: 'John Doe',
      age: 30,
      city: 'New York'
    };

    // 将数据对象转换为JSON字符串并发送
    res.end(JSON.stringify(data));
  } else {
    // 如果不是GET请求，返回405方法不允许
    res.writeHead(405, { 'Content-Type': 'text/plain' });
    res.end('Method Not Allowed');
  }
});

// 服务器监听3000端口
server.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

```javascript
const http = require('http'); // 引入http模块

// 创建一个HTTP服务器
const server = http.createServer((req, res) => {
  // 检查请求是否为POST方法
  if (req.method === 'POST') {
    // 初始化一个变量来存储请求体的数据
    let body = '';
    // 监听数据事件，每当接收到请求体的数据时都会被调用
    req.on('data', chunk => {
      body += chunk.toString(); // 将数据转换为字符串并累加
    });
    // 监听结束事件，当请求体的所有数据都被接收完毕时触发
    req.on('end', () => {
      // 发送响应头，告诉客户端状态码和内容类型
      res.writeHead(200, { 'Content-Type': 'application/json' });
      // 处理请求体数据（在这里我们只是将其作为响应返回）
      const response = {
        message: 'POST request received',
        yourData: body
      };
      // 发送响应体并结束响应
      res.end(JSON.stringify(response));
    });
  } else {
    // 如果不是POST请求，返回405方法不允许
    res.writeHead(405, { 'Content-Type': 'text/plain' });
    res.end('Method Not Allowed');
  }
});

// 服务器监听3000端口
server.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```

# url 模块
提供了一些用于处理 URL 对象和字符串的工具。这个模块可以帮助你解析 URL，重新构建 URL，以及简化 URL 的操作

## 方法
### url.parse()
用于将一个 URL 字符串解析成一个对象，其中包含 URL 的各个组成部分，下面是三个参数

+ urlStr：要解析的 URL 字符串
+ parseQueryString：一个布尔值，指定是否将查询字符串解析为对象。
+ slashesDenoteHost：一个布尔值，指定在没有明确主机名的情况下，URL 字符串中的前导斜杠是否表示主机名的开始



返回一个对象，包含以下属性：

+ href：完整的 URL 字符串
+ protocol：URL 的协议部分，如 http: 或 https:
+ slashes：布尔值，指示协议后是否有斜杠
+ auth：认证信息，如 user:pass
+ hostname：URL 的主机名部分
+ port：URL 的端口部分
+ host：主机名和端口的组合
+ pathname：URL 的路径部分
+ search：URL 的查询字符串部分
+ query：如果 parseQueryString 为 true，这个属性将包含一个表示查询字符串的对象；否则，它将与 search 相同
+ hash：URL 的片段标识符（hash）部分

```javascript
const parsedUrlWithQuery = url.parse(myUrl, true);
console.log(parsedUrlWithQuery.query); // 输出: { name: 'ferret' }
```

### url.format()
用于将一个 URL 对象转换回 URL 字符串，接受一个对象作为参数，返回一个字符串

```javascript
const urlObj = {
  protocol: 'https:',
  hostname: 'example.com',
  pathname: '/path',
  query: { name: 'example', value: 'test' }
};

const urlString = url.format(urlObj);
console.log(urlString); 
// 输出: https://example.com/path?name=example&value=test
```

### url.reslove()
用于解析一个相对 URL 相对于一个基础 URL，并返回一个绝对 URL

接受两个参数：基础 URL 和相对 URL

返回一个字符串，表示解析后的绝对 URL

```javascript
const baseUrl = 'https://example.com/path/';
const relativeUrl = './subpath?test=true';

const resolvedUrl = url.resolve(baseUrl, relativeUrl);
console.log(resolvedUrl); 
// 输出: https://example.com/path/subpath?test=true
```

### new URL()
这是一个构造函数，用于创建一个新的 URL 对象，接受下面两个参数

+ input：要解析的 URL 字符串
+ base：一个可选的基础 URL 字符串，如果 input 是相对 URL，则使用这个基础 URL 进行解析

返回一个 URL 对象，包含 URL 的所有组成部分，如 href、origin、protocol、hostname、pathname、search、hash 等

```javascript
const myUrl = new URL('https://example.com/path?query=123#fragment');
console.log(myUrl.href); 
// 输出: https://example.com/path?query=123#fragment
```

# express 框架
## 概念
是一个灵活的 Node.js Web 应用框架，提供了一套丰富的功能，用于构建单页、多页以及混合网页应用。它被设计为使服务器端开发变得快捷而简单，是最流行的 Node.js 框架之一，形成了庞大的生态系统

****

**特点**

+ 路由：Express 应用围绕路由的概念构建，路由定义了不同的 URL 路径和相应的处理函数，决定了用户访问这些路径时的行为
+ 中间件：Express 支持中间件，中间件是具有请求对象（req）、响应对象（res）和 next 函数（next）的函数，可执行请求的前置和后续处理
+ 模板引擎：支持模板引擎，如 Pug、EJS 等，用于渲染 HTML 页面
+ 静资产服务：可以轻松地为图片、CSS、JavaScript 等静态文件提供服务
+ 强大的 API：提供了一个强大的 API，用于构建 RESTful API
+ 错误处理：支持错误处理中间件，使得错误处理变得更加集中和一致
+ 灵活的视图渲染：可以轻松地渲染视图模板，支持多种模板引擎

## 使用
### 文件目录
1. middleware文件夹：用来写中间件的
2. src文件夹：主要编写代码就在这里，比如说用户接口的模块，用户登录模块，我们就拆成一个个模块化，结构更清晰
3. app.js文件：主文件，也相当于入口文件，可以简单的理解为像vite或者webpack起一个项目中的index.html
4. express.http文件：在这里发送请求

### 方法介绍 req & res
#### req
+ req.params：包含路由参数
+ req.query：包含 URL 查询参数
+ req.body：包含请求体的数据（通常用于 POST 和 PUT 请求）
+ req.headers：包含 HTTP 请求头
+ req.method：包含请求的 HTTP 方法
+ req.url：包含请求的 URL
+ req.path：与 req.url 相同，包含请求的路径
+ req.hostname：包含请求的主机名
+ req.path：包含请求的路径

```javascript
//req.params
app.get('/user/:userId', (req, res) => {
  const userId = req.params.userId;
  res.send(`User ID is: ${userId}`);
});

//req.query , /search?name=jack
app.get('/search', (req, res) => {
  const name = req.query.name;
  res.send(`my name is: ${name}`);
});

//req.body
app.post('/submit', (req, res) => {
  const data = req.body;
  res.send(`Received data: ${JSON.stringify(data)}`);
});

//req.headers
app.get('/headers', (req, res) => {
  const user_agent = req.headers['user-agent'];
  res.send(`User-Agent is: ${user_agent}`);
});
```

#### res
+ res.send()：发送各种类型的响应主体，这个方法会自动根据发送的数据类型设置 Content-Type 头，可以发送字符串， JSON 对象，状态码
+ res.json()：发送一个 JSON 响应
+ res.status()：设置 HTTP 状态码
    - `res.status(404).send('Not Found')`设置状态码为 404 并发送消息。
+ res.set()：设置响应头
    - `res.set('Content-Type', 'application/json')`
+ res.get()：获取一个响应头字段的值
    - `const contentType = res.get('Content-Type')`
+ res.cookie()：设置一个 cookie。
    - `res.cookie('name', 'value', { maxAge: 900000, httpOnly: true })`
+ res.redirect()：重定向到指定的 URL
    - `res.redirect('/home')`
+ res.render()：渲染一个视图模板。
    - `res.render('index', { title: 'Home' })`
+ res.sendFile()：发送一个文件作为响应
    - `res.sendFile('/path/to/file.html')`

### 基础使用
1. 安装：`npm i express`，`npm init`
2. 创建 express 应用

```javascript
// 引入express模块
const express = require('express');

// 创建一个express应用
const app = express();

// 定义端口号
const port = 3000;

// 定义一个路由来响应GET请求
app.get('/', (req, res) => {
  // 使用res.send()发送响应
  res.send('Hello World!');
});

// 使应用监听定义的端口
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

3. 运行：`node app.js`

## 路由
通过请求路径和请求方法进行路径分发处理，由请求方法、路径、回调函数组成

常见的请求方式有四种：get、post、delete、put、all

```javascript
//创建 get 路由
app.get('/home', (req, res) => {
res.send('网站首页');
});

//首页路由
app.get('/', (req,res) => {
res.send('我才是真正的首页');
});

//创建 post 路由
app.post('/login', (req, res) => {
res.send('登录成功');
});

//匹配所有的请求方法
app.all('/search', (req, res) => {
res.send('1 秒钟为您找到相关结果约 100,000,000 个');
});

//自定义 404 路由
app.all("*", (req, res) => {
res.send('<h1>404 Not Found</h1>')
});
```

## 中间件
### 基本概念
中间件是一个函数，它处理 HTTP 请求和响应对象，中间件可以将控制权传递给下一个中间件函数。如果中间件不调用next()方法，请求将被中止，不会继续传递给下一个中间件或路由处理函数

三个参数

+ req
+ res
+ next()



app.use() 用于在 Express 应用中注册中间件函数，它告诉 Express 在处理请求时应该按什么顺序使用这些中间件

+ 无参数：它将中间件应用于所有的请求，无论其路径或 HTTP 方法是什么
+ 一个参数：路径作为参数时，中间件将只应用于匹配该路径的请求
+ 两个参数：当提供一个 HTTP 方法和一个路径作为参数时，中间件将只应用于匹配该方法和路径的请求

### 全局中间件
```javascript
//具名函数
let recordMiddleware = function(request,response,next){
  //实现功能代码
  next();
}

app.use(recordMiddleware);

//匿名函数
app.use(function (request, response, next) {
  //实现功能代码
  next();
})

//箭头函数
app.use((request, response, next) => {
  //实现功能代码
  next();
})
```

### 路由中间件
每当有请求到达时，路由中间件都会被调用

```javascript
const express = require('express');
const app = express();
const port = 3000;

// 定义一个路由中间件
const logRequest = (req, res, next) => {
  console.log(`Request Method: ${req.method}, Request URL: ${req.url}`);
  next(); 
};

// 使用路由中间件
//每当有请求到达时，logRequest 中间件都会被调用。
app.use(logRequest);

// 定义路由
app.get('/', (req, res) => {
  res.send('Welcome to the Home Page!');
});

app.get('/about', (req, res) => {
  res.send('This is the About Page!');
});

app.get('/user/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`User ID is: ${userId}`);
});

// 启动服务器
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

### 静态资源中间件
是指用于提供静态文件（如图片、CSS 文件、JavaScript 文件等）的中间件

这些文件通常不需要服务器进行任何处理，可以直接发送给客户端

Express 提供了一个内置的 express.static 中间件函数，用于简化静态资源的托管

```javascript
const express = require('express');
const app = express();

// 托管 public 目录下的静态资源
app.use(express.static('public'));

// 启动服务器
const port = 3000;
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});

//public的目录结构
/*my-express-app
  node_modules
  public
    -css
      --styles.css
    -js
      --app.js
    -images
      --logo.png
  app.js*/

//然后就可以访问以下 URL 来获取静态资源
//http://localhost:3000/css/styles.css 访问 CSS 文件
//http://localhost:3000/js/app.js 访问 JavaScript 文件
//http://localhost:3000/images/logo.png 访问图片文件
```



# 网络请求
## 请求头和响应头
请求头是HTTP请求的一部分，包含了对服务器有用的信息，例如客户端想要进行的操作或者客户端的环境信息。它们帮助服务器了解如何处理接收到的请求

常见的请求头包括：

+ Host：指定服务器的域名
+ User-Agent：标识发出请求的客户端类型，比如浏览器类型和版本
+ Accept：客户端能够接收的内容类型，如text/html或application/json
+ Authorization：包含认证信息，用于访问需要验证的资源
+ Content-Type：请求体的媒体类型（如application/json），在POST和PUT请求中常见



响应头是服务器响应的一部分，向客户端回报关于响应的信息，如内容类型和状态信息。它们帮助客户端理解服务器返回的数据及如何处理这些数据

常见的响应头包括：

+ Content-Type：响应体的内容类型，告诉客户端实际返回的数据类型
+ Content-Length：响应体的长度
+ Set-Cookie：向客户端发送的Cookie
+ Cache-Control：缓存策略，指示客户端如何缓存接收到的内容
+ Status：HTTP状态码，如200（成功）、404（未找到）或500（服务器错误）

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730192162757-59bad50c-8487-4899-b18e-40452ec6c42f.png)

## 跨域
### 概念
跨域是指在Web开发中，从一个源（origin）的页面去请求另一个源的资源。它涉及到Web应用从一个域名（源）访问另一个域名的资源的权限问题

源由三个部分组成，只要这三者中有一个不同，就构成了跨域

+ 协议（http、https）
+ 主机（IP或域名）
+ 端口号（如80, 443，3000等等）

### 解决方案
#### CORS
CORS（Cross-Origin Resource Sharing，跨域资源共享）是 W3C 标准，允许服务器声明哪些外域可以访问其资源。在 Node.js 中，可以使用 cors 这个 npm 包来轻松实现 CORS

```javascript
npm install cors

const express = require('express');
const cors = require('cors');
const app = express();

// 允许所有跨域请求
app.use(cors());

// 或者配置特定的 CORS 选项
const corsOptions = {
  // 仅允许来自 http://example.com 的请求
  origin: 'http://example.com', 

  // 允许发送凭证
  credentials: true, 

  // 允许的 HTTP 方法
  methods: ['GET', 'POST', 'PUT', 'DELETE'], 

  // 允许的请求头
  allowedHeaders: ['Content-Type', 'Authorization'] 
};
app.use(cors(corsOptions));
```

#### 设置响应头
直接在响应头中设置 CORS 相关的头信息

```javascript
app.all('*', (req, res, next) => {
  // 允许所有域的请求
  res.header("Access-Control-Allow-Origin", "*"); 
  
  // 设置允许的请求头
  res.header("Access-Control-Allow-Headers", "Content-Type,Authorization"); 

  // 设置允许的请求方法
  res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS"); 
  next();
});
```

#### JSONP
JSONP（JSON with Padding）是一种跨域请求解决方案，它通过动态生成 <script> 标签并利用其不受同源策略限制的特性来进行跨域请求

JSONP 只支持 GET 请求

```javascript
//客户端
<script>
  function handleJsonp(data) {
    console.log(data);
  }
</script>
<script src="http://example.com/api/jsonp?callback=handleJsonp"></script>

//服务端
app.get('/api/jsonp', (req, res) => {
  const callback = req.query.callback;
  const data = { message: 'This is a JSONP response' };
  res.send(`${callback}(${JSON.stringify(data)})`);
});
```

#### 代理服务器
代理服务器可以在前端和目标服务器之间充当中间人的角色，将请求转发给目标服务器，从而解决跨域问题

在 Node.js 中，可以使用 http-proxy-middleware 来实现代理服务器

```javascript
npm install http-proxy-middleware

const express = require('express');
const { createProxyMiddleware } = require('http-proxy-middleware');
const app = express();

app.use('/api', createProxyMiddleware({
  target: 'http://example.com', // 目标服务器
  changeOrigin: true
}));
```

## nodemon-监控文件
主要功能是实时监视 Node.js 应用中的文件变动，并在检测到文件修改时自动重启服务器



1. 安装：`npm install nodemon`
2. 在项目的package.json文件中添加一个使用 Nodemon 的启动脚本![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730193646320-4fff1445-a2b4-4891-a15a-3e90693be351.png)
3. 创建一个nodemon.json文件在项目的根目录中，定义多种配置选项
    - ignore：设置忽略的文件或目录，不触发重启
    - watch：设置需要监控的目录
    - delay：设置重启延迟时间（毫秒）
    - ext：设置需要监控的文件扩展名
    - ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730193716521-23c07f20-7d15-4810-bc1a-3af87f40ae2d.png)

## RESTful API
RESTful API（Representational State Transferful APIs）是一种基于 REST 原则构建的API风格，它使得开发者可以通过网络对资源进行CRUD操作（创建、读取、更新、删除）



> REST 原则是什么？
>
> 是一种软件架构风格，用于设计网络应用程序。REST原则是构成REST架构的基础，它强调如何通过HTTP协议在网络中传递信息，下面是 5 个主要原则
>
> 1. 客户端-服务器分离
> 2. 无状态：每个请求从客户端到服务器必须包含所有必要的信息，以便服务器可以处理该请求，服务器不应该保存之前的任何状态信息
> 3. 可缓存：响应应该被标记为可缓存或不可缓存，通过使用HTTP缓存机制，可以减少后续请求的延迟和网络流量
> 4. 统一接口：统一接口简化了系统的复杂性
> 5. 分层系统：通信可以在多个层次上进行，每个层次都可以添加或移除，而不会影响其他层次
>



RESTful API通常使用标准的HTTP状态码来响应请求，例如：

+ 200 OK：请求成功
+ 201 Created：资源被成功创建
+ 400 Bad Request：请求无效
+ 404 Not Found：请求的资源不存在
+ 500 Internal Server Error：服务器内部错误



## OPTIONS 预检请求
OPTIONS请求方法是一种预检请求，用来在实际发送另一个请求之前，检测服务器对特定请求的支持情况，这主要用于网络中的跨源资源共享（CORS）场景



OPTIONS请求是浏览器发起的，满足以下任意一点即会发起：

+ 自定义请求方法：使用非简单请求方法，例如 PUT、DELETE、PATCH
+ 自定义请求头部字段：请求包含自定义的头部字段
    - 自定义头部字段是指不属于简单请求头部字段列表的字段，例如 Content-Type 为 application/json、Authorization 等。
+ 带凭证的请求：请求需要在跨域环境下发送和接收凭证（例如包含 cookies、HTTP 认证等凭证信息）



**原理**

当尝试从一个源（如域名A）发起对另一个源（如域名B）的请求时，浏览器会首先使用OPTIONS请求方法发送一个预检请求到服务器

这个请求会包括以下HTTP头部信息：

+ Access-Control-Request-Method：告诉服务器请求将使用哪HTTP方法（如POST、PUT）
+ Access-Control-Request-Headers：如果实际请求中将包含自定义头部信息，这个头部会列出那些头部



接收到 OPTIONS 请求后，服务器需要响应以下信息：

+ Access-Control-Allow-Origin: 指明哪些域名可以访问资源
+ Access-Control-Allow-Methods: 指明哪些HTTP方法被允许
+ Access-Control-Allow-Headers: 指明在实际请求中可以使用哪些HTTP头部
+ Access-Control-Max-Age: 指明浏览器应该将预检请求的结果缓存多长时间



**使用场景**

+ 跨域请求：在跨域请求中，OPTIONS请求用于确认服务器是否允许来自不同源的特定类型的HTTP请求
+ RESTful APIs：在REST架构中，OPTIONS请求方法可以用来获取关于服务器端某个资源的元信息，如服务器支持的方法列表等

## SSE 单工通讯
### 概念
SSE（Server-Sent Events）单工通讯是一种允许服务器主动向客户端发送信息更新的技术

与传统的请求/响应模式不同，SSE提供了一种建立持久连接的方法，通过这种连接，服务器可以实时地发送新数据到客户端，而无需客户端不断地发起请求

像是webSocket的弱化版本，webSocket属于全双工通讯，也就是前后端可以互相实时发送信息，因为有时候完全用不上webSocket的完整功能，只需要它的部分能力，SSE就是为此诞生的。比如大屏的项目，需要实时展示后端传递过来的信息数据，在页面上及时反馈，只需要后端传递前端

### 实现
#### **服务器**
设置响应头：

+ Content-Type: text/event-stream：告诉客户端这是一个事件流，也告诉浏览器接下来的响应是一个持续的流，不是一次性响应
+ Cache-Control: no-cache：确保数据不会被缓存
+ Connection: keep-alive：保持连接打开



发送事件：事件类型和数据内容

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730196311475-6aac46bc-954b-4698-99a8-d7c56b9a9f17.png)

+ 按照 data: <message>\n\n 的格式发送消息
    - 其中 <message> 是服务器要发送的数据
    - 头部 data: 是必须的，后面跟着具体的数据内容
    - \n\n 是必须的，它告诉客户端数据发送完毕
+ 按照 event: test\n 格式发送
    - 在客户端的 EventSource 中，可以使用 addEventListener 监听这个具体的事件名 test
    - 在没有指定 event 的情况下，事件默认被视为 "message"，可以通过 onmessage 处理器接收
+ 可以发送注释行，以冒号 : 开头，这些行会被客户端忽略，但可以用来保持连接活跃

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/sse_endpoint', (req, res) => {
  // 设置 SSE 特定的响应头
  res.writeHead(200, {
    'Content-Type': 'text/event-stream',
    'Cache-Control': 'no-cache',
    'Connection': 'keep-alive',
  });

  // 发送初始事件
  //没有发送event,默认是Message
  res.write('data: Initial connection established\n\n');

  // 定时发送事件
  const intervalId = setInterval(() => {
    res.write(`data: ${new Date().toLocaleTimeString()}\n\n`);
  }, 1000);

  // 监听请求关闭事件，并清除定时器
  req.on('close', () => {
    clearInterval(intervalId);
    res.end();
  });
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

#### **客户端**
使用 EventSource API 来连接服务器端的 SSE 端点，并监听服务器发送的事件

`new EventSource(url)`：创建连接，url 是服务器端 SSE 端点的 URL

事件监听器：

+ onmessage：当服务器发送消息时触发
+ onopen：当连接成功建立时触发
+ onerror：当连接发生错误时触发

```javascript
// 创建 SSE 连接
const eventSource = new EventSource('sse_endpoint');

// dom 事件监听
eventSource.addEventListener('test', (event) => {
     console.log(event.data)
})

// 监听消息事件
eventSource.onmessage = function(event) {
  console.log('New message from server:', event.data);
};

// 监听连接打开事件
eventSource.onopen = function(event) {
  console.log('Connection opened');
};

// 监听错误事件
eventSource.onerror = function(event) {
  console.error('EventSource failed:', event);
  eventSource.close(); // 尝试重新连接
};
```



# KOA 框架
