# jerry-interviews

帮兄弟们整理大前端面试资料🌝

## JavaScript

### 1、什么是 Async/Await？

Async - 定义异步函数(`async function someName(){...}`)

- 自动把函数转换为 Promise
- 当调用异步函数时，函数返回值会被 resolve 处理
- 异步函数内部可以使用 `await`

Await - 暂停异步函数的执行 (`var result = await someAsyncCall();`)

- 当使用在 Promise 前面时，`await` 等待 Promise 完成，并返回 Promise 的结果
- `await` **只能**和 Promise 一起使用，**不能**和 callback 一起使用
- `await` 只能用在 `async` 函数中

### 2、Async/Await 是否会取代 Promise

不会。

- Async/Await 底层依然使用了 Promise。
- 多个异步函数同时执行时，需要借助 `Promise.all`

```javascript
async function getABC() {
  let A = await getValueA(); // getValueA 花费 2 秒
  let B = await getValueB(); // getValueA 花费 4 秒
  let C = await getValueC(); // getValueA 花费 3 秒

  return A*B*C;
}
```

每次遇到 `await` 关键字时，Promise 都会停下在，一直到运行结束，所以总共花费是 2+4+3 = 9 秒。**`await` 把异步变成了同步**。

```javascript
async function getABC() {
  // Promise.all() 允许同时执行所有的异步函数
  let results = await Promise.all([ getValueA, getValueB, getValueC ]);

  return results.reduce((total, value) => total * value);
}
```

函数总耗时为 4 秒（`getValueB` 的耗时）。

### 3、Async/Await 的错误处理

在 Async/Await 语法中，我们可以使用 try/catch 进行错误处理。在 Promise 中的 `.catch()` 分支会进入 `catch` 语句。

### 引用

[10 分钟学会 JavaScript 的 Async/Await](https://github.com/dev-reading/fe/blob/master/articles/2017-11-01-javascript-async-await-explained.md)

## 网络

>⭐️[简单说说HTTP协议](http://yitaicloud.com/2018/10/12/java%20web%E5%9F%BA%E7%A1%80%E7%AF%87%E2%80%94%E2%80%94Http%E5%8D%8F%E8%AE%AE/)(详情点击阅读)

HTTP是基于请求与响应模式的、无状态的、应用层的协议，常基于TCP的连接方式。HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。

HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。HTTP协议采用了请求/响应模型。客户端向服务器发送一个请求报文，请求报文包含请求的方法、URL、协议版本、请求头部和请求数据。服务器以一个状态行作为响应，响应的内容包括协议的版本、成功或者错误代码、服务器信息、响应头部和响应数据。以下是 HTTP 请求/响应的步骤：

1. 客户端连接到Web服务器
2. 发送HTTP请求
3. 服务器接受请求并返回HTTP响应
4. 释放连接TCP连接
5. 客户端浏览器解析HTML内容

>在浏览器地址栏键入URL，按下回车之后会经历以下流程：

1、浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址；
2、解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立[TCP连接](http://www.jianshu.com/p/ef892323e68f)；
3、浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 [TCP 三次握手](http://www.jianshu.com/p/ef892323e68f)的第三个报文的数据发送给服务器；
4、服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器；
5、释放 [TCP连接](http://www.jianshu.com/p/ef892323e68f)。若connection 模式为close，则服务器主动关闭TCP连接，客户端被动关闭连接，释放TCP连接;若connection 模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求；
6、浏览器将该 html 文本并显示内容；

### 常用的HTTP方法

- GET： 用于请求访问已经被URI（统一资源标识符）识别的资源，可以通过URL传参给服务器。
- POST：用于传输信息给服务器，主要功能与GET方法类似，但一般推荐使用POST方式。
- PUT： 传输文件，报文主体中包含文件内容，保存到对应URI位置。
- DELETE：删除文件，与PUT方法相反，删除对应URI位置的文件。
- HEAD： 获得报文首部，与GET方法类似，只是不返回报文主体，一般用于验证URI是否有效。
- OPTIONS：查询相应URI支持的HTTP方法。

### GET与POST的区别

- 区别一：
  - get重点在从服务器上获取资源，post重点在向服务器发送数据；
- **区别二：**
  - get传输数据是通过URL请求，以field（字段）= value的形式，置于URL后，并用”?”连接，多个请求数据间用”&”连接，如[http://127.0.0.1/Test/login.action?name=admin&password=admin，这个过程用户是可见的；](http://127.0.0.1/Test/login.action?name=admin&password=admin%EF%BC%8C%E8%BF%99%E4%B8%AA%E8%BF%87%E7%A8%8B%E7%94%A8%E6%88%B7%E6%98%AF%E5%8F%AF%E8%A7%81%E7%9A%84%EF%BC%9B)
  - post传输数据通过Http的post机制，将字段与对应值封存在请求实体中发送给服务器，这个过程对用户是不可见的；
- **区别三：**
  - Get传输的数据量小，因为受URL长度限制，但效率较高；
  - Post可以传输大量数据，所以上传文件时只能用Post方式；
- **区别四：**
  - get是不安全的，因为URL是可见的，可能会泄露私密信息，如密码等(页面可以被缓存，或者获得历史访问记录)；
  - post较get安全性较高；
- **区别五：**
  - get方式只能支持ASCII字符，向服务器传的中文字符可能会乱码。
  - post支持标准字符集，可以正确传递中文字符。

### HTTP 1.1 版本新特性

a、默认持久连接，节省通信量，只要客户端服务端任意一端没有明确提出断开TCP连接，就一直保持连接，可以发送多次HTTP请求
b、管线化，客户端可以同时发出多个HTTP请求，而不用一个个等待响应
c、断点续传原理

### HTTP的缺点与HTTPS

a、通信使用明文不加密，内容可能被窃听
b、不验证通信方身份，可能遭到伪装
c、无法验证报文完整性，可能被篡改
HTTPS就是HTTP加上加密处理（一般是SSL安全通信线路）+认证+完整性保护

### 利用HTTP Cache来优化网站

- Last-Modified
- ETag
- Expires
- Max-age