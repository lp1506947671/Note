## 1:JS介绍

**1.JavaScript简介**

JavaScript是运行在浏览器端的脚步语言，JavaScript主要解决的是前端与用户交互的问题，包括使用交互与数据交互，JavaScript是浏览器解释执行的。

1995年JavaScript出生,没有任何一个脚本语言是运行在浏览器端的,为了在浏览器端完成表单验证所以研发JavaScript.

java  和 JavaScript 没有任何关系

**2.调试程序方法**  

- alert调试:用于输出错误信息或者变量/属性信息,会打断程序运行
- 右键检查->console->查看错误信息(包含错误内容,文件名及行号等)
- console.log()函数不会打断程序运行输出调试内容
- document.title(不建议使用)

## 2:JS嵌入页面的方式

**1.嵌入式JavaScript**

```js
<script type="text/javascript">        
    alert('ok！');
</script>
```

**注意:**

1.alert命令相当于python中的print用于输出变量或错误信息

**2.外链式JavaScript**

```js
<script type="text/javascript" src="js/index.js"></script>
```

**3.行内式JavaScript(一般情况下不用)**

```html
<!-- 行内式js要求：必须是事件的格式   事件：用户触发了才执行命令 k="v" -->
<input type="button" name="" onclick="alert('ok！');">
```



## 3:JS基础语法

### 3.1:变量

#### 1.变量的命名规则

```python
1、区分大小写
2、第一个字符必须是字母、下划线（_）或者美元符号（$）
3、其他字符可以是字母、下划线、美元符或数字
4、使用匈牙利命名风格(iIndex,oDiv)
5、js中变量存的是标签  标签都是数据类型为对象型  object
```

#### 2.变量的数据类型 

**1.检查数据类型的函数:typeof()**

**2.js中的数据类型**

```python 
5种基本数据类型：
1、number 数字类型 (不区分整形和浮点型)
2、string 字符串类型
3、boolean 布尔类型 true 或 false
4、undefined undefined类型，变量声明未初始化，它的值就是undefined (只有在程序出错时才会出现)
5、null null类型，表示空对象，如果定义的变量将来准备保存对象，可以将变量初始化为null,在页面上获取不到对象，返回的值就是null
1种复合类型：object
```

#### 3.变量的定义

JavaScript 是一种弱类型语言，javascript的变量类型由它的值来决定。 定义变量需要用关键字 'var'

```js
 var iNum = 123;
 var sTr = 'asd';
 //同时定义多个变量可以用","隔开，公用一个‘var’关键字
 var iNum = 45,sTr='qwe',sCount='68';
```

#### 4.变量作用域

1.变量作用域指的是变量的作用范围，javascript中的变量分为全局变量和局部变量。

```
1、全局变量：在函数之外定义的变量，为整个页面公用，函数内部外部都可以访问。
2、局部变量：在函数内部定义的变量，只能在定义该变量的函数内部访问，外部无法访问。
```

2.js和Python中变量作用域的对比

```
1、函数内部使用全局变量,不需要global，直接变量赋值.
2、js声明变量可以不加var，不加var是全局变量，要求如果是函数体里面定义变量（期望是局部）-- 一定加var
```

3.变量的使用遵循就近原则:多个同名变量使用时,离自己最近的变量是全局变量即使用全局变量,是局部变量集使用局部变量

```js
<script type="text/javascript">
    // 定义全局变量
    var a = 12;
    function myalert()
    {
        // 定义局部变量
        var b = 23;
        alert(a);
        // 修改全局变量
        a++;
        alert(b);
    }
    myalert(); // 弹出12和23
    alert(a);  // 弹出13    
    alert(b);  // 出错
</script>
```



#### 5.语句与注释

1.JavaScript命令末尾需要写";",防止进行压缩后代码无法正常运行

2.javascript注释

```js
<script type="text/javascript">    
// 单行注释
var iNum = 123;
/*  
    多行注释
    1、...
    2、...
*/
</script>
```

### 3.2:条件语句

**1.条件运算符**

```
==、===、>、>=、<、<=、!=、&&(而且)、||(或者)、!(否)
```

**"==","==="的区别**

```js
"==":只判断数值相等
"===":不仅判断数值,还判断数据类型是否相等
```

**2.if else**

```js
var iNum01 = 3;
var iNum02 = 5;
var sTr;
if(iNum01>iNum02){
    sTr = '大于';
}
else
{
    sTr = '小于';
}
alert(sTr);
```

**3.多重if else语句**

```js
var iNow = 1;
if(iNow==1)
{
    ... ;
}
else if(iNow==2)
{
    ... ;
}
else
{
    ... ;
}
```

### 3.3:循环语句

**1.while循环**

```js
    // 1、变量设定初始值
    // 2、while while (条件){命令}
    var i = 0;
    while(i<3)
    {
        alert('while')
        // 增量
        // i+=1等价于i++,  i-=1等价于i--
        i++
    }
```

**2.for循环**

```js
    // for(初始值;条件;增量){命令}
    for(var i=0;i<3;i++)
    {
        alert('for')
    }
```

### 3.4:数组

#### 1.定义数组的方法

```js
//对象的实例创建
var aList = new Array(1,2,3);
//直接量创建
var aList2 = [1,2,3,'asd'];
```

#### 2.操作数组中数据的方法 

```js
//1、获取数组的长度：aList.length;
var aList = [1,2,3,4];
alert(aList.length); // 弹出4

//2、用下标操作数组的某个数据：aList[0];
var aList = [1,2,3,4];
alert(aList[0]); // 弹出1

//3、join() 将数组成员通过一个分隔符合并成字符串
var aList = [1,2,3,4];
alert(aList.join('-')); // 弹出 1-2-3-4

//4、push() 和 pop() 从数组最后增加成员或删除成员
var aList = [1,2,3,4];
aList.push(5);
alert(aList); //弹出1,2,3,4,5
aList.pop();
alert(aList); // 弹出1,2,3,4

//5、reverse() 将数组反转
var aList = [1,2,3,4];
aList.reverse();
alert(aList);  // 弹出4,3,2,1

//6、indexOf() 返回数组中元素第一次出现的索引值
var aList = [1,2,3,4,1,3,4];
alert(aList.indexOf(1));

//7、splice() 在数组中增加或删除成员
var aList = [1,2,3,4];
aList.splice(2,1,7,8,9); //从第2个元素开始，删除1个元素，然后在此位置增加'7,8,9'三个元素
alert(aList); //弹出 1,2,7,8,9,4
arr2.splice(1, 2) // 从第1个元素开始，删除2个元素
aList.splice(3)  // 位置  删除这个位置后面的所有
```

```js
// 多维数组指的是数组的成员也是数组的数组
var aList = [[1,2,3],['a','b','c']];
alert(aList[0][1]); //弹出2;
```

#### 3.循环写入数组数据

```js
    window.onload = function(){
        //(工作中运用极其频繁)
        var oMyul = document.getElementById('myul')
        var arr = ['侏罗纪世纪', '侏罗纪世纪2','侏罗纪世纪3', '侏罗纪世纪4']
        var str = ''
        for(var i=0;i<arr.length;i++)
        {
            str += '<li>'+ arr[i] +'</li>'
        }
        oMyul.innerHTML = str
    }
```

#### 4.数组去重

```js
    var aList = [1,2,3,4,4,3,2,1,2,3,4,5,6,5,5,3,3,4,2,1];
    var aList2 = [];
    for(var i=0;i<aList.length;i++)
    {
        // 方法一:
        if(aList.indexOf(aList[i])==i)
        {
            aList2.push(aList[i]);
        }
        // 方法二:
        if(aList.indexOf(aList[i])!=i)
        {
            aList.splice(i,1);
            i--;
        }
    }
    alert(aList1);
    alert(aList2);
```

### 3.5:字符串

**1、parseInt() 将数字字符串转化为整数**

```js
var sNum01 = '12';
var sNum02 = '24';
var sNum03 = '12.32';
alert(sNum01+sNum02);  //弹出1224
alert(parseInt(sNum01)+parseInt(sNum02))  //弹出36
alert(parseInt(sNum03))   //弹出数字12 将字符串小数转化为数字整数
```

**2、parseFloat() 将数字字符串转化为小数**

```js
var sNum03 = '12.32'
alert(parseFloat(sNum03));  //弹出 12.32 将字符串小数转化为数字小数
```

**3.substring() 截取字符串 用法： substring(start,end)（不包括end）**

```js
var sTr = "abcdefghijkl";
var sTr2 = sTr.substring(3,5);
var sTr3 = sTr.substring(1);

alert(sTr2); //弹出 de
alert(sTr3); //弹出 bcdefghijkl
```

**4.split() 把一个字符串分隔成字符串组成的数组**

```js
var sTr = '2017-4-22';
var aRr = sTr.split("-");
var aRr2= sTr.split("");

alert(aRr);  //弹出['2017','4','2']
alert(aRr2);  //弹出['2','0','1','7','-','4','-','2','2']
```

**5.字符串反转**

```js
var str = 'asdfj12jlsdkf098';
var str2 = str.split('').reverse().join('');

alert(str2);
```

**6.字符串合并操作：“ + ”**

```
var iNum01 = 12;
var iNum02 = 24;
var sNum03 = '12';
var sTr = 'abc';
alert(iNum01+iNum02);  //弹出36
alert(iNum01+sNum03);  //弹出1212 数字和字符串相加等同于字符串相加
alert(sNum03+sTr);     // 弹出12a
```

**7.indexOf() 查找字符串是否含有某字符**

```
var sTr = "abcdefgh";
var iNum = sTr.indexOf("c");
alert(iNum); //弹出2
```

###  3.6:函数

#### 1.普通函数

- 函数就是重复执行的代码片
- 函数的定义和调用
- js中的函数有函数预解析功能.可以先调用再定义(变量不支持预解析)
-  函数中'return'关键字的作用：返回函数中的值或者对象/结束函数的运行

```js
<script type="text/javascript">    
    fnAlert();       // 弹出 hello！
    alert(iNum);  // 弹出 undefined
    function fnAlert(){
        alert('hello!');
    }
    var iNum = 123;
</script>
```

```js
<script type="text/javascript">
function fnAdd(iNum01,iNum02){
    var iRs = iNum01 + iNum02;
    return iRs;
    alert('here!');
}
var iCount = fnAdd(3,4);
alert(iCount);  //弹出
```

#### 2.封闭函数

**1.封闭的定义**

封闭函数是javascript中匿名函数的另外一种写法，创建一个一开始就执行而不用命名的函数。

**2.封闭函数的作用**

封闭函数可以创造一个独立的空间，在封闭函数内定义的变量和函数不会影响外部同名的函数和变量，可以避免命名冲突，在页面上引入多个js文件时，用这种方式添加js文件比较安全

一般定义的函数和执行函数：

```js
function myalert(){
    alert('hello!');
};

myalert();
```

封闭函数：

```js
(function(){
    alert('hello!');
})();
```

还可以在函数定义前加上“~”和“!”等符号来定义匿名函数

```js
!function(){
    alert('hello!');
}()
```

**3.封闭函数的使用方法**

```js
    //方式1:
    ;(function(){
        function fn1(){
            alert(2)
        }
        fn1()
    })()
    // 方式2
    !function(){
        alert(3)
    }()
    // 方式3
    ~function(){
        alert(4)
    }()
```



##  4:JS高级语法

### 4.1:元素

#### 1.获取元素方法

第一种方法：将javascript放到页面最下边

```js
....
<div id="div1">这是一个div元素</div>
....
<script type="text/javascript">
    var oDiv = document.getElementById('div1');
</script>
</body>
```

第二种方法：将javascript语句放到window.onload触发的函数里面,获取元素的语句会在页面加载完后才执行，就不会出错了。

```js
<script type="text/javascript">
    window.onload = function(){
        var oDiv = document.getElementById('div1');
    }
</script>
....
<div id="div1">这是一个div元素</div>
```

#### 2.操作元素属性

**操作元素属性**
读取属性: var 变量 = 元素.属性名 
改写属性: 元素.属性名 = 新属性值

**属性名在js中的写法**
1、html的属性和js里面属性写法一样
**2、“class” 属性写成 “className”**
3、“style” 属性里面的属性，有横杠的改成驼峰式，比如：“font-size”，改成”style.fontSize”

```js
<script type="text/javascript">
    window.onload = function(){
        var oInput = document.getElementById('input1');
        var oA = document.getElementById('link1');
        // 读取属性值
        var sValue = oInput.value;
        var sType = oInput.type;
        var sName = oInput.name;
        var sLinks = oA.href;
        // 写(设置)属性
        oA.style.color = 'red';
        oA.style.fontSize = sValue;
    }
</script>
<input type="text" name="setsize" id="input1" value="20px">
<a href="http://www.baidu.com" id="link1">百度</a>
```

#### 3.控制html内容 

**innerHTML:可以读取或者写入标签包裹的内容**

```js
<script type="text/javascript">
    window.onload = function(){
        var oDiv = document.getElementById('div1');
        //读取
        var sTxt = oDiv.innerHTML;
        alert(sTxt);
        //写入
        oDiv.innerHTML = '<a href="http://www.baidu.com" ">百度</a>';
    	//清除html文本
    	document.getElementById('box').innerHTML = ''
    }
</script>
<div id="div1">这是一个div元素</div>
```

#### 4.控制css

**1.控制Css的方式:**

```js
<script>
    window.onload = function(){
        // 查找到对应的元素.style.要修改的样式 = 属性的值
        document.getElementById('box').style.color = 'red'
        document.getElementById('box').style.background = '#ccc'
        // 在js中xx-xx形式的属性名改写成xxXx形式
        document.getElementById('box').style.fontSize = '60px'
    }
</script>
```

## 

### 4.2:事件

#### 1.事件属性 

```
元素上除了有样式，id等属性外，还有事件属性，常用的事件属性有
鼠标点击事件属性(onclick)，
鼠标移入事件属性(mouseover),
鼠标移出事件属性(mouseout),
将函数名称赋值给元素事件属性，可以将事件和函数关联起来。
```

```js
<script type="text/javascript">
window.onload = function(){
    var oBtn = document.getElementById('btn1');

    oBtn.onclick = myalert;

    function myalert(){
        alert('ok!');
    }
}

</script>
```

#### 2.事件的使用方式

**匿名函数**
定义的函数可以不给名称，这个叫做匿名函数，可以将匿名函数的定义直接赋值给元素的事件属性来完成事件和函数的关联，这样可以减少函数命名，并且简化代码。函数如果做公共函数，就可以写成匿名函数的形式

```js
<script>
    window.onload = function(){
        var oBtn1 = document.getElementById('btn1')
        var oBtn2 = document.getElementById('btn2')
        var oBtn3 = document.getElementById('btn3')
        // 匿名函数：就是没有名字的函数  function (){}
        // **** 事件语法： 事件发生在谁身上(变量).事件属性(事件类型) = 匿名函数
        oBtn1.onclick = function(){
            alert('单击成功')
        }
        oBtn2.onmouseover = function(){
            alert('鼠标滑过')
        }
        oBtn3.onmouseout = function(){
            alert('鼠标离开')
        }
    }
</script>
```

### 4.3定时器

#### 1.定时器的种类

```
单次定时器：用时间控制命令只执行一次-->setTimeout()
多次循环定时器：用时间控制命令重复执行-->setInterval()
```

#### 2.定时器的使用方法

```js
/*
    定时器：
    setTimeout  只执行一次的定时器 
    clearTimeout 关闭只执行一次的定时器
    setInterval  反复执行的定时器
    clearInterval 关闭反复执行的定时器
*/
// 参数1：命令：1、匿名函数形式；2、函数名形式
// 参数2：延迟时间，以毫秒为单位 1000毫秒 = 1秒
var time1 = setTimeout(myalert,2000);
var time2 = setInterval(myalert,2000);
/*
clearTimeout(time1);
clearInterval(time2);
*/
function myalert(){
    alert('ok!');
}

//工作经验:为了释放浏览器缓存会在关闭定时器后使用oTimer = null
```

## 5:JS案例

### 5.1:网页换肤

#### 1.网页换肤css部分 

1.网页换肤使用外链式css(替换样式只需替换文件链接)

2.button标签自带边框使用时一般需设置边框宽度为0px

3.border-radius属性的值可以为像素值,也可以为百分比,当值为百分比时代表宽高的百分比

**pifu.css**

```css
body{
    background: #ccc;
}
button{
    width: 200px;
    height: 50px;
    background: pink;
    border: 0px;
    font-size:20px ;
    color: #000;
    border-radius: 25px ;
}
```

**pifu2.css**

```css
body{
    background: gold;
}
button{
    width: 200px;
    height: 50px;
    background: green;
    border: 0px;
    font-size:20px ;
    color: #cccccc;
}
```

#### 2.网页换肤js部分 

1.所有的html属性，在js中写法只有class=“” 在js中写为className,其余的都相同

```HTML
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="../css/pifu.css" id="mylink">
    <script>
        window.onload = function () {
            // 控制单机.控制link标签 ,换标签的href值
            var oBtn1 = document.getElementById('btn1')
            var oBtn2 = document.getElementById('btn2')
            var oMylink = document.getElementById('mylink')
            oBtn1.onclick=function () {
                oMylink.href = "../css/pifu.css"
            }
            oBtn2.onclick=function () {
                oMylink.href = "../css/pifu2.css"
            }
        }
    </script>
</head>
<body>
<h1 class="aa">网页换肤</h1>
<button id="btn1">皮肤一</button>
<button id="btn2">皮肤二</button>
</body>

</html>
```

###  5.2:打印名片

#### 1.打印名片显示数据 

1.用户在input中输入的值就是input标签中value属性的值

2.js中字符串拼接可以使用"+"进行拼接,字符串内部不能换行

3.如果input标签中value属性的值为空则需要抛出错误终止程序

4.针对不同的名片,需要切换不同的div的类名,来实现不同样式

```js
<script>
        window.onload = function () {
            var oInput01 = document.getElementById("input01")
            var oInput02 = document.getElementById("input02")
            var oInput03 = document.getElementById("input03")
            var oInput04 = document.getElementById("input04")
            var oInput05 = document.getElementById("input05")
            var oInput06 = document.getElementById("input06")
            var oInput07 = document.getElementById("input07")
            var oSetcard = document.getElementById("setcard")
            //    右边显示区域
            var oCard_wrap = document.getElementById('card_wrap')
            var str = ''
            oSetcard.onclick = function () {
                //判断用户的输入是否为空
                if(oInput01.value==''||oInput02.value==''||oInput03.value==''||oInput04.value==''||oInput05.value==''||oInput06.value==''){
                    alert("请输入内容")
                    return
                }
                //    得到用户输入的数据
                str = '            <div class="p01">' + oInput01.value + '<em>' + oInput02.value + '</em></div>' +
                    '            <div class="p02">' +
                    '                <p class="company">' + oInput03.value + '</p>' +
                    '                <p>手机：' + oInput04.value + '</p>' +
                    '                <p>email：' + oInput05.value + '</p>' +
                    '                <p>地址：' + oInput06.value + '</p>' +
                    '            </div>'

                oCard_wrap.innerHTML = str

                if (oInput07.value==0){
                    oCard_wrap.className='idcard01'
                }
                else if (oInput07.value==1){
                    oCard_wrap.className='idcard02'
                }
                else {
                    oCard_wrap.className='idcard03'
                }
            }


        }
```



#### 2.打印名片完成

1.return可以终止代码运行

2.风格切换

```js
    if(oInp07.value == 0)
        {
            oCard_wrap.className = 'idcard01'
        }
        else if(oInp07.value == 1)
        {
            oCard_wrap.className = 'idcard02'
        }
        else
        {
            oCard_wrap.className = 'idcard03'
        }
```

#### 3.扩展(switch)

```js
    switch (oInp07.value) {
        case '0':
            oCard_wrap.className = 'idcard01'
            break;
        case '1':
            oCard_wrap.className = 'idcard02'
            break;
        case '2':
            oCard_wrap.className = 'idcard03'
            break;

        default:
            break;
        }
```

### 5.3:标签位移原理

```js
    window.onload = function(){
        var oBox = document.getElementById('box')
        var num = 0;  // num就是left的值
        var speed = 5
        setInterval(fnMove, 50)
        function fnMove(){
            // 改变left取值
            num += speed
            // 如果num增到600，大于600，左侧运动
            if(num > 600)
            {
                // num-=5
                // num += -5
                speed = -5
            }
            if(num <0)
            {
                speed = 5
            }

            oBox.style.left = num + 'px'
        }
    }
```

### 5.4:无缝滚动

1.无缝滚动的核心思想是将需要滚动的图片复制一份,当超出显示区域时快速切换

2.无缝滚动实现完整js

```js
window.onload = function(){
    // 1、默认左侧移动 产品父级ul的left
    var oList = document.getElementById('list')
    var num = 0 // left值
    var speed = 5
    var oTimer = null
    // 为了产品移动，后面显示区域不落空，复制一份5个产品
    oList.innerHTML += oList.innerHTML
    // oList.innerHTML = oList.innerHTML + oList.innerHTML
    oTimer = setInterval(fnMove, 50)
    function fnMove(){
        num -= speed
        if(num < -1000)
        {
            num = 0
        }
        if(num>0)
        {
            num=-1000
        }
        oList.style.left = num + 'px'
    }

    // 2、左右箭头单击，改变运动方向
    var oBtn01 = document.getElementById('btn01')
    var oBtn02 = document.getElementById('btn02')
    oBtn02.onclick = function(){
        speed = -5
    }
    oBtn01.onclick = function(){
        speed = 5
    }

    // 3、鼠标滑过停止定时器，鼠标离开启动定时器
    var oSlide = document.getElementById('slide')
    oSlide.onmouseover = function(){
        clearInterval(oTimer)
        oTimer = null
    }
    oSlide.onmouseout = function(){
        oTimer = setInterval(fnMove, 50)
        // 定时器累加  setInterval(fnMove, 50)
    }
}
```



