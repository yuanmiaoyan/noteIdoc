
# http

> * 要建立一个http 需要引入核心模块http

> * 服务的特点 通过固定ip和 端口号，响应数据<br>
 主要由两部分组成req请求,res响应(res是个可读可写入的流)
 
> * setHeader:设置头，让浏览器按正常的形式解析(以前我们是res.writeHead)
 
##  1.1 2种写入文件的方式
 ```
 //
 var http = require('http');
 var fs = require('fs');
 
 http.createServer(function (req,res) {
    res.setHeader('content-type','text/html;charset=utf8');    
     //方式一：文件读写的方式
      fs.readFile('./index.html', function (err,data) {
         if(!err)
             res.end(data);
     })
     //方式二：流的方式
     //直接将读取的流导入到可写流里
     fs.createReadStream('./index.html').pipe(res);
 }).listen(8888);
 ```
 ## 1.2 请求
 
 > 当我们请求html的时候，遇到引用文件、图片都会再次发送请求
 
 > * req.url//当前请求的路径
 
 > * req.headers//请求的头部
 
 > * req.method//请求的方法
 
 > * res.statusCode = 404;
 
 ## 1.3 mime-type 
 
 我们要解析mime-type，它是第三方模块，要安装mime-type（webstorm中view-->Tool Windows-->Terminal-->npm install mime安装即可或者git）；<br>
 
 fs.existsSync('.'+pathname)//判断文件是否存在
 
 ### 1.3.1 lookup
 通过mime的lookup方法可以查看对应得mime-type
 ```
 
res.setHeader('content-type',mime.lookup(pathname)+';charset=utf8');
 ```
 
 ## 1.4 url详解
 ```
 
 var url = require('url');
 
 var urlObj = url.parse('https://123:jw@www.baidu.com:90/s?wd=charles&rsv_spt=1&rsv_iqid=0x9d1b5b1e0017bcdc&issp=1&f=8&rsv_bp=0&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=1&rsv_sug3=4&rsv_sug1=4&rsv_sug7=100#123',true);
 console.log(urlObj);
 /*  protocol: 'https:', 协议
     slashes: true,  是否有//
     auth: '123:jw',  当前用户名密码
     host: 'www.baidu.com:90', 地址（端口）
     port: '90', 端口号
     hostname: 'www.baidu.com', 地址
     hash: '#123',hash值
     search: '?wd=charles&rsv_spt=10', 查询串
     query:
     { wd: 'charles',}, 查询对象当参数为true，是对象
     pathname: '/s', 当前的路径地址
     path: '/s?wd=charles, 当前的路径
     href: 'https://123:jw@www.baidu.com:90/s?wd=charles } 完整地址
 ```
 
 ## 1.5 querystring 
 
 > 用来解析查询字符串的 核心模块(即内置)
 
 ```
 
 var querystring = require('querystring');
 //手动设置分隔符 param1是要解析的字符串  param2 每一组内容的分隔符 键值分隔符
 //解析成对象
 var obj = querystring.parse('username==123&&password==321','&&','==');
 //将对象转换成字符串
 var str = querystring.stringify({ username: '123', password: '321' },'*','@');
 console.log(str);
 ```
 ## 1.6 setEncoding
 ```
 
 res.setEncoding('utf8')
 ```