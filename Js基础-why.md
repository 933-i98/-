# 前言
学习到 react 时，发现自己 js 基础不好，遂重新复习一下 js，之前学的是 pink 的课，现在重新看 why 的 js 基础 ppt 进行重修

该笔记仅用来补充黑马课程中未涉及到的部分

# <noscript>
针对早期浏览器不支持 JavaScript 的问题

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714131367843-e97d0cea-81f0-4748-b03f-c17b915e83d3.png)

# 内置类
## Number 类的补充
Number实例方法补充：

+ 方法一：toString(base)，将数字转成指定进制的字符串
    - base 的范围可以从 2 到 36，默认情况下是 10
    - 注意：如果是直接对一个数字操作，需要使用...运算符

```javascript
let a = 10;
let binaryString = a.toString(2); // 将数字转换为二进制字符串
console.log(binaryString); // 输出 "1010"

let hexString = a.toString(16); // 将数字转换为十六进制字符串
console.log(hexString); // 输出 "a"
```

+ 方法二：toFixed(digits)，格式化一个数字，保留digits位的小数；
    - digits的范围是0到20（包含）之间；

```javascript
let a = 3.14159265359;
let roundedNumber = a.toFixed(2); // 保留两位小数
console.log(roundedNumber); // 输出 "3.14"
```

Number类方法补充：

+ 方法一：Number.parseInt(string[, radix])，将字符串解析成整数，也有对应的全局方法parseInt
+ 方法二：Number. parseFloat(string)，将字符串解析成浮点数，也有对应的全局方法parseFloat

```javascript
const num1 = Number.parseInt("10", 10); // 输出 10
const num2 = Number.parseFloat("10.5"); // 输出 10.5
```

## Math
+ Math.floor：向下舍入取整
+ Math.ceil：向上舍入取整
+ Math.round：四舍五入取整
+ Math.random：生成0~1的随机数（包含0，不包含1）
+ Math.pow(x, y)：返回x的y次幂 

```javascript
const num = 5.8;
console.log(Math.floor(num)); // 输出 5

const num = 5.1;
console.log(Math.ceil(num)); // 输出 6

const num1 = 5.4;
const num2 = 5.6;
console.log(Math.round(num1)); // 输出 5
console.log(Math.round(num2)); // 输出 6

const randomNum = Math.random();
console.log(randomNum); // 输出一个介于 0（包含）和 1（不包含）之间的随机数

const result = Math.pow(2, 3); // 计算 2 的 3 次幂
console.log(result); // 输出 8
```

## String
### 访问字符串的字符
+ 方法一：通过字符串的索引 str[0]
+ 方法二：通过str.charAt(pos)方法
+ 它们的区别是索引的方式没有找到会返回undefined，而charAt没有找到会返回空字符串

```javascript
const str = "Hello, world!";
console.log(str.charAt(0)); // 输出 "H"
console.log(str.charAt(7)); // 输出 "w"
console.log(str.charAt(100)); // 输出 ""
```

### 字符串的遍历
+ 方式一：普通for循环
+ 方式二：for..of遍历

```javascript
const str = "Hello";
for (let i = 0; i < str.length; i++) {
    console.log(str[i]);
}

const str = "Hello";
for (const char of str) {
    console.log(char);
}
```

### 字符串的不可变性
字符串在定义后是不可以修改的，所以下面的操作是没有任何意义的  

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714133811141-6eccfe72-a6df-4444-b57f-51c61a7e63d3.png)

### 查找、包含
+ 查找字符串位置：`str.indexof(searchValue[,fromIndex])`，从fromIndex开始，查找searchValue的索引，如果没有找到，那么返回-1。有一个相似的方法，叫lastIndexOf，从最后开始查找（用的较少）
+ 是否包含字符串：`str.includes(searchstring[,position])`从position位置开始查找searchString， 根据情况返回 true 或 false，这是ES6新增的方法

```javascript
const str = "Hello,world!";
console.log(str.indexOf("world")); // 输出 6
console.log(str.indexOf("foo")); // 输出 -1（未找到）
console.log(str.indexOf("o", 5)); // 从索引 5 开始查找，输出 7（第二个 "o" 的位置）

const str = "Hello,world!";
console.log(str.includes("world")); // 输出 true
console.log(str.includes("foo")); // 输出 false
console.log(str.includes("o", 5)); // 从索引 5 开始检查，输出 true
```

### 开头、结尾、替换
+ 以xxx开头：`str.startsWith(searchstring[,position])`从position位置开始，判断字符串是否以searchString开头，这是ES6新增的方法，下面的方法也是
+ 以xxx结尾：`str.endsWith(searchString[,length])`在length长度内，判断字符串是否以searchString结尾
+ 替换字符串：`str.replace(regexp|substr,newSubstr|function)`查找到对应的字符串，并且使用新的字符串进行替代，这里也可以传入一个正则表达式来查找，也可以传入一个函数来替换

```javascript
const str = "Hello, world!";
console.log(str.startsWith("Hello")); // 输出 true
console.log(str.startsWith("world")); // 输出 false

const str = "Hello, world!";
console.log(str.endsWith("world!")); // 输出 true
console.log(str.endsWith("Hello")); // 输出 false

const str = "Hello, world!";
console.log(str.replace("world", "everyone")); // 输出 "Hello, everyone!"
console.log(str.replace(/o/g, "*")); // 将所有的 "o" 替换为 "*"，输出 "Hell*, w*rld!"
```

### 获取子字符串
+ slice(start, end) ：从 start 到 end（不含 end，end 可选）；如果省略 end 则提取到字符串末尾；允许负值参数，如果是负数，就倒着数；start 大于 end 时返回空字符串；
+ substring(start, end) ：从 start 到 end（不含 end，end 可选），负值会被当作 0，如果省略 end 则提取到字符串末尾；start 大于 end，会自动交换它们
+ substr(start, length) ：从 start 开始获取长为 length 的字符串，，length 可选，允许 start 为负数

```javascript
let str = "Hello, world!";
console.log(str.slice(0, 5)); // 输出 "Hello"
console.log(str.slice(-6)); // 输出 "world!"
console.log(str.slice(7)); // 输出 "world!"，w是7,因为w前还有空格

console.log(str.substring(0, 5)); // 输出 "Hello"
console.log(str.substring(7)); // 输出 "world!"
console.log(str.substring(7, 5)); // 实际上会被转换为 substring(5, 7)，输出 "， "

let str = "Hello, world!";
console.log(str.substr(7)); // 输出 "world!"
console.log(str.substr(-6)); // 输出 "world!"
console.log(str.substr(7, 3)); // 输出 "wor"
```

### 其他方法
+ 拼接字符串 `str.concat(str2,[，...strN])`![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714134701166-2d851201-8957-4be2-ac42-312de4ef047c.png)
+ 删除首位空格 `str.trim()`![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714134707474-6f92be56-accd-4924-841f-d1b81966f13f.png)
+ 字符串分割 `str.split([separator[,limit]])`
    - separator：以什么字符串进行分割，也可以是一个正则表达式；
    - limit：限制返回片段的数量![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714134726003-4be899b0-3a54-4246-9b6c-b92a915d0056.png)

## Array
### 创建
两种方式

```javascript
var arr1 = ['aaa','bbb']
var arr2 = new Array('aaa','bbb')
var arr3 = new Array(5)//空数组，长度为5
```

### 访问
两种方式

```javascript
arr[1]
arr.at(2)
arr.at(-1)
//arr.at(i)：
//如果 i >= 0，则与 arr[i] 完全相同。
//对于 i 为负数的情况，它则从数组的尾部向前数 
```

### 添加与删除
push 在末端添加元素

pop 从末端取出一个元素

unshift 在首端添加元素，整个其他数组元素向后移动

shift 取出队列首端的一个元素，整个数组元素向前前移动

push/pop 方法运行的比较快，而 shift/unshift 比较慢

```javascript
arr.push('ccc')
arr.pop()

arr.unshift('ddd')
arr.shift()
```

arr.splice 方法可以说是处理数组的利器，它可以做所有事情：添加，删除和替换元素

`array.splice(start[,deleteCount[,iteml[,item2[,...]]]])`

+  这个方法会修改原数组
+ start：指定修改的起始位置（索引）;可以是负数，表示从数组末尾开始的位置
+ deleteCount：可选，表示要删除的元素个数。如果为 0 或者省略，则不删除元素，仅插入新元素
+ item1, item2, ...: 可选，要添加到数组的新元素。从 start 位置开始插入。如果没有指定新元素，则仅删除元素

```javascript
const array = [1, 2, 3, 4, 5];
// 在索引 2 处插入新元素
array.splice(2, 0, 'a', 'b'); // array 变为 [1, 2, 'a', 'b', 3, 4, 5]

// 删除索引 3 开始的两个元素
array.splice(3, 2); // array 变为 [1, 2, 'a', 4, 5]

// 替换索引 1 处的元素(删除索引为1，添加三个字母)
array.splice(1, 1, 'x', 'y', 'z'); // array 变为 [1, 'x', 'y', 'z', 'a', 4, 5]

// 从索引 -2 开始删除所有元素
array.splice(-2); // array 变为空数组 []
```

### length
length属性用于获取数组的长度，当我们修改数组的时候，length 属性会自动更新。

如果手动增加一个大于默认length的数值，那么会增加数组的长度，但是如果减少它，数组就会被截断。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714135484371-09a246e8-9041-42a8-afc5-8f87d9ef9b23.png) 

清空数组最简单的方法就是：arr.length = 0

### 遍历
4 种方法

+ for
+ for...in
    - 虽然 for...in 循环也可以用于遍历数组，但通常不推荐这样做，因为它不仅会遍历数组的元素，还会遍历数组的所有可枚举属性，包括原型链上的属性。这可能会导致意外的行为，特别是当你使用了自定义的数组方法或者数组原型上的方法时
+ for...of

```javascript
const array = [1, 2, 3, 4, 5];
for (let i = 0; i < array.length; i++) {
    console.log(array[i]);
}

const array = [1, 2, 3, 4, 5];
for (const index in array) {
    console.log(array[index]);
}

const array = [1, 2, 3, 4, 5];
for (const element of array) {
    console.log(element);
}
```

### 分割、重组、拼接
arr.slice 方法：用于对数组进行截取（类似于字符串的slice方法），包含 start 元素，但是不包含end元素（即左闭右开），返回一个新数组

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714135728651-3a24c99f-c70c-4b29-a8d2-9cb27c5ea830.png)

arr.concat方法：创建一个新数组，其中包含来自于其他数组和其他项的值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1714135733653-ac47641e-c960-4b80-bf98-f8e5bd170439.png)

arr.join方法：将一个数组的所有元素连接成一个字符串并返回这个字符串

```javascript
const array = ['apple', 'banana', 'orange'];
const result = array.join(', '); // 使用逗号和空格作为分隔符
console.log(result); // 输出: "apple, banana, orange"

const result2 = array.join(' - '); // 使用短横线和空格作为分隔符
console.log(result2); // 输出: "apple - banana - orange"
```

### 查找元素
arr.indexOf(item, start)

+ 该方法返回数组中**第一次**出现指定元素的索引，如果未找到则返回-1
+ 参数item为要查找的元素，start为搜索的起始位置（可选，默认为0）

```javascript
const arr = [1, 2, 3, 4, 5];
console.log(arr.indexOf(3)); // 输出: 2
console.log(arr.indexOf(6)); // 输出: -1
```

arr.includes(item, start)

+ 该方法用于检测数组中是否包含某个特定的元素，存在则返回true，否则返回false
+ 参数item为要查找的元素，start为搜索的起始位置（可选，默认为0）

```javascript
const arr = [1, 2, 3, 4, 5];
console.log(arr.includes(3)); // 输出: true
console.log(arr.includes(6)); // 输出: false
```

arr.find(callback(element, index, array))

+ 该方法返回数组中**第一个**满足提供的测试函数的元素值，如果未找到则返回undefined
+ 参数callback为用来测试每个元素的函数，它包含三个参数：element（当前元素的值）、index（当前元素的索引）、array（数组本身）

```javascript
const arr = [1, 2, 3, 4, 5];
const found = arr.find(element => element > 3);
console.log(found); // 输出: 4
```

arr.findIndex(callback(element, index, array))

+ 该方法返回数组中第一个满足提供的测试函数的元素的索引，如果未找到则返回-1
+ 参数callback与arr.find相同，用来测试每个元素的函数

```javascript
const arr = [1, 2, 3, 4, 5];
const foundIndex = arr.findIndex(element => element > 3);
console.log(foundIndex); // 输出: 3
```

### 排序
sort()

+ 用于对数组的元素进行排序，并**返回排序后的数组**
+ 默认情况下，sort() 方法会将数组元素转换为字符串，然后按照字符顺序进行排序
+ 如果不传入任何参数，sort() 方法会按照字符编码的顺序进行排序
+ 如果需要按照其他方式排序，则可以传入一个比较函数作为参数，比较函数应返回负值、零或正值以确定元素的顺序

```javascript
const arr = [3, 1, 5, 2, 4];
arr.sort(); // 默认按字符编码排序
console.log(arr); // 输出: [1, 2, 3, 4, 5]

// 按数字大小升序排序
arr.sort((a, b) => a - b);
console.log(arr); // 输出: [1, 2, 3, 4, 5]

// 按数字大小降序排序
arr.sort((a, b) => b - a);
console.log(arr); // 输出: [5, 4, 3, 2, 1]
```

reverse()

+ reverse() 方法用于颠倒数组中元素的顺序，原先排在前面的元素现在排在后面，原先排在后面的元素现在排在前面
+ reverse() 方法会**修改原始数组**，而不是返回一个新的数组

```javascript
const arr = [1, 2, 3, 4, 5];
arr.reverse();
console.log(arr); // 输出: [5, 4, 3, 2, 1]
```

### 其他高阶方法
arr.forEach(callback(element, index, array))

+ forEach() 方法对数组的每个元素执行一次提供的函数。该方法没有返回值
+ 回调函数接收三个参数：当前元素 (element)、当前元素的索引 (index)、被遍历的数组 (array)

```javascript
const arr = [1, 2, 3, 4, 5];
arr.forEach((element, index) => {
  console.log(`Index ${index}: ${element}`);
});
// 输出:
// Index 0: 1
// Index 1: 2
// Index 2: 3
// Index 3: 4
// Index 4: 5
```

arr.map(callback(element, index, array))

+ map() 方法**创建一个新数组**，其结果是该数组中的每个元素都调用一次提供的函数后的返回值
+ 回调函数接收三个参数：当前元素 (element)、当前元素的索引 (index)、被遍历的数组 (array)

```javascript
const arr = [1, 2, 3, 4, 5];
const squaredArr = arr.map(element => element * element);
console.log(squaredArr); // 输出: [1, 4, 9, 16, 25]

```

arr.filter(callback(element, index, array))

+ filter() 方法**创建一个新数组**，其中包含所有通过所提供函数实现的测试的元素
+ 回调函数接收三个参数：当前元素 (element)、当前元素的索引 (index)、被遍历的数组 (array)

```javascript
const arr = [1, 2, 3, 4, 5];
const evenArr = arr.filter(element => element % 2 === 0);
console.log(evenArr); // 输出: [2, 4]
```

arr.reduce(callback(accumulator, element, index, array), initialValue)

+ reduce() 方法对数组中的每个元素执行一个由您提供的 reducer 函数(升序执行)，将其结果汇总为单个返回值
+ 回调函数接收四个参数：累积器 (accumulator)、当前元素 (element)、当前元素的索引 (index)、被遍历的数组 (array)
+ accumulator 是累积器，它存储的是先前的计算结果。在第一次调用时，它是 initialValue 的值
+ 该方法可以接受一个初始值 (initialValue)，如果提供了初始值，则从初始值开始，否则从数组的第一个元素开始

```javascript
const arr = [1, 2, 3, 4, 5];
const sum = arr.reduce((accumulator, element) => accumulator + element, 0);
console.log(sum); // 输出: 15
```

## Date
### 创建 Date 对象
```javascript
// 输出当前的完整日期和时间
const now = new Date();
console.log(now); 
// 通过年、月、日创建日期
const specificDate = new Date(2023, 0, 1); 
console.log(specificDate);// 2023年1月1日

// 通过年、月、日、时、分、秒创建日期和时间
const specificDateTime = new Date(2023, 0, 1, 12, 30, 0); 
console.log(specificDateTime);// 2023年1月1日 12:30:00

// 通过字符串创建日期，注意日期格式
const dateString = '2023-01-01T12:30:00';
const dateFromString = new Date(dateString);
console.log(dateFromString);
```

### dateString时间的表示方式
日期的表示方式有两种：RFC 2822 标准 或者 ISO 8601 标准

RFC 2822：`Day, DD Mon YYYY HH:mm:ss ZZ`，例如`Mon, 01 Jan 2023 12:30:00 -0800`

+ Day: 星期几的缩写名称，例如 Mon 表示星期一
+ DD: 日期，以两位数表示，例如 01 表示第一天
+ Mon: 月份的缩写名称，例如 Jan 表示一月
+ YYYY: 年份，四位数表示，例如 2023
+ HH:mm:ss: 时间，小时、分钟、秒的表示，例如 12:30:00
+ ZZ: 时区偏移量，例如 -0800 表示UTC偏移8小时



ISO 8601：`YYYY-MM-DDTHH:mm:ssZ`，例如 "2023-01-01T12:30:00Z"

+ YYYY：年份，0000 ~ 9999
+ MM：月份，01 ~ 12
+ DD：日，01 ~ 31
+ T：分隔日期和时间，没有特殊含义，可以省略
+ HH：小时，00 ~ 24
+ mm：分钟，00 ~ 59
+ ss：秒，00 ~ 59
+ .sss：毫秒
+ Z：时区

###  获取信息的方法
+ getFullYear()：获取年份（4 位数）
+ getMonth()：获取月份，从 0 到 11
+ getDate()：获取具体日期，从 1 到 31（方法名字有点迷）
+ getHours()：获取小时
+ getMinutes()：获取分钟
+ getSeconds()：获取秒钟
+ getMilliseconds()：获取毫秒
+ getDay()：获取星期几，从 0（星期日）到 6（星期六）

```javascript
const date = new Date(); 
// 当前时间对象，如 Tue Jun 25 2024 08:27:40 GMT+0000 (UTC)
const year = date.getFullYear(); // 2024
const month = date.getMonth() + 1; // 6 （月份从0开始，所以加1）
const day = date.getDate(); // 25
const hours = date.getHours(); // 8
const minutes = date.getMinutes(); // 27
const seconds = date.getSeconds(); // 40
const milliseconds = date.getMilliseconds(); // 123
const weekDay = date.getDay(); // 2 （0是星期日, 所以2是星期二）
```

### 设置信息的方法
+ setFullYear(year, [month], [date])
+ setMonth(month, [date])
+ setDate(date)
+ setHours(hour, [min], [sec], [ms])
+ setMinutes(min, [sec], [ms])
+ setSeconds(sec, [ms])
+ setMilliseconds(ms)
+ setTime(milliseconds)

```javascript
// 创建一个新的 Date 对象，当前时间
let date = new Date();

// 输出当前时间
console.log("当前时间:");
console.log(date.toString());

// 设置年份为 2023 年
date.setFullYear(2023);
console.log("\n设置年份为 2023 年后的时间:");
console.log(date.toString());

// 设置月份为 11 月（12 月，因为从 0 开始计数）
date.setMonth(11);
console.log("\n设置月份为 12 月后的时间:");
console.log(date.toString());

// 设置日期为 31 号
date.setDate(31);
console.log("\n设置日期为 31 号后的时间:");
console.log(date.toString());

// 设置小时为 10 点
date.setHours(10);
console.log("\n设置小时为 10 点后的时间:");
console.log(date.toString());

// 设置分钟为 30 分
date.setMinutes(30);
console.log("\n设置分钟为 30 分后的时间:");
console.log(date.toString());

// 设置秒钟为 45 秒
date.setSeconds(45);
console.log("\n设置秒钟为 45 秒后的时间:");
console.log(date.toString());

// 设置毫秒为 500 毫秒
date.setMilliseconds(500);
console.log("\n设置毫秒为 500 毫秒后的时间:");
console.log(date.toString());

// 设置时间为 5 秒后的时间（相对于当前时间）
date.setTime(date.getTime() + 5000);
console.log("\n设置时间为 5 秒后的时间后的时间:");
console.log(date.toString());
```

### 获取Unix时间戳
Unix 时间戳：它是一个整数值，表示自1970年1月1日00:00:00 UTC以来的毫秒数

 4 种方法获取这个时间戳

+ new Date().getTime()
+ new Date().valueOf()
+ +new Date()
+ Date.now()
+  Date.parse(str)：从一个字符串中读取日期，并且输出对应的Unix时间戳
    - 需要符合 RFC2822 或 ISO 8601 日期格式的字符串

```javascript
let timestamp1 = new Date().getTime();
console.log("getTime 方法获取的时间戳（毫秒）:", timestamp1);

let timestamp2 = new Date().valueOf();
console.log("valueOf 方法获取的时间戳（毫秒）:", timestamp2);

let timestamp3 = +new Date();
console.log("隐式转换方法获取的时间戳（毫秒）:", timestamp3);

let timestamp4 = Date.now();
console.log("Date.now 方法获取的时间戳（毫秒）:", timestamp4);

let timestamp1 = Date.parse("2024-06-25T12:00:00Z");
console.log("ISO 8601 格式解析结果（毫秒）:", timestamp1);
```

# DOM
## 概念
+ DOM： 文档对象模型，将页面所有的内容表示为可以修改的对象
+ BOM：浏览器对象模型，由浏览器提供的用于处理文档(document)之外的所有内容的其他对象，比如navigator、location、history等对象

浏览器会对我们编写的HTML、CSS进行渲染，同时它又要考虑我们可能会通过JavaScript来对其进行操作，于是浏览器将我们编写在HTML中的每一个元素（Element）都抽象成了一个个对象，所有这些对象都可以通过JavaScript来对其进行访问，那么我们就可以通过JavaScript来操作页面，所以，我们将这个抽象过程称之为文档对象模型 DOM

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719304839172-9d6abe6a-dad3-4e91-baea-e6e32ba35ad8.png)

整个文档被抽象到 document 对象中

+ document.documentElement对应的是html元素
+ document.body对应的是body元素
+ document.head对应的是head元素



节点（Node）

节点是 DOM 树中的基本单位，它可以是以下几种类型之一

+ 元素节点（Element Node）：代表 HTML 元素，例如 <div>、<p>、<a> 等标签都是元素节点
+ 文本节点（Text Node）：代表元素中的文本内容，例如 <p>Hello, world!</p> 中的 "Hello, world!" 是一个文本节点
+ 注释节点（Comment Node）：代表 HTML 中的注释，例如 <!-- This is a comment --> 中的 "This is a comment" 是一个注释节点
+ 文档节点（Document Node）：整个文档的根节点，即 document 对象本身
+ 属性节点（Attribute Node）：代表元素的属性，但在 DOM 中并不直接以节点形式表示，而是作为元素节点的一部分

节点在 DOM 中以树形结构组织，每个节点都可以有一个或多个子节点，也可能有一个父节点（除了文档节点）

## 导航
### 节点之间的导航
如果我们获取到一个节点（Node）后，可以根据这个节点去获取其他的节点，我们称之为节点之间的导航

节点之间存在如下的关系

+ 父节点：parentNode
+ 前兄弟节点：previousSibling
+ 后兄弟节点：nextSibling
+ 子节点：childNodes
+ 第一个子节点：firstChild
+ 第二个子节点：lastChild

### 元素之间的导航
如果我们获取到一个元素（Element）后，可以根据这个元素去获取其他的元素，我们称之为元素之间的导航。

节点之间存在如下的关系

+ 父元素：parentElement
+ 前兄弟节点：previousElementSibling
+ 后兄弟节点：nextElementSibling
+ 子节点：children
+ 第一个子节点：firstElementChild
+ 第二个子节点：lastElementChild

### 表格元素的导航
<table> 元素支持 (除了上面给出的，之外) 以下这些属性

+ table.rows：<tr> 元素的集合
+ table.caption/tHead/tFoot：引用元素 <caption>，<thead>，<tfoot>；
+ table.tBodies：<tbody> 元素的集合

<thead>，<tfoot>，<tbody> 元素提供了 rows 属性

+ tbody.rows：表格内部 <tr> 元素的集合

<tr>

+ tr.cells：在给定 <tr> 中的 <td> 和 <th> 单元格的集合
+ tr.sectionRowIndex：给定的 <tr> 在封闭的 <thead>/<tbody>/<tfoot> 中的位置（索引）
+ tr.rowIndex：在整个表格中 <tr> 的编号（包括表格的所有行）

<td> 和 <th>

+ td.cellIndex：在封闭的 <tr> 中单元格的编号

## 获取元素的方法
1. 通过 ID 获取元素

使用 `document.getElementById()` 方法可以根据元素的 ID 属性获取元素节点。

```javascript
let elementById = document.getElementById('myElementId');
```

2. 通过类名获取元素集合

使用 `document.getElementsByClassName()` 方法可以根据元素的类名获取一组元素节点

```javascript
let elementsByClassName = document.getElementsByClassName('myClassName');
```

3. 通过标签名获取元素集合

使用 `document.getElementsByTagName()` 方法可以根据元素的标签名获取一组元素节点

```javascript
let elementsByTagName = document.getElementsByTagName('div');
```

4. 通过选择器获取单个元素

使用 `document.querySelector()` 方法可以使用 CSS 选择器语法获取匹配的**第一个**元素节点

```javascript
let elementByQuerySelector = document.querySelector('#myElementId');
```

5. 通过选择器获取元素集合

使用 `document.querySelectorAll()` 方法可以使用 CSS 选择器语法获取所有匹配的元素节点集合

```javascript
let elementsByQuerySelectorAll = document.querySelectorAll('.myClassName');
```

6. 通过 name 获取元素

使用`getElementsByName()` 方法是用于获取具有指定 name 属性的元素节点的方法

```javascript
<form name="loginForm">
  <input type="text" name="username" />
</form>
let usernameInputs = document.getElementsByName('username');
```

## 节点的属性
### nodeType
nodeType 是 DOM 中节点对象的一个属性，用于标识节点的类型

在 JavaScript 中，每个节点对象都有一个 nodeType 属性，其值是一个整数，代表节点的类型

+ 元素节点 ：值为 1，代表 HTML 或 XML 文档中的元素，例如 <div>、<p>、<a> 等
+ 属性节点：在 DOM 中，并不直接有 nodeType 为 2 的属性节点，元素的属性可以通过 attributes 属性获取，而不是通过单独的节点对象
+ 文本节点：值为 3，代表元素或属性中的文本内容
+ 注释节点：值为 8，代表文档中的注释
+ 文档节点 ：值为 9，代表整个文档，通常通过 document 对象访问
+ 文档类型节点 ：值为 10，代表文档类型声明，例如 <!DOCTYPE html>
+ 文档片段节点 ：值为 11，代表文档的片段或临时存储区域

### nodeName、tagName
nodeName：获取node节点的名字

```javascript
let element = document.createElement('div');
console.log(element.nodeName); // 输出: "DIV"

let textNode = document.createTextNode('Hello, world!');
console.log(textNode.nodeName); // 输出: "#text"

let commentNode = document.createComment('This is a comment');
console.log(commentNode.nodeName); // 输出: "#comment"
```

tagName：获取元素的标签名词

```javascript
let element = document.createElement('div');
console.log(element.tagName); // 输出: "DIV"

// 对于非元素节点，`tagName` 不存在，所以以下代码会抛出错误
// let textNode = document.createTextNode('Hello, world!');
// console.log(textNode.tagName); // 会报错
```

tagName 和 nodeName 有什么不同

+ tagName 属性仅适用于 Element 节点
+ nodeName 是为任意 Node 定义的
    - 对于元素，它的意义与 tagName 相同，所以使用哪一个都是可以的
    - 对于其他节点类型（text，comment 等），它拥有一个对应节点类型的字符串

### innerHTML、 outerHTML、textContent
这三个属性都是用于访问和设置 HTML 元素内容的属性

innerHTML：通过 innerHTML 可以操作和修改元素内部的 HTML 内容

```javascript
let divElement = document.getElementById('myDiv');
// 获取元素内部的 HTML 结构
console.log(divElement.innerHTML); 
// 设置元素内部的 HTML 内容
divElement.innerHTML = '<p>New content</p>'; 
```

outerHTML：用于获取或设置包括当前元素本身在内的整个 HTML 片段，当使用 outerHTML 时，它会返回包括元素标签在内的完整 HTML 内容，并且可以通过赋值操作来替换整个元素及其内容

```javascript
let divElement = document.getElementById('myDiv');
// 获取包含整个元素的 HTML 片段
console.log(divElement.outerHTML); 
// 替换整个元素及其内容
divElement.outerHTML = '<div id="newDiv"><p>New content</p></div>'; 
```

textContent：用于获取或设置元素及其所有子元素的文本内容，当使用 textContent 时，它会忽略 HTML 结构，只返回纯文本内容

```javascript
let divElement = document.getElementById('myDiv');
// 获取元素及其子元素的文本内容
console.log(divElement.textContent); 
// 设置元素及其子元素的文本内容
divElement.textContent = 'New text'; 
```

### nodeValue、data
nodeValue 和 data 属性用于获取和设置文本节点的值

nodeValue

+ 对于文本节点和注释节点，nodeValue 返回的是节点的内容
+ 对于元素节点（如 <div>、<p> 等），nodeValue 为 null

```javascript
let textNode = document.createTextNode('Hello, world!');
console.log(textNode.nodeValue); // 输出: "Hello, world!"

textNode.nodeValue = 'New content';
console.log(textNode.nodeValue); // 输出: "New content"

let element = document.createElement('div');
console.log(element.nodeValue); // 输出: null
```

data

+ data 是一个专用于 CharacterData 接口的可读写属性
    - CharacterData 是 Text、Comment 和 CDATASection 这些节点类型的基类 
    - CDATASection 是 XML 和 HTML 文档中的一种特殊文本节点类型，它的主要作用是允许在文档中包含特殊字符和其他标记，同时保持数据的原始性和完整性
+ data 属性返回或设置文本节点的内容，与 nodeValue 类似，但它仅适用于 CharacterData 节点

```javascript
let textNode = document.createTextNode('Hello, world!');
console.log(textNode.data); // 输出: "Hello, world!"

textNode.data = 'New content';
console.log(textNode.data); // 输出: "New content"
```

### 其他属性
hidden：指示元素是否应该被隐藏

```javascript
<div hidden>
    This content is hidden.
</div>
```

value：指定表单元素的初始值，类型是字符串，主要用于 <input>、<button>、<option>、<textarea> 等表单元素

```javascript
<input type="text" value="Default text">
<button value="Click me">Button</button>
<textarea>Default text area content</textarea>
```

href：定义链接的目标 URL

```javascript
<a href="https://www.example.com">Visit Example</a>
<link rel="stylesheet" href="styles.css">
```

id：定义元素的唯一标识符（ID），类型时字符串，作用范围是几乎所有 HTML 元素，但是每个 ID 在同一文档中必须是唯一的

```javascript
<div id="header">This is the header</div>
<p id="intro">This is an introductory paragraph.</p>
```

## 元素的属性
### attribute
一个元素除了有开始标签、结束标签、内容之外，还有很多的属性（attribute），浏览器在解析HTML元素时，会将对应的attribute也创建出来放到对应的元素对象上

+ 标准的attribute：某些attribute属性是标准的，比如id、class、href、type、value
+ 非标准的attribute：某些attribute属性是自定义的，比如abc、age、height

attribute具备以下特征

+ 它们的名字是大小写不敏感的（id 与 ID 相同）
+ 它们的值总是字符串类型的



对于所有的attribute访问都支持如下的方法

+ elem.hasAttribute(name)：检查指定名称的特性是否存在于元素中，存在则返回 true，否则返回 false

```javascript
if (elem.hasAttribute('id')) {
    console.log('Element has id attribute.');
}
```

+ elem.getAttribute(name)：获取指定名称的特性的值，返回字符串，如果特性不存在，则返回 null

```javascript
var idValue = elem.getAttribute('id');
```

+ elem.setAttribute(name, value)：设置或添加指定名称的特性，并指定特性的值，name 是要设置的特性的名称，value 是要设置的值

```javascript
elem.setAttribute('class', 'example');
```

+ elem.removeAttribute(name)：移除这个特性，name 是要移除的特性的名称

```javascript
elem.removeAttribute('title');
```

+ attributes：返回一个类数组对象，其中包含了元素的所有特性节点（Attr 对象），每个属性节点具有 name 和 value 属性，分别表示特性的名称和值

```javascript
var attrs = elem.attributes;
for (var i = 0; i < attrs.length; i++) {
    console.log(attrs[i].name + ': ' + attrs[i].value);
}
```

### property
Property（属性） 是面向对象编程中对象的特性或数据成员。在JavaScript中，对象的属性是指对象的状态或数据，并且每个属性都有一个名称和值

对于标准的attribute，会在DOM对象上创建与其对应的property属性

```javascript
var person = {
  name: "Alice",
  age: 30
};
```

除非特别情况，大多数情况下，设置、获取attribute，推荐使用property的方式：

这是因为它默认情况下是有类型的

### class
有时候我们会通过JavaScript来动态修改样式，这个时候我们有两个选择：

1. 在CSS中编写好对应的样式，动态的添加class
2. 动态的修改style属性

在大多数情况下，如果可以动态修改class完成某个功能，更推荐使用动态添加 class



元素的class attribute，对应的property并非叫class，而是className

这是因为JavaScript早期是不允许使用class这种关键字来作为对象的属性，所以DOM规范使用了className，虽然现在JavaScript已经没有这样的限制，但是并不推荐，并且依然在使用className这个名称

```javascript
<div id="myDiv" class="box blue"></div>

var divElement = document.getElementById('myDiv');

// 获取元素的 class 属性值
var classes = divElement.className;
console.log(classes);  // 输出: "box blue"

// 设置元素的 class 属性值
divElement.className = "box red";
console.log(divElement.className);  // 输出: "box red"
```

注意：使用 className 属性时，直接覆盖元素的全部类名，因此不适合对类名进行单个添加或移除操作



classList 是 HTML DOM 元素对象的一个属性，它提供了一组方法来操作元素的类名，这些方法包括：

+ add(class1, class2, ...)：向元素添加一个或多个类名
+ remove(class1, class2, ...)：从元素中移除一个或多个类名
+ toggle(className [, force])： 如果类不存在就添加类，存在就移除它，如果 force 参数为 true，则强制添加类名，如果为 false，则强制移除类名。
+ contains(className)：检查元素是否包含指定的类名，返回布尔值

```javascript
<div id="myDiv" class="box blue"></div>

var divElement = document.getElementById('myDiv');

// 添加一个类名
divElement.classList.add('red');

// 移除一个类名
divElement.classList.remove('blue');

// 切换一个类名的状态
divElement.classList.toggle('active');

// 检查是否包含某个类名
var hasClass = divElement.classList.contains('box');
console.log(hasClass);  // 输出: true
```

### style
在 JavaScript 中，可以使用 style 属性来访问和修改元素的内联样式。style 属性是一个对象，包含了所有与该元素相关的样式属性

访问内联样式属性值

```javascript
var myDiv = document.getElementById('myDiv');

// 获取元素的 color 样式值
var colorValue = myDiv.style.color;
console.log(colorValue);  // 输出: "red"

// 获取元素的 font-size 样式值
var fontSizeValue = myDiv.style.fontSize;
console.log(fontSizeValue);  // 输出: "16px"
```

getComputedStyle 

+ 对于内联样式，是可以通过style.*的方式读取到的，而对于style、css文件中的样式，是读取不到的，可以使用getComputedStyle()
+ getComputedStyle()常用于获取计算后的样式的方法
+ 接受两个参数：要获取样式的元素和一个可选的伪元素字符串（例如 ::before 或 ::after）
+ 返回值是一个 CSSStyleDeclaration 对象，它包含了计算后的所有样式属性和值。这些值通常是字符串形式，但也可能包含其他类型，例如颜色值可能是 RGB 表示法。

```javascript
<div id="myElement" style="width: 200px; height: 100px; background-color: yellow;">Hello, world!</div>
// 获取元素
var element = document.getElementById('myElement');

// 获取计算后的样式对象
var styles = window.getComputedStyle(element);

// 访问样式属性
var width = styles.width;
var height = styles.height;
var backgroundColor = styles.backgroundColor;

console.log("Width:", width); // 输出: "200px"
console.log("Height:", height); // 输出: "100px"
console.log("Background Color:", backgroundColor); 
// 输出: "rgb(255, 255, 0)" (或者其他浏览器中的具体颜色格式)

//如果要获取伪元素的样式，可以传入第二个参数指定伪元素的名称
var pseudoStyles = window.getComputedStyle(element, '::before');
```

设置内联样式属性值

```javascript
var myDiv = document.getElementById('myDiv');

// 设置元素的 color 样式
myDiv.style.color = "blue";

// 设置元素的 font-size 样式
myDiv.style.fontSize = "20px";
```

注意事项

+ 优先级：内联样式的优先级高于外部样式表（CSS文件）和 <style> 标签中的样式
+ 单位：设置样式值时，通常需要包括单位（如像素单位 px、百分比 % 等），但有些属性（如 opacity）可以直接设置为数字
+ 属性名称：在 JavaScript 中，属性名使用驼峰命名法（camelCase），如 fontSize、backgroundColor，而不是 CSS 中的短横线分隔的形式（如 font-size、background-color）

## 元素的常见操作
### 创建元素
document.createElement(tag)

这个方法接受一个参数 tag，表示你要创建的元素的标签名

这些代码会在内存中创建对应的 HTML 元素，但是它们还没有被添加到页面中。要将这些元素添加到页面中，你需要使用其他 DOM 方法，例如 appendChild() 将元素添加到父元素中

```javascript
// 创建一个新的 <p> 元素
var newParagraph = document.createElement('p');

// 创建一个文本节点
var paragraphText = document.createTextNode('这是新段落的内容。');

// 将文本节点添加到段落元素中
newParagraph.appendChild(paragraphText);

// 将新段落添加到页面中的 body 元素中
document.body.appendChild(newParagraph);
```

### 插入元素
+ node.append(...nodes or strings) —— 在 node 末尾插入节点或字符串
+ node.prepend(...nodes or strings) —— 在 node 开头插入节点或字符串
+ node.before(...nodes or strings) —— 在 node 前面‘；插入节点或字符串
+ node.after(...nodes or strings) —— 在 node 后面插入节点或字符串
+ node.replaceWith(...nodes or strings) —— 将 node 替换为给定的节点或字符串

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719371535572-c2c84a4c-ce64-4e55-9791-54ebe4f652e8.png)

### 移除和克隆元素
remove() 是 DOM 元素的方法之一，用于将当前元素从其父节点中移除

```javascript
var element = document.getElementById('elementToRemove');
element.remove();
```

cloneNode() 是 DOM 元素的方法，用于创建当前元素的一个克隆副本。这个方法会复制元素本身及其所有的属性和子节点（如果有的话），但不会复制事件处理程序

```javascript
var clone = element.cloneNode(deep);
```

其中，element 是要克隆的元素，deep 是一个布尔值参数，指定是否深度克隆子节点。如果 deep 为 true，则会克隆元素的所有子节点；如果为 false，则只会克隆元素本身而不包括子节点（默认为 false）

```javascript
//浅拷贝，只复制元素本身
var original = document.getElementById('originalElement');
var shallowClone = original.cloneNode(false);
//深拷贝，复制元素及其所有子节点
var original = document.getElementById('originalElement');
var deepClone = original.cloneNode(true);
```

### 旧的元素操作方法(了解)
parentElem.appendChild(node)：在parentElem的父元素最后位置添加一个子元素

parentElem.insertBefore(node, nextSibling)：在parentElem的nextSibling前面插入一个子元素

parentElem.replaceChild(node, oldChild)：在parentElem中，新元素替换之前的oldChild元素

parentElem.removeChild(node)：在parentElem中，移除某一个元素

## 元素的大小和滚动
+ clientHeight：contentHeight+padding
+ clientTop：border-top的宽度
+ clientLeft：border-left的宽度
+ offsetWidth：元素完整的宽度，clientHeight+border
+ offsetHeight：元素完整的高度
+ offsetLeft：距离父元素的x
+ offsetHeight：距离父元素的y
+ scrollHeight：容器的像素高度（完整高度，包括滚动条和隐藏的内容）
+ scrollTop：滚动部分的高度

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725174132310-2ab0969d-61cd-428c-81bf-2f7a42ff03b3.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1725176500536-68210270-7a3d-4fce-be9e-609754ab8e22.png)

## window的大小和滚动
window的width和height

+ innerWidth、innerHeight：获取window窗口的宽度和高度（包含滚动条）
+ outerWidth、outerHeight：获取window窗口的整个宽度和高度（包括调试工具、工具栏）
+ documentElement.clientHeight、documentElement.clientWidth：获取html的宽度和高度（不包含滚动条）

window的滚动位置：

+ scrollX：X轴滚动的位置（别名pageXOffset）
+ scrollY：Y轴滚动的位置（别名pageYOffset）

也有提供对应的滚动方法：

+ 方法 scrollBy(x,y) ：将页面滚动至 相对于当前位置的 (x, y) 位置
+ 方法 scrollTo(pageX,pageY) 将页面滚动至 绝对坐标

# 事件处理
## 事件监听
三种方式

1. 在script中直接监听（很少使用）
2. DOM属性，通过元素的on来监听事件
3. 通过EventTarget中的addEventListener来监听

### addEventListener
addEventListener 是 JavaScript 中用于向指定元素添加事件监听器的方法。通过 addEventListener，我们可以在元素上注册各种类型的事件处理函数，例如点击事件、键盘事件、鼠标事件等

`target.addEventListener(type, listener [, options]);`

+ type: 字符串，表示要监听的事件类型，"click"、"keydown"、"mouseover" 等。事件类型可以是标准的 DOM 事件，也可以是自定义事件
+ listener: 事件处理函数，当指定的事件类型在目标对象上触发时，浏览器会调用这个函数。事件处理函数通常会接收一个事件对象作为参数，这个事件对象包含了关于事件的详细信息，例如事件的类型、触发元素等
+ options (可选): 一个可选的配置对象，用于设置监听器的行为
    - capture：布尔值，默认为 false。指定事件是在捕获阶段（capture phase）还是冒泡阶段（bubble phase）触发监听器。
    - once：布尔值，默认为 false。指定事件处理函数在每个元素上最多只调用一次。调用后即自动移除监听器
    - passive：布尔值，默认为 false。如果设为 true，表示告知浏览器这个监听器永远不会调用 preventDefault() 方法来取消事件的默认行为，这能够提升性能

```javascript
// 获取一个按钮元素
var button = document.getElementById('myButton');

// 添加点击事件监听器
button.addEventListener('click', function(event) {
  console.log('按钮被点击了！');
});

// 添加键盘按下事件监听器
document.addEventListener('keydown', function(event) {
  console.log('按下键盘键:', event.key);
});

// 添加一次性的鼠标移入事件监听器
var box = document.querySelector('.box');
box.addEventListener('mouseover', function(event) {
  console.log('鼠标移入了盒子');
}, { once: true });
```

注意：

+ addEventListener 允许在同一个元素上添加多个相同或不同类型的事件监听器
+ 通过 addEventListener 添加的事件监听器不会覆盖已存在的同类型事件监听器，而是会按照添加的顺序依次执行
+ 可以通过 removeEventListener 方法移除通过 addEventListener 添加的事件监听器

## 事件冒泡和捕获
默认情况下事件是从最内层的span向外依次传递的顺序，这个顺序称之为事件冒泡

还有另外一种监听事件流的方式就是从外层到内层（body -> span），这种称之为事件捕获

为什么会产生两种不同的处理流？

+ 这是因为早期浏览器开发时，不管是IE还是Netscape公司都发现了这个问题
+ 但是他们采用了完全相反的事件流来对事件进行了传递
+ IE采用了事件冒泡的方式，Netscape采用了事件捕获的方式

捕获和冒泡的过程

1. 捕获阶段：事件（从 Window）向下走近元素
2. 目标阶段：事件到达目标元素
3. 冒泡阶段：事件从元素上开始冒泡

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719393828641-f2d55550-1d26-43e3-ac52-1880473554ad.png)

## 事件对象
当一个事件发生时，就会有和这个事件相关的很多信息，比如事件的类型是什么，点击的是哪一个元素，点击的位置是哪里等等相关的信息，这些信息会被封装到一个对象中，这个对象由浏览器创建，称之为event对象

event对象会在传入的事件处理（event handler）函数回调时，被系统传入

事件对象具有多个常用的属性和方法，下面是常见的

+ type：表示事件类型的字符串，如 "click", "mouseover" 等
+ target：触发事件的元素，即事件最初发生的元素
+ currentTarget：当前正在处理事件的元素，即事件处理函数绑定的元素
+ eventPhase：表示事件流的当前阶段，可以是 Event.CAPTURING_PHASE、Event.AT_TARGET 或 Event.BUBBLING_PHASE
+ stopPropagation()：即取消事件的冒泡或捕获
+ preventDefault()：阻止事件的默认行为，比如点击链接时阻止跳转，或在表单提交时阻止默认的提交行为
+ stopImmediatePropagation()：立即阻止事件的进一步传播，并防止任何其他事件处理程序被调用
+ offsetX、offsetY：事件发生在元素内的位置
+ clientX、clientY：事件发生在客户端内的位置
+ pageX、pageY：事件发生在客户端相对于document的位置
+ screenX、screenY：事件发生相对于屏幕的位置

```javascript
document.addEventListener('click', function(event) {
  console.log('点击事件发生了！');
  console.log('事件类型：', event.type);
  console.log('触发事件的元素：', event.target);
});
```

注意

+ 事件对象是唯一的：每次触发事件时，浏览器都会创建一个新的事件对象。因此，即使是相同类型的事件，它们的事件对象也是独立的
+ 跨浏览器兼容性：事件对象的属性和方法在不同的浏览器中可能略有不同，因此在编写跨浏览器的代码时，需要注意一些细微的差异

## 事件处理中的 this
1. 全局环境下的 this：如果事件处理函数是作为全局函数直接调用的，this 将指向全局对象（在浏览器中通常是 window 对象）
2. 作为对象方法调用的 this：如果事件处理函数作为对象的方法调用（例如通过 addEventListener 绑定），this 将指向该对象
3. 使用箭头函数的 this：如果使用箭头函数定义事件处理函数，this 将继承自外部作用域，而不是绑定到触发事件的元素
4. 在 addEventListener 回调函数中使用了 event 对象，则 this===event.target

```javascript
const button = document.getElementById('myButton');

button.addEventListener('click', function() {
  console.log(this); // 输出为 <button id="myButton">Click me</button>
  this.style.backgroundColor = 'red'; // 改变按钮的背景色
});

button.addEventListener('click', function(event) {
  console.log(this === event.target); // 输出为 true
  this.style.backgroundColor = 'red'; // 改变按钮的背景色
});

// 使用箭头函数
const input = document.getElementById('myInput');
input.addEventListener('input', () => {
  console.log(this); // 输出为 Window 或全局对象，不是 input 元素
});
```

## EventTarget类
 EventTarget是一个DOM接口，主要用于添加、删除、派发Event事件，表示能够接收事件的对象，通常是 DOM 中的节点（如元素、文档等）。所有能够接收事件的对象都是 EventTarget 的实例，它们可以通过事件监听器来监听特定类型的事件，并在事件发生时执行相应的操作

### 作用
+ 事件监听器的注册和触发
    - EventTarget 对象可以使用 addEventListener 方法注册事件监听器，监听特定类型的事件（如点击、输入、变化等）。当相应的事件发生时，注册的事件处理函数将被调用
+ 事件流的参与
    - EventTarget 参与事件流的三个阶段：捕获阶段、目标阶段和冒泡阶段。事件在 EventTarget 上触发后，会沿着从根节点到目标节点（捕获阶段）、目标节点（目标阶段）和从目标节点到根节点（冒泡阶段）的路径传播
+ DOM 中的实现
    - 在 DOM 中，几乎所有的节点都是 EventTarget 的实例，包括文档对象、元素节点、文本节点等。因此，可以在这些节点上注册和处理事件

### 方法
+ addEventListener(type, listener, options)：注册事件监听器，当指定类型的事件在目标上触发时，将调用 listener 函数。options 参数是一个可选的配置对象，用于指定是否在捕获阶段处理事件，以及其他配置
+ removeEventListener(type, listener, options)：移除先前注册的事件监听器，使其不再接收特定类型的事件通知
+ dispatchEvent(event)：在 EventTarget 上分派一个事件，即手动触发特定类型的事件。这可以用于模拟用户操作或编程方式触发事件处理

### 例子
在这个例子中，button 是一个 EventTarget，我们使用 addEventListener 方法注册了一个点击事件的监听器 handleClick，当按钮被点击时，将会执行 handleClick 函数并输出点击信息

```javascript
const button = document.getElementById('myButton');
function handleClick(event) {
  console.log('按钮被点击了！', event.target);
}
button.addEventListener('click', handleClick);
// 当按钮被点击时，handleClick 函数将被调用，并输出相应的信息
```

## 事件委托
事件委派（Event delegation）是一种优化事件处理的技术，特别适用于需要对大量子元素进行相同事件处理的情况。它利用了事件冒泡的特性，将事件处理程序绑定到它们的共同祖先，而不是直接绑定到每个子元素上

### 优点
1. 减少事件处理程序的数量：通过将事件处理程序绑定到父元素（或更高层级的祖先），而不是每个子元素上，可以减少页面中的事件处理程序数量。特别是当有很多相似子元素需要相同事件处理逻辑时，这种方式能够显著简化代码
2. 动态添加和删除元素：因为事件是在父元素上处理的，所以对于动态添加或删除的子元素，无需重新绑定或解绑事件处理程序，它们仍然会受到相同的事件处理逻辑影响
3. 提升性能：在大型文档中，使用事件委派可以提高性能。相比每个子元素都绑定事件处理程序，事件委派利用了事件冒泡，将事件处理推迟到更高层级的元素，减少了事件处理的次数

### 实现
在实践中，通过在父元素上注册事件监听器，并在事件处理程序中判断事件目标（event.target 或 event.currentTarget），可以确定具体触发事件的子元素

```javascript
// HTML 结构
<ul id="parentList">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>

// JavaScript 使用事件委派
const parentList = document.getElementById('parentList');

parentList.addEventListener('click', function(event) {
  if (event.target.tagName === 'LI') {
    console.log('点击了列表项:', event.target.textContent);
    // 在这里处理点击列表项的逻辑
  }
});
```

在上面的示例中，将事件监听器绑定到了 parentList 上，而不是每个 li 元素上。当用户点击列表项时，事件会冒泡到 parentList 元素，并被捕获到事件监听器中。通过检查 event.target.tagName 来判断具体点击的是哪个 li 元素，并做出相应的处理

### 标记
某些事件委托可能需要对具体的子组件进行区分，这个时候可以使用data-*对其进行标记

```javascript
<ul id="parentList">
  <li data-id="1">Item 1</li>
  <li data-id="2">Item 2</li>
  <li data-id="3">Item 3</li>
</ul>

const parentList = document.getElementById('parentList');

parentList.addEventListener('click', function(event) {
  if (event.target.tagName === 'LI') {
    const itemId = event.target.dataset.id;
    console.log(`点击了具有ID ${itemId} 的列表项:`, event.target.textContent);
    // 在这里可以根据 itemId 进行特定子组件的操作
  }
});
```

### 使用场景
+ 列表和表格: 处理大量的列表或表格行，其中每个子元素（如列表项或表格行）需要相同的点击事件处理逻辑
+ 动态内容: 当页面上的内容是通过 AJAX 或其他方式动态添加时，事件委派可以确保新元素添加到页面后仍能正常工作，而不需要重新绑定事件处理程序
+ 性能优化: 在需要考虑性能的情况下，使用事件委派可以减少内存消耗和事件处理的复杂性

## 鼠标事件
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719395275789-cb5109c7-5bde-4e2a-b822-027fc53e1da1.png)

mouseenter和mouseleave

+ 不支持冒泡
+ 进入子元素依然属于在该元素内，没有任何反应

mouseover和mouseout

+ 支持冒泡
+ 进入元素的子元素时
    - 先调用父元素的mouseout
    - 再调用子元素的mouseover
    - 因为支持冒泡，所以会将mouseover传递到父元素中

## 键盘事件
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719395355247-78bc27ff-ad8f-4d49-9f78-d85a017f398a.png)

事件的执行顺序是 onkeydown、onkeypress、onkeyup

+ down事件先发生
+ press发生在文本被输入
+ up发生在文本输入完成

## 表单事件
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719395386485-45f74ea0-54ad-4799-bcad-970f913cfc03.png)

## 文档加载事件
文档加载事件是指在浏览器解析HTML文档并完全加载所有资源（如图片、样式表、脚本等）后触发的事件。这些事件可以让开发者在页面完全准备就绪后执行特定的操作或逻辑

DOMContentLoaded

+ 触发时机：当初始的HTML文档被完全加载和解析完成之后，不需要等待样式表、图像和子框架的加载，就会触发 DOMContentLoaded 事件
+ 用途：通常用于在页面元素完全加载后执行初始化脚本、绑定事件处理程序或执行其他DOM操作

```javascript
document.addEventListener('DOMContentLoaded', function(event) {
  console.log('DOM 已加载');
  // 在这里进行初始化操作
});
```

load

+ 触发时机：当整个页面及其依赖资源（如样式表、图片等）都加载完成后，就会触发 load 事件
+ 用途：通常用于处理需要所有资源加载完成后才能进行的操作，比如修改DOM、操作Canvas或执行需要完整页面支持的JavaScript代码

```javascript
window.addEventListener('load', function(event) {
  console.log('页面完全加载');
  // 在这里执行需要页面完全加载后才能进行的操作
});
```

# BOM
## 概念
浏览器对象模型（Browser Object Model），简称 BOM，由浏览器提供的用于处理文档（document）之外的所有内容的其他对象，可以将BOM看成是连接JavaScript脚本与浏览器窗口的桥梁

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1731054736806-0083e093-0497-4229-8dd1-f061531427f8.png)

JavaScript有一个非常重要的运行环境就是浏览器，而且浏览器本身又作为一个应用程序需要对其本身进行操作所以通常浏览器会有对应的对象模型BOM，可以将BOM看成是连接JavaScript脚本与浏览器窗口的桥梁

BOM主要包括一下的对象模型

+ window：包括全局属性、方法，控制浏览器窗口相关的属性、方法
+ location：浏览器连接到的对象的位置（URL）
+ history：操作浏览器的历史
+ navigator：用户代理（浏览器）的状态和标识（很少用到）
+ screen：屏幕窗口信息（很少用到）

## window
### 简介
window对象在浏览器中可以从两个视角来看待：

+ 视角一：全局对象
    - ECMAScript是有一个全局对象的，这个全局对象在Node中是global
    - 在浏览器中就是window对象
+ 视角二：浏览器窗口对象
    - 作为浏览器窗口时，提供了对浏览器操作的相关的API
+ 当然，这两个视角存在大量重叠的地方，所以不需要刻意去区分它们

### 常见的属性
```javascript
// 1. window.location - 当前页面的 URL 信息
console.log(window.location.href); // 当前页面的完整 URL

// 2. window.document - 当前页面的文档对象
console.log(window.document.title); // 当前页面的标题

// 3. window.navigator - 浏览器的用户代理信息
console.log(window.navigator.userAgent); // 浏览器的用户代理字符串

// 4. window.innerWidth - 浏览器窗口的视口宽度
console.log(window.innerWidth); // 当前视口的宽度

// 5. window.innerHeight - 浏览器窗口的视口高度
console.log(window.innerHeight); // 当前视口的高度

// 6. window.history - 浏览器历史记录对象
console.log(window.history.length); // 当前会话中的历史记录条数

// 7. window.screen - 用户屏幕的信息
console.log(window.screen.width); // 用户屏幕的宽度
console.log(window.screen.height); // 用户屏幕的高度

// 8. window.localStorage - 本地存储对象
window.localStorage.setItem("username", "Alice"); // 本地存储数据
console.log(window.localStorage.getItem("username")); // 获取本地存储的数据

// 9. window.sessionStorage - 会话存储对象
window.sessionStorage.setItem("sessionId", "12345"); // 会话存储数据
console.log(window.sessionStorage.getItem("sessionId")); // 获取会话存储的数据

// 10. window.console - 控制台对象，用于调试
console.log("This is a log message"); // 输出日志信息到控制台

// 11. window.name - 当前窗口的名称
window.name = "myWindow";
console.log(window.name); // 输出窗口的名称

// 12. window.parent - 获取父窗口的引用（如果存在嵌套框架）
console.log(window.parent === window); // 在顶层窗口中，window.parent 等于 window 本身

// 13. window.top - 获取最顶层窗口的引用
console.log(window.top === window); // 在顶层窗口中，window.top 等于 window 本身

// 14. window.frames - 包含当前窗口中所有框架的集合
console.log(window.frames.length); // 当前窗口中的框架数量

// 15. window.screenX / window.screenY - 当前窗口相对于屏幕的 X 和 Y 坐标
console.log(window.screenX); // 输出窗口的 X 坐标
console.log(window.screenY); // 输出窗口的 Y 坐标
```

### 常见的方法
```javascript
// 1. window.alert() - 弹出一个警告对话框
window.alert("Hello, World!"); // 显示一个提示框

// 2. window.confirm() - 弹出一个确认对话框
const result = window.confirm("Are you sure?"); // 返回 true 或 false，用户点击 "确认" 或 "取消"

// 3. window.prompt() - 弹出一个输入对话框
const userInput = window.prompt("Enter your name:"); // 返回用户输入的字符串

// 4. window.open() - 打开一个新窗口或新标签页
window.open("https://www.juejin.cn"); // 打开指定 URL 的新页面

// 5. window.close() - 关闭当前窗口
window.close(); // 关闭当前窗口（需要由脚本打开的窗口才有效）

// 6. window.setTimeout() - 设置一个定时器，在指定的时间后执行代码
window.setTimeout(() => {
  console.log("This message is delayed by 2 seconds");
}, 2000); // 延迟 2 秒后输出信息

// 7. window.setInterval() - 设置一个定时器，每隔一定时间重复执行代码
const intervalId = window.setInterval(() => {
  console.log("This message repeats every 1 second");
}, 1000); // 每隔 1 秒输出信息

// 8. window.clearInterval() - 清除通过 setInterval 创建的定时器
window.clearInterval(intervalId); // 停止上面创建的定时器

// 9. window.addEventListener() - 为窗口添加事件监听器
window.addEventListener("resize", () => {
  console.log("Window resized!");
}); // 监听窗口大小变化事件

// 10. window.removeEventListener() - 移除事件监听器
const resizeHandler = () => console.log("Window resized!");
window.addEventListener("resize", resizeHandler);
window.removeEventListener("resize", resizeHandler); // 移除窗口大小变化的监听器

// 11. window.scrollTo() - 滚动到指定位置
window.scrollTo(0, 100); // 滚动到页面的顶部 100 像素处

// 12. window.focus() - 使当前窗口获得焦点
window.focus(); // 使窗口获得焦点

// 13. window.print() - 打印当前页面
window.print(); // 打开打印对话框，用于打印当前页面内容
```

### 常见的事件
window的事件都通过继承自EventTarget的addEventListener实例方法完成的

EventTarget.addEventListener() 方法将指定的监听器注册到 EventTarget上，当该对象触发指定的事件时，指定的回调函数就会被执行，因此addEventListener方法只是一个媒介，具体事件由事件监听器完成

```javascript
// 1. load - 页面加载完成事件
window.addEventListener("load", () => {
  console.log("页面加载完成");
});

// 2. unload - 页面卸载事件
window.addEventListener("unload", () => {
  console.log("页面即将卸载");
});

// 3. resize - 窗口大小改变事件
window.addEventListener("resize", () => {
  console.log("窗口大小改变了");
});

// 4. scroll - 窗口滚动事件
window.addEventListener("scroll", () => {
  console.log("页面滚动了");
});

// 5. beforeunload - 页面即将被关闭或刷新前的事件
window.addEventListener("beforeunload", (event) => {
  event.preventDefault(); // 防止页面关闭或刷新
  event.returnValue = ""; // 显示提示信息
});

// 6. focus - 窗口获得焦点事件
window.addEventListener("focus", () => {
  console.log("窗口获得了焦点");
});

// 7. blur - 窗口失去焦点事件
window.addEventListener("blur", () => {
  console.log("窗口失去了焦点");
});

// 8. error - JavaScript 运行错误事件
window.addEventListener("error", (event) => {
  console.error("发生错误：", event.message);
});

// 9. online - 网络连接恢复事件
window.addEventListener("online", () => {
  console.log("网络连接恢复");
});

// 10. offline - 网络连接断开事件
window.addEventListener("offline", () => {
  console.log("网络连接断开");
});
```

## Location
location 是在浏览器中的一个对象，它提供了当前窗口中加载的文档的信息，并且可以用来导航到新的URL。在浏览器环境中，location 对象是 window 对象的一个属性，可以通过 window.location 或者简写为 location 来访问

### 属性
+ href: 当前window对应的超链接URL, 整个URL
+ protocol: 当前的协议
+ host: 主机地址
+ hostname: 主机地址(不带端口)
+ port: 端口
+ pathname: 路径
+ search: 查询字符串
+ hash: 哈希值
+ username：URL中的username（很多浏览器已经禁用）
+ password：URL中的password（很多浏览器已经禁用）

### 方法
+ assign：赋值一个新的URL，并且跳转到该URL中
+ replace：打开一个新的URL，并且跳转到该URL中（不同的是不会在浏览记录中留下之前的记录）
+ reload：重新加载页面，可以传入一个Boolean类型

```javascript
// 1. location.assign() - 导航到新的 URL，并保存当前页面到历史记录
window.location.assign("https://www.juejin.cn");
// 跳转到指定 URL，并在浏览器历史中记录（可以通过 "后退" 按钮返回）

// 2. location.replace() - 导航到新的 URL，但不保存当前页面
window.location.replace("https://www.baidu.com");
// 跳转到指定 URL，不保留当前页面（无法通过 "后退" 按钮返回）

// 3. location.reload() - 重新加载当前页面
window.location.reload();
// 重新加载当前页面，类似于刷新浏览器
```

### URLSearchParams(理解)
URLSearchParams 是一个 JavaScript 内置对象，用于在 URL 的查询字符串中获取参数以及操作这些参数。它允许你创建、检索、添加、删除查询参数，以及将查询参数对象转换成 URL 查询字符串格式。这个对象通常在处理 URL 查询参数时非常有用

创建 URLSearchParams 对象：可以通过传入一个 URL 查询字符串或者直接创建一个空的 URLSearchParams 对象来初始化

```javascript
// 从 URL 查询字符串创建 URLSearchParams 对象
const params = new URLSearchParams('?query=search&lang=en');

// 创建一个空的 URLSearchParams 对象
const emptyParams = new URLSearchParams();
```

操作查询参数

```javascript
// 获取参数值
console.log(params.get('query')); // 输出: "search"

// 设置或更新参数值
params.set('page', '1');

// 添加新的参数
params.append('sort', 'desc');

// 删除指定的参数
params.delete('lang');

// 检查参数是否存在
console.log(params.has('sort')); // 输出: true
```

生成查询字符串：使用 toString() 方法可以将 URLSearchParams 对象转换为字符串格式的查询参数部分，适合用于构建完整的 URL

```javascript
console.log(params.toString()); 
// 输出: "query=search&page=1&sort=desc"
```

遍历参数：可以使用 forEach() 方法或者 for...of 循环遍历 URLSearchParams 对象中的所有参数和对应的值

```javascript
params.forEach((value, key) => {
    console.log(`${key}: ${value}`);
});
```

## history
history对象允许访问浏览器曾经的会话历史记录

history和hash目前是vue、react等框架实现路由的底层原理

有两个属性：

+ length：会话中的记录条数
+ state：当前保留的状态值

有五个方法：

+ back()：返回上一页，等价于history.go(-1)
+ forward()：前进下一页，等价于history.go(1)
+ go()：加载历史中的某一页
+ pushState()：打开一个指定的地址
    - 三个参数：状态对象、标题（目前大多浏览器忽略此参数）、URL（相对或绝对URL）
    - `history.pushState({ page: 1 }, "Title", "/page1");`
+ replaceState()：打开一个新的地址，并且替换当前历史记录条目
    - 三个参数，与pushState()类似
    - `history.replaceState({ page: 2 }, "New Title", "/page2")`

## navigator(了解)
navigator对象是JavaScript中的一个全局对象，它提供了关于浏览器环境和用户设备的信息。这些信息包括浏览器的类型、版本、所在平台、语言偏好、网络连接状态等

## screen(了解)
screen是JavaScript中的另一个全局对象，它提供了关于用户屏幕的信息，包括分辨率、色深等

## Storage
WebStorage主要提供了一种机制，可以让浏览器提供一种比cookie更直观的key、value存储方式

+ localStorage：本地存储，提供的是一种永久性的存储方法，在关闭掉网页重新打开时，存储的内容依然保留
+ sessionStorage：会话存储，提供的是本次会话的存储，在关闭掉会话时，存储的内容会被清除

两者的区别

+ 关闭网页后重新打开，localStorage会保留，而sessionStorage会被删除
+ 在页面内实现跳转，localStorage会保留，sessionStorage也会保留
+ 在页面外实现跳转（打开新的网页），localStorage会保留，sessionStorage不会被保留

### 常见的方法和属性
属性

+ Storage.length：只读属性，返回一个整数，表示存储在Storage对象中的数据项数量

方法

+ Storage.key()：该方法接受一个数值n作为参数，返回存储中的第n个key名称
+ Storage.getItem()：该方法接受一个key作为参数，并且返回key对应的value
+ Storage.setItem()：该方法接受一个key和value，并且将会把key和value添加到存储中，如果key存储，则更新其对应的值
+ Storage.removeItem()：该方法接受一个key作为参数，并把该key从存储中删除
+ Storage.clear()：该方法的作用是清空存储中的所有key

# JSON
## 简介
在目前的开发中，JSON是一种非常重要的数据格式，它并不是编程语言，而是一种可以在服务器和客户端之间传输的数据格式。

+ JSON的全称是JavaScript Object Notation（JavaScript对象符号），是由Douglas Crockford构想和设计的一种轻量级资料交换格式，算是JavaScript的一个子集
+ 虽然JSON被提出来的时候是主要应用JavaScript中，但是目前已经独立于编程语言，可以在各个编程语言中使用，很多编程语言都实现了将JSON转成对应模型的方式

其他的传输格式：

+ XML：在早期的网络传输中主要是使用XML来进行数据交换的，但是这种格式在解析、传输等各方面都弱于JSON，所以目前已经很少在被使用了
+ Protobuf：另外一个在网络传输中目前已经越来越多使用的传输格式是protobuf，但是直到2021年的3.x版本才支持JavaScript，所以目前在前端使用的较少

使用场景

+ 网络数据的传输JSON数据
+ 项目的某些配置文件
+ 非关系型数据库（NoSQL）将json作为存储格式

## 基本语法
1. 数据格式：JSON数据是用键值对的方式表示的，类似于JavaScript中的对象，数据由花括号 {} 包围，键值对之间使用逗号 , 分隔。
2. 键值对：键和值之间使用冒号 : 分隔，键必须是一个字符串（需要用双引号 "" 包围），而值可以是任意合法的JSON数据类型。
3. 数据类型：
    - 对象：用花括号 {} 表示，对象中的数据是无序的键值对集合
    - 数组：用方括号 [] 表示，数组中的元素按顺序排列，可以是任意类型的值，包括对象和其他数组
    - 字符串：用双引号 "" 包围，表示文本数据
    - 数字：表示数值，可以是整数或浮点数，不使用引号
    - 布尔值：表示true或false
    - null：表示空值
4. 注意
    - 键名区分大小写，例如 "name" 和 "Name" 是不同的键
    - JSON不支持注释

```json
{
  "name": "John Doe",
  "age": 30,
  "isStudent": false,
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "zip": "12345"
  },
  "phoneNumbers": ["123-456-7890", "456-789-0123"]
}
```

## 序列化
某些情况下我们希望将JavaScript中的复杂类型转化成JSON格式的字符串，这样方便对其进行处理，比如我们希望将一个对象保存到localStorage中，但是如果我们直接存放一个对象，这个对象会被转化成 [object Object] 格式的字符串，并不是我们想要的结果

在ES5中引用了JSON全局对象，该对象有两个常用的方法：

### stringify
stringify方法：将JavaScript类型转成对应的JSON字符串，接收参数 replacer

+ 如果指定了一个 replacer 函数，则可以选择性地替换值
+ 如果指定的 replacer 是数组，则可选择性地仅包含数组指定的属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719480704433-39c3c291-39dd-4d43-b409-17c6fe838a00.png)

### parse
parse方法：解析JSON字符串，转回对应的JavaScript类型

`JSON.parse(text[, reviver])`

text：必选参数，要被解析的JSON字符串

reviver：可选参数，如果是一个函数，则可以转换解析后的每个属性值，用于进一步控制解析过程

```javascript
const jsonStr = '{"name": "John", "age": 30, "city": "New York"}';

const obj = JSON.parse(jsonStr);
console.log(obj); // 输出：{ name: 'John', age: 30, city: 'New York' }
```

