# 简介
1999年，微软公司发布IE浏览器5.0版，第一次引入新功能：允许JavaScript脚本向服务器发起HTTP请求。
2005年2月，AJAX这个词第一次正式提出，指围绕这个功能进行开发的一整套做法。从此，AJAX成为脚本发起HTTP通信的代名词，W3C也在2006年发布了它的国际标准。   
# AJAX 步骤
> 1. 创建AJAX对象
> 2. 发出HTTP请求
> 3. 接受服务器传回的数据
> 4. 更新网页数据

总结：AJAX通过原生的XMLHttpRequest对象发出HTTP请求，得到服务器返回的数据后，再进行处理。    
AJAX可以是同步请求，也可以是异步请求。但是，大多数情况下，特指异步请求。因为同步的Ajax请求，对浏览器有“堵塞效应”。

# XMLHttpRequest对象
XMLHttpRequest对象用来在浏览器与服务器之间传送数据
## 典型用法
```js
    var xhr = new XMLHttpRequest();

    // 指定通信过程中状态改变时的回调函数
    xhr.onreadystatechange = function(){
    // 通信成功时，状态值为4
    if (xhr.readyState === 4){
        if (xhr.status === 200){
        console.log(xhr.responseText);
        } else {
        console.error(xhr.statusText);
        }
    }
    };

    xhr.onerror = function (e) {
    console.error(xhr.statusText);
    };

    // open方式用于指定HTTP动词、请求的网址、是否异步
    xhr.open('GET', '/endpoint', true);

    // 发送HTTP请求,括号里写发送的参数
    xhr.send(null);
```
## XMLHttpRequest实例的属性
onreadystatechange: 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。    
readystate:存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。
- 0: 请求未初始化
- 1: 服务器连接已建立
- 2: 请求已接收
- 3: 请求处理中
- 4: 请求已完成，且响应已就绪

status: 	
- 200, OK，访问正常
- 301, Moved Permanently，永久移动
- 302, Move temporarily，暂时移动
- 304, Not Modified，未修改
- 307, Temporary Redirect，暂时重定向
- 401, Unauthorized，未授权
- 403, Forbidden，禁止访问
- 404, Not Found，未发现指定网址
- 500, Internal Server Error，服务器发生错误
## open()
XMLHttpRequest对象的open方法用于指定发送HTTP请求的参数，它的使用格式如下，一共可以接受五个参数。
```js
void open(
   string method,
   string url,
   optional boolean async,
   optional string user,
   optional string password
);
```
>method：表示HTTP动词，比如“GET”、“POST”、“PUT”和“DELETE”。    
>url: 表示请求发送的网址。    
>async: 格式为布尔值，默认为true(异步)，表示请求是否为异步。如果设为false，则send()方法只有等到收到服务器返回的结果，才会有返回值。    
>user：表示用于认证的用户名，默认为空字符串。    
>password：表示用于认证的密码，默认为空字符串
## send()
send方法用于实际发出HTTP请求。如果不带参数，就表示HTTP请求只包含头信息，也就是只有一个URL，典型例子就是GET请求；如果带有参数，就表示除了头信息，还带有包含具体数据的信息体，典型例子就是POST请求。

注意，所有XMLHttpRequest的监听事件，都必须在send()方法调用之前设定。

send方法的参数就是发送的数据。多种格式的数据，都可以作为它的参数。
```js
void send();
void send(ArrayBufferView data);
void send(Blob data);
void send(Document data);
void send(String data);
void send(FormData data);
```
## withCredentials
withCredentals属性是一个布尔值，表示跨域请求时，用户信息（比如Cookie和认证的HTTP头信息）是否会包含在请求之中，默认为false。即向example.com发出跨域请求时，不会发送example.com设置在本机上的Cookie（如果有的话）。
如果你需要通过跨域AJAX发送Cookie，需要打开withCredentials。

    xhr.withCredentials = true;
为了让这个属性生效，服务器必须显式返回Access-Control-Allow-Credentials这个头信息。

    Access-Control-Allow-Credentials: true
.withCredentials属性打开的话，不仅会发送Cookie，还会设置远程主机指定的Cookie。注意，此时你的脚本还是遵守同源政策，无法 从document.cookie或者HTTP回应的头信息之中，读取这些Cookie。

# 文件上传
HTML网页的<form>元素能够以四种格式，向服务器发送数据。
>使用POST方法，将enctype属性设为application/x-www-form-urlencoded，这是默认方法
```js
<form action="register.php" method="post" onsubmit="AJAXSubmit(this); return false;"></form>
```
>使用POST方法，将enctype属性设为text/plain。
```js
<form action="register.php" method="post" enctype="text/plain" onsubmit="AJAXSubmit(this); return false;"></form>
```
>使用POST方法，将enctype属性设为multipart/form-data。
```js
<form action="register.php" method="post" enctype="multipart/form-data" onsubmit="AJAXSubmit(this); return false;"></form>
```
>使用GET方法，enctype属性将被忽略。
```js
<form action="register.php" method="get" onsubmit="AJAXSubmit(this); return false;"></form>
```
某个表单有两个字段，分别是foo和baz，其中foo字段的值等于bar，baz字段的值一个分为两行的字符串。上面四种方法，都可以将这个表单发送到服务器。  

## 使用file控件实现文件上传
```js
<form id="file-form" action="handler.php" method="POST">
  <input type="file" id="file-select" name="photos[]" multiple/>
  <button type="submit" id="upload-button">上传</button>
</form>
```
multiple属性，指定可以一次选择多个文件；如果没有这个属性，则一次只能选择一个文件    
file对象的files属性，返回一个FileList对象，包含了用户选中的文件

```js
    var fileSelect = document.getElementById('file-select');
    var files = fileSelect.files;

//新建一个FormData对象的实例，用来模拟发送到服务器的表单数据，把选中的文件添加到这个对象上面。
    var formData = new FormData();
    for (var i = 0; i < files.length; i++) {
    var file = files[i];
    if (!file.type.match('image.*')) {
        continue;
    }
    formData.append('photos[]', file, file.name);
    }

    //最后，使用Ajax方法向服务器上传文件。
    var xhr = new XMLHttpRequest();
    xhr.open('POST', 'handler.php', true);
    xhr.onload = function () {
    if (xhr.status !== 200) {
        alert('An error occurred!');
    }
    };
    xhr.send(formData);
```
FormData对象的append方法，除了可以添加文件，还可以添加二进制对象（Blob）或者字符串。
```js
// Files
formData.append(name, file, filename);
// Blobs
formData.append(name, blob, filename);
// Strings
formData.append(name, value);    
```
<span style="color:red">append方法的第一个参数是表单的控件名，第二个参数是实际的值，第三个参数是可选的，通常是文件名。</span>
## File API上传
```js
var file = document.getElementById('test-input').files[0];
var xhr = new XMLHttpRequest();

xhr.open('POST', 'myserver/uploads');
xhr.setRequestHeader('Content-Type', file.type);
xhr.send(file);
```