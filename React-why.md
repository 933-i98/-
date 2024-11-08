# 拓展
## 插件
+ ES7 React/Redux/GraphQL/React-Native snippets
    - 提供了丰富的 React、Redux、GraphQL 和 React Native 的代码片段，可以快速生成常见的代码模板
+ ESLint
    - 用于检查和提示 JavaScript 代码中的潜在问题和错误，可以与 React 项目一起使用，提供代码质量保证
+ Prettier - Code formatter
    - 用于格式化代码的插件
+ auto import
    - 识别组件导入
+ vscode-styled-components
    - css-in-js 代码提示

## 插件快捷键-vscode
+ rec：快速创建继承自 Component 的组件
+ rpce：快速创建继承自 PureComponent 的组件
+ rfc：快速创建函数式的组件
+ rsc：快速创建 constroctur(){}
+ dob：const {} = this.state

## 插件快捷键-webstorm
rpce：快速创建继承自 PureComponent 的组件

rcc：快速创建继承自 Component 的组件

rsc：快速创建函数式的组件

在 webstorm 里可以修改，这里采用了和 vscode 相同的快捷键

# 基本使用
特点

+ 声明式编程
+ 组件化开发
+ 一次学习，多平台适配



在使用前，需要安装对应的依赖包（3个）

+ react：包含react所必须的核心代码
+ react-dom：react渲染在不同平台所需要的核心代码
+ babel：将Jsx转换为React代码的工具
    - Jsx代码是无法被浏览器识别的，所以需要转换为普通的js代码
+ 可以cdn引入，下载后本地添加，通过npm(后续脚手架)

```html
<script src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

为什么要进行拆分？原因就是react-native

+ react包中包含了react web和react-native所共同拥有的核心代码。
+ react-dom针对web和native所完成的事情不同：
    - web端：react-dom会将jsx最终渲染成真实的DOM，显示在浏览器中
    - native端：react-dom会将jsx最终渲染成原生的控件（比如Android中的Button，iOS中的UIButton）



 Babel是什么？

+ 又名 Babel.js
+ 是目前前端使用非常广泛的编译器、转移器
+ 比如当下很多浏览器并不支持ES6的语法，但是确实ES6的语法非常的简洁和方便，我们开发时希望使用它
+ 那么编写源码时我们就可以使用ES6来编写，之后通过Babel工具，将ES6转成大多数浏览器都支持的ES5的语法

React和Babel的关系：

+ 默认情况下开发React其实可以不使用babel
+ 但是前提是我们自己使用 React.createElement 来编写源代码，它编写的代码非常的繁琐和可读性差

那么我们就可以直接编写jsx（JavaScript XML）的语法，并且让babel帮助我们转换成React.createElement

+ 后续还会详细讲到
+ 编写React的script代码中，必须添加 type="text/babel"，作用是可以让babel解析jsx的语法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713776556270-4f8888cf-efe1-41cc-9e3c-ba416ecc56cc.png)

# 组件化
## 基本概念
在React中，如何封装一个组件呢？这里我们暂时使用类的方式封装组件：

1. 定义一个类（类名**大写**，组件的名称是必须**大写**的，小写会被认为是HTML元素），继承自React.Component

`class Appname extends React.Component{}`

2. 实现当前组件的render函数：将数据、方法、render函数封装到一起，在根页面只需要render(<app/>)即可

## 数据依赖
组件中的数据，可以分成两类：

+ 参与界面更新的数据：当数据变量时，需要更新组件渲染的内容；
+ 不参与界面更新的数据：当数据变量时，不需要更新将组建渲染的内容；

参与界面更新的数据我们也称之为是**参与数据流**，这个数据是定义在当前对象的state中，可以通过在构造函数中 **this.state = {定义的数据}**

```html
<script>
	class App extends React.Component {
		constructor() {
			super();
			this.state = {
				msg: 'hello'
			}
		}
		render() {
			return (
					<div>
						<h2>{this.state.msg}</h2>
						<button>修改文本</button>
					</div>
			)
		}
}
</script>
```

调用 super() 可以确保父类的构造函数被执行，从而继承父类的特性，在调用 super() 之后，你才能在子类的构造函数中使用 this，访问子类实例的属性和方法

## 事件绑定
当我们的数据发生变化时，我们可以调用 **this.setState** 来更新数据，并且通知React进行update操作；在进行update操作时，就会重新调用render函数，并且使用最新的数据，来渲染界面

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713834565596-cf059b1f-10ca-4832-bfc3-1408acae9515.png)

在类中直接定义一个函数，并且将这个函数绑定到元素的onClick事件上，当前这个函数的this指向的是谁？很奇怪，默认情况下居然是undefined

因为在正常的DOM操作中，监听点击，监听函数中的this其实是节点对象（比如说是button对象）。

这是因为React并不是直接渲染成真实的DOM，我们所编写的button只是一个语法糖，它的本质React的Element对象。那么在这里发生监听的时候，react在执行函数时并没有绑定this，默认情况下就是一个undefined；

我们在绑定的函数中，可能想要使用当前对象，比如执行 this.setState 函数，就必须拿到当前对象的this，我们就需要在传入函数时，给这个函数直接绑定this，类似于这样的写法：<button onClick={this.changeText.bind(this)}>改变文本</button>

## 实例
### 电影列表
![写法1：for循环](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713837450000-61837a07-aa1a-4198-8806-0bc36816e63a.png)

![写法2：map函数](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713837574065-05540e1e-13ad-4bf2-9c9b-dcf9e5530b48.png)

map()方法是用于遍历数组的每个元素，并对每个元素执行提供的函数，最终返回一个新数组，该数组包含每个元素按照提供的函数进行处理后的结果。map()方法接受一个回调函数作为参数，该回调函数会被传递数组中的每个元素和它们的索引作为参数，并且可以选择性地接受数组本身作为第三个参数。map()方法不会修改原始数组，而是返回一个新数组，其中包含对每个元素应用回调函数后的结果。

### 计数器
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713838140259-8ba67cc4-cfd8-461c-b19f-6d177175c766.png)

# JSX语法
## 基本知识
JSX是一种JavaScript的语法扩展（extension），也在很多地方称之为JavaScript XML，因为看起就是一段XML语法，它用于描述我们的UI界面，并且它完全可以和JavaScript融合在一起使用，它不同于Vue中的模块语法，不需要专门学习模块语法中的一些指令（比如v-for、v-if、v-else、v-bind）

## 使用
### 书写规范
+ JSX的顶层只能有一个根元素，所以通常会在外层包裹一个div元素（或者使用后面我们学习的Fragment）
+ 为了方便阅读，我们通常在jsx的外层包裹一个小括号**()**，这样可以方便阅读，并且jsx可以进行换行书写
+ JSX中的标签可以是单标签，也可以是双标签，如果是单标签，必须以**/>**结尾

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713841243653-7334f1fd-8bdc-459a-84b5-95253c1f5029.png)

### 注释
{/*   */}

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713841367388-a5a7e6b7-4e8a-4c69-936e-867b89059726.png)

### 嵌入变量作为子元素
+ 情况一：当变量是Number、String、Array类型时，可以直接显示
+ 情况二：当变量是null、undefined、Boolean类型时，内容为空
    - 如果希望可以显示null、undefined、Boolean，那么需要转成字符串
    - 转换的方式有很多，比如toString方法、和空字符串拼接，String(变量)等方式
+ 情况三：Object对象类型不能作为子元素（not valid as a React child）

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713854595600-802dfb8b-3242-4c06-9ec1-cd21f9b1c662.png)

age,msg,names 均可以正常显示

abc 为空' '

d 为 undefined

### 嵌入表达式
运算表达式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713855050907-59c4cc35-867c-4fe4-9494-57eabb44d934.png)

三元运算符

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713855205843-acc94b31-d541-454a-920b-b67d2750cf07.png)

执行一个函数：`<h2>{add()}</h2>`

### 绑定属性
+ 比如元素都会有title属性
+ 比如img元素会有src属性
+ 比如a元素会有href属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713856170049-d35b136c-4f7e-4388-b11f-4ab14efdf5d0.png)

+ 比如元素可能需要绑定class**(使用 className)**

![方法1：字符串的拼接](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713856563824-6be732b4-0425-4e8a-aa8f-5f75eb508eba.png)![方法2：数组转化字符串](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713856703735-110e3e55-0049-4aca-9508-26db7b6822f6.png)

+ 比如原生使用内联样式style

外层的括号表示引用，内层的括号表示是对象格式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713857034152-5db3acba-d2a8-4a10-9e4e-9cc50111fac0.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713857077593-3dd754a5-d4fe-4dd0-a24d-25e8b39863b9.png)

## 事件绑定
React 事件的命名采用小驼峰式（camelCase），而不是纯小写，需要通过{}传入一个事件处理函数，这个函数会在事件发生时被执行

### this 的绑定问题
在事件执行后，我们可能需要获取当前类的对象中相关的属性，这个时候需要用到this，如果我们这里直接打印this，也会发现它是一个undefined，为什么是undefined呢？

原因是btnClick函数并不是我们主动调用的，而且当button发生改变时，React内部调用了btnClick函数，而它内部调用时，并不知道要如何绑定正确的this

如何解决this的问题呢？

+ 方案一：bind给btnClick显示绑定this![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713926824448-a2be1a79-41dd-477e-8746-0112388782d4.png)
+ 方案二：使用 ES6 class fields 语法
+ ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713926835333-8a35bb26-a9d6-4a92-9634-885b93e21bcb.png)
+ 方案三：事件监听时传入箭头函数（个人推荐）![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713926941931-c9033ab6-3aa8-4691-a67d-e2d4d160feb5.png)

## 事件参数传递
在执行事件函数时，有可能我们需要获取一些参数信息：比如event对象、其他参数

情况一：获取event对象

很多时候我们需要拿到event对象来做一些事情（比如阻止默认行为），那么默认情况下，event对象有被直接传入，函数就可以获取到event对象

情况二：获取更多参数

有更多参数时，我们最好的方式就是传入一个箭头函数，主动执行的事件函数，并且传入相关的其他参

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713927531368-05e70e29-ebe3-4c9c-8bdc-a4d561a92d29.png)

## 条件渲染
 某些情况下，界面的内容会根据不同的情况显示不同的内容，或者决定是否渲染某部分内容：

+ 在vue中，我们会通过指令来控制：比如v-if、v-show
+ 在React中，所有的条件判断都和普通的JavaScript代码一致

三种方式

1. 条件判断语句：适合逻辑较多的情况

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713930105688-40d54599-20f0-41ff-a5ba-e3d9e850df0d.png)

2. 三元运算符：适合逻辑比较简单

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713930286734-bde3a5f3-08a3-4ad0-b91b-c43b11d83593.png)

3. 与运算符&&：获取某一个数据，有数据显示，没数据不显示

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713930637447-cee88e8a-930e-4de4-bcea-4c73bda4f221.png)

## 列表渲染
真实开发中我们会从服务器请求到大量的数据，数据会以列表的形式存储， 在React中并没有像Vue模块语法中的v-for指令，需要我们通过JavaScript代码的方式组织数据，转成JSX。 在React中，展示列表最多的方式就是使用数组的**map**高阶函数

通常在展示一个数组中的数据之前，需要先对它进行一些处理：

+ 过滤掉一些内容：filter函数
+ 截取数组中的一部分内容：slice函数，注意 silce(num1,num2)是**左闭右开** [num1,num2)

![map](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713942309368-2f3935c1-286f-4259-9fc8-f6eb6fd5f7e6.png)![filter(年龄>20)](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713942458449-df5174fc-ac3a-42e6-be7d-96d436c2c704.png)![slice(0和1)](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713942635315-46afc488-c416-43dc-9158-be7c7ade8aa1.png)

这些函数可以写在一起使用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713942882739-6e4f1e2e-4165-420e-99aa-61e1b4515c13.png) 

我们需要在列表展示的jsx中添加一个key，key主要的作用是为了提高diff算法时的效率，这个在后续内容中再进行讲解

## JSX 本质
实际上，jsx 仅仅只是 React.createElement(component, props, ...children) 函数的语法糖，所有的jsx最终都会被转换成React.createElement的函数调用。

createElement需要传递三个参数：

+ 参数一：type
    - 当前ReactElement的类型
    - 如果是标签元素，那么就使用字符串表示 “div”
    - 如果是组件元素，那么就直接使用组件的名称
+ 参数二：config
    - 所有jsx中的属性都在config中以对象的属性和值的形式存储
    - 比如传入className作为元素的class
+ 参数三：children
    - 存放在标签中的内容，以children数组的方式进行存储
    - 如果是多个元素，React内部有对它们进行处理，处理的源码在下方

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714006240768-1a0aebc1-5878-435e-890a-b65bbf2198fb.png)

可以在babel的官网中快速查看转换的过程：[babel官网](https://babeljs.io/repl/#?presets=react)

我们自己来编写React.createElement代码，type="text/babel"可以被我们删除掉了， <script src="../react/babel.min.js"></script>也可以删掉了

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714006802976-cfbb4f43-ab5d-4e32-b582-01a106298b99.png)



虚拟 DOM 的创建过程：我们通过 React.createElement 最终创建出来一个 ReactElement对象，然后React利用ReactElement对象组成了一个JavaScript的对象树，JavaScript的对象树就是虚拟DOM

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714007017385-b66adf8b-45b3-448c-bcfa-16c26322ada2.png)

虚拟DOM帮助我们从命令式编程转到了声明式编程的模式

React官方的说法：Virtual DOM 是一种编程理念。在这个理念中，UI以一种理想化或者说虚拟化的方式保存在内存中，并且它是一个相对简单的JavaScript对象，我们可以通过ReactDOM.render让 虚拟DOM 和 真实DOM同步起来，这个过程中叫做协调（Reconciliation），这种编程的方式赋予了React声明式的API，你只需要告诉React希望让UI是什么状态，React来确保DOM和这些状态是匹配的，你不需要直接进行DOM操作，就可以从手动更改DOM、属性操作、事件处理中解放出来

# 脚手架
## 步骤
1. 安装 node
2. 安装 create-react-app -g
3. create-react-app ra_01
4. 运行 npm run start

## 目录结构分析
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714027292407-92e07ec2-998c-46ed-acde-e71e8145d201.png)

## PWA
了解一下即可

PWA全称Progressive Web App，即渐进式WEB应用。一个 PWA 应用首先是一个网页, 可以通过 Web 技术编写出一个网页应用，随后添加上 App Manifest 和 Service Worker 来实现 PWA 的安装和离线等功能，这种Web存在的形式，我们也称之为是 Web App

PWA解决了哪些问题呢？

+ 可以添加至主屏幕，点击主屏幕图标可以实现启动动画以及隐藏地址栏
+ 实现离线缓存功能，即使用户手机没有网络，依然可以使用一些离线功能
+ 实现了消息推送
+ 等等一系列类似于Native App相关的功能

### 脚手架的 webpack
 React脚手架默认是基于Webpack来开发的，但是我们并没有在目录结构中看到任何webpack相关的内容，原因是React脚手架将webpack相关的配置隐藏起来了（其实从Vue CLI3开始，也是进行了隐藏）

如果我们希望看到webpack的配置信息，我们可以执行一个package.json文件中的一个脚本："eject": "react-scripts eject"， 这个操作是不可逆的，所以在执行过程中会给与我们提示,`npm run eject`

### 基础使用
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714030233900-ac122bfc-5cdc-48f7-bf8b-e5a959cace59.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714030245449-279d0c04-295e-4b71-a4f9-002f45e7ecdc.png)

# 组件化开发
## 分类
React的组件相对于Vue更加的灵活和多样，按照不同的方式可以分成很多类组件：

+ 根据组件的定义方式，可以分为：函数组件和类组件
+ 根据组件内部是否有状态需要维护，可以分成：无状态组件和有状态组件
+ 根据组件的不同职责，可以分成：展示型组件和容器型组件

这些概念有很多重叠，但是他们最主要是关注数据逻辑和UI展示的分离：

+ 函数组件、无状态组件、展示型组件主要关注UI的展示；
+ 类组件、有状态组件、容器型组件主要关注数据逻辑；

当然还有很多组件的其他概念：比如异步组件、高阶组件等，后续再学习。

## 类组件
类组件的定义有如下要求：

+ 组件的名称是大写字符开头（无论类组件还是函数组件）
+ 类组件需要继承自 React.Component
+ 类组件必须实现render函数

使用class定义一个组件：

+ constructor是可选的，通常在constructor中初始化一些数据
+ this.state中维护的就是组件内部的数据
+ render() 方法是 class 组件中唯一必须实现的方法

### render 函数的返回值
当 render 被调用时，它会检查 this.props 和 this.state 的变化并返回以下类型之一：

+ React 元素：
    - 通常通过 JSX 创建
    - 例如，<div /> 会被 React 渲染为 DOM 节点，<MyComponent /> 会被 React 渲染为自定义组件
    - 无论是 <div /> 还是 <MyComponent /> 均为 React 元素
+ 数组或 fragments：使得 render 方法可以返回多个元素。
+ Portals：可以渲染子节点到不同的 DOM 子树中。
+ 字符串或数值类型：它们在 DOM 中会被渲染为文本节点
+ 布尔类型或 null：什么都不渲染。

## 函数组件
函数组件是使用function来进行定义的函数，只是这个函数会返回和类组件中render函数返回一样的内容

函数组件有自己的特点（hooks 会弥补下面的缺点）

+ 没有生命周期，也会被更新并挂载，但是没有生命周期函数
+ this关键字不能指向组件实例（因为没有组件实例）
+ 没有内部状态（state）

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714096408379-b7a8918f-26cc-404c-ae70-13cd78add542.png)

## 生命周期
### 解析
装载阶段（Mount），组件第一次在DOM树中被渲染的过程

更新过程（Update），组件状态发生变化，重新更新渲染的过程

卸载过程（Unmount），组件从DOM树中被移除的过程

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714097630918-e19345c5-2c64-49c3-9b20-ea65b85ff8dd.png)

### 生命周期函数
constructor

+ 如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数
+ constructor中通常只做两件事情
    - 通过给 this.state 赋值对象来初始化内部的state
    - 为事件绑定实例（this）

componentDidMount

+ 在组件挂载后（插入 DOM 树中）立即调用
+ 通常进行如下操作
    - 依赖于DOM的操作可以在这里进行
    - 发送网络请求最好的地方（官方建议）
    - 在此处添加一些订阅（会在componentWillUnmount取消订阅）

componentDidUpdate

+ 在更新后会被立即调用，首次渲染不会执行此方法
+ 当组件更新后，可以在此处对 DOM 进行操作
+ 如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求；（例如，当 props 未发生变化时，则不会执行网络请求）

componentWillUnmount

+ 在组件卸载及销毁之前直接调用
+ 在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在该函数中创建的订阅

### 不常用的生命周期函数
+ getDerivedStateFromProps：state 的值在任何时候都依赖于 props时使用，该方法返回一个对象来更新state
+ getSnapshotBeforeUpdate：在React更新DOM之前回调的一个函数，可以获取DOM更新前的一些信息（比如说滚动位置）
+ shouldComponentUpdate：该生命周期函数很常用，在等待讲性能优化时再来详细讲解

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714098134985-bf741374-00c5-45b6-8844-f5ba4c489595.png)

## 组件的嵌套
App组件是Header、Main、Footer组件的父组件

Main组件是Banner、ProductList组件的父组件

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714098199660-a9924499-6a74-432d-b724-38781ce1cc6d.png)

## 组件间通信
### 父传子
父组件通过 属性=值 的形式来传递给子组件数据，子组件通过 props 参数获取父组件传递过来的数据

其实，在没有使用 state 的前提下，可以省略 constructor，react 内部会自动进行 props 的操作

对于传递给子组件的数据，有时候我们可能希望进行验证，特别是对于大型项目来说。如果你项目中默认继承了Flow或者TypeScript，那么直接就可以进行类型验证。但是，即使我们没有使用Flow或者TypeScript，也可以通过 prop-types 库来进行参数验证 

```jsx
import propTypes from 'prop-types'
MainBanner.propTypes = {
    banners: propTypes.array.isRrquired
		//isRrquired表示必传参数
}
```

如果没有传递，希望有默认值，使用defaultProps就可以了

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714112340992-81e4fcd5-4ffe-4e80-8bd9-8a7ba57981dc.png)

#### 实例
App 传递 msg 和 age 给 son，son 设置类型匹配和默认值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715070752440-7c8f848c-fb07-4797-a0d2-c50a8d171cf0.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715070738493-873f618c-07e0-4467-9b3e-1d85d0a2bc40.png)

### 子传父
通过props传递消息，让父组件给子组件传递一个回调函数，在子组件中调用这个函数即可

子组件在函数中使用 this.props.标志(参数)，当子组件函数被触发时，父组件对应的标志被激活并执行回调函数，回调函数中有传递过来的参数，使用该参数即可

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714182036577-6f8b5059-a313-4056-85af-e9d34627732e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714182046882-3a41826a-b1a0-4b88-bcaa-f8e86c4b4f4c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714182060920-192e0b04-e938-4b15-b573-b17fedeadffb.png)

### 插槽
React对于这种需要插槽的情况非常灵活，有两种方案可以实现：

+ 组件的children子元素
+ props属性传递React元素

#### children
每个组件都可以获取到 props.children，它包含组件的开始标签和结束标签之间的内容

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714182333136-f55f1366-3b9d-4b47-9577-4b222355c9ab.png)

#### props-推荐
通过children实现的方案虽然可行，但是有一个弊端：”通过索引值获取传入的元素很容易出错，不能精准的获取传入的原生“，另外一个种方案就是使用 props 实现，通过具体的属性名，可以让我们在传入和获取时更加的精准

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714182429746-87c9d480-23ac-42fb-9f4f-93b698838a25.png)

### Context
如果层级更多的话，一层层传递是非常麻烦，并且代码是非常冗余的，所以 React提供了一个API：Context

Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props，Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言

下面是一些常用 API

#### React.createContext
+ 创建一个需要共享的Context对象
+ 如果一个组件订阅了Context，那么这个组件会从离自身最近的那个匹配的 Provider 中读取到当前的context值
+ defaultValue是组件在顶层查找过程中没有找到对应的Provider，那么就使用默认值

#### Context.Provider
+ 每个 Context 对象都会返回一个 Provider React 组件，它允许消费组件订阅 context 的变化
+ Provider 接收一个 value 属性，传递给消费组件
+ 一个 Provider 可以和多个消费组件有对应关系
+ 多个 Provider 也可以嵌套使用，里层的会覆盖外层的数据；
+ 当 Provider 的 value 值发生变化时，它内部的所有消费组件都会重新渲染
+ Provider提供Context的值，而Consumer（或contextType、useContext钩子）消费这个值

#### Class.contextType
+ 挂载在 class 上的 contextType 属性会被重赋值为一个由React.createContext() 创建的 Context 对象
+ 可以使用 this.context 来消费最近 Context 上的那个值
+ 可以在任何生命周期中访问到它，包括 render 函数中

#### Context.Consumer
+ 这里，React 组件也可以订阅到 context 变更。这能让你在 函数式组件 中完成订阅 context。
+ 这里需要 函数作为子元素（function as child）这种做法
+ 这个函数接收当前的 context 值，返回一个 React 节点
+ 使用Context.Consumer 的时机
    - 当使用value的组件是一个函数式组件时
    - 当组件中需要使用多个Context时  

#### 步骤
1. 创建Context： 首先，你需要创建一个Context对象。这通常在单独的文件中完成，比如ThemeContext.js

```jsx
import React from 'react';

// 创建一个新的 Context 对象
const ThemeContext = React.createContext();

export default ThemeContext;
```

2. 提供Context值： 在组件层次结构的顶层，你需要提供Context的值。通常，这是在父组件的render方法中完成的

```jsx
import React, { Component } from 'react';
import ThemeContext from './ThemeContext';

class App extends Component {
	state = {
		theme: 'light', // 举例：定义一个主题状态
	};

	render() {
		return (
			<ThemeContext.Provider value={this.state.theme}>
				{/* 子组件 */}
			</ThemeContext.Provider>
		);
	}
}

export default App;
```

3. 消费Context值： 现在，任何组件都可以消费Context的值。可以使用contextType、Context.Consumer或useContext钩子来获取它。
    1. 使用contextType（仅适用于类组件）

```jsx
import React, { Component } from 'react';
import ThemeContext from './ThemeContext';

class MyClassComponent extends Component {
  render() {
    const theme = this.context; // 获取Context值
    return <div>当前主题：{theme}</div>;
  }
}

MyClassComponent.contextType = ThemeContext; // 声明Context类型

export default MyClassComponent;
```

    2. 使用Context.Consumer

```jsx
import React from 'react';
import ThemeContext from './ThemeContext';

const MyFunctionalComponent = () => (
  <ThemeContext.Consumer>
    {theme => <div>当前主题：{theme}</div>}
  </ThemeContext.Consumer>
)

export default MyFunctionalComponent;
```

    3. 使用useContext钩子（仅适用于函数组件）,需要引用后使用

```jsx
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

const MyFunctionalComponent = () => {
  const theme = useContext(ThemeContext); // 获取Context值
  return <div>当前主题：{theme}</div>;
};

export default MyFunctionalComponent;
```

如果你有多个 context，你可以多次使用 useContext 钩子来分别获取它们的值。每次调用 useContext 都需要传入对应的 Context 对象。

```jsx
import React, { useContext } from 'react';
import ThemeContext from './Contexts/ThemeContext';
import UserContext from './Contexts/UserContext';

const MyComponent = () => {
    const theme = useContext(ThemeContext);
    const user = useContext(UserContext);

    return (
        <div>
            <div>当前主题：{theme}</div>
            <div>当前用户：{user}</div>
        </div>
    );
}

export default MyComponent;
```

#### 实例
![创建](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714206859610-82914f07-2129-44c0-a482-7f72bad5d096.png)

![ContextType](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714207048927-ce853b20-5686-4ff6-b81c-ce4c0ea1bdb0.png)

![Context.Consumer](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714206884406-17a26cbd-c8f6-46a0-b386-a57b65a51b14.png)

![useContext](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714207650503-52966ada-883f-4401-baa4-d1e57e74a236.png)

#### hy-event-store
安装：npm install hy-event-store

```javascript
import {HYEventBus} from 'hy-event-store'
const eventBus = new HYEventBus()

export default eventBus
```

在使用的文件中导入 eventBus

使用 `eventBus.emit("事件名",参数 1 参数 2...)`设置事件

使用 `eventBus.on("事件名",回调函数)`来监听事件

一般在 componentDidMount 中设置监听，在 componentWillUnmount 中取消监听

使用 `eventBus.off("事件名"，回调函数)`来取消监听

## setState
### 为什么使用
开发中我们并不能直接通过修改state的值来让界面发生更新，因为我们修改了state之后，希望React根据最新的State来重新渲染界面，但是这种方式的修改React并不知道数据发生了变化，React并没有实现类似于Vue2中的Object.defineProperty或者Vue3中的Proxy的方式来监听数据的变化，我们必须通过setState来告知React数据已经发生了变化

### 异步更新
为什么setState设计为异步呢？  

setState设计为异步，可以显著的提升性能。如果每次调用 setState都进行一次更新，那么意味着render函数会被频繁调用，界面重新渲染，这样效率是很低的，最好的办法应该是获取到多个更新，之后进行批量更新

如果同步更新了state，但是还没有执行render函数，那么state和props不能保持同步，state和props不能保持一致性，会在开发中产生很多的问题

### setState 拓展写法
setState接受两个参数：第二个参数是一个回调函数，这个回调函数会在更新后会执行

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714958465072-904928c2-6374-40f9-936a-b5ed66257e53.png)

### React18 前后 setState 对比
React18 之前分成两种情况：

+ 在组件生命周期或React合成事件中，setState是异步
+ 在setTimeout或者原生dom事件中，setState是同步  

React18 之后默认是异步的

## 性能优化 SCU
![渲染流程](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714959585887-b973d037-8893-48f4-a3c3-352573511dae.png)![更新流程](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714959605976-2ae2563d-dbb7-45f7-a426-f3ede5c6c88c.png)

### 更新流程
React在props或state发生改变时，会调用React的render方法，会创建一颗不同的树。React需要基于这两颗不同的树之间的差别来判断如何有效的更新UI：

如果一棵树参考另外一棵树进行完全比较更新，那么即使是最先进的算法，该算法的复杂程度为 O(n³)，其中 n 是树中元素的数量，如果在 React 中使用了该算法，那么展示 1000 个元素所需要执行的计算量将在十亿的量级范围。这个开销太过昂贵了。React的更新性能会变得非常低效

于是，React对这个算法进行了优化，将其优化成了O(n)

+ 同层节点之间相互比较，不会垮节点比较
+ 不同类型的节点，产生不同的树结构
+ 开发中，可以通过key来指定哪些节点在不同的渲染下保持稳定

### keys 的优化
我们在前面遍历列表时，总是会提示一个警告，让我们加入一个key属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714963006391-fbd05bf9-3477-4084-9cfc-4ee762eb5bc4.png)

方式一：在最后位置插入数据，这种情况，有无key意义并不大

方式二：在前面插入数据。这种做法在没有key的情况下，所有的li都需要进行修改

当子元素(这里的li)拥有 key 时，React 使用 key 来匹配原有树上的子元素以及最新树上的子元素：

+ 在下面这种场景下，key为111和222的元素仅仅进行位移，不需要进行任何的修改
+ 将key为333的元素插入到最前面的位置即可

key的注意事项

+ key应该是唯一的
+ key不要使用随机数（随机数在下一次render时，会重新生成一个数字）
+ 使用index作为key，对性能是没有优化的

### render 函数被调用
在以后的开发中，我们只要是修改了App中的数据，所有的组件都需要重新render，进行diff算法，性能必然是很低的。事实上，很多的组件没有必须要重新render，它们调用render应该有一个前提，就是依赖的数据（state、props）发生改变时，再调用自己的render方法

如何来控制render方法是否被调用？通过shouldComponentUpdate方法

### shouldComponentUpdate
该方法有两个参数

+ 参数一：nextProps 修改之后，最新的props属性
+ 参数二：nextState 修改之后，最新的state属性

该方法返回值是一个boolean类型

+ 返回值为true，需要调用render方法
+ 返回值为false，不需要调用render方法
+ 默认返回的是true，也就是只要state发生改变，就会调用render方法

比如我们在App中增加一个message属性

+ jsx中并没有依赖这个message，那么它的改变不应该引起重新渲染
+ 但是因为render监听到state的改变，就会重新render，所以最后render方法还是被重新调用了

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715049167166-b39586e0-16ef-45bd-8bd7-0788acaa44e1.png)

#### shallowEqual
调用 `!shallowEqual(oldProps, newProps) || !shallowEqual(oldState, newState)`，这个shallowEqual就是进行浅层比较，会返回 boolean

在 PureComponent 中不需要这样操作

```javascript
function shallowEqual(obj1, obj2) {
  if (obj1 === obj2) {
    return true;
  }

  if (
    typeof obj1 !== "object" ||
    obj1 === null ||
    typeof obj2 !== "object" ||
    obj2 === null
  ) {
    return false;
  }

  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);

  if (keys1.length !== keys2.length) {
    return false;
  }

  for (let key of keys1) {
    if (obj1[key] !== obj2[key]) {
      return false;
    }
  }

  return true;
}
```

### PureComponent
如果所有的类，我们都需要手动来实现 shouldComponentUpdate，那么会给我们开发者增加非常多的工作量。

shouldComponentUpdate中的各种判断的目的是props或者state中的数据是否发生了改变，来决定shouldComponentUpdate返回true或者false，事实上React已经考虑到了这一点，所以React已经默认帮我们实现好了，将class继承自PureComponent，自动对 state 和 props 进行判断

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715049612281-9d03394a-8559-4e6d-be2b-a00c9282c967.png)

### memo
目前我们是针对类组件可以使用PureComponent，那么函数式组件呢？

事实上函数式组件我们在props没有改变时，也是不希望其重新渲染其DOM树结构的

需要使用一个高阶组件memo：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715049981359-cbac0baf-d337-4a0e-8cb2-945804c1e91b.png)

### 不可变数据的力量
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715052066372-18a5202a-ee04-43ea-b891-99fd97a4900c.png)

因为新的 state 和 旧的比较后，发现是同一个数组，所以没法添加新的数据

所以在使用时，一定要重现创建一个数组，而不是直接添加数据到原数组上

有点像浅拷贝的方式

> 浅拷贝（Shallow Copy）是一种复制对象的方法，它创建一个新的对象，新对象的内容与原始对象的内容相同，但是它们共享相同的引用类型数据。简而言之，浅拷贝只复制了对象的引用，而不是复制对象的内容
>
> 浅拷贝通常适用于简单的数据结构，例如数组或对象。当执行浅拷贝时，新对象中的引用类型属性将指向原始对象中相同的引用，这意味着对新对象进行修改可能会影响到原始对象。
>
> 下面是一个使用浅拷贝的示例，使用 JavaScript 中的 Object.assign() 方法实现：
>
> ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715052420250-b3b2b3b6-2701-43b8-affb-f64ccaaeebed.png)
>
> 在上面的示例中，我们使用 Object.assign() 方法将 obj1 对象浅拷贝到 obj2 对象。当我们修改 obj2 的 name 属性时，obj1 的 name 属性保持不变。
>
> 需要注意的是，浅拷贝只能复制一层对象的内容，如果原始对象中包含嵌套的引用类型属性（如对象或数组），那么这些属性仍然会共享相同的引用。
>
> 如果需要创建一个完全独立的对象，不受原始对象的修改影响，可以考虑使用深拷贝（Deep Copy）来复制对象的内容。深拷贝会递归地复制对象的所有属性和子属性，确保每个对象都是独立的。
>
> ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715059810248-f60653dd-8a65-4c34-93c4-f15b3f50e37e.png)
>

## 获取 DOM 方式的 refs
### 使用
在React的开发模式中，通常情况下不需要、也不建议直接操作DOM原生，但是某些特殊的情况，确实需要获取到DOM进行某些操作，可以通过refs获取DOM，有三种方式

1. **(已废弃) **给元素绑定 `ref="why"`，使用时通过 `this.refs.why` 获取
2. 导入并在 constructor 中创建 createRef`this.titleRef=createRef()`，给元素绑定上`ref={this.titleRef}`，通过 `this.titleRef.current `获取
3. 在 constructor 中创建 `this.titleEl = null`，然后给元素绑定上一个函数`ref={el =>{this.titleEl = el}}`
    - 该函数会在DOM被挂载时进行回调，这个函数会传入一个元素对象，可以自己保存，使用时，直接拿到之前保存的元素对象即可

### 值的类型
ref 的值根据节点的类型而有所不同：

+ 当 ref 属性用于 HTML 元素
    - 构造函数中使用 React.createRef() 创建的 ref 接收底层 DOM 元素作为其 current 属性
    - 当组件被挂载到 DOM 上后，ref 对象的 current 属性将会被设置为对应的底层 DOM 元素，可以通过访问 current 属性来操作和获取底层 DOM 元素
+ 当 ref 属性用于自定义 class 组件
    - 使用 ref 属性时，ref 对象将接收到组件的挂载实例作为其 current 属性。
    - 在组件被挂载后，ref 对象的 current 属性将会被设置为对应的组件实例，可以通过访问 current 属性来调用组件的方法和访问组件的属性
+ 不能在函数组件上使用 ref 属性
    - 函数式组件是没有实例的，所以无法通过ref获取他们的实例
    - 如果需要在函数组件中使用 ref，可以使用 React.forwardRef() 来创建一个包装组件，将 ref 传递给内部的子组件
    - ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715063370099-c0c2e453-7118-4b04-8ea0-db240bb94276.png)

## 受控和非受控组件
### 受控组件
受控组件是指由 React 组件自身维护和控制状态的表单元素。通过将表单元素的值（value）和事件处理函数（onChange）与组件的状态关联起来，可以实现对输入数据的控制和管理。

受控组件包括

+ <input>：包括文本输入框 <input type="text" />、密码输入框 <input type="password" />、复选框 <input type="checkbox" />、单选按钮 <input type="radio" /> 
+ <textarea>：多行文本输入框
+ <select>：下拉选择框，包括 <select> 元素和其内部的 <option> 元素
+ <input type="file">：文件上传输入框
+ <input type="range">：滑动条输入框

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715311533413-f7377688-b8c7-4066-b10b-59d7fc6da2f6.png)

![text](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715066726881-533746e8-b550-440d-a70b-e67b5d8712d7.png)

![单个checkbox](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715308456735-13abd887-6a8f-4f26-a714-c7931cf4e5eb.png)

![多个checkbox](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715309707583-51bbdc2d-867b-4cc7-9dd0-e155c944b5f4.png)

![select单选](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715310386598-64ac0d7c-a11d-4f6c-958f-5011c09c57c1.png)

![select多选--拓展](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715311217906-776f7e4f-bac4-43aa-85ba-c3d6ae699e4b.png)

### 非受控组件-不常用
非受控组件是指其值不受 React 组件状态控制的表单元素。相比之下，受控组件的值受组件状态的管理。在非受控组件中，表单元素的值由 DOM 自身管理

非受控组件通常用于一些不需要在 React 状态中保存数据的简单表单，或者需要与第三方库（如 jQuery 插件）集成的情况下。使用非受控组件时，你需要注意确保 DOM 和 React 状态的同步性

在非受控组件中，你仍然可以访问表单元素的值，但是你不能直接通过 React 的状态来控制它们。通常情况下，你会使用 ref 来访问非受控组件中的表单元素，并在需要时获取或修改其值

![select案例](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715311777938-db4faa79-e04d-4941-aa50-365975f86b04.png)

在 constructor 中初始化一个 ref

在 onChange 方法中通过 ref 获取选中的选项的值

在 select 使用 defauletValue 表示默认值

## 高阶组件 HOC
### 概念
高阶组件是参数为组件，返回值为新组件的函数

高阶组件本身不是一个组件，而是一个函数，其次这个函数的参数是一个组件，返回值也是一个组件

高阶组件在一些React第三方库中非常常见：

+ 比如redux中的connect（后续会讲到）
+ 比如react-router中的withRouter（后续会讲到）

缺陷：

+ HOC需要在原组件上进行包裹或者嵌套，如果大量使用HOC，将会产生非常多的嵌套，这让调试变得非常困难
+ HOC可以劫持props，在不遵守约定的情况下也可能造成冲突  

组件的名称都可以通过displayName来修改

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715323722027-53f6a32f-372c-4078-8f5c-2bb41cb46a29.png)

### 例子-身份验证及问题
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715325432079-5ce80654-c4b2-4c15-8b76-6a5ae0f7e233.png)

...props 是 JavaScript 中的扩展语法，它允许你将一个对象的所有属性复制到另一个对象中。在 React 中，...props 通常用于向子组件传递父组件的所有 props

在高阶组件中，...props 用于将被包装组件接收到的所有 props 传递给被包装组件的原始版本。这样做可以确保高阶组件不会影响原始组件的 props，同时也使得高阶组件能够像原始组件一样接收来自父组件的 props。

如果你知道父组件传递给子组件的所有 props，并且你只想将其中一部分传递给下一级组件，你可以直接传递这些 props，而无需使用对象展开运算符。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715325784022-0acfe80b-327e-48bb-8f42-6279dd75dd28.png)

### 应用 1-props 的增强
 不修改原有代码的情况下，添加新的props  

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715325923476-3bc5994d-5d9f-4a6c-94c2-de543c1346ee.png)

### 应用 2-渲染判断鉴权
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715324283548-0a757924-3459-495e-a787-5ee946aa86dd.png)

### 应用 3-生命周期劫持
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715324308917-e6c44455-5ed0-4379-8091-03bbe9c2ad14.png)

## portals 和 fragment
### portals
某些情况下，我们希望渲染的内容独立于父组件，甚至是独立于当前挂载到的DOM元素中（默认都是挂载到id为root的DOM元素上的）

`ReactDOM.createPortal(child,container)`接受两个参数

+ child：是任何可渲染的 React 子元素，例如一个元素，字符串或 fragment
+ container：是一个 DOM 元素 

通常来讲，当你从组件的 render 方法返回一个元素时，该元素将被挂载到 DOM 节点中离其最近的父节点

然而有时候将子元素插入到 DOM 节点中的不同位置也是有好处的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715326949864-111edfb9-0f98-43e9-bcff-9c5199429fa5.png)

 比如说，我们准备开发一个Modal组件，它可以将它的子组件渲染到屏幕的中间位置  ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715327190052-f12aaddf-e893-46f9-8bd1-b289c2e06d96.png)

### fragment
在之前的开发中，我们总是在一个组件中返回内容时包裹一个div元素，我们希望可以不渲染这样一个div，那就要使用Fragment

Fragment 允许你将子列表分组，而无需向 DOM 添加额外节点

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715327509629-960b1ce9-6ffb-4b31-8d29-d41ef85d3b09.png)

React还提供了Fragment的短语法

+ 看起来像空标签 <> </>
+ 如果需要在Fragment中添加key，那么就不能使用短语法（比如 map)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715327578994-9b89ee1e-df86-4ae0-b54c-ace10ab7e8b5.png)

## StrictMode 严格模式
StrictMode 是一个用来突出显示应用程序中潜在问题的工具

+ 与 Fragment 一样，StrictMode 不会渲染任何可见的 UI
+ 它为其后代元素触发额外的检查和警告
+ 严格模式检查仅在开发模式下运行；它们不会影响生产构建
+ 可以直接在 index.js 中包裹 </App>

```jsx
import{StrictMode} from 'react'

render(){
	return(
		<div>
			<StrictMode>

			</StrictMode>
		</div>
	)
}
```

### 检查的是什么
1. 识别不安全的生命周期
2. 使用过时的ref API
3. 检查意外的副作用
    - 这个组件的constructor会被调用两次
    - 这是严格模式下故意进行的操作，让你来查看在这里写的一些逻辑代码被调用多次时，是否会产生一些副作用
    - 在生产环境中，是不会被调用两次的
4. 使用废弃的findDOMNode方法
    - 在之前的React API中，可以通过findDOMNode来获取DOM，不过已经不推荐使用了，可以自行学习演练一下
5. 检测过时的context API
    - 早期的Context是通过static属性声明Context对象属性，通过getChildContext返回Context对象等方式来使用Context的
    - 目前这种方式已经不推荐使用，大家可以自行学习了解一下它的用法

# 动画
## react-transition-group
### 介绍
在开发中，我们想要给一个组件的显示和消失添加某种过渡动画，可以很好的增加用户体验。我们可以通过原生的CSS来实现这些过渡动画，但是React社区为我们提供了react-transition-group用来完成过渡动画。

React曾为开发者提供过动画插件 react-addons-css-transition-group，后由社区维护，形成了现在的 react-transition-group。这个库可以帮助我们方便的实现组件的入场和离场动画，使用时需要进行额外的安装

react-transition-group本身非常小，不会为我们应用程序增加过多的负担

```javascript
# npm
npm install react-transition-group --save
# yarn
yarn add react-transition-group
```

### 主要组件
Transition

+ 该组件是一个和平台无关的组件（不一定要结合CSS）
+ 在前端开发中，我们一般是结合CSS来完成样式，所以比较常用的是CSSTransition

CSSTransition

+ 在前端开发中，通常使用CSSTransition来完成过渡动画效果

SwitchTransition

+ 两个组件显示和隐藏切换时，使用该组件

TransitionGroup

+ 将多个动画组件包裹在其中，一般用于列表中元素的动画

### CSSTransition
CSSTransition执行过程中，有三个状态：appear(第一次出现)、enter、exit

它们有三种状态，需要定义对应的CSS样式：

1. 开始状态：对于的类是-appear、-enter、exit
2. 执行动画：对应的类是-appear-active、-enter-active、-exit-active
3. 执行结束：对应的类是-appear-done、-enter-done、-exit-done

常见属性

+ in（必选）
    - 当in为true时，触发进入状态，会添加-enter、-enter-acitve的class开始执行动画，当动画执行结束后，会移除两个class，并且添加-enter-done的class
    - 当in为false时，触发退出状态，会添加-exit、-exit-active的class开始执行动画，当动画执行结束后，会移除两个class，并且添加-enter-done的class
+ classNames（必选）： 决定了在编写css时，对应的class名称：比如card-enter、card-enter-active、card-enter-done
+ timeout（必选）：过渡动画的时间
+ appear：是否在初次进入添加动画，如果需要显示就添加 appear
+ unmountOnExit（必选）：退出后卸载组件

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715417566577-b3194b78-be93-47b4-978a-c441a9d0c0e0.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715417575776-2aa7e880-5664-4aef-9aeb-790dc4003615.png)

### SwitchTransition
有一个属性mode，有两个值

+ in-out：表示新组件先进入，旧组件再移除
+ out-in：表示就组件先移除，新组建再进入

SwitchTransition组件里面要有CSSTransition或者Transition组件，不能直接包裹你想要切换的组件

SwitchTransition里面的CSSTransition或Transition组件不再像以前那样接受in属性来判断元素是何种状态，取而代之的是key属性，key 需要是不同的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715480268781-315ba92e-19ee-4c82-95d0-56a493009b4e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715480285436-205e0902-c785-406e-9818-23e1422109f1.png)

### TransitionGroup
当有一组动画时，需要将这些 CSSTransition放入到一个TransitionGroup中来完成动画

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715480455265-e19fe962-f5f5-4b0b-8a04-0f3bac9a23f7.png)

可以给 TransitionGroup 传入参数 component，比如`component="ul"`

# react 中的 css
## 内联样式写法
内联样式的优点

+ 内联样式, 样式之间不会有冲突
+ 可以动态获取当前state中的状态

内联样式的缺点

+ 写法上都需要使用驼峰标识
+ 某些样式没有提示
+ 大量的样式, 代码混乱
+ 某些样式无法编写(比如伪类/伪元素)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715512251677-6c49383a-c3f5-4e0d-adce-ec8aa9920588.png)

## 普通 CSS 文件写法
普通的css我们通常会编写到一个单独的文件，之后再进行引入。

这样的编写方式和普通的网页开发中编写方式是一致的

+ 如果我们按照普通的网页标准去编写，那么也不会有太大的问题
+ 但是组件化开发中我们总是希望组件是一个独立的模块，即便是样式也只是在自己内部生效，不会相互影响
+ 但是普通的css都属于全局的css，样式之间会相互影响
+ 这种编写方式最大的问题是样式之间会相互层叠掉

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715512707841-b9a9545f-9f95-45c9-8f79-4eb033b59355.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715512700319-5d2a1f01-b2ec-421e-b62b-6418f78d3cb2.png)

## CSS Module 写法
css modules并不是React特有的解决方案，而是所有使用了类似于webpack配置的环境下都可以使用的。如果在其他项目中使用它，那么我们需要自己来进行配置，比如配置webpack.config.js中的modules: true等

React的脚手架已经内置了css modules的配置，.css/.less/.scss 等样式文件都需要修改成 .module.css/.module.less/.module.scss 等，之后就可以引用并且进行使用了

css modules确实解决了局部作用域的问题，也是很多人喜欢在React中使用的一种方案

缺陷：

+ 引用的类名，不能使用连接符(.home-title)，在JavaScript中是不识别的
+ 所有的className都必须使用`className={styleName.title}` 的形式来编写
+ 不方便动态来修改某些样式，依然需要使用内联样式的方式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715513143274-ffb46b31-41d3-400d-993e-2419a10efdb7.png)

## CSS in JS 写法（用的最多）
“CSS-in-JS” 是指一种模式，其中 CSS 由 JavaScript 生成而不是在外部文件中定义,注意此功能并不是 React 的一部分，而是由第三方库提供

事实上CSS-in-JS的模式就是一种将样式（CSS）也写入到JavaScript中的方式，并且可以方便的使用JavaScript的状态，所以React被人称之为 All in JS

优点

+ CSS-in-JS通过JavaScript来为CSS赋予一些能力，包括类似于CSS预处理器一样的样式嵌套、函数定义、逻辑复用、动态修改状态等等
+ 虽然CSS预处理器也具备某些能力，但是获取动态状态依然是一个不好处理的点
+ 所以，目前可以说CSS-in-JS是React编写CSS最为受欢迎的一种解决方案

目前比较流行的CSS-in-JS的库

+ styled-components
+ emotion
+ glamorous

 目前可以说styled-components依然是社区最流行的CSS-in-JS库  

### styled-components 库
#### 基本使用
安装 `npm install styled-components`

styled-components的本质是通过函数的调用，最终创建出一个组件：

+ 这个组件会被自动添加上一个不重复的class；
+ styled-components会给该class添加相关的样式

另外，它支持类似于CSS预处理器一样的样式嵌套：

+ 支持直接子代选择器或后代选择器，并且直接编写样式
+ 可以通过&符号获取当前元素
+ 直接伪类选择器、伪元素等

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715566002972-f4239d3b-96e8-49f7-ad68-bc1b777cf54e.png)

代码是没有提示的，所以需要安装 vscode 插件`vscode-styled-components`

一般都会写一个对应的 CSS 文件，然后再导入到所需组件中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715566452930-895021be-3a2d-4454-a5ff-a1a279a83e70.png)

而且样式组件是可以嵌套使用的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715566789421-e75ef68d-066a-4671-b4ad-277942435b59.png)

#### props 传参
 这种方式可以有效的解决动态样式的问题

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715566959373-5a51c565-c673-4d19-a225-f27e352a1038.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715567076687-e7d20e69-d637-41e5-9d05-e0de893b0196.png)

其实最好是写一个文件，专门放一些 css 参数，在使用时引入就可以了

#### attrs 默认值（基本不用）
可以设置默认值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715567810570-7a369efd-8867-4341-b42f-7604525f0dd2.png)

#### 高级特性
1. 使用 provider 传递参数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715568244868-bbb62792-0b80-4654-9cc1-32b2dd4e3d1e.png)

2. 继承样式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715568312812-b19e845f-867e-44c5-b72a-8cb9e29c22d2.png)

## classnames 库的使用
React在JSX给了开发者足够多的灵活性，你可以像编写JavaScript代码一样，通过一些逻辑来决定是否添加某些class

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715568475172-b3c0fa84-cb22-4e42-8195-a79de5f8fd6f.png)

classnames 这是一个用于动态添加classnames的一个库

在引入后使用`import classNames from 'classnames';`

`<h2 className={classNames(   )}></h2>`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715568579476-7a751af6-1c48-40ff-bd51-6b915c569a07.png)

# Redux
## 纯函数
### 概念
纯函数要符合下面条件

+ 确定的输入，一定会产生确定的输出
+ 函数在执行过程中，不能产生副作用

副作用：表示在执行一个函数时，除了返回函数值之外，还对调用函数产生了附加的影响，比如修改了全局变量，修改参数或者改变外部的存储

React就要求我们无论是函数还是class声明一个组件，这个组件都必须像纯函数一样，保护它们的props不被修改

### 案例
slice：slice截取数组时不会对原数组进行任何操作,而是生成一个新的数组

splice：splice截取数组, 会返回一个新的数组, 也会对原数组进行修改

slice就是一个纯函数，不会修改数组本身，而splice函数不是一个纯函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715582911263-b91e0b93-f124-42c2-93a1-8a4575d9a95c.png)

## 核心理念
Redux就是一个帮助我们管理State的容器：Redux是JavaScript的状态容器，提供了可预测的状态管理

Redux除了和React一起使用之外，它也可以和其他界面库一起来使用（比如Vue），并且它非常小（包括依赖在内，只有2kb）

### **Store**
Redux 中的 Store 是应用程序的状态树（state tree）的唯一来源。它是一个 JavaScript 对象，包含了整个应用程序的状态。通过 Redux 的 API 可以访问状态、派发行为、监听状态变化等

### **Action**
Action 是一个描述发生了什么的普通 JavaScript 对象。它们是信息的负载，通过 store.dispatch() 方法将它们发送到 store。Redux 中的 action 必须包含一个 type 属性，用于指示要执行的动作类型。除了 type 属性之外，可以添加任意其他属性

### **Reducer**
Reducer 是一个纯函数，接收当前 state 和一个 action，并返回新的 state。它负责处理 action，根据不同的 action 类型来更新状态。Reducer 应该是纯函数

三个原则

+ 单一数据源
+ state 是只读的
+ 使用纯函数进行修改

两个参数：上一次的state和action(dispatch传入的)

一个返回值：作为最新的state

如果没有数据更新，则返回当前的 state，所以需要给 state 一个默认值避免 undefined 出现

### **Dispatch**
Dispatch 是 store 对象的一个方法，用于发送 action 到 reducer，从而更新状态。通过调用 store.dispatch(action) 方法，应用程序可以触发状态的改变

### 工作流程
1. UI 触发 action
2. Redux store 接收 action
3. Reducer 根据 action 类型更新状态
4. Store 更新后通知 UI
5. UI 重新渲染以反映新的状态

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715588589676-0e8adb4a-6cf2-45b7-ab35-878f13d5754f.png)

## 基本使用
### 安装
安装 `npm install redux --save`

创建 src/store/Index.js``

### 原始写法
会对代码进行拆分，将store、reducer、action、constants拆分成一个个文件

创建store/index.js文件：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715588437324-94132f17-c8a0-4973-8348-ac94f9f6f050.png)

创建store/reducer.js文件：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715588444718-4d13ff36-00c8-45f8-b630-6528d5b813e1.png)

创建store/actionCreators.js文件：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715588454549-2d7d7cb7-d654-473d-9f71-af7ea51cb686.png)

创建store/constants.js文件：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715588463077-cfe7d264-6196-4782-9327-a42d9641e9c7.png)

## React 结合 Redux
### 案例
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715602086427-806d3388-4d85-4dac-90f0-96c1d804cf6d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715601994768-fc969dd6-47a2-4842-af17-6d2c7bb7e15d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715602009639-dcc659fc-6858-44c3-9270-616d3f5cf47a.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715602033077-50c99748-f9f8-4316-86f8-795f81d1dad7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715602049513-6ff20df7-570e-44f8-941a-ae0c2f0e01e9.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715602060303-267caf62-9253-455b-9c22-3373e09bd038.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715602067840-ba1b2e87-fed2-4942-99ec-b07658449de3.png)

### react-redux 
安装`npm install react-redux`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715655459804-b6eb1931-b086-4c81-88c4-70b204d6a678.png)

> connect 是 React-Redux 库提供的一个函数，用于连接 React 组件与 Redux 的 store。通过 connect，你可以在 React 组件中访问 Redux 中的状态和派发，从而实现 React 组件与 Redux store 的关联
>
> 通常情况下，你会在导出组件时使用 connect 函数对组件进行包装，connect 函数接受两个参数：mapStateToProps 和 mapDispatchToProps，它们分别用于指定如何将 Redux 的状态映射到组件的 props 以及如何将派发动作映射到组件的 props
>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715656908834-bcf44ed3-7502-4e63-b897-30034d7227ab.png)

## 异步操作
### 中间件
在redux中可以使用中间件（Middleware）进行异步的操作

在这类框架中，Middleware可以帮助我们在请求和响应之间嵌入一些操作的代码，比如cookie解析、日志记录、文件压缩等操作

常见的Redux中间件包括：

+ Redux Thunk：允许在action中返回函数而不仅仅是普通的action对象，从而可以进行异步操作
+ Redux Saga：基于generator函数的中间件，用于管理复杂的异步操作和副作用
+ Redux Logger：用于在控制台输出Redux相关的日志信息，方便调试
+ Redux Promise：允许在action中返回Promise对象，从而可以处理异步操作
+ Redux Persist：用于在本地存储中持久化Redux store的状态

这个中间件的目的是在dispatch的action和最终达到的reducer之间，扩展一些自己的代码，比如日志记录、调用异步接口、添加代码调试功能等等

### applyMiddleware 函数
applyMiddleware 是 Redux 提供的一个函数，用于将中间件应用到 Redux store 中。中间件可以增强 Redux 的功能，例如处理异步操作、记录日志、错误处理等。

applyMiddleware 接受一个或多个中间件作为参数，并返回一个函数，这个函数接受 createStore 函数作为参数，然后返回一个新的增强过的 createStore 函数。这个新的 createStore 函数在创建 Redux store 时会应用传入的中间件。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715674771266-3566a23c-49f6-485f-b25f-318a0ee0b608.png)

在上面的示例中，applyMiddleware(...middleware) 将中间件数组中的中间件依次应用到 Redux store 中。这样，创建的 store 对象就会被这些中间件增强，使得 Redux 的功能得到了扩展，例如支持异步操作（使用了 redux-thunk 中间件）和记录日志（使用了 redux-logger 中间件）等。

applyMiddleware 的使用可以使得 Redux 在应用开发中更加灵活和强大，可以根据项目需求选择合适的中间件来增强 Redux 的功能。

### redux-thunk
我们现在要做的事情就是发送异步的网络请求，所以我们可以添加对应的中间件，这里官网推荐的、包括演示的网络请求的中间件是使用 redux-thunk

默认情况下的dispatch(action)，action需要是一个JavaScript的对象，redux-thunk可以让dispatch(action函数)，action可以是一个函数，该函数会被调用，并且会传给这个函数一个dispatch函数和getState函数；

+ dispatch函数用于我们之后再次派发action；
+ getState函数考虑到我们之后的一些操作需要依赖原来的状态，用于让我们可以获取之前的一些状态

安装`npm install redux-thunk`

### 步骤
1. **创建异步 ActionCreators**：通常，一个同步的 action creator 返回一个 action 对象，而异步 action creator 返回一个函数。这个函数通常会在内部执行异步操作，然后 dispatch 一个或多个同步 action。这个过程可以使用异步函数、Promises或者其他异步机制来实现
2. **使用Redux中间件**：为了能够处理这种返回函数的 action creators，你需要在应用中应用 Redux 中间件。最常见的中间件之一是 Redux Thunk。Redux Thunk 允许你的 action creators 返回一个函数而不仅仅是一个 action 对象。这个函数可以接收 dispatch 和 getState 作为参数，让你能够 dispatch action 或者获取 state
3. **Dispatch 异步 Action**：当你使用异步 action creators 时，你会 dispatch 一个函数，而不是一个简单的 action 对象。这个函数会在内部执行异步操作，然后在适当的时候 dispatch 一个或多个同步 action

```javascript
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';

const store = createStore(
  rootReducer,
  applyMiddleware(thunk)
);

export default store;
```

```javascript
const initialState = {
  // 初始状态
};
const rootReducer = (state = initialState, action) => {
  // reducer 代码
};

export default rootReducer;
```

```javascript
export const fetchDataRequest = () => ({ type: 'FETCH_DATA_REQUEST' });
export const fetchDataSuccess = (data) => ({ type: 'FETCH_DATA_SUCCESS', payload: data });
export const fetchDataFailure = (error) => ({ type: 'FETCH_DATA_FAILURE', payload: error });

export const fetchData = () => {
  return (dispatch) => {
    dispatch(fetchDataRequest()); // 发送请求前的 action

    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => {
        dispatch(fetchDataSuccess(data)); // 成功获取数据后的 action
      })
      .catch(error => {
        dispatch(fetchDataFailure(error)); // 获取数据失败后的 action
      });
  };
};
```

## redux-devtool
redux官网为我们提供了redux-devtools的工具，利用这个工具，我们可以知道每次状态是如何被修改的，修改前后的状态变化等

安装该工具需要两步：

1. 第一步：在对应的浏览器中安装相关的插件（比如Chrome浏览器扩展商店中搜索Redux DevTools即可）
2. 第二步：在redux中继承devtools的中间件

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715677964726-e30df7f5-f65c-4e6f-8f6c-79a2fc8c32b3.png)

## reducer 模块拆分
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715682770189-b33efed7-ad28-4671-a241-90419845c97a.png)

在一个典型的 Redux 应用中，reducers 通常按功能或领域划分为多个模块，每个模块负责管理特定部分的状态。这种模块化的划分方式有助于提高代码的可维护性和可扩展性，使得每个 reducer 只关注于特定部分的状态变化，降低了代码的复杂度。

redux给我们提供了一个combineReducers函数可以方便的对多个reducer进行合并

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715682867837-b3d41a95-b579-4029-94c9-089e903f5c13.png)

## ReduxToolkit
### 概念
Redux Toolkit 是官方推荐的编写 Redux 逻辑的方法， 在很多地方为了称呼方便，也将之称为“RTK”  

安装Redux Toolkit`npm install @reduxjs/toolkit react-redux`

Redux Toolkit的核心API主要是如下几个：

+ configureStore
    - 包装createStore以提供简化的配置选项和良好的默认值。它可以自动组合你的 slice reducer，添加你提供的任何 Redux 中间件，redux-thunk默认包含，并启用 Redux DevTools Extension。
+ createSlice
    - 接受reducer函数的对象、切片名称和初始状态值，并自动生成切片reducer，并带有相应的actions
+ createAsyncThunk:
    - 接受一个动作类型字符串和一个返回承诺的函数，并生成一个pending/fulfilled/rejected基于该承诺分派动作类型的 thunk

#### createSlice
主要包含如下几个参数：

+ name：用户标记slice的名词，在之后的redux-devtool中会显示对应的名词
+ initialState：初始化值
+ reducers：相当于之前的reducer函数
    - 对象类型，并且可以添加很多的函数；
    - 函数类似于redux原来reducer中的一个case语句；
    - 函数的参数：
        * 参数一：state
        * 参数二：调用这个action时，传递的action参数；

createSlice返回值是一个对象，包含所有的actions

#### configueStore
常见参数如下：

+ reducer，将slice中的reducer可以组成一个对象传入此处
+ middleware：可以使用参数，传入其他的中间件（自行了解）
+ devTools：是否配置devTools工具，默认为true

### 重构代码
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715739245210-c7fc2a59-8d54-4b93-a2d8-2c16339c3946.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715739270208-5982d963-37db-4a92-8095-43128a713ae9.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715739279581-1c921ea5-f6c3-4b88-9a95-59f0b5a8692f.png)![原来的reducer----对比](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715840309789-437d26a6-0c0e-4c52-b1eb-6bde1843ed4d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715739287628-55fc7f1f-8372-46ea-918a-069cb3426f7a.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715739295351-13990f04-2836-482a-8aa8-68c5c39e9fcb.png)

### 异步
在之前的开发中，我们通过redux-thunk中间件让dispatch中可以进行异步操作，Redux Toolkit默认已经集成了Thunk相关的功能：createAsyncThunk

当createAsyncThunk创建出来的action被dispatch时，会存在三种状态：

+ pending：action被发出，但是还没有最终的结果；
+ fulfilled：获取到最终的结果（有返回值的结果）；
+ rejected：执行过程中有错误或者抛出了异常；

可以在createSlice的entraReducer中监听这些结果

#### extraReducer
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715741705325-b66b0721-7883-43d6-83b6-c006c23907db.png)

其实只有 fullfilled 需要监听

extraReducer还可以传入一个函数，函数接受一个builder参数，我们可以向builder中添加case来监听异步操作的结果

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715743562877-d855910f-f20e-413d-96f3-a5b6ec591f3b.png)

#### 案例
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715742474107-b792abc4-620e-4c0d-b7b1-ab5ebb474b06.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715742742857-47ffd41d-40c6-431b-a5f3-9c8cb898fa1c.png)

## connect 高阶组件（未）
day86

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715825074471-b4a573e1-399f-4d90-a6d2-11e3ee5ceaba.png)

## 中间件的实现原理(未)
Day87 前几节课

## React 状态管理选择
目前我们已经主要学习了三种状态管理方式：

1. 组件中自己的state管理；
2. Context数据的共享状态；
3. Redux管理应用状态；



在开发中如何选择呢？

+ 首先，这个没有一个标准的答案
+ 某些用户，选择将所有的状态放到redux中进行管理，因为这样方便追踪和共享
+ 有些用户，选择将某些组件自己的状态放到组件内部进行管理
+ 有些用户，将类似于主题、用户信息等数据放到Context中进行共享和管理
+ 做一个开发者，到底选择怎样的状态管理方式，是你的工作之一，可以一个最好的平衡方式（Find a balance that works for you, and go with it.）

why 采用的 state 管理方案

+ UI相关的组件内部可以维护的状态，在组件内部自己来维护
+ 大部分需要共享的状态，都交给redux来管理和维护
+ 从服务器请求的数据（包括请求的操作），交给redux来维护

# React-Router
安装`npm install react-router-dom`

## 基本使用
### BrowserRouter、HashRouter
Router中包含了对路径改变的监听，并且会将相应的路径传递给子组件

+ BrowserRouter使用history模式
+ HashRouter使用hash模式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715913982807-dffe39ee-1938-4b42-b1df-30b43d80d725.png)

### 映射配置
Routes：包裹所有的Route，在其中匹配一个路由

+ Router5.x使用的是Switch组件

Route：Route用于路径的匹配

+ path属性：用于设置匹配到的路径
+ element属性：path 对应的组件，Router5.x使用的是component属性
+ exact：精准匹配，只有精准匹配到完全一致的路径，才会渲染对应的组件，Router6.x不再支持该属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715914891150-a1871906-20ad-4b44-a183-777a24f67df6.png)

### 路由配置和跳转
Link和NavLink

NavLink是在Link基础之上增加了一些样式属性

#### Link
通常路径的跳转是使用Link组件，最终会被渲染成a元素

+ to属性：Link中最重要的属性，用于设置跳转到的路径
+ replace：布尔类型，是否可以返回上个页面(true 为否)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715915783517-7b7c5c13-de79-46f9-895f-820ccfdaccf9.png)

#### NavLink
+ style：传入函数，函数接受一个对象，包含isActive属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715916850818-d11beea7-2604-4dad-9a68-a71a27c1b577.png)

+ className：传入函数，函数接受一个对象，包含isActive属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715916987225-e24eeef8-8c94-4d5e-9d7e-ea03d97068d6.png)

默认的activeClassName：

+ 事实上在默认匹配成功时，NavLink就会添加上一个动态的active class；
+ 所以我们也可以直接编写样式
+ 当然，如果你担心这个class在其他地方被使用了，出现样式的层叠，也可以自定义class

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715917155116-1430e272-8ca9-47e2-a74e-c3e64fe697b9.png)

#### Navigate
Navigate 是一个组件，用于路由的重定向，当这个组件出现时，就会执行跳转到对应的to路径中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715917711286-91e76789-d3ac-45ad-848d-e7f3b785d8f8.png)

我们也可以在匹配到/的时候，直接跳转到/home页面

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715917791937-d9a3c2d6-6190-46bb-98fa-af876b19eca1.png)

#### Not Found
如果用户随意输入一个地址，该地址无法匹配，那么在路由匹配的位置将什么内容都不显示

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715916261970-d86ba890-f7ba-42c0-b851-dcf087c410f1.png)

## 路由嵌套
`<Outlet/>` 组件用于在父路由元素中作为子路由的占位元素

注意加上`/home/`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715936233549-428db0f7-e310-4584-bae7-f4b3b0166aac.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715936256518-b7a1928b-3162-41c2-be82-7ab6cd33c725.png)

## 手动路由跳转
### useNavigate()
使用 js 进行跳转

使用 useNavigate()的 hook，注意需要在函数式组件中使用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715938129797-00fa37ac-f849-4508-8fa3-2faf8f2514db.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715937911506-5ace6c61-e959-457f-a0d0-91b08b1e01c4.png)

### withRouter 高阶组件
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715939129545-42d07458-f55e-4c63-81ba-7acd096c6f4a.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1715939120378-40195019-22ea-4c7f-a206-104828984123.png)

## 参数传递
传递参数有二种方式

+ 动态路由的方式
+ search传递参数

### 动态路由
+  比如/detail的path对应一个组件Detail
+ 如果将path在Route匹配时写成/detail/:id，那么 /detail/abc、/detail/123都可以匹配到该Route，并且进行显示

需要封装一个高阶组件，在高阶组件中使用 useParams

不一定要:id，也可以:name 等

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716171727170-527b52f2-60b5-4510-b1b2-e5be2d6a197d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716172300133-f8f2dd81-2501-48e1-be09-686c0114c861.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716172329308-b528d0bd-f9a7-4658-9683-34093e602ece.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716173478260-1736be4d-7980-4747-8315-ec57368e163d.png)

### Search
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716173441617-7a8620ea-9632-436f-9c89-dffd43d2c61c.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716173449547-9f0a34c7-4501-4a95-8f09-a7f069922378.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716173459143-7982b74e-bc85-489f-8bda-4223810a458b.png)

## 配置方式
目前我们所有的路由定义都是直接使用Route组件，并且添加属性来完成的，但是这样的方式会让路由变得非常混乱，所以需要将所有的路由配置放到一个地方进行集中管理

在Router6.x中，提供了useRoutes API可以完成相关的配置

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716175121503-d2b39e8b-ba97-44d1-b9da-458813f50f98.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716175138223-4176b86b-6b18-4ecc-97ed-93cf3b1b43c9.png)

### 懒加载
如果我们对某些组件进行了异步加载（懒加载），那么需要使用Suspense进行包裹

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716191090248-1bae6f94-3c0b-4759-b7e3-00dbe936c83f.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716191131917-a88d755e-3168-4be4-8b12-d06191306c31.png)

# React Hooks
## 为什么需要 Hooks
Hook 是 React 16.8 的新增特性，它可以让我们在不编写class的情况下使用state以及其他的React特性（比如生命周期）

### class 组件和函数式组件的对比
class组件可以定义自己的state，用来保存组件自己内部的状态

+ 函数式组件不可以，因为函数每次调用都会产生新的临时变量

class组件有自己的生命周期，我们可以在对应的生命周期中完成自己的逻辑

+ 比如在componentDidMount中发送网络请求，并且该生命周期函数只会执行一次
+ 函数式组件在学习hooks之前，如果在函数中发送网络请求，意味着每次重新渲染都会重新发送一次网络请求

class组件可以在状态改变时只会重新执行render函数以及我们希望重新调用的生命周期函数componentDidUpdate等

+ 函数式组件在重新渲染时，整个函数都会被执行，似乎没有什么地方可以只让它们调用一次

### class 组件存在的问题
+ 复杂组件变得难以理解
+ 难以理解的 this 指向问题
+ 组件的复用很难：需要编写高阶函数

### Hooks
它可以让我们在不编写class的情况下使用state以及其他的React特性，我们可以由此延伸出非常多的用法，来让我们前面所提到的问题得到解决

Hook的出现基本可以代替我们之前所有使用class组件的地方，但是如果是一个旧的项目，你并不需要直接将所有的代码重构为Hooks，因为它完全向下兼容，你可以渐进式的来使用它；

Hook只能在函数组件中使用，不能在类组件，或者函数组件之外的地方使用

## useState
useState会帮助我们定义一个 state变量，useState 是一种新方法，它与 class 里面的 this.state 提供的功能完全相同  

+ 接受一个参数为初始值，若不设置则为 undefined
+ 返回一个数组，第一个元素是状态值，第二个元素是更新状态值的函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716255411393-4e418dd5-9a62-4b9c-afce-9d1909fa855c.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716255458448-e2230f2b-e6b3-409f-8bfc-a2d4ae6e3b5a.png)

但是使用它们会有两个额外的规则

+ 只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。
+ 只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。

## useEffect
### 基本操作
useEffect 是 React 提供的一个 Hook，用于在函数组件中执行副作用操作。副作用操作指的是在组件内部与外部世界交互的操作，比如获取数据、订阅、手动操作 DOM 等。useEffect 可以模拟类组件中的生命周期方法，比如 componentDidMount、componentDidUpdate 和 componentWillUnmount。

useEffect 接收两个参数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716259342386-25ce3d5b-b535-4e97-b92a-fba98da92c49.png)

+ 第一个参数：一个函数，用于执行副作用操作
    -  在React执行完更新DOM操作之后，就会回调这个函数
+ 第二个参数（可选）：一个依赖数组，用于控制副作用操作的执行时机。如果不传递第二个参数，则每次组件更新时都会执行副作用操作
    - 如果传递一个空数组 []，则表示副作用操作只在组件挂载和卸载时执行，类似于 componentDidMount 和 componentWillUnmount
    - 如果传递一个包含依赖项的数组，则只有依赖项发生变化时才会执行副作用操作，类似于 componentDidUpdate

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716255841328-7bcf84ff-1ded-49e2-bfe0-5846656ebefc.png)

### 清除副作用
如果 useEffect 返回一个函数，则该函数会在组件卸载时执行，用于清除副作用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716258101886-8f7917a9-2051-4e18-8f86-813a728adf6e.png)

等价于 componentWillUnmount

### 多个 effect
Hook 允许我们按照代码的用途分离它们， 而不是像生命周期函数那样：  React 将按照 effect 声明的顺序依次调用组件中的每一个 effect

可以同时写多个 useEffect，每个都有独立的逻辑

默认情况下，useEffect的回调函数会在每次渲染时都重新执行，但是这会导致一些问题，某些代码我们只是希望执行一次即可，类似于componentDidMount和componentWillUnmount中完成的事情（比如网络请求、订阅和取消订阅），另外，多次执行也会导致一定的性能问题

## useContext
用于在函数组件中获取上下文(Context)中的值。上下文允许您将值传递给组件树中的所有组件，而不必手动通过props一级级传递。useContext 让您可以更轻松地访问这些值，而不必一直通过组件树传递props

当组件上层最近的`MyContext.Provider`更新时，该 Hook 会触发重新渲染，并使用最新传递给value 

1. 首先，需要使用`React.createContext`创建一个上下文对象![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716259850824-01a30396-1129-44fa-a7a7-0d2eeefa94c6.png)
2. 然后，在某个父组件中，通过`MyContext.Provider` 提供上下文的值![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716259860947-e70d69ac-500f-422b-857d-a5f015eb6c79.png)
3. 最后，在需要访问上下文值的任何子组件中，使用`useContext Hook` 来获取上下文的值![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716259866726-3c3f84d7-a1c6-410f-8dce-f6e32affaa1d.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716259766535-9f7d7fe5-6a0e-4b6a-b223-dbd1f389f7b2.png)

## useReducer
用于管理具有复杂状态逻辑的组件状态。它允许您根据先前的状态和操作来更新状态，并返回新的状态。通常情况下，当状态具有复杂逻辑并且无法简单地通过 useState 来管理时，可以考虑使用 useReducer

useReducer仅仅是useState的一种替代方案

+ 在某些场景下，如果state的处理逻辑比较复杂，我们可以通过useReducer来对其进行拆分
+ 或者这次修改的state需要依赖之前的state时，也可以使用
+ 所以，useReducer 并不能替代Redux

参数：reducer 函数和初始状态

返回值：当前状态和 dispatch 函数。reducer 函数接受当前状态和一个 action，并根据 action 的类型来计算并返回新的状态

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716260353950-8f34e655-a8bc-41dd-b965-882fc0aa9a62.png)

useReducer 通常与 useContext 一起使用，以便更轻松地在整个应用程序中管理全局状态

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716262683550-1493ea07-c704-44ab-9ef5-86558d3b6eda.png)

## useCallback
通常情况下，当子组件接收一个回调函数作为 prop 时，如果这个回调函数是通过匿名函数传递的，那么每次父组件重新渲染时，都会创建一个新的回调函数实例，这可能会导致子组件不必要的重新渲染。通过使用 useCallback，可以确保父组件重新渲染时，子组件不会因为回调函数的变化而重新渲染。

目的是为了进行性能的优化， 通常使用useCallback的目的是不希望子组件进行多次渲染，并不是为了函数进行缓存

参数：回调函数和依赖项数组。依赖项数组用于指定当数组中的值发生变化时，重新创建回调函数实例

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716272820034-06f72528-a48f-4fad-b5ba-b0840d325840.png)

在这个示例中，handleClick 回调函数通过 useCallback 缓存，在父组件重新渲染时不会创建新的实例，从而避免了不必要的子组件重新渲染。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716273403375-496531ba-54c9-4bc9-a185-cbfac853db76.png)

## useMemo
是一个用于优化性能的 Hook，它会在渲染过程中记忆化（memoize）计算结果。这意味着它可以缓存某个值，只有在其依赖项发生变化时才会重新计算。这对于那些计算开销较大，但是不经常改变的值特别有用

传入两个参数：一个函数和一个依赖项数组

返回：一个计算结果

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716273938858-0f7c3491-7b43-4272-8577-5f76f32d7393.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716273998443-631a1bd7-3fa2-416c-a9e5-99faa2fb0174.png)

## useRef
用于在函数组件中存储可变的数据。它返回一个可变的 ref 对象，该对象的`.current` 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期中保持不变

useRef 主要解决了在函数式组件中存储和访问持久化数据的问题。虽然在一些简单的场景下可能感觉不到它的必要性，但在处理一些复杂的逻辑或需要访问 DOM 元素时，useRef 就显得尤为重要了

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716274658780-ae028ec7-0518-48ba-b150-49a9dae06c6e.png)

在这个例子中，我们创建了一个 inputRef 对象，并将其赋值给输入框的 ref 属性。当用户点击按钮时，handleClick 函数会被调用，它通过 inputRef.current 来访问输入框的引用，并分别调用了 focus() 和修改 value 的方法，实现了聚焦到输入框并在输入框中显示消息的效果。

## useImperativeHand
它可以用于在函数组件中自定义暴露给父组件的实例值。通常情况下，React 推荐使用 props 来传递数据和函数给子组件，但有时需要从子组件中暴露一些特定的方法或属性给父组件，这时就可以使用 useImperativeHandle

这个 Hook 主要用于和 forwardRef 一起使用，forwardRef 可以用来获取到子组件的引用，然后 useImperativeHandle 可以用来定制这个引用的暴露形式

当 useImperativeHandle 在子组件中被调用时，它会接收两个参数：第一个参数是父组件传递给子组件的 ref 对象，第二个参数是一个回调函数，在这个回调函数中可以定义需要暴露给父组件的实例方法或属性

```jsx
import React, { useRef, useImperativeHandle, forwardRef } from 'react';

// 子组件
const ChildComponent = forwardRef((props, ref) => {
  const inputRef = useRef(null);

  // 在子组件中暴露多个方法给父组件使用
  useImperativeHandle(ref, () => ({
    focusInput: () => {
      inputRef.current.focus();
    },
    clearInput: () => {
      inputRef.current.value = '';
    },
    setInputValue: (value) => {
      inputRef.current.value = value;
    }
  }));

  return <input ref={inputRef} type="text" />;
});

// 父组件
function ParentComponent() {
  const childRef = useRef(null);

  const handleFocus = () => {
    childRef.current.focusInput();
  };

  const handleClear = () => {
    childRef.current.clearInput();
  };

  const handleSetValue = () => {
    childRef.current.setInputValue('New Value');
  };

  return (
    <div>
      <ChildComponent ref={childRef} />
      <button onClick={handleFocus}>Focus Input</button>
      <button onClick={handleClear}>Clear Input</button>
      <button onClick={handleSetValue}>Set Input Value</button>
    </div>
  );
}
```

### 总结一下 react 中的 ref(重要)
在 React 中，ref 是一个特殊的对象，用于引用组件的实例或 DOM 元素。ref.current 是 ref 对象的一个属性，用于访问被引用组件的实例或 DOM 节点。

1. 当 ref 关联的是一个 DOM 元素时，ref.current 将会是该 DOM 元素的引用，可以直接使用 ref.current 来操作这个 DOM 元素，比如修改其属性、添加事件监听器等。![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716343339859-79600c98-658a-4359-b323-734a407b0537.png)
2. 当 ref 关联的是一个类组件时，ref.current 将会是这个类组件的实例，可以直接访问该组件的实例方法或属性。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716343362905-1c3e6f76-b59e-451f-a7a3-38f347758fd5.png)

3. 当 ref 关联的是一个函数组件时，并且使用了 forwardRef，ref.current 将会是函数组件返回的组件实例。这时可以通过 useImperativeHandle 在函数组件内部定义需要暴露给父组件的方法或属性，从而可以在父组件中通过 ref.current 来访问这些方法或属性。![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716343438677-7dd850ee-6856-40fd-924b-b9378a71bf57.png)

## useLayoutEffect
它类似于 useEffect，但是它会在所有 DOM 变更之后同步调用 effect。这意味着它会在浏览器 layout 之后、painting 之前执行，因此可以保证 DOM 变更后立即同步获取 DOM 元素的布局信息。

与 useEffect 不同，useLayoutEffect 的调用时机更早，它会在 DOM 更新之后立即同步执行，而不是等待浏览器渲染更新完成后再执行

+ useEffect会在渲染的内容更新到DOM上后执行，不会阻塞DOM的更新；
+ useLayoutEffect会在渲染的内容更新到DOM上之前执行，会阻塞DOM的更新

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716275951344-1872a8da-e395-4772-9a40-3b24d9dbf4b6.png)

需要注意的是，由于 useLayoutEffect 是同步执行的，如果操作量很大，可能会导致性能问题。通常情况下，应该优先考虑使用 useEffect，只有在需要在浏览器 layout 之后立即同步执行一些操作时，才使用 useLayoutEffect

假设有一个需求：在页面加载后，根据某个元素的高度来动态调整另一个元素的样式，以确保布局的正确性

```jsx
import React, { useState, useLayoutEffect } from 'react';

function App() {
  const [height, setHeight] = useState(0);
  const [style, setStyle] = useState({});

  useLayoutEffect(() => {
    // 获取元素的高度
    const elementHeight = document.getElementById('content').clientHeight;
    setHeight(elementHeight);

    // 根据高度动态调整样式
    const newStyle = {
      padding: '20px',
      border: '1px solid black',
      minHeight: `${elementHeight}px`,
    };
    setStyle(newStyle);
  }, []); // 仅在组件挂载时执行

  return (
    <div>
      <div id="content" style={{ height: '300px', background: 'lightblue' }}>
        Content Area
      </div>
      <div style={style}>
        <p>Content area height: {height}px</p>
      </div>
    </div>
  );
}

export default App;
```

## 自定义 Hooks
自定义Hook本质上只是一种函数代码逻辑的抽取，严格意义上来说，它本身并不算React的特性

### context 的共享
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716343719963-33132862-9fa0-4a44-8a06-90b1b56f8a1e.png)

### 获取鼠标滚动位置
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716343728275-03341282-a078-43d3-b57c-8e95985436ed.png)

### localStorage 数据存储
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716343742559-237ed81c-c28c-48bb-b968-03a6f3f8b6be.png)

## useSelector
用于从 Redux store 中获取 state。它允许您在函数组件中访问 Redux store 中的 state 数据，而不必编写传统的 mapStateToProps 函数。

1. 获取特定的 state 数据：您可以在 useSelector 的参数函数中选择返回 Redux store 中的任何数据

例如，如果您只想获取 counter 的值，可以直接返回它：![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716344845254-0b6c769e-143e-4b8a-a76f-5263c067b3b7.png)

或者，如果您的 state 是一个对象，您可以使用点表示法来访问特定的属性：![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716344853778-9a22d418-d96d-4783-b61b-2887262c4daa.png)

2. 订阅多个 state：如果您需要订阅多个 state，您可以在 useSelector 的参数函数中返回一个对象，其中包含所需的 state 数据，这样做可以避免在组件重新渲染时创建新的对象引用，从而提高性能。![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716344921080-08e97778-2c52-4c76-820f-d3fa5b156b91.png)
3. 性能优化：默认情况下，useSelector 会比较每次渲染前后的 state，如果发现 state 发生了变化，组件会重新渲染。您可以使用 shallowEqual 来避免不必要的重新渲染。这样只有在 counter 值发生真正的变化时，才会触发重新渲染。![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716344994460-0820312a-2efc-4815-8fbc-849c84988896.png)

## useId
### 前置知识
#### SSR
SSR 是服务器端渲染（Server-Side Rendering）的缩写。

它是一种将网页内容在服务器端动态生成并发送到客户端的技术。

传统的网页应用程序通常是在客户端（浏览器）中使用 JavaScript 动态生成内容，这种方式称为客户端渲染 CSR（Client-Side Rendering）。

相比之下，SSR 在服务器端生成 HTML，然后将其发送到客户端，客户端接收到的是已经包含了内容的完整 HTML 页面。

SSR 的主要优点包括：

+ SEO 优化：搜索引擎可以更容易地抓取和索引服务器端渲染的网页内容，因为页面内容在服务器端就已经存在。
+ 性能优化：SSR 可以减少客户端加载和渲染页面所需的时间，特别是对于首屏加载速度更为重要的场景。
+ 更好的用户体验：由于页面内容在服务器端就已经渲染完毕，用户可以更快地看到页面内容，降低了等待时间，提升了用户体验。
+ 利于 SEO 和社交分享：因为搜索引擎和社交分享服务通常会抓取和索引 HTML 内容，SSR 可以更好地支持 SEO 和社交分享功能。

早期的服务端渲染包括PHP、JSP、ASP等方式，但是在目前前后端分离的开发模式下，前端开发人员不太可能再去学习PHP、JSP等技术来开发网页，不过我们可以借助于Node来帮助我们执行JavaScript代码，提前完成页面的渲染。

同构（ 一套代码既可以在服务端运行又可以在客户端运行 ）是一种SSR的形态，是现代SSR的一种表现形式。当用户发出请求时，先在服务器通过SSR渲染出首页的内容。但是对应的代码同样可以在客户端被执行。执行的目的包括事件绑定等，以及其他页面切换时也可以在客户端被渲染

#### Hydration
hydration(水合) ：在客户端重新创建静态 HTML 内容（SSR 生成的），并与客户端的 JavaScript 代码进行“绑定”，使得之后的交互和状态管理可以顺利进行

使用 hydration 的好处包括：

+ 加速首屏加载：由于服务器端渲染已经生成了初始的 HTML 内容，客户端只需要重新创建并“水合”这部分内容，而不必等待整个页面的重新渲染，因此可以更快地加载并显示内容
+ 保留 SEO 优势：使用服务器端渲染生成的静态 HTML 内容有利于搜索引擎抓取和索引，而hydration 则可以让客户端应用程序在加载后继续保持 SEO 优势。
+ 渐进增强：对于不支持 JavaScript 的用户代理，他们仍然可以看到通过服务器端渲染生成的静态 HTML 内容，这提供了一种渐进增强的体验

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716348176578-7472f944-be05-498b-879c-903339d52339.png)

### useId
useId 是一个用于生成横跨服务端和客户端的稳定的唯一 ID 的同时避免 hydration 不匹配的 hook

+ useId是用于react的同构应用开发的，前端的SPA页面并不需要使用它
+ useId可以保证应用程序在客户端和服务器端生成唯一的ID，这样可以有效的避免通过一些手段生成的id不一致，造成hydration mismatch 错误![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716362836928-931eded5-4224-482f-8f6f-64810f5c8c7a.png)

## useTransition
 返回一个状态值表示过渡任务的等待状态，以及一个启动该过渡任务的函数，它其实在告诉react对于某部分任务的更新优先级较低，可以稍后进行更新

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716363224380-7437f27e-8bc5-437d-8778-cd3fc03c62cd.png)

## useDeferredValue
useDeferredValue 接受一个值，并返回该值的新副本，该副本将推迟到更紧急地更新之后。

和 useTransition的作用是一样的效果，可以让更新延迟

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716363419146-cdce0d5e-f65d-48d6-90c0-c5d8c0820d2a.png)

# 项目实战：Airbnb
## 项目规范
1. 文件夹、文件名称统一小写、多个单词以连接符（-）连接
2. JavaScript变量名称采用小驼峰标识，常量全部使用大写字母，组件采用大驼峰
3. CSS采用普通CSS和styled-component结合来编写（全局采用普通CSS、局部采用styled-component）
4. 整个项目不再使用class组件，统一使用函数式组件，并且全面拥抱Hooks
5. 所有的函数式组件，为了避免不必要的渲染，全部使用memo进行包裹
6. 组件内部的状态，使用useState、useReducer；业务数据全部放在redux中管理
7. 函数组件内部基本按照如下顺序编写代码：
+ 组件内部state管理
+ redux的hooks代码
+ 其他hooks相关代码（比如自定义hooks）
+ 其他逻辑代码
+ 返回JSX代码
8. redux代码规范如下：
+ redux目前我们学习了两种模式，在项目实战中尽量两个都用起来，都需要掌握；
+ 每个模块有自己独立的reducer或者slice，之后合并在一起；
+ redux中会存在共享的状态、从服务器获取到的数据状态；
9. 网络请求采用axios
+ 对axios进行二次封装
+ 所有的模块请求会放到一个请求文件中单独管理
10. 项目使用AntDesign、MUI（Material UI）
+ 爱彼迎本身的设计风格更多偏向于Material UI，但是课程中也会尽量讲到AntDesign的使用方法
+ 项目中某些AntDesign、MUI中的组件会被拿过来使用
+ 但是多部分组件还是自己进行编写、封装、实现

## 创建项目与配置
创建项目：`create-react-app` 

项目配置：

+ 配置 icon
+ 配置标题
+ 配置 jsconfig.json

### 项目结构
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716537728429-82ce8786-c783-4aeb-af09-95f83d104695.png)

### 设置别名
在 webpack 中给 src 设置别名@

使用 craco

    1. `npm install @craco/craco@alpha -D`
    2. 创建 craco.config.js 文件

![craco.config.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716538394398-06c5150e-cd59-4c65-bb8a-6a3a2e2d5a46.png)![package.json](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716538581063-ecc1a78d-3770-40c1-aa9b-91af791096d6.png)

    3. npm run start

### 配置 less
安装`npm i craco-less@2.1.0-alpha.0`

```javascript
const path=require('path')
const resolve=pathname=>path.resolve(__dirname,pathname)
const CracoLessPlugin = require('craco-less');

module.exports={
	plugins: [
		{
			plugin: CracoLessPlugin,
			options: {
				lessLoaderOptions: {
					lessOptions: {
						modifyVars: { '@primary-color': '#1DA57A' },
						javascriptEnabled: true,
					},
				},
			},
		},
	],
	//webpack
	webpack:{
		alias:{
			// "@":path.resolve(__dirname,"src")
			"@":resolve("src"),
			"components":resolve("src/components"),
			"utils":resolve("src/utils")
		}
	}
}
```

### 重置 css 样式
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716543680061-595cb0ad-c625-41be-8222-c4266a9ebf3e.png)

### 配置 Router
安装：`npm install react-router-dom`

![配置HashRouter](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716541585410-3a7a182b-4520-4951-90a9-5c47079d3a2c.png)

![router/index.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716543105917-1ebd910a-d8cc-42d0-a183-322fdf254604.png)

![在App中使用](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716543139715-bb79cc7d-d29a-496b-b9bc-e65fd1258540.png)

![设置加载效果](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716543393417-ae3b3225-0168-4ef7-8501-21b961689342.png)

### 配置 Redux
1. 普通方式：使用率高
2. @reduxjs/toolkit：未来的趋势

#### toolkit
安装： `npm install @reduxjs/toolkit react-redux`

![为了练习，所以采用了两种redux方法，一个rtk一个普通](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716773010434-ac32bcf9-8bba-431b-86da-a6085b83c00f.png)

![rtk写法](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716773001286-a31fe0c6-b2f5-48f1-a06b-74ca3f449a79.png)

![rtk](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716773062084-acec525b-ab43-4714-b352-1cdf3d9dd926.png)

![普通](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716773073591-e507405b-ba8e-4b6b-b21d-a3afd2c80b26.png)

### 配置 axios
安装： `npm install axios`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716773908680-532c6880-2a96-4369-ae44-7ca2e2d5b9de.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716773883866-d0d482bc-7ba2-4688-8493-3a6a4448b1ca.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716773898871-b693dbeb-ee63-4732-a3aa-e4214d6d5f35.png)

## 项目步骤
1. 创建项目
2. 项目配置
3. AppHeader：定义在 components 中，样式使用 styled-components(css in js)
4. AppFooter

## 问题汇总
### Theme
常用的主题色或者字体大小等，使用 styled-components 提供的方法，统一命名并使用

也可以使用 less 语法，`@primary-color=#fff;`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716799307981-6e5d3eaf-99b3-4c57-a1c1-566c3c6a5f3e.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716799316067-66f5f827-b6e6-4cca-83ce-2aea3cc8612f.png)

![在index.js中使用ThemeProvider](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716799358836-e7756cc8-6de2-44fb-9c5d-d0bedd04b8c2.png)

![在style.js中使用主题色](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716799404632-bf82b02a-f1d6-42e0-880b-36d1f7d381e2.png)

### 账号信息弹窗
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716864515737-23d6d7fc-a0e9-476e-b044-f59fde100961.png)

显示：定义了 profileClickHandle 函数，点击时会设置 showPanel 属性为 true

隐藏：

+ 监听 window 的点击，点击时会将 showPanel 改为 false
+ 因为只执行一次，所以放在了副作用代码中，用 [] 空数组表示执行一次(20h)
+ 同时返回一个回调函数用来取消监听(17h)
+ 因为存在冒泡，所以点击不显示
    - 阻止冒泡：stopPropagation()
    - 将捕获设置为 true，即代码 16h 和 18h(18h 可以不写)

```jsx
import React, {memo, useEffect, useState} from 'react';
import {RightWrapper} from "@/components/app-header/c-cpns/header-right/style";
import IconGlobal from "@/assets/svg/icon-global";
import IconProfileMenu from "@/assets/svg/icon-profile-menu";
import IconProfileAvatar from "@/assets/svg/icon-profile-avatar";

const HeaderRight = memo(() => {
	// 定义组件内部的状态
	const [showPanel,setShowPanel]=useState(false)

	// 副作用代码
	useEffect(()=>{
		function windowHandleClick(){
			setShowPanel(false)
		}
		window.addEventListener("click",windowHandleClick,true)
		return ()=>{
			window.removeEventListener("click",windowHandleClick,true)
		}
	},[])
	// 事件处理函数
	function profileClickHandle(){
		setShowPanel(true)
	}

	return (
		<RightWrapper>
			<div className="btns">
				<span className="btn">登录</span>
				<span className="btn">注册</span>
				<span className="btn"><IconGlobal/></span>
			</div>
			<div className="profile" onClick={profileClickHandle}>
				<IconProfileMenu/>
				<IconProfileAvatar/>
				{showPanel &&(
			<div className="panel">
				<div className="top">
					<div className="item register">注册</div>
					<div className="item login">登录</div>
				</div>
				<div className="bottom">
					<div className="item czfy">出租房源</div>
					<div className="item kzty">开展体验</div>
					<div className="item help">帮助</div>
				</div>
			</div>
		)
				}
			</div>
		</RightWrapper>
	);
});

export default HeaderRight;
```

### webpack 图片引入
在 webpack 中，不管是 url 或者 src，你输入路径，他都识别成字符串，而不是根据路径找图片

所以需要将图片引入文档，再赋值给 url 或 src

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1716866389318-545d7799-4c13-4f51-8e7e-4cde54251fbe.png)

center/cover 指的是`background-size: cover;` 和 `background-position: center;`

### 在网页中发送网络请求
注意老师讲的写法已经被最新版的 toolkit 移除(store/modules/entire/home.js)，所以采用的是 ai 修改后的最新的写法

下面的代码都是发送请求用的，具体数据在组件中是怎么用的，这里不多赘述

步骤

1. 在services/modules/home.js 中，为请求添加路径，并发送请求，将这些操作封装在函数中 (getHomeGoodPriceData)
2. 在 store/modules/entire/home.js 中，设置 redux 的 state 和 reducer(包括 extraReducers)，使用 createAsyncThunk 定义了 fetchHomeDataAction 异步函数，用来发送请求、获取数据保存在 res 中；在 extraReducers 中，当fetchHomeDataAction 的状态变成 fullfilled 时，将获取到的数据 res 传递给 state.goodPriceInfo
3. 在 home/index 中，使用 dispatch(fetchHomeDataAction) 发送网络请求，使用 useEffect发送网络请求(每次浅拷贝发生时都执行)，使用 useSelect 获取请求来的数据



![store/modules/entire/home.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717034658648-606777cf-032b-4257-8ca1-0a0f8e344c58.png)

![services/modules/home.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717034476390-c8c42fec-75db-4c6a-8238-ef5a966a98da.png)

![home/index.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717034338774-3ffdcf20-67a3-4da1-9258-d01b9923525f.png)

### 新的 UI 库
#### Material UI
安装

+ `npm install @mui/material @emotion/react @emotion/styled`（emotion 样式库）
+ `npm install @mui/material @mui/styled-engine-sc styled-components`（styled-components 样式库）

后者在使用时还有有一点问题，推荐使用 emotion

#### AntDesign UI
安装`npm install antd`

### 一个页面中发送多个网络请求
不再使用 async await，直接 dispatch 请求

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717140833655-d12e7508-3aa7-4bba-bb41-dbfa9bd595b8.png)

### home 页面整合
将展示房间的 2 个组件整合到 1 个组件中

![home/index.jsx](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717142863381-d9f1cdb0-092b-4160-8e10-3a71050dc166.png)

![home-section-v1/index.jsx](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717142880503-8963e43b-4700-4db7-a1a5-a79f2fb69676.png)



### useState(初始值)显示问题
![??用来防止没取到值](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717587954285-97e0b795-be1e-43e0-a01a-8b9961479463.png)

useState(初始值)：初始值只有在组件第一次被渲染时才是有效的

意思就是在请求的数据到达之前，infoDate 是空的，此时 initialName 就是空的，所以不显示内容；当数据到达时，infoDate 发生改变，但是此时 useState 不会重新渲染，仍然是空

解决方式：如果 InfoDate 为空，则该组件不渲染

![!!用来转换成boolean](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717588692650-7cbdd728-c059-4d86-bded-036a41887003.png)

![home.jsx](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717588678925-61187b90-809f-4de6-9570-69f1a4fb2fb2.png)

所以之后的组件都可以这样做，用来性能优化

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717589192610-4181f3e0-479f-4796-ad6d-c54b8d6a698c.png)

其实还可以在组件中使用 `useEffect(()=>{setName(xxx)}，[infoDate])`，但是会导致组件被渲染 3 次才显示，1-初始化，2-infoDate 有值，3-useEffect 渲染

### SectionFooter 显示问题
默认显示“显示更多”，如果传进来 name 参数，则显示“显示更多name房源”，且文本颜色也不同![SectionFooter/index.jsx](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717592385629-883ebfae-0167-4900-af85-03ff249f16b0.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717592478903-695f486a-80c3-477e-8aad-78de7ff85a44.png)

### Tabbar 滚动问题
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717657948227-f7d6715d-f642-4f59-aa6a-3e812b9fa3af.png)

如果使用 overflow-x:auto，则一个地址块显示不完整，所以决定使用 transform：translate 位移来实现

事实上，页面中还有很多类似的模块需要滚动，所以需要写一个可以复用的组件

```jsx
import React, { memo, useEffect, useRef, useState } from "react";
import { ScrollWrapper } from "@/base-ui/scroll-view/style";
import IconArrowLeft from "@/assets/svg/icon-arrow-left";
import IconArrowRight from "@/assets/svg/icon-arrow-right";

const ScrollView = memo((props) => {
  //内部状态
  const [showRight, setShowRight] = useState(false);
  const [showLeft, setShowLeft] = useState(false);
  const [posIndex, setPosIndex] = useState(0);
  const totalDistanceRef = useRef();
  //组件渲染完毕，是否显示右边按钮
  const scrollContentRef = useRef();
  useEffect(() => {
    const scrollWidth = scrollContentRef.current.scrollWidth; //总宽度
    const clientWidth = scrollContentRef.current.clientWidth; //目前可以显示的宽度
    const totalDistance = scrollWidth - clientWidth; //需要隐藏的宽度
    totalDistanceRef.current = totalDistance;
    setShowRight(totalDistance > 0);
  }, [props.children]);

  //事件处理
  function controlClickHandle(isRight) {
    const newIndex = isRight ? posIndex + 1 : posIndex - 1;
    const newEl = scrollContentRef.current.children[newIndex];
    const newOffsetLeft = newEl.offsetLeft;
    scrollContentRef.current.style.transform = `translate(-${newOffsetLeft}px)`;
    setPosIndex(newIndex);
    //是否显示右边按钮
    setShowRight(totalDistanceRef.current > newOffsetLeft);
    setShowLeft(newOffsetLeft > 0);
  }

  return (
    <ScrollWrapper>
      {showLeft && (
        <div onClick={(e) => controlClickHandle(false)}>
          <IconArrowLeft />
        </div>
      )}
      {showRight && (
        <div onClick={(e) => controlClickHandle(true)}>
          <IconArrowRight />
        </div>
      )}
      <div className="scroll-content" ref={scrollContentRef}>
        {props.children}
      </div>
    </ScrollWrapper>
  );
});

ScrollView.propTypes = {};

export default ScrollView;

```

```javascript
import styled from "styled-components";

export const ScrollWrapper = styled.div`
  position: relative;
  padding: 8px 0;

  .scroll {
    overflow: hidden;

    .scroll-content {
      display: flex;

      transition: transform 200ms ease;
    }
  }

  .control {
    position: absolute;
    z-index: 9;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 28px;
    height: 28px;
    border-radius: 50%;
    text-align: center;
    border-width: 2px;
    border-style: solid;
    border-color: #fff;
    background: #fff;
    box-shadow: 0px 1px 1px 1px rgba(0, 0, 0, 0.14);
    cursor: pointer;

    &.left {
      left: 0;
      top: 50%;
      transform: translate(-50%, -50%);
    }

    &.right {
      right: 0;
      top: 50%;
      transform: translate(50%, -50%);
    }
  }
`;
```

只需要让其包裹住需要使用的组件内容即可

![SectionTabs/index.jsx](https://cdn.nlark.com/yuque/0/2024/png/29327577/1717675352622-81879486-0300-4497-b434-27718822411d.png)

### 筛选的选定与取消
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718155313251-f592556e-dd8e-4e3d-a07e-49cc0bae51cc.png)

点击按钮会添加 active 属性，再次点击会取消选中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718155387475-baca0335-f3ee-46bb-a534-77d06248460e.png)

### Redux&RTK----重点
#### Redux
entire 页面采用了传统的 redux

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718178781095-3a93ce81-8d4c-4563-b58c-77aa01f4365b.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718178915230-477c3f2e-84dc-4326-99fe-1ae1aea36c8f.png)

下面的代码不仅包含 redux，还包含网络请求

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718179964123-3e40d07c-bdd3-4902-9573-a05009d42921.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718180003558-fc32df91-4f60-494f-b533-9863ddd2eef2.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718180036352-d0e3b18f-9850-444d-b9fe-937ee30b4597.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718180046835-ee1185d4-da0c-4cc3-b5f8-b40bd699d530.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718180371125-5c8a650b-644d-4b9c-bdab-0522391d21ad.png)

#### RTK
home 页面使用了 RTK

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718178772656-1d9b189f-908c-493d-b0ca-89b57f4bf8b9.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718178912624-feac19b4-e506-4fa7-8e52-dc07f9bbd09b.png)

下面这个代码中，既包含 rtk，还包含了 home 页面发送的网络请求

```javascript
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";
import {
  getHomeDiscountData,
  getHomeGoodPriceData,
  getHomeHighScoreData,
  getHomeHotRecommendDate,
  getHomeLongforDate,
  getHomePlusDate,
} from "@/services";

export const fetchHomeDataAction = createAsyncThunk(
  "fetchData",
  (payload, { dispatch }) => {
    getHomeGoodPriceData().then((res) => {
      dispatch(changeGoodPriceInfoAction(res));
    });
    getHomeHighScoreData().then((res) => {
      dispatch(changeHighScoreInfoAction(res));
    });
    getHomeDiscountData().then((res) => {
      dispatch(changeDiscountInfoAction(res));
    });
    getHomeHotRecommendDate().then((res) => {
      dispatch(changeRecommendInfoAction(res));
    });
    getHomeLongforDate().then((res) => {
      dispatch(changeLongforInfoAction(res));
    });
    getHomePlusDate().then((res) => {
      dispatch(changePlusInfoAction(res));
    });
  },
);

const homeSlice = createSlice({
  name: "home",
  initialState: {
    goodPriceInfo: {},
    highScoreInfo: {},
    discountInfo: {},
    recommendInfo: {},
    longforInfo: {},
    plusInfo: {},
  },
  reducers: {
    changeGoodPriceInfoAction(state, { payload }) {
      state.goodPriceInfo = payload;
    },
    changeHighScoreInfoAction(state, { payload }) {
      state.highScoreInfo = payload;
    },
    changeDiscountInfoAction(state, { payload }) {
      state.discountInfo = payload;
    },
    changeRecommendInfoAction(state, { payload }) {
      state.recommendInfo = payload;
    },
    changeLongforInfoAction(state, { payload }) {
      state.longforInfo = payload;
    },
    changePlusInfoAction(state, { payload }) {
      state.plusInfo = payload;
    },
  },
});

export const {
  changeGoodPriceInfoAction,
  changeHighScoreInfoAction,
  changeDiscountInfoAction,
  changeRecommendInfoAction,
  changeLongforInfoAction,
  changePlusInfoAction,
} = homeSlice.actions;
export default homeSlice.reducer;
```

下图中是封装的网络请求，在上面代码中有使用到

![services/modules/home.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718178962158-030c93a7-675d-4e8d-abb3-9ac726817630.png)

### 分页
使用了 mui 组件库，颜色主题有两种解决方案

1. 修改默认 primary 的颜色，即自定义主题
2. 使用 css 覆盖掉原来的颜色

```javascript
import React, { memo } from "react";
import { PaginationWrapper } from "@/views/entire/cpns/entire-pagination/style";
import { Pagination } from "@mui/material";
import { useDispatch, useSelector } from "react-redux";
import { fetchRoomListAction } from "@/store/modules/entire/createActions";

const EntirePagination = memo((props) => {
  const { totalCount, currentPage, roomList } = useSelector((state) => ({
    totalCount: state.entire.totalCount,
    currentPage: state.entire.currentPage,
    roomList: state.entire.roomList,
  }));
  const startCount = currentPage * 20 + 1;
  const endCount = (currentPage + 1) * 20;
  const totalPage = Math.ceil(totalCount / 20); //向上取整
  //点击按钮转换页面-事件逻辑
  const dispatch = useDispatch();

  function pageChangeHandle(event, pageCount) {
    dispatch(fetchRoomListAction(pageCount - 1));
  }

  return (
    <PaginationWrapper>
      {!!roomList.length && (
        <div className="info">
          <Pagination count={totalPage} onChange={pageChangeHandle} />
          <div className="desc">
            第{startCount}-{endCount}个房源，共超过{totalCount}个
          </div>
        </div>
      )}
    </PaginationWrapper>
  );
});

EntirePagination.propTypes = {};

export default EntirePagination;
```

对 redux 的代码进行了修改

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718185423118-6f86b5af-02b4-4338-89d7-f09d283c712a.png)

### 图片轮播图底部指示器
没有第三方组件可以用，一般都是自己手写一个 demo，然后多个项目通用



