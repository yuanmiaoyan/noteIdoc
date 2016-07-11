
# 1. DOM2基础原理
## 1. 事件
### 1.1  事件
一件事或者一个行为，不管是否给其绑定方法，只要当前行为触发，那么事件就会被触发。
> * onmousedown、onmouseup、onmousemove
> * onmouseover、onmouseout、mousewheel(**待研究**)
> * onmouseenter、onmouseleave
> * onfocus、onblur
> * onkeydown、onkeyup、onkeypress
> * onclick、dbclick、onload、onscroll、resize

### 1.2  事件的绑定
给元素的某一个事件绑定一个方法，当事件触发的时候会把绑定的方法执行。

## 2. 事件绑定原理
DOM0和DOM2的绑定是可以共存的
### 2.1 DOM0事件原理
> 原理：给元素对象的某一个事件私有的属性赋一个值(一个函数值)，当元素的事件触发的时候，找到对应的属性值，并且让其执行即可...

```javascript
document.body.onclick=funtion(){}//"DOM0级事件绑定"
document.body.onclick=null；//移除事件绑定
``` 

### 2.2 DOM2级事件原理
> 原理：默认的会给当前元素的某一个事件创建一个“内置的事件池”，通过DOM2给当前元素的某一个事件绑定的所有方法都会依次的存储到这个容器中，当事件触发的时候，会把容器中存储的所有方法依次的执行。

```
document.body.addEventListener("click",fn,false);
//"DOM2级事件绑定“。
//false:控制当前的方法是在事件的冒泡传播阶段执行
//true：控制其在捕获阶段执行(一般不用)
document.body.removeEventListener("click",fn,false);
//“移出DOM2级事件绑定”,移除的时候需要保证三个参数值和绑定的时候一模一样

document.body.attachEvent("onclick", fn1);
document.body.detachEvent("onclick", fn1);
//在IE6~8中不支持addEventListener,应该用attachEvent/detachEvent来实现DOM2的绑定和移除;
并且在IE6~8中绑定的方法只能在冒泡阶段发生,不能控制在捕获阶段发生;
```

### 2.3 DOM2事件的特点
> 1.DOM2可以给元素的某一个事件行为绑定多个方法（尽量绑定不重复的方法）
> 2.DOM2事件绑定中,我们一般绑定的都是实名函数,只有这样以后移除的时候才知道具体要移除谁,绑定匿名函数后期是无法移除的

### 2.4 DOM0级事件和DOM2级事件的区别

> * DOM0级事件只能给当前元素的某一个事件行为绑定一个办法，绑定多次，后面绑定的会把前面的方法覆盖掉
> * DOM2级事件给当前元素的某一个事件行为绑定多个不同的方法

### 2.5 DOM2事件绑定在标准浏览器和IE6~8的兼容问题总结:
```
>  绑定所用的方法及语法是不一样的
>  "this问题":
事件触发,执行对应的每一个方法的时候,标准浏览器下方法中的this是当前的元素,IE6~8下方法中的this是window
>  "重复问题":
标准浏览器下,当前元素某一个事件绑定的所有方法不能重复,如果绑定过就不在重复的绑定了,所以最后执行的时候,只执行一次;
但是IE6~8没有实现去重,哪怕方法重复了,也都会给当前元素绑定上,执行的时候绑定几次就重复执行几次;
>  "顺序问题":
标准浏览器下方法执行的顺序是按照绑定的顺序依次执行的,但是IE6~8下执行的顺序和绑定的顺序没啥关系,是混乱的;
```

## 3. 事件对象
> 事件触发的时候，相关绑定的方法都会被依次执行，不仅仅会执行，浏览器还会默认给每一个方法都传递一个参数值--“事件对象”

### 3.1 鼠标事件对象（mouseEvent）

> 用来存储当前鼠标本次操作相关信息的一个对象（它是mouseEvent这个类的一个实例）

### 3.2 键盘事件对象（KeyboardEvent）

### 3.3 事件对象的常用兼容性问题总结（在标准浏览器和ie6~8浏览器中）
|   标准浏览器    |   IE6~8   | 备注 |
| :----:   | :----:  |  :----:  |
|相同|||
| type       |    type   |存储的是当前操做的事件类型|
|clientX/clientY|clientX/clientY|当前鼠标操作这一点距离"当前屏幕窗口左上角(可视区域的左上角)"的X/Y轴坐标
|不同|||
| e| window.event | 事件对象   |
| pageX |   e.clientX + (document.documentElement.scrollLeft \\\ document.body.scrollLeft)|当前鼠标操作这一点距离"BODY左上角(第一屏幕的左上角)"的X轴坐标|
| pageY |   e.clientY + (document.documentElement.scrollTop \\\ document.body.scrollTop)|当前鼠标操作这一点距离"BODY左上角(第一屏幕的左上角)"的Y轴坐标|
|target|srcElement|事件源,当前鼠标是在谁身上操纵的,那么事件源就是谁|
|preventDefault|returnValue=false|阻止默认行为|
|stopPropagation|cancelBubble=true|阻止冒泡传播|

### 3.4 锚点定位的原理
其实就是在URL地址的末尾增加一个#ID,这样加载页面的时候就会定位到指定的ID地方

### 3.5 事件冒泡传播
> 当前元素的某一个行为被触发,那么它所有父级元素的相关行为都会被触发,这种事件的传播机制叫做“冒泡传播”。
 inner->outer->body->html->document 由里向外依次传播的
 
#### 3.5.1 onmouseenter/onmouseover的区别:
> * onmouseenter默认阻止了事件的冒泡传播,子元素的onmouseenter事件被触发,父级元素的相关事件不会被触发;但是onmouseover存在冒泡传播;
> * 红盒子是绿盒子的子元素,当鼠标从绿盒子进入红盒子,在从红盒子进入绿盒子,会重新的触发绿盒子的onmouseover事件,但是不会触发绿盒子的onmouseenter事件;

### 3.6 事件委托

> 定义：利用了事件的冒泡传播机制，当容器中某个子元素的相关行为触发，当前容器的相关行为也会被触发，如果给容器的这个事件绑定了方法，方法也会被执行，在执行的时候我们可以获取到事件源(存储的是当前操作的是哪个元素)，我们可以通过事件源是谁来做不同的操作.

---





