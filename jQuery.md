# 第一章 初识jQuery

## 一、jQuery简介

### 1.什么是jQuery？

jQuery是一个优秀的JavaScript库，是一个凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档，操作DOM，执行动画和开发Ajax的操作。jQuery封装了很多预定义的对象和函数。其理念：write less,do more.

### 2.常见的javascript库?

>  Prototype:是最早成型的JS库之一，对于JS的内置对象做了大量的扩展。
>       Do jo：提供了很多奇特JS库没有提供的功能。例如：离线存储的API，生成图标的组件等等。
>       YUI：是由Yahoo公司开发的一套完备的，扩展性良好的富交互网页程序工作集。
>       Ext JS：原本是对YUI的一个扩展，主要用于创建前端用户界面。
>       Moo Tools：是一套轻量、简洁、模拟化和面向对象的JS框架。
>       jQuery：同样是一个轻量级的库，拥有强大的选择器等更多优点，吸引了更多开发者去学习使用它。

## **二、jQuery的特性**

*jQuery能做以下事情：*

> ​    HTML元素选取
>
> ​    HTML元素操作
>
> ​    CSS操作 
>
> ​    HTML事件函数   
>
> ​    JavaScript特效和动画
>
> ​    HTML DOM遍历和修改  
>
> ​    AJAX  
>
> ​    Utilities

## **三、jQuery介绍**

### ***1.jQuery的使用方式***

下载后引入

```html
 <script src="jquery-3.3.1/jquery-3.3.1.min.js"></script>

```

### ***2.编写简单的 HelloWorld***

```html
   <html>
       <head>
       <meta charset="UTF-8">
       <title>dom操作</title>
       <script src="jquery-3.3.1/jquery-3.3.1.min.js"></script>
       <script>
          $(function(){
             alert('Hello World');
          });
       </script>
       </head>
    </html>

```

### ***3.jQuery函数***

jQuery库只提供了一个叫jQuery的函数，该函数中以及该元素的原型中定义了大量的方法。jQuery函数具有四种参数：

>       1）选择器（字符串）
>          jQuery函数通过该选择器获取对应的DOM，然后将这些DOM封装到一个人jQuery对象中并返回。
>          
>       2）DOM对象（即Node实例）
>           jQuery函数将该DOM封装成jQuery对象并返回。
>           
>       3）HTML文本字符串
>           jQuery函数根据传入的文本创建好HTML元素并封装成jQuery对象并返回。
>            $("<div class="one">one</div>");
>     
>       4）一个匿名函数    
>           $(function(){});
>           当文当加载完毕之后jQuery函数调用匿名函数。

### ***4.jQuery对象***

>   jQuery对象是jQuery函数的一个实例，是一个类数组对象，数组中存放的是DOM对象，而DOM对象是Node的实例。
>
>   对jQuery对象的操作实际上是对jQuery数组中的DOM对象的批量操作。jQuery对象和DOM对象可以相互转化。
>
>   jQuery对象的获取通常是使用选择器来获取的。比如，获取所有clss为one元素:$(".one");

## **四、jQuery选择器**

### ***1.基本选择器：***

>    所有选择器  *
>    标签选择器  标签名
>    ID选择器    #id
>    类选择器    .className
>    群组选择器  .one,.two   多个选择器使用都好分隔，取并集
>    复合选择器  .one.two   多个选择器组合使用，取交集

### ***2.层次选择器：***

>    后代选择器   .one .two   
>       两个选择器使用空格隔开，表示可以获取当前元素的子代以及孙子代等等后代元素。
>    子代选择器   .one>.two
>       两个选择器使用>隔开，表示只能获取当前选中元素的子代元素。

### ***3.兄弟选择器：***

>    下一个兄弟选择器   .one+.two
>       两个选择器使用+隔开，表示可以获取当前元素的下一个兄弟元素，下一个兄弟元素要能符合.two。
>    之后所有子代选择器  .one~.two
>       两个选择器使用~隔开，表示可以获取当前元素之后的所有兄弟元素，只有所有兄弟元素要能符合.two。

## **五、jQuery过滤器**

jQuery的过滤器必须用在jQuery选择器后，表示对通过前面的jQuery选择器选择到的内容的过滤。是建立在前面选择器已经选择到的元素的基础之上。 语法：selector:过滤器

### ***1.基本过滤器：***

```properties
  selector:first  获取所有已选择到的元素中的第一个元素
  selector:last   获取所有已选择到的元素中的最后一个元素
  selector:even    获取所有已选择到的元素中的索引为偶数的元素
  selector:odd     获取所有已选择到的元素中的索引为奇数的元素
  selector:eq(index) 获取所有已选择到的元素中的索引为index的元素
  selector:lt(num)   获取所有已选择到的元素中的索引值小于num的元素
  selector:gt(num)   获取所有已选择到的元素中的索引值大于num的元素
  selector1:not(selector2)  获取所有已选择到的元素中的除了selector2的元素
  selector:header   获取所有已选择到的元素中的标题元素(h1~h6)    

```

### ***2.内容过滤器：***

>   selector:contains(text) 
>             获取所有已选择到的元素中文本包含text的元素
>   selector:empty 
>             获取所有已选择到的元素中的空元素(没有子节点)
>   selector:parent 
>             获取所有已选择到的元素中的非空元素(有子节点)，如$("div:parent");
>
> ​			获取所有已选择到的元素中包含selector2的元素，如$("div:has('span')");

### ***3.可见性过滤器：***

隐藏类型分两种：

>   1）不占据屏幕空间
>         display:none;
>         <input type="hidden">	
>   2）占据屏幕空间
>         visibility:hidden;
>         opacity:0;//透明度为0
>    使用：
>       :visible   选择所有占据屏幕空间的元素
>       :hidden    选择所有不占据屏幕空间的元素   

### ***4.属性过滤器：***

>    selector[attrKey]  
>            获取所有已选择到的元素中具有属性attrKey的元素
>    selector[attrKey=attrVal]    
>            获取所有已选择到的元素中具有属性attrKey，并且属性值为attrVal的元素
>    selector[attrKey^=attrVal]  
>            获取所有已选择到的元素中具有属性attrKey，并且属性值为以attrVal开头的元素
>    selector[attrKey$=attrVal]  
>            获取所有已选择到的元素中具有属性attrKey，并且属性值为以attrVal结尾的元素
>    selector[attrKey*=attrVal]  
>            获取所有已选择到的元素中具有属性attrKey，并且属性值为包含attrVal的元素
>    selector[attrKey!=attrVal]  
>            获取所有已选择到的元素中具有属性attrKey，并且属性值不为以attrVal的元素或者没有属性attrVal的元素

### ***5.后代选择器：***

>    selector:nth-child(index)
>        获取每个selector元素中索引为index的子元素。【注意】index从1开始
>    selector:first-child
>        获取每一个selector元素中的第一个子元素（每个父元素的第一个子元素）
>    selector:last-child
>        获取每一个selector元素中的最后一个子元素（每个父元素的最后一个子元素）
>    selector:only-child
>        获取每一个selector元素中的独生子子元素（每个父元素如果只有一个孩子元素，获取该元素）
>    selector:first-of-type
>        获取每个selector元素中每种类型子元素中的第一个
>    selector:last-of-type
>        获取每个selector元素中每种类型子元素中的最后一个

### ***6.表单过滤器：***

>    :checked    选取所有被选中的元素，用于复选框、单选框、下拉框
>    :selected   选取所有被选中的元素，该选择器只适用于<option>
>    :focus   选取当前获取焦点的元素
>    :text    选取所有的单行文本框(<input type="text">)
>    :password  选取所有的密码框
>    :input     选取所有的<input>,<textarea>,<select>,<button>元素。
>        *注意，$(":input")是选中可以让用户输入的标签元素；而$("input")是选择名字为input的标签元素。*
>    :enable   选取所有可用元素，该选择器仅可用于支持disable属性的html元素。(<button>,<input>,<optgruop>,<option>,<select>,<textarea>)
>    :disable   选取所有不可用元素，该选择器也仅可用于支持disable属性的html元素。
>    :radio      选取所有的单选框
>    :checkbox   选取所有的多选框
>    :submit     选取所有的提交按钮
>    :image      选取所有的input类型为image的表单元素
>    :reset   选取所有的input类型为reset的表单元素
>    :button  选取所有的input类型为button的表单元素
>    :file    选取所有的input类型为file的表单元素

## **六、jQuery中的Dom操作**

### **1.查找节点**

通过jQuery选择器来完成

### **2.创建节点**

>   创建元素节点：var newTd = $("<td></td>")
>   创建文本节点：var newTd = $("<td>文本内容</td>")

### **3.插入节点**

>   1) $A.append(B)
>        将B追加到A的末尾处，作为它的最后一个子元素
>   2) $A.appendTo(B)
>        将A追加到B的末尾，作为它的最后一个子元素
>   3) prepend() 
>        $A.prependTo(B)
>             将A追加到B的前面，作为它的第一个子元素
>        $A.after(B)
>             在A之后追加B，作为它的兄弟元素
>        $A.insertAfter(B)
>             在B之后追加A，作为它的兄弟元素
>        $A.before(B)
>             在A之前追加B，作为它的兄弟元素
>        $A.insertBefore(B)
>              在B之前追加A，作为它的兄弟元素

### **4.删除节点**

>    remove([selector])
>        从DOM中删除所有匹配的元素，返回值是一个指向已经被删除的节点的引用，可以在以后再使用这些元素。
>        该方法会移除元素，同时也会移除元素内部的一切，包括绑定的事件及与该元素相关的jQuery数据。
>    detach([selector])
>        与remove()类似，但是detach()保存所有jQuery数据和被移走的元素的相关联事件。
>    empty()
>        无参数。从DOM中清空集合中匹配元素的所有的子节点。

### **5.复制节点**

>  $("#id").clone(false);
>   该方法返回的是一个节点的引用，参数默认为false，为浅复制；
>   参数是true,为深复制，含义是：复制元素的同时复制元素中所绑定的事件。

### **6.替换节点**

>    replaceWith(newContent);
>         用新内容替换集合中所有匹配的元素，并且返回被删除的元素的集合。
>         该方法会删除与节点相关联的所有数据和事件处理程序。
>    replaceAll(target);
>         用集合的匹配元素替换每个目标元素。颠倒了replaceWith()操作效果。

### **7.包裹节点**

>    wrap([wrappingElement])
>         在每个匹配的元素外层包上一个html元素
>    warpAll([wrappingElement])
>         将所有匹配的元素用一个元素来包裹，在所有匹配元素外面包裹一层HTML结构
>    warpInner([wrappingElement])
>         每个匹配元素里面内容(子元素)都会被这种结构包裹

### **8.节点遍历**

>    children([selector]) 
>         用于取得匹配元素的子元素集合(只考虑子元素而不考虑任何后代元素)
>       $('.content.inner')=>$('.content').children('.inner');  
>    find(selector)
>         在当前对象元素中的子元素查找，和参数所匹配的所有的后代元素
>    next([selector])
>         取得匹配的元素集合中每一个元素紧邻的后面兄弟元素
>    nextAll([selector])
>         查找当前元素之后所有的同辈元素
>    prev([selector])
>          取得匹配元素前面紧邻的兄弟元素
>    prevAll([selector])
>          取得当前元素之前所有的同辈元素
>    silibinng([selector])
>          取得匹配元素的前后所有的兄弟元素
>    closest(selector)
>          取得和参数匹配的最近的元素，如果匹配不上继续向上查找父元素
>    filter(selector)
>          把当前所选择的所有元素再进行筛选过滤
>    parent([selector])
>          取得匹配元素的集合中，每个元素的父元素
>    parents([selector])
>          获得集合中每个匹配元素的祖先元素

# 第二章 jQuery的事件和API

## **一、事件**

>    on()
>       在选定的元素上绑定一个或多个事件处理函数。
>    off()
>       移除一个事件处理函数。
>    trigger()
>       根据绑定到匹配元素的给定的事件类型执行所有的处理程序和行为。

## **二、鼠标事件**

>    click()  单击
>    dblclick()  双击
>    hover()   悬停
>    mousedown()  按下
>    mouseup()  抬起
>    mouseenter()  移入  不支持子元素
>    mouseleave()  离开  不支持子元素 
>    mouseout()   离开  支持子元素 
>    mouseover()  进入  支持子元素 
>    mousemove()  移动

## **三、键盘事件**

>    keypress()   按键按下
>    keyup()   按键抬起
>    keydown()   按键按下

## **四、表单事件**

>    focus()  聚焦事件
>    blur()    失去焦点事件
>    change()  当元素的值发生改变时激发的事件
>    select()  当textarea或文本类型的input元素中的文本被选择时触发的事件
>    submit()  表单提交事件，绑定在form上

## **五、jQuery中常用的API**

### *1.jQuery中的html(),text(),val()方法*

>    html()
>            无参：获取html的值
>            有参数html：设置html的值
>
>    text()
>            无参：获取文本值
>            有参数text：设置文本值
>    val()
>            无参：获取value的值
>            有参数value：设置value的值

### *2.jQuery中的工具方法*

>    1) get();  //以数组的形式返回DOM节点。
>         console.log($('div').get());
>         
>    2) toArray();  //返回一个包含jQuery对象结合中的所有DOM元素数组。
>         console.log($('div').toArray());
>
>    3) eq(index);  //返回index位置上的jQuery对象。
>         console.log($('div').eq(1).get(0));
>
>    4) filter(function(index,item){});   //过滤器函数，返回jQuery对象。
>         var $result = $('div').filter(function(index,item){
>         return index == 2;
>       });
>       
>    5) map(function(index,item){});   //用于获取或设置元素集合中的值。
>         var $result = $('div').map(function(index,item){
>         return $(item).html()
>       });
>     
>    6) each(function(index,item){});  //遍历一个jQuery对象。
>           $('div').each(function(index,item){
>         console.log(index,'--',item);
>       });
>
>    7) slice(0,3);  //截取
>         var $result = $('div').slice(0,3);
>       console.log($result.get());
>       }); 
>
>   8) not() 
>   9) first()
>   10) last()
>   11) is()
>   12) has()

### *3.jQuery中属性设置函数*

>    //获取属性值
>        attr(key)
>        prop(key)
>     //删除属性   
>        removeAttr(attributeName)
>        removeProp(propertyName)
>     //批量设置属性   
>        css(key)
>     //添加类   
>        addClass(className)
>     //判断有没有指定的类，有，返回true，否则返回false   
>        hasClass(className)
>     //删除类   
>        removeClass(className)

# 第三章 jQuery中的动画

## **一、jQuery样式相关方法**

宽度 = width + 2padding + 2border + 2margin

>    //获取视口区的宽高：
>    1、$(window).height()
>    2、$(window).width()
>
>    //获取内容区的宽高：
>       $('div').width(); 
>       $('div').height();
>
>    //获取内容+padding区的宽高：
>    3、$('div').innerHeight()
>    4、$('div').innerWidth()
>
>    //获取内容+padding+border区的宽高：
>    5、$('div').outerHeight()
>    6、$('div').outerWidth()
>
>    //获取内容+padding+border+margin区的宽高： 
>     $('div').outerWidth(true); 
>     $('div').outerHeight(true);
>
>    //获取相对于文档的偏移
>    7、.offset()
>
>    //获取相对于定位父元素的偏移
>    8、.position()
>
>    //横向滚动条左侧的偏移
>    9、.scrollLeft()
>
>    //横向滚动条上侧的偏移
>    10、.scrollTop()
>
>    //获取离它最近的父定位元素  
>    11、.offsetParent()

## **二、效果**

>     1.基本效果
>       1）隐藏 hide()
>       2）显示 show()
>       3）隐藏与显示 toggle()
>     
>     2.淡入淡出效果
>       1）淡入  fadeIn()
>       2）淡出  fadeOut()
>       3）淡入到 fadeTo()
>       4）淡入与淡出 fadeToggle()
>     
>     3.滑动效果
>       1）滑下 slideDown()
>       2）滑上 slideUp()
>       3）滑上与滑下 slideToggle()
>     
>     4.自定义效果
>       animate()      

