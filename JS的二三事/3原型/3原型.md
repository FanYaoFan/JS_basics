# 原型与原型链
# 1 原型
原型(`prototype`)  
原型链(`__proto__`)  
#### 函数   
普通函数,类(所有的类:内置类;自己创建的类)  
#### 对象
普通对象,数组,正则,Math,Arguments 
实例是对象类型的(除了基本类型的字面量创建的值)
prototype的值也是对象类型的  
函数也是对象类型
## 1.1 原型链(重点)  
#### 1. 所有的函数数据类型都天生自带一个属性:`prototype`(原型),这个属性的值是一个对象,浏览器会默认给它开辟出一个堆内存
#### 2. 在浏览器给prototype开辟的堆内存中有一个天生自带的属性:`constructor`,这个属性的值是当前函数本身 
#### 3.每一个对象都有一个`__proto__`,这个属性指向当前实例所属的类的`prototype`(如果不能确定它是谁的实例,就都是Object的实例)
###  如图  
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/PrototypeImg/prototype.png"></img>
***
凡是遇到该问题,要画图分析  
eg   
如图:    
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/PrototypeImg/ProtoEg1.png"></img>  
*** 
# 2 函数类型与对象类型 
## 2.1 函数类型 
1. 普通函数
2. 构造函数 
## 2.2 对象类型
1. 普通对象
2. Math/Json
3. 类的实例(数组/正则/日期)
4. prototype 或者 `__proto__` 
5. arguments 或者元素集合等类数组 
6. 函数也是一种对象

#####   每一个函数(类)都有一个prototype(原型属性),属性值是一个对象存储了当前类供实例调用的公有属性和方法 
##### 私有属性 自己堆内存中存储的属性相对自己来说就是私有的
##### 公有属性 自己基于__proto__找到的属性,相对自己来说就是公有的
#### [案例code](https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/4prototype.html)

案例 
```JavaScript
    function Fn() {
        this.x = 100;
        this.y = 200;
        this.getX = function () {
            console.log(this.x);
        }
    }
    Fn.prototype.getX = function () {
        console.log(this.x);

    };
    Fn.prototype.getY = function () {
        console.log(this.y);
    }
    var f1 = new Fn;
    var f2 = new Fn;

    console.log(f1.getX === f2.getX);
    console.log(f1.getY === f2.getY);
    console.log(f1.__proto__.getY === Fn.prototype.getY);
    console.log(f1.__proto__.getX === Fn.prototype.getX);
    console.log(f1.__proto__.getX === f2.getX)
    console.log(f1.getX === Fn.prototype.getX);
    console.log(Fn.constructor);
    console.log(Fn.prototype.__proto__.constructor)
    f1.getX();
    f1.__proto__.getX();
    f2.getY();
    Fn.prototype.getY();
// 解析看图
```
# <img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/PrototypeImg/protoEg2.png"></img>   
## 3 原型重定向 
自己开辟的堆内存不知道是谁的实例就指向object.原型(`prototype`).浏览器默认开辟的堆内存会在空闲时期把不占用的释放掉.
1. 自己开辟的堆内存没有`constructor`的这个属性,导致类的原型构造函数缺失.  
解决:自己手动在堆内存中增加constructor属性.
2. 当原型重定向后,浏览器开辟的那个原型堆内存会被释放掉,如果之前已经存储了一些方法或者属性,会丢失.(所以:内置类的原型不允许重定向到自己开辟的堆内存,因为内置类原型上自带很多属性方法,重定向后都没了)
3. 使用场景: 就是给自己的原型(批量)添加方法 

当我们需要给类的原型批量设置属性和方法的时候,一般都是让原型重定向到自己创建的对象中 

```JavaScript
function fun() {
    this.a = 0
    this.b = function () {
        console.log(a)
    }
}
fun.prototype = {
    b : function () {
        this.a = 20
        console.log(this.a)
    }
    c : function () {
        this.a = 30
        console.log(this.a)
    }
}
var my_fun  = new fun()
my_fun.b()
my_fun.c()
```  
解析: 
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/PrototypeImg/protoEg3.png"></img> 
***   

类似数学的集合问题,大的`__proto__`包含着小的`__proto__`;小的`__proto__`指向这个小的自己实例所在的类,而这个类可能又是别的大类的一个实例  
```JavaScript
function Fn() {
    var n = 10
    this.m = 20
    this.aa = function () {
        console.log(this.m)
    }
}
Fn.prototype.bb = function () {
    console.log (this.n)
}
var f1 = new Fn 
Fn.prototype = {
    aa : function () {
        console.log( this.m + 10)
    }
}
var f2 = new Fn 
console.log(f1.constructor)
console.log(f2.constructor)
f1.bb()
f1.aa()
f2.bb()
f2.aa()
f2.__proto__.aa()
```
解析:
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/PrototypeImg/protoEg4.png"></img> 
*** 
阿里巴巴面试原型题
```JavaScript
getName = function Foo() {
        console.log(1);
        return this;
    }
    Foo.getName = function () {
        console.log(2);
    }
    Foo.prototype.getName = function () {
        console.log(3);
    }
    var getName = function () {
        console.log(4);
    }
    function getName() {
        console.log(5);
    }  //普通函数定义，使得一开始getName为5
    Foo.getName();  
    getName();         
    Foo().getName();    
    new Foo.getName();
    new Foo().getName();
    new new Foo().getName();
```  
<img src="https://github.com/FanYaoFan/JS_basics/blob/master/JS%E7%9A%84%E4%BA%8C%E4%B8%89%E4%BA%8B/3%E5%8E%9F%E5%9E%8B/PrototypeImg/protoEg5.png"></img> 
