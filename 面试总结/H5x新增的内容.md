### H5 新增的内容

#### 1、简化的文档类型和字符集

（1）文档类型

```html
<!DOCTYPE HTML>
```

之所以如此简单，是因为 HTML5 不再是 SGML（ Standard Generalized Markup language，标准通用标记语言）的一部分，而是独立的标记语言，不需要再考虑文档类型

#### （2）字符集

```html
<meta charset="UTF-8">
```

只需要 utf-8 即可

2、富有语义化的新结构元素

```html
<header>
　　<h1>HTML5新结构<h1/>
　　<nav>导航部分</nav>
　　<p></p>
</header>
<section></section>
<footer></footer>
```

section 元素 可以认为 div 是 section 元素,一个普通的分块元素，可用来定义网站中的特定的可区别的区域

header 元素包括标题，logo，导航和其他页眉的内容，可以在网站上加多个 header，就像给内容加多个标题

hgroup 元素 即将标题进行分组的元素

footer 元素版权声明和作者信息,包涵一些链接

nav 元素主要用于主导航菜单

article 元素独立成文且以其他格式重用的内容应该置于一个 article 元素中

aside 元素用途包涵内容周围的相关内容

#### 3、新增的内联元素

```html
<figure>
    <p>图片</p>
    <img src="img1.jpg" width="100" height="100">
</figure>
```
figure 元素一个典型用途是包含图像,代码和其他内容对主要内容进行说明，删除不会影响主内容

figcaption 元素主要用于 figure 的标题

mark 元素突出显示以表示引用的内容，或者突出显示与用户当前活动相关的内容，他不同于 en 或 strong 元素

time 元素，当需要在内容中显示时间或者日期时，则建议使用 time 元素

time 元素可以包涵两个属性，一个 datetime 表示在元素中指定的确切日期和世家，pubdate 表示文章或者整个文档发布时 time 元素所指定的日期和时间

meter 元素用于定义度量衡，规定最大最小宽高，通常要结合 css 一起作用，效果如下：

技术分享

progress 元素用于定义一个进度条，有 max（完成值）和 value（进度条当前值）两个属性。

#### 4、支持动态页面
　　 1）菜单<menu>
　　用于表单中组织控件列表，常用属性如下：

技术分享

定义一个选择列表的例子：

```html
<menu>
    <li><input type="checkbox"/>a</li>
    <li><input type="checkbox"/>b</li>
     <li><input type="checkbox"/>c</li>
</menu>		<!--目前主流浏览器都不支持-->
```

2）右键菜单<menitem>
技术分享

3）在<script>标签中使用 async 属性
　　用于指定异步执行的脚本

4）<detail>元素
　　用于描述文档或文档某个部分的细节

```html
<details>
    <summary>details</summary>
    <p>用于描述文档细节<p>
</details>
```

效果：

技术分享

展开后：

技术分享

#### 5、全新的表单设计
　　 HTML5 新的 Input 类型
　　 HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证。
```
email、url、number、range、Date pickers (date, month, week, time, datetime, datetime-local)、search、color
```
　　 HTML5 的新的表单元素：
```
datalist、keygen、output
```
　　新的 form 属性：
```
autocomplete、novalidate
```
　　新的 input 属性：
```
autocomplete、autofocus、form、form overrides (formaction, formenctype, formmethod, formnovalidate, formtarget)、height 和 width、list、min, max 和 step、multiple
pattern (regexp)、placeholder、required
```
　　 autocomplete 属性规定 form 或 input 域应该拥有自动完成功能。
　　当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项：
　　实例：

```html
<form action="/example/html5/demo_form.asp" method="get" autocomplete="on">
First name:<input type="text" name="fname" /><br />
Last name: <input type="text" name="lname" /><br />
E-mail: <input type="email" name="email" autocomplete="off" /><br />
<input type="submit" />
</form>
```

<p>请填写并提交此表单，然后重载页面，来查看自动完成功能是如何工作的。</p>
6、强大的绘图功能和多媒体功能
　　1）canvas 可以动态地绘制各种效果精美的图形，结合js就能让网页图形动起来

2）SVG 绘制可伸缩的矢量图形

3）audio 和 video 可以不依赖任何插件播放音频和视频

7、打造桌面应用的一系列新功能
　　离线缓存 Web Storage（为 HTML5 开发移动应用提供了基础）

传统的 web 应用程序中，数据处理都由服务器处理，html 只负责展示数据，cookie 只能存储少量的数据，跨域通信只能通过 web 服务器。

HTML5 扩充了文件存储的能力，多达 5MB，支持 WebSQL 等轻量级数据库，可以开发支持离线 web 应用程序。

HTML5 Web Storage API 可以看做是加强版的 cookie，不受数据大小限制，有更好的弹性以及架构，可以将数据写入到本机的 ROM 中，还可以在关闭浏览器后再次打开时恢复数据，以减少网络流量。

同时，这个功能算得上是另一个方向的后台“操作记录”，而不占用任何后台资源，减轻设备硬件压力，增加运行流畅性。

8、获取地理位置信息
　　新增 Geolocation API，可以通过浏览器获取用户的地理位置，不再需要借助第三方地址数据库或专业的开发包，提供很大的方便。

9、支持多线程
　　新增 Web Worker 对象，可以在后台运行 js 脚本，也就是支持多线程，从而提高了页面加载效率。
10、history 对象
History 对象包含用户（在浏览器窗口中）访问过的 URL。
1）history.go(); 加载 history 列表中的某个具体页面。
2）history.back(); 加载 history 列表中的前一个 URL。
3）history.forward(); 加载 history 列表中的下一个 URL。
11、废弃的标签

