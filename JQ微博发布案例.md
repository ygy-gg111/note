1微博发布效果

(1)功能

1点击发布，动态生成li，把表格的内容给小li,并让小li滑入ul中

``` javascript

$(".btn").click(function(){
var li = $("<li></li>"); //创建小li
li.html($(".tex").val()+"<a href = "javascript;:">删除</a>");  //赋值给小li
$("ul").prepend(li);   //把小li添加到 ul 不添加 看不见
 li.slideDown(); // 小li 慢慢滑下去
$(".tex").val(""); //把文本清空
  })

```

2点击删除按钮 可以删除当前的微博留言

``` javascript
$("ul").on("click","a",function(){ // 小li是动态生成的，要使用 .on() 绑定事件  // $(this).parent().reomve()
    $(this).parent().slideUp(function(){
        //可以选择把小li滑上去 但是小li还存在
       //  利用回调函数把小li删掉
         $(this).reomve()
    })
})
```

  2 off()解绑事件

  ```javascript
$("div").off() //接触div身上的所有事件
$ ("div").off("click") // 解除div的一个事件
$("ul").off("click","li") //可以阻止事件冒泡
  ```

   只触发一次

```javascript
$("p").one("event",function(){})
```

3自动播放

```javascript
//先要绑定了事件
$("div").on("click",function(){
            执行语句
            })
//1 简写模式 自动播放
$("div").click();
// 2 element.trigger("事件")
 $("div").trigger()
// 3 element.triggerHandler("事件) 这个不会触发冒泡事件
$("div").triggerHandler("click")
```



4事件对象

```javascript	
$("div").on("click",function(e){
    console.log(e);
    e.stopPropagation() //阻止事件冒泡
})
```

$.extend(true/false,复制的对象,被复制的对象)

![](C:\Users\性感荷官\Desktop\拷贝.png)

```javascript	
var targerobj = {};//要复制的对象
var obj = {
    id : 10,
    uname : "andy",
    sex : {
        age : 18
    }
    
} // 被复制的对象
  $.extend(targerobj,obj) //浅拷贝 ，如果有相同属性 则会被覆盖  浅拷贝把原来对象里面的复杂数据类型地址拷贝给目标对象 

   targerobj.sex.age = 20;
 //修改了复制的对象 被复制的对象也会改变 
  console.log(obj.id) //20
$.extend(true,targetobj,obj) //深拷贝
```

5js瀑布流 

Jquery之家  可以下载很多插件 

先下载 然后查看框架代码 

引入js ,css文件 

最后找到html css 样式复制进去 修改

6图片懒加载

只显示用户看到的可视区的图片  

页面滑动加载图片 

JS插件   

vsc 快速替换多个

ctrl + h    <img src = 

​                  <img date

 引入的js文件和js 必须放在图片下面

7 js插件 全屏滚动

 网址 ： gtiHub : https://github.com/alvarotrigo/fullPage.js

中文翻译 ： http://www.dowebok.com/demo/2014/77/

​      点击页面最下的说明

先把css 和js找出来放到对应文件下

必须先引入jquery.min.js文件

再引入其他js文件 和css文件

复制html

 

8bootstrap 

先把css 和js文件放好 

然后先引入jq文件 ，再引入js文件 

模态框调用 

1自定义属性 data-

```javascript	
<button type = "button" data-toggle = "modal",data-target = '#随便起名'>点击</button>
//data-toggle 为切换  data-target 为目标的名字 需要在指定模态框给他一个id 
```





2通过javascript 

```javascript	
$("button").on("click",function(){
    $("要调用的模态框名").modal()
})
```

9阿里佰秀

10todolist

1刷新页面不会丢失数据 因此用到本地存储 localStarage;

2核心思路 ：不管按下回车，还是点击复选框，都是把本地存储的数据加载到页面，这样保证页面关闭不会丢失数据

3   数据格式    

```javascript
var todolist = [ {title : "xxx", done :"false" },{title : "xxx", done :"false" }]
//title里面的值 赋值给动态创建的li上的表单的val
```



  1本地存储里面**只能存储字符串**的数据格式 把我们的数组对象转换为字符串格式 JSON.stringify()

```java	
var todolist = [{title : "xxx",done : false},]
loaclStorage.setItem("todo",todolist)//存不了
localStorage.setItem("todo",JSON.stringify(todolist))  //先把todolist转换为字符串，才可以存储起来  
 var data = localStorage.getItem("todo")
```

2  获取本地存储的数据 我们需要把里面的字符串数据转换为 对象格式 JSON.parse()

```javascript
data = JSON.parse(data);
console.log(typeof data) //object
console.log(data[0].title) //调用数据
```

 1todolist按下回车键把新数据添加到本地存储里面

3todolist 删除模块

1点击里面的a链接 不是删除的li,而是相应的数据

2核心原理  先获取本地存储数据，删除相应的数据，保存给本地存储，重新渲染

3我们可以给链接自定义属性记录当前的索引号



```javascript
//1按下回车把完整数据 存储到本地存储里面
// 里面数据存储的形式 var todolist = [{title : "xxxx",done : false},{},{}]
$(function(){
    load() //一打开就把数据渲染
    $(".title").on("keydown",function(e){
        if(e.keyCode ===13){
           alert("请输入内容")      
        }else{
              //先读取本地存储中原来的数据  因为很多地方都要读取我们  在外面声明一个函数
            var local = getData();
            //  把local数组进行更新数据 把更新的数据追加给local数组
          local.push({title :$(this).val() ,done : false}) // 回车一次 就把title里面的数据给local
             //把local存储到本地存储
            //由于多个地方需要 存储 所以采取函数
            saveDate(local);//local相当实参
            //当我们按下回车 todolist 本地存储数据渲染加载到页面
            load();//在外面申明一个load()函数
             $(this).val("")//按下回车 清空表单内容  
        }
    })；//回车事件结束
    // 3 todolist 删除事件
    //点击a链接  先获取本地存储数据，删除相应的数据，保存给本地存储，重新渲染
    //因为a是动态创建的 所以用.on绑定事件
    $("ul ol").on("click","a",function(){
        //先获取本地存储
        var data = getData();
        console.log(data)//object
        //修改数据
         var index = $(this).attr("id");//获得a的自定义属性  
        console.log(index)//对应的索引号
        //删除得到的某一个数组数据 splice(从哪里开始删，删几个)
        data.splice(index,1)
        //重新保存到本地存储
          saveData(data)  //一定要重新保存到本地存储
        //重新渲染
         load()
        
        
    })//a点击结束
    //4 todolist 正在进行和已完成选项操作
    $("ol ul").on("click","input",function(){
        //先获得本地存储
        var data = getData();
        //修改数据
         var index = $(this).siblings("a").attr("id");)//
        //获得当前ul兄弟a的索引号
        console.log(index);
        data[index].done = ?
        data[index].done = $(this).prop("checked");
        console.log(data)
           // 保存到本地存储
        saveDate(data);
        // 重新渲染页面
        load();
    })


    
     
    
    
    
    
    
    
   //读取本地存储的数据
    function  getData(){
       var data = localStorage.getItem("todolist");//读取本地存储中原来的数据  
        if(data != null){
            //如果data不为空则要数据存取起来 
            //由于本地存储的数据类型是字符串 我们要把它转换为对象 JSON.parse()
            return JSON.parse(data)
        }else{
            []
        } //结束if
    }  //结束getData()
    
    // 保存本地存储
    function saveData(data){ //data 是实参 
  localStorage.setItem('todolist',JSON.stringify(data))//local存不进来 因为本地存储存的是字符串类型的 local开始是数组  要使用JSON.stringify(data)
    }；
    //渲染加载数据
    function load(){
        //先把本地存储的数据获取过来
        var data = getData()；
         //遍历之前先清空ol ul之前里的数据
            $("ol,ul ").empty();
        //先声明二个变量 
        var todoCount = 0;
        var doneCount = 0;
             
        //遍历数组 动态生成li
        $.each(data,function(i,n){
            console.log(n);
            if(n.done){
                //如果done true 就把数据放在完成哪里     //完成时 input的chekde为true
                $("ul").prepend("<li><input type ='checkbox' checked = "checked"> <p>"+n.title+"</p> <a href = 'javascript:;' id= '"+i+"'></a></li>");
                doneCount++ //统计出现次数
                
            }else{
                           //动态生成小li  在生成小a的时候给他一个自定义属性
            $("ol").prepend("<li><input type ='checkbox'> <p>"+n.title+"</p> <a href = 'javascript:;' id= '"+i+"'></a></li>")  // 记得注意""里面的冒号 不能重复 
            //添加变量记得 引引加加
                doneCount++; 
            }
 
        })//遍历结束
        //最后修改相应的元素 text（）
        $("#todocount").text(doneCount);
        $("#donecount").text(doneCount)
    }//渲染结束
})
```

索引号  index()

如果不是亲兄弟 就不会编排号

```javascript
<ul>
    </ul>
```

 删除数组中的元素  splice(从哪个位置开始删除，删除几个元素)

```javascript
var arr = ["a","b","c"]
console.log(arr.splice(1,1))
console.log (arr) //["a","c"]
```

   todolist正在进行和已完成选项操作

1 当我们点击了小复选框 修改本地存储，再重新渲染数据列表

2点击之后，获取本地存储数据

3修改对应数据属性done为当前复选框的checked状态

4之后保存数据加载数据列表

5

todolist统计正在进行个数和已经完成个数

再load()函数里面操作

声明2个变量；todoCount代办个数 /和doneCount已完成个数

最后修改相应的元素 text（）

jquery尺寸和位置

jquery尺寸大小

以下4个都返回数字型

1width() /height()  取得宽度和高度 不包含padding和border 



2innerWidth()/innerHeight()  取得宽度和高度 包含padding

3 outerWidth()/outerHeight()  取得宽度和高度 包含padding + border

4 outerWidth(true) / outerHeight(true) 返回padding+border + margin

jquery位置

offset()  position(), scrollTop() //scrollLeft()

offset()设置和获取元素偏移  相对于文档来说 和父亲没关系



返回一个对象  要返回一个属性 则要调用对象的属性

```javascript
$("").offset().top

$("").offset().left
```

设置元素偏移   

```javascript
$("").offset({

top :xxx,

left :xxx

})
```

position()得到带有定位父级位置相对偏移  如果父级没有定位 则以文档为准  不能设置 只能获取

```javascript
$(".son").position()
```

 scrollTop()/scrollLeft() 得到被滚动减去的偏移量

 scrollTop(100)可以设置  

 ```javascript
  //页面滚动到某一数据 ，返回顶部显示
 var  boxTop = $(".container").offset().top;
$(window).on("scroll",function(){
    if($(this).scrollTop() === boxTop){
        $(".back").fadeIn();
    }else{
          $(".back").fadeOut();
    }
 


})；
$(".back").on("click",function(){
    $(window).scrollTop(0)
    //只有元素才可以做动画
    $("body,html").animate({
        scroll : 0
    })
}) //
 ```

**电梯导航**

 当页面滚动到今日推荐模块 然后让小盒子显示出来

记得引入文件

每一个小盒子对应一个大盒子

点了小盒子 获取当前的索引号 然后让对应的大盒子获取offset().top，赋值给animate函数

当我们页面滚动到某一内容模块，就让电梯导航对应的盒子添加一个类名

利用$.each()拿到内容区域每一个模块的元素和索引号

判断条件  被卷去的头部大于等于内容区域里面的每个模块的offset().top 说明滚动到了盖盒子 然后根据索引号给小盒子添加类名

```javascript
$(". li").eq(i).addclass("").siblings().removeclass();
```

当我们点击了小li，此时不需要去执行 页面滚动事件里面的li的背景选择 添加current

```javascript
//节流阀 互斥锁  
var flag = true;
//一点击小li flag = false;
if(flag){
         $(".floor .w").each(function(i,n){
            console.log(n);
            if($(document).scrollTop >= n.offset().top){
                console.log(i);
           $("").eq(i).addclass("current").siblings().removeclass("current")
            }//判断结束
    },
```

当页面滚动后再打开 

 .animate({},function(){

flag = true}) 

 ```javascript
$(function(){
    //我们想要一开始就加载所以用函数
     toggleTool();
    var flag = true; //节流阀
    function toggleTool(){
           if($(document).scrollTop() >= toolTop){
           $("").fadeIn();
    }else{
                $("").fadeOut();  
                 } 
    }
    var toolTop = $(".").offset().top
    $(window).on("scroll",function(){
      toggleTool();
        if(flag){
              $(".floor .w").each(function(i,n){   //切记 n为dom对象 需要转换为jq对象
            console.log(n);
            if($(document).scrollTop >= $(n).offset().top){
                console.log(i);
           $("").eq(i).addclass("current").siblings().removeclass("current")
            }//判断结束
        }
        })//遍历结束
    })//滚动事件结束
    //点击
    $(". li").on.click(function){
        flag = false;
       console.log($(this).index());//得到当前索引号
        //当我们点击了li,就需要计算出页面要去往的位置   选出对应索引号的内容区的大盒子，计算出他的offset.top()
        var current = $(".floor .w").eq($(this).index()).offset().top;
        $("body,html").animate({
            scrollTop : current
        }，function(){
            flag = te=rue
        });
        //点击之后让当前的小li添加类名 其他兄弟移除     
     $(this).addclass("current").siblings().remove("current")
    }
})
 ```











