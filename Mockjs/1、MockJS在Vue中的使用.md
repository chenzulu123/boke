### MockJS 的使用

#### 1、创建mock.js并在 
mock.js
```js
//引入 mockjs 模块
import Mock from "mockjs";
Mock.mock("getNewsList", {
  "list|5": [
    //随机返回5项
    {
      title: "@ctitle(5,20)",
      url: "@url" //随机生成url
    }
  ]
});
```

#### 2、在main.js中引入自定义的 mock.js

```js
require("./mock");
//此部分引入的是我们所编写的mockjs文档
```

#### 3、一般我们使用mockjs 的时候模拟后台接口时会配合使用 axios模块来接管vue中的ajax 请求工具。

//引入 axios 并且将 axios 设置为默认的 ajax 请求的公户

```js
import axios from "axios";
Vue.prototype.$http = axios;
```

#### 4、请求数据

```js
this.$http
  .get("getNewsList")
  .then(res => {
    this.newsList = res.data.list;
  })
  .catch(err => {
    // alert(2);
    console.log(err);
  });
```
