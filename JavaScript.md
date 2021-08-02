# 1. 什么是JavaScript

JavaScript是一门世界上最流行的脚本语言

==一个合格的后端程序员必须精通JavaScript==

# 2. 快速入门

## 2.1引入JavaScript

```html
1. 内部标签
 <script>
     alert("hello");
 </script>
2. 外部引入
<!--外部引入 注意 script必须成对出现 type不用显示定义-->
<script type="text/javascript" src="js/qj.js"></script>
```

## 2.2基本语法

```html
<script>
        // 1. 定义变量： 变量类型  变量名 = 变量值;
        var score = 1;
        // 2. 条件控制
        if(score>60&&score<70){
            alert("60~70");
        }else if(score>70&&score<80){
            alert("70~80");
        }else{
            alert("other");
        }
        // console.log(score)  //在浏览器控制台打印信息

</script>
```

## 2.3数据类型

==变量==

```html
//全局变量 
i=1
//局部变量用let定义
let i=1;
//'use strict'严格检查模式，预防js的随意性导致产生一些问题
<script>
        //定义严格检查模式,必须写在第一行
        'use strict';
        //局部变量
        let i=1; 
</script>
```



==number：==

js不区分小数和整数

```javascript
123 //整数
123.1 //浮点数
1.123e3 //科学计数
-99 //负数
NaN //Not a number
Infinity //表示无限大
```

==字符串==

1. 正常字符串使用单引号或双引号包裹
2. 转移字符 \

```javascript
\'
\n
\t
\u4e2d 
```

3. 多行字符串编写

```javascript
//tab键上面，esc键下面
var msg=`hello
        hello
        hello`
```

4. 模板字符串

```javascript
//tab键上面，esc键下面  ``
let  name="hello";
let age=3;
let msg=`你好呀，${name}`
```

5. 字符串长度

```javascript
str.length
```

6. 字符串可变性，不可变
7. 大小写转换

```javascript
//注意这是方法 不是属性
str.toUpperCase()
str.toLowerCase()
```

8. str.indexOf(‘t’)
9. substring

```javascript
str.substring(1) //从第一个取到最后一个
str.substring(1,3) //[1,3)
```



==布尔值==

==流程控制==

1. if判断
2. while循环
3. for循环

==逻辑运算==

==比较运算符==！！！！重要

```javascript
=
== 等于（类型不一样，值一样，也会判断为true）
=== 等于（类型一样，值一样，才会判断为true）
不要使用==比较
NaN与所有的数值都不相等，包括自己，只能用isNaN(NaN)判断。
```

浮点数问题

```javascript
console.log((1/3)===(1-2/3))  结果为false 无限小数有精度损失
```

尽量避免使用浮点数进行运算，存在精度问题！

```javascript
console.log(Math.abs(1/3-(1-2/3))<0.000000000001)    结果为true
```

==null和undefined==

- null 空
- undefined 未定义

==数组==

```javascript
//保证代码可读性，尽量使用[]
var arr = [1,2,3,4,null,"hello",true]
```

取数组下标越界时，会出现undefined 

1. 长度

```javascript
arr.length
```

注意：如果给arr.length赋值可以改变数组的长度，元素就会丢失

2. indexOf, 通过元素获得下标索引

```javascript
arr.indexOf(2)
```

3. slice() 截取array的一部分，返回一个新数组，类似于String 中的substring
4. push() pop()

```javascript
push: 压入到尾部
pop: 弹出尾部的一个元素
```

5. unshift() shift()

```javascript
unshift:压入到头部
shift：弹出头部的一个元素
```

6. sort()

```javascript
arr.sort()
```

7. 元素反转reverse()
8. concat()

```javascript
arr.concat([1,2,3])
//注意，这里返回一个新数组，并不改变arr数组
```

9. 连接符join
10. 多维数组

```javascript
arr=[[1,2],[3,4],[6,5],["7","8"]]
```



==对象==

对象是大括号，数组是中括号，每个属性之间使用逗号隔开，最后一个不需要添加

1. 对象定义，若干键值对,所有的键是字符串，所有的值都是任意对象

```javascript
var person={
            name: "xiaowu",
            age: 3,
            tags: ['js','java','web']
        }
```

2. 取对象的值

```javascript
person.name
"xiaowu"
person.age
3
```

3. 动态增删属性

```javascript
//动态删除
delete person.name
//动态增加
person.haha="haha"
```

4. 判断属性值是否在这个对象中

```javascript
"age" in person
"toString" in person //来自父类
```

5. 判断一个属性是否是这个对象自身拥有的

```javascript
person.hasOwnProperty('toString')  //false
person.hasOwnProperty('age')  //true
```



# 3.函数

## 3.1 定义函数

绝对值函数

```javascript
function abs(x){
  if(x>=0){
    return x;
  }else{
    return -x;
  }
}

//定义方式二
var abs=function(x){
  arguments
  if(typeof x!=='number'){
    throw 'Not a Number!';
  }
  if(x>=0){
    return x;
  }else{
    return -x;
  }
}
//function(x){--}这是一个匿名函数，把结果赋值到abs，通过abs就可以调用函数
//arguments是一个js的关键字，代表传递进来的所有的参数，是一个数组
//问题，arguments包含所有的参数，
var abs=function(x){
  for(int i=0;i<arguments.length;i++){
    
  }
}
//rest ES6新特性,获取除了已经定义
var abs=function(x,y,...rest){
  
}
```

## 3.2 变量的作用域

在js中，var定义的变量实际是有作用域的，

假设在函数体中声明，则在函数体外不可以使用（闭包）

内部函数可以访问外部函数定义的变量

js实际上只有一个全局作用域，任何变量（函数也可视为变量），假设没有在函数作用范围内找到，就会向外查找，如果在全局作用都没有找到，报错`RefrenceError`

规范：

由于我们所有的全部变量都绑定到window上。如果不同的js文件，使用了相同的全局变量就会冲突，解决冲突的方法

```javascript
//唯一全部变量
var HeavenApp={};
        
//定义全局变量
HeavenApp.name="xiaowu";
HeavenApp.add=function (a,b){
    return a+b;
}
```

**把自己的代码全部放入自己定义的唯一空间名字中，降低全局命名冲突的问题   jQuery**

局部作用域 let

```javascript
function aaa(){
            for (let i = 0; i < 100; i++) {
                console.log(i);
            }
console.log(i);//报错
        }
```

**建议使用`let`去定义局部作用域变量**

常量 `const` ES6之后加入的

```javascript
const PI=3.14
```

## 3.3 方法

方法就是把函数放在对象内部，

```javascript
var xiaowu={
  
  name:"wu",
  birth: 1997,
  age:function (){
     var now=new Date().getFullYear();
     return now-this.birth;
   }
}
//方法调用
xiaowu.age()
```

在js中可以控制this的指向。`apply`

```javascript
function getAge(){
    var now=new Date().getFullYear();
    return now-this.birth;
}
var xiaowu={
    name:"wu",
    birth: 1997,
    age: getAge
}
//调用
getAge() //报错，this指向window
getAge.apply(xiaowu,[]);//this,指向了xiaowu
```

# 4.内部对象

## 4.1 date

```javascript
var now = new Date();
now.getFullYear();//年
now.getMonth();//月
now.getDate();//日
now.getDay();//星期几
now.getHours();//时
now.getMinutes();//分
now.getSeconds();  //秒

now.getTime(); //时间戳，全世界统一
new Date(now.getTime()); //时间戳转时间

now.toLocalString; //注意，该调用是一个方法
```

## 4.2 JSON

>json是什么，一种轻量级的数据交换格式，

早期的数据传输使用xml

在js中，一切皆是对象，任何js 支持的类型都可以转换为JSON

格式：

- 对象都用{}
- 数组都用[]
- 所有的键值对，都用key:value

```javascript
 var user={
     name:"xiaowu",
     age: 3,
     gender: 'man'
}
//对象转化为JSON字符串
var jsonUser= JSON.stringify(user);
        
//JSON字符串转化为对象
var obj=JSON.parse(jsonUser);
```

## 4.3 Ajax

- 原生的js写法，xhr异步请求
- jQuery封装好的方法：`$("#name").ajax("")`
- axios请求

# 5 面向对象编程

- 类：模板
- 对象：具体的实例

## 5.1 js中的原型：

```javascript
var user={
    name:"xiaowu",
    age: 3,
    gender: 'man',
    run:function (){
        console.log(this.name+"run.....");
    }
}
        
var xiaoming={
    name: "xiaoming"
}
xiaoming.__proto__=user;
```

## 5.2 class继承 ES6引入的

```javascript
class Student{
   constructor(name) {
      this.name=name;
   }
            
   hello(){
      console.log("hello");
   }
}

var xiaoming=new Student("xiaoming");

class xiaoStudent extends Student{
   constructor(name,grade) {
       super(name);
       this.grade=grade;
   }
   myGrade(){
       console.log("wo shi yi ming xiao xue sheng");
   }
}

var xiaohong=new xiaoStudent("xiaohong",3);
```

## 5.3 原型链：

![image-20210521160957152](E:\TyporaImg\image-20210521160957152.png)



# 6. 操作BOM对象(重要)

js和浏览器的关系：js在浏览器中运行

BOM：浏览器对象模型。

- IE 6~11
- Chrome
- Safari
- FireFox 

> window

window 代表 浏览器的窗口

```javascript
window.alert()
```

> Navigator

```javascript
navigator.appName
```

大多数时候，不会使用`navigator`对象，因为会被人修改

> screen 代表屏幕

```javascript
screen.width
```

> location （重要）location 代表当前页面的URL信息

```javascript
host:"www.baidu.com"
href:"https://www.baidu.com/"

location.assign('网页地址')
```

> document 当前页面，信息，HTML DOM文档树

```javascript
document.title

document.getElementById('app') //获取具体的文档树节点，可以动态的删除和增加节点

//获取cookie
document.cookie

//劫持cookie原理通过一段JS代码获取到cookie
//服务器段可以设置cookie: httpOnly
```

> history 网页后退

```javascript
history.back();
history.forward();
```

# 7. 操作DOM（重点）

DOM：文档对象模型，整个浏览器网页就是一个DOM树形结构

- 更新：更新DOM节点
- 遍历DOM节点
- 删除DOM节点
- 增加DOM节点



```html
//获得DOM节点
<div class="father">
    <h1>标题1</h1>
    <p id="p1">p1</p>
    <p class="p2">p2</p>
</div>
<div id="id1">

</div>
<script>
  
   //获得DOM节点
    var h1= document.getElementsByTagName('h1')
    var p1=document.getElementById('p1')
    var p2 = document.getElementsByClassName('p2')

    var father = document.getElementsByClassName('father')
    var childrens=father.children //获取当前节点的所有子节点
    
    //操作节点
    var id1=document.getElementById('id1')
    id1.innerText='456'
  //操作样式
  p1.style.color='red'
  
  //删除节点，先获取父节点，在用父节点删除
  var self=document.getElementById('p1')
  var parent = self.parentElement;
  parent.removeChild(p1)
  
 		

</script>
```

这是原生代码，推荐使用jQuery

## **增加节点**

获得某个DOM节点，假设这个dom节点时空的，通过innerHTML增加一个元素，但是这部DOM节点已经存在元素，就不能这样操作。或会产生覆盖。

```html
<body>
<p id="js">JavaScript</p>
<div id="list">
<p id="se">JavaSE</p>
<p id="ee">JavaEE</p>
<p id="me">JavaME</p>
</div>

<script>
    var js=document.getElementById('js');
    var list=document.getElementById('list');
		list.appendChild(js);//追加节点

var newP=document.createElement('p');//创建一个新的p节点
    newP.id='newP';
    newP.innerText='Hello';
    list.appendChild(newP);//追加节点
    list.insertBefore(newnode,targetnode)
</script>
</body>
```

## 操作表单（验证）

md5加密密码

```html
<!--表单绑定事件
onsubmit 绑定一个提交检测的函数，true,false
将这个结果返回给表单，使用onsubmit接收
onsubmit="return aaa()"
-->
<form action="post" onsubmit="return aaa()">
    <span>用户名：</span><input type="text" id="user">
    <span>密码:</span><input type="password" id="input-psd">
    <input type="hidden" id="md5-psd">

    <button type="button" >submit</button>
</form>

<script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.min.js"></script>
<script>
    function aaa(){
        var uname = document.getElementById('user');
        var inputpsd = document.getElementById('input-psd');
        var md5psd = document.getElementById('md5-psd');
        md5psd.value=md5(inputpsd.value)

        //可以校验判断表单内容，true就是通过提交，false阻止提交
        return true;
    }


</script>
```

# 8.jQuery

jQuery库，里面存在大量的js方法库

引入在线的jquery文件

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://lib.baomitu.com/jquery/3.6.0/jquery.js"></script>
</head>
```

使用js

```html
<body>
<a href="" id="test-jquery">dianji</a>
<script>
    <!--
    JS公式:
  				$(选择器).action()
    -->
    $('#test-jquery').click(function (){
        alert("112121");
    })
</script>
</body>
```

工具网站：http://jquery.cuishifeng.cn/

事件：

鼠标事件，键盘事件，其他事件