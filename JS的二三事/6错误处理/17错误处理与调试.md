# 1 错误处理与调试
## 1.1 try catch 
### 1.1.1 基本语法
 ```JavaScript
 try {可能出错的代码
 }catch(err){
    console.log(err) //出错执行的代码
 }
 ```
### 1.1.2 finally 子句
无论是try语句中正常还是异常要执行catch语句,finally最终都会执行  
eg:
 ```JavaScript  
function testFinally(){
    try{
        return 1
    }catch(err){
        return err
    }finally{
        return 0
    }
}
//没有finally时,会输出1,但这里输出的是0
```  
## 1.2 错误类型  
### 1.2.1   
* Error 
* EvalError
* RangeError
* ReferenceError  
* SyntaxError
* TypeError
* URIError  
1. Error是基本类型,其他错误类型都继承该类型,很少见!  
2. EvalError 类型的错误会使用eval()函数而发生异常时会被抛出.如果没有把eval()当做函数来调用,就会抛出这个异常.  
eg 
```JavaScript   
new eval() or eval = foo  // 都会抛出这个异常
```  
3. RangeError类型的错误会在数值超出相应范围时触发,比如,在定义数组时,如果指定了数组不支持的项数,就会抛出这个异常.
eg 
``` JavaScript 
var items1 = new Array(-20)  
var items2 = new Array(Number.Max_VALUE)
// 以上都会抛出这个异常
```
4. ReferenceError是经常遇到的错误,一般是在找不到对象的情况下,(通常,在访问不存在的变量时,就会发生这种错误)会直接导致(`object expected`浏览器错误) 
```JavaScript
var obj = x  // 在x未声明的情况下就会抛出ReferenceError
```
5. TypeError,一般都跟变量有关,or保存时,or访问变量时,最常发生类型错误的情况,就是传递给函数的__参数__事先没有经过检查,结果传入类型与预期不相符
6. encodeURI()或decodeURI(),而URI格式不正确就会抛出这个错误
eg 
``` JavaScript 
var o = new 10 
alert('name in  true')
Function.protype.toString.call('name')
// 以上都会输出TypeError
```
利用以上的不同的错误类型,可以知其错从而有头绪的解决错误,具体可以如下操作  
```JavaScript  
try{
    somefunction()
}catch(err){
    if(err instanceof TypeError){
        //处理类型错误
    }else if(err instanceof ReferenceError){
        //处理引用类型错误
    }else{
        // 处理其他错误
    }
}
```
__使用try-catch场景__  
当try-catch语句发生错误时,浏览器会认为错误已经处理了,最适合处理我们无法控制的错误,比如把await全部包裹起来.如果自己知道/清楚哪里会出错就无须用try-catch语句了.  
### 1.2.3 抛出错误  
1 throw,可用于随时抛出自定义错误
 eg
```JavaScript  
throw 12345  
throw "Hello World"
throw true  
throw {name : 'JavaScript'}  
```  
在遇到throw时,代码会立即执行(可以运用到手写Promise,模仿Promise抛出异常)当且仅当try-catch语句捕获到被抛出的值时,代码才会继续进行  
通过使用某种内置错误类型,可以更真实的模拟浏览器错误  
eg  
```JavaScript
throw new Error("Something error")  
throw new SyntaxError("I don't like your syntax!!!")
throw new ReferenceError("can not read property of your")  
```
常见的自定义错误类型: Error RangeError Refenrence TypeError  
利用原理链可以通过继承Error的方式来创建自定义错误类型  
```JavaScript
function CustomError(message){
    this.name = "CustomError"
    this.message = message
}
CustomError.protype = new Error()
throw new CustomError("My message")
```
2 抛出错误的时机  
浏览器会在某种特定的条件下执行函数时抛出error eg
下列函数会在传入的函数不是数组时抛出错误(如何解决) 
```JavaScript
function process (value){
  value.sort() // 数组的方法  
  for (var i = 0, i < value.length; i++){
      if(value[i] > 100){
          return value[i]
      }
  }
  return -1
}
// 如果执行这个函数,并给他传入一个字符串参数,在value.sort就会报错,但个别浏览器报错都不是很明确
// 可以改写成  
function process (value){
    if(value instanceof Array){
        throw new Error(\"process(): Argument must be array!")
    }
  value.sort() // 数组的方法  
  for (var i = 0, i < value.length; i++){
      if(value[i] > 100){
          return value[i]
      }
  }
  return -1
}   
```
try-catch & throw  
应该捕获那些你知道如何解决的错误,捕获错误的目的是在于浏览器以默认的方式处理他们;而抛出错误的目的在于提供错误发生具体原因的消息.
# 2 处理错误的策略 
核心: 要知道代码里会发生什么错误(废话);因为js是松散类型的,而且也不会验证函数的参数,因此错误只会在代码运行的期间出现.主要关注三个错误:
1. 类型转换错误
2. 数据类型错误
3. 通信错误  
## 2.1 类型转换错误
通常发生在使用操作符时,比如==,!=orif,for以及while等流控制语句使用非布尔值,最容易发生类型转换  
最好使用(===)和(!==)以避免类型转换(要知道==和===是怎么回事)
在流控制语句中 
eg  
```JavaScript
function concat(str1,str2,str3){
    var result = str1 + str2 
    if(str3){    //don't do that
        result += str3
    }
    return result
}
// 这个函数实现的是拼接字符串,然后返回结果,其中,str3是可选的参数,但如果为传参,其值不是没有,是undefined(这就引申出一个哲学问题 0 是不是没有?)
// undefined的值可以被转换成false 0也意味着false  
function concat(str1,str2,str3){
   var resulr = str1 + str2 
    if (typeof str3 == 'string'){
        result += str3
    }
    return result
}
```
## 2.2 数据类型错误 
indexOf() 方法返回调用它的 String 对象中第一次出现的指定值的索引，从 fromIndex 处进行搜索。如果未找到该值，则返回 -1  
eg  
```JavaScript
function getQueryString(url){
    var pos = url.indexOf("?")
    if(pos > -1 ){
        return url.substring(pos + 1)
    }
    return ""
}
/**实现的是返回给定url中的查询字符串.
1. indexOf()寻找字符串中的?,如果找到,就用substring()返回问号后面的所有字符串,这个例子中的两个函数只能操作字符串,因此只要传入其他数据类型就会导致错误,而添加一条简单的类型数据语句,就可以确保函数不那么容易错误*/
function getQueryString(){
    if(typeof url == "string"){
        // 通过检查类型确保安全
    var pos = url.indexOf("?")
    if(pos > -1 ){
        return url.substring(pos + 1)
    }
    return ""
    }
}
/** 首先检查这个函数传入的值是不是字符串,以避免因为接受到非字符串而导致的错误*/
```
同样在流控制语句中也是.在编写函数时,最好考虑多一点,也就是说你不知道用户或者其他的调用者会传什么参数.
在确切的知道应该传入什么类型,最好用instanceof()方法来检测其数据类型 
instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上(MDN)
```JavaScript
if(value instanceof Array)
// instanceof()方法的使用  
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
var auto = new Car('Honda', 'Accord', 1998);

console.log(auto instanceof Car);
// expected output: true

console.log(auto instanceof Object);
// expected output: true
```
## 2.3 通信错误
1. 与ajax兴起有关,可能url格式不正确.最常见的就是在将数据发送到服务器之前,没有使用encodeURIComponent()对数据进行编码
eg 
```JavaScript
`http:www.xxx.com/?redir=http://www.somexxx.com?a=b&c=d  `
//针对"redir="后面的所有字符串调用encodeURIComponent()就可以解决这个问题,对于查询字符串,应该记住必须要使用encodeURIComponent()方法  
function addQueryStringArg(url,name,value){
if (url.indexOF("?") == -1){
    url += "?"
}else{
    url += "&"
}
url += encodeURIComponent(name) + "=" + encodeURIComponent(value)
return url
}
```
这个函数接收三个参数:要追加查询字符串的URL,参数名和参数值.如果传入的URL不包含问号,就添加问号,否则,就添加&
## 3 记录错误 
有必要的把错误记录到服务器,标明这些错误来自前端,记录到服务器中.
需要在服务器中创建一个页面(或者一个服务器入口点),用于处理错误数据.这个页面的作用无非就是从查询字符串中获得数据,然后再将这个数据写入到错误日志中. 
```JavaScript
function logError(sev,msg){
    var img = new Image()
    img.src = "log.php?sev=" + encodeURIComponet(sev) + "&msg" + encodeURIComponent
}
/**这个logError()函数接收两个参数:表示严重程度的数组or字符串和错误信息,其中使用Image()对象来发送请求()*/
```
为什么要用Image()这个方法来发送请求呢?  
1. 所有浏览器都支持Image对象,包括那些不支持XMLHttpRequest对象的浏览器 
2. 可以避免跨域限制.通常都是一台服务器要负责处理多台服务器的错误,而这种情况下使用XMLHttpRequest是不可行的
3. 在记录错误的过程中出问题的概率低,大都数都不会用原生js来发送请求了,一般都是用axios,fetch等封装好的来处理请求
只要是使用try-catch语句的就应该把相应的错误记录到日志中
```JavaScript
for(var i = 0; i<mods.length; i++){
    try{
        mods[i].init()
    }catch(err){
        logError('nonfatal', "module init failed: " + err.message)
    }
}
```
# 3 错误集合
## 3.1 无效字符
`invalid charactor`无效字符,js文件只包含特定的字符,就是js语法中未定义的字符
## 3.2 未找到成员  
如果对象在被销毁之后,又给他赋值,就会出这个错误,最常见的使用event对象的时候,IE中的event对象是全局属性(window),该对象在事件发生时创建,在最后一个事件处理程序执行完毕后销毁.如果你在闭包里用了event对象,该闭包又不会立即执行,那么在将来调用并给它event的属性赋值时,就会导致未找到成员错误
eg  
```JavaScript
document.onclick = function(){
    var event = window.event
    setTimeout( function () {
        event.returnValue = false //未找到成员错误
    },1000)
}
```
将一个单击事件处理程序指定给了文档,在事件处理中,window.event被保存在event变量中,传入setTimeout()中的闭包里又包含了event变量.当单机事件完成,event对象就会被销毁,因此闭包里的引用对象就成了不存在了=>event.returnValue赋值就会导致未找到成员错误
## 3.3 未知运行时错误  
innerHTML | outerHTML 以下列方式指定html时,就会发生未知运行时错误(Unknow runtime error): 
1. 把块元素插入到行内元素时
2. 访问表格任意部分(<table><tbody>)的任意属性时.  
技术角度上:比如<span>中不能包含div元素
eg 
```JavaScript
span.innerHTML = "<div>Hi</div>: //这里会报错
```
## 3.4 语法错误  
1. maybe () or {} or ; 等缺少从而报错 syntax error 
2. 如果引用了外部js文件,而该文件没有返回jsdiam.同样会报错
## 3.5 系统无法找到指定资源
1. 一般在引用文件/css文件/图片资源/或者es6中的require('路径')因为路径或者别的问题,从而报错 (The system cannot locate the resource specified)
2. 在使用js请求某个资源(axios/ajax)url,而该url长度超过了IE(是浏览器而定)<2083个字符限制时就会报错 
