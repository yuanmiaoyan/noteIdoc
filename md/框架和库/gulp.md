
# 1 全局安装gulp

$npm install -g gulp(网不好的时候可能装不上，不过一般没有问题)


其他源：
> * 淘宝源 npm install -g gulp --registry=http://registry.npm.taobao.org
> * 中国源 npm install -g gulp --registry=http://registry.cnpmjs.org
> * 官方源 npm install -g gulp --registry=http://www.npmjs.org/

安装完成后，可以输入gulp -v
显示`CLI version 3.9.1`版本信息代表全局安装成功

# 2 本地安装gulp

这里以windows系统为例
 
## 2.1 新建一个目录

1）使用命令行：$ mkdir learngulp

2）使用可视化方法：鼠标右键新建文件夹

## 2.2 进入目录

1）使用命令行：cd learngulp

2）使用可视化方法：打开文件夹就好

## 2.3 初始化


1）$ npm init 一路回车下去即可，也可以选填。

最后在当前目录下会生成一个叫pakage.json项目描述文件

2）如果是使用可视化的方法，在这一步需要shift+鼠标右键打开命令窗口

## 2.4 安装gulp

$ npm install gulp --save-dev

这样可以把 gulp 作为项目的开发依赖(只在开发时用，不会发布到线上)

# 3 运行gulp

在当前项目录根目录下新建一个名为`gulpfile.js`的文件。

## 3.1 引入gulp模块

 在gulpfile.js文件中输入
```
 
 var gulp=require('gulp');
 
```
 
## 3.2 创建gulp的任务

```
 
 gulp.task('hello',function(){
    console.log("world");
 })
```
 
## 3.3 执行gulp任务

在当前项目根目录下打开命令行工具，输入gulp+taskName,不输入任务名称的话会默认找default任务，找不到会报错
```

$ gulp hello
```

会返回

```

[21:36:34] Starting 'hello'...
    world
[21:36:34] Finished 'hello' after 559 μs
```



