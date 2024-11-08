**<u><font style="color:#000000;"></font></u>**

**<u><font style="color:#DF2A3F;">完整的学习笔记在该网站中 </font></u>**[<font style="color:#601BDE;">TypeScript 语言简介</font>](https://wangdoc.com/typescript/intro)<font style="color:#DF2A3F;">，</font>**<u><font style="color:#DF2A3F;">这篇笔记主要用来记录个人在学习中所遇到的问题，如果在后续的学习或工作中遇到问题，请去查看阮一峰老师的笔记寻找答案</font></u>**

# <font style="color:#000000;">any 类型</font>
## <font style="color:#000000;">any</font>
<font style="color:#000000;">变量类型一旦设为any，TypeScript 实际上会关闭这个变量的类型检查。即使有明显的类型错误，只要句法正确，都不会报错</font>

<font style="color:#000000;">从集合论的角度看，any类型可以看成是所有其他类型的全集，包含了一切可能的类型。TypeScript 将这种类型称为“顶层类型”，意为涵盖了所有下层</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723714376536-95b1261e-d0fc-4564-9b3f-4f29013d7bcb.png)

### <font style="color:#000000;">使用场景</font>
1. <font style="color:#000000;">出于特殊原因，需要关闭某些变量的类型检查，就可以把该变量的类型设为any</font>
2. <font style="color:#000000;">为了适配以前老的 JavaScript 项目，让代码快速迁移到 TypeScript，可以把变量类型设为any</font>

### <font style="color:#000000;">类型推断</font>
<font style="color:#000000;">对于开发者没有指定类型、TypeScript 必须自己推断类型的那些变量，如果无法推断出类型，TypeScript 就会认为该变量的类型是any</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723715607611-2c0a305a-d05e-4a54-9d77-838e27facd44.png)

<font style="color:#000000;">上面示例中，函数add()的参数变量x和y，都没有足够的信息，TypeScript 无法推断出它们的类型，就会认为这两个变量和函数返回值的类型都是any</font>

<font style="color:#000000;">由于这个原因，建议使用let和var声明变量时，如果不赋值，就一定要显式声明类型，否则可能存在安全隐患。const命令没有这个问题，因为 JavaScript 语言规定const声明变量时，必须同时进行初始化（赋值）</font>

### <font style="color:#000000;">污染问题</font>
<font style="color:#000000;">any类型除了关闭类型检查，还有一个很大的问题，就是它会“污染”其他变量。它可以赋值给其他任何类型的变量（因为没有类型检查），导致其他变量出错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723716703290-685bcca1-b14f-4cf8-95b8-6f16b49af5ca.png)

<font style="color:#000000;">上面示例中，变量x的类型是any，实际的值是一个字符串。变量y的类型是number，表示这是一个数值变量，但是它被赋值为x，这时并不会报错。然后，变量y继续进行各种数值运算，TypeScript 也检查不出错误，问题就这样留到运行时才会暴露。</font>

## <font style="color:#000000;">unknow</font>
<font style="color:#000000;">为了解决any类型“污染”其他变量的问题，TypeScript 3.0 引入了unknown类型。它与any含义相同，表示类型不确定，可能是任意类型，但是它的使用有一些限制，不像any那样自由，可以视为严格版的any，它也属于 TypeScript 的顶层类型</font>

### <font style="color:#000000;">与 any</font>
<font style="color:#000000;">unknown跟any的相似之处，在于所有类型的值都可以分配给unknown类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723871930834-23828ec7-4d9d-41a0-9fc6-e71bf2f200c7.png)

<font style="color:#000000;">unknown类型跟any类型的不同之处在于，它不能直接使用。主要有以下几个限制</font>

+ <font style="color:#000000;">unknown类型的变量，不能直接赋值给其他类型的变量(除了any类型和unknown类型)</font>
+ <font style="color:#000000;">不能直接调用unknown类型变量的方法和属性</font>
+ <font style="color:#000000;">unknown类型变量能够进行的运算是有限的，只能进行比较运算（运算符==、===、!=、!==、||、&&、?）、取反运算（运算符!）、typeof运算符和instanceof运算符这几种，其他运算都会报错</font>

### <font style="color:#000000;">使用</font>
<font style="color:#000000;">只有经过“类型缩小”，unknown类型变量才可以使用</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723872228977-b59caa87-d4ea-42bc-93ce-0e7c1ba77281.png)

<font style="color:#000000;">这样设计的目的是，只有明确unknown变量的实际类型，才允许使用它，防止像any那样可以随意乱用，“污染”其他变量。类型缩小以后再使用，就不会报错</font>

## <font style="color:#000000;">never</font>
<font style="color:#000000;">该类型为空，不包含任何值</font>

### <font style="color:#000000;">使用场景</font>
+ <font style="color:#000000;">主要是在一些类型运算之中，保证类型运算的完整性(后续提到)</font>
+ <font style="color:#000000;">不可能返回值的函数，返回值的类型就可以写成never(后续提到)</font>
+ <font style="color:#000000;">如果一个变量可能有多种类型（即联合类型），通常需要使用分支处理每一种类型。这时，处理所有可能的类型之后，剩余的情况就属于never类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723872485138-48fde3c5-2e8b-428e-9e40-7dc04d8b0b82.png)

### <font style="color:#000000;">特点</font>
<font style="color:#000000;">never类型的一个重要特点是，可以赋值给任意其他类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723872578494-4eb1a368-89b8-4966-a913-0710be8d864e.png)

<font style="color:#000000;">为什么never类型可以赋值给任意其他类型呢？</font>

<font style="color:#000000;">这也跟集合论有关，空集是任何集合的子集。TypeScript 就相应规定，任何类型都包含了never类型。因此，never类型是任何其他类型所共有的，TypeScript 把这种情况称为“底层类型”</font>

<font style="color:#000000;">总之，TypeScript 有两个“顶层类型”（any和unknown），但是“底层类型”只有never一个</font>

# <font style="color:#000000;">类型系统</font>
## <font style="color:#000000;">基本类型</font>
<font style="color:#000000;">JavaScript 语言（注意，不是 TypeScript）将值分成8种类型。</font>

+ <font style="color:#000000;">boolean</font>
+ <font style="color:#000000;">string</font>
+ <font style="color:#000000;">number</font>
+ <font style="color:#000000;">bigint</font>
+ <font style="color:#000000;">symbol</font>
+ <font style="color:#000000;">object</font>
+ <font style="color:#000000;">undefined</font>
+ <font style="color:#000000;">null</font>

<font style="color:#000000;">TypeScript 继承了 JavaScript 的类型设计，以上8种类型可以看作 TypeScript 的基本类型</font>

<font style="color:#000000;">注意，上面所有类型的名称都是小写字母，首字母大写的Number、String、Boolean等在 JavaScript 语言中都是内置对象，而不是类型名称</font>

<font style="color:#000000;">另外，undefined 和 null 既可以作为值，也可以作为类型，取决于在哪里使用它们</font>

<font style="color:#000000;">这8种基本类型是 TypeScript 类型系统的基础，复杂类型由它们组合而成</font>

### <font style="color:#000000;">bigint</font>
<font style="color:#000000;">bigint 类型包含所有的大整数</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723873031649-b810cbd9-d1a7-4735-a2ec-c31e457b6d16.png)

<font style="color:#000000;">bigint 与 number 类型不兼容</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723873042124-fe9813c2-c8b1-4cd7-a41e-4fb4f1088b9c.png)

<font style="color:#000000;">注意，bigint 类型是 ES2020 标准引入的。如果使用这个类型，TypeScript 编译的目标 JavaScript 版本不能低于 ES2020（即编译参数target不低于es2020）</font>

### <font style="color:#000000;">object</font>
<font style="color:#000000;">根据 JavaScript 的设计，object 类型包含了所有对象、数组和函数</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723873196722-d4626707-6c77-4680-8ad8-37093272d8da.png)

## <font style="color:#000000;">包装对象类型</font>
### <font style="color:#000000;">概念</font>
<font style="color:#000000;">JavaScript 的8种类型之中，undefined和null其实是两个特殊值，object属于复合类型，剩下的五种属于原始类型，代表最基本的、不可再分的值</font>

<font style="color:#000000;">这五种原始类型的值，都有对应的包装对象，包装对象指的是这些值在需要时，会自 动产生的对象</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">举个例子：</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723873594457-a37b032d-3d15-46c6-a087-a57f81d5dd0a.png)

<font style="color:#000000;">字符串hello执行了charAt()方法。但是在 JavaScript 语言中，只有对象才有方法，原始类型的值本身没有方法。这行代码之所以可以运行，就是因为在调用方法时，字符串会自动转为包装对象，charAt()方法其实是定义在包装对象上</font>

<font style="color:#000000;">这样的设计大大方便了字符串处理，省去了将原始类型的值手动转成对象实例的麻烦</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">五种包装对象之中，symbol 类型和 bigint 类型无法直接获取它们的包装对象（即Symbol()和BigInt()不能作为构造函数使用），但是剩下三种可以：Boolean()、String()、Number()</font>

<font style="color:#000000;">这三个构造函数，执行后可以直接获取某个原始类型值的包装对象</font>

<font style="color:#000000;">举个例子：</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723873587824-6482264a-0c1d-4f60-9b82-97d2b0a7bb18.png)

<font style="color:#000000;">上面示例中，s就是字符串hello的包装对象，typeof运算符返回object，不是string，但是本质上它还是字符串，可以使用所有的字符串方法。</font>

<font style="color:#000000;">注意，String()只有当作构造函数使用时（即带有new命令调用），才会返回包装对象。如果当作普通函数使用（不带有new命令），返回就是一个普通字符串。其他两个构造函数Number()和Boolean()也是如此。</font>

### <font style="color:#000000;">包装对象类型和字面量类型</font>
<font style="color:#000000;">由于包装对象的存在，导致每一个值都有包装对象和字面量两种情况</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723873723904-292eeffe-88ea-4676-af36-ddc86e16732c.png)

<font style="color:#000000;">为了区分这两种情况，TypeScript 对五种原始类型分别提供了大写和小写两种类型</font>

+ <font style="color:#000000;">Boolean 和 boolean</font>
+ <font style="color:#000000;">String 和 string</font>
+ <font style="color:#000000;">Number 和 number</font>
+ <font style="color:#000000;">BigInt 和 bigint</font>
+ <font style="color:#000000;">Symbol 和 symbol</font>

<font style="color:#000000;">大写类型同时包含包装对象和字面量两种情况，小写类型只包含字面量，不包含包装对象</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723873853279-b47397ac-21e4-4284-becc-accc48008477.png)

<font style="color:#000000;">建议只使用小写类型，不使用大写类型。因为绝大部分使用原始类型的场合，都是使用字面量，不使用包装对象。而且TypeScript 把很多内置方法的参数，定义成小写类型，使用大写类型会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723874223539-3fa06686-15d6-4350-9e0e-04d3dc36e7ca.png)

## <font style="color:#000000;">Object 和 object 类型</font>
### <font style="color:#000000;">Object</font>
<font style="color:#000000;">大写的Object类型代表 JavaScript 语言里面的广义对象，所有可以转成对象的值，都是Object类型。</font>

<font style="color:#000000;">事实上，除了undefined和null这两个值不能转为对象，其他任何值都可以赋值给Object类型</font>

<font style="color:#000000;">空对象{}是Object类型的简写形式，所以使用Object时常常用空对象代替</font>

```typescript
let obj:Object;
===> let obj:{};

obj = true;
obj = 'hi';
obj = 1;
obj = { foo: 123 };
obj = [1, 2];
obj = (a:number) => a + 1;

obj = undefined; // 报错
obj = null; // 报错
```

### <font style="color:#000000;">object</font>
<font style="color:#000000;">小写的object类型代表 JavaScript 里面的狭义对象，即可以用字面量表示的对象，只包含对象、数组和函数，不包括原始类型的值</font>

```typescript
let obj:object;

obj = { foo: 123 };
obj = [1, 2];
obj = (a:number) => a + 1;
obj = true; // 报错
obj = 'hi'; // 报错
obj = 1; // 报错
```

### <font style="color:#000000;">总结</font>
<font style="color:#000000;">大多数时候，我们使用对象类型，只希望包含真正的对象，不希望包含原始类型。所以，建议总是使用小写类型object，不使用大写类型Object</font>

<font style="color:#000000;">注意，无论是大写的Object类型，还是小写的object类型，都只包含 JavaScript 内置对象原生的属性和方法，用户自定义的属性和方法都不存在于这两个类型之中</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723874906083-132edeed-67be-4179-b01e-90480a83a0f8.png)

## <font style="color:#000000;">undefined 和 null </font>
<font style="color:#000000;">undefined和null既是值，又是类型。</font>

<font style="color:#000000;">作为值，它们有一个特殊的地方：任何其他类型的变量都可以赋值为undefined或null</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723874964622-3b86641d-0e8e-45c5-ba11-c54972c1e576.png)

<font style="color:#000000;">这并不是因为undefined和null包含在number类型里面，而是故意这样设计，任何类型的变量都可以赋值为undefined和null，以便跟 JavaScript 的行为保持一致：变量如果等于undefined就表示还没有赋值，如果等于null就表示值为空。所以，TypeScript 就允许了任何类型的变量都可以赋值为这两个值</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">为了避免这种情况，及早发现错误，TypeScript 提供了一个编译选项strictNullChecks。只要打开这个选项，undefined和null就不能赋值给其他类型的变量（除了any类型和unknown类型）</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723876901637-3b794fdc-ff9e-4363-9e87-30e3a84f5e3a.png)

## <font style="color:#000000;">值类型</font>
<font style="color:#000000;">TypeScript 规定，单个值也是一种类型，称为“值类型”</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723875582561-1573755c-64da-405e-b067-8403c67bbd91.png)

<font style="color:#000000;">变量x的类型是字符串hello，导致它只能赋值为这个字符串，赋值为其他字符串就会报错</font>

<font style="color:#000000;">TypeScript 推断类型时，遇到const命令声明的变量，如果代码里面没有注明类型，就会推断该变量是值类型</font>

<font style="color:#000000;">这样推断是合理的，因为const命令声明的变量，一旦声明就不能改变，相当于常量。值类型就意味着不能赋为其他值</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723875647353-6d543c20-19e7-45ff-90de-d3e8384b34b9.png)

<font style="color:#000000;">注意，const命令声明的变量，如果赋值为对象，并不会推断为值类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723875704768-61ebc4cd-27e9-4a7f-842e-29a95789582e.png)

## <font style="color:#000000;">联合类型</font>
<font style="color:#000000;">联合类型指的是多个类型组成的一个新类型，使用符号|表示，联合类型A|B表示，任何一个类型只要属于A或B，就属于联合类型A|B</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723876007806-c51e86bb-c2da-466d-a2dd-c550b1f8d47c.png)

<font style="color:#000000;">联合类型可以与值类型相结合，表示一个变量的值有若干种可能</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723876043202-f0e66134-ed38-469b-96ca-55b2ae728ee1.png)

<font style="color:#000000;">上面的示例都是由值类型组成的联合类型，非常清晰地表达了变量的取值范围。其中，true|false其实就是布尔类型boolean</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">前面提到，打开编译选项strictNullChecks后，其他类型的变量不能赋值为undefined或null。这时，如果某个变量确实可能包含空值，就可以采用联合类型的写法</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723876812326-240bd918-d6ac-4601-a335-cf88e40eb3b9.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">联合类型的第一个成员前面，也可以加上竖杠|，这样便于多行书写</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723877026911-30f1acdc-3665-4abc-a55b-f457e379c968.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">类型缩小是 TypeScript 处理联合类型的标准方法，凡是遇到可能为多种类型的场合，都需要先缩小类型，再进行处理。实际上，联合类型本身可以看成是一种“类型放大”，处理时就需要“类型缩小”</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723877106095-e270d3f6-b9c2-4fd1-8560-d10e9e82cf63.png)

## <font style="color:#000000;">交叉类型</font>
<font style="color:#000000;">交叉类型指的多个类型组成的一个新类型，使用符号&表示</font>

<font style="color:#000000;">交叉类型A&B表示，任何一个类型必须同时属于A和B，才属于交叉类型A&B，即交叉类型同时满足A和B的特征</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">交叉类型的主要用途是表示对象的合成</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723877309218-5fa14a53-7b3e-42fe-95f1-068e08e32697.png)<font style="color:#000000;"> </font>

<font style="color:#000000;"></font>

<font style="color:#000000;">交叉类型常常用来为对象类型添加新属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723878231888-5e63810c-cee1-4355-8d46-e9a5d07a8a65.png)

## <font style="color:#000000;">type 命令</font>
<font style="color:#000000;">type命令用来定义一个类型的别名，别名可以让类型的名字变得更有意义，也能增加代码的可读性，还可以使复杂类型用起来更方便，便于以后修改变量的类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723878276213-9f4e1235-b617-4776-b64e-634a06e64b9a.png)

<font style="color:#000000;">type命令为number类型定义了一个别名Age，这样就能像使用number一样，使用Age作为类型</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">别名不允许重名</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723878380339-b6326442-c15b-45a4-bd6c-94cd23a89d0d.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">别名的作用域是块级作用域，这意味着，代码块内部定义的别名，影响不到外部</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723878493727-3f59d588-ccfa-45de-9dfd-6d3d7c3cea7c.png)

<font style="color:#000000;">if代码块内部的类型别名Color，跟外部的Color是不一样的</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">别名支持使用表达式，也可以在定义一个别名时，使用另一个别名，即别名允许嵌套</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723878545982-5585c4e5-e6b1-41c7-94d1-b176782c9bc5.png)

## <font style="color:#000000;">typeof 运算符</font>
<font style="color:#000000;">JavaScript 里面，typeof运算符只可能返回八种结果，而且都是字符串</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723878607140-9fb196b6-4a0c-46e4-b7a2-51819fc31072.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">TypeScript 将typeof运算符移植到了类型运算，它的操作数依然是一个值，但是返回的不是字符串，而是该值的 TypeScript 类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723878651888-9d7e8785-f9d3-4e97-93c4-7e652cfaa075.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">也就是说，同一段代码可能存在两种typeof运算符，一种用在值相关的 JavaScript 代码部分，另一种用在类型相关的 TypeScript 代码部分，它们的一个重要区别在于编译后，前者会保留，后者会被全部删除。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723880408723-578a8b7e-d846-4d69-a174-939ca591151a.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723880456129-2d5efffb-b552-4829-8528-8947c161bad9.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">由于编译时不会进行 JavaScript 的值运算，所以TypeScript 规定，typeof 的参数只能是标识符，不能是需要运算的表达式</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723880507555-1944484f-3774-48db-89b6-030ebd8203e6.png)

<font style="color:#000000;">typeof 命令的参数不能是类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723880537072-6f43dbf1-42a8-4dfa-9085-49af0b3c458d.png)

## <font style="color:#000000;">块级类型说明</font>
<font style="color:#000000;">TypeScript 支持块级类型声明，即类型可以声明在代码块（用大括号表示）里面，并且只在当前代码块有效</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723880625882-a122d40d-6dd9-4117-842a-5b2194bc83b7.png)

<font style="color:#000000;">这两个声明都只在自己的代码块内部有效，在代码块外部无效</font>

## <font style="color:#000000;">类型的兼容</font>
<font style="color:#000000;">TypeScript 的类型存在兼容关系，某些类型可以兼容其他类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723880736584-4490ac14-327a-49df-9b62-34215a807197.png)

<font style="color:#000000;">变量a和b的类型是不一样的，但是变量a赋值给变量b并不会报错。这时就认为b的类型兼容a的类型</font>

<font style="color:#000000;">TypeScript 为这种情况定义了一个专门术语。如果类型A的值可以赋值给类型B，那么类型A就称为类型B的</font>**<font style="color:#000000;">子类型</font>**<font style="color:#000000;">。在上例中，类型number就是类型number|string的子类型</font>

<font style="color:#000000;">TypeScript 的一个规则是，凡是可以使用父类型的地方，都可以使用子类型，但是反过来不行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723880797719-319d9995-91a0-4070-be2b-5a4c7b6c1dd8.png)

<font style="color:#000000;">hi是string的子类型，string是hi的父类型。所以，变量a可以赋值给变量b，但是反过来就会报错。</font>

# <font style="color:#000000;">数组</font>
<font style="color:#000000;">TypeScript 数组有一个根本特征：所有成员的类型必须相同，但是成员数量是不确定的，可以是无限数量的成员，也可以是零成员</font>

## <font style="color:#000000;">写法</font>
<font style="color:#000000;">数组的类型有两种写法</font>

<font style="color:#000000;">第一种写法是在数组成员的类型后面，加上一对方括号</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723949654602-382f35af-540d-4104-98f1-8c5fb398006c.png)

<font style="color:#000000;">如果数组成员的类型比较复杂，可以写在圆括号里面</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723949715749-6d8eeb63-53c9-4c71-aa8e-2daa4f72bf59.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">数组类型的第二种写法是使用 TypeScript 内置的 Array 接口</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723949767927-2043794f-4f14-4a71-b149-76ed2b4d4514.png)

<font style="color:#000000;">这种写法对于成员类型比较复杂的数组，代码可读性会稍微好一些</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723950018821-a3fc1a8d-cbd8-44f7-b8b0-142ff08bf2ca.png)

## <font style="color:#000000;">特点</font>
<font style="color:#000000;">数组类型声明了以后，成员数量是不限制的，任意数量的成员都可以，也可以是空数组，</font><font style="color:#000000;">这种规定的隐藏含义就是，数组的成员是可以动态变化的</font>

<font style="color:#000000;">正是由于成员数量可以动态变化，所以 TypeScript 不会对数组边界进行检查，越界访问数组并不会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723950183838-c1aae9e1-4ed4-4bea-a321-64fac6cfcda9.png)

<font style="color:#000000;">变量foo的值是一个不存在的数组成员，TypeScript 并不会报错</font>

## <font style="color:#000000;">类型推断</font>
<font style="color:#000000;">如果数组变量没有声明类型，TypeScript 就会推断数组成员的类型。这时，推断行为会因为值的不同，而有所不同</font>

<font style="color:#000000;">空数组==>any[]，为这个数组赋值时，TypeScript 会自动更新类型推断，但是类型推断的自动更新，只发生初始值为空数组的情况</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723950345444-32cf05af-9cc6-41f5-bc7f-18bbbaa74911.png)

## <font style="color:#000000;">只读数组</font>
<font style="color:#000000;">JavaScript 规定，const命令声明的数组变量是可以改变成员的</font>

<font style="color:#000000;">但是，很多时候确实有声明为只读数组的需求，方法是在数组类型前面加上readonly关键字</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723950553197-d84c0b0f-fa0a-4670-ad54-e513067f4e0e.png)

<font style="color:#000000;">TypeScript 将readonly number[]与number[]视为两种不一样的类型，后者是前者的子类型。这是因为只读数组没有pop()、push()之类会改变原数组的方法，所以number[]的方法数量要多于readonly number[]，这意味着number[]其实是readonly number[]的子类型。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723950776024-56f7b07a-e20d-49d8-843b-098646018d96.png)

<font style="color:#000000;">子类型number[]可以赋值给父类型readonly number[]，但是反过来就会报错</font>

<font style="color:#000000;">注意，readonly关键字不能与数组的泛型写法一起使用</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723950918365-515a800a-6457-49cd-adb4-4f1f488b360a.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">实际上，TypeScript 提供了两个专门的泛型，用来生成只读数组的类型</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723950975533-01e9f1c8-2171-4aa2-87fb-c8adcb51ac5b.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">只读数组还有一种声明方法，就是使用“const 断言”</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723951012576-45caced8-9334-4dd5-90ad-5d701c3740e4.png)

<font style="color:#000000;">as const告诉 TypeScript，推断类型时要把变量arr推断为只读数组，从而使得数组成员无法改变</font>

## <font style="color:#000000;">多维数组</font>
<font style="color:#000000;">TypeScript 使用T[][]的形式，表示二维数组，T是最底层数组成员的类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1723951043078-dc325ff2-eb01-43a7-8f76-fd05d950e06b.png)

# <font style="color:#000000;">元组</font>
## <font style="color:#000000;">简介</font>
### <font style="color:#000000;">类型</font>
<font style="color:#000000;">元组表示成员类型可以自由设置的数组，即数组的各个成员的类型可以不同</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724032752210-ad123ab0-0f02-43c0-b0c1-8f23256ee726.png)

<font style="color:#000000;">元组的成员类型是写在方括号里面 [number]</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724032816301-fe5ea42f-1334-4b79-a284-ec9824e25492.png)

<font style="color:#000000;">使用元组时，必须明确给出类型声明，不能省略，否则 TypeScript 会把一个值自动推断为数组</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724033763969-be8c33fd-60cb-43ba-bf46-cc6ab8c589b3.png)

### <font style="color:#000000;">成员数量</font>
<font style="color:#000000;">由于需要声明每个成员的类型，所以大多数情况下，元组的成员数量是有限的，从类型声明就可以明确知道元组包含多少个成员，越界的成员会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724034100167-fb71f72e-53b7-4249-9bb8-21e7cf59a201.png)

<font style="color:#000000;">使用扩展运算符（...），可以表示不限成员数量的元组</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724034229409-d1bfbb84-7380-4b68-bc83-a500e49262a8.png)

<font style="color:#000000;">扩展运算符（...）用在元组的任意位置都可以，它的后面只能是一个数组或元组</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724034348294-5456342a-6161-43ce-8f13-8677d0f5595c.png)

<font style="color:#000000;">如果不确定元组成员的类型和数量，可以写成下面这样</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724034479887-753ec885-0136-4bcf-8a7b-8c27facd199b.png)

### <font style="color:#000000;">可选成员</font>
<font style="color:#000000;">元组成员的类型可以添加问号后缀（?），表示该成员是可选的，但是问号只能用于元组的尾部成员，也就是说所有可选成员必须在必选成员之后</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724033792843-4fa9b66c-daf1-4cbd-9ad9-f995d560c331.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724034042043-62535573-da12-465e-8fa7-79dbe76bba37.png)

## <font style="color:#000000;">只读元组</font>
<font style="color:#000000;">两种写法</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724034732453-eec30fd7-c707-4573-9d21-637ff484900d.png)

<font style="color:#000000;">跟数组一样，只读元组是元组的父类型。所以，元组可以替代只读元组，而只读元组不能替代元组</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035466959-ea7c5b67-9348-49cb-8097-abc57bc21093.png)

## <font style="color:#000000;">成员数量的推断</font>
<font style="color:#000000;">如果没有可选成员和扩展运算符，TypeScript 会推断出元组的成员数量（即元组长度）</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035562284-4c9a2d4a-af3e-4761-9467-1211e70bb0f1.png)

<font style="color:#000000;">如果包含了可选成员，TypeScript 会推断出可能的成员数量</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035586501-95aa1d4f-68b2-4b41-8694-68a585bf79a5.png)

## <font style="color:#000000;">扩展运算符与成员数量</font>
<font style="color:#000000;">扩展运算符（...）将数组（注意，不是元组）转换成一个逗号分隔的序列，这时 TypeScript 会认为这个序列的成员数量是不确定的，因为数组的成员数量是不确定的。这导致如果函数调用时，使用扩展运算符传入函数参数，可能发生参数数量与数组长度不匹配的报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035676664-a7bee03c-dcf6-487c-b670-1958abfed8c4.png)

<font style="color:#000000;">有些函数可以接受任意数量的参数，这时使用扩展运算符就不会报错</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035687750-830c1074-d1a0-4b2f-905b-4a8acf358d6c.png)<font style="color:#000000;"></font>

<font style="color:#000000;">解决方法就是，把成员数量不确定的数组，写成成员数量确定的元组，再使用扩展运算符</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035724513-a2eecf55-0619-4963-b153-0a789dc02850.png)

<font style="color:#000000;">另一种写法是使用as const断言</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035765190-535cf2a2-ea15-4390-a7f0-f1fd3cbf22f8.png)

<font style="color:#000000;">因为 TypeScript 会认为arr的类型是readonly [1, 2]，这是一个只读的值类型，可以当作数组，也可以当作元组</font>

# <font style="color:#000000;">symbol 类型</font>
## <font style="color:#000000;">简介</font>
<font style="color:#000000;">Symbol 是 ES2015 新引入的一种原始类型的值。它类似于字符串，但是每一个 Symbol 值都是独一无二的，与其他任何值都不相等</font>

<font style="color:#000000;">Symbol 值通过Symbol()函数生成。在 TypeScript 里面，Symbol 的类型使用symbol表示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035867767-e1c2b633-e64d-4072-a7e5-8fdc4d0f4b77.png)

## <font style="color:#000000;">unique symbol</font>
<font style="color:#000000;">symbol类型包含所有的 Symbol 值，但是无法表示某一个具体的 Symbol 值。比如5是一个具体的数值，就用5这个字面量来表示，这也是它的值类型。但是，Symbol 值不存在字面量，必须通过变量来引用，所以写不出只包含单个 Symbol 值的那种值类型</font>

<font style="color:#000000;">为了解决这个问题，TypeScript 设计了symbol的一个子类型unique symbol，它表示单个的、某个具体的 Symbol 值。因为unique symbol表示单个值，所以这个类型的变量是不能修改值的，只能用const命令声明，不能用let声明</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035958315-37cf7751-e840-4550-b5bb-c0d1a681a57b.png)

<font style="color:#000000;">const 命令为变量赋值 Symbol 值时，变量类型默认就是unique symbol，所以类型可以省略不写</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724035978855-873ca80f-b7de-4465-894b-ae31a7dba7a6.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">每个声明为unique symbol类型的变量，它们的值都是不一样的，其实属于两个值类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724036208686-838c8fe7-9500-4632-98b6-1a11d5d119c3.png)

<font style="color:#000000;">而且，由于变量a和b是两个类型，就不能把一个赋值给另一个，如果要写成与变量a同一个unique symbol值类型，只能写成类型为typeof a</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724036391570-35e0bfaf-267f-4e90-a734-e1477fe2575f.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724036519360-626bc887-cf1b-459c-9e70-f0db7ef59ba5.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">unique symbol 类型是 symbol 类型的子类型，所以可以将前者赋值给后者，但是反过来就不行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724036572529-614c2c0d-1abc-4442-b5f4-0d91ce8e7b7b.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">unique symbol 类型的一个作用，就是用作属性名，这可以保证不会跟其他属性名冲突。如果要把某一个特定的 Symbol 值当作属性名，那么它的类型只能是 unique symbol，不能是 symbol</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724036617373-b477d410-c7be-4db0-a24b-9252edfa0e24.png)

## <font style="color:#000000;">类型推断</font>
<font style="color:#000000;">如果变量声明时没有给出类型，TypeScript 会推断某个 Symbol 值变量的类型</font>

+ <font style="color:#000000;">let命令声明的变量，推断类型为 symbol</font>
+ <font style="color:#000000;">const命令声明的变量，推断类型为 unique symbol</font>
+ <font style="color:#000000;">const命令声明的变量，如果赋值为另一个 symbol 类型的变量，则推断类型为 symbol</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724036705480-deae5bf7-4fd9-414e-a4d6-8471b31d92c6.png)

+ <font style="color:#000000;">let命令声明的变量，如果赋值为另一个 unique symbol 类型的变量，则推断类型还是 symbol</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724036723789-3caf5a1e-41fb-4200-9e61-14d28ed700df.png)

# <font style="color:#000000;">函数</font>
## <font style="color:#000000;">简介</font>
<font style="color:#000000;">函数的类型声明，需要在声明函数时，给出参数的类型和返回值的类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724054235099-70ad8be9-e371-4578-b9fa-8844d5305e9c.png)

<font style="color:#000000;">返回值的类型通常可以不写，因为 TypeScript 自己会推断出来</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">如果变量被赋值为一个函数，变量的类型有两种写法</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724054262856-aaed1789-4cd9-41ea-a06b-668c59044395.png)

<font style="color:#000000;">写法一是通过等号右边的函数类型，推断出变量hello的类型</font>

<font style="color:#000000;">写法二则是使用箭头函数的形式，为变量hello指定类型，参数的类型写在箭头左侧，返回值的类型写在箭头右侧</font>

<font style="color:#000000;">函数类型里面的参数名与实际参数名，可以不一致</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724054804937-82859f4e-5c0b-4f88-a8c6-d5c9f737854e.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果函数的类型定义很冗长，或者多个函数使用同一种类型，写法二用起来就很麻烦。因此，往往用type命令为函数类型定义一个别名，便于指定给其他变量</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724054849092-d568fd23-48dc-47a1-b47a-f72beb2b9fed.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">函数的实际参数个数，可以少于类型指定的参数个数，但是不能多于，即 TypeScript 允许省略参数</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724054991692-85470111-edfd-407e-8f38-dc5b8717347b.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果一个变量要套用另一个函数类型，有一个小技巧，就是使用 typeof 运算符</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724055058513-92e818ed-bf75-48bd-9ed9-aaeed11c40fb.png)

## <font style="color:#000000;">Function 类型</font>
<font style="color:#000000;">TypeScript 提供 Function 类型表示函数，任何函数都属于这个类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724055984251-79a1a506-a1b1-46cf-ba53-35120dee46a2.png)

## <font style="color:#000000;">箭头函数</font>
<font style="color:#000000;">箭头函数是普通函数的一种简化写法，它的类型写法与普通函数类似</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724056016804-0b0a87e4-1d36-464a-86a1-9619b653e3c9.png)

<font style="color:#000000;">注意，类型写在箭头函数的定义里面，与使用箭头函数表示函数类型，写法有所不同</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724056081614-9134974d-c545-40db-b91f-bbe5efc73be3.png)

<font style="color:#000000;">函数greet()的参数fn是一个函数，类型就用箭头函数表示。这时，fn的返回值类型要写在箭头右侧，而不是写在参数列表的圆括号后面可选参数</font>

## <font style="color:#000000;">可选参数</font>
<font style="color:#000000;">如果函数的某个参数可以省略，则在参数名后面加问号表示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724056510075-b471716a-9ee0-482f-9ab4-e4166a6f1790.png)

<font style="color:#000000;">参数名带有问号，表示该参数的类型实际上是原始类型|undefined，它有可能为undefined。比如，上例的x虽然类型声明为number，但是实际上是number|undefined</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724056535060-45276b86-48d8-4d89-9cb0-0a1b97ac36a5.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">函数的可选参数只能在参数列表的尾部，跟在必选参数的后面</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724056565618-50bce1b6-720a-4ac4-82b9-fc3955cad1bb.png)

<font style="color:#000000;">函数体内部用到可选参数时，需要判断该参数是否为undefined</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724057693541-8ee018c6-5f11-49be-aa77-b6839a92fa65.png)

## <font style="color:#000000;">参数默认值</font>
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724057849520-9249c98d-3b1c-43eb-b565-963789bff3ed.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">可选参数与默认值不能同时使用</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724057878423-06be71d0-a9af-40ae-8d13-e16cd616254c.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">设有默认值的参数，如果传入undefined，也会触发默认值</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724058631749-bfb72d2e-68b7-4b47-9e7a-363874f67dc0.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">具有默认值的参数如果不位于参数列表的末尾，调用时不能省略，如果要触发默认值，必须显式传入undefined</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203211375-133f6377-dc79-459e-89f8-d5d86235e03d.png)

## <font style="color:#000000;">参数解构</font>
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203313371-9c3b8d3b-de0e-401b-b6af-6d54b186d4b5.png)

<font style="color:#000000;">参数解构可以结合类型别名（type 命令）一起使用，代码会看起来简洁一些</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203339479-5969d12a-155d-4dc3-99c9-5c2c0f18bac7.png)

## <font style="color:#000000;">rest 参数</font>
<font style="color:#000000;">rest 参数表示函数剩余的所有参数，它可以是数组（剩余参数类型相同），也可能是元组（剩余参数类型不同）</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203562779-094d787a-65ce-4aa8-93ca-ddf4d1ee242d.png)

<font style="color:#000000;">注意，元组需要声明每一个剩余参数的类型。如果元组里面的参数是可选的，则要使用可选参数</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203742159-70bce9c8-cdd3-4000-838f-7a2d501512c6.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">rest 参数甚至可以嵌套</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203803639-3b5cf09a-e877-44ac-a2a6-71aa428fe296.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">rest 参数可以与变量解构结合使用</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203820890-e01e455e-d6f7-4d17-bf6c-2b4202779dbb.png)

## <font style="color:#000000;">readonly只读参数</font>
<font style="color:#000000;">可以在函数定义时，在参数类型前面加上readonly关键字，表示这是只读参数</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203898261-243a3c7d-7198-4573-9f44-30007b0cd58b.png)

## <font style="color:#000000;">void 类型</font>
<font style="color:#000000;">void 类型表示函数没有返回值</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203957622-bdc75c3d-0557-437f-86e3-5a10d7bf6cc6.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果返回其他值，就会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724203984358-837162de-0a9a-43d7-9c92-78827d16360e.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">void 类型允许返回undefined或null</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204014968-07ec3e21-29f7-4d4f-acda-7099a322198e.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果打开了strictNullChecks编译选项，那么 void 类型只允许返回undefined。如果返回null，就会报错。这是因为 JavaScript 规定，如果函数没有返回值，就等同于返回undefined</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204099931-02e9cd10-d985-43fd-98fb-9dc8b542044d.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">需要特别注意的是，如果变量、对象方法、函数参数是一个返回值为 void 类型的函数，那么并不代表不能赋值为有返回值的函数。恰恰相反，该变量、对象方法和函数参数可以接受返回任意值的函数，这时并不会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204375213-8c7cd63d-2bcc-44a4-b5e9-34a410a2e435.png)

<font style="color:#000000;">这是因为，这时 TypeScript 认为，这里的 void 类型只是表示该函数的返回值没有利用价值，或者说不应该使用该函数的返回值。只要不用到这里的返回值，就不会报错</font>

<font style="color:#000000;">这样设计是有现实意义的，举例来说，数组方法Array.prototype.forEach(fn)的参数fn是一个函数，而且这个函数应该没有返回值，即返回值类型是void。但是，实际应用中，很多时候传入的函数是有返回值，但是它的返回值不重要，或者不产生作用</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204442587-9fce0ef5-636c-40ae-824b-4fa78b95446c.png)

<font style="color:#000000;">push()有返回值，表示插入新元素后数组的长度。但是，对于forEach()方法来说，这个返回值是没有作用的，根本用不到，所以 TypeScript 不会报错</font>

<font style="color:#000000;">如果后面使用了这个函数的返回值，就违反了约定，则会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204494994-7f20ae83-73ae-4d79-b0dd-66ecc879136d.png)

## <font style="color:#000000;">never 类型</font>
<font style="color:#000000;">never 类型表示肯定不会出现的值。它用在函数的返回值，就表示某个函数肯定不会返回值，即函数不会正常执行结束</font>

<font style="color:#000000;">它主要有以下两种情况</font>

1. <font style="color:#000000;">抛出错误的函数</font>
    - ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204868502-9d107a58-7340-4833-b8b2-ead28e2ed5b2.png)
    - <font style="color:#000000;">注意，只有抛出错误，才是 never 类型。如果显式用return语句返回一个 Error 对象，返回值就不是 never 类型</font>
    - ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204860630-7a673e43-6d60-411a-9afa-a33eaea9790b.png)
2. <font style="color:#000000;">无限执行的函数</font>
    - ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204883407-9146a777-a401-4ff4-8b4e-8b9aa8cd9ffa.png)
    - <font style="color:#000000;">如果一个函数抛出了异常或者陷入了死循环，那么该函数无法正常返回一个值，因此该函数的返回值类型就是never。如果程序中调用了一个返回值类型为never的函数，那么就意味着程序会在该函数的调用位置终止，永远不会继续执行后续的代码</font>
    - <font style="color:#000000;">一个函数如果某些条件下有正常返回值，另一些条件下抛出错误，这时它的返回值类型可以省略never</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204962996-dbb1cec5-8bc5-4fa1-b37e-40c7b5ef620f.png)

## <font style="color:#000000;">局部类型</font>
<font style="color:#000000;">函数内部允许声明其他类型，该类型只在函数内部有效，称为局部类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724204596449-3ca4cbac-6a9b-4b83-9346-31b90c7365f9.png)

<font style="color:#000000;">类型message是在函数hello()内部定义的，只能在函数内部使用。在函数外部使用，就会报错</font>

## <font style="color:#000000;">高阶函数</font>
<font style="color:#000000;">一个函数的返回值还是一个函数，那么前一个函数就称为高阶函数，下面就是一个例子，箭头函数返回的还是一个箭头函数</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205029689-cfc8d18d-a6ab-4c30-b57b-db4fa84cc8d9.png)

## <font style="color:#000000;">函数重载(难)</font>
<font style="color:#000000;">有些函数可以接受不同类型或不同个数的参数，并且根据参数的不同，会有不同的函数行为。这种根据参数类型不同，执行不同逻辑的行为，称为函数重载</font>

<font style="color:#000000;">TypeScript 对于“函数重载”的类型声明方法是，逐一定义每一种情况的类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205398552-af1b77ce-6baa-4304-8252-2fe334d45231.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205404252-2e55f56c-1f8c-4715-9da8-ddbdc0289e20.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">有一些编程语言允许不同的函数参数，对应不同的函数实现。但是，JavaScript 函数只能有一个实现，必须在这个实现当中，处理不同的参数。因此，函数体内部就需要判断参数的类型及个数，并根据判断结果执行不同的操作</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205509692-5e6d60ec-92cc-4e13-a7ec-da486370cac8.png)

<font style="color:#000000;">注意，重载的各个类型描述与函数的具体实现之间，不能有其他代码，否则报错</font>

<font style="color:#000000;">另外，虽然函数的具体实现里面，有完整的类型声明。但是，函数实际调用的类型，</font>

<font style="color:#000000;">以前面的</font><font style="color:#000000;">类型声明为准。比如，上例的函数实现，参数类型和返回值类型都</font>

<font style="color:#000000;">number|any[]，但不意味着参数类型为number时返回值类型为any[]</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">函数重载的每个类型声明之间，以及类型声明与函数实现的类型之间，不能有冲突</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205616049-571470b8-28ab-40b6-8be8-83e8e1113eb1.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">重载声明的排序很重要，因为 TypeScript 是按照顺序进行检查的，一旦发现符合某个类型声明，就不再往下检查了，所以类型最宽的声明应该放在最后面，防止覆盖其他类型声明</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205638656-be4d7d0e-8b7f-4364-87ea-d56e721ed0c7.png)

<font style="color:#000000;">第一行类型声明x:any范围最宽，导致函数f()的调用都会匹配这行声明，无法匹配第二行类型声明，所以最后一行调用就报错了，因为等号两侧类型不匹配，左侧类型是0|1，右侧类型是number。这个函数重载的正确顺序是，第二行类型声明放到第一行的位置</font>

<font style="color:#000000;">由于重载是一种比较复杂的类型声明方法，为了降低复杂性，一般来说，如果可以的话，应该优先使用联合类型替代函数重载，除非多个参数之间、或者某个参数与返回值之间，存在对应关系</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724206578684-3cd4b556-0565-4b86-8fb5-897ae791b2f9.png)

## <font style="color:#000000;">构造函数(不理解)</font>
<font style="color:#000000;">JavaScript 语言使用构造函数，生成对象的实例</font>

<font style="color:#000000;">构造函数的最大特点，就是必须使用new命令调用</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205066558-1147c727-9c19-4576-8542-de68cc9d62ef.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">构造函数的类型写法，就是在参数列表前面加上new命令</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205098441-1724f4b5-092b-4e74-a8a0-9c85ce2bc6c1.png)

<font style="color:#000000;">构造函数还有另一种类型写法，就是采用对象形式</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205149241-e5801c76-b6c3-4bb4-a2d9-3f3bf2048f63.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">某些函数既是构造函数，又可以当作普通函数使用，比如Date()。</font>

<font style="color:#000000;">这时，类型声明可以写成下面这样</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724205237945-6aa0de4a-107a-423a-b51e-5ac3d65f6ebe.png)

<font style="color:#000000;">F 既可以当作普通函数执行，也可以当作构造函数使用</font>

# <font style="color:#000000;">对象 -</font>
## <font style="color:#000000;">简介</font>
<font style="color:#000000;">除了原始类型，对象是 JavaScript 最基本的数据结构。TypeScript 对于对象类型有很多规则</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">对象类型的最简单声明方法，就是使用大括号表示对象，在大括号内部声明每个属性和方法的类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724662180048-349cbd04-28ea-418b-94cc-a2ba2b4a1520.png)

<font style="color:#000000;">属性的类型可以用分号结尾，也可以用逗号结尾</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">一旦声明了类型，对象赋值时，就不能缺少指定的属性，也不能有多余的属性，</font><font style="color:#000000;">读写不存在的属性也会报错，也不能删除类型声明中存在的属性，修改属性值是可以的</font><font style="color:#000000;">  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724662472487-d110ae64-f945-48a1-b72d-2640d758ccb2.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">对象的方法使用函数类型描述</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724662554162-ad05a034-a834-4251-8590-7501e23b9c9e.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">对象类型可以使用方括号读取属性的类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724662804281-054c426f-5280-46de-bcd5-8b967f23b928.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">除了type命令可以为对象类型声明一个别名，TypeScript 还提供了interface命令，可以把对象类型提炼为一个接口</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724663238595-ea37d0b3-bc60-4cc6-95ef-6eea4db490d0.png)

## <font style="color:#000000;">可选属性</font>
<font style="color:#000000;">如果某个属性是可选的（即可以忽略），需要在属性名后面加一个问号</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724663270170-7f2f55b0-8a37-4b1a-8d5b-433ca42ffd81.png)

<font style="color:#000000;">可选属性等同于允许赋值为undefined，下面两种写法是等效的</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724663324384-11971b44-c5be-4732-b2a1-f11f1c2ab43f.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">可选属性可以赋值为undefined，读取一个没有赋值的可选属性时，返回undefined，所以读取可选属性之前必须检查一下是否为undefined</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724663498771-3421a6b5-c0c3-4e5d-8c55-1030ee801e78.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">TypeScript 提供编译设置ExactOptionalPropertyTypes，只要同时打开这个设置和strictNullChecks，可选属性就不能设为undefined</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724663592115-a5a357e8-e1f8-468a-a875-0b3090e5161e.png)

## <font style="color:#000000;">只读属性</font>
<font style="color:#000000;">属性名前面加上readonly关键字，表示这个属性是只读属性，</font><font style="color:#000000;">只读属性只能在对象初始化期间赋值，此后就不能修改该属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724663628140-fbad696b-a46e-4ce8-ac12-e4430b28d811.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果属性值是一个对象，readonly修饰符并不禁止修改该对象的属性，只是禁止完全替换掉该对象</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724663689293-7bba738b-6828-4777-9948-3aa2f46f39f1.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果一个对象有两个引用，即两个变量对应同一个对象，其中一个变量是可写的，另一个变量是只读的，那么从可写变量修改属性，会影响到只读变量</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724664054792-76f69e44-93fd-4348-802a-3dbf75b8af8a.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">除了声明时加上readonly关键字，还有一种方法，就是在赋值时，在对象后面加上只读断言as const</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724664080691-f92ad4d1-640e-4023-abbb-7c8942a1f4bf.png)

## <font style="color:#000000;">属性名的索引类型</font>
<font style="color:#000000;">如果对象的属性非常多，一个个声明类型就很麻烦，而且有些时候，无法事前知道对象会有多少属性，比如外部 API 返回的对象。这时 TypeScript 允许采用</font>**<font style="color:#000000;">属性名表达式</font>**<font style="color:#000000;">的写法来描述类型，称为“</font>**<font style="color:#000000;">属性名的索引类型</font>**<font style="color:#000000;">”‘</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">索引类型里面，最常见的就是属性名的</font>**<font style="color:#000000;">字符串</font>**<font style="color:#000000;">索引</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724669429748-09bb1398-ef03-46ff-8d74-95f2322512f8.png)

<font style="color:#000000;">类型MyObj的属性名类型就采用了表达式形式，写在方括号里面。[property: string]的property表示属性名，这个是可以随便起的，它的类型是string，即属性名类型为string。也就是说，不管这个对象有多少属性，只要属性名为字符串，且属性值也是字符串，就符合这个类型声明</font>

<font style="color:#000000;">JavaScript 对象的属性名（即上例的property）的类型有三种可能，除了上例的string，还有number和symbol</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724669745888-9485e0cf-ca6a-41c8-8979-43554b1ec89b.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">对象可以同时有多种类型的属性名索引，比如同时有数值索引和字符串索引。但是，数值索引不能与字符串索引发生冲突，必须服从后者，这是因为在 JavaScript 语言内部，所有的数值属性名都会自动转为字符串属性名</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724670341677-a3953f4e-de7a-410b-becb-13f3e7c3d2ff.png)

<font style="color:#000000;">可以既声明属性名索引，也声明具体的单个属性名。如果单个属性名不符合属性名索引的范围，两者发生冲突，就会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724670422839-5648c038-ce0e-41ce-b8ed-c7284fdf5b1d.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">属性的索引类型写法，建议谨慎使用，因为属性名的声明太宽泛，约束太少。另外，属性名的数值索引不宜用来声明数组，因为采用这种方式声明数组，就不能使用各种数组方法以及length属性，因为类型里面没有定义这些东西</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724670491734-51e62868-f368-4830-abeb-42a7d9f261c5.png)

## <font style="color:#000000;">解构赋值</font>
<font style="color:#000000;">解构赋值用于直接从对象中提取属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724670596933-30bc82ba-7bb4-488c-8b66-b4829e072412.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">解构赋值的类型写法，跟为对象声明类型是一样</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724670724769-b126ffb8-a6ed-440b-ae15-dacc5d955c5d.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">注意，目前没法为解构变量指定类型，因为对象解构里面的冒号，JavaScript 指定了其他用途</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724671130446-5743a45a-030d-4fc0-afb3-6099f4bff756.png)

<font style="color:#000000;">冒号不是表示属性x和y的类型，而是为这两个属性指定新的变量名。如果要为x和y指定类型，不得不写成下面这样，</font><font style="color:#000000;">这一点要特别小心，TypeScript 里面很容易搞糊涂</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724671162592-a62e8ec6-e348-4a5d-ad93-5a0e3a662d89.png)

## <font style="color:#000000;">结构类型原则</font>
<font style="color:#000000;">只要对象 B 满足 对象 A 的结构特征，TypeScript 就认为对象 B 兼容对象 A 的类型，这称为“结构类型”原则</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724671221477-64aa229b-951b-45d6-ad67-f01d527752c6.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724671225905-e551a16c-2f2d-4b5a-a476-3815c75e5016.png)

<font style="color:#000000;">对象A只有一个属性x，类型为number。对象B满足这个特征，因此兼容对象A，只要可以使用A的地方，就可以使用B</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">根据“结构类型”原则，TypeScript 检查某个值是否符合指定类型时，并不是检查这个值的类型名（即“名义类型”），而是检查这个值的结构是否符合要求（即“结构类型”）</font>

<font style="color:#000000;">TypeScript 之所以这样设计，是为了符合 JavaScript 的行为。JavaScript 并不关心对象是否严格相似，只要某个对象具有所要求的属性，就可以正确运行</font>

<font style="color:#000000;">如果类型 B 可以赋值给类型 A，TypeScript 就认为 B 是 A 的子类型，A 是 B 的父类型。子类型满足父类型的所有结构特征，同时还具有自己的特征。凡是可以使用父类型的地方，都可以使用子类型，即子类型兼容父类型</font>

## <font style="color:#000000;">严格字面量检查</font>
<font style="color:#000000;">如果对象使用字面量表示，会触发 TypeScript 的严格字面量检查。如果字面量的结构跟类型定义的不一样（比如多出了未定义的属性），就会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724671397152-010d3325-0ee5-453e-aa98-40b95f100722.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果等号右边不是字面量，而是一个变量，根据结构类型原则，是不会报错的</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724672015178-1a3fe9ab-3b28-4b28-9ce5-bb531e999333.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">规避严格字面量检查，可以使用中间变量</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724672953929-c29f9667-a51a-4489-b129-96d25f1704f5.png)

<font style="color:#000000;">上面示例中，创建了一个中间变量myOptions，就不会触发严格字面量规则，因为这时变量obj的赋值，不属于直接字面量赋值</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">如果你确认字面量没有错误，也可以使用类型断言规避严格字面量检查</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724672992855-12e6fce4-d68e-4f14-86ab-b9358f949bf3.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">由于严格字面量检查，字面量对象传入函数必须很小心，不能有多余的属性</font>![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673026598-b2a8afcf-c7be-421f-8ff7-46b17e6e857a.png)

## <font style="color:#000000;">最小可选属性规则</font>
<font style="color:#000000;">根据“结构类型”原则，如果一个对象的所有属性都是可选的，那么其他对象跟它都是结构类似的</font>

<font style="color:#000000;">为了避免这种情况，TypeScript 2.4 引入了一个“最小可选属性规则”，也称为“弱类型检测”</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673437744-871ca757-b373-4380-a647-a49c4bce1274.png)

<font style="color:#000000;">对象opts与类型Options没有共同属性，赋值给该类型的变量就会报错</font>

<font style="color:#000000;">报错原因是，如果某个类型的所有属性都是可选的，那么该类型的对象必须至少存</font>

<font style="color:#000000;">在一个可选属性，不能所有可选属性都不存在。这就叫做“最小可选属性规则”</font>

## <font style="color:#000000;">空对象</font>
<font style="color:#000000;">空对象是 TypeScript 的一种特殊值，也是一种特殊类型</font>

<font style="color:#000000;">空对象没有自定义属性，所以对自定义属性赋值就会报错。空对象只能使用继承的属性，即继承自原型对象Object.prototype的属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673557196-a05b1809-afe0-45b4-8877-3ba995579931.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">TypeScript 不允许动态添加属性，所以对象不能分步生成，必须生成时一次性声明所有属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673749138-8574a6d5-4f57-4d80-9f7d-05690fea1192.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">如果确实需要分步声明，一个比较好的方法是，使用扩展运算符（...）合成一个新对象</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673765575-8501a6df-6baa-4ceb-9392-43387592c106.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">空对象作为类型，其实是Object类型的简写形式</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673808946-1ee21728-2a3a-48cc-ab0c-65bf50ee9efa.png)

<font style="color:#000000;">各种类型的值（除了null和undefined）都可以赋值给空对象类型，跟Object类型的行为是一样的</font>

<font style="color:#000000;">因为Object可以接受各种类型的值，而空对象是Object类型的简写，所以它不会有严格字面量检查，赋值时总是允许多余的属性，只是不能读取这些属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673884709-c489722b-4d50-48ff-9e6c-5e2847b48b21.png)<font style="color:#000000;">、</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">如果想强制使用没有任何属性的对象，可以采用下面的写法</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724673999969-be66a864-b420-4eda-9f19-0040f320b527.png)<font style="color:#000000;"> </font>

<font style="color:#000000;">[key: string]: never表示字符串属性名是不存在的，因此其他对象进行赋值时就会报错</font>

# <font style="color:#000000;">interface -</font>
## <font style="color:#000000;">简介</font>
<font style="color:#000000;">interface 是对象的模板，可以看作是一种类型约定</font><font style="color:#000000;">，中文译为“接口”</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724721626335-4554b455-8919-4c8f-b814-36c9eeab9c6e.png)

<font style="color:#000000;">定义了一个接口Person，它指定一个对象模板，拥有三个属性firstName、lastName和age。任何实现这个接口的对象，都必须部署这三个属性，并且必须符合规定的类型</font>

<font style="color:#000000;">实现该接口很简单，只要指定它作为对象的类型即可</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724724960906-362829bc-c03a-4934-968b-f534e7f8c3ad.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">方括号运算符可以取出 interface 某个属性的类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725066923-803ff83b-a890-417c-a7f7-4a2a68cd236c.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">interface 可以表示对象的各种语法，它的成员有5种形式</font>

+ <font style="color:#000000;">对象属性</font>
+ <font style="color:#000000;">对象的属性索引</font>
+ <font style="color:#000000;">对象方法</font>
+ <font style="color:#000000;">函数</font>
+ <font style="color:#000000;">构造函数</font>

### <font style="color:#000000;">对象属性</font>
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725305381-8437104a-ec4b-40af-978f-385cf805c0a5.png)

<font style="color:#000000;">如果属性是可选的，就在属性名后面加一个问号</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725314891-c6c2dc7b-0a4d-414f-9c96-623a397ea897.png)

<font style="color:#000000;">如果属性是只读的，需要加上readonly修饰符</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725331711-296a7cfe-b7a5-426d-8cab-1bbc208b8776.png)

### <font style="color:#000000;">对象的属性索引</font>
<font style="color:#000000;">属性索引共有string、number和symbol三种类型，指的是 prop:xxx</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725370097-3b81e4bf-6f77-45b8-b126-f296b419dc34.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">一个接口中，最多只能定义一个字符串索引。字符串索引会约束该类型中所有名字为字符串的属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725405040-ff4c79c1-584d-4dcb-896e-60e0fa258bf1.png)

<font style="color:#000000;">属性索引指定所有名称为字符串的属性，它们的属性值必须是数值（number）。属性a的值为布尔值就报错了</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">属性的数值(number)索引，其实是指定数组的类型</font>

<font style="color:#000000;">一个接口中最多只能定义一个数值索引，数值索引会约束所有名称为数值的属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725602825-bda67423-2d90-4300-8923-b8b2792df808.png)

<font style="color:#000000;">在 TypeScript 中，数组是特殊类型的对象。数组的键是 number 类型的，具体来说，它们是从 0 开始的连续数字索引。因此，数组可以视作一个由 number 键索引的对象。这使得数组可以匹配以 number 为键的索引签名类型</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">如果一个 interface 同时定义了字符串索引和数值索引，那么数值索引必须服从于字</font>

<font style="color:#000000;">符串索引，因为在 JavaScript 中，数值属性名最终是自动转换成字符串属性名</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725821776-42a393e3-9073-4d2d-bad6-76c288037479.png)

### <font style="color:#000000;">对象方法</font>
<font style="color:#000000;">对象的方法共有三种写法</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725865092-a1d622b1-9fde-499c-981c-9f231620838f.png)

### <font style="color:#000000;">函数</font>
<font style="color:#000000;">interface 也可以用来声明独立的函数</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725924633-893f8581-e403-480f-aabc-9e0406504088.png)

### <font style="color:#000000;">构造函数</font>
<font style="color:#000000;">interface 内部可以使用new关键字，表示构造函数</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724725976468-c5f1ad85-c97d-4e83-89d6-ed80fbbb8e5a.png)

## <font style="color:#000000;">interface 的继承</font>
<font style="color:#000000;">interface 可以继承其他类型，主要有下面几种情况</font>

### <font style="color:#000000;">interface 继承 interface</font>
<font style="color:#000000;">interface 可以使用extends关键字，继承其他 interface</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726029634-df4b295c-9356-42b4-aca1-526b2091a132.png)

+ <font style="color:#000000;">Circle继承了Shape，所以Circle其实有两个属性name和radius</font>
+ <font style="color:#000000;">extends关键字会从继承的接口里面拷贝属性类型，这样就不必书写重复的属性</font>
+ <font style="color:#000000;">Circle是子接口，Shape是父接口</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">interface 允许多重继承，多重接口继承</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726100075-23dd264f-1ceb-46fe-a1a4-4f6a53fd4fac.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">实际上相当于多个父接口的合并，如果子接口与父接口存在同名属性，那么子接口的属性会覆盖父接口的属性</font>

<font style="color:#000000;">注意，子接口与父接口的同名属性必须是类型兼容的，不能有冲突，否则会报错</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726237150-8731ed63-7233-4aec-99bd-3bae51372ba3.png)



多重继承时，如果多个父接口存在同名属性，那么这些同名属性不能有类型冲突，否则会报错

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726270427-a3b1604b-327e-4d3a-93b7-b04da3bcb948.png)

### <font style="color:#000000;">interface 继承 type</font>
<font style="color:#000000;">interface 可以继承type命令定义的对象类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726305554-0a71e759-aff6-4c67-9860-7d7e34269c69.png)

CountryWithPop继承了type命令定义的Country对象，并且新增了一个population属性

注意，如果type命令定义的类型不是对象，interface 就无法继承

### <font style="color:#000000;">interface继承class</font>
<font style="color:#000000;">interface 还可以继承 class，即继承该类的所有成员。关于 class 的详细解释，参见下一章</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726467670-7341ec15-8e90-4fda-9a32-29c99ae6c8cb.png)

B继承了A，因此B就具有属性x、y()和z，实现B接口的对象就需要实现这些属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726539509-6c211a80-1c98-46b0-b0d3-fbd058397e00.png)

## <font style="color:#000000;">接口合并</font>
<font style="color:#000000;">多个同名接口会合并成一个接口</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726587656-dd3116f7-c175-45f8-9f3c-8771ce9032b5.png)

两个Box接口会合并成一个接口，同时有height、width和length三个属性



这样的设计主要是为了兼容 JavaScript 的行为。JavaScript 开发者常常对全局对象或者外部库，添加自己的属性和方法。那么，只要使用 <font style="color:#000000;"></font>interface 给出这些自定义属性和方法的类型，就能自动跟原始的 interface 合并，使得扩展外部类型非常方便



<font style="color:#000000;">同名接口合并时，同一个属性如果有多个类型声明，彼此不能有类型冲突</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726673406-21a526cc-1e6a-4e77-8ff4-a9142a7b39cf.png)



<font style="color:rgb(74, 74, 74);">同名接口合并时，如果同名方法有不同的类型声明，那么会发生函数重载。而且，后面的定义比前面的定义具有更高的优先级</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726720457-6d708a4e-1e87-4be9-aebf-7a473d5dc921.png)

clone()方法有不同的类型声明，会发生函数重载。这时，越靠后的定义，优先级越高，排在函数重载的越前面。比如，clone(animal: Animal)是最先出现的类型声明，就排在函数重载的最后，属于clone()函数最后匹配的类型



这个规则有一个例外。同名方法之中，如果有一个参数是字面量类型，字面量类型有更高的优先级

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726766063-a569e6cd-2d63-4ef2-874a-b6b8584a00c0.png)



如果两个 interface 组成的联合类型存在同名属性，那么该属性的类型也是联合类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726792626-2908a709-3a96-4556-88c2-006e6ff7b4fa.png)

## <font style="color:#000000;">interface与type的异同</font>
<font style="color:#000000;">interface命令与type命令作用类似，都可以表示对象类型。</font>

<font style="color:#000000;">它们的相似之处，首先表现在都能为对象类型起名</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">interface 与 type 的区别有下面几点</font>

+ <font style="color:#000000;">type能够表示非对象类型，而interface只能表示对象类型（包括数组、函数等）</font>
+ <font style="color:#000000;">interface可以继承其他类型，type不支持继承</font>
+ <font style="color:#000000;">同名interface会自动合并，同名type则会报错</font>
+ <font style="color:#000000;">interface不能包含属性映射，type可以，详见《映射》一章</font>
+ <font style="color:#000000;">this关键字只能用于interface</font>
+ <font style="color:#000000;">type 可以扩展原始数据类型，interface 不行</font>
+ <font style="color:#000000;">interface无法表达某些复杂类型（比如交叉类型和联合类型），但是type可以</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">继承的主要作用是添加属性，type定义的对象类型如果想要添加属性，只能使用&运算符，重新定义一个类型</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724727005350-fbe58702-c37f-4d0f-82a2-d715e7b6ffbd.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724726979851-231187df-b787-4173-b5a7-7be08619260e.png)

interface 可以继承 type，<font style="color:rgb(74, 74, 74);">type 可以添加 interface 的属性，算是变相的继承</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724727051297-29e3e7b7-aa4c-48e3-817c-d4396cfc67da.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724727062546-e7f9f9ab-f50c-4a52-ae4e-e2805a930d76.png)



如果有复杂的类型运算，那么没有其他选择只能使用type；一般情况下，interface灵活性比较高，便于扩充类型或自动合并，建议优先使用

# class -
## 简介
### 属性的类型
类的属性可以在顶层声明，也可以在构造方法内部声明

对于顶层声明的属性，可以在声明时同时给出类型，如果不给出类型，TypeScript 会认为x和y的类型都是any

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724745610266-45f06f27-2dd3-4085-8729-90dcf8ad71a2.png)

如果声明时给出初值，可以不写类型，TypeScript 会自行推断属性的类型



如果不希望出现报错，可以使用非空断言

属性x和y没有初值，但是属性名后面添加了感叹号，表示这两个属性肯定不会为空，所以 TypeScript 就不报错了

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724745928278-5b37e912-150e-47a7-b9df-ccf7a4fa0f2c.png)

### readonly 修饰符
属性名前面加上 readonly 修饰符，就表示该属性是只读的，实例对象不能修改这个属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746027052-d6723ad2-9cd4-480b-86a9-2be31a65ab4c.png)



<font style="color:rgb(74, 74, 74);">readonly 属性的初始值，可以写在顶层属性，也可以写在构造方法里面</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746063726-032af6af-4493-410e-a845-dd2e15cea2ed.png)

### 方法的类型
<font style="color:rgb(74, 74, 74);">类的方法就是普通函数，类型声明方式与函数一致</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746216324-1360b70b-38fb-4bc0-8c60-930a1b5465dc.png)

构造方法constructor()和普通方法add()都注明了参数类型，但是省略了返回值类型，因为 TypeScript 可以自己推断出来



类的方法跟普通函数一样，可以使用参数默认值，以及函数重载

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746293158-100e3ecb-39cc-436f-a45e-3d0afe29592c.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746302916-352403f4-7424-46ec-b3ac-c2e99f568dfa.png)

### 存取器方法
存取器是特殊的类方法，包括取值器（getter）和存值器（setter）两种方法，<font style="color:rgb(74, 74, 74);">它们用于读写某个属性，取值器用来读取属性，存值器用来写入属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746397242-91b9cced-261c-4cbd-873c-5dfc2fe64166.png)

get name()是取值器，其中get是关键词，name是属性名。外部读取name属性时，实例对象会自动调用这个方法，该方法的返回值就是name属性的值

set name()是存值器，其中set是关键词，name是属性名。外部写入name属性时，实例对象会自动调用这个方法，并将所赋的值作为函数参数传入

TypeScript 对存取器有以下规则

+ 如果某个属性只有get方法，没有set方法，那么该属性自动成为只读属性
+ TypeScript 5.1 版之前，set方法的参数类型，必须兼容get方法的返回值类型，否则报错
+ get方法与set方法的可访问性必须一致，要么都为公开方法，要么都为私有方法

### 属性索引
类允许定义属性索引

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746700587-bb424559-0cec-4b1e-b6ca-91c34d42c2ec.png)

[s:string]表示所有属性名类型为字符串的属性，它们的属性值要么是布尔值，要么是返回布尔值的函数



注意，由于类的方法是一种特殊属性（属性值为函数的属性），所以属性索引的类型定义也涵盖了方法。如果一个对象同时定义了属性索引和方法，那么前者必须包含后者的类型

属性索引的类型里面不包括方法，导致后面的方法f()定义直接报错。正确的写法是右边这样

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746851952-78e89a9a-2222-4a7a-a21c-8d6caa7833eb.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724746872112-2627aa1b-8c2e-4153-a3ce-6eb12839757c.png)



属性存取器视同属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747033101-077a0d6c-6a31-43c9-a1e3-2bfc3b2b890c.png)

属性inInstance的读取器虽然是一个函数方法，但是视同属性，所以属性索引虽然没有涉及方法类型，但是不会报错

## 类的 interface 接口
### implements 关键字
interface 接口或 type 别名，可以用对象的形式，为 class 指定一组检查条件。然后，类使用 implements 关键字，表示当前类满足这些外部类型条件的限制

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747160244-6cf8043b-4401-461a-a8bc-49f1c979f434.png)



interface 只是指定检查条件，如果不满足这些条件就会报错。它并不能代替 class 自身的类型声明

类B实现了接口A，但是后者并不能代替B的类型声明。因此，B的get()方法的参数s的类型是any，而不是string，B类依然需要声明参数s的类型，如右图

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747214641-b416f490-448d-415d-9534-35a95227f258.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747401058-72441569-5d31-49df-b66e-13c46b40be33.png)



<font style="color:rgb(74, 74, 74);">类可以定义接口没有声明的方法和属性</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747765354-f1d0ed60-f569-49bb-ae3a-bee6a352dbb8.png)



implements关键字后面，不仅可以是接口，也可以是另一个类。这时，后面的类将被当作接口

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747875675-5b9eda29-29d7-4f25-b754-2e55627f07d0.png)

implements后面是类Car，这时 TypeScript 就把Car视为一个接口，要求MyCar实现Car里面的每一个属性和方法，否则就会报错。所以，这时不能因为Car类已经实现过一次，而在MyCar类省略属性或方法



注意，interface 描述的是类的对外接口，也就是实例的公开属性和公开方法，不能定义私有的属性和方法。这是因为 TypeScript 设计者认为，私有属性是类的内部实现，接口作为模板，不应该涉及类的内部代码写法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747940172-a8a01ee7-bcc4-40c3-af81-d17b56385fba.png)

### 实现多个接口
类可以实现多个接口（其实是接受多重限制），每个接口之间使用逗号分隔

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724747957432-800e9ad3-88d6-441a-a526-d1fa17303fad.png)

Car类同时实现了MotorVehicle、Flyable、Swimmable三个接口。这意味着，它必须部署这三个接口声明的所有属性和方法，满足它们的所有条件



<font style="color:rgb(74, 74, 74);">但是，同时实现多个接口并不是一个好的写法，容易使得代码难以管理，可以使用两种方法替代</font>

<font style="color:rgb(74, 74, 74);">第一种方法是类的继承</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724748060485-e0d7eaac-219f-4393-8fd3-2cf1aa01b270.png)

Car类实现了MotorVehicle，而SecretCar类继承了Car类，然后再实现Flyable和Swimmable两个接口，相当于SecretCar类同时实现了三个接口

第二种方法是接口的继承

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724748124771-17528280-4bf5-42b4-a830-ab82986785ef.png)

接口B继承了接口A，类只要实现接口B，就相当于实现A和B两个接口



发生多重实现时（即一个接口同时实现多个接口），不同接口不能有互相冲突的属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724749310297-d0fde164-f653-4c51-bcdc-be411fc33441.png)

属性foo在两个接口里面的类型不同，如果同时实现这两个接口，就会报错

### 类与接口的合并
TypeScript 不允许两个同名的类，但是如果一个类和一个接口同名，那么接口会被合并进类

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724749400020-fcd62049-120c-47fc-919e-f16b8348da08.png)

注意，合并进类的非空属性（上例的y），如果在赋值之前读取，会返回undefined

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724749545712-2b933cce-8323-43b7-b4d0-a181e4c37f07.png)

## Class 类型
### 实例类型
TypeScript 的类本身就是一种类型，但是它代表该类的实例类型，而不是 class 的自身类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724750111179-872d7299-2cfc-4060-93ce-a9860e7e6447.png)

定义了一个类Color。它的类名就代表一种类型，实例对象green就属于该类型



对于引用实例对象的变量来说，既可以声明类型为 Class，也可以声明类型为 Interface，因为两者都代表实例对象的类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1724750374777-e9167c0b-f809-4768-8212-f476e93d058e.png)

变量的类型可以写成类Car，也可以写成接口MotorVehicle。它们的区别是，如果类Car有接口MotorVehicle没有的属性和方法，那么只有变量c1可以调用这些属性和方法

### 类的自身类型


### 结构类型原则


## 类的继承


## override 关键字


## 可访问性修饰符


### public


### private


### protected


### 实例属性的简写形式


## 顶层属性的处理方法


## 静态成员


## 泛型类


## 抽象类，抽象成员


## this 问题


## 参考链接


