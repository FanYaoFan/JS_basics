# JavaScript
## 目录
## 1 [作用域与闭包](https://github.com/FanYaoFan/JS_basics/blob/master/JS/1%E4%BD%9C%E7%94%A8%E5%9F%9F/README.md)
## 2 [面向对象设计](https://github.com/FanYaoFan/JS_basics/blob/master/JS/2%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/README.md)
## 3 [原型](https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/3%E5%8E%9F%E5%9E%8B.md)
## 4 [DOM](https://github.com/FanYaoFan/JS_basics/blob/master/JS/4DOM/README.md)
## 5 [事件](https://github.com/FanYaoFan/JS_basics/blob/master/JS/5%E4%BA%8B%E4%BB%B6/README.md)
## 6 [错误处理与调试](https://github.com/FanYaoFan/JS_basics/blob/master/JS/6%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86/README.md)
*** 
# 基本概述
### 可点击目录导航到具体md文件查看)[目录](#javascript)
## 1 作用域 
### 1.1 原理
1 当浏览器的内核渲染和解析JS的时候,会提供一个供JS代码运行的环境,我们把这个环境称之为’全局作用域’ (global/window scope)   
2 代码自上而下执行,变量提升与函数声明.
基本数据类型的值会存储在当前作用域下  
eg var a = 12  
1. 首先开辟出一个空间存储12(栈)
2. 在当前作用域声明一个变量a(var a)
3. 让声明的变量a和存储的12关联.(把存储的12赋值给a =>定义)  
**基本数据**(值类型)是按照值来操作的,把原有的值复制一份放到新的空间/地址上.和原来的值没有关系 `var a =12; var b = a; var b = 13;` a仍是12.与b没有关系
**引用类型**  
引用类型的值不能直接存储到当前作用余下.需要先开辟一个新的空间,把内容存储到这个空间中(堆内存) var obj1 = {n : 100}  
1. 首先开辟出一个新的内存空间,把对象中的键值对一次存储起来.这个空间有一个16进制的地址  
2. 声明一个变量 
3. 让变量和空间地址关联在一起(把空间地址赋值给变量)  
引用类型不是按值操作,按值空间的引用地址来操作.把原来的空间地址赋值给新的变量.但原来空间没有被克隆.还是一个空间.  
### 栈内存: 
 本身就是一个供JS代码执行的环境,所有的基本类型都会直接的在栈内存中开辟一个位置进行存储
### 堆内存: 
用来存储引用类型中的信息值的对象存储的是键值对,函数存储的是代码字符串
[code]()  
js中用来检测数据类型的方式  
* typeof   
* instanceof   
* constructor   
* object.prototype.toString.call()  
练习 code(base) 
### 1.2 闭包
函数执行形成一个私有作用域，保护里面的内容不受外界干扰也可以认为是形成一个不销毁的私有作用域(私有栈内存)  
作用: 
1. 闭包具有保护作用,保护私有变量不受外界干扰
2. 闭包具有保存作用,形成一个不销毁的栈内存,把一些值保存下来,方便后面调用
```JavaScript
 var a = function(){ 
 return function(){
      console.log(n)
    }
     )
    var b = a ;
```
   
### 1.3 面向对象 
面向对象编程(object oriented programming)  
#### 1.3.1 单例设计模式(singleton partern) 
表现形式: let obj = { a : xxx}  
obj 不仅仅是对象名,还是一个命名空间  
2 作用:  
把描述同一事物的属性和特征进行'分组','归类'(避免与全局之间冲突和污染)
3 高级单例模式:  
```JavaScript
let nameSpace =（function（）{
let a=12；
function fn（）{xxx
} return { fn}
})()
```

这种模式的好处：我们可以在a中创造很多内容（变量or函数）， 哪些需要调用，就暴露到返回的对象中。模块化实现的一种思想。
#### 1.3.2 对象字面量
`var obj1 = new Object() 
var obj2 = {name : 'zs',age : 18,} `   
### 1.4 JSON  
* 数据使用名/值对表示。
* 使用大括号保存对象，每个名称后面跟着一个 ':'（冒号），名/值对使用 ,（逗号）分割。
* 使用方括号保存数组，数组值使用 ,（逗号）分割。  
转换  
JSON => JS
```JavaScript
1. JSON.parse() 
eg var obj = JSON.parse( '{ "a" : "Hello", "b" : "world"}') => { a : 'hello' , b : 'world'}
```  
2. JSON.stringify()
```JavaScript
var json = JSON.stringify( a : 'hello' , b : 'world') => { "a" : "Hello", "b" : "world"}  
```
JS => JSON 

## 2 数组
### 2.1 API 
1. 意义
2. 参数  
3. 方法的返回值  
4. 是否改变原有数组
***  
 #### 2.1.1 push/pop         
###### push  
* 作用: 数组末尾添加  
* 参数: 添加的内容
* 返回值: 新增后的数组长度 length
* 原有数组改变
###### pop  
* 作用: 数组末尾删除  
* 参数: 无
* 返回值: 被删除项的值
* 原有数组改变
___
 #### 2.1.2 shift&ushift 
###### shift  
* 作用: 删除数组第一项  
* 参数: 无
* 返回值: 被删除项的值
* 原有数组改变  
###### unshift 
* 作用: 数组开始位置增加新内容 
* 参数: 增加的内容
* 返回值: 新增后的数组长度 length
* 原有数组改变 
___
 #### 2.1.3 splice    
操作任意项     
* 1 __删除__ ary.splice(n,m)
从索引n开始,删除m项的内容
eg 
```JavaScript
var arr = [12,23,34,45,9,20] splice(2,3)
//=> [34,45,9]
```

返回值 删除的部分已新数组返回  
原数组改变  
* 2 __新增__
splice(n, 0 ,xxx)
从索引n开始,删除0项,添加xxx 
返回值 [] 原数组改变  
```JavaScript
var ary = [12,34,45,34,56] 要实现在34之后加一个xx, 则 splice(4,0 ,xx)   
```
* 3 __修改__  
splice(n,m,x) 
作用 把索引n开始,(包括索引n) 删除m项,新增x
思考: 删除数组最后一项有几种方法  
```JavaScript
pop/splice(ary.length-1)/ary.length--  
push/splice(ary.length , 0 ,xx)/ary[ary.length]
```
项末尾追加内容有几种方法  
___
 #### 2.1.4 slice   
在一个数组中,按照条件查找出其中的部分内容    
参数 n,m 从索引n开始找到m,但不包括m  
返回值: 以一个新的数组存储查找内容  
原有数组不变
slice(2,6)  即 找到的是 从 2~5的
concat 
数组克隆,一般实现多个数组拼接 返回拼接后的新数组 原有数组不变
___  
### 2.2 转换  
* 数组转化为字符串   
tostring(默认以逗号分隔)    
join(可以自定义分隔符号)   
* 字符串转化为数组
* split()
___   
### 2.3 数组排序
__sort()__
a-b 升序 
b-a 降序
reverse(反转数组) 
检测数组  
indexOf & lastindexOf 
用来检测当前值 第一次/最后一次出现的索引值 > -1 就是含有这个值  
```JavaScript
if(ary.indexOf(100) > -1 ){ there is 100 }
```
***
### 2.4 数组去重 
注意: 在用splice进行数组去重时会有数组塌陷的现象存在 数组塌陷,每次循环之后,索引进1   
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/img/aryfor.png"  height="350">
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/img/ary.png" width="1200">
### 2.5 其他
__i++和++i__   
i++是先参与运算再自增 (先拉你=在打药)  
++i是先自增再参与运算 (先打药在拉你)     
__逻辑与&&和逻辑或||__  
var a = A || B 先验证A的真假,A为真就返回A 为假就返回b   
var b = A && B 先验证A的真假,A为真就返回B ,为假就返回A  
eg var a = 1 || 2 首先验证1是否为真,是真就把1赋值给a    

