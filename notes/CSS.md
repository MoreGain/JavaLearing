# CSS

### 概述

css(cascading style sheet) 是层叠样式表，用于改变页面的样式

css 分为两个部分：选择元素，设置样式

### css 使用方式

###### 外联样式表

```html
<head>
	<link type="text/css" rel="stylesheet" href="a.css" />
</head>
```

优点：HTML CSS 彻底分离，便于维护，并且代码可以复用

缺点：可能出现小概率的样式表没有请求成功的情况

> 注意：外联样式表中要用到背景图片时，路径是从 CSS 文件夹出发的；外联样式表不影响权重

###### 内嵌样式表

```html
<head>
    <style type="text/css"></style>
</head>
```

优点：保证 HTML 加载的时候，就已经有样式，不会出现 CSS 加载不正常的情况

缺点：HTML CSS 混在一起，不好维护，不能共用

###### 行内引入

```html
<div style=" "></div>
```

### css 特点

1. css 对换行、空格、缩进不敏感

   > 可将 css 压缩为一行来减少代码尺寸

2. 每个选择器最后一项的属性值可以不加分号，其他都要有

### css 选择器

###### 标签选择器

```css
p {
   color: red;
   font-size: 20px;
}
```

选择页面上所有的 p 标签，设置字体颜色为红色，字号为 20 像素

###### 类选择器

```css
.jump {
    text-decoration: none;
}
```

选择页面上所有 class 为 header 的标签，class 命名以英文字母开头，可以有数字、下划线、短横，区分大小写

一个标签可以携带多个类名，用空格隔开

原子类：将一些简单的属性做成一个类，让标签自行选择

###### id 选择器

```html
#news {
	border: 1px solid red;
}
```

选择页面上 id 为 news 的标签，id 在页面上必须是唯一的，命名与 class 相同

一般 calss 用于设置样式，id 用于 js

###### 后代选择器

后代选择器用空格表示

```css
div p {
    font-family: "微软雅黑";
}
```

选择 div 下所有的 p 标签，可以是儿子、孙子......

###### 交集选择器

一般语法为 `标签名.类名` 

```css
p.great {
    color: skyblue;
}
```

选择既是 p 同时又有 great 类的 p 标签

###### 并集选择器

并集选择器用逗号表示

```css
p,.header,#name {
    color: green;
}
```

###### 通配符 *

选择所有元素

```css
* {
    margin: 0;
    padding: 0;
}
```

### 继承性与层叠性

###### 继承性

css 中，有一些属性，某元素设置后，它的后代元素就都拥有这个属性了

能够继承的属性：

> color
>
> text-	如 text-docration、text-indent
>
> font-	如 font-size、font-family
>
> line-	如 line-height

不能继承的属性：background-color、所有盒模型的属性等

通过继承性，我们可以把一些初始的、基本的设置写在 body 中

###### 层叠性

层叠性就是处理冲突的能力，即当多个选择器选中同一个标签后，应该执行谁

权重

> 选择器的基本权重：id 选择器 > 类选择器 > 标签选择器
>
> 权重计算：id 数量，class 数量， 标签数量
>
> 只有选中到文字所在的标签，才能计算权重，如果是继承而来的，权重是0！

- 权重相同

  如果都选择上了，则以 style 列表中后出现的为准

- 继承

  都通过继承而来，则执行就近原则，谁距离标签更近就听谁的，若一样进，则比较权重(不是所选标签的真正权重，因为真正的权重为0)

> 要想让一个特性层叠掉共性，那这个选择器的前半部分一定要和共性相同

!important 提升权重

```css
.warning {
    color: red !important;
    font-weight: bold !important;
}
```

> 提升权重只能在原子类的情况使用，此时权重为无限大

> !important不影响继承性，一个标签是通过继承性影响的，权重是0，加上!important也是0
>
> !important不影响就近原则，远的那个，写上!important也没用，还是以近的那个为准

### css 颜色表示法

###### 单词表示

```css
p {color: red;}
```

black、white、red、green、blue、yellow、pink、orange、purple、gold、gray、yellowgreen、greenyellow

###### rgb() 表示

```css
p {color: rgb(255,0,0);}
```

> 红色： rgb(255,0,0)	绿色： rgb(0,255,0)	蓝色： rgb(0,0,255)
>
> 白色：rgb(255,255,255)	黑色：rgb(0,0,0)	灰色：rgb(100,100,100)
>
> ###### 十六进制表示

```css
p {color: #ffffff;}
```

> 红色：#ff00 或 #f00

### css 文字属性

###### color

文字颜色

###### font-size

文字大小，单位可为 px、em、rem。汉字不是顶天立地的，所以量测出的尺寸可能小于实际尺寸

###### line-height

文字行高，即文字所在行的高度，文字在行中垂直居中，单位可为 px 或使用百分比

###### font-family

文字字体，设置字体必须为用户电脑有的字体，否则为宋体。为了用户体验一致，一般只使用宋体、微软雅黑，可通过逗号设置备选字体

```css
p {font-family: "微软雅黑","宋体";}
```

将字体名设置为英语可提升 css 加载速度

```css
p {font-family: "Microsoft Yahei","SimSun";}
```

font 属性合并书写，font-size/line-height font-family

```css
p {font: 14px/28px "宋体";}
```

###### font-weight

文字加粗，值可为 bold(700px)、normal(400px)

###### font-style

文字样式， `italic` `oblique` 表示文字倾斜

###### text-decoration

文字下划线，值可为 `` `` `underline` `none` `line-through` `overline`

###### font 属性综合

```css
p {font: italic bold 12px/24px arial;}
/*通常以如下方式综合*/
p {font: 12px/24px "Mircosoft Yahei";}
```

### 盒子模型

盒子具有 width、height、border、padding、margin 几大属性

盒子真实占有宽度为：width+padding-left+padding-right+border-left+border-right

###### padding

内边距，内容到边框的距离，padding 区域有背景颜色

```css
div {
    /*四个方向padding均为20*/
    padding: 20px;
    /*上下为10px，右左为20px*/
    padding: 10px 20px;
     /*上为10px，右左为20px，下为30px*/
    padding: 10px 20px 30px;
    /*依次为上、右、下、左的padding*/
    padding: 10px 20px 30px 40px;
}
```

一些元素例如 ul 默认带有 padding、margin，为了防止影响页面，一般会使用如下方式在页面开始进行清除，但这样写存在效率问题

```css
* {
    margin: 0;
    padding: 0;
}
```

###### border

边框三要素：粗细、线型、颜色

```css
/*线型有多种值，一般只使用 solid 和 dashed，其他线型具有兼容问题*/
div { border: 1px solid black;}
```

border 是一个组合属性，由三部分构成

> border-width
>
> border-style
>
> border-color

还可细分为12个小属性

> border-top-width
>
> border-right-width
>
> border-bottom-width
>
> border-left-width
>
> ......

###### margin

当前盒子边框外侧到其他盒子边框外侧的距离

> 坍塌现象：margin 小的会陷入 margin 大的值，以大的值为准

###### 居中策略

- 盒子中文字居中

  ```css
  /*text-align值可为：left/center/right*/
  div {text-align: center;}
  ```

  让盒子中的文字居中，可使用 `text-align` 属性设置，这种方式只能居中文本流的东西，如文字、图片、表单元素，此属性要设置给盒子

- 盒子居中

  ```css
  div {margin: 0 auto;}
  ```

  这种方式可让盒子居中，属性要设置给盒子，它将在自己的父元素内部居中

- 单行文本垂直居中

  ```css
  div {line-height: 50px;}
  ```

  设置 `line-height` 可让单行文本垂直居中，行高=盒子高，只能用于单行文本，对多行文本不适用

### 标准文档流

###### 概述

###### 标准文档流性质

- 空白折叠现象
- 高矮不齐时底边对齐

###### 块级元素

可以设置高度、宽度

不能并排

不设置宽度，宽度默认继承父亲的宽度

> div、h、p、ul、li、dl、dt、dd 等都是块级元素

###### 行内级元素

不能设置高度、宽度

可以并排

> span、a、b、u、i、em、strong 等都是行内级元素

###### 行内元素和块级元素间的转换

`display: block;` 可将任何元素转换为块级元素，`display: inline` 可将任何元素设置为行内元素

实际中经常会将行内元素转换为块级元素使用，一般不进行逆向转换

### 浮动

###### 概述

浮动就是将文档脱离标准流，脱离后就没有了标准流的行块之分了

脱离标准流有三种方式：浮动、绝对定位、相对定位

```css
div {
    /*浮动*/
 	float: left;
    /*绝对定位*/
    position: absolute;
    /*固定定位*/
    position: fixed;
}
```

> span 在标准流中为行内元素，浮动后可以不转换为块直接设置宽度、高度
>
> div 在标准流中为块级元素，不设置宽度时在标准流中自动撑满父元素，浮动后自动收缩为内部文字大小

浮动用于将元素并排，可以左浮动或者右浮动，要让元素并排需要保证父元素的宽度

###### 贴边

浮动的元素会贴向父元素的边，被兄弟元素挡住则贴到兄弟的边，浮动的元素不能撑高父元素了，不能上浮下浮

###### 坍塌现象

margin 坍塌是标准流的性质，浮动后坍塌现象消失，浮动元素会让出标准文档流

###### 子围

一个盒子中具有 img 和文字，若只有 img 浮动，则文字会自动围绕图片

### 清除浮动

父元素不能被浮动的儿子撑出高，只能被标准流的元素撑出高度

可以通过 `overflow: hidden` 解决此问题，通过设置此样式可让父元素识别到脱标的儿子元素，从而被撑出高度

有高度的盒子可以管理住自己内部的浮动元素

###### 清除浮动

```css
div {clear: both;}
```

可通过 clear 清除浮动，值可为 left、right、both

> 此方式不好用：a. box 还是没有高	b. margin 失效

###### 隔墙清除浮动

```html
<div class="float_left">

</div>
<div class="clear_both"></div>
<div class="float_left">
    
</div>
```

在两个浮动的元素之间增加一个 div (墙)来清除浮动，使用这种方式盒子依然没有高度，且 margin 依然失效，但可以通过墙的高度来 margin

###### 内墙法

```html
<div class="float_left">
    <p>111</p>
    <div class="clear_both"></div>
</div>
<div class="float_left">
    <p>111</p>
    <div class="clear_both"></div>
</div>
```

在浮动的元素最后添加一个清除浮动的儿子 div(内墙)，这种方式下盒子具有高度，margin 也能正常使用，但会导致没有语义的 HTML 标签增多

###### overflow: hidden 清除浮动

给没有高度的盒子加上 `overflow: hidden;` ，盒子具有高度，且 margin 可用

> 实际工作中：给内部有浮动的盒子加上 overflow: hidden；
>
> ​			在两个大部分之间添加外墙

### 超链接美化

###### 伪类

一个超级链接，根据用户的点击情况，有自己的样式，总共具有四种状态

> a:link	没有访问的链接
>
> a:visited	已经访问的链接
>
> a:hover	鼠标悬停的时候
>
> a:active	鼠标按下的时候

它们叫做伪类，这些状态是由用户指定的，在页面编辑时我们只能定义，不知道具体属于什么类

伪类的四个顺序必须为 link、visited、hover、active，可以省略但顺序不能变，否则可能造成样式混乱

###### 伪类使用

一般的，我们都会把 a:link、a:visited 写在一起，不使用 a:active

a 要转块、设置宽高(如导航栏)，一般把盒模型的属性写在 a 这个选择器里。把关于文字的属性，写在伪类选择器中

伪类的权重和类一样

### background 属性

###### background-color

```css
div {background: red;}
```

背景颜色属性

###### background-image

```css
div {background-image: url(images/1.jpg);}
```

背景图片属性，默认会横向、纵向铺满盒子

###### background-repeat

此属性可设置背景图片的平铺方式，值可为 `no-repeat` `repeat-x` `repeat-y`

###### background-position

```css
div {
    background-image: url(images/1.jpg);
    background-position: 100px 200px;
}
```

图片位置属性，以上效果为让盒子向右移动 100px，向下移动 200px，向左向下移动要使用负数

```css
div {background-position: left center;}
```

也可使用 `left` `right` `center` `top` `bottom`来定位	

> CSS 雪碧技术：指把多个小杂碎图片，合成一张图片，然后用 background-position 定位只显示其中某一部分。这样能够显著降低 HTTP 请求数

> background-position: center top; 经常用于大背景、通栏 banner 的制作

###### background-attachment

```css
div {background-attachment: fixed;}
```

背景附属属性，它的值 `fixed` 可用于固定背景

```css
div {background: color url(1.jpg) no-repeat center top fixed;}
```

背景属性综合写法

###### 背景图的应用

- 图换文字

  ```css
  .header h1{
  	width: 221px;
  	height: 68px;
  	background:url(images/logo.png);
  	text-indent: -999em;   /*赶走文字，让用户看不到文字*/
  }
  ```

  例如将 logo 图片放到 h1 标签，但又希望 h1 里是文字，利于搜索引擎识别

- 先导符号放到 padding

- CSS 精灵

  先导符号一定要放到精灵图的最右边

### 定位

CSS 三大技术：盒模型、浮动、定位，这三大技术负责了网页的布局

###### 相对定位

```css
div {
    position: relative;
	top: 100px;
    left: 100px;
}
```

相对与自己原来位置进行的调整，移动时需要使用定位值

> top: 正数向下移动
>
> bottom: 正数向上移动
>
> left: 正数向右移动
>
> right: 正数向左移动

相对定位不脱标，并且实际位置还是在原来的位置，不常用，一般用于微调元素，“子绝父相”

###### 绝对定位

```css
div {
    position: absolute;
	bottom: 100px;
    left: 100px;
}
```

绝对定位的元素，脱离标准文档流，以页面左上角为参考点

> 要注意绝对定位的定位参考点

绝对定位在老家不留坑，真实位置是飘动的，在页面中很常用

绝对定位的元素，无视参考盒子的 padding	

###### 祖先盒子当参考点

一个盒子可以以一个离自己最近的、已经定位的祖先盒子作为定位参考点，祖先盒子不管是相对定位、绝对定位还是固定定位都可以作为参考点

定位模型：子绝父相、子绝父绝、子绝父固

###### 绝对定位元素的居中

绝对定位的盒子不再属于标准文档流，不能再使用 `margin: 0 auto` 进行居中

```css
div {
    position: absolute;
    left: 50%;
    top: 0;
}
```

可使用以上方式对绝对定位盒子进行居中

压盖效果一般都是使用绝对定位做的

###### 固定定位

```css
div {position: fixed;}
```

固定定位以浏览器的角为参考点，固定定位的元素脱离标准文档流，固定定位的元素不会随着窗口的卷动消失

固定定位的参考点必须是浏览器，无法让一个盒子当做 `fixed` 的参考点

###### 脱标

三种脱标方式：浮动、固定定位、绝对定位

三种脱标方法只能有一种作用于同一个元素

`overflow: hidden` 可以让浮动的元素撑高它的父元素，但绝对定位和固定定位不可以

### z-index 属性

此属性用于设置压盖顺序，默认情况下：定位的元素能够压住没有定位的；都定位时 HTML 中顺序靠后的能够压住前面的

```css
div {z-index: 5;}
```

此属性没有单位，

数字大的能够压住数字小的，具有从父现象：父元素值小，子元素值再大也没用

```css
<div class="linzhiying">	/*z-index:9;*/
	<p class="kimi"></p>	/*z-index:100000;*/
</div>	
<div class="wangjianlin">	/*z-index:10;*/
	/*z-index:无论是多少肯定能压住kimi因为父亲z-index大*/
	<p class="jame"></p> 
</div>
```

### Hack

针对不同的浏览器写不同的 HTML、CSS ，从而让各种浏览器达到一致的渲染效果

###### HTML Hack

```html
<!-- [if lte IE 8] -->
HTML 内容
<!-- [endif] -->
```

上边的代码浏览器如果是 IE 6、7、8 则会进行解析，其他浏览器则当做注释，此 Hack 不能写在 CSS 里面

###### CSS Hack

- 值 Hack

  ```css
  div {_background: red;}
  ```

  IE 6：短横线、下划线

  ```css
  div {!background: red;}
  ```

  IE 6、7： ! $ & \* ( ) = % + @ , . / ` [ ] # ~ ? : < | \*/  任一

  ```css
  div {background: red\0/;}
  ```

  IE 8、9：\0/

  ```css
  div {background: red\9;}
  ```

  IE：\9

- 选择器 Hack

###### IE6 的问题

- 选择器的兼容问题
- 盒模型的兼容问题
- 浮动的兼容问题
- 定位的兼容问题
- 文字样式的兼容问题

### 透明

###### 盒子透明

```css
div {opacity: 0.40;}
```

opacity 表示透明度，值可为0、1，1表示实心，0表示纯透明

IE6-8 不支持此属性

设置盒子透明后里面的文字也会一起有透明，应该把文字单独放出去，用绝对定位将他们定位到一起

###### 图片透明

- jpg/jpeg
- png
- gif
- bmp