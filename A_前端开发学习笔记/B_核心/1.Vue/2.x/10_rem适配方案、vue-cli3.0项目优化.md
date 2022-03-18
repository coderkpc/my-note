[TOC]

# 一、rem适配方案

## 1. 直接引入 rem.js 文件 

```js
  export default function () {
  // var devicePixelRatio = 1;
  // var scale = 1 / devicePixelRatio;
  // document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
  // 7.5根据设计稿的横向分辨率/100得来
  // alert(document.documentElement.clientWidth)
  var deviceWidth = document.documentElement.clientWidth;
  // var deviceWidth = window.screen.availWidth
  // alert(navigator.userAgent)
  // alert(deviceWidth)
  // console.log(navigator.userAgent)
  if (deviceWidth > 750) {
    // deviceWidth = 750;
    deviceWidth = 7.5 * 50;
  }

  document.documentElement.style.fontSize = deviceWidth / 7.5 + 'px';

  // 禁止双击放大
  document.documentElement.addEventListener('touchstart', function (event) {
    if (event.touches.length > 1) {
      event.preventDefault();
    }
  }, false);
  var lastTouchEnd = 0;
  document.documentElement.addEventListener('touchend', function (event) {
    var now = Date.now();
    if (now - lastTouchEnd <= 300) {
      event.preventDefault();
    }
    lastTouchEnd = now;
  }, false);
}

```

## 2. 淘宝的适配方案  

1. 安装依赖包  

  ```bash 
    yarn add lib-flexible -S   

    yarn add  postcss-px2rem -S  

  ```

2. 在main.js 文件中引入  

  ```js 
    import 'lib-flexible'
  ```

3. 在vue.config.js 文件中配置 css 属性

```js
module.exports = {
    css: {
        loaderOptions: {
            css: {},
            postcss: {
                plugins: [
                    require('postcss-px2rem')({
                        remUnit: 37.5
                    })
                ]
            }
        }
    }
}

```

# 二、Vue-cli3.0 项目优化

## 1. 项目优化策略

1. 生成打包报告

2. 第三方库启用 CDN 

3. ui 组件按需加载

4. 路由懒加载

5. 首屏内容定制

## 2. 项目添加 nprogress 进度条

1. 安装依赖包 

```bash 
  yarn add nprogress -S 
```

2. 在main.js 中导入依赖包和样式 

```js 
  import NProgress from 'nprogress' 
  import 'nprogress/nprogress.css'
```

3. 在axios请求拦截器和响应拦截器中配置启动和结束

```js
// axios 请求拦截
axios.interceptors.request.use(config => {
  NProgress.start()
  return config
},err => {
    Promise.reject(err.message)
})

// axios 响应拦截
axios.interceptors.response.use(function (response) {
  NProgress.done()
  return response
},err => {
    Promise.reject(err.message)
})
```

## 3. 自动移除 console.log 

1. 安装依赖包  

```bash
  yarn add babel-plugin-transform-remove-console -D 
```

2. 在 .babelrc  | babel.config.js 文件中配置 plugins 插件 

```js
const prodPlugins = []
// 生产环境移除console
if (process.env.NODE_ENV === 'production') {
  prodPlugins.push('transform-remove-console')
}
module.exports = {
  'presets': [
    '@vue/app'
  ],
  plugins: [
    [
      'import',
      {
        'libraryName': 'vant',
        'libraryDirectory': 'es',
        'style': true
      },
      'vant'
    ],
    ...prodPlugins
  ]
}
```

## 4. 生成打包报告

1. 直接在命令行添加参数  

```bash 
  vue-cli-service build --report
```

2. 通过可视化的 ui 面板查看报告（推荐）

  在可视化的ui面板中，通过***控制台***和***分析***面板， 可以方便的查看存在的问题

## 5. vue-cli3.0 webpack全局配置 vue.config.js

https://cli.vuejs.org/zh/config

## 6. 为开发模式与发布模式设置不同的打包入口

1. 配置开发入口文件 src/main-dev.js 和发布模式入口文件 src/main-prod.js

2. 通过 configureWebpack 或者 chainWebpack  来修改默认的配置

  configureWebpack 通过对象形式操作

  chainWebpack        通过链式形式操作

3. 在 vue.config.js 文件中通过 chainWebpack 修改入口文件  

```js 
module.exports = {
  chainWebpack: config => {
    // 生产模式/发布模式
    config.when(process.env.NODE_ENV === 'production', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-prod.js')
    })
    // 开发模式
    config.when(process.env.NODE_ENV === 'development', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-dev.js')
    })
  }
}
```

## 7. 通过 externals 加载外部 CDN 资源

1. 配置 vue.config.js 

```js
  chainWebpack: config => {
    // 发布模式
    config.when(process.env.NODE_ENV === 'production', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-prod.js')
      config
        .set('externals', {
         	vant: 'vant',
          vue: 'Vue',
          'vue-router': 'VueRouter',
          axios: 'axios',
          nprogress: 'NProgress',
          moment: 'moment'
        })
    })
    // 开发模式
    config.when(process.env.NODE_ENV === 'development', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-dev.js')
    })
  }

```

2. 在 index.html 文件中引入 CDN服务依赖文件  

   http://www.staticfile.org/

```html 
 <!-- nprogress 的样式表文件 -->
  <link rel="stylesheet" href="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.css" />
  <!-- vant 的样式表文件 -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/vant@2.1/lib/index.css">

  <script src="https://cdn.staticfile.org/vue/2.5.22/vue.min.js"></script>
  <script src="https://cdn.staticfile.org/vue-router/3.0.1/vue-router.min.js"></script>
  <script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
  <!-- nprogress 的 js 文件 -->
  <script src="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.js"></script>
  <!-- moment 的 js 文件 -->
  <script src="https://cdn.staticfile.org/moment.js/2.24.0/moment.min.js"></script>
  <!-- vant 的 js 文件 -->
  <script src="https://cdn.jsdelivr.net/npm/vant@2.1/lib/vant.min.js"></script>



```

## 8. 首页内容定制

1. 配置 vue.config.js 

```js 
  chainWebpack: config => {
    // 发布模式
    config.when(process.env.NODE_ENV === 'production', config => {
      // 配置入口文件
      config
        .entry('app')
        .clear()
        .add('./src/main-prod.js')
      // 配置CDN
      config
        .set('externals', {
        	vant: 'vant'
          vue: 'Vue',
          'vue-router': 'VueRouter',
          axios: 'axios',
          nprogress: 'NProgress',
          moment: 'moment'
        })
      // 配置首页定制
      config.plugin('html').tap(args => {
        args[0].isProd = true
        return args
      })
    })
    // 开发模式
    config.when(process.env.NODE_ENV === 'development', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-dev.js')
      // 配置首页定制
      config.plugin('html').tap(args => {
        args[0].isProd = false
        return args
      })
    })
  }



```

2. 在index.html中通过流程控制 是否加载CDN资源

```html
  <title><%= htmlWebpackPlugin.options.isProd ? '' : 'dev - ' %>mall-vue</title>
  <% if(htmlWebpackPlugin.options.isProd){ %>
  <title>vue-ssr</title>
  <!-- nprogress 的样式表文件 -->
  <link rel="stylesheet" href="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.css" />

  <!-- vant 的样式表文件 -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/vant@2.1/lib/index.css">

  <script src="https://cdn.staticfile.org/vue/2.5.22/vue.min.js"></script>
  <script src="https://cdn.staticfile.org/vue-router/3.0.1/vue-router.min.js"></script>
  <script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
  <!-- nprogress 的 js 文件 -->
  <script src="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.js"></script>
  <!-- moment 的 js 文件 -->
  <script src="https://cdn.staticfile.org/moment.js/2.24.0/moment.min.js"></script>
  <!-- vant 的 js 文件 -->
  <script src="https://cdn.jsdelivr.net/npm/vant@2.1/lib/vant.min.js"></script>
  <%}%>


```

## 9. 实现路由懒加载

1. 安装依赖包  

```bash 
yarn add @babel/plugin-syntax-dynamic-import -D 

```

2. 配置 babel.config.js 

```js
  plugins: [
    '@babel/plugin-syntax-dynamic-import'
  ]

```

3. 按需加载路由组件 

```js
const About = () => import(/* webpackChunkName: "about" */ './views/About.vue')


```

## 10.缓存keep-alive

缓存组件的状态和数据

```js
<keep-alive>
	<router-view></router-view>    
</keep-alive>
```

[注意：碰到有变化的组件不要拦截](https://cn.vuejs.org/v2/api/#keep-alive)

直接在路由中配置

```
{
	path:"/home",
	component:() => import(....),
	meta:{
		keepAlive:true//需要被缓存
	}
}
```

```js
<keep-alive v-if="$route.meta.keepAlive">
	<router-view></router-view>    
</keep-alive>
<router-view v-else></router-view> 
```

## 11.打包以后样式的处理

1. 运行打包后的项目文件，查看哪些样式没有生效或者被覆盖了
2. 在src中的assets文件夹重写scss的样式
3. 在main-prod.js中引入样式

# 三、多人开发

## 1. 网络请求的处理

1. 二次封装axios网络请求，默认暴露
2. 每个页面级的组件用到的api单独放一个文件 `api/home.js`，每个接口分别暴露，避免代码冲突
3. 不要挂到全局的main.js，哪里用到在哪里引入，按需引入`import {getLunbo} from './api/home.js'`

## 2. 三种开发环境

1. 生产环境

   `http://itmirror.top`

1. 开发环境

   `http://127.0.0.1:3000`

2. 测试环境

   `http://127.0.0.1:8000`

## 3. 路由的写法

1. 在router下新建一个index.js，引入vue-router，创建路由对象，==默认暴露==
2. 同级新建一个modules文件夹，每个模块的路由规则单独写成一个js文件，`router/modules/home.js`，写成一个数组然后==默认暴露==出去
3. 在index.js中直接引入，通过扩展运算符...展开到route数组中，再将routes数组挂载到路由配置里

# 四、vue-cli3 配置

## 1. vue-cli3中配置生成环境和开发环境

1. 在项目的根目录创建 .env.development 和 .env.production 环境   

  + .env.development

    ```
    # just a flag
    ENV = 'development'
    
    # base api
    VUE_APP_BASE_API = '/dev-api'
    
    ```

  + .env.production

    ```
      # just a flag
      ENV = 'production'
    
      # base api
      VUE_APP_BASE_API = '/prod-api'
    ```

  + .env.staging 

  ``` 
    NODE_ENV = production

    # just a flag
    ENV = 'staging'

    # base api
    VUE_APP_BASE_API = '/stage-api'

  ```

2. 在封装的axios网络请求中配置baseURL  

```js
const http = axios.create({
  baseURL: process.env.VUE_APP_BASE_API
})
```

3. 配置package.json文件  

```json
"scripts": {
  "dev": "vue-cli-service serve",
  "build:prod": "vue-cli-service build",
  "build:stage": "vue-cli-service build --mode staging",
}
```

## 2. vue-cli3中配置反向代理和node服务

### 2.1 proxy 

#### 方法一   

在`vue.config.js`中配置devServer的proxy属性

​	在vue.config.js中添加如下配置：

```js
devServer:{
  proxy:"http://localhost:5000"
}
```

说明：

1. 优点：配置简单，请求资源时直接发给前端（8080）即可。
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理。
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

  ```js
    module.exports = {
      devServer: {
        proxy: {
          '/api': {
            target: 'https://yys.res.netease.com',//目标地址
            ws: true, //// 是否启用websockets
            changeOrigin: true, //开启代理：在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
            pathRewrite: { '^/api': '/' }    //这里重写路径
          }
        }
      }
    }

  ```

#### 方法二

​	编写vue.config.js配置具体代理规则：

```js
module.exports = {
	devServer: {
      proxy: {
      '/api1': {// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api1': ''}
      },
      '/api2': {// 匹配所有以 '/api2'开头的请求路径
        target: 'http://localhost:5001',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      }
    }
  }
}
/*
   changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
   changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
   changeOrigin默认值为true
*/
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
2. 缺点：配置略微繁琐，请求资源时必须加前缀。

### 2.2 before  

当被调用的接口设置了身份验证（鉴权）时，手动把数据放到before里

 在vue.config.js中配置devServer的before属性

  ```js
    module.exports = {
      devServer: {
        before(app) {
          app.get('/api/data', (req, res) => {
            res.send({
              code: 1,
              msg: [
                {
                  id: 0,
                  name: 'fly'
                }
              ]
            })
          })
        }
      }
    }

  ```

