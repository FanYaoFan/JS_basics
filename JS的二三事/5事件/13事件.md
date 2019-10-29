# 事件 
# 1 事件流 
事件流描述的是从页面中接收事件的顺序(事件冒泡流&事件捕获流)
 ##  1.1 事件冒泡 
 click事件沿DOM树向上传播,在每一个节点都会发生,直到传播到document对象  
 ##  1.2 事件捕获
 不太具体的节点应该更早的接收到事件,最具体的节点应该最后接收到事件,用意就是在事件到达预定目标之前捕获它
 ## 1.3 DOM事件流  
 事件流包括三个阶段:事件捕获阶段,处于目标阶段和事件冒泡阶段
 1. 事件捕获先发生,为截获事件提供了机会
 2. 实际的目标接收到事件  
 3. 冒泡阶段,在这个阶段对事件做出响应
 # 2 事件处理程序  
 什么是事件: 
 事件就是浏览器or用户自身执行的某种动作,像click\load/mouseover,都是事件的名字;响应某个事件的函数就叫做事件处理程序.事件处理程序以"on"开头(onclick/onload)  
 ```JavaScript  
<input type="button" value="Click Me" onclick="showMessage">
<script>
function showMessage(){
    alert('hello world')
}
</script>
 ```
 # 3 事件对象
 在触发DOM的某个事件时,会产生一个事件对象`event`,包含了所有与事件有关的信息,包括导致事件的元素,事件的类型,等其他信息  
 在事件属性方法中常用的尤currentTarget和target  
 1. currentTarget: 其事件处理程序当前正在处理事件的那个元素
 2. target: 事件的目标
 3. detail: 与事件相关的细节信息  
 4. type  : 被触发的事件类型

 在事件处理程序内部,对象this始终=currentTarget的值,而target则只包含事件的实际目标,如果直接将事件处理程序指定给了目标元素,则this的值=currentTarget的值=target的值  
 eg   
 ```JavaScript
var btn = document.getElementById("myBtn")
btn.onclick = function(event){
    console.log(event.currentTarget === this) // true
    console.log(event.target === this)   // true 
}
 ```  

 在处理一个函数多个事件时,可以使用type属性  
 eg  
  ```JavaScript  
var btn = document.getElementById("myBtn")
var handle = function(event){
    switch(event.type){
        case "click" : 
              alert("clicked")
              break
        case "mouseover" : 
              alert("mouseover")
              break
        case "mouseout" : 
              alert("mouseout")
              break
    }
}
btn.onclick = handler
btn.mouseover = handler
btn.mouseout = handler
  ```  
  要阻止特定事件的默认行为,比如(链接的默认行为就是在单击时会导航到其href特性指定的url),就可以用preventDefault()方法阻止  
# 4 事件类型  
1. UI(User Interface)用户界面事件,当用户与页面上的元素交互时触发
2. 焦点事件:当元素获得or失去焦点时
3. 鼠标事件: 用户通过鼠标在页面上执行某些操作时
4. 滚轮事件: 使用鼠标滚轮(类似设备)时
5. 合成事件: 当为IME(input Method Editor)输入字符时
6. 键盘事件: 通过键盘在页面上执行时
7. 变动(mutation)事件:当底层DOM结构发生变化时
## 4.1 UI事件
* load:页面完全加载后在window上面触发,当所有框架加载完毕时,or所有图像加载完毕时在`<img>`上触发,or当嵌入的内容加载完毕时在`<object>`上触发  
* unload:当页面完全卸载后在window上触发,与load卸载后
* select: 当用户选择文本框(`<input>` | `<textarea>`)
* resize: 当窗口或框架大小变化时
* scroll: 用户滚动条  
## 4.2 焦点事件  
1. blur: 失去焦点
2. focus: 获得焦点 
## 4.3 鼠标与滚轮事件 
1. click: 单击鼠标
2. dbclick: 双击鼠标(一般是左侧)
3. mousedown: 用户按下任意鼠标按钮
4. mouseenter: 光标从元素外部移入到内部 
5. mouseleave: 光标从元素移动元素外  
6. mousemove: 鼠标指针在元素内部移动是重复的触发
7. mouseover: 在鼠标指针位于一个元素外部,然后用户首次移入到另个元素边界之内时触发  
8. mouseup: 用户释放鼠标触发    

页面上的所有元素都支持鼠标事件,除了mouseenter & mouseleave,所有鼠标事件都会冒泡也可以被取消  
### 4.3.1 mouseenter和mouseover
1  
* over属于滑过覆盖事件,从父元素到子元素,会触发父元素的out,触发子元素的over  
* enter属于进入,从父元素进入子元素,并不算离开父元素,不会触发父元素的leave,而触发子元素的enter  
2 enter和leave阻止了事件的冒泡,而over和out还存在冒泡,实际开发中, 使用频率 enter > over

### 4.3.2 客户区坐标 
clientX&clientY:表示的是事件发生时,鼠标指针在视口的水平和垂直坐标,可以用下列代码取得鼠标事件的客户端坐标信息  
```JavaScript
var div = document.getElementById("myDiv")
EventUtil.addHandler(div,"click",function(e){
    e = EventUtil.getEvent(e)
    console.log("Client coordinates: " + e.clientX = "," + e.clientY )
})
```  
### 4.3.3 页面坐标位置  
页面没有滚动的时候: pageX 和 pageY 的值与clientX和clientY的值相等
## 4.4 键盘与文本事件  
1. keydown: 按下键盘任意键(若按住不放,重复触发该事件)
2. keypress: 按下键盘
3. keyup: 释放键盘
### 4.4.1 键码 
如图: keyCode
## 4.5 pageshow和pagehide事件 
往返缓存(back-forward cache or bfcatch),可以在用户使用浏览器的"后退"&"前进"按钮时加快页面的转换速度.这个缓存中不仅保存着页面数据,还保存了DOM和JavaSript的状态;实际就是把整个页面都保存在了内存里.
pageshow在页面显示时触发,pageshow会在load事件之后触发.
虽然这个事件的目标是document,但必须将其事件处理程序添加到window中.  
```JavaScript  
(function () {
    var showCount = 0 
    EventUtil.addHandle(window,"load", function() {
        alert("Load fired")
    })
    EventUtil.addHandle(window, "pageshow", function(){
        showCount++
        alert("Show has been fired" + showCount + "times")
    })
})()
```
这个事件使用了私有作用域,防止变量showCount进入全局作用域,当页面首次加载完成时,showCount = 0,伺候,每当触发pageshow事件,showCount的值就会递增并通过警告框显示出来.当你返回这个页面,他们的状态又得到了恢复
## 4.6 触摸事件  
1. touches: 当前跟踪的触摸操作的Touch数组 
2. targettouches: 特定于事件目标的Touch对象的数组
3. changeTouches: 自上次触摸以来发生了什么改变的Touch对象的数组,每个Touch对象包含以下对象  
* clientX/Y:触摸目标在视口中x/y坐标  
* identifier: 触摸的唯一ID
* pageX?Y: 触摸目标在页面中的x/y坐标
* screenX/Y: 触摸目标在屏幕中x/y坐标
* target:触摸的DOM节点  

移动端手指事件  
1. touchstart:手指按下
2. touchmove:手指移动
3. touchend:手指离开
4. touchcancel:因为意外情况导致手指操作取消  

[gesture: 多手指操作]  

1. gesturestart: 多手指按下
2. gesturechange:手指改变
3. gestureend:手指离开

eg    
```JavaScript
function handleTouchEvent(event){
    //只跟踪一次触摸  
    if(event.touches.length == 1){
        var output = document.getElementById('output')
        switch(event,type){
            case 'touchstart':
                 output.innerHTML = "Touch started ( " + event.touches[0].cliendX + ',' + event.touches[0].cliendY + ")" 
                 break
            case 'touchend' : 
                    output.innerHTML = "<br>Touch ended( "+ event.changedTouches[0].cliendX = ',' + event.changedTouches[0].cliendY")"
                    break
            case 'touchmove' : 
            event.preventDefault() // 阻止滚动  
             output.innerHTML = "<br>Touch ended( "+ event.changedTouches[0].cliendX = ',' + event.changedTouches[0].cliendY")"
                    break
        }
    }
}
EventUtil.addHandle(document, "touchstart", handleTouchEvent)
EventUtil.addHandle(document, "touchend", handleTouchEvent)
EventUtil.addHandle(document, "touchmove", handleTouchEvent)
```

以上代码会跟踪屏幕发生的一次 触摸操作.当touchdstart事件发生时,会将触摸的信息输出到div元素中,当touchmove事件发生时,而touched事件则会输出有关触摸操作的最终信息.在touchend事件发生时,touches集合中就没有任何touch对象了,此时,就必须转而使用changeTouches集合  
# 事件委托
1. 绑定的事件越多,浏览器内存占用越多,严重影响性能 
2. ajax的出现,局部刷新的盛行,导致每次加载完,都要重新绑定事件,刷新时,显示数据不同,但操作大都相同,重复绑定,代码的耦合性过大,影响后期维护
3. 浏览器移除元素时,绑定的事件没有被及时移除,导致内存泄漏      

使用事件委托可以解决   
利用事件的冒泡机制,若一个容器内的后台元素中,很多元素的点击行为都要做一些处理,这里不需要在像以前一个个绑定,直接给父级的容器元素绑定,无论点击后代哪一个,都会触发,把对应的方法执行,根据事件源,我们知道点击的是谁,从而实现做不同的事情