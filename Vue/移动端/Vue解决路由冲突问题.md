## 【vuejs 开发】如何在 vue 里面优雅的解决跨域，路由冲突问题！超详细

### 如何在 vue 里面优雅的解决跨域，路由冲突问题
#### 当我们在路由里面配置成以下代理可以解决跨域问题

```js
proxyTable: {
    '/goods/*': {
        target: 'http://localhost:3000'
    },
    '/users/*': {
        target: 'http://localhost:3000'
    }
},
```

这种配置方式在一定程度上解决了跨域问题，但是会带来一些问题，
比如我们的 vue 路由 也命名为 goods，这时候就会产生了冲突，
如果项目中接口很多，都在这里配置是很麻烦的，也容易产生路由冲突。

### 正确的姿势
#### 如果把所有的接口，统一规范为一个入口，在一定程度上会解决冲突
把以上配置统一前面加上 /api/

```js
proxyTable: {
    '/api/**': {
        target: 'http://localhost:3000'
    },
},
```

###### 如果我们配置成这种方式,在使用 http 请求的时候就会发生变化，会在请求前面加上一个 api，相对路由也会发生变化，也会在接口前面加上 api，这样也会很麻烦,我们可以使用以下方式来解决这个问题

```js
proxyTable: {
    '/api/**': {
        target: 'http://localhost:3000',
        pathRewrite:{
                    '^/api':'/'
        }
    },
},
```

##### 上面这个代码，就是把咱们虚拟的这个 api 接口，去掉，此时真正去后端请求的时候，不会加上 api 这个前缀了，那么这样我们前台 http 请求的时候，还必须加上 api 前缀才能匹配到这个代理,代码如下：

```js
logout(){
    axios.post('/api/users/logout').then(result=>{
        let res = result.data;
        this.nickName = '';
        console.log(res);
        })
},
getGoods(){
    axios.post('/api/goods/list').then(result=>{
        let res = result.data;
        this.nickName = '';
            console.log(res);
    })
}
```

##### 我们可以利用 axios 的 baseUrl 直接默认值是 api，这样我们每次访问的时候，自动补上这个 api 前缀，就不需要我们自己手工在每个接口上面写这个前缀了
在入口文件里面配置如下：

```js
import Axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, Axios);
Axios.defaults.baseURL = "api";
```

如果这配置 'api/' 会默认读取本地的域
##### 上面这样配置的话，不会区分生产和开发环境
在 config 文件夹里面新建一个 api.config.js 配置文件

```js
const isPro = Object.is(process.env.NODE_ENV, "production");

module.exports = {
  baseUrl: isPro ? "http://www.vnshop.cn/api/" : "api/"
};
```

然后在 main.js 里面引入,这样可以保证动态的匹配生产和开发的定义前缀

```js
import apiConfig from "../config/api.config";

import Axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, Axios);
Axios.defaults.baseURL = apiConfig.baseUrl;
```

##### 经过上面配置后，在 dom 里面可以这样轻松的访问,也不需要在任何组件里面引入 axios 模块了。

```js
logout(){
    this.$http.post('/users/logout').then(result=>{
        let res = result.data;
        this.nickName = '';
        console.log(res);
    })
},
getGoods(){
    this.$http.post('/goods/list').then(result=>{
        let res = result.data;
        this.nickName = '';
        console.log(res);
    })
}
```

#### 最终代码
##### 在代理里面配置

```js
proxyTable: {
    '/api/**': {
        target: 'http://localhost:3000',
        pathRewrite:{
            '^/api':'/'
        }
    },
},
```

在 config 里面的 api.config.js 配置
在 config 文件夹里面新建一个 api.config.js 配置文件

```js
const isPro = Object.is(process.env.NODE_ENV, "production");
module.exports = {
  baseUrl: isPro ? "http://www.vnshop.cn/api/" : "api/"
};
```

关于生产和开发配置不太了解
可以去 dev-server.js 里面看配置代码

```js
const webpackConfig =
  process.env.NODE_ENV === "testing" || process.env.NODE_ENV === "production"
    ? require("./webpack.prod.conf")
    : require("./webpack.dev.conf");
```

在 main.js 入口文件里面配置

```js
import apiConfig from "../config/api.config";
import Axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, Axios);
Axios.defaults.baseURL = apiConfig.baseUrl;
```

在 dom 里面请求 api 的姿势

```js
logout(){
    this.$http.post('/users/logout').then(result=>{
        let res = result.data;
        this.nickName = '';
        console.log(res);
    })
},
getGoods(){
    this.$http.post('/goods/list').then(result=>{
        let res = result.data;
        this.nickName = '';
         console.log(res);
    })
}
```
