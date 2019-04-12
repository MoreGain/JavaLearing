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

> 提升权重一般只在原子类的情况使用

> !important不影响继承性，一个标签是通过继承性影响的，权重是0，加上!important也是0
>
> !important不影响就近原则，远的那个，写上!important也没用，还是以近的那个为准

