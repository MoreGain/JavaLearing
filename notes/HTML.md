# HTML

### 上网

上网是请求网页的过程，网页是真实的物理存在，它位于远程服务器上，当访问一个网址如 www.baidu.com 时，其实时访问的此服务器根目录的 index.html 文件，服务器可以进行默认页面的配置

### 浏览器

Browser 其实就是一个渲染网页的软件，浏览器都有临时文件夹，请求文件都会存于临时文件夹，然后在本地计算机进行渲染，所以会有第一次打开网页慢第二次打开快的现象

### HTML 文件

Hypertext Markup Language ，超文本标记语言，网页文件的基本格式

###### 纯文本

只有文字，没有其他任何东西，如颜色、字号、样式等的文件。如 .txt 文件就是纯文本文件，.doc 文件就不是纯文本文件

###### HTML 描述文本语义

HTML 是纯文本文件，它可以通过各种标签给文字加上语义，例如通过 `<p></p>` 标签表示其中的文字为一个段落，这就是所谓的超文本

### HTML 基本结构

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
<html>
    <head>
    	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
        <title></title>
    </head>
    <body></body>
</html>
```

###### 文档声明头 DTD(Doctype Definition)

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
```

告知浏览器 HTML 文件的版本

- HTML 4.01

  严格版(HTML 4.01 Strict)：不能使用废弃标签、框架集

  通用版(HTML 4.01 Transitional)：可以使用废弃标签，不能使用框架集

  过渡版(HTML 4.01 Frameset)：可以使用框架集

- XHTML 1.0 

  更严格的 HTML，规定标签必须小写，属性必须双引号封闭，必须有结尾反斜杠等

  严格版(XHTML 1.0 Strict)：不能使用废弃标签，框架集

  通用版(XHTML 1.0 Transitional)：可以使用废弃标签，不能使用框架集

  过渡版(XHTML 1.0 Frameset)：可以使用框架集

- HTML 5

  最新的 HTML 版本，不再区分 strict、transitional、frameset 等版本

###### 命名空间

```html
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
```

xmlns(XML Namespace) 属性在 XHTML 中是必需的，但在 HTML 中不是。不过，即使 XHTML 文档中的 \<html> 没有使用此属性，W3C 的验证器也不会报错。这是因为 "xmlns=http://www.w3.org/1999/xhtml" 是一个固定值，即使您没有包含它，此值也会被添加到 \<html> 标签中

xml:lang="en" 属性表示所有的标签的语言都是英语

这两个属性都是固定写法，没有别的值

###### META 标签

```html
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
```

1. 字符集

   通过 meta 标签的 content 属性可以设置 HTML 的编码方式，简体中文的字符集有 GBK、UTF-8

   GBK(gb2312) 字符集中仅包含汉字，一个汉字占两个字节

   UTF-8 是国际标准字符集，包含所有国家文字和符号，一个汉字占三个字节

   >  通过将网页的字符集变为 gb2312 ，能够降低页面的尺寸，让网页打开速度变快。腾讯首页就使用的 gb2312 编码追求打开速度

2. 关键字和页面描述

   ```html
   <meta name="keywords" content="关键字" />
   <meta name="description" content="页面描述" />
   ```

   良好的关键字和页面描述能够显著提升 SEO(Search Engine Optimization)，即让网站在搜索引擎中的排名更靠前

### HTML 页面结构特点

###### 对换行符、Tab 缩进、空格不敏感

> 标签间的换行符、Tab 缩进、空格不会对 HTML 页面结构造成影响
>
> 通过将 HTML 代码压缩为一行可以提升页面加载速度

###### 空白折叠现象

HTML 中的文字，不管有多少空格、换行、缩进，都会被压缩为一个空格

### HTML 标签

######  h 标签

```html
<h1>热点新闻</h1>
```

h1~h6(head) 表示标题标签，是一个文本级标签

通常一个页面只有一个 h1 标签，其他标签个数不定。h1 标签中的内容权重非常高，搜索引擎会特别注意抓取里面的文字。搜索引擎如果看见页面有多个h1，视为作弊，会降低你的权重。

###### p 标签

```html
<p>今天天气真好！</p>
```

p(paragraph) 表示段落标签，文本级标签，p 标签中只能放文字、图片、表单元素，不能放其他东西

> 标签类别
>
> 1. 容器级标签：可放任何内容，甚至包裹和自己一样的标签。在 HTML 4.01 版本，只有 div、li、dd、dt、td 是容器级标签
> 2. 文本级标签：只能放文字、图片、表单元素和其他文本级标签

###### img 标签

```html
<img src="a.jpg" width="10px" height="5px" alt="this is a image" />
```

img 表示图像标签，用于向网页中插入图片，它和 meta 一样是自封闭标签。网页中能插入图片格式：jpg/jpeg/png/bmp/gif，不能插入 psd 格式图片

img 标签实际上也是一个“文字”，所以有空白折叠现象

- src 

  值为图片路径，可以是相对路径或绝对路径

  **相对路径**：从 html 页面出发，找到图片。这种描述路径方式叫相对路径。`../` 用于获得上层文件，只能用于开头

  > 相对路径的好处：只要文件夹里面的文件不乱动，文件夹可复制到任何地方使用

  **绝对路径**：所有以 `http://` 开头的路径， 或者以 `/` 开头的路径，我们都称为绝对路径， `/` 表示当前服务器的根目录

  > 不要考虑直接插入硬盘中的文件，例如 C/D 盘，因为服务器上没有 C/D 盘
  >
  > 工作中，都是把图片放到同一个工作目录，比如images文件夹里面，用相对路径找到图片

- width/height 

  图片高度宽度，只设置一个时另一个按比例自动设置

- alt(alternate) 

  图片不能正常加载时的代替文字

###### a(anchor) 标签

```html
<a href="http://www.baidu.com" target="_blank" title="百度首页"></a>
```

a 表示超级链接，用于把网页与网页联系起来

- href(hypertext reference) 

  值为要跳转页面的链接地址，可以是相对路径或绝对路径

- target

  设置链接的打开窗口，`_blank` 新窗口打开，`_self` 当前窗口打开

- title

  鼠标悬停时的提示文本

页面内的锚点

```html
<a name="xxx"></a>
```

> 可通过 a 标签的 name 属性将其设置为锚点，这种 a 标签没有 href 属性和链接文字

```html
<a href="#xxx">跳转到锚点xxx</a>
```

> 可通过网站定位到锚点

> 若要把一个段落的所有文字都设置为超级链接，应用 p 标签包裹 a 标签，a 是一个“文字”

###### ul(unordered list) 标签

```html
<ul>
    <li>百度</li>
    <li>阿里巴巴</li>
    <li>腾讯</li>
</ul>
```

ul 标签表示无序列表，它是一个组合标签，必须配合儿子标签 `<li></li>` 使用。只能含有 li，不能有其他标签

li(list item) 标签是一个容器级标签，它里面可以放置任何标签

###### ol(ordered list) 标签

```html
<ol>
    <li>成都</li>
    <li>上海</li>
    <li>北京</li>
</ol>
```

ol 标签表示有序列表，它的使用与 ul 标签一致

###### dl(definition list) 标签

```html
<dl>
	<dt>HTML</dt>
    <dd>HTML是超文本标记语言</dd>
    
	<dt>css</dt>
    <dd>css是层叠样式表</dd>
    
    <dt>javascript</dt>
    <dd>js是一种脚本语言</dd>
</dl>
```

dl 标签表示定义列表，必须配合 `<dt></dt>` `<dd></dd>` 使用

dt(definition title) 与 dd(definition description) 在 dl 标签中交替出现，dd 是对 dt 的解释说明，它负责解释、定义、描述 dt。dt 后可没有 dd，也可出现多个 dd，它们都是对当前 dt 的描述。dd dt 都是容器级标签

###### 表单标签

```html
<form action="" method=""></form>
```

表单中的所有元素都要放到 form 标签中，form 标签是一个功能性标签，用于将数据提交到后台

- 单行文本框

  ```html
  <input type="text" value="" placeholder="">
  ```

  type 为 text 表示这是一个文本输入框控件，value 属性可设置它的默认值

- 单选按钮

  ```html
  <input type="radio" name="sex" id="nan"> <label for="nan">男</label>
  <input type="radio" name="sex" id="nv"> <label for="nv">女</label>
  ```

  type 为 radio 表示这是单选按钮控件，要让多个单选按钮互斥，必须设置相同的 name 属性。通过添加 label 标签，并设置 for 属性值为 radio 的 id 属性值，则点击文字就可选中按钮

- 复选框

  ```html
  <input type="checkbox" name="hobby" id="basketball">
  <label for="basketball">篮球</label>
  <input type="checkbox" name="hobby" id="soccer">
  <label for="soccer">足球</label>
  <input type="checkbox" name="hobby" id="baseball">
  <label for="baseball">棒球</label>
  ```

  type 为 checkbox 表示这是复选框控件，设置相同的 name 表示这是一组复选框

- 密码框

  ```html
  <input type="password">
  ```

  type 为 password 表示这是密码框控件

- 按钮

  ```html
  <input type="submit" value="提交按钮">
  <input type="button" value="普通按钮">
  <input type="reset" value="重置按钮">
  ```

- 下拉列表

  ```html
  <select>
  	<option>1950</option>
  	<option>1951</option>
  	<option>1952</option>
  	<option>1953</option>
  	<option>1954</option>
  </select>
  ```

  select 表示下拉框，它是一个组合标签，必须配合 option 标签使用，option 为它的选项

- 文本域

  ```html
  <textarea cols="40" rows="20">这家伙很懒，什么都没留下！</textarea>
  ```

###### 表格标签

```html
<table>
	<tr>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
    </tr>
</table>
```

基本表格由 table、tr、td 标签构成，table 表示表格，tr 表示表格行，td 表示单元格，每个 tr 内的 td 数量是一致的

th 标签与 td 都代表单元格，th 具有表头的语义

```html
<td rowspan="2" colspan="2"></td>
```

通过 rowspan/colspan 可以合并单元格，只有 td、th 有这两个属性

```html
<table>
    <caption>表格标题</caption>
    <thead>
        <tr>
        	<th>表头一</th>
            <th>表头二</th>
            <th>表头三</th>
        </tr>
    </thead>
    <thead>
        <tr>
        	<td>表格数据</td>
            <td>表格数据</td>
            <td>表格数据</td>
        </tr>
         <tr>
        	<td>表格数据</td>
            <td>表格数据</td>
            <td>表格数据</td>
        </tr>
    </thead>
</table>
```

一个完整的表格，是由 caption、thead、tbody 三部分组成的：thead 就是表格头部体的意思，tbody 就是表格内容体的意思，caption 就是表格标题

###### div(division) 和 span 标签

```html
<div>
    <h3>手机</h3>
    <ul>
    	<li>华为</li>
        <li>苹果</li>
    </ul>
</div>
<span>哈哈</span>
```

div 是典型的容器级标签，将相同或相关语义的一组元素放在同一个 div 里面。div 标签没有任何默认的样式，它是网页布局中最常使用的标签，一般通过 div+css 制作网页

span 标签是文本级的 div，把一些语义相近、功能相同的文本标签放在同一个 span 里面，直觉上 span 比 a 大，比 p 小

div 、span 单独使用无意义，都是配合 css 使用

###### 其它标签（基本不使用）

```html
<br />
<hr />
<b></b>
<u></u>
<i></i>
<del></del>
<strong></strong>
<em></em>
```

### HTML 注释

```html
<!-- 我是注释 -->
```

注释不会执行

良好的注释可以提高代码的可读性

### 转义字符

常用转义字符如下，可到 w3c 考手册 HTML ISO-8859-1 查找

```html
<!-- 空格 non-breaking space -->
&nbsp;	
<!-- 右尖括号 greater than -->
&gt;
<!-- 左尖括号 less than -->
&lt;
<!-- 版权符号 -->
&copy;
```

