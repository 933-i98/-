# 1.this指向
## 简介
+ this 指向的始终是一个对象
+ 函数在调用时，JavaScript会默认给this绑定一个值
+ this的绑定和定义的位置（编写的位置）没有关系
+ this的绑定和调用方式以及调用的位置有关系
+ this是在运行时被绑定的；

## 绑定规则
### 默认绑定
 什么情况下使用默认绑定呢？独立函数调用。

独立的函数调用可以理解成函数没有被绑定到某个对象上进行调用

下面是几个例子

![独立调用](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725328018053-4b51b0e5-56a8-45c1-8354-0db805f07d1a.png)![对象内函数独立调用](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725328698152-541f12f2-d3de-438e-906f-6fec76444f21.png)![高阶函数独立调用：当test函数被调用，obj.foo作为参数传递给test，obj.foo的调用上下文不再是obj对象。这是因为fn()在test函数内部被调用，而fn是一个普通的参数，它没有绑定到obj对象。](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725328909368-0acdd445-2c45-40a9-b177-143d6bf8e9f6.png)

### 隐式绑定
它的调用位置中，是通过某个对象发起的函数调用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725329148342-6557d337-a091-45a2-ba8e-6576372205a2.png)![实际上是通过obj2对象访问obj1对象，然后调用obj1对象上的foo方法。](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725329171925-105e7575-98db-4d9e-b89a-0f80be69fab3.png)

### 显式绑定
隐式绑定有一个前提条件：必须在调用的对象内部有一个对函数的引用（比如一个属性），如果没有这样的引用，在进行调用时，会报找不到该函数的错误，正是通过这个引用，间接的将this绑定到了这个对象上

如果不希望在对象内部包含这个函数的引用，同时又希望在这个对象上进行强制调用，那就需要显式绑定

显式绑定就是函数使用 apply/call/bind 绑定

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725330849606-56933936-3818-4940-ba3f-243fdbb3d8c9.png)

123 会被包装成 Number 对象

undefined 会指向 window

区分一下 apply/call/bind

+ func.call(thisArg, arg1, arg2, ...)
    - call 方法接受多个参数，第一个参数是 this 的值，后面的参数是传递给函数的实际参数
+ func.apply(thisArg, [arg1, arg2, ...])
    - apply 方法接受两个参数，第一个参数是 this 的值，第二个参数是一个包含所有传递给函数的参数的数组
+ func.bind(thisArg, arg1, arg2, ...)
    - bind 返回一个新的函数，该函数的 this 被绑定到 thisArg，并且可以在调用时传递额外的参数
    - 与 call 和 apply 不同，bind 不会立即执行函数，而是返回一个可以稍后执行的新函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725331317641-073cf6ca-2fde-40f1-9b7c-b181a03a938a.png)

### 内置函数的绑定思考
当内置函数或第三方库的函数要求传入另一个函数时，this 的绑定通常依赖于函数被调用的上下文

+ 回调函数的 this：如果内置函数或第三方库函数内部调用了传入的回调函数，回调函数的 this 通常会被绑定到内置函数或库函数的上下文。这意味着 this 可能会指向内置函数或库函数自身，或者是它们的某个属性
+ 方法调用：如果回调函数是作为对象的方法传入的，this 会绑定到该对象。举个例子，如果传入的是对象的方法，并且该对象调用了回调函数，那么 this 会指向那个对象
+ 箭头函数：如果回调函数是箭头函数，那么 this 会继承自外部作用域，而不会被重新绑定。箭头函数没有自己的 this，它的 this 来自于定义时的上下文



![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725332681311-05677884-5540-44bd-9af8-a62574414a8c.png)

forEach 方法内部调用了回调函数，但是在这个回调函数内部，this 的值并不是 obj

因为在 forEach 方法中，回调函数是作为普通函数调用的，所以在非严格模式下，this 指向全局对象（例如浏览器中的 window 对象），在严格模式下，this 是 undefined

在浏览器中 window.name 默认是空字符串，所以 this.name 是空

如果希望回调函数的 this 指向 obj，可以使用箭头函数，箭头函数会从它的词法作用域中继承 this

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725331856038-c089b98c-1f14-4b02-9b6f-b744ca98e5d2.png)

箭头函数不会有自己的 this，而是继承 printName 方法的 this，即 obj

### new 绑定
使用new关键字来调用函数会执行如下的操作：

1. 创建一个新的空对象，这个对象会继承构造函数的原型
2. 构造函数内部的 this 指向新创建的对象
3. 执行构造函数代码：初始化新对象的属性和方法
4. 返回新对象

new 关键字使得构造函数中的 this 绑定到一个新创建的实例对象上

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725329841542-a56a49d9-04cb-407a-b45b-e9975747aaf8.png)

### 优先级
1. 默认规则的优先级最低
2. 显示绑定>隐式绑定
3. new绑定>隐式绑定
4. new绑定>bind
+ new绑定和call、apply是不允许同时使用的，所以不存在谁的优先级更高
+ new绑定可以和bind一起使用，new绑定优先级更高

## this 规则之外
### 忽略显示绑定
如果在显示绑定中传入一个null或者undefined，那么这个显示绑定会被忽略，使用默认规则

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725344241688-24288db1-f141-4d04-86a3-982cf60b42af.png)

### 间接函数引用
创建一个函数的间接引用，这种情况使用默认绑定规则

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725433619151-6f88e2a0-8ed0-4949-873c-840384aa4126.png)

## 箭头函数
关于参数和函数体的几种情况

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725434764787-17973188-ae53-48c0-bf49-d1a44212db56.png)

特点

+ this：箭头函数的一个关键特性是它不绑定自己的 this，而是从外部上下文中继承 this
+ arguments：箭头函数没有自己的 arguments 对象，可以使用比如 rest 参数 ...args
    - `const sum = (...numbers) => { return numbers.reduce((total, num) => total + num, 0); };`
+ 构造方法：箭头函数不能用作构造函数，不可以和 new 一起用，因为没有原型

> arguments 对象是一个类数组对象，它包含了传递给函数的所有参数，在传统函数中，可以通过 arguments 对象访问函数的参数，而不需要明确指定参数列表
>
> ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725434129237-2698e3db-89c9-41c5-8620-0349d560d283.png)
>

## 面试题-！！！
箭头函数 this 查找顺序：上层作用域->全局->报错

注意：函数可能是由对象包裹，但是对象是没有作用域概念的，所以对象不能算是上层作用域

![题一](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725436409182-6de399b4-365a-403b-ad64-d561d0fad419.png)

![题二](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725436235246-7fc8c0f4-0639-4458-9dcb-794f5892089a.png)![题二](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725436888038-9241ec89-5e66-432e-a25d-e3107dc1fb79.png)

![题三](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725436936805-6c36a1bc-4c90-4945-a586-fe3178e8dc2d.png)![题三](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725437439940-8c1fc975-870b-47d1-9c51-73402911a285.png)

![题四](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725437469010-3d69c7d5-f824-4c39-affc-c75d87575c43.png)![题四](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725437968662-aa4df015-a1d2-411f-8988-bb6166e4f684.png)

# 2. 浏览器的渲染原理
## 网页解析过程
域名-----DNS解析---->ip地址---->服务器---->index.html---->下载css和js文件---->加载css，js

## 浏览器内核
浏览器内核指的是浏览器的排版引擎。也称为浏览器引擎、页面渲染引擎或样版引擎

一个网页下载下来后，就是由渲染引擎来帮助解析的

+ Trident （ 三叉戟）：IE、360安全浏览器、搜狗高速浏览器、百度浏览器、UC浏览器
+ Gecko（壁虎）：Mozilla Firefox
+ Presto（急板乐曲）-> Blink （眨眼）：Opera
+ Webkit ：Safari、360极速浏览器、搜狗高速浏览器、移动端浏览器（Android、iOS）
+ Webkit-> Blink ：Google Chrome，Edge

## 渲染过程
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611163028-d65c5f65-139b-4a25-94c5-466e4cd72ef8.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611163334-923ab11b-265c-46a0-97c4-91087611163f.png)

### 过程解析
1. html解析

解析html文件，构架dom tree



2. 生成css规则

下载css文件-解析-生成规则树(cssom tree)，下载时不会影响dom树的建立



3. 构建render tree(渲染树)
+ 当dom和cssom树构建完成后，结合起来构建render tree
+ Iink元素不会阻塞DOM Tree的构建过程，但是会阻塞Render Tree的构建过程，因为Render Tree在构建时，需要对应的CSSOM Tree
+ Render Tree和DOM Tree并不是一一对应的关系，比如对于display为none的元素，压根不会出现在render tree中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725442158294-7042fe41-df78-48ec-8d38-0970e8a30f1f.png)

4. 布局和绘制：
+ Lay out：在渲染树上运行布局(Layout)以计算每个节点的几何体。渲染树会表示显示哪些节点以及其他样式，但是不表示每个节点的尺寸、位置等信息。布局是确定呈现树中所有节点的宽度、高度和位置信息。
+ Paint：在绘制阶段，浏览器将布局阶段计算的每个 frame转为屏幕上实际的像素点。包括将元素的可见部分进行绘制，比如文本、颜色、边框、阴影、替换元素（比如ig)



5. 回流和重绘：

reflow： 布局完成之后对节点的大小、位置修改重新计算称之为回流

回流的契机

+ DOM结构发生改变（添加新的节点或者移除节点）
+ 改变了布局（修改了width、height、padding、font-size等值）
+ 窗口resize(修改了窗口的尺寸等)
+ 调用getComputedStyle方法获取尺寸、位置信息



repaint： 第一次渲染内容称之为绘制，之后重新渲染称之为重绘

重绘的契机：比如修改背景色、文字颜色、边框颜色、样式等

#### 如何尽量避免回流
回流一定会引起重绘，所以回流是一件很消耗性能的事

+ 修改样式尽量一次性完成。比如将需要修改的统一写在一个class里，然后给对应元素添加一个新class
+ 尽量避免频繁的操作dom
+ 尽量避免通过getComputedStyle获取尺寸、位置等信息
+ 对某些元素使用position的absolute或者fixed

### 合成composite
绘制时，会将布局后的元素绘制到多个合成图层中，这是浏览器的一种优化手段。

默认情况下，标准流的内容都是被绘制在同一个图层(layer)中的，而一些特殊的属性，会创建一个新的合成层(CompositingLayer)，并且新的图层可以利用gpu来加速绘制

分层确实可以提高性能，但是它以内存管理为代价，因此不应作为 web 性能优化策略的一部分过度使用。

常见的可以形成新的合成层的：3D transforms；video、canvas、iframe；opacity动画转换时；position:fixed

### script元素和页面解析的关系
浏览器在解析HTML的过程中，遇到了script元素是不能继续构建DOM树的。它会停止继续构建，首先下载JavaScript代码，并且执行JavaScript的脚本。只有等到javaScript脚本执行结束后，才会继续解析HTML,构建DOM树

这是因为javaScript的作用之一就是操作DOM，并且可以修改DOM。如果我们等到DOM树构建完成并且渲染再执行javaScript，会造成严重的回流和重绘，影响页面的性能。所以会在遇到script元素时，优先下载和执行JavaScript代码，再继续构建DOM树。

带来的问题：在目前的开发模式中（比如Vue、React)，脚本往往比HTML页面更"重”，处理时间需要更长。所以会造成页面的解析阻塞，在脚本下载、执行完成之前，用户在界面上什么都看不到。

解决办法：script元素提供了两个属性（attribute）：defer和async

+ defer：需要在文档解析后操作DOM的JavaScript代码，并且对多个script文件有顺序要求的
+ async：通常用于独立的脚本，对其他脚本，甚至DOM没有依赖的

#### defer
defer 属性告诉浏览器不要等待脚本下载，而继续解析HTML，构建DOM Tree

+ 脚本会由浏览器来进行下载，但是不会阻塞DOM Tree的构建过程
+ 如果脚本提前下载好了，它会等待DOM Tree构建完成，在DOMContentLoaded事件之前先执行defer中的代码

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725442901723-c136b1df-c237-44b7-892f-ceff6ff56735.png)

+ 多个带defer的脚本是可以保持正确的顺序执行的

用法：推荐放在head里，`<script defer src='……'></script>`

注意： defer仅适用于外部脚本，script默认内容会被忽略

#### async
async特性与defer有些类似，它也能够让脚本不阻塞页面

async让一个脚本完全独立

+ 浏览器不会因async脚本而阻塞（与defer类似）
+ async脚本不能保证顺序，它是独立下载、独立运行，不会等待其他脚本
+ async不会能保证在DOMContentLoaded之前或者之后执行

用法：`<script async src='……'></script>`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725443030848-3480d3c8-678e-44ca-a374-cdc5aa6a81ec.png)

# 3. 深入javascript的运行
## V8 引擎-见小余
浏览器内核由两部分组成，以 webkit 为例

+ WebCore：负责解析html，布局，渲染
+ JavaScriptCore：解析执行js代码

V8 是一个由 Google 开发的高性能 JavaScript 引擎，它是 Chrome 浏览器和 Node.js 的核心组成部分。V8 的主要作用是将 JavaScript 代码编译成机器代码，从而提升 JavaScript 的执行速度 



![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611164200-32cedc3b-92a6-4c47-8fbb-87316d37402b.png)

### V8引擎的架构
1. Parse模块会将 JavaScript代码转换成AST(抽象语法树)，这是因为解释器并不直接认识JavaScript代码；如果函数没有被调用，那么是不会被转换成AST的；
2. Ignition是一个解释器，会将AST转换成ByteCode(字节码)，同时会收集TurboFan优化所需要的信息（比如函数参数的类型信息，有了类型才能进行真实的运算）；如果函数只调用一次，Ignition会解释执行ByteCode;
3. TurboFan：是一个编译器，可以将字节码编译为CPU可以直接执行的机器码；如果一个函数被多次调用，那么就会被标记为热点函数，那么就会经过TuboFan转换成优化的机器码，提高代码的执行性能；但是，机器码实际上也会被还原为ByteCode，这是因为如果后续执行函数的过程中，类型发生了变化（比如sum函数原来执行的是number类型，后来执行变成了string类型)，之前优化的机器码并不能正确的处理运算，就会逆向的转换成字节码；

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611165269-03767808-56d9-45ca-b467-75f144b2523b.png)



词法分析：将字符串序列转换成token序列(记号化)，scanner是扫描器(词法分析器)

语法分析：把token序列转换成node序列，parser是语法分析器

## JavaScript代码执行原理-见小余
即ECMAScript，以下是es3和es5相关知识

+ 通过ECMAScript3中的概念学习JavaScript执行原理、作用域、作用域链、闭包等概念
+ 通过ECMAScript5中的概念学习块级作用域、let、const等概念

### 全局执行过程
1. 在代码运行前，先在堆内存中创建一个GO对象
2. 在ECS中，给每个全局变量创造若干个 EC，并给每个 EC 定义一个VO对象，GEC 的 VO对象指向GO对象
3. 代码开始执行，将GO对象中的变量名赋值，把函数名指向函数对象

#### 初始化全局对象
Js引擎在执行代码前，会在堆内存中创建一个全局对象Global Object（GO）

+ 该对象可被所有作用域访问
+ GO对象中包含了定义的变量名函数名等，但值都是undefined，还包括一些自带的对象如data对象
+ 其中还有一个window属性指向自己

#### 执行上下文
js引擎内部有一个执行上下文栈(ECS)，它是用于执行代码的调用栈

他执行的是全局的代码块：全局的代码块为了执行会构建一个全局执行上下文GEC，GEC会被放入到ECS中执行

GEC被放入到ECS中里面包含两部分内容：

+ 第一部分：在代码执行前，在parser转成AST的过程中，会将全局定义的变量、函数等加入到GO 中，但是并不会赋值，这个过程称之为变量的作用域提升(hoisting)
+ 第二部分：在代码执行中，对变量赋值，或者执行其他的函数

每一个执行上下文会关联一个VO(变量对象)，变量和函数声明会被添加到这个VO对象中，当全局代码被执行的时候，VO就是GO对象了

### 函数执行过程
1.在执行的过程中执行到一个函数时，就会根据函数体创建一个函数执行上下文(FEC)并且压入到ECS中

2.创建一个AO对象指向当前函数

+ AO对象会使用arguments作为初始化，并且初始值是传入的参数
+ AO对象会作为执行上下文的VO来存放变量的初始化

3.当函数被调用时，给函数传递参数，并将结构体代码执行

4.如果一个函数被多次调用，在每一次调用时都会创建一个新的AO对象

5.函数完全执行结束之后，将ESC中的AO对象删除

### 作用域和作用域链
当进入到一个执行上下文时，执行上下文也会关联一个作用域链

+ 作用域链是一个对象列表，用于变量标识符的求值
+ 当进入一个执行上下文时，这个作用域链被创建，并且根据代码类型，添加一系列的对象

如果函数中的变量，并未在函数体内定义，则会去全局对象GO中寻找该变量，如果在函数体内有定义则会寻找体内定义的这个变量，在区分作用域时要结合执行过程的相关知识，如GO,AO等

## Javacript内存管理-见小余
不管什么样的编程语言，在代码的执行过程中都是需要给它分配内存的，不同的是某些编程语言需要我们自己手动的管理内存，某些编程语言会可以自动帮助我们管理内存

对于开发者来说，JavaScript的内存管理是自动的、无形的



JavaScript会在定义数据时为我们分配内存

+ 对于原始数据类型内存的分配，会在执行时，直接在栈空间进行分配
+ 对于复杂数据类型内存的分配，会在堆内存中开辟一块空间，并且将这块空间的指针返回值变量引用

### 垃圾回收GC
因为内存的大小是有限的，所以当内存不再需要的时候，我们需要对其进行释放，以便腾出更多的内存空间。

对于那些不再使用的对象，我们都称之为是垃圾，它需要被回收，以释放更多的内存空间。

而我们的语言运行环境，比如Java的运行环境VM,JavaScript的运行环境js引擎都会内存垃圾回收器。垃圾回收器我们也会简称为GC，所以在很多地方你看到GC其实指的是垃圾回收器；

### GC算法
如何判断一个对象是不是垃圾，就需要GC算法，常见的有两种

1. 引用计数：当一个对象有引用执行他时，那么这个对象的引用就+1，当引用为0时，就把这个对象销毁。弊端是会产生循环引用
2. 标记清除：设置一个根对象，GC定期从这个根出发，找所有根开始有引用到的对象，对于那些没有引用到的对象，就认为是不可用的对象



![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611166184-7aea58a4-bebe-404e-9957-b5354622d828.png)

## JS 的作用域链
### 概念
它决定了如何访问一个变量或函数，以及在何处查找这些变量或函数

作用域链是在 EC 被创建时构建的，它是一个对象列表，这些对象包含了在当前 EC 中可访问的所有变量和函数

### 组成
1. 当前 EC 的 VO
2. 外层 EC 的 VO
3. GO
+ 作用域链的末端总是全局变量对象，这确保了所有执行上下文都能够访问全局变量

当 JavaScript 引擎需要查找一个变量时，它会从作用域链的头开始查找，如果当前上下文中没有找到该变量，它会沿着作用域链向上查找，直到全局变量对象。如果在全局变量对象中仍然没有找到该变量，就会抛出一个 ReferenceError

## 闭包
### 概念
因为闭包和作用域链关联很大，所以放在一起讲解

总结：一个普通的函数function，如果它可以访问外层作用域的自由变量，那么这个函数就是一个闭包

+ 从广义的角度来说：JavaScript中的函数都是闭包
+ 从狭义的角度来说：JavaScript中一个函数，如果访问了外层作用域的变量，那么它是一个闭包

当我们的代码没有闭包时，往往代码会有非常大的局限性：如果一个函数需要使用到外面作用域的变量，那么必须将所有的变量全部传入进去

### 内存泄露
使用闭包可能会导致内存泄漏：因为闭包会引用外部函数的变量对象，如果这个闭包被长期保存，那么外部函数的变量对象就会一直存在内存中，无法被垃圾回收

怎么处理？

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728724224028-f812172c-ca54-48d3-b4af-644a78b61c23.png)

<font style="color:rgb(1, 1, 1);">将add8设置为null时，就不再对函数对象有引用，那么对应的AO对象也就不可达了，在GC的下一次检测中，它们就会被销毁掉</font>

# 4. 函数增强知识
## 函数对象的属性
在js中，函数可以看做是特殊的对象，那么对象中就可以有属性和方法

+ name：可以访问函数的名字，foo.name
+ length：返回函数参数的个数，foo.length

## arguments
是一个类数组(array-like)的对象，函数的所有传参都会存到argumens中，帮助函数进行初始化

array-like意味着它不是一个数组类型，而是一个对象类型

+ 它拥有数组的一些特性，比如说length，比如可以通过 arguments[x] 来访问
+ 但是它却没有数组的一些方法，比如filter、map等

注意：箭头函数不能使用arguments，，在箭头函数中使用arguments会去上层作用域查找

### arguments 转 Array
在开发中经常需要将arguments转成Array，以便使用数组的一些特性，下面是三种方法

1. 遍历arguments，添加到一个新数组中
2. 调用数组slice函数的call方法，了解即可
3. ES6中的两个方法
+ Array.from(arguments)
+  […arguments]

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725960737322-8c4be954-bdde-43f3-a163-b3e4ca29309f.png)

### 函数的剩余 rest 参数
Es6新增内容rest参数，可以将不定数量的参数放入一个数组中，该数组是真的数组，并且数组名可以自己定义

使用方法：最后一个参数后写上 …数组名

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725960824809-1634491f-c5c4-42b9-bdd9-1cc18c56d3f1.png)

![es6-解构](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729042354426-a22c88e8-26a1-4f8c-9ca5-14fb7d407161.png)

 剩余参数和arguments有什么区别呢？

+ 剩余参数只包含那些没有对应形参的实参，而arguments对象包含了传给函数的所有实参
+ arguments对象不是一个真正的数组，而rest参数是一个真正的数组，可以进行数组的所有操作
+ arguments是早期的ECMAScript中为了方便去获取所有的参数提供的一个数据结构，而rest参数是ES6中提供并且希望以此来替代arguments的

## 纯函数
需要符合以下条件

+ 确定的输入，一定会产生确定的输出
+ 函数在执行过程中，不能产生副作用
    - 副作用：表示在执行一个函数时，除了返回函数值之外，还对调用函数产生了附加的影响。比如修改了全局变量，修改参数或者改变外部的存储

实例：

+ slice：slice截取数组时不会对原数组进行任何操作，而是生成一个新的数组
+ splice：splice截取数组，会返回一个新的数组，也会对原数组进行修改
+ slice就是一个纯函数，不会修改数组本身，而splice函数不是一个纯函数

## 柯里化
### 概念
+ 是一种关于函数的高阶技术，还可以用在别的编程语言
+ 只传递给函数一部分参数来调用它，让它返回一个函数去处理剩余的参数，这个过程就称之为柯里化
+ 在柯里化中，每个函数只接受一个参数，并返回一个新函数，直到所有参数都被接收为止
+ 柯里化不会调用函数，它只是对函数进行转换，转换后的函数叫做柯里化函数

手写柯里化实例：注意箭头函数

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611166860-a2f4b7a0-f776-4447-9d72-6ea2013761b9.png)

### 柯里化的优势
1. 函数的职责单一：将参数依次传入并执行，不会一股脑的全部挤进来![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725961257714-971549f4-fb4e-491d-9078-de3948f7906a.png)
2. 函数的参数复用：makeAdder函数要求传入一个num（并且如果我们需要的话，可以在这里对num进行一些修改），在之后使用返回的函数时，我们不需要再继续传入num了

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611167378-bdf635c8-2308-4760-a857-6db5931046a9.png)

### 自动化柯里化函数
在真实开发时，可以直接调用相应的柯里化库，不需要自己写

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725961437485-86bdee10-75ef-45fc-88d4-4c68f5fc9163.png)

## 组合函数
### 概念
组合函数是在JavaScript开发过程中一种对函数的使用技巧、模式

+ 比如现在需要对某一个数据进行函数的调用，执行两个函数fn1和fn2，这两个函数是依次执行的
+ 如果每次我们都需要进行两个函数的调用，操作上就会显得重复
+ 将这两个函数组合起来，自动依次调用，这个过程就是对函数的组合，称组合函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725961535168-59be250f-5890-4d04-843c-49929580f407.png)

### 实现组合函数
需要考虑更加复杂的情况：比如传入了更多的函数，在调用compose函数时，传入了更多的参数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725961648644-82fc7166-3217-4e7d-8045-7e3376b11703.png)

## with 语句
扩展一个语句的作用域链，将指定对象的属性添加到当前作用域

这种做法在现代JavaScript编程中不推荐使用，因为它可能导致代码难以理解和调试

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725961783083-26be8e2d-9411-4a84-96c4-ace138303604.png)

## eval 函数
将传入的字符串当做js代码来执行，并把最后一个执行语句的结果当做返回值，不推荐使用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725961820838-246ae882-1765-4898-bdbd-76ff3e360b69.png)

## 严格模式
+ 通过抛出错误来消除一些原有的静默(silent)错误
+ 让js引擎在执行代码时可以进行更多的优化（不需要对一些特殊的语法进行处理）
+ 禁用了在ECMAScript未来版本中可能会定义的一些语法



开启方法

+ 全局开启：直接在文件中js部分写上“use strict”(需要引号)
+ 函数内开启：在函数{}内写上“use strict”



严格模式常见的限制

1. 无法意外的创建全局变量
2. 严格模式会使引起静默失败的赋值操作抛出异常
3. 严格模式下试图删除不可删除的属性
4. 严格模式不允许函数参数有相同的名称
5. 不允许0的八进制语法
6. 在严格模式下，不允许使用with
7. 在严格模式下，eval不再为上层引用变量
8. 严格模式下，this绑定不会默认转成对象

# 5. 对象增强知识
## 对象方法补充
+ 获取对象的属性描述符
    - getOwnPropertyDescriptor(obj)
    - getOwnPropertyDescriptors(obj)

```javascript
const obj = { name: 'Alice', age: 25 };
const descriptor = Object.getOwnPropertyDescriptor(obj, 'name');
console.log(descriptor);
// { value: 'Alice', writable: true, enumerable: true, configurable: true }
```

```javascript
const obj = { name: 'Alice', age: 25 };
const descriptors = Object.getOwnPropertyDescriptors(obj);
console.log(descriptors);
/*
输出:
{
  name: { value: 'Alice', writable: true, enumerable: true, configurable: true },
  age: { value: 25, writable: true, enumerable: true, configurable: true }
}
*/
```

+  防止向对象添加新的属性：现有的属性不会被删除或修改，只是无法添加新属性
    - preventExtensions(obj)

```javascript
const obj = { name: 'Alice' };

// 调用 Object.preventExtensions() 方法
Object.preventExtensions(obj);

// 尝试添加新的属性
obj.age = 25;

console.log(obj); // 输出: { name: 'Alice' }
console.log(Object.isExtensible(obj)); // 输出: false
```

+ 密封对象：不许增删属性，可以修改现有的
    - seal(obj)

```javascript
const obj = {
  name: 'Alice',
  age: 25
};

// 密封对象
Object.seal(obj);

// 尝试添加新的属性
obj.gender = 'female'; // 无效，属性不会被添加

// 尝试删除现有属性
delete obj.age; // 无效，属性不会被删除

// 修改现有属性的值
obj.name = 'Bob'; // 有效，属性值可以被修改

console.log(obj); // 输出: { name: 'Bob', age: 25 }
console.log(Object.isSealed(obj)); // 输出: true
```

+ 冻结对象，不允许增删和修改内容
    - freeze(obj)

```javascript
const obj = {
  name: 'Alice',
  age: 25
};

// 冻结对象
Object.freeze(obj);

// 尝试添加新的属性
obj.gender = 'female'; // 无效，属性不会被添加

// 尝试删除现有属性
delete obj.age; // 无效，属性不会被删除

// 尝试修改现有属性的值
obj.name = 'Bob'; // 无效，属性值不能被修改

console.log(obj); // 输出: { name: 'Alice', age: 25 }
console.log(Object.isFrozen(obj)); // 输出: true
```

## 对象的创建方式
创建多个对象的方案其实一共有四种：

+ 工厂模式
+ 构造函数
+ 类class
+ 对象原型和 Object.create()

目前来说，我们只讲解前两种方式，因为类是ES6之后的语法，会放在ES6-ES14系列中来进行学习。而原型链，我们也还没进行学习，此时对我们来说会有点难度，可以等后续文章学习到了原型以及原型链之后再回头来进行思考

### 工厂函数
工厂函数在JS中是一种常用的设计模式，主要用于创建和返回复杂对象的实例。它们不像构造函数那样依赖于new关键字，而是普通函数，但被设计来构造并返回新的对象实例可以接受参数，根据这些参数定制创建的对象，每个对象实例可以根据不同的需求进行个性化配置。而这也是我们所需要的部分

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728816060899-2c8bec2b-11e5-4be3-84f0-ac0d0ecbef69.png)

### 构造函数
<font style="color:rgb(1, 1, 1);">如果一个普通的函数被使用new操作符来调用了，那么这个函数就称之为是一个构造函数</font>

<font style="color:rgb(1, 1, 1);">如果一个函数被使用new操作符调用了，它会执行如下操作：</font>

1. <font style="color:rgb(1, 1, 1);">在内存中创建一个新的对象（空对象）</font>
2. <font style="color:rgb(1, 1, 1);">这个对象内部的[[prototype]]属性会被赋值为该构造函数的prototype属性</font>
3. <font style="color:rgb(1, 1, 1);">构造函数内部的this，会指向创建出来的新对象</font>
4. <font style="color:rgb(1, 1, 1);">执行函数的内部代码，即函数体代码</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728816747101-62032790-352c-4d7a-9224-4afc35e98b71.png)

怎么区分构造函数

+ 在写的时候，我们都以首字母大写开头，使用大驼峰命名，这是一个社区规范
+ 前置条件：在函数内使用了this完成赋值操作，这意味着该函数的内容并不固定，随this变化而动态调整内容，但结构不变
+ 而编辑器会在前置条件满足后，给于我们提示：'此构造函数可能会转换为类声明'
+ 在this指向中，我们有说明new调用是改变this指向最优先的事项，因此在编辑器的眼中使用了this，是一个预备动作，但需要注意，只有new调用后，才真正属于构造函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728817504593-b4f772b2-dde6-4177-bc95-e67878873722.png)



缺点

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728817593205-1d95a257-8845-4017-803f-5cf85277ce13.png)

为什么是f1 === f2是false？

那是因为他们创建出来的对象其实不是同一个对象，虽然打印出来都是[Function: bar]，但是确实是不一样的两个对象

每个对象创建一个函数实例会增加内存的开销，这可能会导致性能问题，有点浪费我们的空间，特别是当我们有大量的对象的时候。此外，如果在构造函数中改变了对象的原型，这也会导致每个对象都有不同的原型，这可能会增加代码的复杂性

# 6. ES5
## 对象的原型
### 概念
对象的原型是 JavaScript 的一个重要特性，用于实现继承和共享功能

每个 JavaScript 对象都有一个内部属性 [[Prototype]]，这个属性是一个链接，它指向另一个对象(原型)，对象可以从它的原型继承属性和方法

[[Prototype]] 是隐式原型，不等于prototype(显示原型)

原型链: 如果在对象本身上找不到某个属性或方法，JavaScript 引擎会沿着原型链向上查找，直到找到该属性/方法/到达原型链的末端（Object.prototype）

### 获取方法
1. Object.getPrototypeOf()：用于获取对象的原型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726196254764-9adb1430-d78a-4045-93c9-7a45b2c89392.png)

2. __proto__：是一个非标准的属性，用于直接访问和设置对象的原型

不推荐使用的原因：在于它没有被保证，当下可以使用，但未来可就不一定了(可能被弃用)。程序稳定性的优先度是非常高了，所有非标准特性都应该是我们应该避免的，而并非只有__proto__一个。使用标准方法可以确保代码的长期兼容性

__proto__并不是[[prototype]]，因为[[prototype]]是ECMAScript规范的一部分，而__proto__不是，Object.getPrototypeOf() 获取到的才是[[prototype]]，因为这个方法是ECMAScript 5标准的一部分，并且在所有现代浏览器中都得到了支持

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726196265291-f5febfce-aa3c-4c21-9e03-9b706fb77f79.png)

## 函数的原型
### 概念
所有的函数都有一个prototype的属性：这个prototype属性被称为显示原型属性，和隐式原型[[prototype]]有点区别

因为函数是特殊的对象，所以除了显式原型 prototype，他也有对象的隐式原型 [[prototype]]

获取方式：foo.prototype，并没有兼容问题

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728887741735-7763df44-d873-40a8-a6a1-de8fbb0f7a11.png)

### 显式原型和隐式原型的区别
1. 归属不同：
    - prototype是函数对象（特别是构造函数）特有的属性
    - 而[[Prototype]]是所有对象都有的内部属性
2. 功能不同：
    - 构造函数的prototype属性定义了通过这个**构造函数**创建的所有**对象实例**的原型(new 出来的对象)，从而实现属性和方法的继承
    - 对象的[[Prototype]]属性指向其原型对象，是对象实际的原型链的一部分，用于实现基于原型的属性查找或继承
3. 设置方式：
    - prototype是在定义构造函数时附带的，通常可以直接修改
    - [[Prototype]]通常在对象创建时由解释器自动设置，可以通过Object.setPrototypeOf()、Object.create()或使用__proto__（非标准，不推荐）修改

### New操作符
构造函数使用new操作符创建对象时，会把函数内部的prototype属性赋值给对象

1. 在内存中创建一个新的对象（空对象）
2. 这个对象内部的[[prototype]]属性会被赋值为该构造函数的prototype属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726198026019-87049580-98c0-4bf8-ab13-406204e2608b.png)

也就意味着所有通过foo函数创建出来的对象，他的 [[prototype]] 都指向构造函数foo的prototype

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726198091942-40f334d4-0e5a-418f-9636-929dc773ea22.png)

所以如果需要创建多个具有相同方法的对象，可以在构造函数的prototype里写上方法，在实例化时会把这些方法都传递给对象的[[prototype]]。添加的不一定非要是函数，也可以时字符串。比如：

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611171377-afbf1f54-5cda-4683-a02c-88bdaca64a64.png)

根据内存来理解prototype

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611171701-0beaa0a3-7737-4008-a047-188355d8569e.png)

实例对象__proto__和函数对象的prototype在内存中其实是同一个东西，不管是在实例对象原型中添加内容或是函数对象中添加内容，都会添加到同一个区域，所以在调用时，若实例对象本身没有该属性，会在原型中找

### 构造函数原型内存图
Person 构造函数本身也是存在隐式原型的，而隐式原型指向于显式原型

通过打印Person函数中的显式原型prototype，能够看到在显式原型中，存在着隐式原型[[prototype]]

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728888511561-25b4c06f-b54d-4809-b034-ad4746348853.png)

new 出来两个对象 p1 和 p2，p1与p2作为对象，是没有显式原型的，也就是只有[[prototype]]，这两个对象的[[prototype]]会指向于该构造函数的prototype属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728888692482-a53bf249-c743-4418-8454-99a33498d40a.png)

简化的内存指向图如下

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728888836320-76028511-7610-49e4-9976-8b3f0140bb72.png)

如果我p1里没有name这个属性，我有几种方法可以通过p1.name获取到name内容？

一共有三种

1. 把name放到p1的隐式原型去，这个之前尝试过
2. name放到Person的显式原型中
3. name放到p2的隐式原型去

**解析**：由于内存指向都是一样的地方，也就是说这道题的核心在于，能够找到几条通往构造函数的显式原型的路

### 函数原型的属性
#### prototype 添加属性
添加方法：`Function.prototype.xxx = function(){}`

添加属性和添加方法的写法是一样的，而添加在哪一层就要根据实际的适用范围需求，范围到底怎么划分的，可以从后面的原型链中寻找答案

在这里，修改的就是显式原型

因为隐式原型偏向于底层，最好不要动；JS 鼓励我们在定义阶段就设定好所有实例共享的属性和方法。而动态修改 [[Prototype]] 则被视为一种“hack”手段，应当避免，除非特殊情况

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728890508440-fee462dc-78b9-4565-9fbf-a2d4c95a45e4.png)

通过修改构造函数的 prototype 属性来设置原型，就是在创建对象(实例)之前完成的(定义阶段)，不会影响已存在的实例

#### Constructor属性
事实上原型对象上面是有一个属性的：constructor，被称为构造器

+ 默认情况下原型上都会添加一个属性叫做constructor，这个constructor指向创建实例对象的构造函数
+ 这个属性在原型继承中扮演着重要的角色，因为它保持了原型链上各级对象与其构造函数之间的联系

constructor在以往进行打印的时候，内在是空的，但其实并不是的

+ 默认是存在属性描述符的，只是被设置了不可枚举，导致我们看不到
+ 通过 getOwnPropertyDescriptors可以看到内在属性



`console.log(foo.prototype,"纯foo.prototype打印")`   打印出来是个空对象{}，但是事实上该对象并不是空的，只是因为可枚举属性被设置为了false

`console.log(Object.getOwnPropertyDescriptors(foo.prototype),"getOwnPropertyDescriptors打印foo.prototype");`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728891826264-d1391185-e490-427f-aec4-b29e762ee688.png)

constructor属性是被ECMA要求必须添加的，不存在兼容性的问题

+ 主要原因是为了维护构造函数与其创建的对象之间的显式关联
+ 通过上面图片中可以看到，枚举出来的不止是属性描述符，还有一个value是foo函数，就是通过这个value实现的构造函数和其创建对象的显示关联
+ 而这也是为什么会形成显式、隐式原型闭环的原因，从new创建出来的对象：隐式原型指向于显式原型，而显式原型内的constructor的value居然又是一个foo构造函数，所以**prototype.constructor = 构造函数本身**

我们知道构造函数同时具备隐式原型和显式原型，在这里就形成闭环。实际上，这

是一个设计决策，确保原型链的完整性和可追溯性

### 重写原型对象
这个互相引用是会回收的，因为JS的垃圾回收机制是标记清除，是从根节点开始看有没有引用的

重写原型对象：指的是将一个构造函数的 prototype 属性设置为一个新的对象，而不是向现有的 prototype 对象添加或修改属性。这种做法通常用于重新定义整个原型链，或者在实现继承时完全替换一个类的行为和属性



例子：

+ foo函数不再指向它的原型对象，而是指向新的对象
+ 刚指向的时候，这个新对象连constructor都没有，指向新对象之后，foo函数的原型对象就会被销毁掉
+ 而当填入内容之后，就可以访问到其内容

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728892399593-69687ac0-c498-422c-a89b-a5bfa9eb9ace.png)

使用场景：而当填入内容之后，就可以访问到其内容

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728892472760-4b61664b-9b03-4609-9cfc-c877439befea.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728892484193-a7f284c5-299e-4428-b8c0-3353b361b128.png)



还需要给新对象加上constructor，指回foo对象形成闭环，原来的显式原型就被完整替代，就会被 GC 回收了

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728892709779-e9852e27-42e6-4f4c-a57f-a6c524c34cf2.png)

这一步可以用 defineProperty 实现

```javascript
Object.defineProperty(foo.prototype,"constructor",{
    enumerable:false,
    writable:true,
    configurable:true,
    value:foo
})
```

### 构造函数创建对象
构造函数的方式创建对象时，有一个弊端：会创建出重复的函数，怎么让所有的对象去共享这些函数数据呢？



1. 错误写法：把数据放到Person.prototype的对象上

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728893780459-8ac621af-cc64-4038-aa0a-c1323a8b566d.png)

+ 往p1，p2传入数据，第一时间肯定是先去p1跟p2各自的身上找，但很显然，我们在Person里面的代码，是直接放到他的显式原型上面了，而Person本身就什么都没有
+ 当p1的数据放到显式原型上后，p2的数据紧随其后跟着放上去了，就会直接在显式原型中直接覆盖掉p1的数据
+ 当我们使用p1.name的时候，本身找不到，紧接着去隐式原型中找，没找到，再去显式原型中找，这次找到了，但是找到的是被p2覆盖掉的数据，所有当p1.name拿出来的时候就会是p2的name数据



2. 正确的写法：数据绑定通过this来去进行判定，而共通的函数则放到原型之中

```javascript
function Person(name,age,sex,address){
    this.name = name,
    this.age = age,
    this.sex = sex,
    this.address = address
}
Person.prototype.eating = function(){
    console.log(this.name+"今天吃烤地瓜了");
}

Person.prototype.running = function(){
    console.log(this.name+"今天跑了五公里");
}

var p1 = new Person("小余",18,"男","福建")
var p2 = new Person("coderwhy",35,"男","广州")
console.log(p1.name);//小余
```

## 原型链详解
### 原型链
#### JS 中的原型链
从一个对象上获取属性，如果在当前对象中没有获取到就会去它的原型上面获取

查找顺序

1. 在当前对象上查找
2. 找不到则沿着原型链查找
3. 如果未找到，则返回undefined

这个查找机制是通过[[GET]]来实现的，我们之前说过，通过[[]]括起来的内容，表示内部机制的内容，这是JS内在运行的原理之一，而[[GET]]也不例外

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728896829931-dd631898-988b-4ba4-8b02-67757d741c0f.png)



hasOwnProperty判断自有属性是否有对应的属性,在 ES13 中，该方法已经被 Object.hasOwn() 代替，但是使用的规则是一样的

替代的原因：由于 hasOwnProperty 是 Object.prototype 上的方法，它可以被继承对象覆盖，这可能导致意外行为；Object.hasOwn() 作为静态方法，不受继承影响，提供了更安全的方式来检查属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728954889787-a5c13fc5-a2d9-4815-9879-346ac53c1c3a.png)

#### Object 的原型
从Object直接创建出来的对象的原型都是 [Object: null prototype] {}，这个原型也是最后一层原型，该对象上有很多默认的属性和方法

原型链的尽头是由 null 表示的，当这个内部属性的值为 null 时，就意味着没有进一步的原型可以被访问，因此这是原型链的终点

为什么要这样设计，为什么要让Object.prototype 的原型是 null？

+ 原型链越短越好，如果原型链很长，每次属性访问都可能需要遍历整个链
+ 避免无限循环，如果无限的循环就会产生无限的遍历原型链，会卡死在这里
+ 通过设置终点，JS引擎可以更快地确定一个属性是否在原型链上。ES13语法的hasOwn方法就是直接静态方法，放在构造函数身上，直接从根源上避免了这个问题，因而做到性能的提升

#### Person 构造函数原型
使用构造函数 new 对象时的过程

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728955527472-3961e449-7434-4291-bc96-79d0b7d1c1bf.png)

1. 首先会在内存中创建一个对象，也就是p
2. 将Person函数的显式原型prototype赋值给p对象的隐式原型，首先只有函数才有显式原型，其次p是因Person函数new调用而来的，所以p的初始基础内容来自 Person
3. Person.prototype指向了p的隐式原型，而p本身是一个对象，和直接new出来的对象本质是一样的，所以p的隐式原型直接指向于Object.prototype

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728955830632-6d79900f-c67a-4868-9338-74d2f0e0b6fc.png)

那么完整的原型链为

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728955987901-7a8f319f-6997-4d4a-82e9-f211c15501fa.png)

用 B 表示 [[prototype]]

1. 第一层 "B"：p 是 Person 的一个实例，p 自身有一个 [[Prototype]] 链接，这是第一层 "B"
2. 第二层 "B"：通过第一层的 [[Prototype]] 链接，我们找到 p 的原型，即 Person.prototype，这是第二层 "B"
3. 第三层 "B"：Person.prototype 也有自己的 [[Prototype]] 链接，它指向 Object.prototype，这是第三层 "B"
4. 末端 "NULL"：Object.prototype 的 [[Prototype]] 链接指向 null，表示原型链的结束



那么 new 的全部过程：

```javascript
var obj = {}//该创建方法是下面这种的语法糖

var obj2 = new Object()//obj2.__proto__ = Object.prototype

function Person(){

}

var p = new Person()
```



new出构造函数这个操作发生的步骤：

1. 在内存中创建一个对象，var moni = {}
2. this的赋值，this = moni
3. 将Person函数的显示原型prototype赋值给前面创建出来的对象的隐式原型，moni.__proto__=Person.prototype
4. 执行代码体



但是当我们在使用new Object()的时候，赋值的情况如下：

```javascript
var moni = {}
this = moni
moni.__proto__ = Object.prototype
obj2.__proto__ = Object.prototype
//那这时候就证明了一点，obj.__protot__ === Object.prototype
```

#### 原型链的内存图
+ 最顶层的原型就来自Object.prototype，因为再接下去就是null了，也就是原型链的终点
+ 从最顶层出发，Object和Object.prototype其实通过[[prototype]]和constructor相互引用
+ 紧接着是普通对象(字面量和new)直接指向于Object.prototype
+ 通过改变__proto__指向其他地方的，最终正如下图右边所示，指向多层隐式原型后，最终也会指回Object.prototype

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728956634950-72081e0d-f945-4c07-bc49-75290dc5ff00.png)



JavaScript 中所有对象都是通过构造函数创建的，并且对象都有一个隐式的属性，称为原型 (prototype)。每一个构造函数都有一个prototype属性，这个属性指向一个对象，这个对象也叫原型对象，继承了一些属性和方法

每当一个新对象被创建，它的内部指针就会指向它的构造函数的原型对象。这意味着如果我们在原型对象上修改了一个属性或方法，那么所有继承了这个原型对象的对象都会受到影响

所以原型属性可以用来实现继承

### 继承
#### 为什么讲继承和原型链
为了让开发者能够创建可重用的组件和更丰富的交互式行为，JS 提供一种机制来实现对象间代码的重用成为了一个关键需求，这就是原型链。

原型链是JavaScript中对象之间通过原型相互链接形成的链式结构，而继承则是利用这种链式结构来实现属性和方法的共享与复用

每个对象都有一个原型对象，对象从其原型继承属性和方法。

这种模型相对于传统的类继承模型来说，更加灵活和动态。在JS中，可以在运行时修改原型，从而改变一个类的行为



继承的作用：继承可以帮助我们将重复的代码和逻辑抽取到父类中，子类只需要直接继承过来使用即可

继承的目的：重复利用另外一个对象的属性和方法



两者的关系：

在JavaScript的设计和发展中，原型链的引入和继承的需求是相辅相成的。可以说，原型链是为了实现继承而设计的，而继承的需求又进一步推动了原型链机制的发展和完善。

这不是一个单向的过程，而是一个互动的发展过程，其中原型链提供了继承机制的实现基础，而继承的概念则指导了原型链如何被构造和优化以满足实际的编程需求

#### 原型链继承
父类是Person(人)，子类是Student(学生)，这两个类现在本质上都是函数

我们的目的：new调用的对象是Student构造函数，也就是子类。返回的是stu对象，需要stu既继承Student(学生)的内容，又继承Person(人)的内容

```javascript
//未实现继承的效果，打印出来效果为undefined
//父类，公共属性和方法
function Person(){
    this.name = "小余"
}

Person.prototype.eating = function(){
    console.log(this.name +"eating~");
}

//子类：特有属性和方法
function Student(){
    this.sno = 111
}

Student.prototype.studying = function(){
    console.log(this.name + "studying");
}

var stu = new Student()
console.log(stu.name);//undefined
console.log(stu.eating);//undefined
//很明显，顺着原型链也是找不到name跟eating这两个属性的
//因为stu是接收Student产生的新对象，只会在构造函数Student上面追溯
```

所以我们stu实例想要拿到Person或者Person原型里的内容，就需要把Student原型和Person原型关联起来，组成完整的原型链

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728957804612-d4f1b55d-5363-473f-b751-29bd5a20306a.png)

创建出一个p实例指向于Person原型，而这个p可以放到Student原型当中，就能够成功将Student原型和Person原型关联起来了，p实例在这里产生一个中间人的作用，也可以不要中间人 p 

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728957920613-ad841d6e-3be7-452b-bcdd-ab798382f595.png)

接着就来看看完整的过程：

```javascript
//实现继承的效果，关键在步骤1、2，顺序不能调整
//定义父类构造函数
function Person(){
    this.name = "小余"
    this.friends = []
}
//往父类原型上添加内容
Person.prototype.eating = function(){
    console.log(this.name +"eating~");
}

//定义子类构造函数
function Student(){
    this.sno = 111
}
//步骤1：创建父类对象，并且作为子类的原型对象(关键)
var p = new Person()
Student.prototype = p
//这一步赋值的操作之后，Student原来的原型对象就不再被指向，会在下一轮中被垃圾回收掉
//我个人认为这个p更像是链接子类跟父类的中转站，但是它是会替代掉子类原来的原型

//步骤2：在子类原型上添加内容 
Student.prototype.studying = function(){
    console.log(this.name + "studying");
}

var stu= new Student()
console.log(stu.name);
stu.eating()
```

步骤 2 不能够跟步骤1换是很好理解的，因为步骤1要替换掉原型，如果步骤2先的话，会绑定到要被替换掉的原型身上，然后跟着一起被替换掉。所以不能够这么做

##### 弊端
1. 直接打印stu对象，会发现原型发生了变化(new调用Student，原型却是Person)，继承来属性是看不到的(但可以打印出来)；正常来说，继承来的属性，也应该属于继承者的一部分如果开发的时候看不到这个信息，在出现问题需要进行排查的时候就会造成信息差的误解
2. 这个属性会被多个对象共享，如果这个对象是一个引用类型，那么就会造成问题；所以应该在构造函数中定义引用类型的属性，而不是在原型上
3. 不能给Person传递参数，因为不能确定传给哪一层参数

#### 借用构造函数继承-不推荐
为了解决原型链继承中存在的问题，开发人员(非官方)提供了一种新的技术: 借用构造函数 constructor stealing，在子类型构造函数的内部调用父类型构造函数

因为函数可以在任意的时刻被调用，因此通过apply()和call()方法也可以在新创建的对象上执行构造函数，通过这两个方法，配合this的绑定，从而撬动借用继承，实现借鸡生蛋的效果(在Person函数运行Student的初始化代码)。就无需在Student构造函数中进行处理

```javascript
//解决无法在子类传递参数的问题
function Person(name,age,sex,address){
  this.name = name 
  this.age = age
  this.sex = sex
  this.address = address
  this.friends = []
}

Person.prototype.eating = function(){
  console.log(this.name +"在吃早餐");
}


function Student(name,age,sex,address){
  Person.call(this,name,age,sex,address)
  //这里的this是new Student时创建出来的对象，通过call调用这三个参数，就是一个普通的函数调用，就会去父类中调用函数了(子类型构造函数的内部调用父类型构造函数)
  //不能像下面这样够这么传递，这样就把处理逻辑放到子类里面了，公共的应该抽到父类中去
  // this.name = name 
  // this.age = age
  // this.sex = sex
}

var p = new Person()
Student.prototype = p

Student.prototype.learn = function(){
  console.log(this.name+"在学习");
}

var stu = new Student("小余","男",20,"福建")
//成功传递参数，但是类型还是Person，还有问题，后续解决
console.log(stu);
//Person { name: '小余', age: '男', sex: 20, address: '福建' }
```

在这个过程，巧妙的规避掉原型链弊端的第二点(属性被多方共享)和第三点(无法定制化)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728962723082-03971ed2-d2c9-495e-a43b-a159abf7f789.png)

##### 弊端
+ 组合继承最大的问题就是无论在什么情况下，都会调用两次父类构造函数
+ stu的原型对象上会多出一些属性, 但是这些属性是没有存在的必要，多出的属性来自p对象的属性，因为我们原来的原型被p对象替换了
+ 子类和父类脱离了原型链体系，导致了子类不能够访问父类原型上定义的方法，所以后续所有相关联类型都只能使用构造函数模式

#### 原型式继承函数
原型式继承函数是一种在JavaScript中实现对象之间继承的技术，主要通过克隆已存在的对象来创建新对象。这种继承方式不依赖于传统的类结构或构造函数，而是直接操作对象的原型。

在ES5 中，这种方法被标准化为Object.create函数，ES5 之前则通常通过自定义函数来实现

原型式继承函数最终的目的：Student对象原型指向了Person对象原型

`Student.prototype = Person.prototype`

这样甚至连Person都不需要变成构造函数，内容直接传到Person的原型上面了。这样就影响不到Person自身的对象了，也就不会在Person自身对象多出一些不需要的属性了

但是具有局限性:此时Person跟Student指向同一个原型(Person原型)了。此时如果再多一个Teacher来指向Person原型的话(同种方案)，我们对Student的修改影响了Person的原型，那Person的原型也会影响到Teacher

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728974158051-c32d5d8a-69bc-4e2e-86c4-a7151c8bdf83.png)

这是原型式继承的主要缺陷：共享的引用属性，原型中包含的引用值属性将在所有实例间共享，这在多数情况下是不被期望的

在正式的使用当中，我们并不会像上图这样直接改变已有对象的原型，因为已有对象只有一个，而我们该对象有可能还需要使用而不能改变，并且需要使用的对象数量是不固定的。所以，我们应该以已有对象为蓝本，去创建出新的对象，这种做法会更加稳定，该做法的思想也被称为数据的不可变性

在原型式继承当中，我们并没有利用到构造函数，在这里最核心的就是中间一整条路线的原型赋值原型，进而让两个newObj对象的原型指向为Person原型，从而实现重复利用另外一个对象的属性和方法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728974571041-5b01507d-b1e7-4764-b042-c59c60503482.png)

实现步骤：

1. 创建出一个newObj新对象
2. 原对象赋值新对象(该操作是最核心的一步，实现了"副本"的作用，新对象后续操作不会影响到原对象)
3. 新对象原型指向Person原型对象
4. 返回新对象

```javascript
var Person = {
  name: "xiaoyu",
  age: 18,
}

function createObject(o) {
  var newObj = {}
  Object.setPrototypeOf(newObj,o)
  return newObj
}

var Student = createObject(Person)
console.log(Student.__proto__);//{ name: 'xiaoyu', age: 18 }
```

```javascript
var Person = {
  name: "xiaoyu",
  age: 18,
}

var Student = Object.create(Person)
console.log(Student.__proto__);//{ name: 'xiaoyu', age: 18 }
```

#### 寄生式继承函数
寄生式继承也是JavaScript中一种实现对象继承的模式，它结合了原型式继承和工厂模式的特点。

这种继承方式的核心在于创建一个“寄生”函数，这个函数的作用是创建一个已有对象的副本，然后在副本上扩展新的特性，最后返回这个副本

这种方式的优点是可以在不改变原型链的情况下进行继承，并且可以在创建新对象时不需要使用关键字 new

步骤：

1. **<font style="color:rgb(0, 0, 0);">创建寄生函数</font>**<font style="color:rgb(1, 1, 1);">：定义一个函数，这个函数通常不会执行任何操作，它的目的是用于借用构造函数</font>
2. **<font style="color:rgb(0, 0, 0);">原型链继承</font>**<font style="color:rgb(1, 1, 1);">：将寄生函数的原型指向要继承的对象的原型，从而实现原型链的继承</font>
3. **<font style="color:rgb(0, 0, 0);">创建新对象</font>**<font style="color:rgb(1, 1, 1);">：使用 new 操作符创建寄生函数的实例，这将执行寄生函数并创建一个新对象</font>
4. **<font style="color:rgb(0, 0, 0);">修改新对象</font>**<font style="color:rgb(1, 1, 1);">：在寄生函数内部，创建一个要继承的对象的副本，并在这个副本上添加或修改属性和方法</font>
5. **<font style="color:rgb(0, 0, 0);">返回新对象</font>**<font style="color:rgb(1, 1, 1);">：返回修改后的副本对象，这个对象将具有寄生函数的实例属性，同时也继承了原型链上的属性和方法</font>

```javascript
//原型函数
var personObj = {
    running:function(){
        console.log("小余正在跑步");
    }
}
// 1. 创建寄生函数
// 定义一个函数，这个函数将作为创建新对象的工厂
function createStudent(name) {
  // 2. 原型链继承
  // 使用Object.create创建一个新对象，其原型是personObj对象
  var student = Object.create(personObj);

  // 3. 创建新对象
  // 给新对象添加特定的属性
  student.name = name;

  // 4. 修改新对象
  // 给新对象添加特有的方法
  student.learn = function() {
    console.log(name + "在学习");
  };

  // 5. 返回新对象
  // 返回创建并修改过的对象
  return student;
}

//寄生式继承 = 工厂函数+原型函数
var stu1 = createStudent("小余")
var stu2 = createStudent("coderwhy")
//一般情况下我们也不会使用，因为是有缺陷的
```

寄生式继承函数就像是一个中间商，把原有共性的部分和后续添加特有的属性进行一个汇聚，形成我们想要的内容。

+ 原型式继承：只有共性部分
+ 寄生式继承：共性部分+特有部分

#### 寄生组合式继承
组合继承(原型链继承+借用构造函数继承+原型式继承)是比较理想的继承方式, 但是存在两个问题:

1. 构造函数会被调用两次: 一次在创建子类型原型对象的时候, 一次在创建子类型实例的时候
2. 父类型中的属性会有两份: 一份在原型对象中, 一份在子类型实例中

现在可以利用寄生式继承将这两个问题给解决掉，即寄生组合式继承

该方案其实是我们前面四种方案：原型链继承、借用构造函数继承、原型式继承、寄生式继承的结合体

**Object.setPrototypeof() **静态方法可以将一个指定对象的原型（即 [[Prototype]]）设置为另一个对象或者 null

## 对象方法补充
除了 JS 基础的对象方法，剩下的部分会分为两块内容：ES6之前的在此补充，ES6及之后的部分会放到后面的模块 ES6 中讲解

### 原型相关
Object.create() 

+ prototype：新对象的原型对象。这个参数是可选的，如果不提供，新对象的原型将是 null，这意味着新对象没有任何可枚举的属性，并且没有任何方法
+ propertiesObject（可选）：一个对象，其属性和属性描述符将被复制到新对象中

```javascript
// 创建一个空对象，没有原型
var obj1 = Object.create(null);
console.log(obj1); // {}

// 创建一个对象，其原型是Object的原型
var obj2 = Object.create(Object.prototype);
console.log(obj2); // {}

// 创建一个对象，其原型是某个对象
var person = {
   isHuman: false,
   printIntroduction: function() {
      console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
   }
};

var me = Object.create(person);
me.name = 'Matthew'; // 添加属性
me.isHuman = true; // 修改属性

console.log(me.name); // 输出：Matthew
me.printIntroduction(); // 输出：My name is Matthew. Am I human? true

// 使用第二个参数添加属性和属性描述符
var anotherObj = Object.create({}, {
  myProp: {
    value: 42,
    writable: true,
    enumerable: true,
    configurable: true
  }
});

console.log(anotherObj.myProp); // 输出：42
```

Object.defineProperty()

+ obj：要在其上定义属性的对象
+ prop：要定义或修改的属性的名称或 Symbol
+ descriptor：要定义或修改的属性的属性描述符
    - value：属性的值。
    - writable：若为 true，则属性的值可以被赋值操作改变。
    - enumerable：若为 true，则属性会体现在对象的枚举属性中(例如 for-in)
    - configurable：若为 true，则属性可以从对象上删除
    - get：给属性提供 getter ，如果没有 getter，则为 undefined
    - set：给属性提供 setter ，如果没有 setter，则为 undefined

```javascript
var obj = {};

// 定义一个不可写、不可枚举、不可配置的属性
Object.defineProperty(obj, 'constant', {
  value: 42,
  writable: false,
  enumerable: false,
  configurable: false
});

console.log(obj.constant); // 输出：42
obj.constant = 24; // 不会改变 obj.constant 的值

// 定义一个有 getter 和 setter 的属性
var x = 1;
Object.defineProperty(obj, 'x', {
  get: function() {
    return x;
  },
  set: function(value) {
    x = value;
  },
  enumerable: true,
  configurable: true
});

console.log(obj.x); // 输出：1
obj.x = 2;
console.log(obj.x); // 输出：2

// 检查属性是否可枚举
console.log(obj.propertyIsEnumerable('x')); // 输出：true
console.log(obj.propertyIsEnumerable('constant')); // 输出：false
```

Object.defineProperties可以同时定义或修改多个属性，并返回该对象

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611171026-9ef8b1b6-87fc-438f-848f-8c286a757766.png)

### 判断方法
1. hasOwnProperty：
    - 判断对象是否有某一个属于自己的属性（不是在原型上的属性）
    - 更好的上位替代：hasOwn方法
2. in/for in ：
    - 遍历一个对象的所有可枚举属性
    - 遍历对象时会把继承来的属性一起遍历出来，想要进行区分的话，得在内部执行体使用hasOwn方法进行判断一下
3. instanceof：
    - 用来判断一个对象是否是某个构造函数的实例
    - object instanceof constructor
4. isPrototypeOf：
    - 检查一个对象是否存在于另一个对象的原型链中
    - instanceof运算符检测来源只能是构造函数，而isPrototypeOf的检测来源是一个对象本身以及该对象身后的原型链
    - `obj.isPrototypeOf(info)`：obj是不是出现在info的原型链上面

# 7. ES6
## Class类
### 类的 constructor
本质上就是构造函数，也有原型和constructor，在es6代替构造函数创建对象

在之前，传递参数是通过函数的形参与实参进行传递的，方法的定义通过继承等多种方式。在Class中，这些内容得到了统一的明确，也就是前面所说的构造函数与继承

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728979749201-e3d71649-17c9-4b3e-9375-0d2fcf2d1a35.png)

每个类都可以有一个自己的构造函数，这个方法的名称是固定的constructor

当我们通过new操作符操作一个类的时候，会调用这个类的构造函数constructor

在这里constructor是一个特殊的函数，对应Person.prototype.constructor，这个特殊的函数只要 new 调用对应的类就会触发，就算没有编写constructor函数也会触发默认的情况

### 类的实例方法
定义在类的原型上的方法，这些方法可以被类的所有实例访问和执行

这些实例方法存储的位置就在构造函数的显式原型上

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728979945779-9110713d-c396-4724-a138-b48bc683acc6.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728980003378-0bdcd1b5-e7b1-401a-920e-1a0c20495096.png)

在p实例和p2实例都可以调用到running方法，这依赖于他们指向的位置是一样的，也就是Person原型，同时在调用实例对应的方法时，由于new调用改变了this的指向，实现了获取我们真正想要的name

### 类和 function 构造函数的对比
不管是class类还是function构造函数，他们所代表的含义都是相通的不同之处在于书写方式

```javascript
function Person1(name,age){
    this.age = age
    this.name = name
}
Person1.prototype.running = function(){}
Person1.prototype.eating = function(){}

var p1 = new Person1("coderwhy",35)
console.log(p1.__proto__ === Person1.prototype);//true
```

```javascript
class Person2{
    constructor(name,age){
        this.name = name
        this.age = age
    }
    running(){}
    eating(){}
}

var p2 = new Person2("小余",20)
console.log(p2.__proto__ === Person2.prototype);//true
console.log(Person2.prototype.constructor);
//指回类本身，跟上面Person1是一样的
console.log(typeof Person1,typeof Person2);
//都是function
```

不同之处在于class的边界更加清晰明确

+ function模拟出来的类只是普通的函数，而普通的函数是可以做到随便调用的
+ 但class类不行，class类只能通过new调用，在这里划定了非常清晰的调用界限，因为我们知道只有通过new调用的函数才是构造函数，才是我们的类，ES6之后的这点特性得到了强化



类 new 调用时的步骤

1. 在内存中创建一个新的对象 (空对象)
2. 这个对象内部的[[prototype]]属性会被赋值为该类的prototype属性
3. 构造函数内部的this，会指向创建出来的新对象
4. 执行构造函数的内部代码(函数体代码）
5. 如果构造函数没有返回非空对象，则返回创建出来的新对象

### 类的 Accessor
访问器（Accessor）是一种使用get和set关键字定义的特殊类型的方法，允许我们对对象的属性进行更加细致和控制的读取和修改操作。这些方法通常被称为访问器属性，包括一个获取器（getter）和一个设置器（setter）

虽然封装性更好，但前端用的情况不是特别的多，因为应用场景主要在于属性变化监听，而在正式的项目中，通常都由框架封装好了。如果想前端向后端过渡，可以多注意这方面的内容

![方式1](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728981957549-d636e09d-8ec1-4d43-86df-48d33af0cac3.png)![方式2](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728981973029-a583dd0e-2e82-4744-913c-5310fb6cd78a.png)

### 类的静态方法(类方法)
实例方法通常都是基于实例对象去进行调用的，也可以说通过创建出来的对象去进行一个访问。而类的静态方法，是通过类名去访问的，所以调用静态方法不需要通过实例对象，自然也就不需要new调用

+ 类方法(静态方法)：通过类调用的方法
+ 实例方法：通过实例对象调用的方法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728982323322-0a082bd8-60ef-436c-82e2-e0df75de7189.png)![static](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728982275586-da1ac134-0026-412d-bbaf-a0ba38e8d71e.png)

## 类的继承
### extends
使用extend可以继承父类的方法和属性

能够明确的继承内容(来自父类)有：构造函数、实例方法、静态方法、属性和访问器方法（getters 和 setters）、原型链

如果子类对父类的方法不满意，可以对父类的方法重写，不需要修改父类，只需要在子类中写一个相同名字不同作用的方法即可

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611175614-92680866-c11b-4370-99b8-a6c4a5594edb.png)

### super()
super 关键字在类的继承中起重要作用

主要用于调用父类的构造函数、方法和访问父类的属性。从而做到子类能够重用和扩展父类的功能

super的使用位置有三个：子类的构造函数、调用方法(实例方法/静态方法)

1. 使用 this 关键字之前必须调用 super()，这是因为在 JavaScript 的类继承机制中，如果一个类继承自另一个类，子类的构造函数必须先调用 super()，这样才能正确地初始化父类的构造函数
2. 从子类构造函数返回之前必须调用 super()，不过使用return返回的时候，我们一般都是在最后面才进行使用

通过super关键字调用父类的构造函数，将 name 和 age 属性正确地设置在子类的实例，确保了从父类继承的属性在子类实例上被正确初始化

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728982692550-73096a2f-ecb5-41df-9b51-9ad3c55cac51.png)



如果不满意父类的方法(不想在子类中使用)，可以直接在子类中重写方法，但大多数情况下，是部分满意，部分不满意，也就是不全部修改，只修改部分

那么可以在子类的重写方法中使用super关键字调用父类的对应方法

```javascript
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  static running(){
    console.log('逻辑1');
    console.log('逻辑2');
    console.log('逻辑3');
  }
}

class Student extends Person {
  constructor(name,age) {
    super(name, age)
  }
//重写静态方法
  static running1(){
    //父类方法融为子类方法的一部分
    super.running()
    console.log('逻辑4');
    console.log('逻辑5');
    console.log('逻辑6');
  }
}

Student.running1()//逻辑1-6
```

### Babel-了解
浏览器会随着时间的推移而不断发展，从而延伸出多种版本，而大多数的用户的浏览器并不会一直处在当前最新版本，甚至会处在一个较为落后的阶段，从而和代码产生兼容性的问题，这导致很多新的语法代码是没办法在这些地方运行的，但好在有工具babel能够解决这个问题

Babel 是一个广泛使用的 JavaScript 编译器，主要用于将采用最新 ECMAScript 标准编写的 JavaScript 代码转换为向后兼容的版本(例如ES5版本)。从而做到让开发者可以使用最新的 JavaScript 语法和功能，而无需担心兼容性问题，特别是在旧版浏览器和环境中



babel 处理过程

1. 解析（Parsing）：Babel首先将源代码解析成一个抽象语法树（AST），这个树结构描述了代码的语法结构
2. 转换（Transforming）：在这一步，Babel会遍历AST 并应用各种转换插件，这些插件可以修改、添加或删除节点，从而实现语法的转换
3. 生成（Generating）：最后，Babel将更新后的AST转换回JavaScript 代码，这个过程可能包括代码的美化和源码映射的生成



转化后的代码其实是比较多的，可读性和结构性是远不如原有代码来得好的

可以看到14行的class继承代码经过转化，暴增到133行。但并不是说14行的ES6代码需要一百多行的ES5代码才能实现。而是在这个转化过程当中，Babel会额外的进行边界处理，包括但不限于处理不同浏览器的兼容性问题、保持语义的一致性，以及包括一些运行时支持以确保新特性的正确执行

## 类的混入与解构
### 扩展继承内置类
在使用继承的时候，不仅可以继承自己写出来的类，还可以继承几乎所有的内置构造函数，包括但不限于以下几个：

+ Array：创建具有额外功能或自定义行为的数组类
+ String：创建扩展字符串处理功能的类
+ Map和Set：添加额外的数据结构方法
+ Promise.创建具有额外行为（如记录、性能监测等）的Promise类
+ Error：创建自定义错误类型，这在管理大型应用程序的错误时非常有用

默认的情况下，所有的类都继承自Object，也就是以下代码1与代码2所具备的含义是相同的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728983582760-9a4ae68d-23ad-426e-8694-2cd0f05614e5.png)

在以往，我们可以通过new运算符来调用具有构造函数的内置对象的实例，但这有一个缺点，在于内置对象的方法都是固定的，我们并不容易进行扩展，此时扩展继承内置类就有对应的作用了

我们知道想要让数组对象具备更多的方法是可以直接添加在数组的原型链身上的，但这样做并不好，需要一个中间缓冲层来进行定制化处理

在扩展继承内置类中，则是以类为中间层，基于继承来源进行二次的封装或者添加额外功能，而不影响继承来源本身，在使用的时候不需要 new 继承来源，而是直接new调用改动后的类

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728983723405-7e9ecf29-48ba-4e26-a5f1-b37ceadfcd58.png)

例子

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728984115397-2f2b7bec-0868-420f-bce4-c0beed92e686.png)

### 混入 mixin
它将一个类的方法和属性注入到另一个类中，从而达到功能组合和代码重用的目的，所以在这里的混入mixin是一种思想体现，而非新的语法

之所以出现混入这种做法，是因为我们extends继承只能够继承一个父类，当需要多个父类结合继承的时候，就难以做到

```javascript
// 工具函数：实现类的混入(残缺 缺陷版本 仅供参考)
function applyMixins(derivedCtor, baseCtors) {
  baseCtors.forEach(baseCtor => {
    Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
      derivedCtor.prototype[name] = baseCtor.prototype[name];
    });
  });
}

// 示例基类
class A {
  methodA() {
    console.log('Method A');
  }
}

class B {
  methodB() {
    console.log('Method B');
  }
}

// 目标类
class C {
  methodC() {
    console.log('Method C');
  }
}

// 应用混入
applyMixins(C, [A, B]);
//更好的实践角度，不影响原有类
//const MixedClass = createMixedClass([A, B]);

// 测试
const instance = new C();
instance.methodA(); // 输出: Method A
instance.methodB(); // 输出: Method B
instance.methodC(); // 输出: Method 
```

### JS多态
JS面向对象的三大特性之一，多态指不同数据类型进行同一个操作，表现出不同的行为

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611176555-882a7851-ce53-4457-bcfb-76fdaeecb752.png)

### 对象字面量的增强
+ 属性的简写：当key和value相同时，可以省略value，如name:name==name
+ 方法的简写：fn:function(){} == fn:()=>{}
+ 计算属性名：在对象外部定义一个变量，让这个变量的值作为对象的一个属性名，并给这个属性赋值；
    - 如变量的值是运算，则把运算后的结果当做属性名
    - 格式       [变量名]:属性值
    - ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1728985922589-2142f528-5669-4a64-83a3-21cf49be7035.png)

### 解构 Destructuring 
解构是一种特殊的语法，用来从数组或对象中获取数据

#### 数组的解构
+ 基本结构
    - ![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611176786-2a69a24a-475c-49f5-8875-764d5c344e1a.png)
+ 顺序结构：数组具有严格的顺序，如果想跳过中间的取后面的，可以用空格隔开
    - `var[name1,  ,name3]=names`
+ 解构出数组
    - 重命名：可以修改取出来的值
    - 在最后用 …数组名 结尾，可以把剩余的内容放进新数组中
    - `Var[name1='aaa',name2, …newNames]=names`
+ 默认值：如果取数的时候发现值是undefined，则可以在取出来的时候给设置默认值
    - `var[name1,name2 ,name3='默认值']=names`

#### 对象的解构
+ 基本结构
    - ![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611177021-da3f674c-9958-4edd-9545-21c1933908b1.png)
+ 任意顺序
+ 重命名：可以对取出来的值进行修改
    - Var { name : 'zff' , age}=obj
+ 默认值
    - `var {height , age = 30} = obj`

## Var/Let/const
### 基本用法
+ let和var没啥区别
+ const定义的数据一旦被赋值 ，便不可修改(常量)，但是定义对象时，可以修改对象的属性和方法
    - 数据在JS中分简单数据和复杂数据，前者放在栈中，后者放在堆中。
    - 常量所做的事情是锁住栈内容，在这种情况下，如果是简单数据类型，本身数据就在栈中无法改动。如果是复杂数据类型，则存储在栈中的内存地址 如0xb00将无法改变，这个表达形式为引用无法改变，但const常量并不对堆内容进行封锁，导致位于堆位置的内容是可以改变的
+ let和const不允许重复声明变量，var 可以

### 作用域提升
+ var声明的变量会进行作用域提升，导致var一旦声明就能够在所有地方进行使用，哪怕在声明之前使用也可以
+ 但是let和const定义的标志符在真正执行前，是不能被访问的，但是其实已经创建了
+ 从块级作用域的顶部一直到变量声明完成之前，这个变量处于暂时性死区 TDZ
    - ReferenceError: Cannot access 'name' before initialization

### Windows对象添加属性
内容存放的位置，取决了哪些地方可以访问到这些变量

在全局通过var来声明一个变量，事实上会在window上添加一个属性，window就是var的变量存储位置，所处位置是导致全局可访问可修改的根源

而let/const解决了"全局变量"的问题，自然这两者是不会给window上添加任何属性的，let/const所存储或者说所关联的位置叫做VE(变量环境)

每一个执行上下文会关联到一个变量环境（VariableEnvironment）中，在执行代码中变量和函数的声明会作为环境记录（EnvironmentRecord）添加到变量环境中。对于函数来说，参数也会被作为环境记录添加到变量环境中

### 词法环境
词法环境是一种规范类型，用于在词法嵌套结构中定义关联的变量、函数等标识符

+ 一个词法环境是由环境记录（Environment Record）和一个外部词法环境（outer Lexical Environment）组成
+ 一个词法环境经常用于关联一个函数声明、代码块语句、try-catch语句，当它们的代码被执行时，词法环境被创建出来

在 ES6 中，块级作用域包括两个环境

+ 变量环境 (VE)：主要用于存储由 var 声明的变量和函数声明
+ 词法环境 (LE)：用于存储由 let 和 const 声明的变量，以及包含块级作用域的其他信息

这也是let/const与var主要的不同来源：两个环境都具备类似的结构，但它们的关键区别在于如何处理变量和作用域

+ var 声明的变量因为没有块级作用域，所以它们被放在变量环境中
+ let 和 const 声明的变量具备块级作用域，它们被放在词法环境中，这允许 JS 引擎处理嵌套的块级作用域

### 块级作用域
ES6新增了块级作用域，通过let、const、function、class声明的标志符具备块级作用域的限制

+ 内层能够链式访问外层
+ 外层无法访问内层

但是函数虽然具有块级作用域，但是在{}外依然可以访问，因为不同的浏览器有不同的实现方式，大部分浏览器为了兼容以前的代码，放开该层限制，让function不具备块级作用域。这种操作很容易产生二义性，让人无法正确预估产生结果，所以类似这种有可能令人误解效果的代码应该少进行



包括说和{}所结合的结构，比如if判断语句、switch循环语句、for循环语句，其内部都是具备块级作用域效果的

我们在使用变量名的时候，很多情况下不可避免会遇到重复的情况，有一些地方会产生冲突，有些地方不会，块级作用域有明确的冲突界限

在不会冲突的地方，不应该让重复变量会产生关联，理解一个变量主要取决于两点：

1. 变量名本身
2. 变量所处环境

在不同环境下，相同的变量名是不同的含义，let变量在解决变量名冲突时，是根据第二点的因素去决定的，以场景作为切割，会让目的清晰明确

### 如何选择var、let、const
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729040481506-3c369294-d19f-4970-a410-7fb7bff49bba.png)

+ var 的缺陷：作用域提升、window全局对象、没有块级作用域等都是一些历史遗留问题
+ 优先推荐使用const，这样可以保证数据的安全性不会被随意的篡改
+ 只有当明确知道一个变量后续会需要被重新赋值时，这个时候再使用let

## 模板字符串
### 概念
模板字符串：用``编写的字符串

在模板字符串中，可以在字符串内部加上动态的变量，格式如 ${变量}

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611177448-10a734c5-5c1f-4017-b79a-a6991682a7cb.png)

### 标签模板字符串
这个函数接收模板字符串中的各个部分作为参数，包括字符串数组和插值表达的表达式值，并可以返回处理后的任何结果

事实上模板字符串被拆分了，第一个元素是数组，是被模块字符串拆分的字符串组合；后面的元素是模块字符串传入的内容；

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729041976917-236923a5-f6b4-4275-8596-2d707c0f0c5b.png)



## 函数的默认参数
### 概念
在ES6之前，编写函数是不可以有默认参数的，ES6之后就可以使用了

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611178869-75acda4a-5a31-4458-9735-ee6421547f44.png)

参数的默认值一般会放到最后，因为需要用到默认值的往往是不一定会用到的

默认值及之后的参数都不计算在length之内

### 箭头函数的补充
+ 箭头函数是没有显式原型prototype的，所以不能作为构造函数
+ 箭头函数也不绑定this、arguments、super参数
+ 以后要使用类，就不再使用function了，而是使用class

箭头函数不创建自己的 this 上下文，因此 this 有别于传统函数的动态作用域绑定。通过Babel，可以看出在箭头函数内部，所有对 this 的引用都被替换为对 _this 的引用。这样做确保了箭头函数体内的 this 值与其定义时所在的上下文中的 this 一致，与运行时的上下文无关

## 展开语法
### 概念
使用场景

+ 函数调用：简化多参数的数组传递
+ 构建新数组：合并多个数组或添加新元素时无需使用额外的数组方法如 concat
+ 对象拷贝和合并：方便地进行对象属性的拷贝或合并，无需额外库或复杂逻辑

```javascript
//1.基础使用
const names = ["coderwhy","小余","JS高级","李银河"]
function foo(name1,name2,...args){
  console.log(name1,name2,args);
}

foo(...names)
//或者也可以直接展开字符，不过用得很少
const str = "Hello"
foo(...str)

//2.构建新数组
const parts = ['shoulders', 'knees'];
const body = ['head', ...parts, 'toes'];
// body: ['head', 'shoulders', 'knees', 'toes']

//3. 对象合并
const obj1 = { foo: 'bar', x: 42 };
const obj2 = { foo: 'baz', y: 13 };
const mergedObj = { ...obj1, ...obj2 };
// mergedObj: { foo: 'baz', x: 42, y: 13 }  
// 注意 foo 的值是 obj2 中的，后者覆盖前者
```

和剩余参数的区别

+ 剩余参数是收集剩余的多个参数到一个集合（数组）中
+ 展开语法是将一个集合（数组或对象）分散成单独的元素或属性



展开操作本质上是浅拷贝，即复制对象或数组的第一层属性到一个新的对象或数组中。这意味着：

+ 原始类型的属性值：直接复制
+ 对象或数组类型的属性值：复制引用（指针），而不是对象本身，而这种内存地址指向同一堆内容的话，会导致修改一处，处处都跟着修改的结果

### 浅拷贝与深拷贝
+ 浅拷贝只复制对象或数组的第一层元素。
    - 如果被复制的元素是原始类型（如数字、字符串、布尔值），则直接复制值。
    - 如果元素是对象或数组，则复制引用（即指针），而不是复制对象或数组本身
+ 深拷贝会复制对象的所有层级，创建一个完全独立的副本。不仅第一层的元素被复制，所有更深层的元素也都被递归复制，因此修改新对象不会影响原始对象
+ 浅拷贝是复制了栈的内容，而深拷贝是复制堆的内容

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729043417185-54724e3e-fa03-4ca8-9702-804b633ff4c5.png)

## Symbol
### 概念
在ES6之前，对象的属性名都是字符串形式，那么很容易造成属性名的冲突如原来有一个对象，我们希望在其中添加一个新的属性和值，但是我们在不确定它原来内部有什么内容的情况下，很容易造成冲突，从而覆盖掉它内部的某个属性

Symbol就是为了解决上面的问题，用来生成一个独一无二的值

+ Symbol值是通过Symbol函数来生成的，生成后可以作为属性名
+ 也就是在ES6中，对象的属性名可以使用字符串，也可以使用Symbol值，Symbol不与字符串划等号



总结一下

+ 它用于创建一个独一无二的标识符
+ 主要用途是作为对象属性的键，而这些属性是独一无二的，可以防止属性名的冲突
+ Symbol函数执行后每次创建出来的值都是独一无二的
+ 可以在创建Symbol值的时候传入一个描述description

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729044430946-b8688a80-b5ab-4cdf-84f4-c7f7023bc79a.png)



从内存分配角度来说，每个 Symbol 值都指向一个独立的内存地址。

在该基础上，Symbol本身都具备一个唯一标识，每次调用 Symbol() 时，由 JavaScript 引擎生成。这个标识是隐蔽的，开发者无法访问。虽然标识无法访问，但描述是可以的，通过对应的实例属性进行获取

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729044541095-b28685b3-83c7-4fc5-ac96-8520bfd9f30c.png)

### symbol 作为对象的 Key
在key中使用Symbol，需要使用动态属性名写法 [] 进行扩起来，如`[name]:'zff'`

方括号允许使用任何表达式作为键名，包括了 Symbol，从而确保 Symbol 值被正确解析和访问，并且Symbol不会被动态属性名转化为字符串



在原有对象基础上去新增内容时，也有不同

+ 正常的新增属性中，我们使用.操作符访问属性
+ 而使用方括号 [] 访问属性允许动态确定属性名，属性名可以是变量，也可以是任意表达式的结果

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729044839553-e723634e-923e-4729-97e8-59e0ffbdc7eb.png)



注意，使用Symbol作为key的属性名的话，在遍历/Object.keys等情况中，获取不到这些Symbol值

+ 使用getOwnPropertyNames方法只能获取字符串形式的属性名
+ 对于Symbol类型的属性名，有相似的方法getOwnPropertySymbols去进行获取

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729044954992-6df526f2-0374-4a49-a35e-b69ac8bdbbc8.png)

### symbol 方法
Symbol的方法分为实例方法和静态方法

1. 实例方法：toString、valueOf 和  [Symbol.toPrimitive]() 方法，前两个比较熟悉，而后面这个则用于将Symbol对象转为symbol值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729045776533-9806c182-34e4-480a-bd30-02c98ef14645.png)

2. 静态方法：Symbol.for(key)、Symbol.keyFor(sym)

两个方法都是以Symbol注册表为根基的，从而造成该方式与普通Symbol是不同用法的形式，在正常的使用中，需要先填入内容到该表中，才能进行查询获取

for方法将Symbol本身以及对应的描述字符串，存入Symbol注册表中，所以此时的Symbol.keyFor方法就可以，从注册表中获取到对应的描述字符串

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729045859054-a654158a-6cf2-49b2-acb7-612cec6e9ec5.png)

使用目的：解决全局状态管理的问题，在复杂的应用或多个合作组件间需要共享某些全局状态或标识时，能够得以实现（pinia、redux）

## Set
### 基本用法
+ 新增的数据结构，可以用来存储数据，类似于数组
+ 和数组的区别是元素不能重复，创建时需要使用Set 构造函数(目前无法通过字面量创建)
+ Set非常常用的功能就是给数组去重
+ Set支持for of遍历

```javascript
const numbers = [2, 3, 4, 4, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7, 5, 32, 3, 4, 5];

// 使用 Set 来去除重复元素，得到的是 Set 类型的结果
const numberSet = new Set(numbers); // Set(7) { 2, 3, 4, 5, 6, 7, 32 }
console.log(numberSet);

// 将 Set 转化为数组类型
const uniqueNumbers1 = Array.from(numberSet);
console.log(uniqueNumbers1); // [2, 3, 4, 5, 6, 7, 32]

// 另一种将 Set 转换为数组的方法
const uniqueNumbers2 = [...numberSet];
console.log(uniqueNumbers2); // [2, 3, 4, 5, 6, 7, 32]
```

### 常见方法
+ Size：返回Set中元素的个数
+ add(value)：添加某个元素，返回Set对象本身
+ delete(value)：从set中删除和这个值相等的元素，返回boolean类型
+ has(value)：判断set中是否存在某个元素，返回boolean类型
+ clear()：清空set中所有的元素，无返回值
+ forEach(callback，[thisArg])：通过forEach遍历set

```javascript
const numbers = [2, 3, 4, 4, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7, 5, 32, 3, 4, 5];
const set = new Set(numbers)
//常见属性：获取数组大小(个数)
console.log(set.size);//7
//常见方法：增、删、判断存在(查)、清空、遍历这5种常见功能
//增
set.add("666")
console.log(set);//Set(8) { 2, 3, 4, 5, 6, 7, 32, '666' }
//删
set.delete(2)
console.log(set);//Set(7) { 3, 4, 5, 6, 7, 32, '666' }
//判断存在
console.log(set.has("666"));//true
//forEach遍历
set.forEach(item => console.log(item))//3 4 5 6 7 32 '666'
//清空
set.clear()
console.log(set);//Set(0) {size: 0}
```

## Weakset
### 概念
和set类似，内部元素也是不能重复的数据结构，const wset = new WeakSet（）

和set区别就是

+ Weakset内部只能存放对象类型，不能存放基本数据类型
+ 对元素的引用是弱引用，如果没有其他引用对某个对象的引用，那么GC会对该对象进行回收

常见的方法

+ add(value)：返回weakset对象本身
+ delete(value)：从weakSet中删除和这个值相等的元素，返回boolean类型；
+ has(value)：判断WeakSet中是否存在某个元素，返回boolean类型：

weakest的应用：因为weakset不能遍历，所以存储到weakset中的对象是没办法获取的

```javascript
const ws = new WeakSet();
const foo = {};
const bar = {};
//add 添加方法
ws.add(foo);
ws.add(bar);
// has判断方法
ws.has(foo); // true
ws.has(bar); // true
// delete 删除方法
ws.delete(foo); // 从 set 中删除 foo 对象
ws.has(foo); // false，foo 对象已经被删除了
ws.has(bar); // true，bar 依然存在
```

### 弱引用和强引用
强引用是指一个对象被另一个对象或变量直接引用，只要这种引用关系存在，垃圾回收器就不会回收被引用的对象。这意味对象会持续保留在内存中，直到所有引用它的变量都不再指向它。

变量、对象属性、数组元素等几乎所有的日常使用都是通过强引用进行的，并且需要牢记什么是引用类型，常见的如下：

+ 变量引用：在函数或全局作用域中定义的变量
+ 属性引用：对象属性指向另一个对象，形成了一个引用关系
+ 数组元素：数组中的元素可以是对象，数组对这些对象的引用也是强引用
+ 函数闭包：函数可以通过闭包引用外部作用域中的变量或对象，这些也都是强引用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729047467233-3ae1c369-aa28-4352-bbc1-b3620a621a02.png)

### 应用
WeakSet不能遍历，如果遍历获取到其中的元素，那么有可能造成对象不能正常的销毁

call、apply 这些方法在调用时会进行创建，当调用结束后就会进行销毁，并不是长期都需要这个this，而是在创建时需要进行判断。使用WeakSet来进行判断时，就很适合调用时跟随创建，当方法销毁后，不存在强引用之后，pWeakSet也会跟着被垃圾回收。弱引用是用在该地方，避免长期持有造成不必要的引用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729048772774-f0fb0cdd-9fd9-4b3e-a4a7-dd4321e94ac2.png)



## Map
### 概念
新增数据结构，用于存储映射关系，也就是键值对

和对象很类似，都是key-value，但是不同的是map可以把很多类型作为key，而对象只能用字符串(ES6 新增的 Symbol)

某些情况下可能希望通过其他类型作为key，比如对象作为key，但这是不可以的，这个时候键值对会自动将对象转成字符串来作为key

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611180710-32030ba0-de80-4afa-acbd-375d9a70d0e5.png)

### 常见方法
+ size：返回Map中元素的个数
+ set(key,value)：在Map中添加key、value，并且返回整个Map对象
+ get(key)：根据key获取Map中的value
+ has(key)：判断是否包括某一个key，返Boolean类型
+ delete(key)：根据key删除一个键值对，返回Boolean类型
+ clear()：清空所有的元素
+ forEach(callback,[thisArg])：通过forEachi遍历Map

```javascript
const info = {name:"小余"}
const info2 = {name:"why"}

//Map映射类型
const map = new Map()
//set方法，设置内容
map.set(info,"XiaoYu")
map.set(info2,"coderwhy")

//Map的常见属性
console.log(map.size);//2

//get方法，获取刚刚设置的内容
console.log(map.get(info));//XiaoYu

//delete方法，删除内容
map.delete(info2)
console.log(map);
//Map(1) { { name: '小余' } => 'XiaoYu' }，info2内容删除成功

//has判断
console.log(map.has(info2));//false，刚刚删除掉了，所以是false

//clear清空内容
map.clear()
console.log(map);//Map(0) {size: 0}

//forEach方法，遍历map中设置的内容
map.forEach((item,key) => {
    console.log(item,key,"forEach水印");
//XiaoYu { name: '小余' } forEach水印 coderwhy { name: 'why' } forEach水印
});

//for of遍历迭代，使用这种方式可以拿到里面的具体内容，更加细化
//for([key,value] of map)
for(item of map){
  console.log(item,"for of水印");
  //遍历1：[ { name: '小余' }, 'XiaoYu' ] for of水印
  //遍历2：[ { name: 'why' }, 'coderwhy' ] for of水印
  [key,value] = item
  console.log(key,value);
  //遍历1：{ name: '小余' } XiaoYu
  //遍历2：{ name: 'why' } coderwhy
}
```

## weakmap
和map类似，内部元素也是不能重复的数据结构，`const wMap = new WeakMap()`

和map区别就是

+ Weakmap 内部只能存放对象类型，不能存放基本数据类型
+ 对对象 的引用是弱引用，如果没有其他引用对某个对象的引用，那么GC会对该对象进行回收

常见的方法

+ set(key,value)：在Map中添加key、value,并且返回整个Map对象
+ get(key)：根据key获取Map中的value
+ has(key)：判断是否包括某一个key,返Boolean类型
+ delete(key)：根据key删除一个键值对，返回Boolean类型

WeakMap 作用：Vue3的响应式原理，可以考虑在学习Vue时，再去深入研究

# 8. ES7~ES12
## ES7
### Array.includes
在ES7之前，如果我们想判断一个数组中是否包含某个元素，需要通过 indexOf 获取结果，并且判断是否为 -1

在ES7中，我们可以通过includes来判断一个数组中是否包含一个指定的元素，根据情况，如果包含则返回 true，否则返回false

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611181059-456a13d8-95bc-4e86-988f-1c0ffe88eac1.png)

### 指数运算符
在ES7之前，计算数字的乘方需要通过 Math.pow 方法来完成。在ES7中，增加了 ** 运算符，可以对数字来计算乘方

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729060931740-4cc7aa8c-de2c-46e1-b5f5-c09bd51b028d.png)

## ES8
### Object.values
之前我们可以通过Object.keys获取一个对象所有的key，在ES8中提供 Object.values来获取所有的value值

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611181569-fe3ccd6b-813f-4f47-b36e-fb0ed9701a0c.png)

### Object.entires
通过Object.entries可以获取到一个数组，数组中会存放可枚举属性的键值对数组

对于数组与字符串而言，被entries方法解析，将索引作为key(始终是字符串)，内容则被拆解作为value值

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611182021-1659f0f6-1158-4e4f-bae7-c84ea94b16de.png)



### String Padding
某些字符串我们需要对其进行前后的填充，来实现某种格式化效果，ES8中增加了padStart和padEnd方法，分别是对字符串的首尾进行填充的

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611182422-bf53cb2e-66cc-428a-a970-3edfd56f4eeb.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611182714-fc402752-0a19-45c3-baa2-fdc31c278382.png)

### Object Descriptors
`Object.getOwnPropertyDescriptors`，获取一个对象其对应的描述符，前面有提到

## ES9
异步迭代器 async iterators，后面详细讲

展开运算符，前面提到过

promise 的 finally，后面详细讲

## ES10
### flat & flatMap
Flat()方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729061816390-5681dc2a-118c-4b7c-a269-3af35daa8eaf.png)

flatMap()方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组

注意

+ 相当于map方法和flat方法的结合体：`arr.map(...args).flat()`，但比分别调用这两个方法稍微更高效一些
+ flatMap是先进行map操作，再做flat的操作
+ flatMap中的flat相当于深度为1
+ 如果需要添加深度，那就链式调用即可`flatMap(xxx).flat(叠加深度)`

```javascript
const message = [
  "Hello World",
  "Hello 小余",
  "出来了，一只coderwhy"
]

const newMessage1 = message.flatMap(item => item.split(" "))
console.log(newMessage1);
//['Hello', 'World', 'Hello', '小余', '出来了，一只coderwhy']
```

### Object.fromEntries
那么如果有一个entries键值对数组，可以用Object.fromEntries转换成对象

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611183745-8bcd841b-d347-476e-bec5-3afb8a3c53d5.png)

### trimStart & trimEnd
Trim可以去除收尾的空格， ES10 提供了trimStarti和trimEnd用来单独去除字符串前面或者后面

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729062805711-2ae1a022-4583-4842-a08d-51246d297adc.png)

## ES11
### Bigint
用于表示大的整数，表示方法是在数值的后面加上n

n 在这里起到标识作用，明确这是一个 BigInt 类型的数字，而不是普通的 Number。主要避免类型混淆

特别是进行数值计算时，BigInt 和 Number 不能直接进行运算，必须显式转换。且转换需要小心，因为用到BigInt的数值都很大，转为Number数字有可能出现精度丢失的问题，所以建议仅在值可能大于 2^53 时使用 BigInt 类型，并且不在两种类型之间进行相互转换

因此BigInt和Number是两个不同的类型，不能用Math对象中的方法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729063697859-e26b986d-1791-4dec-80d3-c0fa487737cb.png)

在表达BigInt类型时，我们可以直接在数字后面加n来定义BigInt，会自动识别，也可以调用函数 BigInt()（但不包含 new 运算符）并传递一个整数值或字符串值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729063692815-64206852-05e3-46c7-8d73-57f147c43401.png)

### for in
在ES11之前，虽然很多浏览器支持for...in来遍历对象类型，但是并没有被ECMA标准化

for...in是用于遍历对象的key

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611184334-f51f9417-8dbc-4f21-8037-31bc08ff8769.png)

### 控制合并运算符
代码形式为 ??，用于逻辑判断

当左侧的操作数为 null 或者 undefined 时，返回其右侧操作数，否则返回左侧操作数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729064729860-f3a71bed-ac4e-487d-8fde-4260d32ce8d1.png)

### 可选运算符
可选运算符，用  ?.  表示，可以用于函数调用、属性访问和null值的检查

`obj?.friend?.running?.()` 有就执行，没有就不执行

### Global This
在之前我们希望获取JavaScript环境的全局对象，不同的环境获取的方式是不一样的

+ 在浏览器中可以通过this、window来获取
+ 在Node中我们需要通过global来获取

在ES11中对获取全局对象进行了统一的规范：globalThis，这是内置对象的一种，能够自动判断当前环境(浏览器 or Node)，从而进行切换可使用的全局对象，因此可以放心使用，而不用担心运行环境

基本原理是在 JavaScript 引擎的启动或初始化阶段，识别环境并将相应的全局对象赋值给 globalThis，也就是环境检测、赋值操作(赋值对应的全局对象)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729065269970-cae99fa1-bb31-4139-97a3-1fccbe048b48.png)

## ES12
### FinalizationRegistry-了解
Finalization Registry提供：当一个在注册表中注册的对象被回收时，请求在某个时间点上调用一个清理回调。（清理回调有时被称为finalizer)

可以通过调用register方法，注册任何你想要清理回调的对象，传入该对象和所含的值

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611184609-d75efad3-8708-40cf-93ca-64e55c3e8932.png)

### WeakRefs
如果我们默认将一个对象赋值给另外一个引用，那么这个引用是一个强引用；如果我们希望是一个弱引用的话，可以使用WeakRef

当new创建WeakRef对象时，初始化传入的参数是WeakRef 要指向的目标对象 

WeakRef对象只有一个原型方法deref，用于返回 WeakRef 的目标对象，如果该对象已被垃圾收集，则返回undefined

通过WeakRef使用的数据，都属于弱引用，且必须通过deref方法调用才能使用

```javascript
let info = {name:"xiaoyu",age:20}

let obj1 = new WeakRef(info)

console.log(obj1.name);//undefined

console.log(obj1.deref().name);//xiaoyu
```

### 逻辑赋值判断
ES12中对一系列逻辑赋值运算符多出另一种使用的可行性：赋值

+ 逻辑或 || 进阶为逻辑或赋值||=
+ 空值合并 ?? 进阶为逻辑空赋值 ??=
+ 逻辑与 && 进阶为逻辑与赋值 &&=

当加上赋值功能后，判判定条件不变,返回值将赋值于左侧，用于设置默认值是非常不

错的选择

```javascript
function bar(message){
  //以前判断传入有没有值，通常这么做
  message = message || "默认值"
  
  //上面这种方式已经简化了
  message ||= "默认值"
  
  //前面的方式对空字符串，0，false，之类的无能为力。这种方式则可以解决
  message = message ?? "默认值"
  
  //那这种方式也能够简化的啦
  message ??= "默认值" 
  console.log(message);
}
```

### String.replaceAll
字符串全体替换，但不改变原数据，返回新数据

```javascript
const name = "参与共学计划，一起成长进步，一起携手同行"
const newName = name.replace("共学计划","JS共学")//只会修改第一个遇到的
const newName1 = name.replaceAll("一起","共同")//会将全部都进行修改
console.log(newName);//参与JS共学，一起成长进步，一起携手同行
console.log(newName1);//参与共学计划，共同成长进步，共同携手同行
```

# 9. Proxy-Reflect
## Proxy
### 概念
Proxy 其实就是对象的代理，对象属性的监听操作都由代理执行

如果我们希望监听一个对象的相关操作，那么我们可以先创建一个代理对象（Proxy对象），之后对该对象的所有操作，都通过代理对象来完成，代理对象可以监听我们想要对原对象进行哪些操作

创建proxy

+ `const p = new Proxy (target, handler)`
+ handler 是处理对象，在该对象里面有各种属性，这些属性会在我们对Proxy代理执行操作时，拦截下对应的一些操作
+ handler对象内的属性又称为拦截器，一共13个，都可以算成handler对象的实例方法
+ 每一个handler实例方法都和正常对象的某个实例方法对应上，从而实现拦截。例如我们实现监听对象的"查找"、"改动"这两个操作，不管是defineProperty还是Proxy的实现方式，都是使用set与get方法

### proxy 的捕获器
捕获器是如何生效的

1. 拦截机制：JS引擎会在内部，为Proxy对象维护一个关联的目标对象和处理器对象。当对Proxy对象进行操作时，这些操作首先被送到处理器对象
2. 方法查找与执行：对于每种可以拦截的操作，如get、set、apply等，处理器对象可以提供一个同名的方法来拦截相应的操作，在处理器对象中查找到对应方法进行执行



常用的是 set、get、has、deleteProperty

get(target, property, receiver)：

+ 拦截对象属性的读取操作
+ target 是被代理的对象
+ property 是被访问的属性名
+ receiver 是调用属性时的上下文（this 值）



set(target, property, value, receiver)：

+ 拦截对象属性的设置操作
+ target 是被代理的对象
+ property 是被设置的属性名
+ value 是要设置的值
+ receiver 是调用属性时的上下文（this 值）



has(target, property)：

+ 拦截 in 操作符，用于检查对象是否具有某个属性
+ target 是被代理的对象
+ property 是要检查的属性名



deleteProperty(target, property)：

+ 拦截 delete 操作符，用于删除对象的属性
+ target 是被代理的对象
+ property 是要删除的属性名

```javascript
const target = {};
const handler = {
  get(target, property, receiver) {
    return `Property '${property}' has been read.`;
  },
  set(target, property, value, receiver) {
    console.log(`Property '${property}' set to '${value}'.`);
    return true; // 表示成功
  },
  has(target, property) {
    return `${property} is present in the object.`;
  },
  deleteProperty(target, property) {
    console.log(`Property '${property}' has been deleted.`);
    return true; // 表示成功
  }
};

const proxy = new Proxy(target, handler);

console.log(proxy.a); // Property 'a' has been read.
proxy.a = 'test'; // Property 'a' set to 'test'.
console.log('a' in proxy); // a is present in the object.
delete proxy.a; // Property 'a' has been deleted.
```

## Reflect
ES6新增对象，不能使用New创建，类似于Math内置对象，提供了很多操作对象的方法，类似于Object操作对象的方法，比如Reflect.getPrototypeOf() ，类似于 Object.getPrototypeOf()

+ 这是因为在早期的ECMA规范中没有考虑到这种对对象本身的操作，如何设计会更加规范，所以将这些API放到了Object上面；
+ 但是Object作为一个构造函数，这些操作实际上放到它身上并不合适
+ 还包含一些类似于 in、delete操作符，让JS看起来是会有一些奇怪
+ 所以在ES6中新增了Reflect，让这些操作都集中到了Reflect对象上
+ 在使用Proxy时，可以做到不操作原对象

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729072687968-70757fda-f3db-4b3e-b9db-9b185744c65f.png)

常见方法

+ Reflect. has(target，propertyKey)：判断一个对象是否存在某个属性，和in运算符的功能完全相同。
+ Reflect. get(target，propertyKey[,receiver])：获取对象身上某个属性的值，类似于target[name]
+ Reflect. set(target，propertyKey，value[，receiver])：将值分配给属性的函数。返回一个Boolean，更新成功返回true。
+ Reflect. deleteProperty(target，propertyKey)：作为函数的delete操作符，相当于执行delete target[name]

```javascript
// 定义一个简单的对象
const obj = {
    name: "coderwhy",
    age: 30
};

// 使用 Reflect.has() 检查对象中是否存在指定的属性
console.log("检查 'name' 是否存在:", Reflect.has(obj, 'name'));  // 输出 true
console.log("检查 'gender' 是否存在:", Reflect.has(obj, 'gender'));  // 输出 false

// 使用 Reflect.get() 获取对象属性的值
console.log("Name:", Reflect.get(obj, 'name'));  // 输出 "coderwhy"
console.log("Age:", Reflect.get(obj, 'age'));  // 输出 30

// 如果属性不存在，可以提供一个默认值
console.log("Gender:", Reflect.get(obj, 'gender', 'Not specified'));  // 输出 "Not specified"

// 使用 Reflect.set() 设置对象属性的值
Reflect.set(obj, 'age', 31);  // 设置 age 属性为 31
console.log("Updated Age:", obj.age);  // 输出 31

// 使用 Reflect.deleteProperty() 删除对象的一个属性
Reflect.deleteProperty(obj, 'name');  // 删除 name 属性
console.log("Name after deletion:", obj.name);  // 输出 undefined

// 再次使用 Reflect.has() 检查 name 属性是否还存在
cname' 删除后是否存在:onsole.log("检查 'name' 删除后是否存在:", Reflect.has(obj, 'name'));  // 输出 false
```

## receiver
receiver参数较难理解，是位于Proxy、Reflect这两者的get与set方法中的最后一个参数

如果我们的源对象有setter、getter的访问器属性，那么可以通过receiver来改变里面的this

![对象的setter、getter的监听层效果](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729072855792-1b45569e-4f65-484c-99c6-8749ec9fb3a6.png)

此时加上我们的Proxy代理层和对应的get、set捕获器，以及对应的Reflect的set、get方法来实现

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729072904317-5cb0ac60-9d7d-4b3f-9599-4efb8e0d13b1.png)

那么顺序就是

+ get：实际使用->Proxy代理层get拦截->Reflect.get触发->监听层getter触发->从数据源中获取到数据->返回查询结果
+ set：实际使用->Proxy代理层set拦截->Rflect.set触发->监听层setter触发->修改数据源

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729073088533-87ee5703-5127-4f7a-b615-d4fe330991f8.png)

# 10. Promise
## 基础使用
Promise是一个类，是 JavaScript 中用于异步编程的一种模式，它代表了异步操作的最终完成（或失败）及其结果值

当需要给予调用者一个承诺："待会儿我会给你回调数据"时，就可以创建一个Promise的对象

用 new 创建 Promise 对象，接受一个回调函数 executor，这个回调函数会立即执行

executor 中需要传入两个参数

+ reslove：
    - pending => fullfilled
    - 调用resolve回调函数时，会执行then方法传入的回调函数
+ reject：
    - pending => rejected
    - 当调用reject回调函数时，会执行catch方法传入的回调函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729147879349-71179c8c-2d00-420b-81b8-8f8e04fc2bf2.png)

一旦状态被确定下来，Promise的状态会被锁死，该Promise的状态是不可更改的

在调用resolve的时候，如果resolve传入的值本身不是一个Promise，那么会将该Promise的状态变成兑现（fulfilled），之后就无法调用 reject 了

## 传入不同值给 reslove
1. 传入一个普通的值或者对象，那么这个值会作为then回调的参数
2. 传入另外一个Promise，那么这个新Promise会决定原Promise的状态
3. 传入一个对象，并且这个对象有实现then方法，那么会执行该then方法，并且根据then方法的结果来决定Promise的状态

![第一种](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729148033700-38ddd2be-7886-4011-b115-2b7174ba608b.png)

![第二种](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729148063509-2511be5a-4aab-4fdd-855f-d1b786b2e111.png)

![第三种](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729148101816-98f736ef-70be-4fb8-b72c-96aaffef3aae.png)

## Promise 的方法
Promise分为**对象方法**和**类方法**，在MDN文档中分别表示为**实例方法**和**静态方法**

实例方法：在Promise实例上调用，用于处理Promise对象的状态变化

静态方法：直接在Promise构造函数上调用的，非常适合封装各种工具函数

### 实例(对象)方法
#### then方法
接受两个参数

+ fulfilled的回调函数res：当状态变成fulfilled时会回调的函数
+ reject的回调函数err：当状态变成reject时会回调的函数

但是更推荐.then.catch的写法

```javascript
promise.then(
  res => {console.log('res:', res);}, 
  err => {console.log('err:', err);}
)
```



一个Promise的then方法是可以被多次调用的，这里的多次调用并非指链式调用，需要进行区分

当promise的resolve方法被回调时，所有基于该promise的then方法都会被调用

```javascript
const promise = new Promise((resolve, reject) => {
  resolve("小余")
})
//多次调用
promise.then(res => {
  console.log("res1:", res)
})

promise.then(res => {
  console.log("res2:", res)
})

promise.then(res => {
  console.log("res3:", res)
})

// res1: 小余
// res2: 小余
// res3: 小余
```



then方法是有返回内容的

+ 可以是一个普通值，该值会被作为一个新的Promise的resolve值，这是能够使用链式调用的原因

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729149098218-e2b451eb-ff69-4c12-8bde-15a3be7b6f28.png)

+ 也可以直接自行编写新的Promise进行返回，该返回具备更高自由度和调整空间

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729149154473-dd4dcf8d-63e8-4bec-9ed0-df1a4f75ce78.png)

+ 返回一个具有 then 方法的对象（thenable 对象），这允许自定义解决过程

> thenable 对象指的是任何具有 then 方法的对象
>
> thenable 对象可以被 Promise 机制识别，并被处理为 Promise，这就是它的作用，也是Promise识别其对象中自己实现then方法的原因
>
> 因此开发者可以创建自定义的 thenable 对象，而不必严格继承或依赖于特定的 Promise 类，拥有更大的灵活性。通过实现 then 方法，简单的对象也可以参与到 Promise 的链式调用和异步处理过程中，统一了异步操作的接口
>

#### catch方法
catch方法具备两种调用形式，隐式与显式

+ 隐式调用：then方法第二参数
+ 显式调用：then方法之后接着链式调用catch方法
+ 推荐使用显示调用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729149458490-abe48eb8-e1a9-4aa0-8bf9-939e1f7849df.png)



一个Promise的catch方法是可以被多次调用的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710473266857-6add0b96-54b3-43bb-a653-9dff728c407d.png)



由于.catch()返回一个新的Promise，可以在其后面继续调用.then()或.catch()方法，形成链式调用

+ **错误传播**：如果回调函数返回一个被拒绝的Promise或抛出错误，那么返回的Promise将变为rejected状态，并且错误将传递给链中的下一个.catch()方法
+ **无返回值**：如果回调函数没有返回任何值（即返回undefined），那么返回的Promise将被解决，其解决值将是undefined
+ **非函数参数**：如果参数不是函数，则这些参数将被忽略，Promise将继续以原始状态向下传递
+ **多个 catch**： 可以在Promise链中使用多个.catch()方法，每个.catch()都可以处理特定类型的错误，或者为不同的错误提供不同的处理逻辑。



小题目

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729149986196-9553dfe8-1444-4a9b-b605-90c87c953af2.png)

如上代码的返回值，在这次处理中，分为四阶段：then(阶段一）=> catch(阶段二) => then(阶段三) => catch(阶段四）

阶段一无法处理错误，直接跳到阶段二，如果在第二阶段已经捕获错误了，此时再返回内容，那打印出来的是阶段三的内容还是阶段四的内容？

答案是输出阶段三，因为第一个.catch()被执行，它会打印错误并返回"catch return value"。此时返回的Promise对象的状态是fulfilled，并且它的值是"catch return value"。因此控制流会跳过第二个.catch()，直接进入第二个.then()，这里的回调函数会接收到"catch return value"

#### finally方法
无论Promise对象无论变成fulfilled还是rejected状态，最终都会被执行的代码

finally方法是不接收参数的，因为无论前面是fulfilled状态，还是rejected状态，它都会执行

如果想在 promise 敲定时进行一些处理或者清理，无论其结果如何，那么 finally() 方法会很有用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710483454841-62fd78ec-0116-4df9-94cf-a34dce41e825.png)

### 静态(类)方法
#### resolve方法
有时候已经有一个现成的内容了，希望将其转成Promise来使用，这个时候我们可以使用 Promise.resolve 方法来完成

Promise.resolve的用法相当于new Promise，并且执行resolve操作，相当于一种快捷方式来创建一个已经处于兑现（fulfilled）状态的 Promise

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729150736480-431ba838-7ac6-4177-b29b-673829d75d24.png)

#### reject方法
将Promise对象的状态设置为reject状态Promise.reject的用法相当于new Promise，只是会调用reject

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729150763372-09d2299c-fd7c-4a93-82fc-6ca3c2c6e03d.png)

Promise.reject传入的参数无论是什么形态，都会直接作为reject状态的参数传递到catch的

#### all方法
将多个 Promise 实例包裹在一起形成一个新的 Promise 实例

新Promise状态由包裹的所有Promise共同决定

+ 当所有的Promise状态变成fulfilled状态时，新的Promise状态为fulfilled，并且会将所有Promise的返回值组成一个数组
+ 当有一个Promise状态为reject时，新的Promise状态为reject，并且会将第一个reject的返回值作为参数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710483880592-b381ddb5-7b9b-4c68-b888-0de9711a9dc0.png)

#### allSettled方法
all方法有一个缺陷：当有其中一个Promise变成reject状态时，新Promise就会立即变成对应的reject状态。那么对于resolved的，以及依然处于pending状态的Promise，是获取不到对应的结果的

在ES11中，添加了新的API-Promise.allSettled，设计目的就是为了可以观察多个 Promise 的结果，无论它们是 fulfilled 还是 rejected ，结果一定是fulfilled的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729152075587-2a408650-2fab-4606-9307-758675ccf530.png)

```javascript
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject(new Error("失败")),
  Promise.resolve(3)
]).then(results => {
  console.log(results);
  // 输出：
  // [
  //   { status: 'fulfilled', value: 1 },
  //   { status: 'rejected', reason: Error: 失败 },
  //   { status: 'fulfilled', value: 3 }
  // ]
});
```

#### race方法
如果有一个Promise有了结果，就让他决定最终新Promise的状态

race方法表示竞争，多个Promise相互竞争，谁先有结果，那么就使用谁的结果

这在多源数据获取或具有多个备用请求的网络请求中特别有用，可以减少等待时间快速响应，提高应用性能

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710483970016-53e9e9b9-1469-49a0-9cdf-2029a737240f.png)

#### any方法
any方法是ES12中新增的方法，和race方法是类似的

any方法会等到一个fulfilled状态，才会决定新Promise的状态

如果所有的Promise都是reject的，那么也会等到所有的Promise都变成rejected状态，会报一个AggregateError的错误

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710484087442-e934a4c1-c6e9-454f-b9fa-adcfed9296ed.png)

因此rece与any的区别，在于目标导向不同

1. rece追求第一个结果
2. any方法追求第一个好的结果

# 11. Iterator 迭代器
## 概念
迭代器就是一个让用户在容器对象（例如链表或数组）上遍访的对象，使用该接口无需关心对象的内部实现细节(细节指我们不需要关心他是数组还是链表或者说是哈希表、树结构这些，统统不需要关注)

在JavaScript中，迭代器也是一个具体的对象，如果我们想要将一个对象变为迭代器，这个对象需要符合迭代器协议：迭代器协议定义了产生一系列值（无论是有限还是无限个）的标准方式，这在JavaScript中这个标准就是一个特定的next方法。换个说法则是：next方法在js中决定了产生值的方式，用于按顺序生成序列中的下一个值



next方法有如下的要求：

+ 一个无参数或者一个参数的函数，返回一个拥有以下两个属性的对象：
+ done（boolean）
    - 如果迭代器可以产生序列中的下一个值，则为 false
    - 如果迭代器已将序列迭代完毕，则为 true。这种情况下，value 是可选的，如果它依然存在，即为迭代结束之后的默认返回值
+ value
    - 迭代器返回的任何 JavaScript 值。done 为 true 时可省略。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711350876606-e8da4747-7dd1-48c0-a309-9a19a6d49f56.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711350978432-9b121565-7f13-46c1-b605-559209e8c38f.png)

## 可迭代对象
但是上面的代码整体来说看起来是有点奇怪的：我们获取一个数组的时候，需要自己创建一个index变量，再创建一个所谓的迭代器对象，可以对上面的代码进行进一步的封装，让其变成一个可迭代对象；

可迭代对象：当一个对象实现了iterable protocol协议时，它就是一个可迭代对象，有两个要求

+ 必须实现一个特定的函数[Symbol.iterator]
    - Symbol.iterator 是一个特殊的Symbol值，用于获取一个对象的默认迭代器
+ 函数必须返回一个迭代器

好处

+ 当一个对象变成一个可迭代对象的时候，就可以进行某些迭代操作
+ 比如 for...of 操作时，其实就会调用它的 @@iterator 方法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711353217561-dd4e5bb7-318a-4773-a6bf-52fd89fbec93.png) 

## 原生迭代器对象
事实上我们平时创建的很多原生对象已经实现了可迭代协议，会生成一个迭代器对象的： String、Array、Map、Set、arguments对象、NodeList集合

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711356694757-34423274-09ea-4182-890c-638285f21536.png)

## 可迭代对象的应用
+ JavaScript中语法：for ...of、展开语法、yield*（后面讲）、解构赋值
+ 创建一些对象时：new Map([Iterable])、new WeakMap([iterable])、new Set([iterable])、new WeakSet([iterable])
+ 一些方法的调用：Promise.all(iterable)、Promise.race(iterable)、Array.from(iterable);

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711356457934-5f5c90b1-20e6-4d46-8584-dd30de55b580.png)

## 自定义类的迭代
在面向对象开发中，我们可以通过class定义一个自己的类，这个类可以创建很多的对象，如果我们也希望自己的类创建出来的对象默认是可迭代的，那么在设计类的时候我们就可以添加上 @@iterator 方法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711358174544-3a0a3ebb-3aa1-47b1-8007-02af384a7a39.png)

## 迭代器的中断
迭代器在某些情况下会在没有完全迭代的情况下中断：

+ 比如遍历的过程中通过break、return、throw中断了循环操作
+ 比如在解构的时候，没有解构所有的值

那么这个时候我们想要监听中断的话，可以添加return方法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711358367685-a9c472d3-25d5-4fd0-84f9-0e38934b08d1.png)

# 12. Generator 生成器
## 概念
生成器是ES6中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执行等

生成器函数也是一个函数，但是和普通的函数有一些区别：

+ 生成器函数需要在function的后面加一个符号：*****
+ 生成器函数可以通过**yield**关键字来控制函数的执行流程
+ 生成器函数的返回值是一个**Generator 生成器**
    - 生成器事实上是一种特殊的迭代器

## 执行
生成器的工作原理：

1. 初始化：调用生成器函数时，并不会立即执行函数体，而是返回一个生成器对象
2. 执行与暂停：调用迭代器对象的 next() 方法时，生成器函数开始执行，直到遇到第一个 yield 表达式，函数暂停执行，并返回 yield 的值
3. 继续执行：再次调用 next() 方法，生成器函数从上次暂停的地方继续执行，直到下一个 yield 或函数结束

```javascript
function* foo() {
  console.log("函数开始执行~")

  const value1 = 100
  console.log("第一段代码:", value1)
  yield value1

  const value2 = 200
  console.log("第二段代码:", value2)
  yield value2

  const value3 = 300
  console.log("第三段代码:", value3)
  yield value3

  console.log("函数执行结束~")
  return "123"
}

// generator本质上是一个特殊的iterator
const generator = foo()
console.log("返回值1:", generator.next())
console.log("返回值2:", generator.next())
console.log("返回值3:", generator.next())
console.log("返回值3:", generator.next())

// 函数开始执行~
// 第一段代码: 100
// 返回值1: { value: 100, done: false }
// 第二段代码: 200
// 返回值2: { value: 200, done: false }
// 第三段代码: 300
// 返回值3: { value: 300, done: false }
// 函数执行结束~
// 返回值3: { value: '123', done: true }
```

## 传递参数
如果想要在生成器函数中，yield的下一段片段代码中拿到上一段片段代码的返回值rv，要怎么样才能做到

直接使用变量进行接收即可，因为 **yield具备返回值**，yield与next方法之间存在信息传递

+ generator.next(value) 的参数 value 相当于输入，作为上一个 yield 表达式的返回值
+ 首次调用 next() 时，如果不传参数，或者传入的参数会被忽略，因为此时还没有遇到第一个 yield
+ 从第二次调用 next(value) 开始，传入的参数才会被传递给 yield 表达式，作为其返回值

```javascript
function* foo() {
  console.log("函数开始执行~")

  const value1 = 100
  console.log("第一段代码:", value1)
  const rv = yield value1//rv是输入，yield value1是输出
  
  console.log("第二段代码:", rv)
}


const generator = foo()
//yield 表达式的值取决于下一次调用 next(value) 时传入的参数
console.log("返回值1:", generator.next())//next是接收到输出的内容
console.log("返回值2:", generator.next(25))//next()传入的内容是输入

//函数开始执行~
//第一段代码: 100
//返回值1: { value: 100, done: false }
//第二段代码: 25
//返回值2: { value: undefined, done: true }
```

因此yield具备双向通信功能，yield 表达式返回一个值给外部，同时可以接收外部通过 next(value) 传入的值

+ 作为输出： yield 后面的值会被 next() 方法返回给外部
+ 作为输入： yield 表达式的值取决于下一次调用 next(value) 时传入的参数

## 提前结束
可以通过return函数给生成器函数传递参数，return传值后这个生成器函数就会结束，之后调用next不会继续生成值了

```javascript
function* gen() {
  yield 1;
  yield 2;//相当于在这里不使用yield，直接return 42
  yield 3;
}

const g = gen();

console.log(g.next());        // { value: 1, done: false }
console.log(g.return(42));    // { value: 42, done: true }
console.log(g.next());        // { value: undefined, done: true }
```

## 抛出异常
除了给生成器函数内部传递参数之外，也可以给生成器函数内部抛出异常

```javascript
console.log(generator.next('first'));
console.log(generator.throw(new Error("next2 throw err")));
console.log(generator.next('next3'));
```

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711422607655-e2fd042c-e56f-43b3-b20a-b325db3e0315.png)

抛出异常后我们可以在生成器函数中捕获异常，但是在catch语句中不能继续yield新的值了，但是可以在catch语句外使用yield继续中断函数的执行

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711422808846-d66d389e-388c-46f4-976e-c89c3c4c4ad8.png)

## 应用
### 替代迭代器
生成器是一种特殊的迭代器，在某些特殊情况可以用生成器替代迭代器

```javascript
const names = ['kobe', 'curry', 'jcak']

function* createArrayIterator(arr) {
    for (const item of arr) {
       yield item
    }
}

const namesIterator = createArrayIterator(names)

console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());
console.log(namesIterator.next());
```

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711423517137-760dc589-f6d8-48ff-b92a-5ef6135c5cc1.png)

#### yield* 语法糖
还可以使用yield*来生产一个可迭代对象，这个时候相当于是一种yield的语法糖，会依次迭代这个可迭代对象，每次迭代其中的一个值

简单来说就是：yield* 可迭代对象 === 依次迭代该对象

```javascript
        const names = ['kobe', 'curry', 'jcak']
        function* createArrayIterator(arr) {
            yield* arr
        }

        const namesIterator = createArrayIterator(names)
        console.log(namesIterator.next());
        console.log(namesIterator.next());
        console.log(namesIterator.next());
        console.log(namesIterator.next());
```

#### yield* 替代类实现
```javascript
        class Classroom {
            constructor(name, address,initialstudent) {
                this.name = name
                this.address = address
                this.students = initialStudent || []
            }
            push(student) {
                this.students.push(student)
            }
            *[Symbol.iterator]() {
                yield* this.students
            }
        }
```

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711424744485-2805d298-a820-4373-ae9b-5e5be9a7bbc5.png)

### 生成某个范围的值
```javascript
function* createRangeIterator(start, end) {
    for (let i = start; i < end; i++) {
        yield i
    }
}

const rangeIterator = createRangeIterator(10, 20)

console.log(rangeIterator.next());
console.log(rangeIterator.next());
console.log(rangeIterator.next());
console.log(rangeIterator.next());
```

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711423914824-6c63e1f6-b7cb-465c-a441-07b903bdaa53.png)

### 异步处理 - generator 方案
学完了前面的Promise、生成器等，来看一下异步代码的最终处理方案

案例需求：

1. 我们需要向服务器发送网络请求获取数据，一共需要发送三次请求
2. 第二次的请求url依赖于第一次的结果
3. 第三次的请求url依赖于第二次的结果
4. 依次类推

```javascript
function requestData(url) {
  // 异步请求的代码会被放入到executor中
  return new Promise((resolve, reject) => {
    // 模拟网络请求
    setTimeout(() => {
      // 拿到请求的结果
      resolve(url)
    }, 2000);
  })
}
//使用方式
requestData('juejin.cn').then(res => {
  console.log(res)
}).catch(err => {
  console.log(err);
})
```

#### 方式一：层层嵌套
如果以目前我们Promise的then、catch做法，很容易形成回调地狱

+ 嵌套太多层难以阅读维护称为xxx地狱，由回调产生的则称为回调地狱

```javascript
requestData("why").then(res => {
  requestData(res + "aaa").then(res => {
    requestData(res + "bbb").then(res => {
      console.log(res)
    })
  })
})
```

#### 方式二：链式调用
可以使用链式调用来解决，相对于回调地狱来说，不会存在嵌套的问题，也是正常情况下我们会使用的方式

then可以接收上一阶段所返回的内容进行调用(返回内容会被包裹一层Promise，本身为Promise则直接返回)

```javascript
requestData("why").then(res => {
  return requestData(res + "aaa")
}).then(res => {
  return requestData(res + "bbb")
}).then(res => {
  console.log(res)
})
```

#### 方式三：generator
```javascript
function* getData() {
  //左侧接收请求，右侧发送请求
  const res1 = yield requestData("why")//拿到res1的结果执行res2结果
  const res2 = yield requestData(res1 + "aaa")//拿到res2结果执行res3结果
  const res3 = yield requestData(res2 + "bbb")//...依次执行
  const res4 = yield requestData(res3 + "ccc")
  console.log(res4)
}
```

# 13. 异步函数 async function
## 基本写法
async关键字用于声明一个异步函数，有很多种写法

```javascript
async function foo(){}

const foo = async function(){}

const foo = async () => {}

class Person{
  async foo(){}
}
```

## 执行流程
异步函数的内部代码执行过程和普通的函数是一致的，默认情况下也是会被同步执行

当异步函数有返回值时，和普通函数会有区别：

+ 如果是普通值，异步函数的返回值相当于被包裹到Promise.resolve中
+ 如果异步函数的返回值是Promise，状态由会由Promise决定
+ 如果异步函数的返回值是一个对象并且实现了thenable，那么会由对象的then方法来决定

如果在async中抛出了异常，那么它并不会像普通函数一样报错，而是会作为Promise的reject来传递，如果使用了foo().catch，就会被捕获到

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711455406861-7bad9e45-ec48-44f1-82de-d4a6dc45d9fb.png)

## await 关键字
async函数另外一个特殊之处就是可以在它内部使用await关键字，而普通函数中是不可以的。

await关键字的特点

+ 通常使用await是后面会跟上一个表达式，这个表达式会返回一个Promise
+ 那么await会等到Promise的状态变成fulfilled状态，之后继续执行异步函数![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711456606822-95233d2b-c0d7-43c8-9b35-6de4ac4bf83b.png)
+ 如果await后面是一个普通的值，那么会直接返回这个值
+ 如果await后面是一个thenable的对象，那么会根据对象的then方法调用来决定后续的值
+ 如果await后面是一个表达式，返回的Promise是reject的状态，那么会将这个reject结果直接作为函数的Promise的reject值

```javascript
async function foo() {
  const res1 = await new Promise((resolve) => {
    resolve("coderwhy")
  })
  console.log("res1:", res1)
}

async function foo2() {
  const res1 = await requestData()
  console.log("res1:", res1)
}

foo2().catch(err => {
  console.log("err:", err)
})
```

## 方案四：async/await解决方案
```javascript
function requestData(url){
  return new Promise((resolve,reject) => {
    setTimeout(()=>{
      resolve(url)
    },2000)
  })
}

async function getData(){
  const res1 = await requestData(url1)
  //这里未返回结果不能执行下一个，因为需要res1的结果
  console.log("res1",res1);
  const res2 = await requestData(res1 + "coderwhy")
  console.log("res2",res2);
  const res3 = await requestData(res2 +"不知道去哪了")
  console.log("res3",res3);
}

getData()
```

# 14. 事件循环
## 浏览器中的JavaScript线程
JavaScript的代码执行是在一个单独的线程中执行的，这就意味着JavaScript的代码，在同一个时刻只能做一件事，如果这件事是非常耗时的，就意味着当前的线程就会被阻塞

为什么不多线程：因为多线程操作是很容易产生不安全的数据的，我们访问数据的时候通常都是需要上锁的(多线程的情况)，而上锁再解锁的操作是比较耗时的

由于浏览器的每个进程是多线程的，那么可以由其他线程来完成这个耗时的操作，让JS线程专注于代码的运行，比如网络请求、定时器等

耗时操作都由浏览器其余线程完成，例如定时器设置2s后执行，当运行到该行代码时，会传递给浏览器，由浏览器的其余线程来"计时"这2s，当2s结束后，浏览器会通知JS线程执行定时器内的代码，因此这2s的时间是不在JS线程中处理的

这些都是浏览器算好时间后，通知JS线程把代码执行一下，那如果有很多的定时器、网络请求等耗时操作，那JS线程怎么知道到时间后要执行的是哪一行代码，这就需要说明到一个概念：事件循环

在事件循环中，有一个事件队列，浏览器准备让JS线程执行代码时，会将要执行的代码放入这个事件队列中，然后JS线程可以从这个队列中挨个取出需要执行的部分，该队列遵循先进先出原则这是浏览器执行代码的完整逻辑

## 概念
这一套完整的流程，形成一个循环，也是被称为事件循环的原因，因此也可以认为这是一个执行模型

1. 执行代码脚本
2. 栈操作(同步代码)
3. 推送耗时操作给浏览器其余线程 
4. 推送到事件队列 
5. 任务处理(宏微任务) 
6. 4与5循环重复(保持应用持续响应外部事件)

事件循环的“闭环”机制基于持续监控调用栈和任务队列的状态。事件循环确保每当调用栈为空时，都会从任务队列中取出新的事件处理。通过这种方式，它循环处理所有待办的事件和任务，直到没有更多任务为止。整个过程是一个连续的循环，确保了JavaScript环境可以连续运行，处理所有异步事件和操作

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1729926039185-29ae148c-ff10-494a-8014-0280ad1cfac7.png)

## 宏任务和微任务
在之前我们主要使用过以下3种异步，分别为定时器、queueMicrotask和Promise，他们都接收一个回调且非立即执行，这些异步方法，最终都会将回调放入事件队列中(结束耗时操作后)，等待着JS线程的执行



事件循环中并非只维护着一个队列，事实上是有两个队列：

+ 宏任务队列（macrotask queue）：ajax、setTimeout、setInterval、DOM监听、UI Rendering等
+ 微任务队列（microtask queue）：Promise的then回调、 Mutation Observer API、queueMicrotask()等



为什么要区分？

+ 细分为宏任务和微任务队列可以优先处理更紧急的操作，提高应用的响应性并避免界面阻塞
+ 耗时更多的交给宏任务队列，关键的小任务（如更新数据绑定、处理 Promise）交给微任务
+ 这种快速响应是现代 Web 应用能够提供良好用户体验的关键因素之一
+ 因此通过优先执行微任务，JS 引擎确保在重渲染界面之前，状态更新等操作可以尽快执行，从而提高界面响应性



事件循环对于两个队列的优先级是这样的：

1. <font style="color:rgb(1, 1, 1);">main script中的代码优先执行（编写的顶层script代码）</font>
2. <font style="color:rgb(1, 1, 1);">在执行任何一个宏任务之前（不是队列，是一个宏任务），都会先查看微任务队列中是否有任务需要执行</font>
+ <font style="color:rgb(1, 1, 1);">也就是宏任务执行之前，必须保证微任务队列是空的</font>
+ <font style="color:rgb(1, 1, 1);">如果不为空，那么就优先执行微任务队列中的任务（回调）</font>

## 面试题
### 题 1-代码的执行顺序
```javascript
//1
setTimeout(function () {
  console.log("setTimeout1");
  new Promise(function (resolve) {
    resolve();
  }).then(function () {
    new Promise(function (resolve) {
      resolve();
    }).then(function () {	
      console.log("then4");
      });
    console.log("then2");
  });
});

//2
new Promise(function (resolve) {
  console.log("promise1");
  resolve();
}).then(function () {
  console.log("then1");
});

//3
setTimeout(function () {
  console.log("setTimeout2");
});

//4
console.log(2);

//5
queueMicrotask(() => {
  console.log("queueMicrotask1")
});

//6
new Promise(function (resolve) {
  resolve();
}).then(function () {
  console.log("then3");
});
```

解题思路：

> 分为 6 个区域，其中1、3是定时器，2、6是Promise，4为main script代码，5为queueMicrotask方法
>
> 执行优先级：同步代码执行、微任务执行、宏任务执行，在面对同优先度时，先进先出
>
> 1. 同步：main script，Promise阶段一的代码为main script代码
> 2. 微任务：Promise的then方法，queueMicrotask
> 3. 宏任务：定时器耗时
>
> 因此执行先后顺序为：4、2、5、6、1、3，下面进行细分
>
> 1. 同步代码：2中的promise1，4中的数字2
> 2. 微任务：2中then方法的then1，5中的queueMicrotask1，6中的then3
> 3. 宏任务：1中的setTimeout1、(then2、then4 宏任务中的微任务)，3中的setTimeout2
>

因此输出结果为：promise1，2，then1，queueMicrotask1，then3，setTimeout1，then2，then4，setTimeout2

### 题 2- async/await
```javascript
async function async1() {
  console.log('async1 start')
  await async2();
  console.log('async1 end')
}

async function async2() {
  console.log('async2')
}
//1
console.log('script start')

//2
setTimeout(function () {
  console.log('setTimeout')
}, 0)

//3
async1();

//4
new Promise(function (resolve) {
  console.log('promise1')
  resolve();
}).then(function () {
  console.log('promise2')
})

//5
console.log('script end')
```

解题思路

> 1. 同步代码：1、3的同步代码、4的Promise阶段一、5
> 2. 微任务代码：3的await，4的then方法
> 3. 宏任务代码：2
>
> async2 内没有await关键字存在，与普通函数一致，属于同步任务
>
> await 后续代码会直接归属到微任务队列中  
因此执行顺序为：
>
> 1. 同步代码：script start、async1 start、async2、promise1、script end
> 2. 微任务代码：async1 end、promise2
> 3. 宏任务代码：setTimeout
>

### 题 3- Promise 状态调度
```javascript
//第一个Promise链
Promise.resolve().then(() => {
  console.log(0);
  return Promise.resolve(4)
}).then((res) => {
  console.log(res)
})

//第二个Promise链
Promise.resolve().then(() => {
  console.log(1);
}).then(() => {
  console.log(2);
}).then(() => {
  console.log(3);
}).then(() => {
  console.log(5);
}).then(() => {
  console.log(6);
})
```

答案：1、2、3、4、5、6，解析见小余

[JS高级-async/await与事件循环队列](https://mp.weixin.qq.com/s/C_-u74utLeunKqAv7Dm5kg)

## Node 事件循环-了解
浏览器中的事件循环是根据HTML5定义的规范来实现的，不同的浏览器可能会有不同的实现，而Node中是由libuv实现的



Node.js 的事件循环主要分为以下几个阶段：

1. 定时器：这一阶段执行已经达到指定时间的 setTimeout 和 setInterval 回调函数
2. I/O回调：在这个阶段，除了关闭的回调外，几乎所有的回调函数都会被执行。这包括文件系统操作、网络请求等
3. 闲置的准备：在这一阶段，Node.js 会执行一些特定的操作，例如，清空延迟关闭的TCP连接，清空已经排队的待处理数据包等
4. 轮询：在这个阶段，Node.js 会检查是否有新的I/O事件。如果有，它们会被加入到事件队列中。如果轮询阶段执行了足够长的时间，事件循环将进入“检查”阶段
5. 检查：在这个阶段，setImmediate 的回调函数会被执行
6. 关闭的回调：在这个阶段，一些关闭的回调函数会被执行，例如，socket或者服务器的关闭



工作流程

1. 从定时器阶段开始，执行所有到期的定时器回调
2. 执行I/O回调
3. 执行闲置的准备阶段的操作
4. 进入轮询阶段，检查新的I/O事件，并执行I/O回调
5. 如果有 setImmediate 调用，执行这些回调
6. 执行关闭的回调
7. 回到步骤1，重复以上步骤



Node.js中的事件循环不仅仅涉及微任务和宏任务，还包括了前面提到的多个阶段

+ 宏任务（macrotask）：setTimeout、setInterval、IO事件、setImmediate、close事件
+ 微任务（microtask）：Promise的then回调、process.nextTick、queueMicrotask

# 15.  正则表达式
## 基本概念
正则表达式是一种字符串匹配利器，可以帮助我们搜索、获取、替代字符串。正则表达式主要由两部分组成：规则（patterns）和修饰符/标识（flags）

### 创建
正则表达式使用RegExp类来创建，也有对应的字面量(/…/)的方式。Const re1 = new RegExp(规则，标识)  或者  /规则/标识

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611195244-9d852f97-2728-42bb-a939-f39040ef5b76.png)

## 规则的使用
### 字符类
+ \d：数字字符，0到9
+ \s：空格符号，包括空格，制表符 \t，换行符 \n 和其他少数稀有字符，例如 \v，\f 和 \r。
+ \w：“单字”字符，拉丁字母或数字或下划线 _。
+ . ：点 ，是一种特殊字符类，它与 “除换行符之外的任何字符” 匹配

### 反向类
+ \D 非数字：除 \d 以外的任何字符，例如字母。
+ \S 非空格符号：除 \s 以外的任何字符，例如字母。
+ \W 非单字字符：除 \w 以外的任何字符，例如非拉丁字母或空格。

锚点类

+ 符号 ^ 匹配文本开头；
+ 符号 $ 匹配文本末尾；

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611195638-e7ed81ec-6b45-4490-9900-e93bd2686f25.png)

### 转义字符串
如果要把特殊字符作为常规字符来使用，需要对其进行转义，只需要在它前面加个反斜杠\。常见的需要转义的字符：[] \ ^ $ . | ? * + ()

斜杠符号 ‘/’ 并不是一个特殊符号，但是在字面量正则表达式中也需要转义；

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611196128-854fa757-0711-4652-8c51-dbbc8be7364a.png)

### 集合类
[eao] 意味着查找在 3 个字符 ‘a’、‘e’ 或者 `‘o’ 中的任意一个

### 范围类
方括号也可以包含字符范围；比如说，[a-z] 会匹配从 a 到 z 范围内的字母，[0-5] 表示从 0 到 5 的数字；[0-9A-F] 表示两个范围：它搜索一个字符，满足数字 0 到 9 或字母 A 到 F；

+ \d —— 和 [0-9] 相同；
+ \w —— 和 [a-zA-Z0-9_] 相同；

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611196401-d3bf8270-797b-4f5c-9a09-a8f67ba77561.png)

### 量词类
给数量一个范围

### 数量 {n}
+ 确切的位数：{5}
+ 某个范围的位数：{3,5}

### 缩写：
+ +：代表“一个或多个”，相当于 {1,}
+ ?：代表“零个或一个”，相当于 {0,1}。换句话说，它使得符号变得可选；
+ *：代表着“零个或多个”，相当于 {0,}。也就是说，这个字符可以多次出现或不出现；

## 贪婪模式和惰性模式
如果要匹配字符串中所有使用《》包裹的内容，默认情况下的匹配规则是查找到匹配的内容后，会继续向后查找，一直找到最后一个匹配的内容，这种匹配的方式，我们称之为贪婪模式

懒惰模式中的量词与贪婪模式中的是相反的。只要获取到对应的内容后，就不再继续向后匹配；我们可以在量词后面再加一个问号 ‘?’ 来启用它；所以匹配模式变为 *? 或 +?，甚至将 '?' 变为 ??

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611196840-06e2b709-3ec0-42c5-a474-6729ae3fc33e.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611197116-0e48d442-9b79-4de7-8313-a6368f438fe9.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611197475-59455cc8-edc3-49ad-b7d2-76372640935b.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611197790-87c7be45-dc1a-492a-a398-c4e619c51a54.png)

修饰符的使用

g：全部参与匹配

i：忽略大小写

m：多行匹配



正则的使用

两种方法：

+ 直接使用正则对象上的实例方法
    - ![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611198299-5dffb0a7-37cc-478e-aefc-1cdb04eba0f2.png)
+ 使用字符串的方法，传入一个正则表达式
    - 比如要替换文本中所有的abc为cba，不管大小写，只要是Abc组合统统替换
    - 字符串本身的方法是message.replaceAll(‘abc’,’cba’)，但是只能替换全部为小写的cba
    - 使用正则就可以实现预期功能，message.replaceAll(/abc/ig,’cba’)

# 16.  防抖和节流函数
## 基本概念
### 防抖 debounce 函数
防抖：在事件停止触发后的延迟时间内只执行一次，可以理解为游戏回城，回城无冷却，但是否达到回城生效条件，以最后一次开始回城时间为准

过程：

+ 当事件触发时，相应的函数并不会立即触发，而是会等待一定的时间
+ 当事件密集触发时，函数的触发会被频繁的推迟。只有等待了一段时间也没有事件触发，才会真正的执行响应函数

#### 应用场景
在某个搜索框中输入自己想要搜索的内容，比如想要搜索一个MacBook

+ 当我输入m时，为了更好的用户体验，通常会出现对应的联想内容，这些联想内容通常是保存在服务器的，所以需要一次网络请求
+ 当继续输入ma时，再次发送网络请求
+ 那么macbook一共需要发送7次网络请求
+ 这大大损耗我们整个系统的性能，无论是前端的事件处理，还是对于服务器的压力

但是我们不需要这么多次网络请求，正确的做法应该是在合适的情况下再发送网络请求；

+ 比如用户快速的输入一个macbook，那么只是发送一次网络请求；
+ 比如用户是输入一个m想了一会儿，这个时候m确实应该发送一次网络请求
+ 也就是我们应该监听用户在某个时间，比如500ms内，没有再次触发时间时，再发送网络请求

这就是防抖的操作：只有在某个时间内，没有再次触发某个函数时，才真正的调用这个函数

#### 实现防抖
假设我们需要防抖的内容为fn函数，如何使防抖作用于fn函数：通常会对fn函数包裹一层防抖函数，可以看做是拦截器

```javascript
//需要防抖功能的内容
const fn = ()=> {
    console.log('Hello, World!')
}
// 参数1：需要防抖功能的内容，参数2：防抖时间间隔
//假设debounce方法来自名为lodash的第三方库
const newFn = lodash.debounce(fn, 500)
//如果在这 500 毫秒内事件没有再次触发，那么函数就会执行
//如果在这段时间内事件再次被触发，延迟时间会重新计时
console.log(newFn)//...
```

### 节流 throttle 函数
不管事件触发的频率有多高，函数也会按照预定的时间间隔固定执行

节流和防抖的对比：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1731059627007-1d2e8259-c04b-43c8-a71a-005fd0963237.png)

## 第三方库
以通过一些第三方库来实现防抖操作，其中较为出名的有lodash或underscore

+ Lodash:提供内置的 debounce() 和 throttle() 函数，用于实现防抖和节流
+ Underscore：也提供了 debounce() 和 throttle() 函数，用于实现防抖和节流，功能与 Lodash 类似，但在性能和灵活性上相对不如 Lodash，特别是在优化细节和边界情况的处理上

### 使用 Underscore
安装有很多种方式：

+ 下载Underscore，本地引入
+ 通过CDN直接引入
+ 通过包管理工具（npm）管理安装

![模拟代码](https://cdn.nlark.com/yuque/0/2024/png/29327577/1731060420743-8865696c-a335-4232-8849-52c573145ee1.png)

现在就对上述的代码进行防抖节流处理

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1731060456539-815c519b-207a-4508-b842-f86979c95ef6.png)

# 17.  网络请求
## AJAX
### 引入
早期的网页都是通过后端渲染来完成的：服务器端渲染，即客户端发出请求 -> 服务端接收请求并返回相应HTML文档 -> 页面刷新，客户端加载新的HTML文档；

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611199356-9f332b3b-84d6-438a-9315-69fa727a8f3d.png)

它有一定的缺点：当用户点击页面中的某个按钮向服务器发送请求时，页面本质上只是一些数据发生了变化，而此时服务器却要将重绘的整个页面再返回给浏览器加载，这显然有悖于程序员的“DRY（ Don‘t repeat yourself ）”原则；而且明明只是一些数据的变化却迫使服务器要返回整个HTML文档，这本身也会给网络带宽带来不必要的开销。

所以就衍生出了一种技术叫AJAX，即Asynchronous JavaScript And XML(异步的JavaScript和XML)，是一种实现无页面刷新获取服务器数据的技术。也就是说它可以在不重新刷新页面的情况下与服务器通信，交换数据，或更新页面。

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611199899-6189ad19-717b-465a-a349-52edac83de03.png)



### 网络请求基本知识
超文本传输协议(HyperText Transfer Protocol)，是一种用于分布式、协作式和超媒体信息系统的应用层协议。HTTP是万维网的数据通信的基础，设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法。

HTTP是一个客户端（用户）和服务端（网站）之间请求和响应的标准。通过使用网页浏览器、网络爬虫或者其它的工具，客户端发起一个HTTP请求到服务器上指定端口。我们称这个客户端为用户代理程序。响应的服务器上存储着一些资源，比如HTML文件和图像。我们称这个响应服务器为源服务器。

组成

一次HTTP请求主要包括请求request和响应respond



请求信息：第一行为请求行，中间是请求头，最下面是请求体

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611200497-c1a421ef-6876-4837-9466-7a651fdd622e.png)

响应信息：第一行为响应行，中间是响应头，最下面是响应体

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611200823-8fd50cde-2d57-4fb3-8797-6a368ea88cc3.png)

版本

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611201170-2fd2eee8-3c4a-4626-8c00-baa6cc53d146.png)

### 请求方式
常用的有

+ GET：GET 方法请求一个指定资源的表示形式，使用 GET 的请求应该只被用于获取数据。
+ HEAD：HEAD 方法请求一个与 GET 请求的响应相同的响应，但没有响应体。比如在准备下载一个文件前，先获取文件的大小，再决定是否进行下载；
+ POST：POST 方法用于将实体提交到指定的资源。
+ PUT：PUT 方法用请求有效载荷（payload）替换目标资源的所有当前表示；
+ DELETE：DELETE 方法删除指定的资源；
+ PATCH：PATCH 方法用于对资源应部分修改；

### 请求头的内容
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611201563-a6379dc6-5647-488f-b082-0f8d6ea01350.png)

+ content-type是这次请求携带的数据的类型
    - application/x-www-form-urlencoded：表示数据被编码成以 '&' 分隔的键 - 值对，同时以 '=' 分隔键和值
    - application/json：表示是一个json类型；
    - text/plain：表示是文本类型；
    - application/xml：表示是xml类型；
    - multipart/form-data：表示是上传文件；
+ content-length：文件的大小长度
+ keep-alive：http是基于TCP协议的，但是通常在进行一次请求和响应结束后会立刻中断；在http1.0中，如果想要继续保持连接：
    - 浏览器需要在请求头中添加 connection：keep-alive；
    - 服务器需要在响应头中添加 connection：keep-alive；
    - 当客户端再次放请求时，就会使用同一个连接，直接一方中断连接；
    - 在http1.1中，所有连接默认是 connection: keep-alive的；不同的Web服务器会有不同的保持 keep-alive的时间；
+ accept-encoding：告知服务器，客户端支持的文件压缩格式
+ accept：告知服务器，客户端可接受文件的格式类型
+ user-agent：客户端相关的信息

### Response响应状态码
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611201953-e4a0566f-6f42-405e-a07c-0092b2204e1e.png)

### 响应头的内容
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611202343-28951abb-a442-46f9-913d-335cd19b1f35.png)

### AJAX发送请求
可以使用 JSON，XML，HTML 和 text 文本等格式发送和接收数据

步骤

+ 创建网络请求的AJAX对象（使用XMLHttpRequest）
+ 监听XMLHttpRequest对象状态的变化，或者监听onload事件（请求完成时触发）
+ 配置网络请求（通过open方法）
+ 发送send网络请求

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611202607-6de06f0b-c282-4e20-8698-22917e5dbda1.png)

### XMLHttpRequest的state（状态）
我们在一次网络请求中看到状态发生了很多次变化，这是因为对于一次请求来说包括如下的状态，这个状态并非是HTTP的相应状态，而是记录的XMLHttpRequest对象的状态变化

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1685611202867-4b05602b-84d9-41c8-a3f7-37816869c951.png)

## AJAX 与 AXIOS
定义和用途：

+ Ajax：是一种在无需重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术。它允许在浏览器中进行异步数据更新
+ Axios：是一个基于Promise的HTTP客户端，用于浏览器和node.js。它提供了一个简单的API来执行HTTP请求



实现方式：

+ Ajax：通常使用XMLHttpRequest对象来实现异步请求。这是一个较老的技术，直接操作较为复杂
+ Axios：提供了一个更现代、更简洁的API来发送HTTP请求。它基于Promise，使得异步请求的处理更加直观和易于管理



兼容性和易用性：

+ Ajax：需要手动处理请求和响应，包括状态码和错误处理，这可能会使代码变得复杂
+ Axios：提供了更丰富的功能，如请求和响应拦截器、转换数据格式、取消请求等，使得HTTP请求的处理更加灵活和强大



错误处理：

+ Ajax：错误处理需要手动编写，通常涉及到检查状态码和处理不同类型的异常
+ Axios：错误处理更加简洁，因为它基于Promise，可以使用.then()和.catch()方法来处理成功和失败的情况



社区和维护：

+ Ajax：作为一种较老的技术，其社区和维护可能不如现代库那么活跃
+ Axios：有一个活跃的社区和定期的更新，这意味着它有更多的资源和更好的支持



跨平台支持：

+ Ajax：主要在浏览器中使用，虽然可以通过某些库在Node.js中使用，但这不是它的主要用例
+ Axios：设计时就考虑了跨平台使用，既可以在浏览器中使用，也可以在Node.js环境中使用



Axios可以看作是Ajax的一个现代化替代品，它提供了一个更简洁、更强大的API来处理HTTP请求，随着现代Web开发实践的发展，Axios因其易用性和强大的功能而变得越来越流行

# 18. 包管理工具
## Npm
npm（全称 Node Package Manager）是 Node.js 的包管理工具，它是一个基于命令行的工具，用于帮助开发者在自己的项目中安装、升级、移除和管理依赖项

### 功能
+ 包安装：可以通过命令行工具安装、更新、卸载包。
+ 依赖管理：在项目的package.json文件中自动记录项目所依赖的包及其版本，确保开发环境和生产环境的一致性。
+ 脚本运行：可以在package.json中定义脚本命令，如启动应用、运行测试等。
+ 版本控制：npm支持语义化版本控制，可以定义包的版本号以指示更新的性质（如主要更新、次要更新或补丁）。
+ 包发布：开发者可以将自己编写的模块发布到npm，供其他人使用。

### 常用命令
npm init：初始化一个新的 npm 项目，创建 package.json 文件

npm config get registry ：显示当前设置的npm源的URL

npm config set registry newUrl：切换npm源



npm install：安装包，install可以简写为i，当在项目目录中运行这个命令时，npm 会查看 package.json 文件，读取依赖项列表，并安装它们

+ npm install <package-name> --save：安装指定的包，并将其添加到 package.json 文件中的依赖列表中。
+ npm install <package-name> --save-dev：安装指定的包，并将其添加到 package.json 文件中的开发依赖列表中。
+ npm install -g <package-name>：全局安装指定的包
+ npm update <package-name>：更新指定的包
+ npm uninstall <package-name> ：卸载指定包

### package.json 的配置项
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

### npm install 原理
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

### npm run serve & npm run dev
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

### npx
npx 是一个 npm 包执行器，它随 npm 5.2.0 版本一起引入。npx 的主要用途是方便开发者执行安装在项目本地 node_modules/.bin 目录或全局安装的 npm 包的命令

#### 特点
+ 临时安装并运行包：
    - 使用 npx 可以执行未全局安装的包，npx 会临时安装这些包并运行它们，执行完毕后不会保留这些包
+ 运行项目本地安装的包：
    - 项目中经常会安装一些命令行工具作为开发依赖，例如 eslint、webpack 等。通常情况下，如果想要运行这些工具，需要在项目的脚本中定义它们或者通过项目的 node_modules/.bin 路径来运行。npx 简化了这个过程，允许直接在命令行中输入命令来执行
+ 避免全局安装包：
    - 有时全局安装 npm 包可能会导致版本冲突或其他问题。npx 允许我们执行包而无需全局安装，减少了全局空间的污染
+ 执行 GitHub 上的包：
    - npx 还可以直接执行存储在 GitHub 等代码托管服务上的包，这对于试运行开源项目中的示例脚本或工具非常方便

#### npx 和 npm 联系
1. npx 是 npm 包的一部分，是 npm 的一个功能，随 npm 一起安装
2. npx侧重于执行命令的，执行某个模块命令。虽然会自动安装模块，但是重在执行某个命令
    - 比如使用 npx create-react-app my-app 来创建一个新的 React 应用，而不需要事先安装，虽然执行时会安装，但是执行完就删除了
3. npm侧重于安装或者卸载某个模块的。重在安装，并不具备执行某个模块的功能。

### 发布 Npm 包
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

### npm 搭建私服
npm私服（私有仓库）指的是建立在企业或组织内部的、独立于公共 npm 注册中心的私有 Node 包管理器。私服允许团队在不公开源码的情况下共享和复用代码，也可以用来代理公共 npm 注册中心，缓存下载过的包，提高安装速度，减少对外部网络的依赖

### **常见仓库**
+ Nexus Repository OSS：一个广泛使用的仓库，支持多种包类型，包括 npm
+ JFrog Artifactory：一种企业级的解决方案，支持多种包管理和自动化工具
+ Verdaccio：一个轻量级的私有 npm 代理注册中心，容易部署和维护
+ GitHub Packages：GitHub 提供的包管理服务，支持 npm 私有包

#### 示例
下面用 verdaccio 做示例

1. 安装`npm install verdaccio -g`
2. 启动：`verdaccio`
3. 配置：
    1. 自定义端口号运行 `verdaccio --listen 9333`，默认是 4873
    2. 指定配置文件运行 `verdaccio --config /xxx/xxx/xxx`
4. 创建用户： `npm adduser --registry http://localhost:4873`
5. 登录：`npm login --registry http://localhost:4873`
6. 发布：`npm publish --registry http://localhost:4873`
7. 使用托管：`registry=http://localhost:4873`，之后就可以使用 Npm install 安装了
8. 保持 Verdaccio 服务的运行
9. 取消后缀
10. 每次都需要加上--registry http://localhost:4873/这样的一个后缀就会很麻烦
11. 可以使用包管理器 xmzs 提供的 mmp add 命令
12. ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730075903560-9c70b441-e99c-418a-a087-423b180ba5f8.png)
13. 然后使用 mmp use 这个镜像
14. ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1730075934419-559ccb41-2b1d-4904-b0d6-79a8a148541b.png)
15. 后面使用直接用指令即可

## Yarn
另一个node包管理工具是yarn，主要是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具(这种做法在国外常见，国内较少)

yarn 是为了弥补 npm早期 的一些缺陷(安装依赖速度很慢、版本依赖混乱等)而出现的。到目前为止，yarn很多的精华都被npm所吸取，基本上没有差距，但是依然很多人喜欢使用yarn

## Cnpm
由于一些特殊的原因，某些情况下我们没办法很好的从npm的registry仓库下载下来一些需要的包，npm仓库位于国外，而我们位于国内，由于这类原因导致访问npm仓库需要一点运气，不是很可控

镜像是实际存储在服务器上的数据副本，包含了和官方服务器一致的内容，我们可以简单理解为：在国内开一个服务器，定时从国外npm仓库拷贝一份数据，这个国内的服务器就是镜像

镜像源则是用户访问这些镜像的入口，是一个 URL 地址，用户通过这个地址来获取需要的资源(也就是访问国内的服务器)。因此，镜像源是指向镜像的路径，是一种对外暴露的访问接口常见的镜像源有淘宝、华为云、腾讯云、中国科学技术大学等一系列大学镜像，选择其一进行使用即可

修改设置镜像源：`npm config set registry URL(镜像源)`

```javascript
# 淘宝 npm 镜像 (npmmirror)
https://registry.npmmirror.com/
# 阿里巴巴公司维护的 npm 镜像，又称为淘宝镜像。特别适合中国大陆地区的用户，加速 npm 包的下载。

# 淘宝 npm 镜像 (旧地址)
https://registry.npm.taobao.org/
# 这个地址是淘宝旧的 npm 镜像，仍然有效，重定向到新镜像 npmmirror。

# Yarn 官方镜像
https://registry.yarnpkg.com/
# Yarn 自己的 npm 镜像源，用户可以用这个地址来替代默认的 npm 官方源。

# 腾讯云 npm 镜像
https://mirrors.cloud.tencent.com/npm/
# 腾讯云维护的 npm 镜像，适合腾讯云用户以及想加速 npm 下载的用户。

# 中国科学技术大学 (USTC) npm 镜像
https://mirrors.ustc.edu.cn/npm/
# 中国科学技术大学提供的 npm 镜像，提供给国内用户，尤其是教育和科研用途。

# 华为云 npm 镜像
https://repo.huaweicloud.com/repository/npm/
# 华为云维护的 npm 镜像，用于加速 npm 包下载，适合中国地区的用户。

# npmjs 中国镜像 (cnpm)
https://r.cnpmjs.org/
# 一个由 cnpm 提供的镜像，类似于淘宝镜像的替代方案，也能用于加速下载。

# npm 镜像设置示例
npm config set registry https://registry.npmmirror.com/
# 将 npm 镜像设置为淘宝镜像，加快 npm 包的安装速度，特别适合中国大陆用户。
```

但这里其实还有几个问题：

+ 镜像源并不是时刻和npm仓库同步，这里存在一个间隔时间，比如淘宝是每隔10分钟同步一下
+ 镜像未来有一天会有可能不再维护，而这件事情我们并不知道(不会特地通知我们)，突然间不能继续使用，会对我们排查问题造成一定困扰，因此不能过多依赖于镜像，会存在一定风险
+ npm官网是一个非盈利组织，相对于公司更多依靠于商业来说，稳定性会高很多(哪怕创始人不再维护，也会有一代代人接手)

这时候，通常会使用cnpm，像这类工具就能够进行一个全局安装，以便日常使用，直接使用npm安装cnpm即可，cnpm的所有用法与npm保持一致，cnpm默认指向淘宝镜像，设置镜像源方式与npm一致

```javascript
npm install cnpm -g//安装
cnpm config set registry URL//设置镜像源
```

## Pnpm
目前最流行的包管理工具是pnpm，最快的是bun，这里主要讲pnpm

一个庞大的包很可能会依赖其他包，并且很可能不止一个包而是很多大大小小的包，而多个包Pinia、Vue、Vite、axios等等...会构建出一个庞大的下载链出来，这也是导致node_modules文件夹非常大的原因，在pnpm出来之前，大家的做法是删掉node_modules文件夹，等需要的时候再重新下载



pnpm解决遗留存储空间占据问题的关键在于硬链接和软链接的概念

+ 硬链接是一种指向文件物理数据块的指针。对于硬链接来说，文件在磁盘中的物理数据会被多个文件名引用
+ 软链接是一类特殊的文件，其包含有一条以绝对路径或者相对路径的形式指向其它文件或者目录的引用，类似快捷方式(但实际并不是快捷方式)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1731053095015-354bd5eb-1555-4152-a0e0-8bff0a48f884.png)



当使用 npm 或 Yarn 时，如果有 100 个项目，并且所有项目都有一个相同的依赖包，那么， 在硬盘上就需要保存 100 份该相同依赖包的副本

如果使用 pnpm，依赖包将被存放在一个统一的位置，因此：

+ 如果对同一依赖包使用相同的版本，那么磁盘上只有这个依赖包的一份文件
+ 如果对同一依赖包需要使用不同的版本，则仅有版本之间不同的文件会被存储起来
+ 所有文件都保存在硬盘上的统一的位置

当安装软件包时， 其包含的所有文件都会硬链接到此位置，而不会占用额外的硬盘空间。需要记住存储的是所有的文件，而不是包的目录，因为硬链接无法操作目录，只能操作文件，这让我们可以在项目之间方便地共享相同版本的依赖包，因为搭建硬链接并不会导致磁盘数据的拷贝

