# node入门(2)
### fs模块
>fs模块是我们node中读写文件的核心模块，方法比较多，也比较复杂，所以单独来说，模块中的方法均有异步和同步版本，例如读取文件内容的函数有异步的 fs.readFile() 和同步的 fs.readFileSync()。
>特别注意：异步的方法函数最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)，也就是说第一个参数强制为错误信息。

1. 通过异步的方式来获取数据
```
var fs = require('fs');//同步方和异步方法，通常同时出现,出来的文件格式默认是buffer，他是全部读到缓存内，不能读取比内存大的文件
//异步方式是通过回调函数来获取数据,异步谁先执行完看谁先返回数据
//param1 错误对象 param2:获取的数据默认也是buffer类型,指定编码格式和默认的flag
var person = {};
fs.readFile('./name.txt','utf8',function (err,data) {
    person.name = data;
    out();//因为不知道什么时候算拿完了，只能写入后就调一下方法
});
fs.readFile('./age.txt','utf8', function (err,data) {
    person.age = data;
    out()
});
function out(){
    if(Object.keys(person).length==2)//获得person对象的属性的长度,获取数据的速度是读取文件最久的那个时间
    console.log(person);
}//解决异步的问题就用计数器,也可以用promise来解决。
```
2. 写入文件：
```
fs.writeFileSync('./hello.txt',"珠峰",{flag:'w'});//path文件的路径，如果文件不存在 会帮我们创建文件，data 要写入的数据 字符串格式和buffer格式，w代表的是我们打开文件要做的操作是写入，{flag:'a'}等同于appendFile
fs.appendFileSync('./hello.txt',"小米");
//return O_TRUNC 截断 | O_CREAT 创建 | O_WRONLY 只写
```
3. 同步的方法创建目录
```
function makeP(path){
    var arr = path.split('/');//将path以/的方式分割开
    for(var i = 1; i<=arr.length;i++){//在原数组里，不停的取相对应的目录，加入/
       var curDir =  arr.slice(0,i).join('/');
       var flag = fs.existsSync(curDir);//我们在创建目录之前，要先查看curDir是否存在
        if(!flag){
            fs.mkdirSync(curDir);//创建目录
        }
    }
};
makeP('a/b/c/d');//必须要保证上一目录有了才可以创建目录
```
4. 读取和操作目录以及删除文件或者目录
```
var curArr = fs.readdirSync('./dir');
console.log(curArr);
curArr.forEach(function (item) {//读取目录后要操作目录，的到目录后先查看目录是文件夹还是文件
    //判断目录的状态要用全目录
    var stat = fs.statSync('./dir/'+item);//stat代表当前状态 通过这个状态判断是文件还是文件夹//删除的时候只能删除空文件夹
    if(stat.isDirectory()){//isDirectory()
        console.log('./dir/'+item+"是文件夹");
        fs.rmdirSync('./dir/'+item);//删除目录
    }
    if(stat.isFile()){//isFile()
        console.log('./dir/'+item+"是文件");
        fs.unlinkSync('./dir/'+item); //删除文件
    }
});
```
5. 监听文件
```
var fs = require('fs');
fs.watchFile('./4.dir.js', function (cur,old) {
    console.log(cur)
})
```
6. 拷贝文件
```
var fs = require('fs');
//source原文件的路径
//target目标文件的地址
function copy(source,target){//同步的copy方法
    var data = fs.readFileSync(source,'utf8');
    //写数据把都出来的数据写入
    fs.writeFileSync(target,data);
}
//异步的copy方法
function copy(source,target){
    //异步的读取通过回调函数的方式，data读到的数据
    fs.readFile(source, function (err,data) {
        if(!err){
            //如果没有错误，写入到文件中将data数据
            fs.writeFile(target,data, function (err, data) {
            });
        }
    });
}
copy('./hello.png','./b.png');
```

7. path模块 处理路径的核心模块
```
var path = require('path');
//1.path.join方法
console.log(path.join('a','b','..'));
//会把两个路径拼接起来，以自己系统的分隔符
//1.支持多个连接
//2.支持上一个目录
//__dirname 当前文件的目录名字
console.log(path.join(__dirname,'..','a.js'));
//require.resolve();
//path.resolve解析路径
console.log(path.resolve('..','a.js/'));
//1.什么都不写会以当前的目录做解析
//2.只写/会回到根目录
//3.支持..
//4.不会保留最后一个/
//将不合法的路径转换成标准路径
var format = path.normalize('a////c////d///..//e/');
console.log(format);
//默认多个/会转换成一个/
//最后一个/会保留
//两个点会变成上一级目录


//path.basename获得文件的基本名字
//后面写上匹配出来的当前的文件的后缀
//拿去一个文件的名字 在拿去另一个文件的后缀，生成新的文件
console.log(path.basename('a.js','.js'));
//path.extname
console.log(path.extname('b.png'));
console.log(path.basename('a.js','.js')+path.extname('b.png'));

console.log(path.sep);//获得目录分割符

console.log(path.delimiter); //获取环境变量分隔符 mac是冒号，//可以区分是mac还是windows
```