
# Angular常用指令
参考：[wakeuppig](https://wakeuppig.github.io/jw_blog/html/AngularJS/angular%E5%85%A5%E9%97%A8.html)
>所有带ng开头的都是angular自带的指令,ng很强大，也很枯燥，学习ng的道路很长，学好了你就可以飞了。

### 1. 样式相关的指令

> * ng-class ：用来给元素绑定类名

> * ng-style：用来绑定元素的css样式

> * ng-show/ng-hide：值为boolean类型的表达式，当值为true时，对应的show或hide生效。只是简单的操作css样式

> * ng-if：效果跟ng-show一样， 不同的是ng-if如果是false，那么这个元素的dom节点都没有了，如果为true，那么该节点重新加载到dom；所以需要频繁的进行元素显隐控制的话，使用ng-show就可以了。

### 2. 动态添加锚点链接和图片链接

> ng-href: 如果一个锚点的链接是动态的，比如<a href="{{href}}">gogogo</a>，如果你点击那么界面会调到{{href}},为了动态添加href，那么ng-href就出现了,这样页面上就不会先出现一个地址错误的链接。

> ng-src: 在页面开始加载到ng编译完成之前，<img src=”{{imgUrl}}” />,页面上会一直显示一张错误的图片，因为路径{{imgUrl}}还未被替换，
使用ng-src指令，在路径被正确得到之前就不会显示找不到图片（也就是裂图）。

### 3. 事件相关

> * ng-click

> * ng-change
事件绑定指令的取值为函数，并且需要加上括号，例如：

```
<select ng-change=”change($event)”></select>

然后在controller中定义如下：

$scope.change = function($event){
         alert($event.target);
         //……………………
}
```

> * ng-paste， ng-copy， ng-cut是一伙的，如果输入框的值是粘帖的，那么ng-paste就为真， ng-copy，ng-cut也是同理 

### 4. 表单指令

> * ng-disabled: 这个只要出现就会生效的属性，在Angular中我们通过表达式返回值true/false另其生效。禁用表单输入字段。

> * ng-readonly：通过表达式返回值true/false将表单输入字段设为只读。是指ng-model的只读， 不能随便更改

> * ng-options：select循环数据

    <!--<select ng-options="d.value as d.name for d in data"></select>
    
    --- d in data“在哪里遍历出来的值”
    
    --- value as一个作为vaule的值，用户是看不到的
    
    --- name for 用户可见的值-->
    

### 5. 其它
> * ng-app：告诉angular当前的启动范围, 当前根作用域rootScope。
使用这个命令启动一个应用（AngularJS应用）。我们一般在body元素上添加ng-app属性， 添加了ng-app属性的元素会自动加载到angular模块里面去， 所以我们就不要写angular.bootstrap(element, ["moduleName"])， 但是自动加载的ng-app在一个页面只能有一个， 两个或者更多就会出现问题, 如果在一个html文件添加多个angular模块， 我们就就不能给元素设置ng-app属性，我们手动通过angular.bootstrap加载模块；

> * ng-model：是很重要的指令，所有学习angular开发者对这个指令都无比熟悉，ng-model主要绑定的元素包括input， select， textarea 。ng-model 挂载数据。指定ng-model会在当前作用域下查找，没有的话就变成声明

> * ng-include：指这个指令标签的innerHTML为指定的url，这个url为相对与当前目录的url地址或者绝对地址，angular会通过ajax请求该地址， 然后把地址的内容作为指令元素innerHTML填充；

> * ng-non-bindable指令指该元素的内部{{****}}不被绑定和转义，不受angular的掌控;

> * ng-repeat里面提供了几个变量，为开发者提供一些快捷的操作，
    
    　　　　--$index : 表示当前item的索引,
    
    　　　　--$first : 如果item为第一个，那么$first为true ,
    
    　　　　--$middle : 如果item不是开头，不是结尾$middle为true,
    
    　　　　--$last : 如果item是最后一个，  $last为true,
    
    　　　　--$even : 如果索引为偶数， 那么$even为true,否则为false
    
    　　　　--$odd : 同上， 索引为奇数$odd为true....;
    
    　　如果你喜欢的话，可以循环对象的key和value,{key,value} in person;

> * ng-init: 该指令被调用时会初始化内部作用域。这个指令一般会出现在比较小的应用中。

> * ng-bind用法等同于{{}} ，可以用这个指令来避免FOUC(Flash Of Unrendered Content)，也就是未渲染导致的闪烁。将挂载的数据取出来

> * ng-cloak：也可以为我们解决FOUC。ng-cloak会将内部元素隐藏，直到数据获取到以后。

> * ng-switch: 一般和ng-model配合使用。小栗子如下：

     <!--<input type="text" ng-model="name">
     <div ng-switch="name">
       <div ng-switch-when="hello">你好</div>
       <div ng-switch-when="world">世界</div>
       <div ng-switch-default>hello world</div>
     </div>-->
       //根据我们的数据显示不同的代码块
       //ng-switch 获取的是模型上的数据 scope
       //ng-switch-when="hello"  这里的hello带表的是字符串不是变量
      
> * ng-bind-template 绑定多组模板。仍然可以解决闪烁问题

```

<div ng-bind-template="{{name}}{{age}}"></div>

```




