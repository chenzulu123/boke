偶遇 this 之坑
前言
在写一个懒加载插件时，遇到一个坑，就是 this 的指向问题，我想这种情况大部分人都会遇到，就写下来，新手也有个参考。

事件
有些页面图片比较多，但用户还不一定会全看，这样的话，全部去加载这些图片，就有点浪费资源了，于是想写一个通用的插件来解决这个问题

想法
根据滚动条高度加屏幕高度来判断是否加载这张图片，需要懒加载的图片这样写

```html
<img data-src="image/bg.jpg">
```

凡是加上这个属性的都会做懒加载处理。

this 之坑
以下是最终的代码
```js
function SetImg(top) {
  var imgs = Array.prototype.slice.apply(document.getElementsByTagName("img"));
  this.imgs = imgs.filter(function(item, index) {
    return item.dataset.src;
  });
  this.top = top || 150;
}
SetImg.prototype = {
  init: function() {
    this.event();
  },
  setSrc: function() {
    if (this.imgs.length === 0) {
      window.removeEventListener("scroll", this.setSrc);
    }
    var _this = this;
    this.imgs.forEach(function(item, index) {
      if (
        document.documentElement.clientHeight +
          document.body.scrollTop +
          _this.top >
          item.offsetTop ||
        item.offsetTop < document.documentElement.clientHeight
      ) {
        item.src = item.dataset.src;
        _this.imgs.splice(index, 1);
      }
    });
  },
  event: function() {
    this.setSrc = this.setSrc.bind(this);
    window.addEventListener("load", this.setSrc);
    window.addEventListener("scroll", this.setSrc);
  }
};
new SetImg().init();

```

这个代码已经解决了 this 的指向问题，也就是下面这句

```js
this.setSrc = this.setSrc.bind(this);
```
一开始是没有这句话的，但问题是，我在setSrc里面使用了this，而恰恰那里面的this指向的是window，为什么指向window？因为这句话

```js
window.addEventListener("load",this.setSrc);
window.addEventListener("scroll",this.setSrc);
```
我们说this始终指向一个对象，而现在给添加的事件，就是在window身上添加的，正如
```js
element.addEventListener("click",fn);
```
这句不就是指向element吗，那上面的那个指向window，也就不足为奇了。因此这也是为什么最后我添加这么一句
```js
this.setSrc = this.setSrc.bind(this);
```
其实一开始我没想要这么做，但这也是迫不得已的事，一开始我是这样写的：
```js
window.addEventListener("load",this.setSrc.bind(this));
window.addEventListener("scroll",this.setSrc.bind(this));
```
但这样的问题是，bind返回的是一个新对象，而不是原本的this.setSrc。一般情况下，这也不是什么大问题，但坑就坑在this.setSrc里面的这句
```js
if(this.imgs.length===0){
    window.removeEventListener("scroll",this.setSrc);
};
```
看似一切都正常，但这是一个大坑，这里面的this.setSrc指向的是SetImg.setSrc，而
```js
window.addEventListener("scroll",this.setSrc.bind(this));
```
this.setSrc.bind(this)这是一个新对象，因此你根本就无法remove掉这个新对象。所以最终才想出个迫不得已的方法就是让this.setSrc变成新的那个对象。

可能有些朋友，不太懂为什么要写这么一句：
```js
if(this.imgs.length===0){
    window.removeEventListener("scroll",this.setSrc);
};
```
这句话的作用是，如果所有图片都加载完毕了，这个滚动事件，就不需要了。当然如果你直接使用window.onscroll这种情势来写，或许这个问题可以很好的解决，但作为一个插件用addEventListener是迫不得已的，因为我不知道哪个页面使用了onscroll事件，如果我直接那样写，就会把其他人写的事件覆盖掉。

结语
这种情况出现的概率还是蛮高的，导致这种问题的出现就是，事件里面的this和构造函数里面的this，指向的是不同的对象，所以啊这就是坑点。

说到正题，这个插件还不能用T_T，再改改吧
