
# 第一周专栏（ HTML & CSS ）
 

1. meta 标签  --  透彻理解 name 和 http-equiv 相关作用，
    1. name="view-port"      
    2. http-equiv="content-security-policy"
2. HTML5 新特性
3. css 选择器与权重描述
4. css 几种布局方式
5. BFC 是什么，哪些情况会构成 BFC
6. css position定位 
7. 盒子模型
8. css3 的新特性有哪些
9. css 性能优化
10. flex       flex: 后面可以跟几个值，以及他们的定义
11. 双飞翼 和 圣杯布局  至少写一种，移动端的 顶栏 和 底栏 布局
12. 接上面的flex布局，了解使用网格布局
13. 移动端适配方案   ----   这里需要看文章了解
14. 移动端解决 1px 方案
15. 了解几种 css 预编译方案 

部分答案可参考文章：[https://github.com/CavsZhouyou/Front-End-Interview-Notebook](https://github.com/CavsZhouyou/Front-End-Interview-Notebook)

## 1. mata标签

meta是html语言head区的一个辅助性标签。meta标签的作用有：搜索引擎优化（SEO），定义页面使用语言，自动刷新并指向新的页面，实现网页转换时的动态效果，控制页面缓冲，网页定级评价，控制网页显示的窗口等

#### name的作用
name属性主要用于描述网页，对应于content（网页内容），以便被搜索引擎查找、分类（目前几乎所有搜索引擎都通过meta来个网页分类）
语法格式
```html
<meta name="参数"content="具体的参数值">
```
name属性主要有以下几种
| name        | 作用                             |
| ----------- | -------------------------------- |
| keywords    | 用来告诉搜索引擎你网页的关键字么 |
| description | 告诉搜索引擎你的网站主要内容     |
| author      | 标注页面作者                     |
| copyright   | 网站版权信息                     |


**viewport详解**

移动设备上的viewport就是设备的屏幕上能用来显示我们的网页的那一块区域，但viewport又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域要大，也可能比浏览器的可视区域要小。

1.移动端视口概念：
| 视口                      | 含义                                                                    |
| :------------------------ | ----------------------------------------------------------------------- |
| 布局视口`layout viewport` | 默认980px，手机查看会导致网页内容压缩变小，不方便查看，只能手动缩放查看 |
| 视觉视口`visual viewport` | 用户正在看的网站区域，通过缩放操作视觉视口                              |
| 理想视口`ideal viewport`  | 设备有多宽，布局的视口有多宽                                            |
2.物理像素和CSS像素

物理像素：设备屏幕的物理像素，任何设备的物理像素数量都是固定的。

CSS像素：是 CSS 和 JS 中使用的一个抽象单位。

设备像素比：物理像素/CSS像素，每个设备都有固定设备像素比

例：iphone8默认条件下一个375px的div可以占满全屏幕
           设备像素比 = 750 / 375 = 2
3.viewport实现理想布局
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0,user-scalable=no">
```
| key           | 取值                         | 含义                                       |
| :------------ | ---------------------------- | ------------------------------------------ |
| width         | 正整数或device-width 默认980 | 布局视口的宽度，大于该宽度会出现横向滚动条 |
| height        | 正整数或device-height        | 指定布局视口高度，很少使用                 |
| initial-scale | [0.0-10.0]                   | 初始缩放比例，页面第一次加载的缩放比例     |
| maximum-scale | [0.0-10.0]                   | 最大缩放比例                               |
| minimum-scale | [0.0-10.0]                   | 最小缩放比例                               |
| user-scalable | yes/no                       | 是否可以手动缩放                           |

`viewport`作用就是通过传入的`width`控制初始布局视口宽度,`device-width`就是设备`物理像素值/设备像素比`得到的`CSS像素值`,当`width==device-width`时，不缩放的情况下页面宽度就等于屏幕宽度，不会出现左右滚动条。  

例如iphone8的横向分辨率750,设备像素比为2,则可以计算`device-width=750/2=375`,对于body标签，css设置`width:100%`等价于`width:375px`,切换其他设备时会根据不同的设备像素比得到不同的`device-width`，body使用`width:100%`依然可以占满全屏。

##### http-equiv
http-equiv相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，与之对应的属性值为content，content中的内容其实就是各个参数的变量值。 
```html
<meta http-equiv="参数" content="参数变量值">
```
| key                    | 举例                                                                    | 含义                                                          |
| :--------------------- | :---------------------------------------------------------------------- | ------------------------------------------------------------- |
| Expires(期限)          | content="Fri,12Jan200118:18:18GMT"                                      | 可以用于设定网页的到期时间,旦网页过期，必须到服务器上重新传输 |
| Pragma(cache模式)      | content="no-cache"                                                      | 禁止浏览器从本地计算机的缓存中访问页面内容                    |
| Refresh                | content="2;URL=http://www.jb51.net"                                     | 自动刷新并指向新页面                                          |
| Set-Cookie(cookie设定) | content="cookievalue=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/" | 如果网页过期，那么存盘的cookie将被删除                        |
| content-Type           | "content="text/html;charset=gb2312"                                     | 设定页面使用的字符集                                          |


**Content-Security-Policy（内容安全政策,下文简称为CSP）**
主要作用：  
- 使用白名单的方式告诉客户端（浏览器）允许加载和不允许加载的资源。
- 向服务器举报这种强贴牛皮鲜广告的行为，以便做出更加针对性的措施予以绝杀。
```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self' *.baidu.com www.google-analytics.com 'unsafe-inline' 'unsafe-eval';">
```

CSP指令及其含义

| 属性         | 举例                                                                     |
| :----------- | :----------------------------------------------------------------------- |
| default-src  | 默认加载策略                                                             |
| script-src   | 对 JavaScript 的加载策略                                                 |
| style-src    | 限制script-src版的样式表                                                 |
| img-src:     | 用于定义可从中加载图像的来源                                             |
| font-src     | 用于指定可提供网页字体的来源                                             |
| media-src    | 用于限制允许传输视频和音频的来源                                         |
| object-src   | 针对`<object>`、`<embed>`或`<applet>`等标签引入的flash等插件的加载策略。 |
| plugin-types | 用于限制页面可以调用的插件种类                                           |
| child-src    | 用于列出适用于工作线程和嵌入的帧内容的网址(框架)                         |
| connect-src  | 对Ajax、WebSocket等请求的加载策略                                        |

## 2. HTML5 新特性

##### 语义化标签
摘了很形象的一句话来描述：
> 一个网页相当于一个画中的树，html结构相当于这个数的枝干，而标签则是这个枝干上的果实叶子，css则是颜料，用来装饰这幅画的整体风格，HTML标签语义化相当于将果实和叶子用不同语义的标签将它们区分开来，让别人在看这幅画的时候，一眼能够明白哪部分代码的是果实，哪部分代表的是叶子。

语义化优点：
- 易于用户阅读，样式丢失的时候能让页面呈现清晰的结构。
- 有利于SEO，搜索引擎根据标签来确定上下文和各个关键字的权重。
- 方便其他设备解析，如盲人阅读器根据语义渲染网页
- 有利于开发和维护，语义化更具可读性，代码更好维护，与CSS3关系更和谐。

常见语义化标签及含义

|  标签   |                           含义                           |
| :-----: | :------------------------------------------------------: |
| header  |                       文档头部区域                       |
|   nav   |                           导航                           |
|  main   |                         主要内容                         |
| article | 文档、页面、应用或网站的独立结果，可独立分配和复用的结构 |
|  aside  |      侧边栏或嵌入内容，与其他页面内容几乎无关的部分      |
| section |   表示文档中的一个区域（或节），比如内容中的一个专题组   |
| footer  |                       文档尾部区域                       |
| dialog  |                       会话或提示框                       |

##### 拖放API

在拖放的过程中会触发以下事件：

在拖动目标上触发事件 (源元素):
- ondragstart - 用户开始拖动元素时触发
- ondrag - 元素正在拖动时触发
- ondragend - 用户完成元素拖动后触发

释放目标时触发的事件:
- ondragenter - 当被鼠标拖动的对象进入其容器范围内时触发此事件
- ondragover - 当某被拖动的对象在另一对象容器范围内拖动时触发此事件
- ondragleave - 当被鼠标拖动的对象离开其容器范围内时触发此事件
- ondrop - 在一个拖动过程中，释放鼠标键时触发此事件

传送门：[HTML5中drag和drop使用](https://blog.csdn.net/weixin_42060896/article/details/105638537)

##### Web Storage
使用HTML5可以在本地存储用户的浏览数据。早些时候,本地存储使用的是cookies。但是Web 存储需要更加的安全与快速. 这些数据不会被保存在服务器上，但是这些数据只用于用户请求网站数据上.它也可以存储大量的数据，而不影响网站的性能。数据以 键/值 对存在, web网页的数据只允许该网页访问使用。

客户端存储数据的两个对象为：

- localStorage - 没有时间限制的数据存储
- sessionStorage - 针对一个 session 的数据存储, 当用户关闭浏览器窗口后，数据会被删除。

localStorage操作数据的API(sessionStorage只需替换名字即可)
- 保存数据：localStorage.setItem(key,value)
- 读取数据：localStorage.getItem(key)
- 删除数据：localStorage.removeItem(key)
- 清空数据：localStorage.clear()
- 根据索引获取key：localStorage.key(Index)
##### Web Worker
什么是 Web Worker？
当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。

web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

传送门：[HTML 5 Web Worker](https://blog.csdn.net/weixin_42060896/article/details/105057197)


##### WebSocket
WebSocket时HTML5提供的一种在单个TCP连接上进行全双工通讯的协议，浏览器向服务器发出建立WebSocket连接的请求，连接建立后，两端就可以通过TCP连接直接互相主动交换数据，与http不同服务端可以主动向客户传送数据，客户端可以使用send向服务端发送数据，onmessage事件接受服务器返回的数据
```javascript
/* 后台 */
let WebSocket = require('ws');
/* 创建服务 */
let wss = new WebSocket.Server({ port: 3333 });
wss.on('connection', ws => {
    ws.on('message', e => {
        console.log(e);//前端数据
        ws.send('后台数据');
    })
})

/* HTML */
<script>
    let socket = new WebSocket('ws://localhost:3333');
    socket.onopen = function () {
        socket.send('前端数据');
    }
    socket.onmessage = ({ data }) => {
        console.log(data);//后台数据
    }
</script>
```

## 3.CSS选择器及其权重描述

##### 基本选择器

| 选择器 | 描述                       |
| :----- | :------------------------- |
| *      | 匹配所有元素               |
| E      | 匹配所有E标签              |
| .app   | 匹配所有class="app"的元素  |
| #app   | 匹配id="app"的元素，仅一个 |

##### 组合选择器
| 选择器 | 描述                                        |
| :----- | :------------------------------------------ |
| A，B   | 并列选择器,匹配A和B                         |
| A B    | 后代选择器,匹配A包裹的所有B元素             |
| A > B  | 子元素选择器,匹配A包裹的儿子元素B           |
| A + B  | 相邻选择器,匹配所有紧随A元素之后的同级元素B |
| A ~ B  | 后兄弟选择器,匹配任何A标签之后的同级B标签   |

##### 属性选择器
| 选择器                | 描述                                                                    |
| :-------------------- | :---------------------------------------------------------------------- |
| E[attribute]          | 具有attribute属性的E元素，E可省略                                       |
| E[attribute = value]  | attribute属性值为value的E元素                                           |
| E[attribute ~= value] | 匹配所有attribute属性具有多个空格分隔的值、其中一个值等于“value”的E元素 |
| E[attribute ^= value] | attribute属性值以value开头的E元素                                       |
| E[attribute $= value] | attribute属性值以value结尾的E元素                                       |
| E[attribute *= value] | attribute属性值包含value字符串的E元素                                   |
| [attribute \|= value] | 用于选取带有以指定值开头的属性值的元素，该值必须是整个单词              |

`~=`和`*=`区别，`~=`匹配空格包含的整个值，`*=`匹配字符串是否包含字符串，例如以下输出结果为红色背景，字体颜色未生效，
```html
<div class="abcd" >test</div>
```
``` css
div[class*=abc] {
            background-color: red;
}
div[class~=abc] {
    color: green;
}
```
![](https://img-blog.csdnimg.cn/20200507185711567.png)

##### 伪类选择器
| 选择器               | 描述                       |
| :------------------- | :------------------------- |
| E:first-child        | 第一个子元素E              |
| E:nth-child(n)       | 第n个子元素E               |
| E:link               | 未被点击的链接             |
| E:hover              | 鼠标悬停在E上的效果        |
| E:active             | 鼠标点击还未释放的效果     |
| E:visited            | 点击过的链接               |
| E:focus              | 获得当前焦点的E元素        |
| E:target             | 特定id的元素点击后的效果   |
| E:lang(c)            | lang属性等于c的E元素       |
| E:enabled            | 表单中激活的E元素          |
| E:disabled           | 表单中禁用的E元素          |
| E:checked            | 表单中选中的元素           |
| E::selection         | 匹配被用户选取的选取的部分 |
| E:not(:nth-child(n)) | 除了第n个的其它E元素       |

target伪类选择器使用方式：
```css
:target{
    border: 2px solid #D4D4D4;
    background-color: green;
}
```
```html
<!-- 点击后对应id的样式会改变 -->
<p><a href="#news1">跳转至内容 1</a></p>
<p><a href="#news2">跳转至内容 2</a></p>

<p id="news1"><b>内容 1...</b></p>
<p id="news2"><b>内容 2...</b></p>
```

##### 伪元素选择器
| 选择器         | 描述              |
| :------------- | :---------------- |
| E:first-line   | E标签第一行       |
| E:first-letter | E标签内第一个字母 |
| E:before       | E前面插入内容     |
| E:after        | E后面插入内容     |

注：`::before`和`::after`在CSS3引入，作用与`:before`和`:after`相同

优先级：id > class > 标签 > 相邻选择器 > 子选择器 > 后代选择器 > * > 属性选择器 > 伪类选择器
## 5. BFC
#### 定义
> BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

#### BFC布局规则：
- 内部box再垂直方向一个一个放置
- 同一个BFC相邻两个Box垂直方向上的margin会重叠
- 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
- BFC不会与float box重叠
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
- 计算BFC的高度时，浮动元素也参与计算。

#### 如何创建BFC
- 根元素是一个BFC
- 浮动元素 (元素的 float 不是 none)
- 绝对定位(position的值不是static或者relative)
- display的值是inline-block、table-cell、flex、table-caption或者inline-flex
- overflow的值不是visible

#### BFC作用

**1. 避免margin重叠**
两个box宽高都是100px,margin都是10px
```html
<div class="box1">box1</div>
<div class="box2">box2</div>
```
![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508132805.png)    
可以看到margin发生了重叠，我们改变一下布局，用box3来包裹box2并设置box3属性`overflow:hidden`激活成为BFC

```html
<div class="box1">box1</div>
<div class="box3">
    <div class="box2"></div>
</div>
```
![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508133059.png) 

可以看到box2与box1的margin不再重叠

**2. 自适应两栏布局**
两个盒子left为向左浮动,right不浮动
```html
<div class="left">left</div>
<div class="right">right</div>
```
![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508133734.png)   

原因：每个盒子的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

right设置属性`overflow:hidden`激活成为BFC后 

![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508134010.png)

原因：BFC的区域不会与float box重叠

当right宽度去掉以后就会自适应实现两栏布局。

**3. 清除浮动**
box1不设置高度，box2浮动且高度为100px
```html
<div class="box1">box1
    <div class="box2">box2</div>
</div>
```
![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508135439.png)    

当我们给box1加`overflow:hidden`后   

![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508135720.png)

由于计算BFC的高度时，浮动元素也参与计算，box2成功被box包裹  

<font color="red">总结：因为BFC内部的元素和外部的元素绝对不会互相影响，因此， 当BFC外部存在浮动时，它不应该影响BFC内部Box的布局，BFC会通过变窄，而不与浮动有重叠。同样的，当BFC内部有浮动时，为了不影响外部元素的布局，BFC计算高度时会包括浮动的高度。避免margin重叠也是这样的一个道理。</font>


## 6. position定位
| value    | 含义                                                                                   |
| -------- | :------------------------------------------------------------------------------------- |
| static   | 默认值，没有定位，忽略 top, bottom, left, right 或者 z-index 声明                      |
| relative | 相对定位，相对于元素正常位置进行定位                                                   |
| absolute | 绝对定位，相对于static定位以外的第一个父元素进行定位                                   |
| fixed    | 相对于浏览器窗口定位                                                                   |
| sticky   | 粘性定位，所在区域在屏幕中可见时随父元素滚动，当原本的区域超出父元素时相对于父元素定位 |
sticky示例：
<div style="width: 400px; height: 500px; position: relative; overflow: auto; border: 10px solid #eee;">
    <div style="height: 300px; background: skyblue;">111111</div>
    <div style="position: sticky; top: 0px; height: 30px; background: pink;">没错，我就是sticky，请向上滚动</div>
    <div style="height: 300px; background: skyblue;">2222222</div>
    <div style="height: 300px; background: skyblue;">3333333</div>
    <div style="height: 300px; background: skyblue;">444444444</div>
    <div style="height: 300px; background: skyblue;">55555555555</div>
</div>

## 7. 盒子模型

box-sizing属性是用户界面属性里的一种，用来确定width与各种边距的关系

- content-box   
默认值，设置的width就是内容区域的宽高,盒子实际占用页面的宽度= width + padding + border + margin
- border-box    
设置的width值其实是除margin外的border + padding + content的总宽度,盒子实际占用页面宽度=width+margin
- inherit   
继承父元素box-sizing属性

## 9. CSS性能优化

##### (1)减少CSS嵌套

CSS的匹配原理是从右到左，例如div p span{}组合的选择，先查找页面所有span元素，然后往上找，把父元素不是p的删去，再往上把父元素不是div的p元素删去，可想而知嵌套层数过多性能肯定不高,一般情况下块级元素加上类，里面的内联元素不用加，css写的时候块级class套内联tag，这样不仅可以减少css文件大小，还能减少性能浪费。

##### (2)动画性能优化

一般浏览器的渲染刷新频率是 60 fps，所以在网页当中，帧率如果达到 50-60 fps 的动画将会相当流畅，让人感到舒适。
- 如果使用js的动画，应尽量使用requestAnimationFrame,避免使用SetTimeout
- 避免使用jQuery animate()-style改变眼视光，使用CSS自带的Animation会得到更好的浏览器优化
- translate取代absolute性能更好，动画也更顺滑

##### (4)CSS精灵

合成所有icon图片，用宽高加上bacgroud-position的背景图方式显现出我们要的icon图，这是一种十分实用的技巧，极大减少了http请求。

#### (5)把 styleSheets 放在HTML页面头部

浏览器在所有的 stylesheets 加载完成之后，才会开始渲染整个页面，在此之前，浏览器不会渲染页面的任何内容，页面呈现空白。如果放在HTML页面底部，页面渲染就不仅仅是等待 stylesheet 的加载，还要等待 html 内容加载完成，这样一来，用户看到页面的时间会更晚。

对于 @import 和 link 两种加载外部CSS文件的方式： @import 就相当于是把 标签放在页面的底部，所以从优化性能的角度看，应该尽量避免使用 @import 命令。


## 10. flex后面可以跟几个值，以及定义

```css
flex: flex-grow flex-shrink flex-basis|auto|initial|inherit;
```
flex-basis:子项的占用空间,未设置情况下就是子项width/height,优先级比width/height高
```html
<div class="box">
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
</div>
```
```css
.box {
    display: flex;
    width: 400px;
    height: 100px;
    border: 4px solid rgb(19, 146, 231);
}

.box1 {
    width: 50px;
    background: tomato;
}

.box2 {
    flex-basis: 50px;/* 覆盖width,box2宽度还是50px */
    width: 100px;
    background-color: mediumpurple;
}

.box3 {
    flex-basis: 100px;
    background-color: mediumseagreen;
}
```
![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508213116.png)    

flex-grow:根据比例瓜分除了width或flex-basis所占的空间的剩余空间
```css
/* 省略。。。 */
.box1 {
    width: 50px;
    background: tomato;
}
.box2 {
    flex-basis: 50px;
    flex-grow: 1;/* (400-50-50-100)*1/2=100 */
    background-color: mediumpurple;
}
.box3 {
    flex-basis: 100px;
    flex-grow: 1;/* (400-50-50-100)*1/2=100 */
    background-color: mediumseagreen;
}
```
![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508214019.png)    

剩余宽度=400-50-50-100=200px    
box1宽度=50px  
box2宽度=基础宽度(50px)+剩余宽度*1/(1+1)=150px  
box3宽度=基础宽度(100px)+剩余宽度*1/(1+1)=200px


flex-shrink:用来吸收子项超出的空间,默认为1

```css
.box1 {
    width: 100px;
    background: tomato;
}

.box2 {
    flex-basis: 100px;
    flex-shrink: 2;
    background-color: mediumpurple;
}

.box3 {
    flex-basis: 300px;
    flex-shrink: 2;
    background-color: mediumseagreen;
}
```
![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508215404.png)

超出宽度=100+100+300-400=100px;     
box1宽度=100 - 100\*1/(100\*1+100\*2+300\*2)*超出宽度=100-11.11=88.89px     
box2宽度=100 - 100\*2/(100\*1+100\*2+300\*2)*超出宽度=100-22.22=77.78px     
box3宽度=300 - 300\*2/(100\*1+100\*2+300\*2)*超出宽度=100-66.66=233.33px


## 11. 圣杯布局和双飞翼布局
圣杯布局和双飞翼布局是前端工程师需要日常掌握的重要布局方式。两者的功能相同，都是为了实现一个两侧宽度固定，中间宽度自适应的三栏布局。
圣杯布局来源于文章In Search of the Holy Grail，而双飞翼布局来源于淘宝UED。虽然两者的实现方法略有差异，不过都遵循了以下要点：
- 两侧宽度固定，中间宽度自适应
- 中间部分在DOM结构上优先，以便先行渲染
- 允许三列中的任意一列成为最高列
- 只需要使用一个额外的`<div>`标签
### 圣杯布局
结构
```html
<div class="container">
    <div class="main">中间</div>
    <div class="left">左边</div>
    <div class="right">右边</div>
</div>
```
样式
```css
.container {
    /* 初始化 */
    height: 100px;
    background-color: wheat;
    /* 防止left和right遮住main */
    padding: 0 100px;
    /* 防止转行 */
    min-width: 100px;
}

.left,
.right {
    /* 初始化 */
    background-color: mediumslateblue;
    width: 100px;
    height: 100px;
    float: left;
}

.main {
    /* 初始化 */
    width: 100%;
    height: 100px;
    background-color: mediumseagreen;
    float: left;
}

.left {
    /* 回到上一行开头 */
    margin-left: -100%;
    /* 移动到父元素padding区域 */
    position: relative;
    left: -100px;
}

.right {
    /* 回到上一行结尾 */
    margin-left: -100px;
    /* 移动到父元素padding区域 */
    position: relative;
    right: -100px;
}
```
实现过程
- 给main,left,right加浮动，left,right宽100px,main宽度100%;


![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508103853.png)
- 左右元素设置margin-left,到达上一行左右两端


![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508104327.png)
- main部分区域被left,right遮掉了, 设置container左右padding为100px,然后相对定位移动left，right位置
- 设置container最小宽度100px防止窗口缩小转行


![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508105644.png)

### 双飞翼布局
结构
```html
<div class="container">
    <div class="main">中间</div>
</div>
<div class="right">右边</div>
<div class="left">左边</div>
```
样式
```css
body {
    min-width: 300px;/* 防止换行 */
}

.container {
    /* 初始化 */
    width: 100%;
    height: 100px;
    background-color: wheat;
}

.main {
    height: 100px;
    margin: 0 100px;
    background-color: mediumseagreen;
}

.left {
    /* 初始化 */
    width: 100px;
    height: 100px;
    background-color: mediumslateblue;
    /* 转到上一行 */
    margin-left: -100%;
}

.right {
    /* 初始化 */
    width: 100px;
    height: 100px;
    background-color: mediumturquoise;
    /* 转到上一行 */
    margin-left: -100px;
}

.left,
.right,
.container {
    /* 给三个盒子加浮动从而可以通过margin换行 */
    float: left;
}
```

实现过程
- 设置main的左右margin(或者padding)使其内容在container中间，流出两边位置给left,right


![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508113946.png)
- 设置container，left，right的浮动


![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508114142.png)
- 设值left和right的margin使其回到上一行


![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508114246.png)

- 防止换行，设置三者的父元素min-width=left的宽度+right的宽度+container的最小宽度(假设100)=300px  


![](https://gitee.com/aeipyuan/picture_bed/raw/master/images/20200508114456.png)

通过对圣杯布局和双飞翼布局的介绍可以看出，圣杯布局在DOM结构上显得更加直观和自然，且在日常开发过程中，更容易形成这样的DOM结构（通常`<aside>`和`<article>`/`<section>`一起被嵌套在`<main>`中）；而双飞翼布局在实现上由于不需要使用定位，所以更加简洁，且允许的页面最小宽度通常比圣杯布局更小。

其实通过思考不难发现，两者在代码实现上都额外引入了一个`<div>`标签，其目的都是为了既能保证中间栏产生浮动（浮动后还必须显式设置宽度），又能限制自身宽度为两侧栏留出空间。


## 13. 移动端适配方案

#### 百分比方案 
>使用 百分比% 定义 宽度，高度 用px固定，根据可视区域实时尺寸进行调整，尽可能适应各种分辨率，通常使用max-width/min-width控制尺寸范围过大或者过小。

优势：原理简单，不存在兼容性问题    
劣势：
- 如果屏幕尺度跨度太大，相对设计稿过大或者过小的屏幕不能正常显示
- 设置盒模型的不同属性时，其百分比设置的参考元素不唯一，容易使布局问题变得复杂

#### Media Queries

主要是通过查询设备的宽度来执行不同的 css 代码，最终达到界面的配置。核心语法是：
```css
@media screen and (max-width: 600px) { /*当屏幕尺寸小于600px时，应用下面的CSS样式*/
  /*你的css代码*/
}
```

优势：
- media query可以做到设备像素比的判断，方法简单，成本低，特别是对移动和PC维护同一套代码的时候。目前像Bootstrap等框架使用这种方式布局
- 图片便于修改，只需修改css文件
- 调整屏幕宽度的时候不用刷新页面即可响应式展示

缺点：
- 代码量大，维护不方便~
- 为了兼顾大屏幕或高清设备，会造成其他设备资源浪费，特别是加载图片资源
- 为了兼顾移动端和PC端各自响应式的展示效果，难免会损失各自特有的交互方式


#### vh/vw方案

视口是浏览器中用于呈现网页的区域，移动端的视口通常指的是 布局视口

- vw : 1vw 等于 视口宽度 的 1%
- vh : 1vh 等于 视口高度 的 1% 
- vmin : 选取 vw 和 vh 中 最小 的那个
- vmax : 选取 vw 和 vh 中 最大 的那个

使用 css 预处理器把设计稿尺寸转换为 vw 单位，包括 文本，布局高宽，间距 等，使得这些元素能够随视口大小自适应调整。

优势：纯 css 移动端适配方案，不存在脚本依赖问题，逻辑清晰简单，视口单位依赖于视口的尺寸

不足：存在一些兼容性问题，Android4.4以下不支持

