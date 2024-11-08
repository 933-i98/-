```vue
    <div id="app"></div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            template: `<h1>{{title}}</h1>`, 
            data: function () {
                return {
                    title: "hello practicer"
                }
            }
        })
        app.mount('#app')
    </script>
```

# Options API
## v-once
```vue
<div id="app">
        <h2 v-once>点击次数：{{number}}</h2>
        <button @click="add">+1</button>
</div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    number: 1
                }
            },
            methods: {
                add: function () {
                    this.number++
                }
            }
        })
        app.mount('#app')
    </script>
```

## v-text
```vue
<div id="app">
    <h2 v-text="number">abc</h2>
</div>
  <script src="./lib/vue.js"></script>
  <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    number: 1
                }
            },
        })
        app.mount('#app')
  </script>
```

## v-html
默认情况下，如果我们展示的内容本身是 html 的，那么vue并不会对其进行特殊的解析。

如果我们希望这个内容被Vue可以解析出来，那么可以使用 v-html 来展示；

```vue
<div id="app">
        <h2 v-html="info"></h2>
</div>
<script src="./lib/vue.js"></script>
<script>
        const app = Vue.createApp({
            data: function () {
                return {
                    info: `<h2>你好</h2>`
                }
            },
        })
        app.mount('#app')
</script>
```

## v-pre
用于跳过编译某个部分的模板内容

```vue
<template>
  <div>
    <span v-pre>{{ this will not be compiled }}</span>
  </div>
</template>
```

## v-cloak
直到加载完才显示

```vue
<div id="app">
        <h2 v-cloak>{{msg}}</h2>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    msg: 1
                }
            },
        })
        app.mount('#app')
    </script>
```

## v-bind
绑定一个或多个属性值，或者向另一个组件传递props值；

在开发中，有以下属性需要动态进行绑定，图片的链接src、网站的链接href、动态绑定一些类、样式

```javascript
<div id="app">
  <img :src="url" alt="">
</div>
  <script src="./lib/vue.js"></script>
  <script>
  const app = Vue.createApp({
  data: function () {
    return {
      url: "https://www.csdn.net/"
    }
  },
})
app.mount('#app')
</script>
```

### 绑定class
#### 对象语法
```javascript
<template id="my-app">
  //1.普通的绑定方式
  <div :class="classes"></div>
  //2.对象式绑定
  <div class="why" :class="{nba:true,'james':true}"></div>
  //4.绑定对象
  <div :class="classObj">哈哈哈</div>
  //5.从methods中获取
  <div :class="getClassObj()">呵呵呵</div>
</template>
```

#### 数组语法
```javascript
<template id="my-app">
  //1.直接传入一个数组
  <div :class="['why'，nba]">哈哈哈</div>
  //2.数组中也可以使用三元运算符或者绑定变量
  <div :class="['why',nba,isActive? 'active': '']">呵呵呵</div>
  //3.数组中也可以使用对象语法
  <div	:class="['why'，nba,{'actvie': isActive}]">嘻嘻嘻</div>
</template>
```

### 绑定style
#### 对象语法
```vue
<template id="my-app">
  <!-- 1.基本使用：传入一个对象，并且对象内容都是确定的,注意驼峰命名法 -->
  <div :style="{color:'red',fontSize:'30px','background-color':'blue'}">{{message}}</div>
  <!-- 2.变量数据：传入一个对象，值会来自于data -->
  <div :style="{color:'red',fontSize:size+'px','background-color':'blue'}">{{message}}</div>
  <!-- 3.对象数据：直接在data中定义好对象在这里使用 -->
  <div :style="styleobj">{{message}}</div>
</template>
```

#### 数组语法
```vue
<template>
  <div :style="[styleObj1,styleObj2]">{{msg}}</div>
</template>
```

### 动态绑定属性
如果属性名称不是固定的，我们可以使用 :[属性名]=“值” 的格式来定义

```vue
<template>
  <ul>
    <li v-for="(item, index) in itemList" :key="index">
      <div :[item.type]="item.value">{{ item.label }}</div>
    </li>
  </ul>
</template>

<script>
  export default {
    data() {
      return {
        itemList: [
          { type: 'class', value: 'red', label: 'Red Item' },
          { type: 'style', value: 'font-weight: bold;', label: 'Bold Item' },
        ]
      };
    }
  };
</script>
```

### 绑定一个对象
将一个对象的所有属性，绑定到元素上的所有属性

```vue
<template>
  <div v-bind="info">{{msg}}</div>
</template>
```

## v-on
绑定事件(比如点击、拖拽)

### 基本使用
```javascript
<body>
    <div id="app">
        <h2>{{counter}}</h2>
        <button @click="add">Nihao</button>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    counter: 0
                }
            },
            methods: {
                add() {
                    this.counter++
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

""里面可以是表达式

```javascript
    <div id="app">
        <h2>{{counter}}</h2>
        <button @click="counter++">Nihao</button>
    </div>
```

### 传递参数
```javascript
<body>
    <div id="app">
        <button @click="click1('jack will',21)">年龄</button>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {}
            },
            methods: {
                click1(name, age) {
                    console.log(name, age)
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

传递的参数也可以是data中的变量(不带引号)

```vue
<body>
  <div id="app">
    <button @click="click1('jack will',age)">年龄</button>
  </div>
<script src="./lib/vue.js"></script>
    <script>
      const app = Vue.createApp({
        data: function () {
          return {
            age: 21
          }
        },
        methods: {
          click1(name, age) {
            console.log(name, age)
          }
        }
      })
      app.mount('#app')
    </script>
</body>
```

在 Vue.js 中，$event 是一个特殊的参数，它表示触发事件时生成的事件对象。通过使用 $event，您可以在处理事件的方法中访问事件对象，从而获取有关事件的信息，比如鼠标位置、键盘按键等。

例如，当在一个按钮的点击事件处理方法中使用 $event 时，您可以获取触发点击事件的按钮元素、鼠标点击位置等相关信息。

```vue
<template>
  <button @click="handleClick($event)">Click me</button>
</template>

<script
  methods: {
    handleClick(event) {
      // 通过$event参数访问事件对象，也可以直接使用event参数
      console.log('按钮被点击了');
      console.log('触发事件的元素:', event.target);
      console.log('鼠标点击的位置:', event.clientX, event.clientY);
    }
  }
}
</script>
```



### 修饰符
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1706951761062-ace87d66-663a-4894-9a39-0baeba066244.png)

```javascript
<div id="app">
    <button @click.stop="click1">年龄</button>
</div>
```

## v-if
```javascript
    <div id="app">
        <h2 v-if="score>90">优秀</h2>
        <h2 v-else-if="score>80">良好</h2>
        <h2 v-else-if="score>60">普通</h2>
        <h2 v-else>不及格</h2>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return { score: 61 }
            }
        })
        app.mount('#app')
    </script>
```

```javascript
<body>
    <div id="app">
        <h2 v-if="isShow">你好</h2>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    isShow: false
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

如果我们希望切换的是多个元素呢,此时我们渲染div，但是我们并不希望div这种元素被渲染；这个时候，我们可以选择使用template；template元素可以当做不可见的包裹元素，并且在v-if上使用，但是最终template不会被渲染出来

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1706953155862-12260eba-80ef-49cf-88f1-0ccf012c583e.png)

## v-show
v-show和v-if的用法看起来是一致的，也是根据一个条件决定是否显示元素或者组件：

```javascript
<div id="app">
   <h2 v-show="false">你好</h2>
</div>
```

和v-if的区别

+ 首先，在用法上的区别：
    - v-show是不支持template；
    - v-show不可以和v-else一起使用；
+ 其次，本质的区别：
    - v-show元素无论是否需要显示到浏览器上，它的DOM实际都是有存在的，只是通过CSS的display属性来进行切换；
    - v-if当条件为false时，其对应的原生压根不会被渲染到DOM中；
+ 开发中如何进行选择
    - 如果我们的原生需要在显示和隐藏之间频繁的切换，那么使用v-show；
    - 如果不会频繁的发生切换，那么使用v-if；

## v-for
### 基本使用
v-for="item in 数组"

+ 数组通常是来自data或者prop，也可以是其他方式；
+ item是我们给每项元素起的一个别名，这个别名可以自定来定义

需要索引，可以使用格式： "(item, index) in 数组"，注意顺序：数组元素项item是在前面的，索引项index是在后面的

```javascript
<body>
    <div id="app">
        <h2>歌曲列表</h2>
        <ul>
            <li v-for="songs in list">{{songs}}</li>
            //位置不能错，给li添加v-for，而不是ul
        </ul>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    list: ['天天', '就是爱你', '流沙']
                    //注意加引号
                    //数组中也可以是对象
                    //list: [{ id:1,name:'david'}]
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

```javascript
<body>
    <div id="app">
        <h2>歌曲列表</h2>
        <ul>
            <li v-for="(songs,index) in list">{{index}}-{{songs}}</li>
            //index是从零开始，可以{{index+1}}从1开始
        </ul>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    list: ['天天', '就是爱你', '流沙']
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

```vue
<body>
    <div id="app">
        <h2>歌曲列表</h2>
        <ul>
            <li v-for="someone in list">
                <h3>{{someone.id}}</h3>
                <h3>{{someone.name}}</h3>
            </li>
        </ul>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    list: [
                        { id:1, name: 'david' },
                        { id:2, name: 'jack' },
                        { id:3, name: 'will' }
                    ]
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

遍历对象

```vue
<body>
    <div id="app">
        <h2>歌曲列表</h2>
        <ul>
            <li v-for="(value,key,index) in list">
                {{index}}-{{key}}-{{value}}
            </li>
        </ul>
    </div>
    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    list: { id: 1, name: 'jack will' }
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

### 数据更新检测
如果要v-for绑定的数组发生变化，有两种方法

+ 直接将数组修改为一个新数组：this.names = [new array]
+ 通过数组的方法：
    - push()、pop()、shift()、unshift()、splice()、sort()、reverse()
    - 有的不会修改原来的数组，而是会生成新的数组，比如 filter()、concat() 和 slice()；

### v-for中的key
在使用v-for进行列表渲染时，我们通常会给元素或者组件绑定一个key属性，这个 key的作用是帮助 Vue 识别列表中的每一项，从而在数据发生变化时更高效地更新 DOM。

具体来说，使用 v-for 渲染列表时，给每个项提供一个 key 可以帮助 Vue 识别每个子组件的身份，从而在数据更新时，尽可能地复用已有的 DOM 元素而不是销毁再重建，以提高性能。另外，给 v-for 提供 key 也能帮助 Vue 在进行列表项的状态维护时，更准确地定位到对应的列表项，从而减少不必要的重新渲染。

```vue
    <div id="app">
        <h2>歌曲列表</h2>
        <ul>
            <li v-for="songs in list" :key="id">{{songs}}</li>
        </ul>
    </div>
```

## 计算属性computed
在某些情况，我们可能需要对数据进行一些转化后再显示，或者需要将多个数据结合起来进行显示。比如我们需要对多个data数据进行运算、三元运算符来决定结果、数据进行某种转化后显示；

```vue
<body>
    <div id="app">
        <h2>{{fullName}}</h2>
        <h2>{{result}}</h2>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    firstName: 'sual',
                    lastName: 'goodman',
                    score: 60
                }
            },
            computed: {
                fullName() {
                    return this.firstName + this.lastName;
                },
                result() {
                    return this.score >= 60 ? '及格' : '不及格';
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

在数据不发生变化时，计算属性是不需要重新计算的；但是如果依赖的数据发生变化，在使用时，计算属性依然会重新进行计算。

上面是语法糖写法(只有get)，下面是完整写法

```vue
computed: {
  fullName: {
    get: function () {
      return this.firstName + this.lastName
    },
    set: function (value) {
      const scores = value
    }
  }
}
```

## 侦听器watch
```vue
const app = Vue.createApp({
  data: function () {
    return {
      score: 60
  },
  watch: {
    score(){
      console.log('分数变了')
    }
  }
})
```

通过2个参数可以拿到旧值和新值

```vue
watch: {
  score(newValue, oldValue) {
    console.log('分数变了', newValue, oldValue)
  }
}
```

默认情况下，watch对于内部属性的变化是不会做出响应的，可以使用选项deep进行更深层的侦听

immediate选项：这个时候无论后面数据是否有变化，侦听的函数都会有限执行一次；

```vue
watch: {
  info: {
    score(newValue, oldValue) {
      console.log('分数变了', newValue, oldValue)
    },
    deep: true,
    immediate: true
  }
}
```

## 表单V-model
v-model指令可以在表单 input、textarea以及select元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素；

```vue
<body>
    <div id="app">
        <input type="text" v-model="message">
        <h1>{{message}}</h1>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    message: '你好'
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

v-model的原理其实是背后有两个操作：

+ v-bind绑定value属性的值；
+ v-on绑定input事件监听到函数中，函数会获取最新的值赋值到绑定的属性中；

```vue
<!-- 单选 true/false -->
<body>
    <div id="app">
        <label for="agreement">
            <input id="agreement" type="checkbox" v-model="isAgree">
        </label>
        <h1>{{isAgree}}</h1>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    isAgree: false
                }
            }
        })
        app.mount('#app')
    </script>
</body>

<!-- 多选 选中了几个 -->
  
<body>
    <div id="app">
        <label for="篮球">
            <input id="篮球" type="checkbox" value="篮球" v-model="hobbies">篮球
        </label>
        <label for="足球">
            <input id="足球" type="checkbox" value="足球" v-model="hobbies">足球
        </label>
        <label for="棒球">
            <input id="棒球" type="checkbox" value="棒球" v-model="hobbies">棒球
        </label>
        <h2>{{hobbies}}</h2>
    </div>、

    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    hobbies: []
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

```vue
<!-- 当我们选中option中的一个时，会将它对应的value赋值到fruit中 -->
<body>
    <div id="app">
        <select v-model="fruit">
            <option value="apple">apple</option>
            <option value="orange">orange</option>
            <option value="banana">banana</option>
        </select>
        <h2>{{fruit}}</h2>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    fruit: []
                }
            }
        })
        app.mount('#app')
    </script>
</body>

// 当选中多个值时，就会将选中的option对应的value添加到数组fruit中
<body>
    <div id="app">
        <select v-model="fruit" multiple size="">
            <option value="apple">apple</option>
            <option value="orange">orange</option>
            <option value="banana">banana</option>
        </select>
        <h2>{{fruit}}</h2>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data: function () {
                return {
                    fruit: []
                }
            }
        })
        app.mount('#app')
    </script>
</body>
```

默认情况下，v-model在进行双向绑定时，绑定的是input事件，那么会在每次内容输入后就将最新的值和绑定的属性进行同步；如果在v-model后跟上lazy修饰符，那么会将绑定的事件切换为 change 事件，只有在提交时（比如回车）才会触发；

```vue
    <div id="app">
        <input type="text" v-model.lazy="message">
        <h2>{{message}}</h2>
    </div>
```

message总是string类型，即使在我们设置type为number也是string类型；如果我们希望转换为数字类型，那么可以使用 .number 修饰符：

```vue
<input type="text" v-model.number="message">
```

如果要自动过滤用户输入的守卫空白字符，可以给v-model添加 trim 修饰符

```vue
<input type="text" v-model.trim="message">
```

# 组件
## 全局组件
注册组件用app.component()，第一个参数是组件的名

```vue
<body>
    <div id="app">
        <zujian1></zujian1>
        <zujian1></zujian1>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const App = {}
        const productItem1 = {
            template: `<h1>我是组件1</h1>`
        }
        const app = Vue.createApp(App)
        app.component('zujian1', productItem1)
        app.mount('#app')
    </script>
</body>
```

组件内部可以有template、data等方法

```vue
<body>
    <div id="app">
        <zujian1></zujian1>
        <zujian1></zujian1>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const App = {}
        const productItem1 = {
            template: `<h1>我是组件1</h1>`,
            data(){
                return{
                    title:'title1'
                }
            },
            methods:{
                btnClick(){
                    console.log('点击')
                }
            }
        }
        const app = Vue.createApp(App)
        app.component('zujian1', productItem1)
        app.mount('#app')
    </script>
</body>
```

## 局部组件
全局组件往往是在应用程序一开始就会全局组件完成，那么就意味着如果某些组件我们并没有用到，也会一起被注册：

比如我们注册了三个全局组件：ComponentA、ComponentB、ComponentC。在开发中我们只使用了ComponentA、ComponentB，如果ComponentC没有用到但是我们依然在全局进行了注册，那么就意味着类似于webpack这种打包工具在打包我们的项目时，我们依然会对其进行打包；这样最终打包出的JavaScript包就会有关于ComponentC的内容，用户在下载对应的JavaScript时也会增加包的大小；

所以在开发中我们通常使用组件的时候采用的都是局部注册

局部组件命名时可以驼峰命名也可以-命名，但是使用时需要-格式

```vue
<body>
    <div id="app">
        <h1>{{age}}</h1>
        <button @click="add"></button>
        <zujian></zujian>
        <product-del></product-del>
    </div>

    <script src="./lib/vue.js"></script>
    <script>
        const app = Vue.createApp({
            data() {
                return {
                    age: 20
                }
            },
            methods: {
                add() {
                    this.age++
                }
            },
            components: {
                'product-del': productDel
            }
        })
        const productDel = {
            template: `<h1>组件3</h1>`
        }
        const productItem = {
            template: `<h1>组件1</h1>`,
        }
        app.component('zujian', productItem)
        app.mount('#app')
    </script>
</body>
```

## vue开发模式
可以通过一个后缀名为 .vue 的single-file components (SFC单文件组件) 来解决逻辑复杂的问题，并且可以使用webpack或者vite或者rollup等构建工具来对其进行处理。

使用SFC的方法有两种

+ 使用vue cli(脚手架)
+ 使用webpack、rollup、vite等打包工具

### 脚手架vue cli
CLI是Command-Line Interface, 翻译为命令行界面。可以通过CLI选择项目的配置和创建出我们的项目。Vue CLI已经内置了webpack相关的配置，用户不需要从零来配置；

+ 安装：npm install @vue/cli -g
+ 升级：npm update @vue/cli -g
+ 创建：vue create 项目名

还有一种安装方式，正在逐渐普及：npm init vue@latest



## 组件间通信
### 父传子
可以通过props来完成组件之间的通信。

Props你可以在组件上注册一些自定义的attribute。父组件给这些attribute赋值，子组件通过attribute的名称获取到对应的值

+ 方式一：字符串数组，数组中的字符串就是attribute的名称；
+ 方式二：对象类型，对象类型我们可以在指定attribute名称的同时，指定它需要传递的类型、是否是必须的、默认值等等；



数组用法

```vue
<template>
  <div>
    <son-prac name="ffz" age="16"></son-prac>
  </div>
</template>


<script>
import sonPrac from './components/son.vue'

export default {
  components: {
    sonPrac
  }
}
</script>

<style scpoed></style>
```

```vue
<template>
  <div>
    <h2>{{ name }}</h2>
    <h2>{{ age }}</h2>
  </div>
</template>

<script>
export default {
  props: ['name', 'age']
}
</script>

<style scoped></style>
```



对象用法

+ 指定传入的attribute的类型；
    - String
    - Number
    - Boolean
    - Array
    - Object
    - Date
    - Function
    - Symbol
+ 指定传入的attribute是否是必传的；
+ 指定没有传入时，attribute的默认值；

```vue
<script>
export default {
  props:{
    name:{
      type:String,
      required:true,
      default:'张三'
    }
  }
}
</script>
```

```vue
export default {
  props: {
    name: String,
    age: age,
    sex: {
      type: String,
      required: ture
    },
    propF:{
      validator(value){
        return['case1','case2'].includes(value)
      }
    }
  }
}
```

当我们传递给一个组件某个属性，但是该属性并没有定义对应的props或者emits时，就称之为 非Prop的Attribute，常见的包括class、style、id属性等。

当组件有单个根节点时，非Prop的Attribute将自动添加到子组件根元素的Attribute中



如果我们不希望组件的根元素继承attribute，可以在组件中设置 inheritAttrs: false

+ 禁用attribute继承的常见情况是需要将attribute应用于根元素之外的其他元素；
+ 可以通过 $attrs来访问所有的非props的attribute

```vue
<template>
  <div>
    <h2 :class="$attrs.sex">{{ name }}</h2>
    <h2>{{ age }}</h2>
  </div>
</template>
```

### 子传父
1. 在子组件中定义好在某些情况下触发的事件名称；
2. 在父组件中以v-on的方式传入要监听的事件名称，并且绑定到对应的方法中；
3. 在子组件中发生某个事件的时候，根据事件名称触发对应的事件；

子组件使用this.$emit("参数1",参数2)，父组件在使用<子组件>时加上   @参数1=父组件methods   即可

+ 参数1是自定义事件的名称
+ 参数2是传递的参数
+ 参数count记得在函数()中表明
+ 在子组件中可以加入一个emits:['add','de']的api，方便他人阅读

```vue
<template>
  <div>
    <button @click="addCounter(1)">+1</button>
    <button @click="addCounter(5)">+5</button>
    <button @click="addCounter(10)">+10</button>
  </div>
</template>

<script>
export default {
  emits:['add'],
  //在子组件中可以加入一个emits:['add','de']的api，方便他人阅读
  methods: {
    addCounter(count) {
      this.$emit('addclick', count)
    }
  }
}
</script>
```

```vue
<template>
  <div>
    <h2>{{ counter }}</h2>
    <add-click @addclick="addBtn"></add-click>
  </div>
</template>


<script>
import addClick from './components/son.vue'

export default {
  components: {
    addClick
  },
  data() {
    return {
      counter: 0
    }
  },
  methods: {
    addBtn(count) {
      this.counter += count
    }
  }
}
</script>
```

### 插槽slot
#### 基本使用
插槽的使用过程其实是抽取共性、预留不同：将共同的元素、内容依然在组件内进行封装，同时将不同的元素使用slot作为占位，让外部决定到底显示什么样的元素

```vue
<template>
  <div>
    <h2>{{title}}</h2>
    <slot></slot>
  </div>
</template>
```

```vue
<template>
  <div>
    <son>
      <button></button>
    </son>
  </div>
</template>
```

#### 插槽的默认内容
在使用插槽时，如果没有插入对应的内容，那么需要显示一个默认的内容

```vue
<template>
  <div>
    <h2>{{title}}</h2>
    <slot>
      <h2>我是默认内容</h2>
    </slot>
  </div>
</template>
```

#### 多个插槽
如果一个组件中含有多个插槽，默认情况下每个插槽都会获取到插入的内容来显示；

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708857753082-a26fa18e-4a25-4374-8ac7-7156cc42e53f.png)

#### 具名插槽
具名插槽顾名思义就是给插槽起一个名字，<slot> 元素有一个特殊的 attribute：name；

一个不带 name 的slot，会带有隐含的名字 default；

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708857845392-fa961f0f-bdc4-476f-8917-f1b68d34e266.png)

跟 v-on 和 v-bind 一样，v-slot 也有缩写，即把参数之前的所有内容 (v-slot:) 替换为字符 #

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708857958341-c841f3e3-5a68-48b6-929f-d4c1e5ae0a9f.png)

#### 动态插槽名
可以通过 v-slot:[name]方式动态绑定一个名称；

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708857902205-526e9223-35ee-4ec4-949f-a0e13c396d91.png)

#### 插槽作用域
渲染作用域示例：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708858012499-ca2f0611-9b38-470d-a91d-0c43c6a8de88.png)

但是有时候希望插槽可以访问到子组件中的内容

比如：一个组件被用来渲染一个数组元素时，我们使用插槽，并且希望插槽中没有显示每项的内容；

有两种方法

1. 子元素把数据传递给父元素，父元素在 slot 中使用传过来的数据
2. 使用 v-slot:default="slotProps"，可以简写为v-slot="slotProps"

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726739148895-e8cd5cbc-3621-4003-9b98-eda0f7455d81.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726740707741-27168dc9-3402-4bd7-beb6-bf46fae1f50a.png)

#### 默认插槽和具名插槽混合
如果同时有默认插槽和具名插槽，那么按照完整的template来编写。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708906937002-e2ead617-bd59-4467-a3a5-6ad31e6a3567.png)

 只要出现多个插槽，每个插槽都要有<template> ：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708906955310-d09e3bee-f545-4fd7-b06a-b3ab3848780c.png)

### 非父子组件的通信
#### 全局事件总线
全局事件总线mitt库：Vue3从实例中移除了 $on、$off 和 $once 方法，所以如果想继续使用全局事件总线，要通过第三方的库：例如 mitt 或 tiny-emitter；

在这里使用的是 hy-event-store

安装hy-event-store：npm install hy-event-store，然后新建一个js文件，就可以引入使用了![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708910603846-011765f2-099f-4ae4-b823-97e6cf2c1b21.png)

在A组件中使用eventBus.on监听，在B组件中使用eventBus.emit触发

```vue
<script>
import son from './components/son.vue'
import eventBus from './event-bus'

export default {
  components: {
    son
  },
  created() {
    eventBus.on('bbtnClick', (age, number) => {
      console.log(age, number);

    })
  }
}
</script>
```

```vue
<template>
  <div>
    <button @click="btnClick">点击</button>
  </div>
</template>

<script>
import eventBus from '../event-bus';

export default {
  methods: {
    btnClick() {
      eventBus.emit('bbtnClick', 'age', 18)
    }
  }
}
</script>
```

如果想取消注册的函数监听，在A组件中使用eventBus.off取消监听

```vue
<script>
import son from './components/son.vue'
import eventBus from './event-bus'

export default {
  components: {
    son
  },
  methods: {
    eventListener() {
      console.log('你好');
    }
  },
  created() {
    eventBus.on('bbtnClick', this.eventListener)
  },
  unmounted() {
    eventBus.off('bbtnClick', this.eventListener)
  }
}
</script>
```

#### Provide/inject
```vue
<template>
  <div>
    <hello-hi>
    </hello-hi>
  </div>
</template>


<script>
import helloHi from './components/son.vue'

export default {
  components: {
    helloHi
  },
  provide: {
    name: 'jillwick',
    age: 18
  }
}
</script>

<style scpoed></style>
```

```vue
<template>
  <div>
    <h2>{{ name }}-{{ age }}</h2>
  </div>
</template>

<script>
export default {
  inject: ['name', 'age']
}
</script>

<style scoped></style>
```

 如果Provide中提供的一些数据是来自data，那就需要通过this来获取，这时候需要吧provide写成函数，在通过this获取数据

```vue
<script>
import helloHi from './components/son.vue'

export default {
  components: {
    helloHi
  },
  data() {
    return {
      names: ['jill', 'kobe'],
      age: 18
    }

  },
  provide() {
    return {
      name: 'ko',
      age: 18,
      length: this.names.length
    }

  }
}
</script>
```

```vue
<template>
  <div>
    <h2>{{ name }}-{{ age }}-{{ length }}</h2>
  </div>
</template>

<script>
export default {
  inject: ['name', 'age', 'length']
}
</script>

<style scoped></style>
```

如果修改了this.names的内容，使用length的子组件不是响应式的，如果想变成响应式，需要使用computed函数(后面讲)

```vue
<script>
import helloHi from './components/son.vue'

export default {
  components: {
    helloHi
  },
  data() {
    return {
      names: ['jill', 'kobe'],
      age: 18
    }

  },
  provide() {
    return {
      name: 'ko',
      age: 18,
      length: computed(() => this.names.length)
    }

  }
}
</script>
```

## 组件化额外知识
### 生命周期
每个组件都可能会经历从创建、挂载、更新、卸载等一系列的过程。在这个过程中的某一个阶段，我们可能会想要添加一些属于自己的代码逻辑（比如组件创建完后就请求一些服务器数据）。但是如何可以知道目前组件正在哪一个过程呢？Vue提供了组件的生命周期函数；

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708993892332-883f8bd1-7d13-43fe-ac2e-e6ff9442a5d7.png)

1. App/Home/Banner/ShowMessaae
    1. beforeCreate
2. 创建组件实例
    1. created（重要：1.发送网络请求2.事件监听3.this.Swatch0）
3. template模板编译
    1. beforeMount
4. 挂载到虚拟DOM-虚拟DOM->真实的DOM->界面看到h2/div
    1. mounted（重要：元素已经被挂载获取DOM，使用DOM）
5. 数据更新：message改变
    1. beforeUpdate
6. 根据最新数据生成新的VNode生成新的虚拟DOM->真实的DOM
    1. updated
7. 不再使用 v-if='false'
    1. beforeUnmount
8. 将之前挂载在虚拟DOM中VNode从虚拟DOM移除
    1. unmounted（相对重要：回收操作（取消事件监听)）
9. 将组件实例销数掉

### 在Vue中获取DOM元素
某些情况下，在组件中想要直接获取到元素对象或者子组件实例，在Vue开发中是不推荐主动的获取DOM并且修改DOM内容

如果一定要获取DOM元素，可以给元素或者组件绑定一个ref的attribute属性；

通过this.$refs.ref名就可以获取对应元素，在进行DOM操作即可

```vue
<template>
  <div>
    <h2 ref="msg">{{ message }}</h2>
    <button @click="change">修改</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'hello'
    }
  },
  methods: {
    change() {
      this.$refs.msg.textContent = 'hi'
    }
  }
}
</script>
```

可以通过$parent来访问父元素

```vue
console.log(this.$parent.message);
```

如果想要根组件的DOM元素，可以通过$root来实现，因为App是我们的根组件

```vue
console.log(this.$root.message);
```

### 动态组件
动态组件是使用 component 组件，通过一个特殊的attribute-:is 来实现

:is后面的组件要么是全局注册的组件组件，要么是局部注册的组件

:is后面也可以跟数组等

```vue
<template>
  <div>
    <button v-for="tab in tabs" 
    :key="tab" 
    :class="{ active: currentTab === tab }" 
    @click="tabclick(tab)">{{ tab }}
    </button>
    <component :is="currentTab"></component>
  </div>
</template>

<script>
import currentTab from'./components/son.vue'
export default {
  components:{
    currentTab
  }
}
</script>
```

动态组件也可以传递参数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708996694297-8503e2d1-8d0b-4d5f-af02-a6d905b5426b.png)

### keep-alive
在其中增加了一个按钮，点击可以递增的功能。比如我们将counter点到10，那么在切换到home再切换回来about时，状态是否可以保持呢？答案是否定的。这是因为默认情况下，我们在切换组件后，about组件会被销毁掉，再次回来时会重新创建组件

但是，在开发中某些情况我们希望继续保持组件的状态，而不是销毁掉，这个时候我们就可以使用一个内置组件：keep-alive

keep-alive有一些属性：

+ include - string | RegExp | Array。只有名称匹配的组件会被缓存；
+ exclude - string | RegExp | Array。任何名称匹配的组件都不会被缓存；
+ max - number | string。最多可以缓存多少组件实例，一旦达到这个数字，那么缓存组件中最近没有被访问的实例会被销毁；

```vue
<template>
  <div>
    <keep-alive include="a,b">
      <component :is="view"></component>
    </keep-alive>
    <keep-alive :include="/a|b/">
      <component :is="view"></component>
    </keep-alive>
    <keep-alive :include="['a', 'b']">
      <component :is="view"></component>
    </keep-alive>
  </div>
</template>
```

### 缓存组件的生命周期
对于缓存的组件来说，再次进入时，我们是不会执行created或者mounted等生命周期函数的：但是有时候我们确实希望监听到何时重新进入到了组件，何时离开了组件；这个时候我们可以使用activated 和 deactivated 这两个生命周期钩子函数来监听；

### Vue中实现异步组件
如果我们的项目过大了，对于某些组件我们希望通过异步的方式来进行加载（目的是可以对其进行分包处理），那么Vue提供了一个函数：defineAsyncComponent

defineAsyncComponent接受两种类型的参数：

+ 类型一：工厂函数，该工厂函数需要返回一个Promise对象
+ 类型二：接受一个对象类型，对异步函数进行配置

```vue
<script>
import defineAsyncComponent from 'vue';
const AsyncCategory = defineAsyncComponent(() => import("./views/Category.vue"));
export default {
  components: {
    Category: AsyncCategory
  }
}
</script>
```

类型二：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1708997926254-283d250c-e072-44db-809f-f8591962fa06.png)

### 组件的v-model
在这个例子中，父组件中使用了v-model指令来将message的值与CustomInput组件进行了双向绑定。这样，当用户在CustomInput中输入内容时，message值会被同步更新，并反之亦然。

```vue
<template>
  <input :value="value" @input="$emit('input', $event.target.value)">
</template>

<script>
export default {
  props: {
    value: String
  }
};
</script>
```

```vue
<template>
 <div>
    <CustomInput v-model="message"></CustomInput>
    <p>当前输入内容: {{ message }}</p>
  </div>
</template>

<script>
import CustomInput from './CustomInput.vue';

export default {
  components: {
    CustomInput
  },
  data() {
    return {
      message: ''
    };
  }
};
</script>

```

### Mixin
#### 基本使用
是组件和组件之间有时候会存在相同的代码逻辑，我们希望对相同的代码逻辑进行抽取。在Vue2和Vue3中都支持的一种方式就是使用Mixin来完成

Mixin提供了一种非常灵活的方式，来分发Vue组件中的可复用功能。一个Mixin对象可以包含任何组件选项

当组件使用Mixin对象时，所有Mixin对象的选项将被混进入该组件本身的选项中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709000935880-1c66cd01-5754-4504-a158-dfbb5ddc2496.png)

#### Mixin的合并规则
如果Mixin对象中的选项和组件对象中的选项发生了冲突，那么Vue会分成不同的情况来进行处理；

+ 情况一：data函数的返回值对象
    - 返回值对象默认情况下会进行合并；
    - 如果data返回值对象的属性发生了冲突，那么会保留组件自身的数据；
+ 情况二：生命周期钩子函数
    - 生命周期的钩子函数会被合并到数组中，都会被调用；
+ 情况三：值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象。
    - 比如都有methods选项，并且都定义了方法，那么它们都会生效；
    - 但是如果对象的key相同，那么会取组件对象的键值对；

#### 全局混入Mixin
如果组件中的某些选项，是所有的组件都需要拥有的，那么可以使用全局的mixin：

+ 全局的Mixin可以使用应用app的方法 mixin 来完成注册；
+ 一旦注册，全局混入的选项将会影响每一个组件

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709001146097-6d3e327d-c6db-45fd-adbf-7ef31b6a968f.png)

# CompositionAPI
## OptionAPI的弊端
Options API的一大特点就是在对应的属性中编写对应的功能模块；比如data定义数据、methods中定义方法、computed中定义计算属性、watch中监听属性改变，也包括生命周期钩子

当我们实现某一个功能时，这个功能对应的代码逻辑会被拆分到各个属性中。当我们组件变得更大、更复杂时，逻辑关注点的列表就会增长，那么同一个功能的逻辑就会被拆分的很分散

这种碎片化的代码使用理解和维护这个复杂的组件变得异常困难，并且隐藏了潜在的逻辑问题。并且当我们处理单个逻辑关注点时，需要不断的跳到相应的代码块中

如果我们能将同一个逻辑关注点相关的代码收集在一起会更好。这就是Composition API想要做的事情，以及可以帮助我们完成的事情。也有人把Vue Composition API简称为VCA

## Setup
为了开始使用Composition API，我们需要有一个可以实际使用它（编写代码）的地方，在Vue组件中，这个位置就是 setup 函数。setup其实就是组件的另外一个选项，只不过这个选项强大到我们可以用它来替代之前所编写的大部分其他选项

+ setup函数要有返回值，需要返回定义的变量和函数名
+ setup函数定义的变量不是响应式的，需要在定义时使用vue自带的ref包裹
+ ref定义之后，变量值的改变需要用.value

```vue
<template>
  <div>
    <h2>{{ counter }}</h2>
    <button @click="add">+1</button>
  </div>
</template>


<script>
import { ref } from 'vue'
export default {
  setup() {
    const counter = ref(1);
    const add = () => {
      counter.value++
    }
    return {
      counter,
      add
    }
  }
}
</script>
```

它主要有两个参数：props和context

props非常好理解，它其实就是父组件传递过来的属性会被放到props对象中，我们在setup中如果需要使用，那么就可以直接通过props参数获取

+ 对于定义props的类型，我们还是和之前的规则是一样的，在props选项中定义
+ 并且在template中依然是可以正常去使用props中的属性，比如message
+ 在setup函数中想要使用props，不可以通过 this 去获取，因为props有直接作为参数传递到setup函数中，所以我们可以直接通过参数来使用即可

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726748914731-8875abf9-c74c-4b08-a788-dad501be8016.png)

另外一个参数是context，我们也称之为SetupContext，它里面包含三个属性：

+ attrs：所有的非prop的attribute
+ slots：父组件传递过来的插槽（在以渲染函数返回时会有作用）
+ emit：当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可以通过 this.$emit发出事件）

官方关于this有这样一段描述（这段描述是我给官方提交了PR之后的一段描述）

+ 表达的含义是this并没有指向当前组件实例
+ 并且在setup被调用之前，data、computed、methods等都没有被解析
+ 所以无法在setup中获取this

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726749090083-354f43e7-beab-43cc-8e0f-ff365c90c44d.png)

## Reactive API
### 基本使用
如果想为在setup中定义的数据提供响应式的特性，那么我们可以使用reactive的函数

reactive API对传入的类型是有限制的，它要求必须传入的是一个对象或者数组类型

```vue
import { reasctive } from 'vue'
const counter = reactive({
      name:'kobe',
      counter:100,
  });
```

这是因为当我们使用reactive函数处理我们的数据之后，数据再次被使用时就会进行依赖收集。当数据发生改变时，所有收集到的依赖都是进行对应的响应式操作（比如更新界面）。事实上，我们编写的data选项，也是在内部交给了reactive函数将其编程响应式对象的；

### 应用场景
+ 本地产生的数据
+ 多个数据之间是有关系的(组织在一起会有特定的作用)

### reactive判断的api
+ isProxy
    - 检查对象是否是由 reactive 或 readonly创建的 proxy
+ isReactive
    - 检查对象是否是由 reactive创建的响应式代理
    - 如果该代理是 readonly 建的，但包裹了由 reactive 创建的另一个代理，它也会返回 true
+ isReadonly
    - 检查对象是否是由 readonly 创建的只读代理
+ toRaw
    - 返回 reactive 或 readonly 代理的原始对象（不建议保留对原始对象的持久引用。请谨慎使用）
+ shallowReactive
    - 创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (深层还是原生对象)
+ shallowReadonly
    - 创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的）

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726749286800-4d2bbf78-5719-417b-98eb-54dfb9e9e675.png)

## Ref API
### 基本使用
ref 会返回一个可变的响应式对象，该对象作为一个响应式的引用维护着它内部的值，它内部的值是在ref的 value 属性中被维护的；

在模板中引入ref的值时，Vue会自动帮助我们进行解包操作，所以我们并不需要在模板中通过 ref.value 的方式来使用；

但是在 setup 函数内部，它依然是一个ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式。

### 应用场景
+ 定义本地的一些简单数据
+ 定义从网络中获取的数据

### ref其他的api
+ unref
    - 如果我们想要获取一个ref引用中的value，那么也可以通过unref方法：
    - 如果参数是一个 ref，则返回内部值，否则返回参数本身；
    - 这是 val = isRef(val) ? val.value : val 的语法糖函数；
+ isRef
    - 判断值是否是一个ref对象。
+ shallowRef
    - 创建一个浅层的ref对象；
+ triggerRef
    - 手动触发和 shallowRef 相关联的副作用

## readonly
readonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不能对其进行修改）

在开发中常见的readonly方法会传入三个类型的参数：

+ 普通对象
+ reactive返回的对象
+ ref的对象

readonly返回的对象都是不允许修改的，但是经过readonly处理的原来的对象是允许被修改的

比如 const info = readonly(obj)，info对象是不允许被修改的。当obj被修改时，readonly返回的info对象也会被修改，但是我们不能去修改readonly返回的对象info，本质上就是readonly返回的对象的setter方法被劫持了而已

```vue
import { ref,reactive } from 'vue'
export default {
  setup() {
    const info ={
      name:'jack',
      age:21
    }
    const s1 = readonly(info)

    const info1 = reactive({
      name:'will',
      age:54
    })
    const s2 = readonly(info1)

    const info2 =ref('soul')
    const s3 = readonly(info2)
  } 
}
```

## toRefs
如果我们使用ES6的解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive，返回的state对象，数据都不再是响应式的

那么有没有办法让我们解构出来的属性是响应式的呢？Vue为我们提供了一个toRefs的函数，可以将reactive返回的对象中的属性都转成ref，那么我们再次进行结构出来的 name 和 age 本身都是 ref的

这种做法相当于已经在state.name和ref.value之间建立了链接，任何一个修改都会引起另外一个变化

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726749730437-a4cfdefc-97e8-44e6-a35e-6c677390b276.png)

## toRef
如果我们只希望转换一个reactive对象中的属性为ref,，那么可以使用toRef的方法

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726749722282-ea6b6e03-6c38-4c7c-9221-9ecff8a60ba9.png)

## 在setup中使用ref
只需要定义一个ref对象，绑定到元素或者组件的ref属性上即可

通过 titleRef，你可以直接访问和操作 <h2> 元素

```vue
<template>
  <div>
    <h2 ref="titleRef"></h2>
  </div>
</template>


<script>
import { ref } from 'vue'
export default {
  setup() {
    const titleRef = ref(null);
    return{
      titleRef
    }
  }
}
</script>
```

## Setup的生命周期钩子
setup 可以用来替代 data 、 methods 、 computed 等等这些选项，也可以替代生命周期钩子，可以使用直接导入的 onX 函数注册生命周期钩子

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709534273635-b4312e04-a255-493e-8f09-6cffb3797043.png)

## Provide和Inject
之前还学习过Provide和Inject，Composition API也可以替代之前的 Provide 和 Inject 的选项，我们可以通过 provide来提供数据，可以通过 provide 方法来定义每个 Property；

provide可以传入两个参数：

+ name：提供的属性名称
+ value：提供的属性值

在 后代组件 中可以通过 inject 来注入需要的属性和对应的值，可以通过 inject 来注入需要的内容；

inject可以传入两个参数：

+ 要 inject 的 property 的 name
+ 默认值

```vue
<template>
  <div>
    <son></son>
  </div>
</template>

<script>
import { provide } from 'vue'
import son from './components/son.vue'

export default {
  components: {
    son
  },
  setup() {
    provide('name', 'kobe')
  }
}
</script>
```

```vue
<template>
  <div>
    <h2>{{ name }}</h2>
  </div>
</template>

<script>
import { inject } from 'vue'
export default {
  setup() {
    const name = inject('name')
    return {
      name
    }
  }
}
</script>
```

为了增加 provide 值和 inject 值之间的响应性，我们可以在 provide 值时使用 ref 和 reactive

```vue
export default {
  components: {
    son
  },
  setup() {
    let age = ref(18)
    provide('age', age)
  }
}
```

## watch
watch的API完全等同于组件watch选项的Property

+ watch需要侦听特定的数据源，并且执行其回调函数
+ 默认情况下它是惰性的，只有当被侦听的源发生变化时才会执行回调
+ 如果我们希望侦听一个深层的侦听，那么依然需要设置 deep 为true,也可以传入 immediate 立即执行

```vue
  setup() {
    const msg = ref('hi nihao')
    watch(msg, (newValue, oldValue) => {
      console.log(newValue, oldValue);
    },{
      immediate: true,
      deep: ture
    })
  }
```

侦听器还可以使用数组同时侦听多个源

```vue
  setup() {
    const msg = ref('hi nihao')
    const age = ref(18)
    watch([msg,age], (newValue, oldValue) => {
      console.log(newValue, oldValue);
    })
  }
```

## watchEffect
当侦听到某些响应式数据变化时，我们希望执行某些操作，这个时候可以使用 watchEffect

传入的函数会直接执行一次，在执行的过程，会**自动**收集依赖的响应式数据

```vue
import { watchEffect} from 'vue'
  
setup() {
    const msg = ref('hi nihao')
    const age = ref(18)
    watchEffect(() => {
      console.log('been done',msg.value,age.value);
    })
  }
```

如果在发生某些情况下，我们希望停止侦听，这个时候我们可以获取watchEffect的返回值函数，调用该函数即可

```vue
import { watchEffect, stopWatch } from 'vue'

export default {

  setup() {
    const msg = ref('hi nihao')
    const age = ref(18)

    const stopWatch = watchEffect(() => {
      console.log('been done', msg.value, age.value);
    });
    const change = () => {
      stopWatch();
    };
    return{
      msg,
      age,
      change
    }
  }
}
```

## 自定义hook函数
可以将counter相关的代码进行抽取，放到hooks文件夹中，再导入到App.vue中

注意是需要return的

```vue
import { ref } from 'vue'
export function useCounter() {
  const counter = ref(0);
  const increment = () => counter.value++
  const decrement = () => counter.value--
  return {
    counter,
    increment,
    decrement
  }
}
```

## script的setup
里面的代码会被编译成组件 setup() 函数的内容，这意味着与普通的 <script> 只在组件被首次引入的时候执行一次不同，<script setup> 中的代码会在每次组件实例被创建的时候执行

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709538306566-c7d2ca00-8b8a-4cbe-a557-bae07f95a320.png)

当使用 <script setup> 的时候，任何在 <script setup> 声明的顶层的绑定 (包括变量，函数声明，以及 import 引入的内容) 都能在模板中直接使用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709538298263-0cbc1c58-aece-47ce-973e-ce540d1281eb.png)

<script setup> 范围里的值也能被直接作为自定义组件的标签名使用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709538315360-543d3e98-722d-46b8-9b92-986e16623676.png)

为了在声明 props 和 emits 选项时获得完整的类型推断支持，我们可以使用 defineProps 和 defineEmits API，它们将自动地在 <script setup> 中可用，不需要导入

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709538943066-2f273aa9-dda3-43ae-8662-3cc169926b61.png)

使用 <script setup> 的组件是默认关闭的，通过模板 ref 或者 $parent 链获取到的组件的公开实例，不会暴露任何在 <script setup> 中声明的绑定，通过 defineExpose 编译器宏来显式指定在 <script setup> 组件中要暴露出去的 property

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709538990864-2fe1427a-673f-4e5d-8a68-cb5951c95155.png)

# Router-路由
## 历史
路由的概念在软件工程中出现，最早是在后端路由中实现的， 原因是web的发展主要经历了这样一些阶段：

+ 后端路由阶段；
+ 前后端分离阶段；
+ 单页面富应用（SPA）

### 后端路由阶段
早期的网站开发整个HTML页面是由服务器来渲染的，服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示。

但是一个网站这么多页面，服务器如何处理呢？一个页面有自己对应的网址, 也就是URL，

URL会发送到服务器，服务器会通过正则对该URL进行匹配，并且最后交给一个Controller进行处理，Controller进行各种处理, 最终生成HTML或者数据，返回给前端。

上面的这种操作, 就是后端路由。

当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户端，这种情况下渲染好的页面, 不需要单独加载任何的js和css，可以直接交给浏览器展示，这样也有利于SEO的优化.

后端路由的缺点:

+ 一种情况是整个页面的模块由后端人员来编写和维护的
+ 另一种情况是前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码
+ 而且通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情

### 前后端分离
前端渲染的理解：每次请求涉及到的静态资源都会从静态资源服务器获取，这些资源包括HTML+CSS+JS，然后在前端对这些请求回来的资源进行渲染。需要注意的是，客户端的每一次请求，都会从静态资源服务器请求文件。同时可以看到，和之前的后端路由不同，这时后端只是负责提供API了；

前后端分离阶段：随着Ajax的出现, 有了前后端分离的开发模式。后端只提供API来返回数据，前端通过Ajax获取数据，并且可以通过JavaScript将数据渲染到页面中。这样做最大的优点就是前后端责任的清晰，后端专注于数据上，前端专注于交互和可视化上，并且当移动端(iOS/Android)出现后，后端不需要进行任何处理，依然使用之前的一套API即可，但目前比较少的网站采用这种模式开发；

单页面富应用阶段：其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由，也就是前端来维护一套路由规则，前端路由的核心是改变URL，但是页面不进行整体的刷新  

### URL的hash
前端路由是如何做到URL和内容进行映射呢？监听URL的改变

URL的hash：URL的hash也就是锚点(#), 本质上是改变window.location的href属性，可以通过直接赋值location.hash来改变href, 但是页面不发生刷新

### H5的history
history接口是HTML5新增的, 它有六种模式改变URL而不刷新页面：

+ replaceState：替换原来的路径；
+ pushState：使用新的路径；
+ popState：路径的回退；
+ go：向前或向后改变路径；
+ forward：向前改变路径；
+ back：向后改变路径；

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709601944909-689dd271-59c1-41c3-8c92-020bd1551e03.png)

## router的使用
1. 创建路由需要映射的组件（打算显示的页面）；
2. 通过createRouter创建路由对象，并且传入routes和history模式；
    1. 配置路由映射: 组件和路径映射关系的routes数组；
    2. 创建基于hash或者history的模式；
3. 使用app注册路由对象（use方法）；
4. 路由使用: 通过<router-link>和<router-view>

```vue
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from '../vuerouter/views/home.vue'
import About from '../vuerouter/views/about.vue'

const router = createRouter({
    history: createWebHashHistory(),
    routes: [
        { path: '/home', component: Home },
        { path: '/about', component: About }
    ]
})
export default router
```

```vue
import { createApp } from 'vue'
import App from './App.vue'
import router from '../src/components/vuerouter/index'

const app = createApp(App)
app.use(router)
app.mount('#app')
```

```vue
<template>
    <h2>app</h2>
    <router-link to="/home">首页</router-link>
    <router-link to="/bout">关于</router-link>
    <router-view></router-view>
</template>
```

首页一般不需要点击，所以在path里使用/即可，表示根目录，redirect是重定向

```vue
routes: [
        { path:'/', redirect:'/home' },
        { path: '/home', component: Home },
        { path: '/about', component: About }
]
```

显示的时候还有另外一种不含*的模式：history

```vue
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../vuerouter/views/home.vue'
import About from '../vuerouter/views/about.vue'

const router = createRouter({
    history: createWebHistory(),
    routes: [
        { path:'/', redirect:'/home' },
        { path: '/home', component: Home },
        { path: '/about', component: About }
    ]
})
export default router
```

router-link事实上有很多属性可以配置：

+ to属性：是一个字符串，或者是一个对象
+ replace属性：设置 replace 属性的话，当点击时，会调用 router.replace()，而不是 router.push()；
+ active-class属性：设置激活a元素后应用的class，默认是router-link-active
+ exact-active-class属性：链接精准激活时，应用于渲染的 <a> 的 class，默认是router-link-exact-active

## 路由懒加载、分包处理
当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就会更加高效，也可以提高首屏的渲染效率；

其实这里还是前面讲到过的webpack的分包知识，而Vue Router默认就支持动态来导入组件，这是因为component可以传入一个组件，也可以接收一个函数，该函数需要放回一个Promise；而import函数就是返回一个Promise；

```javascript
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    { path: '/', redirect: '/home' },
    { path: '/home', component: ()=>import ('../vuerouter/views/home.vue') },
    { path: '/about', component: ()=>import('../vuerouter/views/about.vue') }
  ]
})
```

路由还有两个属性

+ name：路由独一无二的名称
+ meta：自定义的属性

```javascript
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    { path: '/', redirect: '/home' },
    {
      path: '/home',
      name: 'home',
      component: () => import('../vuerouter/views/home.vue')
    },
    {
      path: '/about',
      name: about,
      component: () => import('../vuerouter/views/about.vue'),
      meta: {
        age: 18
      }
    }
  ]
})
```

如果你的项目使用 webpack，动态导入会自动处理代码分割。懒加载的组件会被打包成单独的 chunk

还可以在懒加载时显示一个加载状态或过渡效果，以提升用户体验

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1726794882558-733711fb-8b04-4dba-91ba-d94ef653d90f.png)

## 动态路由和路由嵌套
### 动态路由
在Vue Router中，我们可以在路径中使用一个动态字段来实现，称之为路径参数

```vue
{
  path: '/home/:id',
  component: () => import('@/vuerouter/views/home.vue')
}
```

如何获取动态路由的值？

+ 在template中，直接通过 $route.params获取值
+ 在created中，通过 this.$route.params获取值
+ 在setup中，我们要使用 vue-router库给我们提供的一个hook函数useRoute
    - 该Hook会返回一个Route对象，对象中保存着当前路由相关的值

```vue
<template>
    <div>
        <h2>Home:{{ $route.params.id }}</h2>
    </div>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
    created(){
        console.log(this.$route.params.id);
    },
    setup(){
        const route = useRoute()
        console.log(route);
        console.log(route.params.id);
    }
}
</script>
```

对于那些没有匹配到的路由，我们通常会匹配到固定的某个页面，比如NotFound的错误页面中，这个时候我们可编写一个动态路由用于匹配所有的页面，可以通过 $route.params.pathMatch获取到传入的参数

```vue
{
  path: '/:pathMatch(.*)',
  component: () => import('../vuerouter/views/NotFound.vue')
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709633826897-65b977d6-9de0-4318-a077-57376c09246f.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709633876779-54fc7445-8cd4-4d33-b8dd-03e2b1c916f4.png)

### 路由的嵌套
目前我们匹配的Home、About、User等都属于第一层路由，我们在它们之间可以来回进行切换，但是Home页面本身也可能会在多个组件之间来回切换。比如Home中包括Product、Message，它们可以在Home内部来回切换。这个时候我们就需要使用嵌套路由，在Home中也使用 router-view 来占位之后需要渲染的组件。

```vue
{
  path: '/home',
  component: () => import('../vuerouter/views/home.vue'),
  children:[
            {
              path:'/about',
              component:()=>import('../vuerouter/views/about.vue')
            }		
  ]
}
```

## 路由的编程式导航
### 代码的页面跳转
```vue
import { useRouter } from 'vue-router'

const router = useRouter()
function changepage(){
        router.push('/about')
}
```

```vue
import { useRouter } from 'vue-router'
const router = useRouter()
function changepage(){
        router.push({
            path:'/about'
        })
}
```

```vue
import { useRouter } from 'vue-router'
const router = useRouter()
function changepage(){
        router.push({
            name:'about'
        })
}
```

```vue
function changepage(){
        router.push({
            path:'/about',
            query:{
                name:'jack',
                age:18
            }
        })
}
```

在页面可以通过$route.query来获取参数

```vue
<h2> query:{{ $route.query.name }} </h2>
```

使用push的特点是压入一个新的页面，那么在用户点击返回时，上一个页面还可以回退，但是如果我们希望当前页面是一个替换操作，那么可以使用replace

### 页面的前进后
router有go方法，可以让页面前进后退

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709636781640-51bc75f4-4c8e-4f98-96d6-964fb9a049a6.png)

+ 通过调用 router.back() 回溯历史。相当于 router.go(-1)；
+ 通过调用 router.forward() 在历史中前进。相当于 router.go(1)

## 动态管理路由对象
某些情况下我们可能需要动态的来添加路由，比如根据用户不同的权限，注册不同的路由，这个时候我们可以使用一个方法 addRoute

如果我们是为route添加一个children路由，那么可以传入对应父亲的name

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709772787354-f4897363-db68-4e1a-bf1a-b2301014b8f6.png)

删除路由有以下三种方式：

+ 添加一个name相同的路由
+ 通过removeRoute方法，传入路由的名称
+ 通过addRoute方法的返回值回调

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709772810924-eb70f5d2-71b6-40d2-a128-643330caeed5.png)

路由的其他方法补充： 

+ router.hasRoute()：检查路由是否存在。
+ router.getRoutes()：获取一个包含所有路由记录的数组

## 路由导航守卫钩子
vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航，全局的前置守卫beforeEach是在导航触发时会被回调的 

有两个参数：

+ to：即将进入的路由Route对象；
+ from：即将离开的路由Route对象；

有返回值：

+ false：取消当前导航
+ 不返回或者undefined：进行默认导航
+ 返回一个路由地址(想要跳转的地址)
    - 可以是一个string类型的路径；
    - 可以是一个对象，对象中包含path、query、params等信息；

```vue
router.beforeEach((to, from) => {
    if (to.path !== '/login') {
        return '/login'
    }
})
```

比如我们完成一个功能，只有登录后才能看到其他页面

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709817790989-938f76ac-e965-4328-8783-b03f1e19d968.png)

# Vuex-状态管理
## 状态管理
在开发中，我们会的应用程序需要处理各种各样的数据，这些数据需要保存在我们应用程序中的某一个位置，对于这些数据的管理我们就称之为是状态管理。

在Vue开发中，我们使用组件化的开发方式

+ 而在组件中我们定义data或者在setup中返回使用的数据，这些数据我们称之为state；
+ 在模块template中我们可以使用这些数据，模块最终会被渲染成DOM，我们称之为View；
+ 在模块中我们会产生一些行为事件，处理这些行为事件时，有可能会修改state，这些行为事件我们称之为actions；



JavaScript需要管理的状态越来越多，越来越复杂，这些状态包括服务器返回的数据、缓存数据、用户操作产生的数据等等，也包括一些UI的状态，比如某些元素是否被选中，是否显示加载动效，当前分页，我们的应用遇到多个组件共享状态时，单向数据流的简洁性很容易被破坏。

我们是否可以通过组件数据的传递来完成呢？对于一些简单的状态，确实可以通过props的传递或者Provide的方式来共享状态，但是对于复杂的状态管理来说，显然单纯通过传递和共享的方式是不足以解决问题的，比如兄弟组件如何共享数据呢？

管理不断变化的state本身是非常困难的，状态之间相互会存在依赖，一个状态的变化会引起另一个状态的变化，View页面也有可能会引起状态的变化，当应用程序复杂时，state在什么时候，因为什么原因而发生了变化，发生了怎么样的变化，会变得非常难以控制和追踪。

因此，我们是否可以考虑将组件的内部状态抽离出来，以一个全局单例的方式来管理呢？

+ 在这种模式下，我们的组件树构成了一个巨大的 “试图View”；
+ 不管在树的哪个位置，任何组件都能获取状态或者触发行为；
+ 通过定义和隔离状态管理中的各个概念，并通过强制性的规则来维护视图和状态间的独立性，我们的代码边会变得更加结构化和易于维护、跟踪

## State
### 基本概念
```vue
import { createStore } from 'vuex'
const store = createStore( {
    state(){
        return{

        }
    }
})
export default store

const store = createStore({
    state: () => ({

    })
})
```

上面的写法有两种，第一种需要return，第二种不需要，但是第二种需要在{}外加一个()

```vue
import { createApp } from 'vue'
import App from './App.vue'
import router from '../src/components/vuerouter/index'
import store from './store/index'

const app = createApp(App)
app.use(router)
app.use(store)
app.mount('#app')
```

然后就可以使用了

```vue
<template>
    <h2>我是{{ $store.state.counter }}</h2>
</template>
```

每一个Vuex应用的核心就是store（仓库），store本质上是一个容器，它包含着你的应用中大部分的状态（state）

Vuex和单纯的全局对象有什么区别呢？

+ 第一：Vuex的状态存储是**响应式**的，当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会被更新
+ 第二：**不能直接改变store中的状态**，改变store中的状态的唯一途径就显示提交 (commit) mutation，这样使得我们可以方便的跟踪每一个状态的变化，从而让我们能够通过一些工具帮助我们更好的管理应用的状态

```vue
<template>
    <h2>我是{{ $store.state.counter }}</h2>
    <button @click="change">改变</button>
</template>

<script setup>
import { useStore } from 'vuex';

const store = useStore()

function change(){
    store.state.counter++
}
</script>

<style scpoed></style>
```

```vue
<script>
export default {
    computed:{
        StoreChange(){
            return this.$store.state.counter
        }
    }
}
</script>
```

### 单一状态树
Vuex 使用单一状态树，即用一个对象就包含了全部的应用层级的状态，采用的是SSOT，Single Source of Truth，也可以翻译成单一数据源；

这也意味着，每个应用将仅仅包含一个 store 实例，单状态树和模块化并不冲突，后面我们会讲到module的概念；

单一状态树的优势：

+ 如果你的状态信息是保存到多个Store对象中的，那么之后的管理和维护等等都会变得特别困难；
+ 所以Vuex也使用了单一状态树来管理应用层级的全部状态；
+ 单一状态树能够让我们最直接的方式找到某个状态的片段；
+ 而且在之后的维护和调试过程中，也可以非常方便的管理和维护

### 组件获取状态（难点）
用$store.state.counter获取状态太繁琐，所以可以使用mapState辅助函数，他可以接受对象类型和数组类型，也可以使用展开运算符和原来有的computed混合在一起

```vue
<template>
    <h2>{{name}}-{{age}}-{{level}}</h2>
</template>

<script> 
import { mapState } from 'vuex';
export default{
    computed:{
        datum(){
            return 'xxx'
        },
        ...mapState(['name','age','level'])
    }
}
</script>
```

默认情况下，Vuex并没有提供非常方便的使用mapState的setup方式，这里进行了一个函数的封装

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709887091499-78d6c5a9-e3f7-44d0-acca-79f641014233.png)

## Getters
### 基本使用
某些属性可能需要经过变化后来使用，这个时候可以使用getters

```vue
import { createStore } from 'vuex'
const store = createStore({
    state: () => ({
        name: 'jack',
        age: 18,
    }),
    getters: {
        doubleAge(state) {
            return state.counter * 2
        }
    }
})
export default store
```

```vue
<template>
    <h2>{{$store.getters.doubleAge}}</h2>
</template>
```

getters可以接收第二个参数，可以在一个getters里获取其他的getters

```vue
import { createStore } from 'vuex'
const store = createStore({
    state: () => ({
        name: 'jack',
        age: 18,
    }),
    getters: {
        doubleAge(state) {
            return state.age * 2
        },
        msg(state,getters) {
            return `age:${state.age} doubleAge:${getters.doubleAge}`
        }
    }
})
export default store
```

getters中的函数本身，可以返回一个函数，那么在使用的地方相当于可以调用这个函数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1709948372941-8828d030-ce9a-4de2-a758-96a403e6155d.png)

### mapGetters
getters也有映射函数，叫mapGetters，好处是在模板中不需要写那么复杂了

```vue
<template>
    <h2>{{doubleAge}}</h2>
    <h2>{{mag}}</h2>
</template>

<script>
import { mapGetters } from 'vuex'
export default{
    computed:{
        ...mapGetters(['doubleAge,','msg'])
    }
}
</script>
```

## Mutation
### 基本使用
更改 Vuex 的 store 中状态的唯一方法是提交 mutation

```vue
import { createStore } from 'vuex'
const store = createStore({
    state: () => ({
        name: 'jack',
        age: 18,
    }),
    mutations: {
        changeName(state) {
            state.name = 'will'
        },
        minus(state) {
            state.age--
        }
    }
})
export default store
```

```vue
<template>
<h2>{{ $store.state.age }}</h2>
<h2>{{ $store.state.name }}</h2>
<button @click="change">改名</button>
<button @click="minusA">减</button>
</template>

<script>
export default{
    methods:{
        change(){
            this.$store.commit("changeName")
        },
        minusA(){
            this.$store.commit('minus')
        }
    }
}
</script>
```

在提交mutation的时候，会携带一些数据，这个时候可以使用参数

```vue
import { createStore } from 'vuex'
const store = createStore({
    state: () => ({
        name: 'jack',
        age: 18,
    }),
    mutations: {
        changeName(state, payload) {
            state.name = 'will' + payload
        },
        minus(state, payload) {
            state.age -= payload
        }
    }
})
export default store
```

```vue
<script>
export default{
    methods:{
        change(){
            this.$store.commit("changeName",' kobe')
        },
        minusA(){
            this.$store.commit('minus',10)
        }
    }
}
</script>
```

### Mutation常量类型
在store中创建一个叫mutation_type.js文件，里面存放mutation常量

```vue
export const CHANGE_NAME = "CHANGE_NAME"
```

```vue
import { createStore } from 'vuex'
import { CHANGE_NAME } from '../store/mutation_type.js'

const store = createStore({
    state: () => ({
        name: 'jack',
        age: 18,
    }),
    mutations: {
        [CHANGE_NAME](state, payload) {
            state.name += payload
        }
    }
})
export default store
```

```vue
<script>
import {CHANGE_NAME} from './store/mutation_type.js'
export default{
    methods:{
        change(){
            this.$store.commit("CHANGE_NAME",' kobe')
        },
    }
}
</script>
```

### mapMutations
也可以借助于辅助函数，帮助我们快速映射到对应的方法中

说实话不是特别好用

```vue
<script>
import {CHANGE_NAME,MINUS} from './store/mutation_type.js'
import { mapMutations } from 'vuex'
export default{
    methods:{
        ...mapMutations({
            change: CHANGE_NAME,
            minusA: MINUS
        })
    }
}
</script>

```

```vue
<script setup>
import {CHANGE_NAME,MINUS} from './store/mutation_type.js'
import { mapMutations } from 'vuex'
const mutations = mapMutations({
    change: CHANGE_NAME
})
</script>
```

### 原则
一条重要的原则就是要记住mutation必须是同步函数。

这是因为devtool工具会记录mutation的日记，每一条mutation被记录，devtools都需要捕捉到前一状态和后一状态的快照，但是在mutation中执行异步操作，就无法追踪到数据的变化，所以Vuex的重要原则中要求 mutation必须是同步函数

## Actions
Action类似于mutation，不同在于Action提交的是mutation，而不是直接变更状态，而且Action可以包含任意异步操作

### 基本使用
```vue
import { createStore } from 'vuex'

const store = createStore({
    state: () => ({
        name: 'jack',
        age: 18,
    }),
    mutations: {
        add(state) {
            state.age++
        }
    },
    actions: {
        addAction(context) {
            context.commit('add')
        }
    }
})
export default store
```

这里有一个非常重要的参数context：context是一个和store实例均有相同方法和属性的context对象，所以我们可以从其中获取到commit方法来提交一个mutation，或者通过 context.state 和 context.getters 来获取 state 和getters 

要在组件中使用actions需要进行派发dispatch

```vue
<script>
export default{
    methods:{
        add(){
            this.$store.dispatch('addAction')
        }
    }
}
</script>
```

在派发时可以携带参数

```vue
add(){
  this.$store.dispatch('addAction', { level : 17 })
}
```

也可以通过对象的形式派发

```vue
<script>
export default{
    methods:{
        add(){
            this.$store.dispatch({
                type:'addAction',
                level:17
            });
        }
    }
}
</script>
```

### mapActions
分为两种写法：数组和对象

```vue
<script>
import {mapAction} from 'vuex'
export default{
    methods:{
        ...mapActions({
            add:'addAction'
        }),
        ...mapActions(['addAction'])
    }
}
</script>
```

```vue
<script>
import { mapActions } from 'vuex';

const action1  = mapActions(['addAction'])
const action2 = mapActions({
    add:'addAction'
})
</script>
```

### 异步操作
Action 通常是异步的，那么如何知道 action 什么时候结束呢？

可以通过让action返回Promise，在Promise的then中来处理完成后的操作

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710124419052-7549ea46-55dd-41a9-b946-0a41998b8631.png)

## module
### 基本使用
由于使用单一状态树，应用的所有状态会集中到一个比较大的对象，当应用变得非常复杂时，store 对象就有可能变得相当臃肿，为了解决以上问题，Vuex 允许我们将 store 分割成模块（module），每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块

可以在store文件夹里建一个modules文件夹，在该文件夹中放分割好的modules，即js文件。每个文件中都export default输出一些内容，比如：

```javascript
export default {
  state: () => ({ }),
  mutations: {},
  actions: {},
  getter: {},
}
```

```javascript
import { createStore } from 'vuex'
import homeModule from './home'

const store = createStore({
    modules:{
        home:homeModule
    }
})
expor
```

如果要使用homeModule的内容，需要$store.state.home.age

### 局部状态
对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态对象

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710312360085-6f0ca247-0159-4f6e-aa89-d7b1af71abd6.png)

getters不管定义在index或者是module中，使用时都是$store.getters.xxx

### 命名空间
默认情况下，模块内部的action和mutation仍然是注册在全局的命名空间中的，这样使得多个模块能够对同一个 action 或 mutation 作出响应，Getter 同样也默认注册在全局命名空间。

通过命名空间，可以避免不同模块之间的状态、getter、mutation 和 action 名称冲突，使得模块更加独立和可维护

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710313114701-76c621e8-5b27-435c-a55a-6efe27f7b462.png)

右图注释中，在使用独立命名空间的getters时，采用下面的格式，A/B指的是A模块中的B方法

```javascript
$state.getters["add/addLevel"]
```

### 修改或派发根组件
如果想在action中修改root中的state，那么有如下的方式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710313824133-a4b20aac-6c2f-41fc-ab85-50efed0209a9.png)

用commit提交根组件中的名字，加额外的{root:ture}

# Pinia-状态管理
## store
### 安装和定义
安装：npm install pinia

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710315529390-a220ccb8-3af8-4baf-8038-3f3af37e5e17.png)

一个 Store （如 Pinia）是一个实体，它会持有为绑定到你组件树的状态和业务逻辑，也就是保存了全局的状态，它始终存在，并且每个人都可以读取和写入的组件，可以在你的应用程序中定义任意数量的Store来管理你的状态

Store有三个核心概念：

+ state、getters、actions
+ 等同于组件的data、computed、methods
+ 一旦 store 被实例化，你就可以直接在 store 上访问 state、getters 和 actions 中定义的任何属性

```javascript
//定义关于counter的store
import { defineStore } from "pinia"
const useCounter = defineStore('counter', {
    state: () => ({
        counter: 99
    })
})

export default useCounter
```

注意

+ 第一个参数是这个store的名字name，也叫Id，Pinia 使用它来将 store 连接到 devtools
+ 第二个参数是store的内容
+  返回的函数统一使用useX作为命名方案，这是约定的规范 

### 使用
Store在它被使用之前是不会创建的，我们可以通过调用use函数来使用Store

```vue
<template>
  <div class="home">
    <h2>我是home</h2>
    <h5>counter:{{ counterStore.counter }}</h5>
  </div>
</template>

<script setup>
  import useCounter from '../stores/counter.js'

  const counterStore = useCounter()

</script>
```

注意Store获取到后不能被解构，那么会失去响应式，为了从 Store 中提取属性同时保持其响应式，您需要使用storeToRefs()

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710335506296-5073b04b-12e8-4727-a3a4-0ff4791782c5.png)

## state
+ 读取和写入

默认情况下，可以通过 store 实例访问状态来直接读取和写入状态

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710336824292-dc81c9a3-ee5f-4d91-b7b0-ed6aba5235fb.png)

+ 重置

可以通过调用 store 上的 $reset() 方法将状态 重置 到其初始值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710336840714-edbc9d48-fd8d-40b0-b06c-13da7977e63e.png)

+ 更改

除了直接用 store.counter++ 修改 store，还可以调用 $patch 方法，它允许使用部分“state”对象同时应用多个更改

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710336914918-1227c27a-624f-470b-a412-b1b6241f3299.png)

+ 替换

可以通过将其 $state 属性设置为新对象来替换 Store 的整个状态

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710336933241-1f6259a3-66b3-4132-9f9f-c5b572450d53.png)

## getters
Getters相当于Store的计算属性，它们可以用 defineStore() 中的 getters 属性定义，getters中可以定义接受一个state作为参数的函数

在组件模版中使用时，直接.方法名即可，不需要加上getters

```javascript
import { defineStore } from "pinia"
const useCounter = defineStore('counter', {
    state: () => ({
        count: 10
    }),
    getters: {
        doubleCounter(state) {
            return state.count * 2
        }
    }
})

export default useCounter
```

```vue
<template>
  <div class="home">
    <h2>我是home</h2>
    <h2>{{counterStore.count }}</h2>
    <h2>doublecounter:{{ counterStore.doubleCounter }}</h2>
  </div>
</template>

<script setup>
  import useCounter from '../stores/counter.js'

  const counterStore = useCounter()

</script>
```

Getters中访问自己的其他Getters：可以通过this来访问到当前store实例的所有其他属性

```javascript
import { defineStore } from "pinia"
const useCounter = defineStore('counter', {
    state: () => ({
        count: 10
    }),
    getters: {
        doubleCounter(state) {
            return state.count * 2
        },
        addCounter() {
            return this.doubleCounter + 1
        }
    }
})

export default useCounter
```

getters中用其他state中的数据：导入其他useX，定义并使用

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710375676811-e003a029-cfc1-45f5-bd7f-0ac32097b9d8.png)

getters也可以返回一个函数，可以接受参数

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710375503389-7e670893-511c-4b4f-9916-db6349189374.png)

## actions
Actions 相当于组件中的 methods,可以使用 defineStore() 中的 actions 属性定义，并且它们非常适合定义业务逻辑，和getters一样，在action中可以通过this访问整个store实例的所有操作

```javascript
import { defineStore } from "pinia"
const useCounter = defineStore('counter', {
    state: () => ({
        count: 10,
    }),
    actions: {
        increment() {
            this.count ++
        }
    }
})

export default useCounter
```

```vue
<template>
  <div class="home">
    <h2>我是home</h2>
    <h2>{{counterStore.count }}</h2>
    <button @click="increment">点击</button>
  </div>
</template>

<script setup>
  import useCounter from '../stores/counter.js'

  const counterStore = useCounter()
  
  function increment(){
    counterStore.increment()
  }

</script>
```

actions中支持异步操作，并且可以编写一步函数，在函数中使用await

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710376146196-cf36e2ff-3134-4d9c-b315-f452f38d7cca.png)

# axios-网络请求库
## 请求
axios.request 是 axios 库提供的一个方法，用于发起各种类型的 HTTP 请求（GET、POST、PUT、DELETE 等）。通过 axios.request 方法，你可以根据需要指定特定的请求参数、URL、请求头等，以灵活地发起定制化的请求。

请求方式有很多个，最常用的是get和post

+ axios.get(url, config)
    - 用于从服务器获取资源。GET 请求通常用于获取数据，它不应该对服务器上的资源产生任何副作用。在 HTTP 请求中，GET 请求的参数通常附在 URL 中，通过查询字符串的方式发送至服务器。例如，从服务器获取用户信息、文章内容或者其他数据可以使用 GET 请求。
+ axios.delete(url, config)
    - 用于请求服务器删除指定的资源。DELETE 请求用于删除服务器上的特定资源，并且需要谨慎使用，因为它会直接影响到服务器上的数据。例如，删除用户账号或者删除特定文章可以使用 DELETE 请求。
+ axios.post(url, data, config)
    - 用于向服务器提交数据，以创建新的资源。POST 请求通常用于提交表单数据、上传文件、执行登录操作以及其他需要在服务器创建新数据的场景。在 POST 请求中，数据通常作为请求主体的一部分发送至服务器。
+ axios.put(url, data, config)
    - 用于向服务器更新已存在的资源。PUT 请求通常用于更新特定 ID 的资源，它会将提交的数据替换服务器上已存在的数据。例如，修改用户信息或者更新文章内容可以使用 PUT 请求。
+ axios.patch(url, data, config)
    - 对指定资源的某些属性或字段(部分资源)进行更新，而不需要提供完整的资源表示。这对于需要更新资源的特定属性或字段，而不改变其他属性或字段的情况很有用。

## 配置项
常见的配置选项

+ url: '/user'，完整路径
+ method: 'get'
+ data：用作请求主体的数据，用于 POST、PUT 等需要请求主体的方法
    - data: { key: 'aa'}
+ baseURL: 'http://www.mt.com/api'，可以使用相对路径
+ headers:{'x-Requested-With':'XMLHttpRequest'}
+ params:{ id:12}
+ timeout: 1000
+ auth：用于 HTTP 基本认证的对象，包含 username 和 password。
    - auth: {username: 'john_doe',password: 'secretpassword'}
+ 查询对象序列化函数：paramsSerializer: function(params){}
+ 请求前的数据处理：transformRequest:[function(data){}]
+ 请求后的数据处理：transformResponse: [function(data){}]

```javascript
import axios from 'axios'

axios.request({
  url: '',
  methods: 'get',
}).then(res => {
  console.log('res:', res);
})
```

## 请求和响应拦截器
axios的也可以设置拦截器：拦截每次请求和响应

+ axios.interceptors.request.use(请求成功拦截, 请求失败拦截)
+ axios.interceptors.response.use(响应成功拦截, 响应失败拦截)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710579078243-164eddfd-1b89-4b01-bde3-d12ca480da89.png)

## 实例
axios.create 是 axios 库提供的一个方法，用于创建一个新的 axios 实例。通过创建新的 axios 实例，你可以为该实例配置默认的请求参数、请求头、拦截器等，以及复用这些配置来发起多个请求。

```javascript
import axios from 'axios';

// 创建 Axios 实例
const instance = axios.create({
  baseURL: 'https://api.example.com', // 设置基础的请求 URL
  timeout: 5000,  // 请求超时时间
  headers: {
    'Content-Type': 'application/json',  // 设置请求头
  },
});

// 添加请求拦截器
instance.interceptors.request.use(
  function(config) {
    // 在发送请求之前做些什么
    config.headers.Authorization = 'Bearer token123'; // 添加认证信息
    // 可以在这里添加 loading 效果等
    return config;
  },
  function(error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  }
);

// 添加响应拦截器
instance.interceptors.response.use(
  function(response) {
    // 对响应数据做点什么
    return response;
  },
  function(error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  }
);

export default instance;

```

```javascript
import api from './api';  // 将上述示例的实例导入为 "api"

api.get('/data')
  .then(response => {
    // 处理成功的响应
    console.log(response.data);
  })
  .catch(error => {
    // 处理错误
    console.error(error);
  });
```

# 项目实战--98旅途
## 基本步骤
### 创建
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710636797850-5a4448ee-94c3-49ef-ba44-4643f69afebd.png)

### 项目结构划分
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1710636815352-d7ebc6cb-b4e8-4706-8811-643c9182b065.png)

+ assets：img,css,font,audio,video
+ components：组件
+ hooks：公用函数
+ mock：模拟数据
+ router：路由
+ service：网络请求
+ store：状态管理
+ utils：工具函数
+ views/pages：页面

### css样式的重置
+ normalize.css：定义好的css库，专门用于对页面样式进行重置
    - npm install --save normalize.css
+ reset.css
+ common.css

```css
body,h1,h2,h3,h4,ul,li {
    padding: 0;
    margin: 0;
}

ul,li {
    list-style: none;
}

a {
    text-decoration: none;
    color: #333;
}

img {
    vertical-align: top;
}
```

```css
/* 下面是示例，里面写一些公共的东西 */
body{
    font-size:14px;
    font-family:'Courier New', Courier, monospace;
}
```

```css
@import "./common.css";
@import "./reset.css";
```

```javascript
import { createApp } from 'vue'
import App from './App.vue'

import "normalize.css"
import "./assets/css/index.css"


const app = createApp(App)
app.mount('#app')
```

### 路由配置
在项目创建所需页面，下面用Home来演示

```vue
<template>
  <div class="home">
    <h2>home</h2>
  </div>
</template>

<script setup>

</script>

<style scoped>

</style>
```

```vue
<template>
  <div class="app">
    <h2>App Conponnet</h2>
    <RouterView></RouterView>
    <RouterLink to="/home">首页</RouterLink>
  </div>
</template>

<script setup>

</script>

<style scoped>

</style>
```

```javascript
import { createRouter, createWebHashHistory } from "vue-router"
const router = createRouter({
    history:createWebHashHistory(),
    routes:[
        {
            path: '/',
            redirect: '/home'
        },
        {
            path: '/home',
            component: () => import('@/views/home/home.vue')
        },
    ]
})
export default router
```

```javascript
import { createApp } from 'vue'
import App from './App.vue'

import "normalize.css"
import "./assets/css/index.css"
import router from './router/index'


const app = createApp(App)
app.use(router)
app.mount('#app')
```

### 状态管理配置
### 扩展vant4插件
[https://vant-ui.github.io/vant/#/zh-CN/quickstart](https://vant-ui.github.io/vant/#/zh-CN/quickstart)

使用该插件的步骤(vite方式)

1. 安装：npm i vant
2. 安装插件：npm i @vant/auto-import-resolver unplugin-vue-components unplugin-auto-import -D
3. 修改vite.config.js内部配置

```vue
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

import AutoImport from 'unplugin-auto-import/vite';
import Components from 'unplugin-vue-components/vite';
import { VantResolver } from '@vant/auto-import-resolver';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    AutoImport({
      resolvers: [VantResolver()],
    }),
    Components({
      resolvers: [VantResolver()],
    }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```

如果对第三方库中某个类的样式不满意，怎么修改？

+ 用插槽，插入自己的元素，就可以在自己的作用域中修改元素样式
+ 全局定义一个变量，覆盖他默认变量的值
+ 局部定义一个变量，覆盖默认变量的值
+ 直接查找到对应的子组件选择器，进行修改
    - 如果在父组件中修改子组件样式，可能会因为scoped无法生效，需要加上 :deep(子组件类名){font-size:10px}

### tabbar隐藏显示
有两种方式

1. 设置一个属性，当为true时v-if显示

```javascript
{
  path: '/city',
    component: () => import('@/views/city/city.vue'),
    meta: {
    hideTabBar: true
  }
}
```

```vue
<template>
  <div class="app">
    <tabbar v-if="!route.meta.hideTabBar"></tabbar>
    <router-view></router-view>
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router';
import tabbar from './components/tabbar.vue';

const route = useRoute()

</script>
```

2. 单独给不显示的那个页面设置样式让其压面滚动

```css
<style lang="less" scoped>
.city {
  height: 100vh;
  position: relative;
  z-index: 9;
  background-color: #fff;

  overflow: auto;
}
</style>
```

### 页面展示大量数据
既要保证top不动，也要保证top不会遮挡数据，如图便是遮挡了1-6

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711608986928-9554bffc-9b5d-4c29-aa53-3108bcb24eb7.png)

#### 方案1-全局滚动
```vue
<template>
    <div class="city">
        <div class="top">
            <van-search v-model="searchValue" placeholder="城市/区域/位置" shape="round" show-action @cancel="cancelClick" />
            <van-tabs v-model:active="tabActive" color="#ff9854" line-height="3">
                <template v-for="(value, key, index) in allCity" :key="key">
                    <van-tab :title="value.title"></van-tab>
                </template>
            </van-tabs>
        </div>
        <div class="content">
            <template v-for="item in 100">
                <div>List:{{ item }}</div>

            </template>
        </div>
    </div>
</template>

<style lang="less" scoped>
.city {
    .top {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;

    }

    .content {
        margin-top: 98px;
    }
}
</style>
```

但是这样写的话，滚动的其实是整个页面，滚动条也会显示在整个页面中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1711609255612-31e21668-8ef5-4bbe-9254-f8b355fad5c6.png)

#### 方案2-局部滚动
> overflow-y: auto属性可以在元素的内容超出其框时自动显示垂直滚动条。
>
> 这在元素的内容高度超过其指定高度时很有用，可以让用户滚动查看全部内容。
>
> 注意：需要指定高度
>

> vh是CSS中的长度单位，表示视口高度的百分比。
>
> 具体来说，1vh等于视口高度的1%，这使得vh单位可以根据视口大小进行自适应调整，非常适合用于制作响应式布局。
>

> calc()是CSS中的一个函数，用于在CSS属性中执行简单的算术运算。这个函数可以结合不同的长度单位、百分比和数值，进行加减乘除等基本运算，从而动态地计算属性的值。
>
> 使用calc()函数可以帮助实现一些动态的布局效果，例如：动态计算宽度或高度：可以在设置元素的宽度或高度时，结合固定值、百分比或其他长度单位，实现根据不同情况进行动态计算。
>

```vue
<style lang="less" scoped>
.city {
    .content {
        height: calc(100vh - 98px);
        overflow-y: auto;
    }
}
</style>
```

此时不仅正常显示数据，top不会遮挡，而且滚动条长度也合适

### 获取当前时间
如果在项目中需要获取当前时间，并按照规定的格式，可以安装第三方库dayjs

```javascript
import dayjs from "dayjs"

export function formatMonthDay(date){
  return dayjs(date).format("MM月DD日")
}
```

```javascript
const nowDate = new Date()
const startDate = ref(formatMonthDay(nowDate))
const newDate = nowDate.setDate(nowDate.getDate + 1)
const endDate = ref(formatMonthDay(newDate))
```

### 发送网络请求
网页中发送大量网络请求时，在vue文件中书写是不大规范的，所以需要将其封装到store中，再引入到vue使用

```javascript
const hotSuggests = ref([])
hyRequest.get({
    url: '/home/hotSuggests'
}).then(res => {
    hotSuggests.value = res.data
})
```

```javascript
import { defineStore } from "pinia";
import hyRequest from '@/service/request/index'

const useHomeStore = defineStore('home', {
    state: () => ({
        hotSuggests: [],
        categories: []
    }),
    actions: {
        async fetchHotSuggestData() {
            const res = hyRequest.get({ url: '/home/hotSuggests' })
            this.hotSuggests = res.data
        }
    }

})

export default useHomeStore
```

同时const res = hyRequest.get({ url: '/home/hotSuggests' })显得很臃肿，所以会在service/modules里封装一个叫home.js的文件

```javascript
import { defineStore } from "pinia";
import hyRequest from '@/service/request/index'
import { getHomeHotSuggests } from "@/service";

const useHomeStore = defineStore('home', {
    state: () => ({
        hotSuggests: [],
        categories: []
    }),
    actions: {
        async fetchHotSuggestData() {
            const res = await getHomeHotSuggests()
            this.hotSuggests = res.data
        }
    }

})

export default useHomeStore
```

```javascript
import hyRequest from '../request/index'
export function getHomeHotSuggests() {
    return hyRequest.get({ url: '/home/hotSuggests' })
}
```

### 获取用户位置
chrome和edge获取的方式不同

+ edge会先找电脑里有没有存用户的位置，有的话就直接用，没有才会去获取IP
+ chrome会去直接获取用户的IP，并且把用户的IP发送给谷歌的服务器，所以要求用户能连接上谷歌服务器，国内用户需要使用VPN
+ 但是手机无所谓，只要授权位置，那就一定可以获取位置信息

### 跳转页面并传递参数
```javascript
const searchBtnClick = () => {
    router.push({
        path: '/search',
        query: {
            //因为是ref，所以需要.value
            startDate: startDate.value,
            endDate: endDate.value,
            currentCity: currentCity.value.cityName,
        }
    })
}
```

### 把评分显示成星级
:model-value，这个和v-model不一样，这个是单项绑定值

因为后面是字符串，所以需要转化成数字，有两种方法

+ 直接使用Number(字符串)

```javascript
<van-rate :model-value="Number(itemData.commentScore)" />
```

+ 在computed中使用（显得不臃肿）

```vue
<van-rate :model-value="itemStore" />

  <script setup>
    import { computed } from 'vue';
    const props = defineProps({
      itemData: {
        type: Object,
        default: () => ({})
      }
    })

    const itemStore = computed(() => {
      return Number(props.itemData.commentScore)
    })
  </script>
```

### 监听滚动到底部时展示数据
```vue
const scrollListenerHandler = () => {
    const clientHeight = document.documentElement.clientHeight
    const scrollTop = document.documentElement.scrollTop
    const scrollHeight = document.documentElement.scrollHeight
    if (clientHeight + scrollTop >= scrollHeight) {
        homeStore.fetchHouselistData()
    }
}

onMounted(() => {
    window.addEventListener('scorll', scrollListenerHandler)
})

onActivated(() => {
    window.addEventListener('scorll', scrollListenerHandler)
})

onUnmounted(() => {
    window.removeEventListener('scorll', scrollListenerHandler)
})

onDeactivated(() => {
    window.removeEventListener('scorll', scrollListenerHandler)
})
```

这个函数是通用的，所以可以封装一下

```vue
import { onMounted, onUnmounted, ref } from 'vue'

export default function useScroll() {
    const isReachBottom = ref(false)
    const scrollListenerHandler = () => {
        const clientHeight = document.documentElement.clientHeight
        const scrollTop = document.documentElement.scrollTop
        const scrollHeight = document.documentElement.scrollHeight
        if (clientHeight + scrollTop >= scrollHeight) {
            isReachBottom.value = true
        }
    }

    onMounted(() => {
        window.addEventListener('scorll', scrollListenerHandler)
    })

    onUnmounted(() => {
        window.removeEventListener('scorll', scrollListenerHandler)
    })


    return { isReachBottom }
}
```

```vue
<script>
const homeStore = useHomeStore()
homeStore.fetchHotSuggestData()
homeStore.fetchCategoriesData()
homeStore.fetchHouselistData()

const { isReachBottom } = useScroll()
watch(isReachBottom, (newValue) => {
    if (newValue) {
        homeStore.fetchHouselistData()
        isReachBottom.value = false
    }
})
</script>
```

### 页面加载效果-拦截器
```vue
<template>
    <div class="loading" v-if="isLoading" @click="loadingClick">
        <div class="bg">
            <img src="@/assets/img/home/full-screen-loading.gif" alt="">
        </div>
    </div>
</template>

<script setup>
import useMainStore from '@/store/modules/main'
import { storeToRefs } from 'pinia';
const mainStore = useMainStore()
const { isLoading } = storeToRefs(mainStore)

const loadingClick = () => {
    mainStore.isLoading = false
}


</script>

<style lang="less" scoped>
.loading {
    position: fixed;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
    left: 0;
    bottom: 0;
    right: 0;
    top: 0;
    background-color: rgba(0, 0, 0, .6);

    .bg {
        height: 104px;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 104px;
        background: url(@/assets/img/home/loading-bg.png) 0 0 / 100% 100%;

        img {
            width: 70px;
            height: 70px;
        }
    }
}
</style>
```

注意isLoading在store内定义，然后在网络请求中设置拦截器，当拦截器运行时，调整isLoading

拦截器在下面代码的14-28行

```javascript
import axios from "axios";
import { BASE_URL, TIMEOUT } from "./config";
import useMainStore from "@/store/modules/main";
import { storeToRefs } from "pinia";
const mainStore = useMainStore()


class hyRequest {
    constructor(BASE_URL, TIMEOUT) {
        this.instance = axios.create({
            baseURL: BASE_URL,
            timeout: TIMEOUT,
        });
        this.instance.interceptors.request.use(config => {
            mainStore.isLoading = true
            return config
        }, err => {
          //发送失败就没必要加载了，加上也无所谓
          // mainStore.isLoading = true
            return err
        })
        this.instance.interceptors.response.use(res => {
            mainStore.isLoading = false
            return res
        }, err => {
            mainStore.isLoading = false
            return err
        })
    }

    request(config) {
        return new Promise((resolve, reject) => {
            this.instance.request(config).then(res => {
                resolve(res.data)
            }).catch(err => {
                reject(err)
            })
        })
    }

    get(config) {
        return this.request({ ...config, method: "get" });
    }

    post(config) {
        return this.request({ ...config, method: "post" });
    }
}

export default new hyRequest(BASE_URL);
```

### 组件直接使用@click
在父组件的template中，可以直接在子组件中添加@click，方法写在父组件中即可

```vue
<template>
  <house-item-v9 v-if="item.discoveryContentType === 9" :item-data="item.data" @click="itemClick" />
  <house-item-v3 v-else-if="item.discoveryContentType === 3" :item-data="item.data" @click="itemClick" />
</template>

<script setup>
  const itemClick = (item) => {
    console.log(item.houseId);
  }
</script>
```

### 动态路由的使用
```vue
<template>
<house-item-v3 v-else-if="item.discoveryContentType === 3" :item-data="item.data"
      @click="itemClick(item.data)" />
</template>

<script setup>
  const itemClick = (item) => {
    router.push("/detail/" + item.houseId)
}
</script>

```

```vue
{
 path: "/detail/:id",
 component: () => import('@/views/details/detail.vue')
}
```

### 百度地图开发平台的使用
注册开发者后，创建一个应用，选择需要的应用类别，然后打开index.html，进行如下引用

```html
<script type="text/javascript" 
  src="https://api.map.baidu.com/api?v=3.0&&type=webgl&ak=您的密钥">
</script>

<!-- 实例 -->
<script type="text/javascript"
  src="https://api.map.baidu.com/api?v=3.0&&type=webgl&ak=uTy5is6gCTl0BlzWztVpM2qNDhClTJAY">
</script>
```

通过设计文档进行编码：[jspopularGL | 百度地图API SDK](https://lbsyun.baidu.com/index.php?title=jspopularGL/guide/helloworld)

```vue
<template>
  <div class="baidu" ref="mapRef"></div>
</template>

<script>
  import { onMounted, ref } from 'vue';

  const mapRef = ref()
  onMounted(() => {
    const map = new BMapGL.Map(mapRef.value);
    const point = new BMapGL.Point(116.404, 39.915);
    map.centerAndZoom(point, 15);
  })
</script>
```

注意要在onmounted中使用（因为是setup）



实例

```vue
<template>
    <div class="map">
        <detail-section title="位置周边" more-text="查看更多周边信息">
            <div class="baidu" ref="mapRef"></div>
        </detail-section>
    </div>
</template>

<script setup>
import DetailSection from '@/components/detail-section/detail-section.vue';
import { onMounted, ref } from 'vue';

const props = defineProps({
    position: {
        type: Object,
        default: () => ({})
    }
})

const mapRef = ref()
onMounted(() => {
    const map = new BMapGL.Map(mapRef.value);
    const point = new BMapGL.Point(props.position.longitude, props.position.latitude);
    map.centerAndZoom(point, 15);
    
    const marker = new BMapGL.Marker(point);
    map.addOverlay(marker);
})

</script>

<style lang="less" scoped>
.baidu {
    height: 300px;
}
</style>
```

# Vue3其他高级语法
## 自定义指令
通常在某些情况下，你需要对DOM元素进行底层操作，这个时候就会用到自定义指令

自定义指令分为两种：

+ 自定义局部指令：组件中通过 directives 选项，只能在当前组件中使用；
+ 自定义全局指令：app的 directive 方法，可以在任意组件中被使用；

下面使用三种方式：

+ 实现方式一：使用默认的实现方式
+ 实现方式二：自定义一个 v-focus 的局部指令
+ 实现方式三：自定义一个 v-focus 的全局指令

### 方式一
```vue
<template>
    <div class="app">
        <input type="text" ref="inputRef">
    </div>
</template>

<script setup>
import useInput from './hooks/useInput';
const { inputRef } = useInput()

</script>

<style scoped></style>
```

```javascript
import { ref, onMounted } from 'vue'
export default function useInput() {
  const inputRef = ref()
  onMounted(() => {
    inputRef.value?.focus()
  })
}
```

### 方式二
这个自定义指令实现非常简单，只需要在组件选项中使用 directives 即可

在 Vue 3 中，directives 是一种用于操作 DOM 的工具，允许你对元素进行底层的行为控制。通过自定义指令，你可以实现特定的功能，比如自动聚焦、动态样式、事件处理等

它是一个对象，在对象中编写自定义指令的名称（optionsAPI不需要加v-，但是setup中需要加v），自定义指令有一个生命周期，是在组件挂载后调用的 mounted，可以在其中完成操作

![optionsAPI写法](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712663588991-41d2a50b-b255-4c6b-9a09-4bd0965baf66.png)![setup写法](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712664193204-38e39401-f243-4d1c-8765-316be21f47a2.png)

### 方式三
直接在main.js中定义

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712664425487-db859cbd-4075-479e-abd3-51f60e8ed540.png)

但是如果自定义组件有很多个，那就把代码抽取到directives文件夹中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712664731048-920bccd4-20fe-4a07-a24a-b697ae6595ae.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712664878996-99d75d11-3cb8-482f-bf10-4134158dbfd9.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712665009972-241a4094-bf33-41a8-9404-610e2b424f34.png)

### 自定义组件的生命周期
+ created：在绑定元素的 attribute 或事件监听器被应用之前调用；
+ beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用；
+ mounted：在绑定元素的父组件被挂载后调用；
+ beforeUpdate：在更新包含组件的 VNode 之前调用；
+ updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用；
+ beforeUnmount：在卸载绑定元素的父组件之前调用；
+ unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次；

### 自定义指令的参数和修饰符
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712667224838-dee6ef56-05c9-4463-85f7-847c415cb1e3.png)

+ nihao 是参数的名称
+ hello、nba是修饰符的名称
+ 后面是传入的具体的值

在生命周期中，可以通过 bindings 获取到对应的内容：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712667450973-7e21e9f3-72e0-4c62-ac2a-4b211c238cdf.png)

#### 实例：给价格拼接一个￥符号
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712667777192-98215628-41fd-4ff7-b26d-5b0742f8280a.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712667751322-972ff926-a8a9-4515-9783-ecf539c5c073.png)

#### 实例：时间格式化
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712717677753-1d47bc3f-aab6-4f53-90c2-a7cc46952ab4.png)

## 内置组件Teleport
在组件化开发中，我们封装一个组件A，在另外一个组件B中使用，那么组件A中template的元素，会被挂载到组件B中template的某个位置，最终应用程序会形成一颗DOM树结构

但是某些情况下，我们希望组件不是挂载在这个组件树上的，可能是移动到Vue app之外的其他位置，比如移动到body元素上，或者我们有其他的div#app之外的元素上(弹出菜单或通知等)，这个时候我们就可以通过teleport来完成



Teleport是一个Vue提供的内置组件，类似于react的Portals

它有两个属性：

+ to：指定将其中的内容移动到的目标元素，可以使用选择器
+ disabled：是否禁用 teleport 的功能

```vue
<template>
    <div class="app">
        <div class="hi">
            <div class="content">
                <Teleport to="body">
                    <son />
                </Teleport>
                <Teleport to="#mother">
                    <son />
                </Teleport>
            </div>
        </div>
    </div>
</template>
```

如果我们将多个teleport应用到同一个目标上（to的值相同），那么这些目标会进行合并

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712731980409-c595db3e-850f-49fb-b602-b8eed413980f.png)

## vue中安装插件的方式
通常我们向Vue全局添加一些功能时，会采用插件的模式，它有两种编写方式：

+ 对象类型：一个对象，但是必须包含一个 install 的函数，该函数会在安装插件时执行![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712732955880-fc6dcdee-c30f-4fa9-9aff-3f1baf587ae7.png)
+ 函数类型：一个function，这个函数会在安装插件时自动执行![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712733052545-e1c721d9-1049-42a1-bece-4ace3fc078fc.png)

插件可以完成的功能没有限制，比如下面的几种都是可以的：

+ 添加全局方法或者 property，通过把它们添加到 config.globalProperties 上实现
+ 添加全局资源：指令/过滤器/过渡等
+ 通过全局 mixin 来添加一些组件选项
+ 一个库，提供自己的 API，同时提供上面提到的一个或多个功能

插件的编写方式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712733299922-70cbdbf0-6481-4603-a998-a53022ea3af6.png)

## 渲染函数h()
Vue推荐在绝大数情况下使用模板来创建你的HTML，然后一些特殊的场景，你真的需要JavaScript的完全编程的能力，这个时候你可以使用渲染函数 ，它比模板更接近编译器

所以可以使用h()函数

+ h() 函数是一个用于创建 vnode 的一个函数
+ 其实更准备的命名是 createVNode() 函数，但是为了简便，Vue将其简化为 h() 函数

### 基本使用
接受三个参数

+ tag：html标签/组件
+ props：与attribute、props和事件对应的对象
+ children：String/Array/Object，子VNodes

注意事项

+ 第一个是必须写的，后两个可选
+ 如果没有props，那么通常可以将children作为第二个参数传入
+ 如果会产生歧义，可以将null作为第二个参数传入，将children作为第三个参数传入；

例如：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712972224083-8f4f27f1-9194-4df6-8ddd-96925f8eefe4.png)

### OptionsAPI-CompositionAPI
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712972461135-58dc647c-551f-4c88-919f-81ce5d05982a.png)

###  实例：计数器
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712972480057-abc9e3eb-0dfe-451a-9bc2-a12c58205f3f.png)

## jsx
### 介绍
JSX（JavaScript XML）是一种JavaScript的语法扩展，通常与React一起使用。它允许开发者以类似于XML或HTML的方式编写组件结构，使得在JavaScript代码中直接嵌入UI组件更加容易和直观。

在React中，JSX被用来描述用户界面的结构。例如：

const element = <h1>Hello, world!</h1>;

在这个例子中，<h1>Hello, world!</h1>就是一个JSX表达式，它会被编译成对应的JavaScript代码，类似于下面的形式：

const element = React.createElement('h1', null, 'Hello, world!');

这种方式使得开发者可以在JavaScript中直接编写类似HTML的代码，提高了代码的可读性和可维护性。

### babe设置
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712974079144-6aad293a-5f62-459e-ac71-99cad4d7a615.png)

### 案例：计数器
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712974094203-5d6c078f-bf74-46a5-84fc-1d13fed33e32.png)、

# Vue3实现过渡动画
## Transition
在开发中，我们想要给一个组件的显示和消失添加某种过渡动画，可以很好的增加用户体验：

+ React框架本身并没有提供任何动画相关的API，所以在React中使用过渡动画我们需要使用一个第三方库 react-transitiongroup；
+ Vue中为我们提供一些内置组件和对应的API来完成动画，利用它们我们可以方便的实现过渡动画效果；

如果我们希望给单元素或者组件实现过渡动画，可以使用 transition 内置组件来完成动画

### 基本使用
Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡：

+ 条件渲染 (使用 v-if)条件展示 (使用 v-show)
+ 动态组件
+ 组件根节点

常用的class

+ 进入过渡的起始状态类名：.fade-enter-from
+ 进入过渡的结束状态类名：.fade-enter-to
+ 离开过渡的起始状态类名：.fade-leave-from
+ 离开过渡的结束状态类名：.fade-leave-to
+ 过渡过程中的活动状态类名：.fade-enter-active 和 .fade-leave-active

```vue
<template>
    <div class="app">
        <button @click="isShow = !isShow">拆火</button>
        <transition>
            <h2 v-if="isShow">披坚执锐 嗦橡皮泥</h2>
        </transition>

    </div>
</template>

<script setup>
import { ref } from 'vue';
const isShow = ref(true)

</script>

<style scoped>
.v-enter-from {
    opacity: 0;
    transform: scale(0.6);
}

.v-enter-to {
    opacity: 1;
    transform: scale(1);
}

.v-enter-active {
    transition: all 2s ease;
}
</style>
```

### transition的原理
当插入或删除包含在 transition 组件中的元素时，Vue 将会做以下处理：

+ 自动嗅探目标元素是否应用了CSS过渡或者动画，如果有，那么在恰当的时机添加/删除 CSS类名；
+ 如果 transition 组件提供了JavaScript钩子函数，这些钩子函数将在恰当的时机被调用；
+ 如果没有找到JavaScript钩子并且也没有检测到CSS过渡/动画，DOM插入、删除操作将会立即执行；

### 过渡动画Class
#### 常用的class
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712978074588-f6b944dc-0bcb-45d4-8d21-014af391a587.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712978558094-830b68b7-4da0-4f22-9cae-749c23fb7ff0.png)

#### fade-和v-的区别
.fade-enter-from 和 .v-enter-from 是 Vue.js 中用于过渡效果的类名，它们通常与过渡组件（如 <transition> 或 <transition-group>）一起使用。

+ .fade-enter-from 是在使用 CSS 过渡时，Vue 自动为元素添加的类名
+ .v-enter-from 则是在使用 JavaScript 钩子函数时，Vue 在过渡过程中手动添加的类名，通常情况下，你不需要手动编写这些类名，而是通过使用 Vue 的过渡功能，Vue 会自动添加这些类名来实现过渡效果。

#### 添加class的时机
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1712978183551-4acc9b55-dd1d-45b0-a90d-c685e2eea25f.png)

#### 命名规则
+ 如果使用的是一个没有name的transition，那么所有的class是以 v- 作为默认前缀
+ 如果添加了一个name属性，比如 ，那么所有的class会以 why- 开头； 

## animation
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713077594473-d7438d74-b424-4605-b7b7-bc7ba09aba74.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713077602038-684816cf-2c8c-4af4-a427-5d037d8ec4f6.png)

## 动画的属性设置
### type
Vue为了知道过渡的完成，内部是在监听 transitionend 或 animationend，到底使用哪一个取决于元素应用的CSS规则。

如果我们只是使用了其中的一个，那么Vue能自动识别类型并设置监听

但是如果同时使用了过渡和动画。并且在这个情况下可能某一个动画执行结束时，另外一个动画还没有结束。在这种情况下，我们可以设置 type 属性为 animation 或者 transition 来明确的告知Vue监听的类型

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713078308312-219c0e08-af70-467d-b1ac-665d93ff052f.png)

注意：一般不这样写

### duration
duration 来指定过渡的时间，可以设置两种类型的值：

+ number类型：同时设置进入和离开的过渡时间
+ object类型：分别设置进入和离开的过渡时间

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713078386861-e642c884-cd88-4b3e-9bf4-68ba2171b958.png)

### mode
动画在两个元素之间切换的时候存在的问题：会发现 Hello World 和 你好啊，李银河是同时存在的，这是因为默认情况下进入和离开动画是同时发生的

如果不希望同时执行进入和离开动画，那么我们需要设置transition的过渡模式：

+ in-out: 新元素先进行过渡，完成之后当前元素过渡离开；
+ out-in: 当前元素先进行过渡，完成之后新元素过渡进入；

### 动态组件的切换
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713079079871-cb442120-d049-4f84-a911-6a91bb6df4ab.png)

图中的home和about是两个组件，在点击按钮时，会切换show的值，component绑定的is就会随之改变

### appear初次渲染
默认情况下，首次渲染的时候是没有动画的，如果我们希望给他添加上去动画，那么就可以增加另外一个属性appear

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713079332091-96d380d3-751e-444e-87c5-6a44b24602be.png)

## 列表的过渡
### 概念
目前为止，过渡动画我们只要是针对单个元素或者组件的，要么是单个节点，要么是同一时间渲染多个节点中的一个。如果希望渲染的是一个列表，并且该列表中添加删除数据也希望有动画执行呢？这个时候要使用 <transition-group> 组件来完成；

使用<transition-group> 有如下的特点：

+ 默认情况下，它不会渲染一个元素的包裹器，但是你可以指定一个元素并以 tag 属性进行渲染

```vue
<transition-group tag="div">
  <template v-for="item in nums" :key="item">
    <span>{{item}}</span>
  </template>
</transition-group>
```

+ 过渡模式不可用，因为我们不再相互切换特有的元素(即不可以添加mode属性)
+ 内部元素总是需要提供唯一的key值
+ CSS 过渡的类将会应用在内部的元素中，而不是这个组/容器本身

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713080033007-baf1b499-e96d-4b09-b0fd-8caff7c47f18.png)

虽然新增的或者删除的节点是有动画的，但是对于哪些其他需要移动的节点是没有动画的，

可以通过使用一个新增的 v-move 的class来完成动画，它会在元素改变位置的过程中应

用，像之前的名字一样，可以通过name来自定义前缀

```vue
<template>
    <div class="app">
        <button @click="addNum">添加数字</button>
        <button @click="removeNum">删除数字</button>
        <button @click="shuffleNum">打乱数字</button>
        <transition-group tag="div" name="why">
            <template v-for="item in nums" :key="item">
                <li>{{ item }}</li>
            </template>
        </transition-group>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { shuffle } from 'underscore'

const nums = ref([1, 2, 3, 4, 5, 6, 7, 8, 9])

const addNum = () => {
    nums.value.splice(randomIndex(), 0, nums.value.length)
}

const removeNum = () => {
    nums.value.splice(randomIndex(), 1)
}

const shuffleNum = () => {
    nums.value = shuffle(nums.value)
}

const randomIndex = () => {
    console.log(Math.floor(Math.random() * nums.value.length))
    return Math.floor(Math.random() * nums.value.length)
}

const afterEnter = () => {
    console.log("after-enter")
}

</script>

<style scoped>
span {
    margin-right: 10px;
    display: inline-block;
}

.why-enter-from,
.why-leave-to {
    opacity: 0;
    transform: translateX(30px);
}

.why-move {
    transition: all 2s ease;
}

.why-leave-active {
    position: absolute;
}

.why-enter-active,
.why-leave-active {
    transition: all 2s ease;
}
</style>
```

# 响应式原理（面试重点）
## 推理过程
当数据发生变化时，使用到该数据的函数会被重新执行，但是怎么区分一个函数有没有使用该数据呢？

这里使用watchFn函数，如图

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713082954991-50d8e415-5772-4fde-8a39-39e3131c6436.png)

### 响应式依赖的收集
目前我们收集的依赖是放到一个数组中来保存的，但是这里会存在数据管理的问题，我们在实际开发中需要监听很多对象的响应式，这些对象需要监听的不只是一个属性，它们很多属性的变化，都会有对应的响应式函数，我们不可能在全局维护一大堆的数组来保存这些响应函数

所以我们要设计一个类，这个类用于管理某一个对象的某一个属性的所有响应式函数，相当于替代了原来的简单 reactiveFns 的数组

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713083145964-6048adec-db12-443b-a420-31e84f24d84f.png)

### 监听对象的变化
+ 通过 Object.defineProperty的方式（vue2）
+ 通过new Proxy的方式（vue3）

这里演示一下vue3的new Proxy

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713083280711-b305f564-18a3-4ed7-bc53-c3632a2e0e60.png)

### 对象的依赖管理
我们目前是创建了一个Depend对象，用来管理对于name变化需要监听的响应函数，但是实际开发中我们会有不同的对象，另外会有不同的属性需要管理，我们如何可以使用一种数据结构来管理不同对象的不同依赖关系呢？

WeakMap是JavaScript中的一种数据结构，它类似于Map，但具有一些不同之处。WeakMap对象是一组键/值对的集合，其中键是弱引用的。这意味着，当键不再被引用时，它会被垃圾回收机制自动移除，这样可以防止内存泄漏。

以下是WeakMap的一些特点和用法：

+ 弱引用键：WeakMap中的键是弱引用的，这意味着如果没有其他变量或对象引用这个键，它就会被垃圾回收，即使它存在于WeakMap中。
+ 只能使用对象作为键：在WeakMap中，只有对象可以作为键，而且这些对象必须是独一无二的，即不能重复。
+ 无法迭代：由于WeakMap中的键是弱引用的，无法获取WeakMap的所有键或值，也无法进行迭代。
+ 安全性：由于键是弱引用的，WeakMap在一定程度上可以防止内存泄漏和保护隐私数据，因为即使键被意外地丢弃了，WeakMap中相应的值也会被自动移除。

WeakMap通常用于存储与对象相关的私有数据或元数据，或者用于实现对象之间的一对一关系，同时希望在没有其他引用时自动清理相关数据。例如，在JavaScript中，WeakMap可以用于实现对象的私有属性或缓存数据，而无需担心内存泄漏问题。![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713083422951-a169f030-011e-4916-9ea2-61033198ca46.png)

### 对象依赖管理的实现
可以写一个getDepend函数专门来管理这种依赖关系

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713083709916-5b8c0568-7cb4-4068-8fb4-cdb916843c93.png)

### 正确的依赖收集
我们之前收集依赖的地方是在 watchFn 中，但是这种收集依赖的方式我们根本不知道是哪一个key的哪一个depend需要收集依赖，你只能针对一个单独的depend对象来添加你的依赖对象

那么正确的应该是在哪里收集呢？应该在我们调用了Proxy的get捕获器时，因为如果一个函数中使用了某个对象的key，那么它应该被收集依赖

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713083797268-9d4fbe48-70cc-4e78-92b4-b7b60113c9f3.png)

### 对Depend的重构
但是这里有两个问题：

+ 问题一：如果函数中有用到两次key，比如name，那么这个函数会被收集两次；
+ 问题二：我们并不希望将添加reactiveFn放到get中，以为它是属于Dep的行为；

所以我们需要对Depend类进行重构：

+ 解决问题一的方法：不使用数组，而是使用Set
+ 解决问题二的方法：添加一个新的方法，用于收集依赖

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713084124147-de8284b6-b1d9-4c2c-86d6-aab50eefdbb9.png)

### 创建响应式对象
我们目前的响应式是针对于obj一个对象的，我们可以创建出来一个函数，针对所有的对象都可以变成响应式对象

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713084168369-94bd58b3-04e2-4922-977a-6942048c2b47.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713084181650-df1bed92-5b1b-4196-8978-5166df87ec7e.png)

## Vue2的响应式原理
我们可以将reactive函数进行如下的重构，在传入对象时，我们可以遍历所有的key，并且通过属性存储描述符来监听属性的获取和修改，在setter和getter方法中的逻辑和前面的Proxy是一致的

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1713084359959-09e4d173-7091-419d-9da4-4e00fc8525b5.png)





