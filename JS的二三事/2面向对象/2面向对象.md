# object oriented programming
# 1 面向对象编程
## 1.1 this指向问题 
### 1.1.1 this的作用域 
this在任何情况下都不指向函数的词法作用域。在JavaScript内部，作用域确实和对象类似，可见的标识符都是它的属性。但是作用域“对象”无法通过JavaScript代码访问，它存在于JavaScript引擎内部。  
### 1.1.2 调用位置  
分析调用栈(就是为了到达当前执行位置所调用的所有函数)
**调用位置就在当前正在执行的函数的__前__一个调用中**  
调用位置决定了this的绑定(有点难理解)
```JavaScript
function baz(){
//当前调用栈baz
//=>当前调用位置是全局作用域
console.log('baz')
bar()//bar的调用位置
}
function bar(){
//当前调用栈(正在执行的函数)是baz->barjs解析到这,调用栈从baz切到bar
//因为调用位置是指当前正在执行的函数的上一个调用中
//因此，当前调用位置在baz中
console.log("bar");
foo();//<--foo的调用位置
}

function foo(){
//当前调用栈是baz->bar->foo
//因此，当前调用位置在bar中
console.log("foo");
}
baz();//<--baz的调用位置

```
## 1.1.3 绑定规则
判断this指向,方法遵循以下顺序  
1. 函数是否在new 中调用（new 绑定）？如果是的话this 绑定的是新创建的对象。
`var bar = new foo()`
2. 函数是否通过call、apply（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是指定的对象。  
`var bar = foo.call(obj2)`
3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上下文对象。  
`var bar = obj1.foo()`
4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到undefined，否则绑定到全局对象。

#### 1. new绑定
通常是 `something  = new Myclass()`
```JavaScript  
var a = 1 
function fn(a) {
    this.a  = a 
}
var b = new fn(2)
console.log(b , a)
 // new fn(2) 传参给fn, this.a =a 即把传的a赋值给window下的a 
//   b  = fn{
//       a : 2 
//     }
// a 还是全局下的1 
```

使用new来调用函数(发生构造函数调用时,会自动执行下面的操作) 
1. 创建(构造)一个全新的对象
2. 这个新对象会被执行的[原型]连接
3. 这个新对象会绑定到函数调用的this
4. 若函数没有返回对象,那么new表达式中的函数调用会自动返回这个新对象  
#### 2 硬绑定  
bind(),call(),apply()强制绑定
bind():能让我们的函数延迟执行
apply()和call()调用就执行  
所以bind这样的形式我们也称为函数柯里化  
```JavaScript
functionfoo(){
    console.log(this.a);
}
var obj={
        a:2
    };
var  ba = function () {
    foo.call(obj);
} ;
bar();//2
setTimeout(bar,100);//2
//硬绑定的bar不可能再修改它的this
bar.call(window);//2
```  

我们创建了函数bar()，并在它的内部手动调用了foo.call(obj)，因此强制把foo的this绑定到了obj。无论之后如何调用函数bar，它总会手动在obj上调用foo。这种绑定是一种显式的强制绑定，因此我们称之为硬绑定。
#### 3 隐式绑定  
API调用上下文
分析隐式绑定时，我们必须在一个对象内部包含一个指向函数的属性，并通过这个属性间接引用函数，从而把this间接（隐式）绑定到这个对象上  
```JavaScript
function foo(el) {
        console.log(el, this.id);
    }
    var obj = {
        id: "awesome"
    };
    // 调用foo(..) 时把this 绑定到obj
    [1, 2, 3].forEach(foo, obj);
// 1 awesome 2 awesome 3 awesome
```
**对象属性引用链中只有最顶层或者说最后一层会影响调用位置**  
```JavaScript
var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
var obj0 = {
    a: 3,
    obj 
}
obj0.obj.test() // => 2
```
#### 4 默认绑定  
独立函数调用,可以把这条规则看做是无法应用其他规则时的默认规则
```JavaScript
function foo(){
    console.log(this.a)   //=>指向window
}
var a=2;
foo();//2
```   
严格模式下  
如果使用严格模式（strictmode），那么全局对象将无法使用默认绑定，因此this会绑定到undefined
### 若还是无法理解,直接遵循以下方法
1. 给当前元素的某个事物绑定方法,当事件触发方法执行的时候,方法中的this是当前操作的元素对象 
```JavaScript 
 div.onclick = function ()  {
    //  this => div
 }
```   
2. this取决于执行主体,方法执行看方法名前面是否有点,有的话,前面是谁,this就是谁;没有,this就是指向window  
```JavaScript
function fn() {
    console.log(1)
}
var obj =  {
    fn : fn
}
obj.fn()
obj.fn() 和 fn() // 执行的是相同的方法  
fn() // this执行window
obj.fn() // this 指向obj
```
## 2 面向对象 
需要掌握对象,类,实例的概念    
* 对象: 万物皆对象
* 类  : 对象中的具体细分(按照功能特点进行分类:大类,小类)
* 实例: 类中具体的一个事物  
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS的二三事/2面向对象/ObjectOrientedimg/Object.png" width="1000px"></img>

## 2.1 基本写法: 
```JavaScript
var person = {
    name : "Hi",
    age  : 29,
    job :  "Software Engineer",
    sayName : function () {
        alert(this.name)
    }
}
```
## 2.2 数据属性  
1. Configurable: 表示能否通过delete删除属性从而重新定义属性,能否把属性修改为访问器属性(默认值:true)
2. Enumerrable: 表示能否通过`for in`循环访问属性(默认值:true)  
3. Writeable: 表示能否修改属性的值(默认值:true)  
4. Value : 包含这个属性的数据值,读取属性值的时候,从这个位置读:写入属性值的时候,把新值保存在这个位置(默认值:undefined)   
```JavaScript
var person = {
    name : "gakki"
}
// 这里创建了一个名为name的属性,为它指定的值是gakki 
// value 特性被设置为gakki
```

要修改属性默认的特性,必须使用ES5里的Object.defineProperty()方法,接收三个参数(属性所在的对象,属性的名字,描述符的对象)描述符(discriptor)对象的属性必须是:configrable,enumerable,writable,value.
eg  
```JavaScript
var person = {}
Object.defineProperty(person,"name", {
    writable : false,
    value : "gakki",
})
console.log(person.name) // => gakki
// 如果该名呢? 
person.name = "Tom"
console.log(person.name) // => 仍旧输出为gakki(你对你老婆的爱至死不渝啊)
```  
这是因为,将writable的值设为了false,也就是不可修改;  严格 模式下,会抛出错误    
configurable设为false,表示不能从对象中删除属性 
## 2.3 读取属性 
Object.getOwnPropertyDescription()  
可以取得给定属性的描述符   
接收两个参数:(属性所在的对象和要读取其描述符的属性名称),返回值是一个对象,如果是访问器属性,这个对象的属性有(configurable,enumerable,get,set)
# 3 创建对象
## 3.1 单例设计模式(重点)(sington partren)
单例设计模式的由来:  每一个命名空间都是js中这个object这个内置基类的实例,是相互独立互不干扰的,所以称之为:单独的实例    
在给命名空间复制到饿时候,不是直接赋值一个对象(let nameSpace = {} `NO`),而是先执行匿名函数,形成一个私有作用域AA(不销毁的栈内存),在AA中创建一个堆内存,把堆内存地址赋值给命名空间  
可以在 AA作用域中创造很多内容(变量or函数),谁被需要就把谁暴露出去给别人使用
基本形式: 
```JavaScript
let nameSpace = (function (){
    let a = 1
    function fn() {
        xxx 
    }
    return {
        init : fn
    }
} )()


let obj = {
    a : xxx
}
// 高级 
let sum = (function () {
  let show = function （）{}

   return {
       init ： function () {
           show ()
       }
   }
}) () 
// 调用的时候 
sum.init()  

// 当需要调用别人的方法时:

let average = (function () {
     let hello = function (){
         console.log('hello')
     }
     return  {
            init : function () {
                hello() // 自己模块中的方法
                sum.show() // 调取别人的方法
            }
     }
})()
average().init()
```
`(function)()`: 自函数执行(立即函数执行),形成一个私有作用域  
这里obj/sum/average不仅仅是对象名,还是一个命名空间  
好处: 把描述同一事物的属性和特征进行"分组","归类"(存储在同一空间中,避免与全局之间的冲突)  
## 3.2 工厂模式(factory pattern)
1. 创建多个对象,把实现相同的功能代码进行封装,一次来实现批量生产,后期想要实现这个功能,我们只需要执行这个函数即可
2. 低耦合高内聚:减少页面中的冗余代码,提高代码的重复利用率  
   
eg
```JavaScript
function createPerson(name,age,job){
    var o = new Object()
    o.name = name
    o.age = age
    o.job = job
    o.sayName = function () {
        alert(this.name)
    }
    return o
} 
var person1 = cratePerson("gakki","29","actor")
var person2 = cratePerson("Greg", "10","student")
p1和p2是相互独立,之间没有关系 
``` 
# 4 构造函数 
可以改写上面的例子  
```JavaScript
function Person(name,age,job){
    var o = new Object()
    this.name = name
    this.age = age
    this.job = job
    this.sayName = function () {
        alert(this.name)
    }
} 
var person1 = new Person("gakki","29","actor")
var person2 = new Person("Greg", "10","student")
```
注意:用person()取代了createPerson(),区别
1. 没有显示的创建对象
2. 直接将属性和方法赋给了this对象
3. 没有return语句
4. new一个对象,首字母都要大写  

在创建person实例的时候,也就是new 一个新的person时,要经历一下步骤: 
1. 创建一个新对象
2. 将构造函数的作用域赋给了新对象(this也指向了这个新对象)
3. 执行构造函数中的代码(为这个新对象添加属性)
4. 返回新对象  
   
person1和person2都有一个constructor(构造函数属性),该属性指向Person,该属性最初是用来标识对象类型的,但通常使用instanceof更可靠一些.
```JavaScript
console.log(person1 instanceof Object) //=>true
console.log(person1 instanceof Person) //=>true
console.log(person2 instanceof Object) //=>true
console.log(person2 instanceof Person) //=>true
```  
创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型,而这正是构造函数模式胜过工程模式的地方
## 4.1 Js创建值
### 4.1.1
Js创建值有两种方式  
1. 字面量表达式  
2. 构造函数模式
   
不管是哪一种方式创造出来的都是Object实例,而实例之间是独立分开的
```JavaScript
// 1 字面量表达式 
var obj = { }
// 2 构造函数模式
var obj = new Object() 
Fn()  //普通函数执行
```  
#### 基本数据类型基于两者模式创建出来的值的区别  
基本数据类型基于两者方式创建出来的值是不一样的  
基于字面量方式创建出来的是__基本类型值__  
基于构造函数创建出来的是__引用类型值__
```JavaScript
var num1 = 12 
var num2 = new Number(12)
console.log(typeof num1) // => number
console.log(typeof num2) //=>  Object
```  
num2也是number的实例,num1也是数字类的实例,它是js表达数字的方式之一,都可以调用Number类的属性和方法(toFixed())
### 4.1.2创建自定义类
基于构造函数创建自定义类(constructor)
1. 在普通函数执行的基础上`new xxx()`,这样就是构造函数执行,当前的函数名称称之为"类名",接收的返回结果是当前类的一个实例  
2. 自定义类的时候(自己创建的类名),最好第一个单词字母大写. 
eg   
```JavaScript
function Fn(){
    
}
var p1 = new Fn() // Fn是类  p1是Fn的一个实例
console.log(p1) 
var p2 = new Fn() // p2也是Fn的一个实例,
// 但是,p1和p2两者互相独立,互不影响
```
3. 这种构造函数设计模式执行,主要用于组件,框架,类库,插件的封装,平时编写代码不这样处理
4. 创建新的实例,两者相互独立
## 4.3 构造函数执行的机制  
### 4.3.1 普通函数执行 Fn()   
1. 形成一个私有作用域
2. 形参赋值
3. 变量提升
4. 代码执行
5. 栈内存释放与否 
### 4.3.2 构造函数执行(new())
两者兼之
1. 向普通函数执行一样,形成一个私有作用域(栈内存)
2. 形参赋值
3. 变量提升

2,3都是私有变量
以下构造函数执行独有的机制
4. 在Js代码自上而下执行之前,首先在当前形成的私有栈中创建一个对象(创建一个堆内存:暂时不存储任何的东西),并且让函数中的执行主体(this)指向这个新的堆内存(this ===> 创建的对象[可以参考this指向问题])
5. 代码自上而下执行 
6. 5完成后,把之前创建的堆内存地址返回(浏览器默认返回)
   
eg 
```JavaScript
function Fn(name, age){
    var n = 10 
    this.name = name
    this.age = age + n
}
new Fn(gakki,19)
```
 解析:
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/2%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/ObjectOrientedimg/Constructor.png" width="1000px"></img>
 *** 
 ### 4.3.3 构造函数中的一些细节问题 
 ##### 自己写了return会发生什么?  
 1. return返回一个基本值,返回的结果依然是类的实例,没有受到影响
 2. return返回的如果是引用值,则会把默认返回的实例覆盖,此时接收到的实例就不是当前类的实例

#### 构造函数执行的时候,尽量减少return的使用,防止覆盖实例
fn 不加() => fn代表函数本身,加() => fn()代表函数执行  
在构造函数执行的时候,如果Fn不传递实参,我们可以省略()
### 4.3.4 检测属性
1. `instanceof` : 检测某一个实例是否属于这个类
```JavaScript 
var p1 = new Fn() // => new Fn
var p1 = new Fn
console.log(p1 instanceof Object) // => true
console.log(p1 instanceof Array) // => false
console.log(p1 instanceof Fn) //=> true
```
2. `in`:检测当前对象是否存在某个属性;不管当前这个属性是公有属性还是私有属性,只要有结果就是true 
```JavaScript
console.log( "m" in f) //false
console.log( 'toString' in f) // true  
``` 
3. hasOwnProperty: 检测当前属性是否为对象的私有属性(必须是私有的才可以) 

编写一个hasOwnproperty方法,检测当前属性是否为对象的公有属性,和hasOwnproperty方法相反(有问题,后期在思考)
总结: 
```JavaScript
var n = 2 
var obj = {
    n : 3,
    fn : (function (n) {
        n *= 2
        this.n += 2
        var n = 5
        return function (m) {
            this.n *= 2 
            console.log (m + (++ n))
        }
    })(n)
}
var fn = obj.fn
fn(3)
obj.fn(3)
console.log(n, obj.n)
```  
解析:   
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/2%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1/ObjectOrientedimg/this1.png"></img>
# 5 硬绑定解析
先理解原型 [原型](https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/3%E5%8E%9F%E5%9E%8B.md) 
## 5.1 call()
```JavaScript
fn.call(this)
ary.slice = ary._proto_.slice
```
fn.call:当前实例(fn)通过原型链的查找机制找到function.prototype上的call方法
fn.call():把找到的call方法执行,执行会经过下面步骤:
1. 首先要把操作函数中的this关键字变为call方法传的第一个实参值
2. 把call方法第二个及第二个以后的实参获取到
3. 把要操作的函数执行，并且把第二个以后的传递进来的实参传给函数
```JavaScript
Function.prototype.call = function () {
    let param1 = arguments[0]
    paramOther = [] //把arg中除了第一个以外的实参获取到
    this : fn //当前要操作的函数(函数类的一个实例)
    // 把Fn中的this关键字修改为Param1=> 把this(call)中的this修改为param1
    // 把fn执行,把paramOther分别传递给fn
    // this(pramaOther)
}
fn.call()
```
获取第一个实参(this),把第一个以外的调用传递给fn或者其他函数 
或者说,调用一个对象的一个方法,以另一个对象替换当前对象
```JavaScript
let sum = function(a,b){
    console.log(this)
}
let obj = {
    n : 20
}
sum.call(obj,20,30)
```
call中的this是sum,把this(call里面的)中this 修改为 obj,
然后把20,30分别传递给它, 即sum中this 是obj, a = 20,b =30

```JavaScript
sum.call.call(obj)
```
1. sum.call 找到Function.prototype上的call方法(也是一个函数,也是函数类的一个实例),也可以继续调用call/apply等方法 A
2. A.call(obj)继续 找原型上的call方法,把call方法执行:把A中的this关键字修改为obj,再把A执行
   
 eg  
```JavaScript  
function fn1(){console.log(1)}
function fn2(){console.log(2)}
fn1.call(fn2)
// fn1.call(fn2)   // 1 (this指向了f2,但仍在fn1中执行)
fn1.call === Function.prototype.call // =>true
fn1.call.call(fn2)  //2 (this指向了fn2,但在fn1.call中执行,那么谁是fn1.call呢?)
fn1.call === Function.prototype.call // =>true
fn1.call.call.call.call.call(fn2)
fn1.call.call.call.call.call === Function.prototype.call  // =>true
//**无论多少个call,都是倒数第二个后面的执行**

Function.prototype.call(fn1)
Function.prototype.call.call(fn1)
``` 
***
**无论多少个call,都是倒数第二个后面的执行** 
`Function.prototye.call.call(fn1) ` (F.P.call) 
1. 先找到call-AA把它执行,它中的this就是F.P.call 2. 再把F.P.call中的this修改为fn1,让F.P.Call执行(F.P.Call(call-AA))此时的this是fn1
3. 这一次其实就是让fn1执行 
```JavaScript

fn1.call.call(fn2)
fn1.call === Function.prototype.call // =>true
fn1.call.call.call.call.call(fn2)
fn1.call.call.call.call.call === Function.prototype.call  // =>true
```
总结: fn.call(thisArg, arg1,arg2,/..) 
Function.prototype.call的函数签名是fun.call(thisArg,arg1,arg2,...)
* 第一个参数为调用函数时this的指向
* 随后的参数则作为函数的参数调用,也就是fn(arg1,arg2)
*** 
例题: 

```JavaScript
let fn = function (a,b) {
    console.log(this,a,b)
}
let obj = {
    name : 'OBJ'
}
 fn.call(obj,10,20) 
 fn.call(10, 20)  // this 是10, 第三个参数是undefined
```
#### call中的细节: 
1. 非严格模式下,如果参数不传,或者第一个传递的是null/undefined this都指向window
2. 在严格模式下,第一个参数是谁,this就指向谁(包括null/undefined) 
``` JavaScript
fn.call(obj,10,20) // this : obj a = 10,b = 20
fn.call(10,20) // this : 10 , a = 20, b = undefined
fn.call()  // this : window a & b都 =undefined
fn.call()  // this: window  
```
## 5.2 apply() 
apply和call基本一样,在性能上次于call,唯一区别在于传参   
apply需要把传递给fn的参数放到一个数组(或类数组),虽然写的是一个数组,但是也是相当于给fn一个个传递  
## 5.3 bind() 
和call一样,但区别是bind等待执行(当点击的时候才执行),call是立即执行  
当点击的时候执行fn,让fn中的this是obj  
```JavaScript  
document.onclick = fn // this : document
document.onclick = fn.call(obj) // this 指向obj,这时候就把fn执行了,call是立即执行函数,点击执行
// 就等于是fn()(),此时就是fn的返回值了,undefined
document.onclick = fn.bind(obj) //bind属于把this进行预处理,此时fn没有执行,等点击的时候才会把fn执行
```
