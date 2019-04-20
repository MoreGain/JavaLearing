# Deprecated

##### html标签

```html
<h1></h1>	<p></p>	<br/>	<b></b>	<strong></strong>	<i></i>	<em></em>	<hr/>

<font color="" size="" face=""></font>

<img src="" width="400px(30%)" height="" alt="" />	<!-- 相对路径与绝对路径 ../../表示上上层目录 -->

<ul type="suqare"><li><a href="" target="_blank"></a></li></ul>
<ol type="a" start=""></ol>

<table border="" width="" height="" bgcolor="" align="">
    <tr>
        <td colspan="" rowspan=""></td>
    </tr>
</table>

<form action="" method=""></form>
<input type="" name="" id="" />
<!--  type:
text password radio checkbox file submit button reset hidden
h5新增: date number tel email...
-->
<textarea cosl="" rows=""></textarea>
<seclect>
    <option></option>
</seclect>

<frameset cols="" rows="">
    <frame src="" name=""/>
</frameset>

<div></div>	<span></span>
```

[带语义标签](https://www.jianshu.com/p/8f1ca8b776d3)

### CSS笔记总结

##### 引入方式

```html
<link rel="stylesheet" href="style.css" /> <!-- 外联样式 -->

<style></style> <!-- 内联样式 -->

<p style="color:red;font-size:7"></p> <!-- 行内样式 -->
```

##### 选择器

- 元素选择器

  ```html
  <style>
      p {
          color: red;
      }
  </style>
  <p>haha</p>
  ```

- ID选择器

  ```html
  <style>
      #p1 {
          color: red;
      }
  </style>
  <p id="p1">haha</p>
  ```

- 类选择器

  ```html
  <style>
      .p2 {
          color: red;
      }
  </style>
  <p class="p2">haha</p>
  ```

- 选择器优先级

  > 1.行内样式>ID选择器>类选择器>元素选择器
  >
  > 2.就近原则

- 其他选择器

  - 分组选择器	

    > h1,h2

  - 属性选择器	

    > a[title]	
    >
    > a[title='aaa']
    >
    > a[href]\[title='aaa']

  - 后代选择器

    > h1 em

  - 子元素选择器

    > h1 > em

  - 伪类

    > ```html
    > a:link {color: #FF0000}		/* 未访问的链接 */
    > a:visited {color: #00FF00}	/* 已访问的链接 */
    > a:hover {color: #FF00FF}	/* 鼠标移动到链接上 */
    > a:active {color: #0000FF}	/* 选定的链接 */
    > ```

  

##### 浮动与清除

> float: right; left; none; inherit;
>
> clear:  left; right; both; none; inherit;

##### 常用属性

> text-decoration
>
> text-align
>
> line-height
>
> list-style
>
> display

##### 盒子模型

> 外边距 	margin
>
> 内边距 	padding
>
> 绝对定位 	position: absolute	top	left

### JS笔记总结

##### JS引入方式

```html
<script src="xxx.js"></script>

<script>javascript code</script> <!-- <head>与<body>均可放入 -->
```

##### JS输出

```html
<script>
	//document.getElementById("").innerHTML = "";
    //document.write("");
    //通过反斜杠可以将代码换行
</script>
```



##### JS数据类型

- 基本类型

  > String, Number, Boolean, Null, Undefine

- 引用类型

  > 对象
  >
  > 内置对象

- 变量定义

  > var i;	//弱语言类型
  >
  > 使用typeof可以查看类型

##### JS函数

- 函数语法

  > function function_name(argument1, arguement2){	}
  >
  > var function_name = function(argument1, arguement2){	}

- 变量

  > 局部变量
  >
  > 全局变量
  >
  > 变量生命周期
  >
  > 未定义的变量直接赋值将被视为全局变量

##### JS组成

> ECMAScript	DOM	BOM

##### JS常用对象

- Browser对象

  - Window

    > setInterval()	setTimeout()	clearInterval()	clearTimeout()

- HTML对象

  - DOM Event

    > onload	onclick	onfocus	onblur	onkeyup	onchange	ondbclick

  - Style

    > display(布局)

##### DOM树

- 结构

  > element， attribute， text

- 常用方法

  > getElementById()	getElementByTagName()	getElementByClassName()
  >
  > createElement()	createTextNode()	createAttribute()	appendChild()

### JQuery笔记总结

##### JQuery文档就绪函数

> $(document).ready(function(){	})；
>
> $(function(){	});

##### JQuery效果

> show(),hide(),toggle()

##### JQuery选择器

> 基本	层级	过滤

### Mysql入门

##### MySQL数据类型

> int, double, char/varchar, date/time/datetime/time..., blog/

##### SQL分类

> DDL: create	drop	alter
>
> DML: insert	update	delete
>
> DCL: grant	
>
> DQL: select