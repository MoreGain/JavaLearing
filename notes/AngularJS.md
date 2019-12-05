# AngularJS

### 一、概述

###### 模块化开发

两种基本模式：AMD/CMD

###### 模块化开发思想

MVC: model/view/controller

SPA: single page application

###### Angular

MVC 前端框架，自称 MVVM 框架，适合大型 SPA 项目，符合 AMD 规范

Angular 不是类库也不是框架，直接更改看待 HTML 的方式。

Angular 是纯模块化的，遵守 AMD 规范开发



### 二、MVC、MVVM 和 MVP

###### MVC

模型 model，视图 view，控制器 controller

controller 是 model 和 view 之间的协调者(coordinator)

###### MVVM

模型 model，视图 view，视图模型 viewmodel

与 MVC 区别，模型中的数据一旦变化，会自动影响视图，而不需要控制器协调，也就是啥双向绑定思想

###### MVP

模型 model，视图 view，代理人 presentor



### 三、安装

###### 使用 Bower 下载 angular

npm: 管理后端依赖、项目依赖、CLI 程序的工具；Bower: 管理前端依赖的工具



### 四、语法

1. 指令

   - ng-app

      一个页面仅能出现一个；表示了 angular 的控制范围；

   - {{}}

      表达式模板定位符，可以是运算、三元运算，不能是函数、循环、判断、赋值等

   - ng-controller

     控制器，通过模块创建 `module.controller("name", [])`

   - ng-model

     two-way databinding，双向数据绑定，视图和控制器中的数据实时双向绑定

   - ng-click

     angular 的点击事件指令

   - ng-style

     使用 angular 指令控制元素样式

   - ng-disabled

     可通过条件控制

   - ng-repeat（展示集合数据，进行增删等操作）

     变量名 in 数组数据

   - ng-show

     通过条件控制元素的显示

   - ng-options（省级联动效果）

     数组：item for item  in arr

     数组对象：提交值 as 显示值 `item.attr1 as item.attr2 for item  in arr`

     对象（JSON）：提交值 as 显示值 `value as key for (key,value) in obj`

   - 表单验证

     验证的控价必须有 ng-model 属性双向数据绑定；form 必须有 name 属性；满足这两个条件时表单验证就已经开始了

     有错误时，控件会自动产生  `myform.控件name.$error.错误名` 这样的值为 true

   - ng-bing

     等价于 {{}}

   - 自定义指令

     ```js
     // 定义指令需要使用驼峰命名，使用时变为短横使用
     myapp.directive("MyDirective", [function(){
         return {
             template: "<h1>我是自定义指令MyDirective</h1>"
         }
     }])
     ```

     默认情况下，自定义指令可以通过属性（A）、元素（E）两种方式使用，但指令的使用方法总共有四种：AECM，即属性、元素、类、注释，可通过 restrict 来定义其使用方式

     ```js
     myapp.directive("MyDirective", [function(){
         return {
             restrict:"AEC",
             template: "<h1>我是自定义指令MyDirective</h1>",
             // 此属性可将html模板放在外边，angular通过ajax进行读取，此时页面需运行在服务器环境
             templateUrl: "/a.html",
             // 此属性表示链接指令内部和外部的关系函数
             link: function($scope, ele, attr){
                 $scope.a = 100;
             }
         }
     }])
     ```

     link 属性，

2. 控制器

   angular 1.3.2 之后允许控制器被显示实例化，在此之前使用 $scope 表示 ng-controller 控制的标签的作用域

   控制器被实例化的时机：当 ng-controller 元素上 DOM 树的时候，就会实例化控制器，并执行其其中的语句，ng-show 不会导致控制器的重新实例化，因为它是元素的 css 显示隐藏，ng-if 会导致控制器重新实例化，因为它会导致 DOM 上下树

3. 服务

   angular 服务都是单例对象，永远只有一个实例会被创建，当第一次调用服务的时候，服务被实例化，此后使用的都是这个实例

   - service 函数

     service 函数用于定义一个服务，其实就是一个普通函数，这个函数是一个类，在使用时会被系统自动实例化一次，必须使用类名打点调用其中的方法

     `myapp.service("MyService", [function](){});`

   - factory() 函数

     返回一个对象的函数，就是工厂，工厂函数一定要返回一个对象，factory 函数严格要求只能用暴露的函数来操作值，它返回的其实就是一个 API 清单，它不能使用 this 来定义属性，方法，必须使用 return 来返回，封装性好于 service()

     ```js
     myapp.factory("MyFactory", [function(){
         return {
             
         }
     }])
     ```

   - constant() 函数

     用于定义公共的常数，直接定义对象，这个对象是唯一的，使用时直接使用`服务名.属性`

     ```js
     myapp.constant("MyConstant",{
         "pi": 3.14,
         "phone": {
             "北京": "010",
             "上海": "021"
         },
         "zhochang": function(r){
             return 2*this.pi*r;
         }
     })
     ```

   - value() 函数

     与 constant() 函数类时，都是用于存放一些数据或方法以供使用，constant 一般存放固定内容，value 存放的内容可能会被修改

   - provider() 函数

     它是 service、factory 的底层函数，它接受两个参数，第一个为服务名，第二个为一个函数，函数需返回一个对象，返回的对象必须要有 $get 方法，$get 方法必须返回一个对象 obj，这个对象就是真正注入的服务

     ```js
     myapp.provider("serviceProvider", function() {
         return {
             $get: function(){
                 return {
                     // 使用时直接通过点调用 service.MyService
                     serviceName: MyService
                 }
             }
         }
     })
     ```

     provider 函数外层的东西用于 config() 函数，config 表示配置

     ```js
     myapp.provider("serviceProvider", function() {
         var a = 1;
         return {
             // 此方法只能通过config调用，表示对服务的配置
             set: function(number){
             	a = numeber;
             },
             $get: function(){
                 return {
                     value: a
                 }
             }
         }
     })
     ```

     ```js
     // 配置的参数名字必须在最后加上Provider，angular通过名字来识别配置对象
     myapp.config(function(serviceProviderProvider){
         serviceProvider.set(666);
     });
     ```

   - config() 函数

     配置函数，所有内置函数都可以通过 config 来配置

     一般习惯将其写在最上面，angular 执行流程： `扫描代码->执行config()->执行服务的定义->注入控制器`

4. 路由