---
title: AJAX初窥门径教程
date: 2016-06-06 14:15:38
tags: ['javascript','Ajax']
description: 本篇完成了ajax中主要的入门讲解
photos:
- http://o84rkcsji.bkt.clouddn.com/coding-img.jpg
---


从2005年开始AJAX这个名次逐渐进入人们的视野，一时之间炙手可热、风头无两，甚至说互联网行业因为这项技术而发生了改变也不为过。

前端开发者走到这里突然窥到了后端的门径，而后端开发者走到这里似乎也摸到了前端的奥秘。所以穿上AJAX套装的，可以骄傲的宣称 __「我是双持狂暴战」__（大雾）。

<!-- more -->

# 1.AJAX起源
AJAX是“Asynchronous JavaScript and XML”的缩写，意即“异步的Javascript与XML”。根据考证，AJAX技术最早由由杰西·詹姆士·贾瑞特所提出。目的是为了减少表单提交过程中过多请求相同页面导致的带宽浪费问题，同时可以减少表单填写错误而必须重新填写整套表单而带来的用户体验下滑。

# 2.AJAX的过程描述
## 2.1 基础的铺垫

前面说了这么多AJAX的历史，那么这种技术的本质到底是什么呢？这种一听名字就觉得高端大气上档次的名字其实很唬人的，但是如果剖悉一下就会发现其实也很简单，从名字的字面来看其实只有三个内容：异步-javascript-XML。

对于以上三个内容，javascript不用多解释了。所谓「异步」就是相对于「同步」而言，最初的表单提交方式是同步的，我们输入完整表单->点击提交->数据发往服务器->服务器处理表单->进入新页面，这种模式下如果某个表单填写不对也只能等到服务器处理完表单之后我们才能发现；异步方式就方便多了，我们每填写一条表单浏览器都可以单独将这些内容发往服务器进行验证，而我们的浏览器还是保留在当前的页面不用跳转，服务器返回的提示也可以更新页面上的一部分内容，这个用户体验就上去了，B格max啊。
XML的全称是e**X**tensible **M**arkup **L**anguage，意思既可扩展的标记语言，最初的AJAX用这种格式来传递数据。

## 2.2实现的原理
现在我们了解AJAX是几项技术糅杂在一起的一门技术，那么串联他们的是什么呢？`XMLHttpRequest对象`。

XMLHttpRequest对象是一组浏览器脚本语言（例如javascript）可用的**API集合**，用于向服务器发送http或者https请求，并且在后台载入服务器的响应。虽然名字中带有XML，但是随着技术的发展，发送数据的手段早已不仅仅局限与Xml语言，现在更多json来传递数据。

XMLHttpRequest对象是浏览器端的一套API集合，表面上来看所有的工作都是前端开发者在做，但是一套完备的AJAX流程却同时需要前端与后端的知识才能全部跑通。接下来我会用浅显一点的内容来解释一下这个过程。

> 1. 整个过程的发起者来自于浏览器端，浏览器的某一个事件会触发发起请求的过程。
> 2. 请求发起，需要同服务器进行[三次握手](http://www.cnblogs.com/rootq/articles/1377355.html)来打开连接通路。
> 3. 连接建立后，浏览器将发出包含`请求头与请求体`的请求包。
> 4. 服务器在接受到请求包后会进行一系列的业务逻辑工作，并在最终向浏览器发送响应（什么都不响应也是一种响应，超时响应）。
> 5. 响应也已打包的方式回到浏览器，内部包括响应头与响应体。浏览器收到响应后，连接关闭。
> 6. 浏览器接受的响应体中一般包含我们所需要的数据。

在大概了解以上内容后，我们开始最实际的操作过程。

# 3.前端层面的最基础使用方法

XMLHttpRequest对象的使用方法非常简单，通常只需要创建一个对象的实例：
``` javascript
var xhr = new XMLHttpRequest();
```

之后我们将使用xhr对象中的各种方法和属性来与服务器不断通讯，并且动态更新当前的页面内容。
之后的写法是：
```javascript
xhr.onreadystatechange = function(){
    //do something
}
```
这里我们让xhr对象的状态机做好准备，监控readystate随时可能发生的变化，并且在捕获到变化的时候执行紧随其后的函数。
技能CD转完，附魔合剂也都就绪，接下来就是开怪时刻：
```javascript
xhr.open(string method,string url,[boolen async]);
xhr.send([data]);
```
对于open方法中的三个参数的说明：
1. method代表本次请求的方法，比较常见的两种请求方法是`"POST"`和`"GET"`。
2. url为请求发往的地址，同样以字符串的形式传入。
3. 第三个参数代表是否使用异步，默认为true，这个属性为选填，多数情况下我们都使用默认值即可。

对于请求方法这里，其实展开说的话内容非常多，这只简单介绍一下最常见的这两种。
1. GET方法顾名思义是向服务器获取内容，不涉及更改，这种请求方式是通过URL参数将数据提交到服务器的。因为在传输过程中请求的参数都是明文暴露在url中的，所以 **对于敏感内容（如登录信息）和需要服务器端作出更改的内容一定不要使用这种方式**。
2. POST方法与GET的主要区别在于传输数据的方式不同。所谓post即发送的意思，明确表示将携带数据发往服务器。数据会封装在send方法的参数中传递。需要注意的是必须在 **send()之前设置请求头**：
```javascript
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
```

至此我们在浏览器上已经实例化了一个XHR对象，并使用这个对象设置了一个状态机，打开了通往服务器的后门，并且发出了数据。那我们暂且停下浏览器端的行为，看看数据发往服务器后会怎样。

# 4.服务器层面的最基础使用方法

现在假设我们在服务器上使用了世界上最好的语言——php，并且建立了一个名为`testget.php`的文件用来准备接受前端发来的请求。那么前端层面如果使用：

```javascript
xhr.open("GET","testget.php?name='monkey'",true);//所以你看，get方式传递参数就是要靠自己拼装url的字符串来进行，讲道理的话应该用encodeURIComponent()方法进行编码。
```

数据发送后在后端应该如何使用呢？非常简单：

```php
<?php 
    $name = $_GET['name'];//对于前端发来的请求都会用$_GET[],$_POST,$_REQUEST[]这样的全局变量来保存的。
    echo $name;//monkey
 ?>
```

然而还不够，服务器获取到了浏览器发来的请求，可以做自己的业务逻辑了，但是还必须要为浏览器再返回一些数据，让整个过程完整。如何返回内容呢？刚才代码中的`echo $name`是一种办法，但是回传的只能是字符串。最开篇已经说到现在流行的方案是使用json来传输数据，那么我们就不考虑其他的数据类型，只考虑传输一个json对象的方法，刚才的代码做点修改：

```php
<?php
    header("Content-type: application/json; charset=utf-8"); //为了告诉浏览器：「哥哥这次给你送来的是一个json数据包」那么必须设置一下响应头。
    $monkey=array("first"=>"孙行者","second"=>"者行孙","third"=>"行者孙");
    echo json_encode($monkey);//json_encode是php中生成json对象的方法
?>
```

现在一个打着「我是json」邮戳的顺丰加急快递已经发货了，之后我们又要回到浏览器端。

# 5.重回前端层面
还记得我们之前定义过一个状态机么？

```javascript
xhr.onreadystatechange = function(){
    //do something
}
```
这里还什么都没做呢，后续的处理过程其实就是完善状态机触发后执行的函数，此前先了解一点关于 `请求的状态：readystate`的知识：
> 请求有五种状态，分别是：
>  - 0 UNSENT (未打开)    open()方法还未被调用.
>  - 1   OPENED  (未发送)   send()方法还未被调用.
>  - 2   HEADERS_RECEIVED (已获取响应头)   send()方法已经被调用, 响应头和响应状态已经返回.
>  - 3   LOADING (正在下载响应体)   响应体下载中; responseText中已经获取了部分数据.
>  - 4   DONE (请求完成) 整个请求过程已经完毕.
 
也就是说在一个请求过程中，onreadystatechange会在每次readystate发生改变的时候被触发，而最重要的一次就应该是readystate为4的那一次。如果再配合我之前一篇关于[AJAX响应状态码](http://phoeshow.github.io/2016/05/20/Ajax%E4%B8%AD%E7%9A%84%E7%8A%B6%E6%80%81%E7%A0%81/)的描述，这里的过程就变得清晰明了。

那么浏览器里的完整js代码应该是：
```javascript
var xhr = new XMLHttpRequest();
xhr.responseType='json';//虽然指定了回传的是个json数据包，但是还是要告知一下浏览器，这个xhr对象接受到的是个json响应。
xhr.onreadystatechange = function(){
    if(xhr.readyState==4 && xhr.status==200){//请求状态为4，响应状态为200 OK ，表明本次请求完成且正确返回。
        var data = xhr.response;//xhr.response中既是本次响应的返回。
        console.log(data.first);//孙行者
    }
}
xhr.open('GET','testget.php',true);//好吧，因为php代码里也没啥非要从前端获取的，就是回传一个json回来，那就不用考虑url传递参数了。
xhr.send();
```

# 6.感谢封装了JQuery的的大神
一套完整的AJAX流程跑完是不是感觉很简单，但是真的这么容易么？当然不是啦，其实上面这个写法实际中是不好用的，主要有以下几个问题：
- 兼容性不好，IE7才开始有XMLHttpRequest对象，而此之前的IE浏览器是用ActiveX来实现功能。
- 扩展性不好，readyState还有1/2/3这三种状态，而status更是有400/404/500/503等等常见状态会出现。并没有相关的操作。
- 复用性不好，一个页面中会有很多地方使用到AJAX，总不能每次调用我们就按照上面这个方法写一遍代码。

为了解决上述几个问题，其实我们可以封装一个AJAX库。不过如果使用JQuery的话一切都变得非常简单了：
```javascript
$.ajax({
    type:'GET',
    url:'testget.php',
    dataType:'json',
    success:function(data){
        console.log(data.first);//服务器响应成功的话，响应体以data参数的形式传入函数中供调用。
    },
    error:function(){
        console.log('请求失败');
    }
    })
```

是不是方便多了？


### 参考文献：
- [Ajax中的状态码](http://phoeshow.github.io/2016/05/20/Ajax%E4%B8%AD%E7%9A%84%E7%8A%B6%E6%80%81%E7%A0%81/)
- [JQuery手册中Ajax的底层接口](http://www.jquery123.com/jQuery.ajax/)
- [MDN XMLHttpRequest对象](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest#send())
- [原生JS写Ajax的请求函数](http://caibaojian.com/ajax-jsonp.html)



