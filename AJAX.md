# AJAX编程

## Ajax

即 Asynchronous Javascript And XML，AJAX 不是一门的新的语言，而是对现有持术的综合利用。

1. 基于web标签的xhtml+css
2. 可以使用dom进行动态的显示和交互
3. 使用XML和XSLT(是一种用于将XML[文档](https://baike.baidu.com/item/%E6%96%87%E6%A1%A3)转换任意文本的描述语言)进行数据的交换和操作
4. 使用XMLHttpRequest进行异步的数据查询和检索等操作。。。

本质:是在HTTP协议的基础上以异步的方式通过XMLHttpRequest对象与服务器进行通信。

作用：可以在页面不刷新的情况下，请求服务器，局部更新页面的数据；

## XMLHttpRequest

浏览器内建对象，用于在后台与服务器通信(交换数据) ，由此我们便可实现对网页的部分更新，而不是刷新整个页面。

### 请求

1. 首先先实例化对象（异步对象）   var xhr = new XMLHttpRequest();
2. 请求行  xhr.open()  请求方式 目标地址  协议
3. 请求头   get请可以不设置请求头，post方式需要请求头   xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded'); 浏览器发送给服务端的一些额外的数据
	. 请求体   xhr.send（null），因为get请求没有请求体，它是url拼接参数来传递的   发给服务器端的真正数据		格式必须是字符串形式的键值对“username=aa&userpwd=20”

### 响应

由于服务器做出响应需要时间（比如网速慢等原因），所以我们需要监听服务器响应的状态，然后才能进行处理。

onreadystatechange(不是驼峰命名法) 是   Javascript 的事件的一种，其意义在于监听 XMLHttpRequest 的状态 

```javascript
xhr.onreadystatechange=function(){
    if(xhr.readyState==4 && xhr.status==200){
        var res=document.querySelector('.res');
        res.innerHTML=xhr.responseText;
    }
}
```

1.获取状态行（包括状态码&状态信息）

```javascript
xhr.status//状态码   美 [ˈstetəs]
xhr.statusText  //状态信息
```

2.获取响应头

```javascript
xhr.getResponseHeader('Content-Type');//获取指定头信息   response  美 [rɪ'spɑns] 响应
xhr.getAllResponseHeaders();//获取所有响应头信息
```

3.响应主体

```javascript
xhr.responseText;
xhr.responseXML;
```

我们需要检测并判断响应头的MIME类型后确定使用request.responseText或者request.responseXML

### API详解

```javascript
xhr.open();//发起请求，可以是get、post
xhr.setRequestHeader();//设置请求头
xhr.send();//发送请求主体 get方式使用xhr.get(null);
xhr.onreadystatechange=function(){}监听响应状态   state  美 [stet] 
```

readstate 属性有五个状态：

- xhr.readyState = 0时，（未初始化）还没有调用send()方法
- xhr.readyState = 1时，（载入）已调用send()方法，正在发送请求
- xhr.readyState = 2时，（载入完成）send()方法执行完成，已经接收到全部响应内容
- xhr.readyState = 3时，（交互）正在解析响应内容
- xhr.readyState = 4时，（完成）响应内容解析完成，可以在客户端调用了

`不用记忆状态，只需要了解有状态变化这个概念`

xhr.status表示响应码，如200

xhr.statusText表示响应信息，如OK

xhr.getAllResponseHeaders() 获取全部响应头信息

xhr.getResponseHeader('key') 获取指定头信息

xhr.responseText、xhr.responseXML都表示响应主体

****注GET和POST请求方式的差异（面试题）****

1、GET没有请求主体，使用xhr.send(null)

2、GET可以通过在请求URL上添加请求参数

3、POST可以通过xhr.send('name=itcast&age=10')

4、POST需要设置请求头

```javascript
xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');
```

5、GET大小限制约4K，POST则没有限制

##  XML extensible markup language

XML是一种标记语言，很类似HTML，其宗旨是用来传输数据，具有自我描述性（固定的格式的数据）。

### 语法规则

1、必须有一个根元素 

2、标签名称不可有空格、不可以数字或.开头、大小写敏感

3、不可交叉嵌套

4、属性双引号（浏览器自动修正成双引号了）

5、特殊符号要使用实体

6、注释和HTML一样

虽然可以描述和传输复杂数据，但是其解析过于复杂并且体积较大，所以实现开发已经很少使用了。

## JSON

即 JavaScript Object Notation，另一种轻量级的文本数据交换格式，独立于语言。

###   语法规则

1、数据在名称/值对中

2、数据由逗号分隔(最后一个健/值对不能带逗号)

3、花括号保存对象方括号保存数组

4、使用双引号 

### JSON解析

JSON数据在不同语言进行传输时，类型为字符串，不同的语言各自也都对应有解析方法，需要解析完成后才能读取

1、Javascript 解析方法

JSON对象   JSON.parse()（字符串转对象或数组）、 JSON.stringify()（数组或对象转字符串）；

2、PHP解析方法

json_encode()、json_decode()

总结：JSON体积小、解析方便且高效，在实际开发成为首选。

## jQuery中的Ajax

jQuery为我们提供了更强大的Ajax封装

$.ajax({}) 可配置方式发起Ajax请求

- url 接口地址 *
- type 请求方式 *
- timeout 请求超时 *
- dataType 服务器返回格式 *
- data 发送请求数据 *
- headers 请求头
- beforeSend:function () {} 请求发起前调用 *
- success 成功响应后调用 *
- error 错误响应时调用 *
- complete 响应完成时调用（包括成功和失败） *

$.get(url,[data],[callback],[type]) 以GET方式发起Ajax请求

$.post(url,[data],[callback],[type])以POST方式发起Ajax请求

$('form').serialize()序列化表单（即格式化key=val&key=val）

## 模板引擎

插件 template_native.js   template.js

作用：渲染数据时  代替 拼接字符串的操作 

### 流行模板引擎

**BaiduTemplate**：http://tangram.baidu.com/BaiduTemplate/

**ArtTemplate**：http://aui.github.io/art-template/zh-cn/

                            https://aui.github.io/art-template/
**velocity.js**：https://github.com/shepherdwind/velocity.js/

**Handlebars**：http://handlebarsjs.com/
                          http://blog.jobbole.com/56689/

###   art-Template  插件使用

标准语法----逻辑表达式 `<% 与 %> 符号包裹起来的语句则为模板的逻辑表达式`<%=content%>

```javascript
<h1><%=value%></h1>
<ul>
    <%for(i = 0; i < arr.length; i ++) {%>
        <li>数组元素的内容 <%=i + 1%> ：<%=arr[i]%></li>
    <%}%>
</ul>
```

简洁语法---逻辑表达式：`{{` 与 `}}` 符号包裹起来的语句则为模板的逻辑表达式 {{content}}

循环遍历表达式:无论数组或者对象都可以用 each 进行遍历

```javascript
{{each list as value index}}
    <li>{{index}} - {{value.user}}</li>
{{/each}}
// 也可以简写为：
{{each list}}
    <li>{{$index}} - {{$value.user}}</li>
{{/each}}

```

函数：template(‘模板id’,数据对象)：返回动态生成的页面结构

注意事项 调用函数 template（）时，如果数据不是对象要转换成对象，y一般作为对象的属性的值例如{obj：res}，调用的函数返回的是字符串。

## 同源&跨域

### 同源

同源策略是浏览器的一种安全策略，所谓同源是指域名，协议，端口完全相同

### 跨域

不同源则是跨域

1-不允许进行DOM操作

2-不能进行ajax请求

### 服务器端跨域 CORS跨域

原理

![1535453755748](assets/1535453755748.png)

header( 'Access-Control-Allow-Origin:*' ); 

header( 'Access-Control-Allow-Origin:http://www.study.com');

### 跨域方案

前端的使用jsonp跨域

前台在发送请求的时候，要传一个函数名称过来

后台接收到这个函数名称后加（）拼接一个函数的字符串，再返回给前台，如果有参数的话，再括号里面进行拼接

前台进行调用

**1**、原理剖析

其本质是利用了<script src=""></script>标签具有可跨域的特性，由服务端返回一个预先定义好的Javascript函数的调用，并且将服务器数据以该函数参数的形式传递过来，此方法需要前后端配合完成。只能以GET方式请求

### jQuery中的JSONP

jQuery 的$.ajax() 方法当中集成了JSONP的实现，可以非常方便的实现跨域数据的访问。

dataType:'jsonp' 设置dataType值为jsonp即开启跨域访问

jsonp 可以指定服务端接收的参数的“key”值，默认为callback

jsonpCallback 可以指定相应的回调函数，默认自动生成

## XMLHttpRequest2.0

### 设置超时

1. 设置超时 xhr.timeout=3000；
2. 监听超时时间 xhr.ontimerout=function(){};

### FormData

提供了一个新的内建对象，可用于管理表单数据

1. 首先要获取一个表单元素form/DOM对象
2. 然后实例化时 new FormData（form），将表单元素from传进去  var formData=new FormData(form);
3. 会返回一个对象，此对象可以直接做为xhr.send(formData)的参数
4. 此时我们的数据就是以二进制形式传递的
5. 注意这里只能以post形式传递，浏览器会自动为我们设置一个合适的请求头，所以用2.0的不用设置请求头

###  二进制

a)    我们上传文件是以二进制形式传递的

b)    我们可以通过表单<input type=”file”>获取到一个文件对象

c)    然后this.files[0]可以获取文件信息

d)    然后再利用var formData = new FormData() 实例化

e)    然后再利用formData.append(‘upload’, this.files[0])将文件转成二进制

f)     最后将 formData 做为xhr.send(formData)的参数

### 上传进度

a)    利用XMLHttpRequest我们可以实现文件的上传

b)    并且我们可以通过xhr.upload.onprogress = function (ev) {// code}，监听上传的进度 注意的是progress事件必须写在xhr.send()之前

c)    这时我们上传的进度信息会保存在事件对象ev里

d)    ev.loaded 表示已上传的大小，ev.total表示文件整体的大小

e)    var percent = ev.loaded / ev.total



