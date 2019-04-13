# CSS

### 概述

css(cascading style sheet) 是层叠样式表，用于改变页面的样式

css 分为两个部分：选择元素，设置样式

### css 使用方式

###### 外部引入

```html
<head>
	<link type="text/css" rel="stylesheet" href="a.css" />
</head>
```

###### 内部引入

```html
<head>
    <style type="text/css"></style>
</head>
```

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

