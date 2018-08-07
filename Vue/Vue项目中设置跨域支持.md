## Vue 项目设置跨域请求

#### 1、在使用 vue 开发的时候经常要涉及到跨域的问题，其实在 vue cli 中是有我们设置跨域请求的文件的。

#### 2、当跨域无法请求的时候我们可以修改工程下 config 文件夹下的 index.js 中的 dev:{}部分。

```js
dev: {
    env: require('./dev.env'),
    port: 8080,
    autoOpenBrowser: false,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
        '/api': {
            target: 'http://api.douban.com/v2',
            changeOrigin: true,
            pathRewrite: {
                '^/api': ''
            }
        }
    },
    cssSourceMap: false
}
```

将 target 设置为我们需要访问的域名。

#### 3、然后在 main.js 中设置全局属性：

```js
Vue.prototype.HOST = '/api'
```

#### 4、至此，我们就可以在全局使用这个域名了，如下：

```js
var url = this.HOST + "/movie/in_theaters";
this.$http.get(url).then(
  res => {
    this.movieList = res.data.subjects;
  },
  res => {
    console.info("调用失败");
  }
);
```
