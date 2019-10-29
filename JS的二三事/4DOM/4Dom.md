# DOM 
DOM (document object model) 是针对HTML和 XML文档一个API(应用程序编程接口) 
# 1 节点层次
文档节点是每个文档的根节点,文档节点只有一个子节点(html元素) => 文档元素  
 文档中的所有元素都包含在文档元素中
 ## 1.1 node类型
 每个节点都拥有各自的属性(attribute),数据和方法;每个节点都有一个nodeType属性
 1. Node.element_node(1)
 2. Node.attribute_node(2)
 3. Node.text_node(3)
 4. Node.cdata_section_node(4)
 5. Node.entity_refenrence_node(5)
 6. Node.entity_node(6)
 7. Node.processing_instruction_node(7)
 8. Node.comment_node(8)
 9. Node.document_node(9)
 10. Node.document_tyoe_node(10)
 11. Node.document.fragment_node(11)
 12. Node.notation.node(12)
## 1.2 节点关系  
每个节点都有一个childNodes属性,其中保存着一个nodeList对象,是一个类数组对象,用于保存一组有序的节点.虽然它也有length属性,也可以通过[]的形式来访问,但它不是数组的一个实例对象
```JavaScript
var firstChild = someNode.childNodes[0]
var secondChild = someNode.chileNodes[1]
var count = someNode.childNodes.length
``` 
length属性的访问是访问nodelist那一刻,其中包括节点数量   
对arguments对象使用Array.prototype.slice()方法可以将其转换为数组  
```JavaScript
function converToArray(nodes){
    var array = null 
    try{
        array = array.prototype.slice.call(nodes,0) // 针对非IE浏览器
    }catch(err){
        array = new array()
        for (var i = 0 ; i < arr.length; i++){
            array.push(nodes[i])
        }
        return array
    }
}
```
首先尝试创建数组的最简单方式,如果报错,则听过catch来捕获错误,然后手动来创建数组.
### 1.2.1  
如图: NodeRelationship 
可以通过previousSibling和nextSibling属性访问统一列表中的其他节点.(注意:在列表中不止一个节点时,列表中的第一个节点的previousSibling属性值为null,同样的最后一个节点的nextSibling属性也为null;当只有一个节点时,都为null,且firstChild和lastChild都为null)  
1. hasChildNodes()在节点包含一个或多个的情况下,返回true
2. 所有节点都有最后一个属性是ownerDocument,都指向整个文档的文档节点
### 1.2.2 操作节点 
1. 插入节点  
appendChild(新节点)
```javascript
var returnNode = someNode.appendChild(newNode) 
alert (returnNode == newNode)  // true
alert (someNode.lastChild == newNode ) // true
```
意指,在当前列表子节点中的末尾添加一个节点.每个关系指针都会更新 如果要在前面插入一个节点呢?  
insertBefore(要插入的节点,作为参照的节点)在参照节点前插入  
2. 替换节点
replaceChild(新节点,要被替换的节点)
3. 移除节点 
removeChild() 
知道父级节点时: 父级节点.removeChild(子节点)
在不知道父级节点时,node.parentNode.removeChild(node)
4. 复制节点  
oldnode.cloneNode(true) // 执行深复制  
oldnode.cloneNode(false)//执行浅复制  
# 2 Document  
Js通过Document类型表示文档,document对象时HTMLDocument(继承自Document类型的一个实例),表示整个HTML文档.
## 2.1 获得元素
### 2.1.1 getElementById()
接收一个参数:要取得该元素的ID,找到相应元素就返回该元素,找不到就返回null (与页面的id(attribute)一致,包括大小写)  
### 2.1.2 getElementByTagName() 
接收标签名,返回的是多个元素的nodeList
### 2.1.3 getElementClassName()
为某些类的元素添加事件处理程序,或者更改样式等  
在操作类名的时候,,需要通过className属性添加,删除和替换类名  因为className是一个字符串,每次操作都要设置整个字符串  
eg 在有三个类名中删除一个user类名  
```javascript 
<div class="bd user disabled">*</div>
// 1 取得类名字符串并拆分成数组  
var className = div.className.split(/\s+/)
// 2 找到要删除的类名      
var pos = -1 ,i ,len; 
for(i = 0, len = className.length; i<len>){
    if(className[i] = "user"){
        pos = i
        break
    }
}
// 3 删除类名
className.splice(i,1)
// 4 把剩下的类名拼成字符串并重新设置 
div.className = className.join(" ")
```
其他方法: 
1. add: 将给定的字符串值添加到列表中
2. contains 表示列表中是否存在给定的值,存在就返回true,否则就返回false
3. remove 删除
4. toggle 若列表中已经存在给定的值,删除它;若列表,没有就添加

## 2.2 取得特性 
每个元素都有一个或多个特性,操作特性的DOM方法有三个
1. getAttribute() //取得特性
2. setAttribute() // 设置特性
3. removeAttribute() // 移除特性  
```JavaScript 
<div id="myDiv" myattribute="hello!"></div>
// 取得特性:
var value = div.getAttribute('myattribute')
// 自定义特性应该+data-前缀以便验证
```
## 2.3 创建元素  
document.createElement()参数=要创建元素的标签名
还可以 div.id = "myDiv" | div.className =" box"   
eg  
```javascript
if(client.browser.ie && client.browser.id <=7>){
    var iframe = document.createElement("<iframe name =\"myframe"\></iframe>")
}
// 创建input元素
var input = document.createElement("<input type=\"checkbox"\")
```
## 2.4 元素的子节点
nodetype =1 元素节点(div等) 
nodetype =2 属性节点(src等)
nodetype =3 文本节点(item)
通过nodeType可以判断节点类型
eg 
```javascript
<ul  id="mylist">
   <li>item1</li>
   <li>item2</li>
   <li>item3</li>
</ul>
```
ul元素会有3个子节点=>3个li元素,有4个文本节点(item*)
如果需要通过childNodes属性遍历子节点,那么一定要检查nodeType属性,个别浏览器不一致 
```javascript
for (var i = 0;i < element.childNodes.length; i++){
    if(element.childNodes[i].nodeType == 1)
    // 执行某些操作
}
```
## 2.5 操作表格
使用DOM的方式来创建HTML表格  
```javascript
HTML样式:
<table border="1" width="100%">
    <tbody>
      <tr>
          <td>1</td>
          <td>2</td>
      </tr>
      <tr>
        <td>3</td>
        <td>4</td>
      </tr>
    </tbody>
</table>
// 1  创建table 
var table = document.createElement("table")
table.border = 1
table.width = "100%"
// 2 创建tbody 
var tbody = document.createElement("tbody")
table.appendChild(tbody)  
// 3 创建第一行
// 创建第一行的第一个tr元素
var tr1 = doucument.createElement("tr")
// 要注意列表子节点的关系
tbody.appendChild(tr1)
var td1 = doucument.createElement("td")
// 在td后面添加文本内容,所以用appendChild()
td1.appendChild(document.createTextNode("1"))
tr1.appendChild("td")
var td2 = document.createElement("td")
td2.appendChild(document.createTextNode("2"))
tr1.appendChile("td")
// 创建第二行  
var tr2 = document.createElement("tr")
tbody.appendChild(tr2)
var td3 = document.createElement("td")
td3.appendChild(document.createTextNode("3"))
tr2.appendChild("td") // 是否这样写就保证了td整个在tr里面了
var td4 = decument.createElement("td")
td4.appendChild(document.createTextNode("4"))
tr2.appendChild("td") 
// 保证了td添加到了tr内部最后一个元素 因为tr是td的父节点
// 父节点.appendChild(子节点)
```
## 2.6 使用NodeList  
理解NodeList和NameNodeMap和HTMLCollection,这三个集合都是从__动态的__   
所有Nodelist对象都是在访问DOM文档时实时运行的查询 
无限循环的代码  
```javascript 
var divs = document.getElementByTagName("div"), i ,div
for (i=0,i<divs.length;i++){
    div = document.createElement("div")
    document.body.appendChile(div)
}
```
第一行代码会取得所有的<div>元素的HTMLCollection,由于这个集合是动态的,因此,只要有新的<div>元素添加到页面中,这个元素也会被添加到这个集合中.因为for循环每次都要访问divs.length,divs.length的值都会递增  
所以想要迭代一个nodeList,最好使用 `len = divs.length`  
然后i<len. 为什么要这么做?   
因为len保存着对divs.length在循环开始时的一个快照,因此会避免出现上述的无效循环问题
```javascript 
var divs = document.getElementByTagName("div"), i ,div,len
for (i = 0,len = divs.length; i < len; i ++){
    div = document.createElement("div")
    document.body.appendChile(div)
}
```
# 3 DOM扩展
## 3.1 选择API
Jquery核心就是通过CSS选择符查询DOM文档取得元素的引用,从而抛弃了getElementById()和getElementTagName()  
### 3.1.1 querySelect() 
接收一个css选择符,返回与该模式匹配的第一个元素,如果没有,就返回null
```javascript
//取得body元素
var body = document.querySelector("body")
// 取得id为myDiv元素
var myDiv = document.querySelector("#myDiv")
// 取得类为"button"的第一个图像元素
var  img = document.body.querySelector("img.button")
```
document.querySelector()是从整个文档查找;document.body.querySelector()是从其后代查找
### 3.1.2 querySelectorAll() 
接收参数与querySelector一致,但返回的是所有匹配的元素(NodeList)
```JavaScript
// 取得myDiv中所有的em元素 
var ems = document.getElementById("myDiv").querySelectorAll("em")
// 取得类为selected的所有元素
var selecteds = document.querySelectorAll(".Selected")
// 取得所有p元素下的所有strong元素 
var strongs = document.querySelector("p strong")
```
## 3.2 焦点管理 
document.activeElement()  
元素获得焦点的方式有页面加载,用户输入和在代码中调用focus()方法
```javascript
var button = document.getElementById("myButton")
button.focus()
alert(document.activeElement === button) // true
```
document.hasFocus() 用于确定文档是否获得了焦点  
# 4 滚动
## 4.1 
* 1 scrollIntoView()
滚动页面,可以在所有的HTML元素上调用,通过滚动浏览器窗口或多个容器元素,调用的元素就出现在视口中.
传入true or 不传参=>窗口滚动会让调用的元素的顶部与视口顶部尽可能平齐  
 传入false => 调用元素尽可能全部出现在视口中
* 2 scrollIntoViewNeeded(aligncenter):只在当前元素不可见时,才滚动浏览器窗口或容器元素;若可见,则do nothing; 若aligncenter = true, 表示,尽量将元素显示在视口中部(垂直方向)
* 3 scrollByLines(lineCount): 元素的内容滚动指定的行高,lineCount值可正可负
* 4 scrollByPages(pageCount): 将元素的内容滚动指定的页面高度,具体高度由元素的高度决定
# 5 DOM2和DOM3
## 5.1 访问元素的样式  
CSS的样式在js中用短划线分隔不同的词汇,且必须转换成驼峰大小形式  
常见的CSS属性和style对象中对应的属性名  
<table width="100%" style="text-align:center;">
   <tbody>
     <tr>
       <th>CSS属性</th>
       <th>JS属性</th>
     </tr>
    <tr>
      <td>backgroud-image</td>
      <td>style.backgroudImage</td>
    </tr>
    <tr>
       <td>color</td>
       <td>style.color</td>
    </tr> 
    <tr>
       <td>display</td>
       <td>style.display</td>
    </tr>
    <tr>
       <td>font-family</td>
       <td>style.fontFamily</td>
    </tr>
   </tbody>
</table>    
多数情况下,都可以通过简单的转换属性名的格式来实现转换
不能直接转换的CSS属性就是float  

```JavaScript
var myDiv = document.getElementById("myDiv")
// 设置背景颜色  
myDiv.style.backgroundColor = "red"
// 设置高度
myDiv.style.height = "200px"
```
# 6 盒子模型  
client/offset/scroll(top/left/width/height)  
offsetParent  
## 6.1 client
client top/left/width/height  
clientWidth&clientHeight  
获取当前元素可视区的宽高(内容宽高+左右+上下的padding)和内容是否溢出无关,就是我们自己设置的宽高+padding  
兼容写法: 
```JavaScript
document.documentElement.clientWidth || document.body.clientWidth
```
## 6.2 offerset  
top/left/width/height/parent
在client的基础上加上border,和内容溢出没有关系; 如图 
# client.png  
offersetParent:当前盒子的父级参照物 
同一平面,元素的父级参照物个结构没有必然关系,默认他们的父级参照物都是body,参照物可以改变,构建出不同的平面即可(比如定位)
```JavaScript 
father.offsetParent     // =>body
son.offsetParent        // =>body
grandson.offsetParent   // =>body
```
offsetTop/left:获取从当前盒子距离其父级参照物的偏移量(上/左)当前盒子的外边框开始=>父级参照物的内边框(body)
## 6.3 scroll 
top/left/width/height
scrollWidth/Height : 真实内容的宽高(需要考虑溢出)+padding
scrollTop/left:滚动条卷去的宽度或者高度  
最小值: 0  
最大值: 真实页面的高度 - 屏幕的高度

