

# 1. 指令可以做什么？
指令可以装饰我们的页面，也可以创造标签<br>
ng开头的指令是angular自带的指令，我们写指令的时候，不要用ng开头

# 1.1 指令的参数

 我们的指令是`不依赖`于`控制器`的，默认返回一个对象 
 
> * 通过模块声明指令
> * param1：指令的名字
> * param2: 函数

# 1.2 指令的分类

    指令的分类，1）装饰型指令 2）组件式指令
    
```

//声明的默认指令不会创建作用域，不是独立作用域，找到最近的作用域，没有其他作用域的情况下，找到$rootscope根作用域上 

app.directive('myDirective', function () {

        return{
        
            restrict:'E',
            
            template:'<div>hello angular  {{age}}</div>',
            
            replace:true,
            
            link:function(scope,element,attrs){
            
                scope.age="wendy"
                
            }
            
        }
        
    });   
```

# 2. 指令返回的对象中的属性详解
 
## 2.1 restrict
 > 作用：限制指令使用范围。
 > 值有4个：ECMA<br>
   默认为`EA`,class样式默认不会替换，注释内容被插入后仍然看不到<br>
   一般我们的指令只写在EA上
   > * E:element
   > * C:class
   > * M:comment
   > * A:attribute
   
## 2.2 replace
  > 值为true时，只保留模板内容,原有的指令被替换掉 
   
## 2.3 template

> 作用：作为模板，把自定义指令替换为模板值。
 如果把原有的指令替换掉(即relace:true),根节点必须只有一个,否则会报错。

## 2.4 link

> 作用：连接视图和作用域。

> 若指令中定义有require选项，则link函数会有第四个参数，代表控制器或者所依赖的指令的控制器（即实例注入）
> * scope代表当前作用域
> * element代表当前的元素, element为jq元素, angular.element
> * attrs代表当前指令上的所有属性
> * 属性的名字用-的话需要转换成驼峰命名
> * 在页面上如果使用了`<my-directive>` 在js中要变成驼峰命名法

## 2.5 templateUrl

> 使用template可以直接写结构标签，但是过多的结构标签放在javascipt代码中，不是不可以，只是不够优雅，我们追求的是优雅的代码。<br>
  templateUrl应运而生，我们像ng-include那样，可以新建一个xxx.html文文件，把需要的结构直接放在html文件中就可以了，templateUrl:'xxx.html',就可以引用到需要的结构，并替换或者放到指令中了
  
## 2.6 基本的transclude
（transclude还具有很有深度的意义，这里不做深究）
简单的讲，transclude主要完成以下工作，取出自定义指令中的内容(就是写在指令里面的子元素)，以正确的作用域解析它,然后再放回指令模板中标记的位置(通常是ng-transclude标记的地方)
> transclude:true,将指令内部的代码保存下来，放到指定transclude标签内。<br><br>
一般是新建一个空标签，例如`<span ng-transclude></span>`,否则标签内部的内容会被替换掉，当然，当我们足够强大的时候，这个问题就是小菜一碟了.

## 2.7 scope（重要）

指定scope:{}，设立多个独立作用域，这样取得的值将不会被覆盖，就例如for循环中的i值。当我们设置了独立作用域，那么就不能找上级作用域上定义的属性

scope中很重要的三个符号：

>"@"：引用的是字符串，不支持双向数据绑定,取到的只是字符串,<br>
例如：title:'@', //相当于在当前指令下声明一个title属性,去引用当前指令上的属性,当引用的名字，和声明的名字可以省略引用的名字.<br>


>"="：等号取得是变量，在当前作用域下查找到的变量对应的值。支持双向数据绑定，引用的是当前作用域下对应的变量

>"&"：引用的是控制器上的方法，传递参数时，要通过对象传递，键代表的是名字

## 2.8 compile

> * compile选项可以返回一个对象或者函数。<br>

在这里我们可以在指令和实时数据被放到DOM中之前进行DOM操作，

比如我们可以在这里进行添加或者删除节点的DOM的操作。<br>

所以编译函数是负责对模板的DOM进行转换，并且仅仅只会运行一次。

> 对于我们编写的大部分的指令来说，并不需要对模板进行转换，所以大部分情况只要编写link函数就可以了。

> 在指令中compile与link选项是互斥的，如果同时设置了这两个选项，那么就会把compile所返回的函数当做是链接函数，而link选项本身就会被忽略掉


## 2.9 controller

指令中的controller是用来让不同指令间通信用的。

## 2.10 require

> * require:'girl',//直接写girl表示在当前指令下查找（平级）
> * require:'^girl'//直接写girl表示在当前指令下查找（平级+上一级）
> * require:'?^girl',//找不到的话不报错
> * require:'?girl',//找不到的话不报错*/

## 2 控制器的实例
```

<!DOCTYPE html>
<html lang="en" ng-app="appModule">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<!--vm就代表当前控制器的实例-->
<body ng-controller="myctrl as vm">

<my mytile = {{vm.title}}></my>
<!--相当于声明了一个title = 'hello zfpx'-->
<script src="angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('myctrl', function () {
        this.title = 'hello zfpx';//为了省略$scope
    });
    app.directive('my', function () {
         return {
             template:'<div>hello {{title}}</div>',
             scope:{
                 title:'@mytile'
             },
         }
    });
    /*function myctrl(){
        this.name = 123;
    }
    var vm = new myctrl();
    vm.name*/

</script>
</body>
</html>


```