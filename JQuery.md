# 1-概述
JavaScript库：即library，是一个封装好的特定的集合（方法和函数）。就是在这个库中，封装了很多预先定义好的函数在里面，比如动画animate、hide、show,比如获取元素等

简单理解：就是一个JS文件，里面对原生js代码进行了封装，存放到里面。这样可以速高效的使用这些封装好的功能了。

jQuery就是一个JS库，目的是为了快速方便的操作DOM，里面基本都是函数（方法）。

# 2-基本用法
## 2.1-使用
+ 引入：在head中引入，<script src="jquery.js"></script>，注意路径
+ 使用：在body中<script>内写即可

## 2.2-入口函数
等DOM结构渲染完毕再执行内部代码，不必等到**所有**外部资源都加载完成，相当于原生js中的DOMContentLoaded

不同于原生js中的load事件，load是等页面文档、外部的js文件、css文件、图片都加载完毕才执行内部代码。

```javascript
//方法一
$(function(){
  .....
  })
//方法二
$(document).ready(function(){
  ...
}
```

## 2.3-顶级对象$
+ $是jQuery的别称，在代码中可以使用jQuery代替$，但一般为了方便，通常都直接使用$
+ $是JQuery的顶级对象，相当于原生JavaScript中的window，把元素利用$包装成jQuery对象，就可以调用jQuery的方法

## 2.4-DOM对象与JQuery对象
### 2.4.1-区别
用原生js获取的对象就是DOM对象

```javascript
var ddiv = document.querySelector('div'); //ddiv就是DOM对象
```

JQuery方法获取的对象就是JQuery对象

```javascript
$('div'); //$('div')就是一个JQuery对象
```

两个对象并不一样，DOM对象包含的信息更多，而JQ就比较少

JQ对象的本质是：利用$对DOM对象包装，进而产生一个新的对象，新对象用伪数组的形式存储

jQuery对象只能使用jQuery方法，DOM对象则使用原生的JavaScirpt属性和方法

```javascript
ddiv.style.display ='none';
$('div').style.display='none';
//$('div')是jQuery对象,不能使用原生js的属性和方法
```

### 2.4.2-转换
DOM对象和JQ对象是可以相互转换的

因为原生JS更大，里面有很多的属性和方法是JQ没有组合封装的，如果想要使用哪些属性和方法，就需要把JQ对象装换为DOM对象

DOM--->JQ：$('DOM对象')

JQ------>DOM：

+ $('JQ对象')[index]，index是索引号
+ $('JQ对象').get(index)，index是索引号

```javascript
//DOM->JQ
$('ddiv');
//JQ->DOM
$('div')[index]
$('div').get(index)
```

## 2.5-常用API
### 2.5.1-选择器
#### 基础选择器
$("选择器")，里面选择器直接写CSS选择器即可，但是要加引号(无所谓''或')

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700465107680-65e0074f-5683-47b9-9154-ec18d55c3ad4.png)

#### 层级选择器
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700465436831-76c51e0d-81b9-490c-80bb-6ead1ebd52eb.png)

#### 隐式迭代
JQ设置样式：$('div').css('属性', '值')

遍历内部 DOM 元素（伪数组形式存储）的过程就叫做隐式迭代。

简单理解：给匹配到的所有元素进行循环遍历，执行相应的方法，而不用我们再进行循环，简化我们的操作，方便我们调用

#### 筛选选择器
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700466395522-4cc0b694-e3b7-4169-b0d3-0d3392c4fec7.png)

#### 筛选方法
重点： parent()、children()、find()、siblings()、eq()  

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700466408118-e9e81dd8-d212-4c3f-b039-fecb688e4731.png)

#### 排他思想
想要多选一的效果，就需要排他思想，即当前元素设置样式，其余的兄弟元素清除样式。

```javascript
$(this).css('color','red');
$(this).siblings().css('color','blue');
```

### 2.5.2-样式操作
#### 操作css方法
jQuery 可以使用 css 方法来修改简单元素样式； 也可以操作类，修改多个样式。

参数只写属性名，则是返回属性值

```javascript
$(this).css('color');
```

参数是属性名，属性值，逗号分隔，是设置一组样式，属性必须加引号，值如果是数字可以不用跟单位和引号

```javascript
$(this).css('color', 'red');
```

参数可以是对象形式，方便设置多组样式。属性名和属性值用冒号隔开， 属性可以不用加引号

```javascript
$(this).css({ "color":"white","font-size":"20px"});
```

#### 操作类样式方法
作用等同于以前的 classList，可以操作类样式， 注意操作类里面的参数不要加点。

添加类

```javascript
$('div').addClass('current');
```

移除类

```javascript
$('div').removeClass('current');
```

切换类

```javascript
$('div').toggleClass('current')
```

#### 类操作与className的区别
原生 JS 中 className 会覆盖元素原先里面的类名。

jQuery 里面类操作只是对指定类进行操作，不影响原先的类名。

### 2.5.3-效果
#### 综述
JQ封装了很多动画效果

![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700467464866-42e5d616-fe6b-467a-ae66-724815154e25.png)

参数都可以省略， 无动画直接显示。

speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。

easing：(Optional) 用来指定切换效果，默认是“swing”(慢-快-慢)，可用参数“linear”(匀速)

fn: 回调函数，在动画完成时执行的函数，每个元素执行一次

#### 显示隐藏效果
+ show显示语法：show([speed,[easing],[fn]])
+ hide隐藏语法：hide([speed,[easing],[fn]])
+ toggle切换语法：toggle([speed,[easing],[fn]])

```javascript
$("button").eq(0).click(function(){
	$("div").show(1000,function(){
		alert(1);
	})
})

$("button").eq(1).click(function(){
	$("div").hide(1000,function(){
		alert(1);
	})
})

$("button").eq(3).click(function(){
	$("div").toggle(1000);
})
```

#### 事件切换
hover([over,]out)

+ over: 鼠标移到元素上要触发的函数（相当于mouseenter）
+ out: 鼠标移出元素要触发的函数（相当于mouseleave）
+ 如果只写一个函数，则鼠标经过和离开都会触发它

#### 滑动效果
+ 下滑：slideDown([speed,[easing],[fn]])
+ 上滑：slideUp([speed,[easing],[fn]])
+ 滑动切换：slideToggle([speed,[easing],[fn]])

```javascript
$("nav>li").mouseout(function(){
	$(this).children("ul").slideUp(200);
});

$(".nav>li").hover(function(){
	$(this)children("ul").slideDown(200);
});

$(".nav>li").hover(function(){
	$(this).children("ul").slideToggle();
});
```

#### 动画队列及其停止排队方法
动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行。

stop()

+ stop() 方法用于停止动画或效果
+ 注意： stop() 写到动画或者效果的**前面**， 相当于停止结束上一次的动画。

```javascript
$(".nav>li").hover(function(){
	$(this).children("ul").stop().slideToggle();
})；
```

#### 淡入淡出效果
+ 淡入：fadeIn([speed,[easing],[fn]])
+ 淡出： fadeOut([speed,[easing],[fn]])  
+ 淡入淡出切换：fadeToggle([speed,[easing],[fn]])  
+ 渐进： fadeTo([[speed],opacity,[easing],[fn]])  
    -  opacity 透明度必须写，取值 0~1 之间。 

```javascript
//淡入
$("button").eq(0).click(function(){
	$("div").fadeIn(1000);
});

//淡出
$("button").eq(1).click(function(){
	$("div").fadeOut(1000);
});

//淡入淡出切换
$("button").eq(2).click(function(){
	$("div").fadeToggle(1000);
});

//渐进
$("button").eq(2).click(function(){
	$("div").fadeTo(1000,0.5);
});
```

#### 自定义动画animate
animate(params,[speed],[easing],[fn])

params: 想要更改的样式属性，以对象形式传递，必须写。 属性名可以不用带引号， 如果是复合属性则需要采取驼峰命名法 ，如borderLeft，其余参数都可以省略。

```javascript
$(function(){
	$("button").click(function(){
		$("div").animate({
			left:500,
			top:300,
			opacity:.4,//原来透明度的40%
      width: 500
		},500);
	})
})
```

### 2.5.4-属性操作
#### prop()
设置或获取元素固有属性值，所谓元素固有属性就是元素本身自带的属性，比如 <a> 元素里面的 href ，比如 <input> 元素里面的 type

+ 获取：prop('属性')
+ 设置：prop('属性','属性值')

```javascript
<a href='www.xxx.com' title="你好"></a>

$(function(){
	console.log(("a").prop("href"));
	$("a"):prop("title","我们都挺好");
})
```

#### attr()
设置或获取元素自定义属性值

用户自己给元素添加的属性，称为自定义属性。 比如给 div 添加 index =“1”

+ 获取：attr(''属性'') // 类似原生 getAttribute()
+ 设置：attr(''属性'', ''属性值'') // 类似原生 setAttribute()

```javascript
console.log($("div").attr("index"));
$("div").attr("index",4);
```

#### data()
数据缓存，data() 方法可以在指定的元素上存取数据，并不会修改 DOM 元素结构。一旦页面刷新，之前存放的数据都将被移除。

+ 附加数据： data(''name'',''value'')  
+ 获取数据： date(''name'')  

```javascript
$("span").data("uname","andy");
console.log(("span").data("uname"));
```

### 2.5.5-内容文本值
主要针对元素的内容以及表单的值进行的操作

+ 普通元素内容html(相当于原生的innerHTML)
    - 获取：html()
    - 设置：html('内容')
+ 普通元素文本内容text()(相当于原生的innerText)
    - 获取：text()
    - 设置：text('内容')
+ 表单的值val()(相当于原生的value)
    - 获取：val()
    - 设置：val('内容')

实例：

```javascript
//1.获取设置元素内容html()
console.log($("div").html());
$("div").html("123");

//2.获取设置元素文本内容text()
console.log($("div").text());
$("div").text("123")

//3.获取设置表单值val()
console.log(("input").val());
$("input").va1("123");
```

### 2.5.6-元素操作
#### 遍历
jQuery 隐式迭代是对同一类元素做了同样的操作，如果想要给同一类元素做不同操作，就需要用到遍历。

语法1：$("div").each(function (index, domEle) { xxx; }）

+ each() 方法遍历匹配的每一个元素，主要用DOM处理。 
+ 里面的回调函数有2个参数： index 是每个元素的索引号(名字自定义); demEle 是每个DOM元素对象，不是jquery对象(名字自定义)
+ 所以要想使用jquery方法，需要给这个dom元素转换为jquery对象 $(domEle)

语法2：$.each(object，function (index, element) { xxx; }）

+ $.each()方法可用于遍历任何对象。主要用于数据处理，比如数组，对象
+ 里面的函数有2个参数： index 是每个元素的索引号; element 遍历内容

```javascript
//语法1
$("div").each(function(i,domEle){
	console.log(i);
	console.log(domEle);
}
//语法2
$.each(arr,function(i,ele){
	console.log(i);
	console.log(ele);
}
```

#### 创建
$('html元素')

```javascript
$('<li></li>'); 
```

#### 添加
+ 内部添加
    -  把内容放入匹配元素内部最后面： element.append(''内容'')  
    -  把内容放入匹配元素内部最前面： element.prepend(''内容'')  
+ 外部添加
    - 把内容放入目标元素后面：element.after(''内容'') 
    - 把内容放入目标元素前面：element.before(''内容'') 
+ 内部添加元素，生成之后，它们是父子关系。
+ 外部添加元素，生成之后，他们是兄弟关系。

```javascript
// 内部添加
$("ul").append(li);
$("ul").prepend(li);
// 外部添加
var div =  $("<div>我是后妈生的</div>");
$(".test").after(div);
$(".test").before(div);
```

#### 删除
+ 删除匹配的元素本身：element.remove() 
+ 删除匹配的元素集合中所有的子节点：element.empty() 
+ 清空匹配的所有子节点：element.html("") 
    - remove 删除元素本身。
    - empty() 和 html("") 作用等价，都可以删除元素里面的内容，只不过 html 还可以设置内容。

```javascript
$("ul").remove();
$("ul").empty();
$("u1").html("");
```

### 2.5.7-尺寸、位置操作
#### 尺寸
![](https://cdn.nlark.com/yuque/0/2023/png/29327577/1700528150820-240da260-2bd1-42b6-95c4-5f5a9ee438b5.png)

+ 以上参数为空，则是获取相应值，返回的是数字型。
+ 如果参数为数字，则是修改相应值。
+ 参数可以不必写单位

#### 位置
设置或获取元素偏移：offset() 

+ 设置或返回被选元素相对于**文档**的偏移坐标，跟父级没有关系。
+ 该方法有2个属性 left、top 
    - offset().top 用于获取距离文档顶部的距离
    - offset().left 用于获取距离文档左侧的距离。
+ 可以设置元素的偏移：offset({ top: 10, left: 30 });

获取元素偏移：position()

+ position() 方法用于返回被选元素相对于带有定位的父级偏移坐标，如果父级都没有定位，则以文档为准。
+ 该方法有2个属性 left、top
    - position().top 用于获取距离定位父级顶部的距离
    - position().left 用于获取距离定位父级左侧的距离。
+ 该方法只能获取。

设置或获取元素被卷去的头部和左侧：scrollTop/scrollLeft

+ scrollTop() 方法设置或返回被选元素被卷去的头部
+ 不跟参数是获取，参数为不带单位的数字则是设置被卷去的头部。 

# 3-事件
## 3.1-事件注册
语法：element.事件(function(){})

比如：$("div").click(function(){ 事件处理程序 })

其他事件和原生基本一致，比如mouseover、mouseout、blur、focus、change、keydown、keyup、resize、scroll 等

## 3.2-事件处理
### 3.2.1-绑定事件on()
在匹配元素上绑定一个或多个事件的事件处理函数

语法：element.on(events，[selector]，fn)  

+ events：一个或多个用空格隔开的事件类型，如"click" 或 "keydown" 
+ selector：元素的子元素选择器 。
+ fn：回调函数

优势：

+ 可以绑定多个事件，多个处理事件处理程序。  
+ 可以事件委派操作 。事件委派的定义就是，把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素  
+ 动态创建的元素，click() 没有办法绑定事件， on() 可以给动态生成的元素绑定事件  

```javascript
$("div").on({
	mouseover: function(){}, 
	mouseout: function(){},
	click: function(){}
});

//回调函数相同
$("div").on("mouseover mouseout", function() {
	$(this).toggleClass("current");
});

//事件委派
$('ul').on('click', 'li', function() {
	alert('hello world!');
});
```

### 3.2.2-解绑事件off()
可以移除通过 on() 方法添加的事件处理程序。

```javascript
$("p").off()  //解绑p元素所有事件处理程序
$("p").off("click")  //解绑p元素上面的点击事件 后面的foo是回调函数
$("ul").off("click","li");  //解绑事件委托
```

如果有的事件只想触发一次， 可以使用 one() 来绑定事件

### 3.2.3-自动触发事件trigger()
有些事件希望自动触发, 比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发。

+ 简写形式：element.click() 
+ 自动触发模式：element.trigger("type") 
+ 自动触发模式：element.triggerHandler(type)
    - triggerHandler模式不会触发元素的默认行为，这是和前面两种的区别。

```javascript
//第一种
$("p").on("click", function(){
	alert("hi~");
});
$("p").click();

//第二种
$("p").on("click", function(){
	alert("hi~");
});
$("p").trigger("click"); //此时自动触发点击事件，不需要鼠标点击

//第三种
$("input").on("focus",function(){
	$(this).val("你好吗")；
})；
$("input").triggerHandler("focus");
```

## 3.3-事件对象
事件被触发，就会有事件对象的产生。

阻止默认行为：event.preventDefault() 或者 return false 

阻止冒泡： event.stopPropagation()

# 4-其他方法
## 4.1-拷贝对象
如果想要把某个对象拷贝（合并） 给另外一个对象使用，此时可以使用 $.extend() 方法

语法：$.extend([deep], target, object1, [objectN])

+ deep：如果设为true 为深拷贝， 默认为false 浅拷贝
+ target：要拷贝的目标对象(接受)
+ object1：待拷贝到的第一个对象(发送)
+ objectN：待拷贝到的第N个对象
+ 浅拷贝：把被拷贝的对象复杂数据类型中的地址拷贝给目标对象，修改目标对象会影响被拷贝对象。
+ 深拷贝：前面加true， 完全克隆(拷贝的对象,而不是地址)，修改目标对象不会影响被拷贝对象。

## 4.2-多库共存
jQuery使用$作为标示符，其他 js 库也会用这$作为标识符，一起使用会引起冲突。所以需要一个解决方案，让jQuery和其他的js库不存在冲突，可以同时存在，这就叫做多库共存。

jQuery 解决方案：

+ 把里面的 $ 符号 统一改为 jQuery。 比如 jQuery(''div'')
+ jQuery 变量规定新的名称：$.noConflict()
    - 比如：var xx = $.noConflict();

## 4.3-JQ插件
jQuery 功能比较有限，想要更复杂的特效效果，可以借助于 jQuery 插件完成。这些插件也是依赖于jQuery来完成的，所以必须要先引入jQuery文件，因此也称为 jQuery 插件。

jQuery 插件常用的网站：

+ jQuery 插件库 [http://www.jq22.com/](http://www.jq22.com/) 
+ jQuery 之家 [http://www.htmleaf.com/](http://www.htmleaf.com/) 

jQuery 插件使用步骤：

1. 引入相关文件。（jQuery文件和插件文件）
2. 复制相关html、css、js (调用插件)。

 bootstrap 框架也是依赖于 jQuery 开发的，因此里面的 js插件使用 ，也必须引入jQuery 文件。  



