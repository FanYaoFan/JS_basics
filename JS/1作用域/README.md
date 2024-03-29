# 作用域与闭包
# 1 堆栈内存:
就是中的内存分为:堆内存&栈内存  
堆内存:存储引用类型数据值(对象:键值对,函数:代码字符串)  
栈内存: 提供Js代码执行的环境,存储基本类型值,(eg,函数会开辟一个栈内存执行js)
## 1.1 数据类型  
基本类型:number,string,undefined,null,boolean
复杂(引用)数据类型:Array,function,object(对象)  
简单数据类型:传输值   
引用数据类型:传输地址(修改同一内存空间)  
__当你想修改成员变量中的简单数据类型时，不要用参数传递，，要在函数中修改全局变量__  
# 2 变量提升  
## 2.1 浏览器解析顺序  
1. 在全局作用域下,先进行变量提升和函数的声明+定义(函数的定义就是在堆内存开辟出一个地址存储代码字符串,在将地址返回给浏览器)
2. 浏览器自上从下解析

当栈内存(作用域)形成,js代码自上而下执行之前,会将带var&function进行提前声明(函数时声明+定义),预先处理这种机制就是变量提升  
* 声明(declare):var a/function fn
* 定义(defined): a = 12   
带var只声明不定义,带function是既声明又定义  
变量提升只发生在当前作用域,函数会先开辟出一个地址空间
## 2.2 带var和不带var的区别  
`var a = 12`,全局变量值更改,window的属性也在更改,即window.a = 12,在全局作用下,声明一个a,也就是相当于给全局的window设置了一个属性a,然后赋值12(注:私有作用域中声明的私有变量和window没有关系,可以用in来检测,不加var相当于是给window设置一个属性)
```JavaScript 
var a = 12 , b = 14; // ==> var a = 12, var b = 14(用逗号) 
var a=b=13,  // 连等,没有逗号 即 var a =13; b =13 (b没有var,相当于全局有一个属性b=13)
```
## 2.3 常见的变量提升 
### 2.3.1. 只对等号左边进行变量提升 
普通函数 function(){} (这种要进行变量提升)
匿名函数 var fn = function(){} (只对左边fn进行变量提升 => 在全局作用域下 var fn)
### 2.3.2 条件判断下的变量提升
```JavaScript  
// 一
console.log(a)    // undefined
if(1===2){
    var a = 12
}
console.log(a)  //undefined 
// 二
console.log(a)   // undefined 
if('a' in window ){
    var a = 100
}
console.log(a)  // 100 
```
`在当前作用域下,不管条件是否成立都要进行变量提升`  
在新版本浏览器中,(对于条件判断中的函数)不管条件是否成立,都只是先声明没有定义,类似于var.  
**要区分fn和fn()和f()还有fn()()的意思**  
1. fn: 代表函数本身
2. fn():函数执行
3. fn()():函数执行后的返回结果(return)再执行一次
条件判断的坑:  
```JavaScript
if(1 === 1){
    console.log(fn)
    function fn(){
        console.log('ok')
    }
}
// 在全局作用域下,声明(像var)函数fn,即window下有个属性fn,
// 其结果在控制台查看就是: 
ƒ fn(){
        console.log('ok')
    }
```
```JavaScript
if( !("a" in window)){
    var a = 1
}
console.log(a) //undefined

```
### 2.3.4 逻辑或与非
看第一个参数的真假,然后再赋值 
1. ||: var x=A || B (若A为真,就返回第二个参数B)
2. &&: var x=A && B (若A为真,就返回第一个参数A)
3. !=: 
```JavaScript
    f = function () {
        return true
    }
    g = function () {
        return false
    }
    ~function () {
        if (g() && [] == ![]) {
            //对g进行声明,在~function的私有作用域中
            // 声明一个g()但没有定义
            // 报错 g is not function
            f = function () {
                return false
            }
            function g() {
                return true
            }
        }
    }()
    console.log(f())
    console.log(g())
```
### 2.3.5 变量提升遇到重名问题 
1. 带var和function关键字声明有相同的名字
eg  
```JavaScript
var fn = 12 
function fn(){} 
// 两者重名 
```
2. 关于重名，不会重新声明，而是重新赋值，（无论是变量提升还是代码阶段)  
   
eg  
```JavaScript
fn() //=> 4
function fn(){console.log(1)}
fn() //=> 4 
function fn(){console.log(2)}
fn() //=> 4 
var fn = 100 
fn() //=> 报错 这里执行的是 fn=100 即 100()
function fn() {console.log(3)}
function fn() {console.log(4)}
```
# 条件判断变量源码(1Variable.html)  
# 3 闭包 
函数执行形成一个私有作用域，保护里面的内容不受外界干扰也可以认为是形成一个不销毁的私有作用域(私有栈内存)
作用:
1. 闭包具有保护作用,保护私有变量不受外界干扰
2. 闭包具有保存作用,形成一个不销毁的栈内存,把一些值保存下来,方便后面调用

在私有作用域中(闭包中),思路是:
1. 先形参赋值
2. 变量提升 

在私有作用域中,只有下面两种是私有变量  
A:声明过的变量  
B:形参  
剩下的都不是,要根据作用域链向上查找
```JavaScript
var a = 10, b = 11, c = 12
    function test(a) {
        a = 1  
        var b = 2
        c = 3
    }
    test(10) 
    // a=10 作为实参传给形参a,则a是test()的私有变量,所以使得a(10)=>a=1,但不会影响到全局下的a,
    // 全局下的a仍为10
console.log(a)
console.log(b)
console.log(c)
```

查找上级作用域 
当前函数执行的时候,会形成一个私有作用域,一个函数的上级作用域在哪跟它在哪里执行是没有关系的,只和它在哪里创建(定义)有关系,在哪创建它的上级作用域就是谁  
eg 
```JavaScript
var a = 12
function fn(){
    console.log(a)
}
function sum() {
    var a =120
    fn()
}
sum() 
//fn()是在全局下创建的,所以其上级作用域就是 window
// => 12 
```
##  [作用域练习代码](https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/1%E4%BD%9C%E7%94%A8%E5%9F%9F/2Scope.html) 
## 练习: 
### 1 
```JavaScript
  var a = 12, b = 12, c = 14;
        function fn(a) {
            console.log(a, b, c)
            var b = c = a = 20
            console.log(a,b,c)
        }
    fn(a) 
    console.log(a,b,c)
```  
解析  
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS/1%E4%BD%9C%E7%94%A8%E5%9F%9F/scopeImg/scope1.png"></img>
***
### 2 
```JavaScript
var ary = [12,23]
    function fn(ary){
        console.log(ary)
        ary[0] = 100
        ary = [100]
        ary[0] = 0
        console.log(ary) 
    }
    fn(ary) 
    console.log(ary)  
```
解析:
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS/1%E4%BD%9C%E7%94%A8%E5%9F%9F/scopeImg/Scope3.png"></img>
***
### 3  
```JavaScript
     var n = 10 
    function fn() {
        var n =20 
        function f() {
            n ++ 
            console.log(n)
        }
        f() 
        return f
    }
    var x = fn() 
    x()
    x() 
    console.log(n)
```
解析:
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS/1%E4%BD%9C%E7%94%A8%E5%9F%9F/scopeImg/Scope4.png"></img>
