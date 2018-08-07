## JS 中深拷贝的几种实现方法

### 1、使用递归的方式实现深拷贝

```js
//使用递归的方式实现数组、对象的深拷贝
function deepClone1(obj) {
  //判断拷贝的要进行深拷贝的是数组还是对象，是数组的话进行数组拷贝，对象的话进行对象拷贝
  var objClone = Array.isArray(obj) ? [] : {};
  //进行深拷贝的不能为空，并且是对象或者是
  if (obj && typeof obj === "object") {
    for (key in obj) {
      if (obj.hasOwnProperty(key)) {
        if (obj[key] && typeof obj[key] === "object") {
          objClone[key] = deepClone1(obj[key]);
        } else {
          objClone[key] = obj[key];
        }
      }
    }
  }
  return objClone;
}
```

### 2、通过 JSON 对象实现深拷贝

```js
//通过js的内置对象JSON来进行数组对象的深拷贝
function deepClone2(obj) {
  var _obj = JSON.stringify(obj),
    objClone = JSON.parse(_obj);
  return objClone;
}
```
JSON对象实现深拷贝的一些问题
* 无法实现对对象中方法的深拷贝
### 3、通过jQuery的extend方法实现深拷贝
```js
var array = [1,2,3,4];
var newArray = $.extend(true,[],array);
```

### 4、Object.assign()拷贝 
```
当对象中只有一级属性，没有二级属性的时候，此方法为深拷贝，但是对象中有对象的时候，此方法，在二级属性以后就是浅拷贝。
```
### 5、lodash函数库实现深拷贝
```
lodash很热门的函数库，提供了 lodash.cloneDeep()实现深拷贝
```

