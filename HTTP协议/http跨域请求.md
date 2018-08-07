## HTTP 的跨域请求

###  浏览器的同源策略

什么是同源策略？

```
只有当协议、端口、和域名都相同的页面，则两个页面具有相同的源。只要网站的 协议名protocol、 主机host、 端口号port 这三个中的任意一个不同，网站间的数据请求与传输便构成了跨域调用，会受到同源策略的限制。 同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的关键的安全机制。浏览器的同源策略，出于防范跨站脚本的攻击，禁止客户端脚本（如 JavaScript）对不同域的服务进行跨站调用（通常指使用XMLHttpRequest请求）。
```

### 跨域请求方式

```
​解决跨域问题，最简单的莫过于通过nginx反向代理进行实现，但是其需要在运维层面修改，且有可能请求的资源并不再我们控制范围内（第三方），所以该方式不能作为通用的解决方案,除了nginx反向代理之外还有其他的一些方式能解决跨域请求
```

#### 方式一：图片 ping 或 script 标签跨域

```
图片ping常用于跟踪用户点击页面或动态广告曝光次数。
script标签可以得到从其他来源数据，这也是JSONP依赖的根据。
缺点：只能发送Get请求 ，无法访问服务器的响应文本（单向请求）
```

#### 方式二：JSONP 跨域

```
JSONP（JSON with Padding）是数据格式JSON的一种“使用模式”，可以让网页从别的网域要数据。根据 XmlHttpRequest 对象受到同源策略的影响，而利用 <script>元素的这个开放策略，网页可以得到从其他来源动态产生的JSON数据，而这种使用模式就是所谓的 JSONP。用JSONP抓到的数据并不是JSON，而是任意的JavaScript，用 JavaScript解释器运行而不是用JSON解析器解析。所有，通过Chrome查看所有JSONP发送的Get请求都是js类型，而非XHR。
```
缺点：
- 只能使用Get请求
- 不能注册success、error等事件监听函数，不能很容易的确定JSONP请求是否失败
- JSONP是从其他域中加载代码执行,容易受到跨站请求伪造的攻击,其安全性无法确保

#### 方式三：CORS
```
Cross-Origin Resource Sharing（CORS）跨域资源共享是一份浏览器技术的规范，提供了 Web 服务从不同域传来沙盒脚本的方法，以避开浏览器的同源策略，确保安全的跨域数据传输。现代浏览器使用CORS在API容器如XMLHttpRequest来减少HTTP请求的风险来源。与 JSONP 不同，CORS 除了 GET 要求方法以外也支持其他的 HTTP 要求。服务器一般需要增加如下响应头的一种或几种：
```
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
```
跨域请求默认不会携带Cookie信息，如果需要携带，请配置下述参数：
```
"Access-Control-Allow-Credentials": true
// Ajax设置
"withCredentials": true
```
#### 方式四：window.name+iframe
```
 window.name通过在iframe（一般动态创建i）中加载跨域HTML文件来起作用。然后，HTML文件将传递给请求者的字符串内容赋值给window.name。然后，请求者可以检索window.name值作为响应。
```
* iframe标签的跨域能力；
* window.name属性值在文档刷新后依旧存在的能力（且最大允许2M左右）。
```
每个iframe都有包裹它的window，而这个window是top window的子窗口。contentWindow属性返回<iframe>元素的Window对象。你可以使用这个Window对象来访问iframe的文档及其内部DOM。
```
#### 方式五：window.postMessage()
```
​HTML5新特性，可以用来向其他所有的 window 对象发送消息。需要注意的是我们必须要保证所有的脚本执行完才发送 MessageEvent，如果在函数执行的过程中调用了它，就会让后面的函数超时无法执行。
```