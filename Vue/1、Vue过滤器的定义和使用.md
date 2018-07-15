### Vue 中过滤器的使用和定义

###  过滤器的定义

#### 1、全局过滤器的定义

mian.js 中进行定义

```js
Vue.filter("moneyFilter", function(value, num, type) {
  //value是使用过滤器是的表达式或者值,num和tyep中的参数
  return "￥" + value.toFixed(num) + type;
});
```

过滤器的参数解析

```js
moneyFilter表示的是过滤器的名称
函数中的参数解析：
value是通过管道传来的数据
num在调用过滤器时在圆括号中第一个参数
type在调用过滤器时在圆括号中第二个参数
```

#### 2、文件组件  中(局部 )过滤器的定义

Vue 文件组件中定义

```js
//文件组件内的自定义过滤器
  filters: {
    moneyFilter: function(value, num, type) {
     return "￥" + value.toFixed(num) + type;
    },
    capitalize: function (value) {
    return 'id'+value+'test';
  }
  }
```

过滤器的参数解析

```js
moneyFilter过滤器名称;
myInput是通过管道传来的数据;
num在调用过滤器时在圆括号中第一个参数;
type在调用过滤器时在圆括号中第二个参数;
```

#### 3、通过文件定义的全局过滤器

customFilter.js

```js
//日期格式过滤器
let dateServer = value => {
  return value.replace(/(\d{4})(\d{2})(\d{2})/g, "$1-$2-$3");
};
//金额格式过滤器
let moneyFilter = (value, num, type) => {
  return "￥" + value.toFixed(num) + type;
};
//时间格式过滤器
let dateFilter = (input, format = "yyyy-MM-dd hh:mm:ss") => {
  var o = {
    "M+": input.getMonth() + 1, //月份
    "d+": input.getDate(), //日
    "h+": input.getHours(), //小时
    "m+": input.getMinutes(), //分
    "s+": input.getSeconds(), //秒
    "q+": Math.floor((input.getMonth() + 3) / 3), //季度
    S: input.getMilliseconds() //毫秒
  };
  if (/(y+)/.test(format))
    format = format.replace(
      RegExp.$1,
      (input.getFullYear() + "").substr(4 - RegExp.$1.length)
    );
  // 不够2位的前面补0
  for (var k in o)
    if (new RegExp("(" + k + ")").test(format))
      format = format.replace(
        RegExp.$1,
        RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length)
      );
  return format;
};
export { dateServer, moneyFilter, dateFilter };
```

main.js 中注册  全局自定义指令

```js
//引入文件
import * as custom from "./common/filters/customFilter";
//注册自定义过滤器
Object.keys(custom).forEach(key => {
  Vue.filter(key, custom[key]);
});
```

#### 4、过滤器的使用

在双花括号中使用

```html
<div>{{money|moneyFilter(2,'元')}}</div>
```

```
money是通过挂到传递的值或者是表达式
moneyFilter是过滤器名称
2是参数1
'元'是参数2
```

在 `v-bind` 中使用
不带参数的

```html
<div v-bind:id="id | capitalize"></div>
```

带参数的

```html
<div :id="id |capitalize(12)">我是过滤器</div>
```

```js
id是过滤器中传递的值;
capitalize是过滤器名称;
capitalize括号中的是过滤器传递的参数;
```

总结：
vue1.*版本是有内置的过滤器，但是在 vue2.*所有的版本都已经没有自带的过滤器了。Vue.js 允许自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：双花括号插值和 v-bind 表达式。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示：
