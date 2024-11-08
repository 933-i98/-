**<u><font style="color:#DF2A3F;">why 的 ts 讲的晦涩难懂，而且有的东西更新了，所以请去看阮一峰的笔记学习</font></u>**

# 初识
## 引入
编程开发中有一个共识：错误出现的越早越好

+ 能在写代码的时候发现错误，就不要在代码编译时再发现（IDE的优势就是在代码编写过程中帮助我们发现错误）
+ 能在代码编译期间发现错误，就不要在代码运行期间再发现（类型检测就可以很好的帮助我们做到这一点）
+ 能在开发阶段发现错误，就不要在测试期间发现错误，能在测试期间发现错误，就不要在上线后发现错误

JavaScript因为从设计之初就没有考虑类型的约束问题，所以造成了前端开发人员关于类型思维的缺失，所以我们经常会说JavaScript不适合开发大型项目，因为当项目一旦庞大起来，这种宽松的类型约束会带来非常多的安全隐患，多人员开发它们之间也没有良好的类型契约。

我们可以将TypeScript理解成加强版的JavaScript

+ JavaScript所拥有的特性，TypeScript全部都是支持的，并且它紧随ECMAScript的标准，所以ES6、ES7、ES8等新语法标准，它都是支持的
+ TypeScript在实现新特性的同时，总是保持和ES标准的同步甚至是领先
+ 并且在语言层面上，不仅仅增加了类型约束，而且包括一些语法的扩展，比如枚举类型（Enum）、元组类型（Tuple）等
+ 并且TypeScript最终会被编译成JavaScript代码，所以你并不需要担心它的兼容性问题，在编译时也可以不借助于Babel这样的工具

## 运行
在使用 TypeScript之前，需要搭建对应的环境：npm install typescript -g(全局)

写好 ts 代码后，需要再终端 `tsc 文件名`进行编译，然后在 Html 文件中引入编译后 js 代码即可

每次为了查看TypeScript代码的运行效果，都通过经过两个步骤的话就太繁琐了：

1. 通过tsc编译TypeScript到JavaScript代码；App.vuewebpack
2. 在浏览器或者Node环境下运行JavaScript代码

可以通过两个解决方案来完成：

+ 通过webpack(ts-loader)，配置本地的TypeScript编译环境和开启一个本地服务，可以直接运行在浏览器上
+ 通过ts-node库，为TypeScript的运行提供执行环境

这里采用ts-node库：

+ 安装：npm install ts-node -g
+ 安装依赖包：npm install tslib @types/node(项目内安装即可)
+ 运行：ts-node xxx.ts

# 变量声明
声明了类型后TypeScript就会进行类型检测，声明的类型可以称之为**类型注解**

`var/let/const 变量名(标识符) ：数据类型 = 赋值`

声明变量的关键字 var 尽量不要使用

当使用 let 时，如果没有声明数据类型，会根据标识符自动推导数据类型(常用类型)

## JS 类型
### 基本数据类型
+ number：不区分 int 和 double
+ string：小写
+ boolean

### Array 类型
Array<T> 或 T[ ]：表示一个类型为 T 的数组，如 string[]  或  Array<string>

开发当中数组一般存储相同的数据类型，不要存储不同的类型

### Object 类型
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723536920713-0e60981f-24cb-4138-8e7e-1405585d7484.png)

### Symbol 类型
 在ES5中，是不可以在对象中添加相同的属性名称的，比如下面的做法  

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723536987400-4c8c7024-5dd2-43b6-962b-c4954b9a056c.png)

 但是可以通过symbol来定义相同的名称，因为Symbol函数返回的是不同的值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723537012386-06266415-868b-4f0a-93b0-098853cac931.png)

## 函数参数类型
### 参数
声明函数时，可以在每个参数后添加类型注解，以声明函数接受的参数类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723537138114-e0be8e76-3c9d-4c12-9acb-d14f3cb35eeb.png)

### 返回值
我们也可以添加返回值的类型注解，这个注解出现在函数列表的后面

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723537187984-3e363297-889f-4dee-8d24-f7fc24715d5a.png)

和变量的类型注解一样，通常情况下不需要返回类型注解，因为TypeScript会根据return 返回值推断函数的返回类型，某些第三方库处于方便理解，会明确指定返回类型，看个人喜好

### 匿名函数的参数
匿名函数的参数不需要标明数据类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723537227418-b63d6de5-2335-48ef-89e5-02d2f259fe43.png)

这里并没有指定item的类型，但是item是一个string类型

这是因为TypeScript会根据forEach函数的类型以及数组的类型推断出item的类型

这个过程称之为**上下文类型**，因为函数执行的上下文可以帮助确定参数和返回值的类型  

### 对象类型
 如果想限定一个函数接受的参数是一个对象，可以使用对象类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723537427242-a4f4c3d8-6da6-40ca-b740-b031a7f2d15f.png)

+ 在对象我们可以添加属性，并且告知TypeScript该属性需要是什么类型
+ 属性之间可以使用, 或者; 来分割，最后一个分隔符是可选的
+ 每个属性的类型部分也是可选的，如果不指定，那么就是any类型

 对象类型也可以指定哪些属性是可选的，可以在属性的后面添加一个?

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723537661911-3ded777e-4483-4f10-aa30-8b57f0a33d2d.png)

## TS 类型
### any
在某些情况下，无法确定一个变量的类型，并且可能它会发生一些变化，这个时候可以使用any类型

any类型有点像一种讨巧的TypeScript手段：

+ 可以对any类型的变量进行任何的操作，包括获取不存在的属性、方法
+ 给一个any类型的变量赋值任何的值，比如数字、字符串的值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723537879695-91f359d5-e84c-47d1-bca3-4ed456445b97.png)

使用场景： 对于某些情况的处理过于繁琐不希望添加规定的类型注解，或者在引入一些第三方库时，缺失了类型注解，这个时候可以使用any

### unknown
用于描述类型不确定的变量，和any类型有点类似，但是unknown类型的值上做任何事情都是不合法的

### void
通常用来指定一个函数是没有返回值的，函数没有写任何返回值类型，那么默认的类型就是void的，也可以指定返回值是void

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723538048167-6f9ab337-d0b6-473b-a419-795577116c3b.png)

### never
如果一个函数中是一个死循环或者抛出一个异常，那么写void类型或者其他类型作为返回值类型都不合适，于是就可以使用never类型

**很少使用**

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723538294294-f64c0502-f08c-4d79-9061-f521d6ba43d9.png)

### tuple
tuple是元组类型，很多语言中也有这种数据类型，比如Python

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723538417138-0d076252-500e-41a2-8acf-a0f7b4008dcd.png)

tuple和数组的区别

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723538402090-8049b80e-65af-4573-a9db-e734ec7290bf.png)

+ 数组中通常建议存放相同类型的元素，不同类型的元素是不推荐放在数组中（可以放在对象或者元组中）
+ 元组中每个元素都有自己特定的类型，根据索引值获取到的值可以确定对应的类型

使用场景： 当一个函数接受参数，返回一个数组并且数组中的两个值是不同类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723539077465-f734e59f-d443-4e2a-bfc1-8601fb60fcef.png)

### 联合类型
联合类型是由两个或者多个其他类型组成的类型，表示可以是这些类型中的任何一个值，联合类型中的每一个类型被称之为联合成员

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723541172183-4e869b4d-c0d7-46e8-8936-3e45d4405b34.png)

 需要使用缩小联合来使用联合类型的标识符，TypeScript可以根据我们缩小的代码结构，推断出更加具体的类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723541160312-e31738ab-0f79-438c-8621-0bb6489d29da.png)

### 类型别名
#### type
定义：用于创建类型别名，可以是基本类型、联合类型、交叉类型、元组等

用途：适用于需要组合多种类型或定义复杂类型的情况![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723605326195-2cfad2eb-0c82-470d-b51a-a8f5beee07ab.png)

#### interface
定义：用于定义对象的结构和接口，通常用于描述类的形状和对象的类型

用途：适用于描述对象的结构、继承和扩展类型，特别是在面向对象编程中![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723605331895-25e480ba-3263-46ac-884a-3c04089e0143.png)

#### interface和type区别
+ type 需要=，interface 不需要
+ 定义非对象类型，使用type
+ 定义对象类型
    - interface 可以重复的对某个接口来定义属性和方法
    - 而type定义的是别名，别名是不能重复的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723541559631-f388f3d2-0f46-4d41-a0d6-06e89d35298d.png)

### 交叉类型
表示需要满足多个类型的条件，使用&符号

```typescript
interface A {
  a: string;
}

interface B {
  b: number;
}

type C = A & B;

const example: C = {
  a: 'Hello',
  b: 42
};
```

### 类型断言 as
有时候TypeScript无法获取具体的类型信息，这个需要使用类型断言

比如通过document.getElementById，TypeScript只知道该函数会返回 HTMLElement ，但并不知道它具体的类型![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723604123553-75f1e7a8-9fc5-40c0-8d0f-717393d57227.png)

TypeScript只允许类型断言转换为更具体或者不太具体的类型版本，此规则可防止不可能的强制转换

```typescript
const someValue: unknown = 'Hello, world!';
const strLength: number = (someValue as string).length;
```

### 非空断言 !
用于告诉 TypeScript 编译器某个值不可能为 null 或 undefined。这可以通过在变量名后面加上感叹号 ! 来实现

```typescript
const value: string | undefined = getValue();
const length: number = value!.length; 
// 这里断言 value 绝对不为 null 或 undefined
```

在这个例子中，value 的类型是 string | undefined。为了访问 length 属性，使用了非空类型断言 value!，告诉 TypeScript 编译器 value 不会是 undefined 或 null，这样编译器就允许你访问 length 属性

注意事项：

+ 使用非空类型断言时，你要非常确定变量在使用时不为 null 或 undefined，否则可能会在运行时导致错误
+ 尽量避免过度使用非空类型断言，优先考虑通过其他方式确保类型安全，如使用条件检查或默认值

### 字面量类型
允许你指定一个变量的值只能是某个具体的值。这在 TypeScript 中用于限制变量的取值范围，使其更精确

```typescript
let direction: 'left' | 'right';

direction = 'left';  // 正确
direction = 'right'; // 正确
direction = 'up';    // 错误: 'up' 不在类型 'left' | 'right' 中
```

### 字面量推理
字面量推理根据实际赋给变量的值来自动推断变量的字面量类型。这意味着 TypeScript 会使用你提供的具体值来推断最精确的类型，而不是使用广泛的类型

```typescript
let color = 'blue'; // TypeScript 推断 color 的类型为 'blue'
color = 'red';      // 错误: 类型 '"red"' 不能赋给类型 '"blue"'
```

```typescript
let color = 'blue'; // TypeScript 推断 color 的类型为 'blue'
color = 'red';      // 错误: 类型 '"red"' 不能赋给类型 '"blue"'
```

## 类型缩小
通过根据不同的条件和控制流来缩小变量的类型，从而提高类型安全性并减少运行时错误

类型缩小使得 TypeScript 编译器能够根据代码逻辑推断出更具体的类型，从而允许你更精确地处理这些类型

### typeof
typeof 可以用来检查基本类型，如 string、number 和 boolean![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723604815344-6a63ab97-fe70-4290-aa0c-fc7ce458b035.png)

### 平等缩小
可以使用Switch或者相等的一些运算符来表达相等性（比如===, !==, ==, and != ）

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723604878037-2821fd6f-b62e-45f4-b873-9e3fbb121131.png)

### instanceof
instanceof 用于检查对象是否为某个类的实例，从而缩小类型范围![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723604830677-d27d44d1-13cc-460b-84a0-0fc4ed497f1e.png)

### in
in 操作符可以用于检查对象是否具有特定属性，从而缩小类型范围![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723604924615-cb9faf66-5411-4a95-8f4f-fd7d06c9911e.png)



### 控制流分析
TypeScript 可以根据控制流自动缩小类型。例如在条件语句中，TypeScript 会根据条件判断来推断变量的更具体类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723604979615-286fc595-a5fa-40bc-b3e7-ff407e999f8d.png)

## 函数类型
### 声明
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723703788474-f6493353-547f-4ffc-9b5e-96ef86bfef76.png)

### 函数类型表达式
使用 type 或 interface 定义一个函数类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723703815653-531d81c7-8bdd-4770-a8d6-0d69c2d143fd.png)

如图这样定义就是函数类型的表达式

### 调用签名
为了描述一个带有属性的函数，可以在一个对象类型中写一个调用签名

`(参数列表)：类型名`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723704401643-8f28e8b5-6e01-4749-853d-e11ecdb07b87.png)

注意这个语法跟函数类型表达式稍有不同，在参数列表和返回的类型之间用的是: 而不是=>

### 构造签名
构造签名用于描述一个类的构造函数的类型，当你需要使用类构造函数的类型信息来进行类型检查时，这是特别有用的

### 参数
可以有可选参数和默认参数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723703852661-8793b52a-7b39-42bf-9b4e-b63bd29a1829.png)

使用 ... 语法来处理多个参数， 剩余参数语法允许我们将一个不定数量的参数放到一个数组中  

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723704295526-8bec197f-8063-4616-b4e4-7fd8187aec52.png)

### 返回值
可以定义返回值也是一个函数的函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723704061245-ba060877-dbf7-4c96-83fe-09df3084a20f.png)

