### 彻底理解 js 中 this 的指向，不必硬背。

首先必须要说的是，this 的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定 this 到底指向谁，实际上 this 的最终指向的是那个调用它的对象（这句话有些问题，后面会解释为什么会有问题，虽然网上大部分的文章都是这样说的，虽然在很多情况下那样去理解不会出什么问题，但是实际上那样理解是不准确的，所以在你理解 this 的时候会有种琢磨不透的感觉），那么接下来我会深入的探讨这个问题。

为什么要学习 this？如果你学过面向对象编程，那你肯定知道干什么用的，如果你没有学过，那么暂时可以不用看这篇文章，当然如果你有兴趣也可以看看，毕竟这是 js 中必须要掌握的东西。

例子 1：

```js
function a() {
  var user = "追梦子";
  console.log(this.user); //undefined
  console.log(this); //Window
}
a();
```

按照我们上面说的 this 最终指向的是调用它的对象，这里的函数 a 实际是被 Window 对象所点出来的，下面的代码就可以证明。

```js
function a() {
  var user = "追梦子";
  console.log(this.user); //undefined
  console.log(this); //Window
}
window.a();
```

和上面代码一样吧，其实 alert 也是 window 的一个属性，也是 window 点出来的。

例子 2：

```js
var o = {
  user: "追梦子",
  fn: function() {
    console.log(this.user); //追梦子
  }
};
o.fn();
```

这里的 this 指向的是对象 o，因为你调用这个 fn 是通过 o.fn()执行的，那自然指向就是对象 o，这里再次强调一点，this 的指向在函数创建的时候是决定不了的，在调用的时候才能决定，谁调用的就指向谁，一定要搞清楚这个。

其实例子 1 和例子 2 说的并不够准确，下面这个例子就可以推翻上面的理论。

如果要彻底的搞懂 this 必须看接下来的几个例子

本文出处：追梦子博客

例子 3：

```js
var o = {
  user: "追梦子",
  fn: function() {
    console.log(this.user); //追梦子
  }
};
window.o.fn();
```

这段代码和上面的那段代码几乎是一样的，但是这里的 this 为什么不是指向 window，如果按照上面的理论，最终 this 指向的是调用它的对象，这里先说个而外话，window 是 js 中的全局对象，我们创建的变量实际上是给 window 添加属性，所以这里可以用 window 点 o 对象。

这里先不解释为什么上面的那段代码 this 为什么没有指向 window，我们再来看一段代码。

```js
var o = {
  a: 10,
  b: {
    a: 12,
    fn: function() {
      console.log(this.a); //12
    }
  }
};
o.b.fn();
```

这里同样也是对象 o 点出来的，但是同样 this 并没有执行它，那你肯定会说我一开始说的那些不就都是错误的吗？其实也不是，只是一开始说的不准确，接下来我将补充一句话，我相信你就可以彻底的理解 this 的指向的问题。

情况 1：如果一个函数中有 this，但是它没有被上一级的对象所调用，那么 this 指向的就是 window，这里需要说明的是在 js 的严格版中 this 指向的不是 window，但是我们这里不探讨严格版的问题，你想了解可以自行上网查找。

情况 2：如果一个函数中有 this，这个函数有被上一级的对象所调用，那么 this 指向的就是上一级的对象。

情况 3：如果一个函数中有 this，这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this 指向的也只是它上一级的对象，例子 3 可以证明，如果不相信，那么接下来我们继续看几个例子。

```js
var o = {
  a: 10,
  b: {
    // a:12,
    fn: function() {
      console.log(this.a); //undefined
    }
  }
};
o.b.fn();
```

尽管对象 b 中没有属性 a，这个 this 指向的也是对象 b，因为 this 只会指向它的上一级对象，不管这个对象中有没有 this 要的东西。

还有一种比较特殊的情况，例子 4：

```js
var o = {
  a: 10,
  b: {
    a: 12,
    fn: function() {
      console.log(this.a); //undefined
      console.log(this); //window
    }
  }
};
var j = o.b.fn;
j();
```

这里 this 指向的是 window，是不是有些蒙了？其实是因为你没有理解一句话，这句话同样至关重要。

this 永远指向的是最后调用它的对象，也就是看它执行的时候是谁调用的，例子 4 中虽然函数 fn 是被对象 b 所引用，但是在将 fn 赋值给变量 j 的时候并没有执行所以最终指向的是 window，这和例子 3 是不一样的，例子 3 是直接执行了 fn。

this 讲来讲去其实就是那么一回事，只不过在不同的情况下指向的会有些不同，上面的总结每个地方都有些小错误，也不能说是错误，而是在不同环境下情况就会有不同，所以我也没有办法一次解释清楚，只能你慢慢地的去体会。

构造函数版 this：

```js
function Fn() {
  this.user = "追梦子";
}
var a = new Fn();
console.log(a.user); //追梦子
```

这里之所以对象 a 可以点出函数 Fn 里面的 user 是因为 new 关键字可以改变 this 的指向，将这个 this 指向对象 a，为什么我说 a 是对象，因为用了 new 关键字就是创建一个对象实例，理解这句话可以想想我们的例子 3，我们这里用变量 a 创建了一个 Fn 的实例（相当于复制了一份 Fn 到对象 a 里面），此时仅仅只是创建，并没有执行，而调用这个函数 Fn 的是对象 a，那么 this 指向的自然是对象 a，那么为什么对象 a 中会有 user，因为你已经复制了一份 Fn 函数到对象 a 中，用了 new 关键字就等同于复制了一份。

除了上面的这些以外，我们还可以自行改变 this 的指向，关于自行改变 this 的指向请看 JavaScript 中 call,apply,bind 方法的总结这篇文章，详细的说明了我们如何手动更改 this 的指向。

更新一个小问题当 this 碰到 return 时

```js
function fn() {
  this.user = "追梦子";
  return {};
}
var a = new fn();
console.log(a.user); //undefined
```

再看一个

```js
function fn() {
  this.user = "追梦子";
  return function() {};
}
var a = new fn();
console.log(a.user); //undefined
```

再来

```js
function fn() {
  this.user = "追梦子";
  return 1;
}
var a = new fn();
console.log(a.user); //追梦子
function fn() {
  this.user = "追梦子";
  return undefined;
}
var a = new fn();
console.log(a.user); //追梦子
```

什么意思呢？

如果返回值是一个对象，那么 this 指向的就是那个返回的对象，如果返回值不是一个对象那么 this 还是指向函数的实例。

```js
function fn() {
  this.user = "追梦子";
  return undefined;
}
var a = new fn();
console.log(a); //fn {user: "追梦子"}
```

还有一点就是虽然 null 也是对象，但是在这里 this 还是指向那个函数的实例，因为 null 比较特殊。

```js
function fn() {
  this.user = "追梦子";
  return null;
}
var a = new fn();
console.log(a.user); //追梦子
```

知识点补充：

1.在严格版中的默认的 this 不再是 window，而是 undefined。

2.new 操作符会改变函数 this 的指向问题，虽然我们上面讲解过了，但是并没有深入的讨论这个问题，网上也很少说，所以在这里有必要说一下。

```js
function fn() {
  this.num = 1;
}
var a = new fn();
console.log(a.num); //1
```

为什么 this 会指向 a？首先 new 关键字会创建一个空的对象，然后会自动调用一个函数 apply 方法，将 this 指向这个空对象，这样的话函数内部的 this 就会被这个空的对象替代。


注意: 当你 new 一个空对象的时候,js 内部的实现并不一定是用的 apply 方法来改变 this 指向的,这里我只是打个比方而已.

if (this === 动态的\可改变的) return true;
