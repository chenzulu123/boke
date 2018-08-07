## Typeof能区分的数据类型
使用typeof能区分出来的类型有以下几种：
```
1、number
2、udefined
3、string
4、boolean
5、object
6、function
```
代码样例
```js
var a = 123;
typeof a;//number

var str = 'hello javascript';
typeof str;//string

var b = undefined;
typeof b;//undefined

var c=true;
typeof c;//boolean

var d = null;
typeof d;//object

var array = [];
typeof array;//object

typeof console.log;//function

var obj = {};
typeof obj;//boject
```
### 总结
* 一般使用typeof来区分值类型和function
* 引用类型智能区分函数,对象、数组以及null都会归为object。无法具体确定是属于哪一个引用类型的