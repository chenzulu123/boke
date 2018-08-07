### Nodejs 中的 this

以下内容都是关于在 nodejs 中的 this 而非 javascript 中的 this，nodejs 中的 this 和在浏览器中 javascript 中的 this 是不一样的。

在全局中的 this

```js
console.log(this);//{}
this.num = 10;
console.log(this.num);//10;
console.log(global.num);
//undefined;
```

全局中的 this 默认是一个空对象。并且在全局中 this 与 global 对象没有任何的关系，那么全局中的 this 究竟指向的是谁？在本章节后半部分我们会讲解。

在函数中的 this

```js
function fn() {
  this.num = 10;
}
fn();
console.log(this);//{}
console.log(this.num);
//undefined;
console.log(global.num);//10;
```

在函数中 this 指向的是 global 对象，和全局中的 this 不是同一个对象，简单来说，你在函数中通过 this 定义的变量就是相当于给 global 添加了一个属性，此时与全局中的 this 已经没有关系了。

如果不相信，看下面这段代码可以证明。

```js
function fn() {
  function fn2() {
    this.age = 18;
  }
  fn2();
  console.log(this);//global;
  console.log(this.age);//18;
  console.log(global.age);//18;
}
fn();
```

对吧，在函数中 this 指向的是 global。

构造函数中的 this

```js
function Fn() {
  this.num = 998;
}
var fn = new Fn();
console.log(fn.num);
//998;
console.log(global.num);
//undefined;
```

在构造函数中 this 指向的是它的实例，而不是 global。

我们现在可以聊聊关于全局中的 this 了，说到全局中的 this，其实和 Nodejs 中的作用域有一些关系，如果你想了解 Nodejs 中关于作用域的信息可以看探讨 Nodejs 中的作用域问题。这篇文章。

回到正题，全局中的 this 指向的是 module.exports。

```js
this.num = 10;
console.log(module.exports);
{
  num: 10;
}
console.log(module.exports.num);
```

为什么在全局中 this 会指向 module.exports，那就需要先了解更多关于 module.exports 的相关知识了，暂时我们先了解到这里，后面有机会我们会聊到 module
