# 概述
## 技术选型
原生

+ 微信小程序：WXML，WXSS，JS；[微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/framework)
+ 支付宝小程序：AXML，ACSS，JS；[小程序文档 - 支付宝文档中心](https://opendocs.alipay.com/mini/developer)

框架

+ mpvue：是一个使用Vue开发小程序的前端框架，也是支持微信小程序、百度智能小程序，头条小程序和支付宝小程序；该框架在2018年之后就不再维护和更新了，所以目前已经被放弃
+ wepy：是由腾讯开源的，一款让小程序支持组件化开发的框架，通过预编译的手段让开发者可以选择自己喜欢的开发风格去开发小程序。该框架目前维护的也较少，在前两年还有挺多的项目在使用，不推荐使用
+ uni-app：是一个使用 Vue 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/飞书/QQ/快手/钉钉/淘宝）、快应用等多个平台。uni-app目前是很多公司的技术选型，特别是希望适配移动端App的公司
+ taro：是一个开放式 跨端 跨框架 解决方案，支持使用 React/Vue/Nerv 等框架来开发微信 / 京东 / 百度/支付宝/字节/QQ/ 飞书/ H5 / RN 等应用；taro因为本身支持React、Vue的选择，给了我们更加灵活的选择空间，特别是在Taro3.x之后，支持Vue3、React Hook写法等

## 基础知识
### 预备知识
1. 页面布局：WXML，类似HTML；
2. 页面样式：WXSS，几乎就是CSS(某些不支持，某些进行了增强，但是基本是一致的) 
3. 页面脚本：JavaScript+WXS(WeixinScript) ；

如果之前已经掌握了Vue或者React等框架开发，那么学习小程序是更简单的，因为里面的核心思想都是一致的（比如组件化开发、数据响应式、mustache语法、事件绑定等等）

### 开发准备
申请 AppID：[小程序AppID申请](https://mp.weixin.qq.com/cgi-bin/wx)，<font style="color:rgb(53, 53, 53);">wx96d8ba767f38b0ac</font>

选择开发工具：微信开发者工具/VSCode，这里选择微信开发者工具，

[微信开发者工具下载地址与更新日志 | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

### 项目结构
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718677122757-98f166c5-305e-426b-b26d-613d7d67a5a4.png)

### 小程序的架构模型
宿主环境：微信客户端（宿主环境为了执行小程序的各种文件：wxml文件、wxss文件、js文件）

当小程序基于 WebView 环境下时，WebView 的 JS 逻辑、DOM 树创建、CSS 解析、样式计算、Layout、Paint (Composite) 都发生在同一线程，在 WebView 上执行过多的 JS 逻辑可能阻塞渲染，导致界面卡顿。以此为前提，小程序同时考虑了性能与安全，采用了目前称为「双线程模型」的架构。

#### 双线程模型
1. WXML模块和WXSS样式运行于渲染层，渲染层使用WebView线程渲染（一个程序有多个页面，会使用多个WebView的线程）。
2. JS脚本（app.js/home.js等）运行于 逻辑层，逻辑层使用JsCore运行JS脚本。

这两个线程都会经由微信客户端（Native）进行中转交互。

### 小程序的配置文件
小程序的很多开发需求被规定在了配置文件中

+ project.config.json：项目配置文件, 比如项目名称、appid，
+ sitemap.json：小程序搜索相关的内容
+ app.json：全局配置
+ page.json：页面配置

#### project.config.json
[项目配置文件 | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/devtools/projectconfig.html)，这个文件一般不打开修改，而是点开详情页修改

可以在 project.config.json 文件中配置公共的配置，在 project.private.config.json 配置个人的配置

可以将 project.private.config.json 写到 .gitignore 避免版本管理的冲突。

#### sitemap.json
[小程序页面接入效果优化建议 | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/framework/sitemap.html)

微信现已开放小程序内搜索，开发者可以通过 sitemap.json 配置，或者管理后台页面收录开关来配置其小程序页面是否允许微信索引。

当开发者允许微信索引时，微信会通过爬虫的形式，为小程序的页面内容建立索引。当用户的搜索词条触发该索引时，小程序的页面将可能展示在搜索结果中。 

爬虫访问小程序内页面时，会携带特定的 user-agent：mpcrawler 及场景值：1129。需要注意的是，若小程序爬虫发现的页面数据和真实用户的呈现不一致，那么该页面将不会进入索引中。

#### app.json 
[全局配置 | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)

全局配置比较多, 这里写 3 个比较重要的，完整的查看官方文档

1. pages: 页面路径列表
    - 用于指定小程序由哪些页面组成，每一项都对应一个页面的 路径（含文件名） 信息
    - 小程序中所有的页面都是必须在pages中进行注册的
2. window: 全局的默认窗口展示
    - 用户指定窗口如何展示, 其中还包含了很多其他的属性
3. tabBar: 顶部tab栏的展示
    - ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718703578508-a0596fe7-0121-4c6e-b419-5459b0183eca.png)

#### page.json 
app.json 中的部分配置，也支持对单个页面进行配置，可以在页面对应的 .json 文件来对本页面的表现进行配置

页面中配置项在当前页面会覆盖 app.json 中相同的配置项（样式相关的配置项属于 app.json 中的 window 属性，但这里不需要额外指定 window 字段），具体的取值和含义可参考全局配置文档中说明

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718703935206-bd4b568f-98d6-42d8-994c-d3bdb82cf88a.png)

#### app.js
[App(Object object) | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html)

每个小程序都需要在 app.js 中调用 App 函数注册小程序

```javascript
App({
  ...
})
```

在注册时, 可以绑定对应的生命周期函数，在生命周期函数中, 执行对应的代码 

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718784611357-a85337f2-bca8-4a1a-a2cf-7d92a8110e2d.png)

注册App时，一般会做什么

+ 判断小程序的进入场景
+ 监听生命周期函数，在生命周期中执行对应的业务逻辑，比如在某个生命周期函数中进行登录操作或者请求网络数据
+ 设置一些共享数据

##### 判断进入场景
群聊会话中打开、小程序列表中打开、微信扫一扫打开、另一个小程序打开

如何确定： 在onLaunch和onShow生命周期回调函数中，会有options参数，其中有scene值

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718784925857-60fa5b3e-3e8f-427d-8412-5ee5507299c5.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718784697767-b05edf7e-643a-436f-b23f-0f14f51b7829.png)

##### 定义全局的数据
```javascript
App({
  //不是响应式的，都是固定数据
  globalData:{
    token:"123",
    userInfo:{name:"jack"}
  }
})
```

```javascript
Page({
 data:{
   //定义数据
   userInfo:{},
   token:""
 },
 onLoad(){
   //获取app实例对象
  const app = getApp()
   //从app实例对象中获取数据
   const token = app.globalData.token
   const userInfo = app.globalData.userInfo
   //设置数据
   this.setData({
     userInfo,token
     //useInfo:userInfo
   })
 }
})
```

```xml
<view>{{userInfo.name}}-{{token}}</view>
```

##### 生命周期函数
在生命周期函数中，完成应用程序启动后的初始化操作

+ 登录操作（这个后续会详细讲解）
+ 读取本地数据（类似于token，然后保存在全局方便使用）
+ 请求整个应用程序需要的数据

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718785241973-f6ba3619-47b0-4382-9e97-8bb27c26aac9.png)

#### page.js
 [Page(Object object) | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html)

小程序中的每个页面, 都有一个对应的js文件, 其中调用Page函数注册页面

注册一个Page页面时，一般需要做什么

+ 在生命周期函数中发送网络请求，从服务器获取数据
+ 初始化一些数据，以方便被wxml引用展示
+ 监听wxml中的事件，绑定对应的事件函数
+ 其他一些监听（比如页面滚动、下拉刷新、上拉加载更多等）

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718789493357-70d995d1-d9c8-447f-98b8-e65917350925.png)

# 常用的内置组件
### Text 文本组件
[text | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/component/text.html)

用来显示文本，类似 span，是行内元素

### Button 按钮组件
[button | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/component/button.html)

用于创建按钮，默认是块级元素

open-type 属性：用户获取一些特殊性的权限，可以绑定一些特殊的事件![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718790455028-f1039b1d-f521-4b85-8d35-8b1f076eb6b7.png)

### View 视图组件
[view | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/component/view.html)

用作容器组件，块级元素

### Image 图片组件
[image | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)

用于显示图片 ，默认是行内块元素，有默认宽度和高度(320*240)

src可以是本地图片，也可以是网络图片

图片的重要属性 mode

+ `mode="..."`
+ 如果是方位词，会裁剪图片，只剩那个方位，不缩放图片，比如`mode="top right"`
+ <font style="color:rgb(0, 0, 0);background-color:rgb(251, 251, 251);">scaleToFill/aspectFit/aspectFill：给图片设置缩放模式</font>
+ <font style="color:rgb(0, 0, 0);background-color:rgb(251, 251, 251);">最常用的 mode</font>
    - <font style="color:rgb(0, 0, 0);background-color:rgb(251, 251, 251);">widthFix：缩放模式，宽度不变，高度自动变化，保持原图宽高比不变</font>
    - <font style="color:rgb(0, 0, 0);background-color:rgb(251, 251, 251);">heightFix：高度不变，宽度自动变化</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(251, 251, 251);"> wx.chooseMedia：拍摄或从手机相册中选择图片或视频</font>

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718846406887-03f8c327-7eab-4a5e-82a8-35e29e905532.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718846392332-26ba9ae3-8b59-4123-9ae2-8d209499de96.png)

<font style="color:rgb(0, 0, 0);background-color:rgb(251, 251, 251);"></font>

### ScrollView 滚动组件
[scroll-view | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html)，实现局部滚动

实现滚动效果必须添加scroll-x或者scroll-y属性

使用竖向滚动时，需要给scroll-view一个固定高度，通过 WXSS 设置 height

横向滚动需打开 enable-flex 以兼容 WebView，`<scroll-view scroll-x enable-flex style="flex-direction: row;"/>`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718847266836-07dea0ad-05ec-4834-9169-832565889475.png)

### 组件的共同属性
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718847190275-b0071695-ecc7-466f-90b1-a292417c24d0.png)

# WXSS-WXML-WXS语法
## WXSS
#### 写法
页面样式的三种写法：行内样式、页面样式、全局样式（app.wxss）

权重：行内样式 > 页面样式 > 全局样式

行内样式：`<view style='color:red；font-size:20px;'>行内样式</view>`

三种样式可以同时写

`<view style='color:orange′ class='page app'>AAA</view>`

#### 选择器
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718848212633-9359c3f0-5586-4b4b-bf44-7cbdf7a02f72.png)

#### 扩展-尺寸单位
rpx（responsive pixel）: 可以根据屏幕宽度进行自适应，规定屏幕宽为750rpx。

如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素

建议开发微信小程序时，设计师可以用 iPhone6 作为视觉稿的标准

## WXML
### 特点
+ 类似于HTML代码：比如可以写成单标签，也可以写成双标签
+ 必须有严格的闭合：没有闭合会导致编译错误
+ 大小写敏感：class和Class是不同的属性

### Mustache
和 vue 一样，{{....}}

### 条件渲染
wx:if – wx:elif – wx:else

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718849570680-2c4b9530-9ee9-423a-b21f-e4822fe5d0d6.png)

hidden和wx:if的区别

+ hidden控制隐藏和显示是控制是否添加hidden属性
+ wx:if是控制组件是否渲染

### 列表渲染
可以使用wx:for来遍历一个数组(字符串/数字)

`<view wx:for="{{......}}">{{item}}</view>`

注意中间是 Mustache{{...}}，如果不写 Mustache，会默认当成字符串来遍历

#### item-index
默认情况下，遍历后在wxml中可以使用变量index 作为索引，内容是 item(默认是固定的)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718849984342-8d7ff9ca-6ea7-4725-b401-d221c667a713.png)

但是某些情况下，我们可能想使用其他名称，或者当出现多层遍历时，名字会重复，所以需要指定 item 和 index 的名称

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718850247696-22ab04a9-f899-410e-998d-8cc15bac2d17.png)

#### block
某些情况下，我们需要使用 wx:if 或 wx:for时，可能需要包裹一组组件标签

<block/> 并不是一个组件，它仅仅是一个包装元素，不会在页面中做任何渲染，只接受控制属性

使用block 的好处

+ 将需要进行遍历或者判断的内容进行包裹
+ 将遍历和判断的属性放在block便签中，提高代码的可读性

#### key
wx:key 的值以两种形式提供

+ 字符串：代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变
+ 保留关键字 *this：代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1718850606827-fa50cf3d-1fc9-46a6-8159-a42acd0e165c.png)

## WXS
### 基本知识
WXS（WeiXin Script）是小程序的一套脚本语言，结合 WXML，可以构建出页面的结构。

官方：WXS 与 JavaScript 是不同的语言，有自己的语法，并不和 JavaScript 一致。（不过基本一致）

为什么要设计WXS语言：在WXML中是不能直接调用Page/Component中定义的函数的，但是某些情况, 希望使用函数来处理WXML中的数据(类似于Vue中的过滤器)，这个时候就使用WXS了

WXS使用的限制和特点：

+ WXS 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行
+ WXS 的运行环境和其他 JavaScript 代码是隔离的，WXS 中不能调用其他 JavaScript 文件中定义的函数，也不能调用小程序提供的API
+ 由于运行环境的差异，在 iOS 设备上小程序内的 WXS 会比 JavaScript 代码快 2 ~ 20 倍。在 android 设备 上二者运行效率无差异

### 写法
WXS有两种写法

+ 写在<wxs>标签中，在 wxml 文件中，需要命名 `<wxs module="xxx"></wxs>`
+ 写在以.wxs结尾的文件中，一般保存在 utils 目录中，在使用时导入`<wxs module="xxx" src="/utils/xxx"></wxs>`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719044269698-650891e8-2ab8-40a2-abf4-97779ad7f4c2.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719044670763-63ba7dd4-54c8-44bf-8292-aba214936369.png)

# 事件处理
## 事件监听
事件可以将用户的行为反馈到逻辑层进行处理。事件可以绑定在组件上，当触发事件时，就会执行逻辑层中对应的事件处理函数。事件对象可以携带额外信息，如 id, dataset, touches

事件是通过bind/catch这个属性绑定在组件上的(key="value")，key以bind或catch开头, 从1.5.0版本开始, 可以在bind和catch后加上一个冒号

在当前页面的Page构造器中定义对应的事件处理函数, 如果没有对应的函数, 触发事件时会报错

某些组件会有自己特性的事件类型，大家可以在使用组件时具体查看对应的文档，比如input有bindinput/bindblur/bindfocus等

下面是常见的公共事件：

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719108797950-e2f60bb0-87fc-419e-8193-e1d3ce138fba.png)

## 事件对象 event
当某个事件触发时, 会产生一个事件对象, 并且这个对象被传入到回调函数中

![event的属性](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719109209843-d41d9664-9338-43e4-a253-edfe6d042509.png)

### currentTarget和target的区别
+ target：是触发事件的源头，即事件最初发生的地方。
    - 在事件冒泡过程中，target表示最先被点击的元素，即事件最初触发的元素。
    - 例如，如果你点击了一个子元素，那么事件的target就是这个子元素。
+ currentTarget：是事件绑定的当前元素，即事件处理函数当前正在处理的元素。
    - 在事件冒泡过程中，currentTarget表示当前正在处理事件的元素，它不会随着事件冒泡而改变。
    - 例如，如果你在父元素上绑定了事件，并且点击了子元素，那么事件的currentTarget就是这个父元素。
+ 这种区别在处理事件冒泡和事件委托等复杂交互时非常有用，可以帮助我们精确地获取和操作相关元素的信息
+ 案例

```javascript
<view id="outer" bindtap="handleTapOuter">
  <view id="inner" bindtap="handleTapInner">
      点我
  </view>
</view>

Page({
  handleTapOuter: function(event) {
    console.log("Outer View clicked.");
    console.log("event.target.id:", event.target.id);
    console.log("event.currentTarget.id:", event.currentTarget.id);
  },
  handleTapInner: function(event) {
    console.log("Inner View clicked.");
    console.log("event.target.id:", event.target.id);
    console.log("event.currentTarget.id:", event.currentTarget.id);
  }
})

```

1. 当我们点击内层容器(id="inner")时，事件处理程序handleTapInner被触发
    - event.target.id会打印出被点击的具体元素的id，即inner
    - event.currentTarget.id会打印出当前绑定事件处理函数的元素的id，即outer，因为点击事件冒泡到了外层容器上
2. 当我们点击外层容器(id="outer")时，事件处理程序handleTapOuter被触发
    - event.target.id会打印出被点击的具体元素的id，即outer，因为外层容器就是被点击的元素
    - event.currentTarget.id同样会打印出当前绑定事件处理函数的元素的id，即outer

###  touches和changedTouches的区别
1. touches:
    - 是一个包含所有当前屏幕上所有触摸点信息的列表
    - 它是一个包含了所有当前触摸点的列表，每个触摸点是一个对象，包含了触摸点的详细信息，如位置、标识符等
    - 当多个手指同时触摸屏幕时，touches会包含所有触摸点的信息
    - touches是一个即时更新的列表，会随着手指的移动和触摸状态的改变而更新
2. changedTouches:
    - changedTouches是一个包含了最近一次触摸屏幕上发生变化的触摸点的列表
    - 它描述了最近一次触摸事件中发生变化的触摸点，无论是新增的触摸点、移动的触摸点还是结束的触摸点
    - 通常在touchstart、touchmove和touchend事件的处理函数中，可以通过changedTouches来获取当前触摸事件中发生变化的触摸点信息
+ touches适用于需要实时跟踪多点触控的场景
+ changedTouches适用于需要处理触摸事件的增加、移动和结束等情况的场景

## 事件参数传递方法
当视图层发生事件时，某些情况需要事件携带一些参数到执行的函数中, 这个时候就可以通过data-属性来完成：

+ 格式：data-xxx
+ 获取：event.currentTarget.dataset.xxx

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719110689982-bf9377dc-8b4a-4d54-86f2-acbb75af7950.png)

## 事件传递案例练习
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719112074850-5bc2566e-7988-435b-a99c-6d068feb2dec.png)



# 组件化
## 自定义组件
### 创建
自定义组件由 json wxml wxss js 4个文件组成

1. 会先在根目录下创建一个文件

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719125657125-6459415f-f886-470f-baa8-bce520156b51.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719125699590-b1356514-24b1-4dc4-b578-dc3ae8e43e9d.png)

2. components,：里面存放我们之后自定义的公共组件
3. my-cpn: 包含对应的四个文件



自定义组件的步骤：

1. 首先需要在 json 文件中进行自定义组件声明（component：true）

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719125798109-12fd72de-b7a6-43e5-b437-10e343a62532.png)

2. 在wxml中编写属于我们组件自己的模板
3. 在wxss中编写属于我们组件自己的相关样式
4. 在js文件中, 可以定义数据或组件内部的相关逻辑(后续使用)

### 注意事项
+ 如果要在某个组件中使用别的组件，需要配置 json 的 usingComponents![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719125989895-d50f681f-90e4-4ce8-8405-17bd485d70e9.png)
+ 自定义组件也是可以引用自定义组件的，使用usingComponents 字段
+ 自定义组件和页面所在项目根目录名 不能以“wx-”为前缀，否则会报错
+ 如果在app.json的usingComponents声明某个组件，那么所有页面和组件可以直接使用该组件

## 组件样式
课题一：组件内的样式对外部样式的影响

+ 组件内的class样式，只对组件wxml内的节点生效，对于引用组件的Page页面不生效
+ 组件内不能使用id选择器、属性选择器、标签选择器

课题二：外部样式对组件内样式的影响

+ 外部使用class的样式，只对外部wxml的class生效，对组件内是不生效的
+ 外部使用了id选择器、属性选择器不会对组件内产生影响
+ 外部使用了标签选择器，会对组件内产生影响

课题三：如何让class可以相互影响

+ 在Component对象中，可以传入一个options属性，其中options属性中有一个stylelsolation（隔离）属性
+ ![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719126509237-787f4d08-5a92-46a1-bda3-3dcb0f57241a.png)
+ stylelsolation有三个取值：
    - isolated：互不影响
    - apply-shared：页面影响组件，组件不影响页面
    - shared：相互影响

## 组件的通信
组件内展示的内容（数据、样式、标签），并不是在组件内写死的，而且可以由使用者来决定

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719126219466-9d1dbeda-bd41-47e3-b250-2c3f7dff5ac3.png)

### properties-传数据
大部分情况下，组件只负责布局和样式，内容是由使用组件的对象决定的，所以，经常需要从外部传递数据给组件，让组件来进行展示

支持的类型：String、Number、Boolean、Object、Array、null（不限制类型）

默认值：可以通过value设置默认值

![js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719127117379-055f9309-c9bf-4cd2-a78b-6be56a8e8c12.png)

### externalClasses-传样式
有时候，不希望将样式在组件内固定不变，而是外部可以决定样式这个时候，可以使用externalClasses属性

1. 在Component对象中，定义externalClasses属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719127353478-26c593f8-25fe-478b-b108-1c4c720fefb7.png)

2. 在组件内的wxml中使用externalClasses属性中的class![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719127424367-abc627fc-1c48-4a70-ac1b-61dc07a9baed.png)
3. 在页面中传入对应的class，并且给这个class设置样式![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719127491339-c05ab677-51a0-4a98-bd95-474536bfd0b5.png)

### 组件向外部传递事件
#### 自定义事件
组件内部：

1. 在组件的 WXML 文件中，定义一个触发事件的元素（比如按钮、视图等）并绑定一个触发事件，如 bindtap
2. 在触发事件的处理函数中，通过 this.triggerEvent() 方法来触发自定义事件，并传递需要传递的数据，数据可以是变量或者对象

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719127700897-c52516d5-a041-40fe-8e97-266cfb4a86f7.png)

组件的使用者（页面或父组件）

1. 在组件标签上使用 bind:customEvent 或 catch:customEvent 来监听自定义事件
2. 在对应的事件处理函数中，可以获取到组件传递过来的数据

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719127741091-03264665-d74d-496b-8319-c1f557d6e125.png)

#### 页面直接调用组件方法
可在父组件里调用 this.selectComponent ，获取子组件的实例对象。

调用时需要传入一个匹配选择器 selector，如：this.selectComponent(".my-component")

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719128316037-d2281080-b2e3-44a2-8ea4-3cc3b03329a9.png)

#### 触发事件-----AI 补充
组件还可以直接触发页面或父组件中定义的事件。

这种方式需要确保在组件的 JSON 配置文件中声明了 relations，并且通过 this.selectComponent() 方法获取到组件实例，然后调用其方法触发事件

```javascript
// custom-component.js
Component({
  relations: {
    '../parent-component/parent-component': {
      type: 'parent', // 关联的组件路径
      linked(target) {
        // 在关联时，可以做一些初始化操作
      }
    }
  },
  methods: {
    triggerOuterEvent() {
      const parentComponent = this.getRelationNodes('../parent-component/parent-component')[0];
      if (parentComponent) {
        parentComponent.triggerOuterEvent({ key: 'value' });
      }
    }
  }
})
```

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719127884010-42cd1f0b-40ed-452f-abc2-7a3d79c1782f.png)

## 组件插槽
### 单个插槽
除了内容和样式可能由外界决定之外，也可能外界想决定显示的方式，比如我们有一个组件定义了头部和尾部，但是中间的内容可能是一段文字，也可能是一张图片，或者是一个进度条。

在不确定外界想插入什么其他组件的前提下，可以在组件内预留插槽

```html
<view class="container">
    <!-- 插槽位置 -->
    <slot></slot>
</view>
```

`<view class="content">这里是插槽内容</view> `是被传递给子组件中的插槽的内容

```html
<custom-component>
    <view class="content">这里是插槽内容</view>
</custom-component>
```

注意：

+ 插槽可以用于传递任意类型的内容，包括文本、组件、样式等
+ 插槽的样式可以在父组件中定义，影响传入的内容的显示效果
+ 插槽可以帮助实现组件的通用性和灵活性，适用于多种情况下的布局和样式需求

### 多个插槽
如果需要多个插槽，可以在组件中定义多个插槽，每个插槽可以有自己的名称：

```html
<view class="container">
    <!-- 命名插槽 -->
    <slot name="header"></slot>
    <slot></slot>
</view>
```

```html
<custom-component>
    <view slot="header" class="header">这里是头部插槽内容</view>
    <view class="content">这里是默认插槽内容</view>
</custom-component>
```

### behaviors 混入
behaviors（行为）是一种可复用的代码块，可以定义组件的公共行为

+ 通过使用behaviors，可以将组件的生命周期、数据、属性等进行封装，并在多个组件之间进行共享
+ 组件引用它时，它的属性、数据和方法会被合并到组件中，生命周期函数也会在对应时机被调用
+ 每个组件可以引用多个 behavior ，behavior 也可以引用其它 behavior 

```javascript
// 定义一个名为highlight的behavior
let highlightBehavior = {
  data: {
    // 初始数据
    highlighted: false
  },
  methods: {
    // 自定义方法
    toggleHighlight: function() {
      this.setData({
        highlighted: !this.data.highlighted
      });
    }
  }
};

// 使用highlight behavior的组件
Component({
  behaviors: [highlightBehavior], // 在behaviors字段中引入highlight behavior
  data: {
    content: '这是一个使用highlight behavior的组件'
  },
  methods: {
    onTap: function() {
      // 调用highlight behavior中的方法
      this.toggleHighlight();
    }
  }
});
```

![导入behaviors](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719219959866-4bb26785-3280-4159-a907-e256747f6624.png)

## 组件的生命周期
### 组件自己的生命周期
组件的生命周期，指的是组件自身的一些函数，这些函数在特殊的时间点或遇到一些特殊的框架事件时被自动触发。

其中，最重要的生命周期是 created attached detached ，包含一个组件实例生命流程的最主要时间点

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719220146450-f698406a-64d4-455a-a38c-3e36bccc7b96.png)

### 组件所在页面的生命周期
还有一些特殊的生命周期，它们并非与组件有很强的关联，但有时组件需要获知，以便组件内部处理。这样的生命周期称为“组件所在页面的生命周期”，在 pageLifetimes 定义段中定义

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719220204705-410b1ec0-ee83-4816-a32e-4be3dd81abbf.png)

## Component 构造器
里面到底可以写什么

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719220310747-48262829-53c1-4e3e-a6a3-79594090b777.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719220321809-5de81464-3db0-4245-be15-880ccf7595e6.png)

# 小程序系统 API 调用
## 网络请求 API 和封装
### wx:request
微信提供了专属的API接口,用于网络请求: wx.request(Object object)

比较关键的几个属性解析

+ url：必选参数，表示要请求的地址，可以是相对路径或绝对路径
+ data：可选参数，表示要发送的数据，可以是对象或字符串。如果是GET请求，数据会附加在url后面
+ header：可选参数，设置请求的 header，如设置 token、Content-Type 等
+ method：可选参数，指定请求的方法，如 GET、POST 等，默认为 GET
+ dataType：可选参数，指定返回的数据格式，支持 json、text 等，默认为 json
+ responseType：可选参数，指定响应的数据类型，可设置为 text 或 arraybuffer，默认为 text
+ success、fail、complete：请求成功、失败和请求完成后的回调函数，其中 success 必选，其他两个可选

```javascript
wx.request({
    url: 'string', 
    data: {}, 
    header: {}, 
    method: 'GET', 
    dataType: 'json', 
    responseType: 'text', 
    success: function(res) {
        ...
    },
    fail: function(err) {
        ...
    },
    complete: function() {
       ...
    }
})
```

### 网络请求域名配置
每个微信小程序需要事先设置通讯域名，小程序只可以跟指定的域名进行网络通信。

+ 小程序登录后台 – 开发管理 – 开发设置 – 服务器域名
+ 验证是否备案、合法等

## 展示弹窗
小程序中展示弹窗有四种方式: showToast、showModal、showLoading、showActionSheet

### showToast
showToast 方法用于显示轻量级提示框，常用于操作成功、失败等简短信息的提示

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719904200623-45c92f46-be46-4c8a-b911-111cf29269e0.png)

```javascript
wx.showToast({
    title: '提示的内容',
    icon: 'success', // 提示图标，有效值为'none'、'success'、'loading'
    duration: 2000, // 提示的延迟时间，默认为1500ms
    mask: false, // 是否显示透明蒙层，防止触摸穿透，默认为false
    success: function() {
        // 提示框显示成功的回调
    },
    fail: function() {
        // 提示框显示失败的回调
    },
    complete: function() {
        // 提示框显示结束的回调（成功或失败都会执行）
    }
})
```



### showModal
showModal 方法用于显示模态弹窗，常用于警示性操作确认或交互提示

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719904208749-fa489b47-d85a-4f8b-bc93-deb9e2cc5d4f.png)

```javascript
wx.showModal({
    title: '提示',
    content: '这是一个模态弹窗',
    showCancel: true, // 是否显示取消按钮，默认为true
    cancelText: '取消', // 取消按钮的文字，默认为'取消'
    cancelColor: '#000000', // 取消按钮的文字颜色，默认为'#000000'
    confirmText: '确定', // 确定按钮的文字，默认为'确定'
    confirmColor: '#3CC51F', // 确定按钮的文字颜色，默认为'#3CC51F'
    success: function (res) {
        if (res.confirm) {
            // 用户点击确定按钮时的回调函数
        } else if (res.cancel) {
            // 用户点击取消按钮时的回调函数
        }
    }
})
```

### showLoading
showLoading 方法用于显示加载提示框，表示正在加载中，常用于网络请求或其他需要等待的场景

```javascript
wx.showLoading({
    title: '加载中',
    mask: true, // 是否显示透明蒙层，默认为false
    success: function() {
        // 加载提示显示成功的回调
    },
    fail: function() {
        // 加载提示显示失败的回调
    },
    complete: function() {
        // 加载提示显示结束的回调（成功或失败都会执行）
    }
})
```

### showActionSheet
showActionSheet 方法用于显示操作菜单，用户点击后可以选择不同的操作

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719904241699-33ead76a-3ff9-4395-ba9a-cc60ad1e1209.png)

```javascript
wx.showActionSheet({
    itemList: ['选项一', '选项二', '选项三'],
    itemColor: '#333', // 按钮的文字颜色，默认为'#000000'
    success: function (res) {
        if (!res.cancel) {
            console.log('用户点击了', itemList[res.tapIndex]);
        }
    }
})
```

## 页面分享
小程序中有两种分享方式

1. 点击右上角的菜单按钮，之后点击转发
2. 点击某一个按钮，直接转发

当转发给好友一个小程序时，通常小程序中会显示一些信息，通过 onShareAppMessage决定这些信息的展示，当用户点击页面右上角的分享按钮时，会触发该函数，开发者可以在其中自定义分享内容

+ title：分享的标题，用户可以看到这个标题
+ path：分享的路径，一般是当前页面的路径，可以带参数，如 /pages/index/index?id=123
+ imageUrl：自定义分享的图片，显示在分享卡片上（可选）。注意图片必须是网络地址或者本地临时路径，否则会分享失败
+ success：分享成功时的回调函数，参数 res 是一个对象，可以通过它获取分享的详细信息
+ fail：分享失败时的回调函数，参数 res 是一个对象，可以通过它获取失败的原因

```javascript
Page({
  onShareAppMessage: function () {
    return {
      title: '自定义分享标题',
      path: '/pages/index/index', 
      imageUrl: '/images/share-img.png', 
      success: function (res) {
        ...
      },
      fail: function (res) {
        ...
      }
    }
  }
})
```

## 设备信息
需要获取当前设备的信息，用于手机信息或者进行一些适配工作,小程序提供了相关API：wx.getSystemInfo()

当调用成功时，success 回调函数将会收到一个包含设备信息的对象 res，对象中包含以下属性：

+ res.model：设备型号
+ res.pixelRatio：设备的像素比，例如 2 表示 2 倍像素密度
+ res.windowWidth：可使用窗口宽度
+ res.windowHeight：可使用窗口高度
+ res.language：设备的语言
+ res.version：微信版本号
+ res.platform：客户端平台，如 "ios"、"android" 

```javascript
wx.getSystemInfo({
  success: function(res) {
    console.log(res.model) // 设备型号
    console.log(res.pixelRatio) // 设备像素比
    console.log(res.windowWidth) // 窗口宽度
    console.log(res.windowHeight) // 窗口高度
    console.log(res.language) // 设备的语言
    console.log(res.version) // 微信版本号
    console.log(res.platform) // 客户端平台
  }
})
```

## 位置信息
开发中需要经常获取用户的位置信息，以方便给用户提供相关的服务，可以通过API获取：wx.getLocation()

参数

+ type：坐标类型，可选值为 'wgs84' 和 'gcj02'
    - 默认为 'wgs84'，返回 gps 坐标
    - 'gcj02' 返回的是中国测绘坐标，可用于微信的地图接口
+ success：获取地理位置成功时的回调函数，返回一个对象 res，包含以下属性
    - res.latitude：纬度，浮点数，范围为 -90~90。负数表示南纬
    - res.longitude：经度，浮点数，范围为 -180~180。负数表示西经
    - res.speed：速度，浮点数，单位 m/s
    - res.accuracy：位置的精确度，单位 m

```javascript
wx.getLocation({
  type: 'wgs84', 
  success: function(res) {
    var latitude = res.latitude
    var longitude = res.longitude
    var speed = res.speed
    var accuracy = res.accuracy
  }
})
```

### 权限
对于用户的关键信息，需要获取用户的授权后才能获得，可以在 app.json 中配置

```json
{
  "pages": ["pages/index/index"],
  "permission": {
    "scope.userLocation": {
      "desc": "你的位置信息将用于小程序位置接口的效果展示"
    }
  }
}
```

## Storage 存储
### 同步存取数据的方法
wx.setStorageSync(string key, any data)

+ 同步将数据存储到本地缓存中
+ key 是数据的键名，data 是要存储的数据

any wx.getStorageSync(string key)

+ 同步从本地缓存中获取指定 key 对应的内容
+ 返回存储的数据，如果不存在，则返回空字符串或指定的默认值

wx.removeStorageSync(string key)

+ 同步移除本地缓存中指定 key 的数据

wx.clearStorageSync()

+ 同步清理本地缓存数据，将所有缓存数据全部清除

```javascript
// 同步存储数据到本地缓存
try {
  wx.setStorageSync('username', 'Alice');
  console.log('数据存储成功');
} catch (e) {
  console.error('数据存储失败:', e);
}

// 同步获取本地缓存中的数据
try {
  const username = wx.getStorageSync('username');
  if (username) {
    console.log('获取到的用户名:', username);
  } else {
    console.log('未找到对应的用户名');
  }
} catch (e) {
  console.error('数据获取失败:', e);
}

// 同步移除本地缓存中的数据
try {
  wx.removeStorageSync('username');
  console.log('数据移除成功');
} catch (e) {
  console.error('数据移除失败:', e);
}
```

### 异步存储数据的方法
wx.setStorage(Object object)

+ key：数据的键名
+ data：要存储的数据
+ success：存储成功的回调函数
+ fail：存储失败的回调函数
+ complete：存储操作完成的回调函数（可选）

wx.getStorage(Object object)

+ key：数据的键名
+ success：获取成功的回调函数，回调参数中包含 data 字段
+ fail：获取失败的回调函数
+ complete：获取操作完成的回调函数（可选）

wx.removeStorage(Object object)

+ key：数据的键名
+ success：移除成功的回调函数
+ fail：移除失败的回调函数
+ complete：移除操作完成的回调函数（可选）

wx.clearStorage(Object object)

+ success：清理成功的回调函数
+ fail：清理失败的回调函数
+ complete：清理操作完成的回调函数（可选）

```javascript
// 异步存储数据到本地缓存
wx.setStorage({
  key: 'username',
  data: 'Bob',
  success: function(res) {
    console.log('数据存储成功');
  },
  fail: function(err) {
    console.error('数据存储失败:', err);
  }
});

// 异步获取本地缓存中的数据
wx.getStorage({
  key: 'username',
  success: function(res) {
    if (res.data) {
      console.log('获取到的用户名:', res.data);
    } else {
      console.log('未找到对应的用户名');
    }
  },
  fail: function(err) {
    console.error('数据获取失败:', err);
  }
});

// 异步移除本地缓存中的数据
wx.removeStorage({
  key: 'username',
  success: function(res) {
    console.log('数据移除成功');
  },
  fail: function(err) {
    console.error('数据移除失败:', err);
  }
});
```

## 页面跳转
### 页面跳转
#### wx.navigateTo
可以用于跳转到应用内的**普通**页面。跳转后，新页面会被加入到页面栈中

```javascript
// 在当前页面跳转到目标页面
wx.navigateTo({
  url: '/pages/target/target', // 目标页面的路径，注意是以 / 开头的绝对路径
  success: function(res) {
    console.log('跳转成功');
  },
  fail: function(err) {
    console.error('跳转失败:', err);
  }
});
```

#### wx.redirectTo
关闭当前页面并跳转到应用内的其他页面

```javascript
// 关闭当前页面并跳转到目标页面
wx.redirectTo({
  url: '/pages/target/target', // 目标页面的路径
  success: function(res) {
    console.log('重定向成功');
  },
  fail: function(err) {
    console.error('重定向失败:', err);
  }
});
```

#### wx.switchTab
跳转到应用内的 TabBar 页面。注意，目标页面必须在 app.json 的 tabBar 字段中定义

```javascript
// 切换到 TabBar 中的某个页面
wx.switchTab({
  url: '/pages/index/index', // TabBar 中某个页面的路径
  success: function(res) {
    console.log('切换 TabBar 页面成功');
  },
  fail: function(err) {
    console.error('切换 TabBar 页面失败:', err);
  }
});
```

#### wx.reLaunch
关闭所有页面，打开到应用内的某个页面

```javascript
// 关闭所有页面，打开到目标页面
wx.reLaunch({
  url: '/pages/index/index', // 目标页面的路径
  success: function(res) {
    console.log('重启页面栈跳转成功');
  },
  fail: function(err) {
    console.error('重启页面栈跳转失败:', err);
  }
});
```

### 页面返回
wx.navigateBack：关闭当前页面，返回上一页面或多级页面，不支持跨 tabBar 页面的跳转，效果与用户手动点击页面左上角的返回按钮相同

参数delta：表示要返回的页面数，1 表示返回上一级页面。如果 delta 大于现有页面数，则返回到首页。例如，如果当前页面栈有 A -> B -> C，如果 delta 是 1，则返回到 B 页面；如果 delta 是 2，则返回到 A 页面

```javascript
// 返回上一页
wx.navigateBack({
  delta: 1, // 返回的页面数，如果 delta 大于现有页面数，则返回到首页
  success: function(res) {
    console.log('返回上一页成功');
  },
  fail: function(err) {
    console.error('返回上一页失败:', err);
  }
});
```

### 数据传递
#### 2.7.3 之前
步骤：

1. 构造目标页面的 URL：构建目标页面的 URL，并在其后面添加需要传递的参数作为查询参数。查询参数通常以 ? 开始，多个参数之间使用 & 分隔
2. 在目标页面中获取参数：在目标页面的生命周期函数 onLoad(options) 中可以获取传递过来的参数。options 参数是一个对象，包含所有传递过来的参数信息。

假设有两个页面：index 页面和 detail 页面。在 index 页面点击一个按钮跳转到 detail 页面，并传递参数

```javascript
//index.wxml
<button bindtap="gotoDetail">跳转到详情页并传参</button>

// index.js
Page({
  gotoDetail: function() {
    // 构造跳转的 URL，并传递参数
    wx.navigateTo({
      url: '/pages/detail/detail?id=123&name=example'
    })
  }
})
```

```javascript
Page({
  onLoad: function(options) {
    // 获取传递过来的参数
    const id = options.id;
    const name = options.name;
    console.log('id:', id); // 输出：123
    console.log('name:', name); // 输出：example
  }
})

```

#### 2.7.3 之后
在微信小程序基础库 2.7.3 及更高版本中，引入了 events 参数，这是一个用于传递数据的新机制。它可以在 navigateTo、redirectTo、navigateBack 方法中使用，用于在页面间传递数据，相比传统的 URL 查询参数更为灵活和方便

步骤

1. 在跳转源页面调用 navigateTo 方法

```javascript
wx.navigateTo({
  url: '/pages/detail/detail',
  events: {
    // 传递的数据
    data1: 'value1',
    data2: 'value2'
  }
});
```

2. 在目标页面接收并处理数据

```javascript
Page({
  onLoad: function(options) {
    const data1 = options.data1;
    const data2 = options.data2;
    console.log('data1:', data1);
    console.log('data2:', data2);
    //返回数据
    const eventChannel = this.getOpenerEventChannel();
    eventChannel.emit('someEvent', { data: '传递的数据' });
  }
});
```

3. 在源页面接收目标页面传回的数据：在 success 回调中，使用 res.eventChannel.on 方法监听目标页面通过 emit 发送的事件，并获取传递回来的数据

```javascript
wx.navigateTo({
  url: '/pages/detail/detail',
  events: {
    data1: 'value1',
    data2: 'value2'
  },
  success: function(res) {
    res.eventChannel.on('someEvent', function(data) {
      console.log(data); // 输出从目标页面传递回来的数据
    });
  }
});
```

### navigator组件
主要就是用于界面的跳转的，也可以跳转到其他小程序中

1. 页面内跳转`<navigator url="/pages/target/target">跳转到目标页面</navigator>`
2. 打开网页链接`<navigator url="https://www.example.com">打开外部链接</navigator>`
3. 跳转到其他小程序

```javascript
<navigator 
  open-type="navigate" 
  target="miniProgram"
  app-id="wx1234567890abcdef"	//app-id 是目标小程序的 AppID
  path="/pages/index"					//path 是目标小程序的页面路径
>
  打开其他小程序
</navigator>
```

## 小程序登录流程
1. 小程序初始化： 用户打开小程序后，小程序会初始化，检查用户是否已经登录过或者获取用户的登录状态
2. 获取用户信息： 如果用户之前没有登录过或者需要更新登录状态，小程序会调用 wx.getUserInfo 接口获取用户的基本信息，包括用户昵称、头像等
3. 登录凭证获取： 小程序调用 wx.login 接口获取临时登录凭证 code，这个凭证有效期较短，一般为 5 分钟，用于后续向微信服务器换取用户唯一标识符 openid 和会话密钥 session_key
4. 发送登录凭证到开发者服务器： 小程序将获取到的 code 发送到开发者自己的服务器
5. 开发者服务器向微信服务器换取用户唯一标识符和会话密钥： 开发者服务器使用 code 向微信服务器发送请求，换取用户的 openid 和 session_key
6. 生成会话： 开发者服务器通过获取到的 openid 和 session_key 生成一个会话，用于标识用户的登录状态，并将会话标识返回给小程序端
7. 会话存储与使用： 小程序端将会话保存在本地，一般通过 wx.setStorageSync 方法将会话信息存储到本地缓存中，后续在小程序的请求中携带该会话信息，以便开发者服务器可以识别用户的身份和处理用户的请求
8. 登录态维护： 开发者服务器需要定期检查用户的登录态是否过期，如果过期可以通过 wx.checkSession 接口检查用户登录态是否有效，或者重新走登录流程更新用户的登录状态
9. 权限管理（可选）： 根据需要，开发者可以在登录流程中加入权限管理，例如获取用户手机号码、用户位置等敏感信息的授权流程，这些需要用户确认授权才能获取

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719904833849-ff2f34f6-f2d8-4bfe-b76d-f4ace4f4a5fc.png)

前端需要做的是将 code 发送给后端，后端返回 token，将 token 存储到 storage 中，然后下次发请求时就可以携带上 storage 中的 token

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1719904992931-b85678f2-dcaa-45dd-b355-7cd8c877d4e7.png)



# 项目-FFMusic
## 项目记录
### 前置
首先创建一个项目，这个项目并没有使用云服务，然后将项目目录中不需要的部分删除掉。

因为这个项目的tab bar有音乐和视频两个选项。所以我们要先创建两个 Pages，并且在APP.json 中标明pages的路，定义这两个 tabbar 的 text、icon、 SelectedIcon，这样两个页面就搭建好了。

### video 页面
因为 music界面的功能较复杂，所以我们先来完成main-video界面的编写。首先需要搭建 main-video页面的WXML，这里使用了内置的组件 video-item。我们需要在根目录下的components里创建一个component名字叫video-item，并且在 main-video页面的JSON中的 UsingComponent 中引用 video-item 组件。

#### 网络请求
将WTML页面和WXSS页面编写好之后，我们需要发送网络请求获取到videoList这个数组，之后将这个数组的数据展示在WXML中。这里的网络请求我们在main-video.js 中发送，网络请求是在onLoad时被调用的。我们需要编写网络请求的代码，我们将这些代码编写在一个文件中封装成一个类，并在文件的结尾处调用这个类，生成一个新的实例。这些代码我们写在根目录下的services 里，因为需要输入一些网址，所以我们将网址定义在config.js 中，而网络请求的代码则编写在index.js 中。

为了使代码结构看起来更整洁，修改代码时更方便。我们将video页面的网络请求写在services目录下的video.js 中。当我们需要修改代码时直接在video.js 中修改即可。在video.js 中我们定义了一个方法叫getTopMv，然后我们在main-video.js 中引用这个方法，同时为了异步发送网络请求，我们将这个方法封装在一个函数异步函数中叫fetchTopMV。然后我们在onLoad时调用这个fetchTopMV即可。这就是视频页面展示推荐视频的过程

#### 上拉加载更多
这部分代码写在main-video.js 中，调用 onReachButtom 函数，当上拉时判断 data 中的 hasmore 是否为 true，如果是就调用 fetchTopMV，同时需要修改fetchTopMV 函数，他在被调用时，需要将新数据添加到上一次显示的 videoList 里，然后将添加过后的 newVideoList 赋值给 videoList，这样页面显示的就是上拉之后加载的数据，需要注意的是fetchTopMV 函数体中也需要修改 offset 的值和 hsamore 的值，用来申请之后的数据和判断是否还能继续上拉

#### 下拉刷新
首先需要在main-video.json 中设置 enablePullDownfresh 为 true，才能允许页面下拉刷新，当下拉刷新时，调用onPullDownRefresh 函数，清空之前的 videoList 和 offset，并将 hasmore 设置为 true，同时重新发送网络请求，发送成功后停止下拉刷新的加载，即 wx.stopPullDownRefresh()

#### 点击 item 进入详情页
当点击 main-video 中的元素时，跳转到 detail-video 页面，跳转的逻辑写在 video-item 中，给 Item 绑定了一个事件 onItemTap，先拿到 properties 中的的 ItemData，然后跳转页面并将 itemData 中的 id 传递过去，使用的是模板字符串

```javascript
wx.navigateTo({
  url: `/pages/detail-video/detail-video?id=${item.id}`,
})
```

跳转到指定 id 的 detai-video 页面，onLoad 发送网络请求，将和 main-video 页面一样，封装了一个 fetchXXX 函数，onLoad 中调用 fetchXXX 函数，而 fetchXXX 中调用 services/video.js 中的 getXXX 函数，以此来发送请求，并将请求到的数据存储在页面 Data 中，发送请求时使用的是 Item-video 传递过来的 id，这个 id 在 onLoad 的 options 中

```javascript
onLoad(options){
    const id = options.id
    this.setData({id})
    this.fetchMVUrl()
}
```

值得注意的是，全部使用的都是异步函数

```javascript
async fetchMVUrl(){
  const res = await getMVUrl(this.data.id)
  this.setData({MVUrl:res.data.url})
}
```

这个页面不算完整，因为有两个接口已经不再返回数据了

### music 页面
#### 搜索框
搜索框用的 vant-search，但是点击并不会直接搜，而是跳转到一个页面 detail-search，再那个页面进行搜索

#### 轮播图样式
在小程序中设置圆角需要同时使用 border-radius 和 overflow: hidden，如下图给轮播图设置样式

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721637301488-2437fad6-4db7-440a-9134-3483a1e05933.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721637775521-1ab59c56-2373-44ee-a8b5-dcee5709eb9a.png)

mode=“widthFix”和 width:100% 导致 image 没法撑满整个 banner，也就导致了图片显示的是完整的，但是只有上面是圆角，而下面还是直角

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721637619093-c825178a-7c79-42ea-a978-ab8ef3367f35.png)

如果将mode设置为 aspectFill， 则可以撑满整个 banner 但是图片显示不完整，只有完整图片的一部分。

原因就是轮播图组件是有默认高度的，为 150px ，我们不能给图片设置高度为150px，所以就需要将 banner 的高度设置为图片的高度；但是高度又不能写死为某一个 px，因为在不同的设备上显示的是不一样的，并且 rpx 也不是最优解。

最终的解决办法就是定义一个变量叫bannerHeight，然后通过mustache语法绑定到 swiper 的style上，bannerHeight 设置为 image 组件的高度(当 width:100%时图片的高度)，而图片的高度可以通过监听函数获取(bindload 表示当图片加载完毕时)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721640157155-e7472e4f-e08d-432f-8cd9-c1c754a7349d.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721640179934-21e34fe0-07fa-4186-a55c-6321ba1be572.png)

在视频课里老师封装了一个 utils/querySelector.js 工具用来保存这个复杂的代码段，这里就不多操作了

#### 歌曲推荐
##### ”歌曲推荐“
![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721706783009-59516be1-a6e5-4f63-b47c-0d2eb0fe9890.png)，这里封装了一个组件 area-header，在组件的 properties 中有一个变量 hasmore，为 true 时显示更多>，还有一个变量 title，在使用组件时只需要传参数给组件即可

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721706948449-264f2795-8afd-42a6-86c5-44f330902fcd.png)

并且在歌曲推荐组件中还绑定了点击事件，当点击时触发 onMoreTop 函数，将其传递给组件使用者，组件使用者通过右图监听，点击时跳转到 detail-song 页面

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721707045863-77f3a284-3878-43b0-b93e-841676a00c8c.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721707102657-3477f4e1-ea33-489f-9279-79a6f0146450.png)

##### 歌曲
接下来就要发送网络请求，获取到推荐的歌曲保存到 recommendSongs 对象中，并在页面中显示。这里有一个小问题：请求的数据别的页面也想使用，这就需要数据共享(具体见问题总结-4)

创建 store 文件夹，里面用来存储需要共享的数据，在里面创建了recommendStore.js，创建了一个 HYEventStore 的实例对象 recommendStore，里面包含 state(声明数据类型)和 actions(声明当数据变化时执行的函数)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721884180952-0025db5a-164f-4d31-8117-b81320c4b622.png)

在这个地方，actions 中函数通过调用 getPlaylistDetail ()发送网络请求，并且把获取到的数据存储在 recommendSongs 中，接着就需要在页面中使用这个 recommendStore![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721884967328-d12cc4fe-fb98-4456-8417-a25e3378afcd.png)

首先导入，然后定义一个数组 recommendSongs ，当加载成功时使用 onState 挂载 recommendStore，并且将 store 里的 recommendSongs 的前六个赋值给当且页面的数组 recommendSongs 中，并且使用 dispatch 调用 store 的 fetch 方法用以发送网络请求获取数据

获取到了 6 个推荐歌曲，接下来就要显示在页面中，这里封装了一个组件 song-item-v1，main-music 页面使用这个组件，并且把这6个歌曲传递给组件![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721885518370-5af20df7-ad91-486c-b58c-8f544b7c257f.png)



##### 更多
当点击更多时，调用 onRecommendMoreclick()，跳转到detail-song 页面，在这个页面中，也需要用到 recommendStore 的数据，具体是在 onLoad 和 onUnload 中，然后让数据展示页页面中即可![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721885739407-47238206-3f43-44c1-91d5-03897f0b4fc0.png)

#### 歌单
##### 歌单展示
歌单分为热门歌单和推荐歌单两部分，但是整体结构都是一样的，所以这里封装了一个组件 menu-area

，在 main-music 页面发送请求，将热门歌单和推荐歌单的数据拿到，然后传递给 menu-area 组件

`<menu-area title="热门歌单" menuList="{{hotMenuList}}"/>`

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721961253767-9c435681-86dd-4910-a0da-393cdf2747b1.png)

在 menu-area 组件中，使用了 area-header 组件作为头部，然后 scroll-view 作为主体，让歌单可以左右滑动展示，具体歌单的格式使用了 menu-item 组件，同时 menuList 数据传递给该子组件

这个部分有一个很大的问题，就是左右边距

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721961558261-ebd9c7d8-de53-4d1c-b620-5ee1d8709bc2.png)

不可以设置 display-flex 和 flex-shrink，所以将 item 设置成 inline-block，然后调整样式，如果距离写死，那不同的机型，边距就不同了，所以需要获取机型的屏幕宽度和高度，这个操作放在 app.js 中的 globalData 中，然后在使用时，`const app = getApp()`即可

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721961682308-b38a58f5-ee37-4209-a90e-0987edba08ef.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721961739793-1e88197a-c42e-457b-a0f4-cbf4d6fae7ba.png)

##### 更多
当点击更多，会跳转到 detail-menu 页面，该页面展示更多歌单，当 load 时，发送请求获取对应的数据，然后展示数据即可

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721962071594-ee3dfe40-e02f-45f2-bfc7-0b72c00681f8.png)

#### 巅峰榜
##### 巅峰榜主体
这里发送网络请求有点麻烦，因为这个巅峰榜由三个榜单组成，而且这些数据也需要共享，所以定义了一个 rankingStore.js，用来保存共享的三组数据

首先定义了一个rankingsIds，用来保存三个榜单的名字和对应的 id

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721998885528-0db7dee6-761d-4c30-b6bb-e805ecb4a46c.png)

然后定义rankingStore，包括 store 和 actions，actions 中是fetchRankingDataAction 函数，里面是一个循环，将三个请求分别发送

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721999060961-f0c067ed-0c4e-4544-8ebc-bf7f0e457a15.png)

然后就可以在 main-music 页面使用这些数据，首先需要获取![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721999130672-e650b5cd-efea-4d1e-a644-d544aa68cc36.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721999156441-f6e4d6bc-166e-4202-83e7-56e460ab5202.png)

这里需要注意 handle 函数在 onState 时不可以加()，因为加了会直接调用，而不是等待对应的数据变化后才执行

然后就是数据的展示，使用了子组件ranking-item，因为有三个绑定，所以循环执行了三次

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721999283600-a5cf6beb-6e7d-4cf6-9c35-0663f8ab21b4.png)

在ranking-item 页面，拿到传递过来的数据，就可以直接展示

##### 详细页面
当点击具体的榜单时，就会跳转到 detail-song 页面，这个页面是巅峰榜部分和歌单部分公用的组件，所以会先判断 type(巅峰榜/歌单)，然后判断 key(具体是哪个榜单/歌单)，即 `url：/pages/detail-song/detail-song?type=ranking&key=${key}`，这个页面的请求逻辑很复杂

首先定义了 type、key、id(请求需要的 id)、songInfo(歌曲信息)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721999725479-dc74ac38-d5ca-4c27-99d4-77ce64354c46.png)

onLoad 时，判断 type 和 key，然后对应的发送请求或者去 store 中拿数据

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721999859398-8d2ca2a4-98e0-40b0-8d57-c6513df64047.png)

之后就是数据的展示，这里封装了一个组件 song-item-v2，并且传递数据，song-item-v2 页面没什么好说的，就是基础页面的编写

然后回到 main-music 页面，当点击任一歌单时，也需要看详细的页面，而且比巅峰榜多了一个头部，所以这里封装了一个叫 menu-header 的组件，接受该歌单中歌曲作为数据

为了让这个头部只在歌单页面显示，需要把这个头部加上一个条件判断

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722000186213-15a9d081-a446-4f5b-b20e-f1543aab785f.png)

### music-player 歌曲播放页面
不管是 v1 还是 v2，点击后都跳转到对应的歌曲播放页面 music-player，跳转的时候携带参数 id， music-player 通过判断 id 来识别是哪首歌，该页面有一个背景图，使用了毛玻璃效果，css 是backdrop-filter: blur(20px)

#### 自定义导航栏
从上到下看，首先是导航栏，这里的导航栏没有使用 wx 自带的，而是自定义导航栏，需要在 json 设置`"navigationStyle": "custom"`,之后就是编写样式，因为导航栏可以复用，所以这里封装成一个组件 nav-bar

组件包括左中右三部分，左边是一个返回按钮<，中间是歌曲 | 歌词，右边没有具体内容

左边有一个名为 left 的插槽，左边插槽默认是返回按钮，也可以在页面中自行决定；中间的是 center 插槽，由使用者规定内容，默认显示{{title}}，title 默认是"默认标题"

#### 主体部分
接着就是播放页面的主体部分，包括播放器页面和歌词页面，要实现播放器右滑转换为歌词页面的效果，这里可以使用轮播图 swiper

##### 导航栏
首先需要将导航栏设置好，定义了一个变量 currentPage 用来识别当前是哪个 item，并根据不同的 item 改编导航栏的白色字体

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722233301417-f18998f2-c8d9-47ce-b1c1-2445de9b06ff.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722233309890-1020dbbc-6643-43db-bfc7-2cab9691a870.png)

接着获取整个页面可操作高度，目的是规定用户手指滑动轮播的区域

![music-player.wxml](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722233429050-b0ee163d-004c-4767-b8df-661a2e40faf2.png)![music-player.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722233455493-d50169a0-dc4e-4845-9fd9-6ca5946322dd.png)

![app.js](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722233523011-edd70c8a-693f-4425-9a76-86094fb6ce29.png)

点击左侧返回按钮，调用函数onNavBackTap 返回上一页

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722596419369-c6455e81-d919-4b8d-babf-444901758348.png)

##### 进度条
接着在页面中有一个居中显示的封面图片，然后是歌名和歌手名，然后是居中显示的一句歌词和居中显示的进度条，下面是歌曲当前播放的时间和歌曲总时间，再往下是歌曲循环、上一首下一首、暂停键、播放目录。封面歌名和歌手名比较简单，主要是CSS的问题，这里不多讨论。

跳过歌词展示先来实现进度条，使用了 slider 组件，将他的值绑定到 silderValue 中，绑定了两个事件：点击跳转和拖动跳转，分别是 onSliderChange 和onSliderChanging

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722592443336-c60accb9-8eb0-4adf-abf9-76000f5b357e.png)

先说进度条下面的当前播放时间 currentTime 和歌曲总时长durationTime，通过 getSongDetail(id)获取到当前播放歌曲的信息，分别将durationTime 和歌曲的基本信息保存在 data 中，currentTime 默认是 0

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722593306199-c41fa44e-d74b-4df3-8bf9-cb6235c58b99.png)

onSliderChange，当触发该事件时，首先要将歌曲的播放暂停，这里就是将数据isWaiting设置为true。然后设置了一个setTimeout1.5秒之后将is Waiting设置为false。这样当我们多次点击时，中间会有一定的时间间隔用来反应。

然后获取到点击滑块位置对应的value，这个value可以从event事件中拿到。拿到之后，要计算点击位置的时间，需要用value除以 100 得到百分比，用这个百分比 × 这个歌的总时间durationTime。

然后将播放器的播放时间设置为得到的数值，并且将isSliderChanging改为false，并且把Value赋值给 sliderValue用来匹配歌词。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722593348725-6a64ee54-b88c-4ccd-8e69-55cef54c37d6.png)

然后是onSliderChanging，他的延迟比较大，所以这里用了节流函数throttle，和上面的onSliderChange 步骤差不多，主要是获取value值，计算出对应的时间，然后设置对应的时间。需要注意的是我们需要。将isSliderChanging设置为true。isSliderChanging 和 isWaiting 两个数值的使用将在接下来进行。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722593365113-73d33883-33db-47ec-a6d8-1db689c5d83b.png)

##### 当前进度歌词
getSongLyric(id)获取到对应歌曲的歌词数据，保存成 lrcString，但是歌词是需要进行格式化的，所以这里先封装了parseLyric函数用来格式化歌词![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722593624516-bb4b0806-a6cd-4583-9bd7-6f8ce74aeaee.png)

将格式化后的数据保存在lyricInfos 中，要想拿到对应的歌词，首先需要将歌词和当前播放时间进行匹配。

首先要播放这个歌曲，设置`audioContext.src = `https://music.163.com/song/media/outer/url?id=${id}.mp3``，然后在onTimeUpdate函数被触发时，判断 isSlidingChanging和isWaiting是否为true，如果都为 false 的话，调用节流函数throttleUpdateProgress，节流函数回调updateProgress 函数，记录当前的时间(第一次播放/调整过进度条后播放)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722594382919-0a3ce8ed-52a8-47d4-93bd-f345f2de02f7.png)

然后来判断当前播放的是哪一句歌词，如果当前播放的时间currentTime小于某一句歌词，对应的时间。那么这一句歌词的上一句就是currentTime对应的歌词。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722593926897-942cb637-3fec-465b-9b5c-6a5a29680872.png)

或得到对应歌词的索引之后，可以获得到对应的句子currentLyricText，然后将这个句子和当前歌词的索引保存下来，展示在页面中

##### 暂停
设置 isPlaying 用来确认当前是否在播放，动态调整按钮的图片![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722594525012-4f0b574b-1c9e-4895-ae0d-655ac130beb8.png)点击按钮，触发onPlayOrPauseTap 函数，先确定是否被暂停`(audioContext.paused)`，然后进行暂停/播放

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722594572871-38662eec-efc7-4d59-804d-a383017200fa.png)

##### 歌曲播放模式
三种模式，用playModeIndex 确认是哪一种 (//0 顺序播放， 1 单曲循环， 2 随机播放)，playModeName 保存当前模式的名字，默认是'order'(顺序播放)，并且定义了数组保存这三个模式的名字![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722594698115-8393d46d-3e1e-4e48-8c25-69f9cbb2a544.png)

在 wxml 页面动态绑定图片![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722594725498-4da7dd73-b91b-46e7-a345-c897df561e20.png)

当点击时，触发onModeBtnTap 函数，先获取到当前模式对应的次序modeIndex，然后将modeIndex自加 1。如果这个index已经达到了 3，那么就将它设置为零。

然后判断是否为单曲循环，如果是单曲循环，就将播放器的 loop 设置为true，如果不是设置为false，这个的用处后面再说，接着保存数据。![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722594975709-567db9d9-6c31-4704-8e10-6e1bfc83f118.png)

##### 上一首下一首
因为逻辑比较相似，所以将这些逻辑都封装在函数changeNewSong中。当触发上一首或下一首时都调用这个函数。在函数中需要传入一个参数isNext，用来确认是否点击的是下一首，当点击的是上一首时传入false

首先我们需要拿到当前播放歌曲所在的歌曲列表，我们将这个封装在一个store中叫playerStore

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722595671346-19878509-431d-4fea-8d07-761249824001.png)

当 onLoad 时，获取 store 中的共享数据`playerStore.onStates(["playSongList", "playSongIndex"], this.getPlaySongInfosHandler)`，并保存在当前页面

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722595865942-607db4da-0d53-4e90-943f-802b56c65896.png)

然后获取到当前歌曲列表的长度 length 和正在播放歌曲的次序 index，然后需要根据当前歌曲的播放模式来判断下一步操作。这里使用switch来判断。

单曲循环和循环播放是一样的操作，首先要拿到下一首播放歌曲的index： `index = isNext ? index + 1: index - 1`，如果当前index已经和length相等，也就是最后一首歌时，将index设置为 0，也就是第一首歌。当列表的一首歌，想要拿到它的上一首也就是-1 对应的歌曲，需要将index设置为 length-1，也就是最后一首歌。

如果是随机播放的话，我们使用随机函数来获取到下一首播放的歌的index：`index = Math.floor(Math.random() * length)`

然后通过 Index 获取到下一首歌 newSong![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722595507764-703f02f3-b2f3-42da-94c0-74caf4928941.png)

然后将当前播放歌曲的信息、进度条、当前播放时间、总时间全部重置，防止在切换歌时留下残影![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722595568800-5739a4cf-3bee-44ca-bc43-74da83e33d65.png)

然后就可以播放新的歌曲

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722595812317-08b2c9a4-703f-40fd-8d06-e55a10e064c5.png)

然后将这个新歌曲的索引保存在playerStore中

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722595800658-05ed1aa1-8004-43dd-92f1-6041690f915d.png)

当歌曲播放完时，如果是单曲循环的话就不需要切换下一首歌，如果不是，就要切换下一首歌

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722596229058-cf433386-f815-4218-b80e-7b8562deef39.png)

##### 播放歌曲
在 onLoad 时，获取到当前歌曲的 id，然后调用 setupPlaySong(id)函数播放歌曲

在这个函数中主要执行 4 件事情

1. 保存当前歌曲的 id
2. 根据 id 获取歌曲的详情和歌词的信息
3. 播放当前歌曲
4. 监听播放的进度，并实时的调整歌词的显示

这个函数是在上面的功能实现之后才进行封装的

#### 代码重构&逻辑抽取
将有关音乐播放的代码统一抽取到 playerStore.js 中，这样当我们在别的页面也需要播放音乐时就可以调用 store中的方法，而不是重新再写一次

需要注意的就是传入参数的问题，store中的action可以传入 ctx 参数用来表示当前调用 store的实例

### music 页面播放栏
这个页面比较简单，主要包括了一个图标、歌名、播放暂停键、歌单列表图标。歌单列表这次项目没有做，所以这里就不再多说了。

这里给图标添加了一个动画就是旋转，根据isPlaying来判断是否进行旋转，如果暂停的话就不旋转

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722653769594-9c486af0-fced-4b8a-9923-c3ac5731c7fa.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722653786458-00f286ae-5d49-4ec3-ac97-8258b61b0964.png)

当点击播放暂停按钮时触发onPlayOrPauseBtnTap函数，函数调用playerStore中的action来实现播放与暂停的功能

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722653885058-bcbd4063-85a6-4f26-a8ef-180d1d4dd553.png)

为了拿到当前歌曲的信息 currentSong 和isPlaying，我们需要在 onLoad时挂载 playerStore

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722653952819-951d9c9d-0d32-41d2-8371-a29396f0d03f.png)

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722653964130-bd6e7672-24d6-4231-b869-8d364e342535.png)

### 后续工作
#### 分包
[分包加载 | 微信开放文档](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html)

某些情况下，开发者需要将小程序划分成不同的子包，在构建时打包成不同的分包，用户在使用时按需进行加载。

在构建小程序分包项目时，构建会输出一个或多个分包。每个使用分包小程序必定含有一个主包。所谓的主包，即放置默认启动页面/TabBar 页面，以及一些所有分包都需用到公共资源/JS 脚本；而分包则是根据开发者的配置进行划分。

在小程序启动时，默认会下载主包并启动主包内页面，当用户进入分包内某个页面时，客户端会把对应分包下载下来，下载完成后再进行展示。

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722654723132-3a6d3ad1-611c-4a57-8daf-4b558a2b0dfa.png)![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722654735653-38bffb54-abd5-4f42-9d9b-cb94312c9250.png)

需要注意的是，当进行完分包之后，里面的路径也会发生变化，需要将所有跳转页面的路径全部更改。

主包下面的分包是需要等主包加载完之后才会加载的，不能跳过主包独立加载，如果想要独立加载需要设置一个属性

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1722655070258-51453b8f-2bf0-43a7-bf5c-bcfd306fed6b.png)

为了让分包的加载变得更快，我们可以为分包设置预下载：preloadRule

```json
{
  "pages": ["pages/index"],
  "subpackages": [
    {
      "root": "important",
      "pages": ["index"],
    },
    {
      "root": "sub1",
      "pages": ["index"],
    },
    {
      "name": "hello",
      "root": "path/to",
      "pages": ["index"]
    },
    {
      "root": "sub3",
      "pages": ["index"]
    },
    {
      "root": "indep",
      "pages": ["index"],
      "independent": true
    }
  ],
  "preloadRule": {
    "pages/index": {
      "network": "all",
      "packages": ["important"]
    },
    "sub1/index": {
      "packages": ["hello", "sub3"]
    },
    "sub3/index": {
      "packages": ["path/to"]
    },
    "indep/index": {
      "packages": ["__APP__"]
    }
  }
}
```

也可以跨分包调用代码，具体的实现看开发者文档的分包异步化。

#### 手动优化分包-vant
在整个项目代码中其实vant库所占的内存是非常大的，所以我们需要将vant库中没有使用的组件删除，只保存使用到的vant组件，手动删除无用的组件之后再进行打包上传

要注意，有些组件的文件夹中引用了其他的组件，被引用的组件的文件夹也不能删除，在JS文件中可能会有引用别的组件，在Json的usingComponent中也可能会引用，这两个地方都是需要注意的。

#### 上传和发布
打包分包之后点击右上角的上传，然后在成员管理中添加测试的用户，让该用户对相应的页面进行测试。

## 问题总结
### 1. 小程序背景图
wxss 中使用背景图时，不可以使用本地图片，使用 base64 图片格式

### 2. 网络请求
在自己的 xxx.js 页面中封装一个 fetchXXX 函数，onLoad 中调用 fetchXXX 函数，而 fetchXXX 中调用 services/xxx.js 中的 getXXX 函数，以此来发送请求，并将请求到的数据存储在Data 中

### 3. 使用第三方库
这里使用的是 Vant Weapp 第三方库

1. 安装：需要先在小程序项目中构建npm环境，就是在命令行输入`npm init`，然后会在目录中生成 package-lock.json 和 package.json 两个文件，配置完之后输入 `npm i @vant/weapp`
2. 构建 npm：选择工具-构建 npm，然后就会将node_modules中需要的的文件放到miniprogram_npm中
3. 使用：在使用第三方库的 json 文件中注册即可，比如使用 button

`"usingComponents": { "van-button":"@vant/weapp/button/index"}`

### 4. 数据共享
小程序有一个叫globalData 可以存储公共的数据，但是不推荐那样做。

这里使用 hy-event-store

1. 安装：npm install hy-event-store
2. 构建：工具-构建 npm
3. 注册：创建 store 文件夹，在里面创建需要共享的数据文件，比如 recommendStore.js![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721884180952-0025db5a-164f-4d31-8117-b81320c4b622.png)
4. 在组件中使用，挂载使用 onState，取消挂载使用 offState![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721884351149-4551cf09-5fd1-4abd-a6d2-3de261ff34ac.png)



### 5. 获取设备信息
写在 app.js 中，以便实现共享

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721961682308-b38a58f5-ee37-4209-a90e-0987edba08ef.png)

在使用时，首先`const app = getApp()`，接着如下图

![](https://cdn.nlark.com/yuque/0/2024/png/29327577/1721961739793-1e88197a-c42e-457b-a0f4-cbf4d6fae7ba.png)



