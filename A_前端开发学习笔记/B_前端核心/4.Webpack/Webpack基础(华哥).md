## 一、webpack 基本使用

### 1. 全局配置(不推荐)

① 新建项目空白目录，并运行` npm init –y` 命令，初始化包管理配置文件 package.json

② 新建 src 源代码目录

③ 新建 src -> index.js 入口文件

④ 安装webpack全局依赖包，`npm install webpack@4.44.1 webpack-cli -g `

⑤ 在当前项目根目录运行 webpack 命令

> webpack 的 4.x 版本中默认约定：
> 打包的入口文件为 src  -> index.js
> 打包的输出文件为 dist -> main.js

### 2. 局部配置

① 运行` npm install webpack@4.44.1 webpack-cli@3.3.12 –D `命令，安装 webpack 相关的包

② 在项目根目录中，创建名为 webpack.config.js 的 webpack 配置文件

③ 在 webpack 的配置文件中，初始化如下基本配置：

```js
module.exports = {
  mode: 'development' // mode 用来指定构建模式
}
```

④在 package.json 配置文件中的 scripts 节点下，新增 build 脚本如下：

```js
"scripts": {
	"build": "webpack" // script 节点下的脚本，
}
```

⑤在终端中运行 `npm run build` 或 `yarn build `命令，启动 webpack 进行项目打包。  

### 3.单入口/出口

```js
const path = require('path') // 导入 node.js 中专门操作路径的模块
module.exports = {
  entry: path.join(__dirname, './src/index.js'), // 打包入口文件的路径
  output: {
    path: path.join(__dirname, './dist'), // 输出文件的存放路径
    filename: 'build.js' // 输出文件的名称
  }
}
```

### 4.多入口/出口

```js
const path = require('path') // 导入 node.js 中专门操作路径的模块
module.exports = {
  entry: {
    index: path.join(__dirname, './src/index.js'), // 打包入口文件的路径         
    home: path.join(__dirname, './src/home.js'),   // 打包入口文件的路径
  }
  output: {
    path: path.join(__dirname, './dist'), // 输出文件的存放路径
    filename: '[name].js' // 根据输入文件的名称决定输出文件的名称
  }
}
```

## 二、webpack 打包 html

① 运行` npm install html-webpack-plugin@4.3.0 –D` 命令，安装生成预览页面的插件

② 修改 webpack.config.js 文件头部区域，添加如下配置信息：

```js
// 导入生成预览页面的插件，得到一个构造函数
const HtmlWebpackPlugin = require('html-webpack-plugin')
const htmlPlugin = new HtmlWebpackPlugin({ // 创建插件的实例对象
  template: './src/index.html', // 指定要用到的模板文件
  filename: 'index.html' // 指定生成的文件的名称，该文件存在于内存中，在目录中不显示
})
```

③ 修改 webpack.config.js 文件中向外暴露的配置对象，新增如下配置节点：

```js
module.exports = {
  plugins: [ htmlPlugin ] // plugins 数组是 webpack 打包期间会用到的一些插件列表
}
```



## 三、webpack 自动打包

### 1.如何使用

① 运行 `npm install webpack-dev-server@3.11.0 –D`命令，安装支持项目自动打包的工具

② 修改 package.json -> scripts 中的 dev 命令如下：

```js
"scripts": {
  "dev": "webpack-dev-server" // script 节点下的脚本，可以通过 npm run 执行
}
```

③ 在 dist下 新建 index.html ，并使用script 引入 "/build.js“

④ 运行 npm run dev 命令，重新进行打包

⑤ 在浏览器中访问 [http://localhost:8080](http://localhost:8080/) 地址，查看自动打包效果

> 注意：
> webpack-dev-server 会启动一个实时打包的 http 服务器
> webpack-dev-server 打包生成的输出文件，默认放到了项目根目录中，而且是虚拟的、看不见的

### 2.配置参数

```js
// package.json中的配置
// --open 打包完成后自动打开浏览器页面
// --host 配置 IP 地址
// --port 配置端口
// --mode 配置打包方式 只有 production 生产 和 development 开发 两种环境
"scripts": {
    "dev": "webpack-dev-server --open --host 127.0.0.1 --port 8888"
},

```



## 四、webpack 加载器

### 1.通过 loader 打包非 js 模块

在实际开发过程中，webpack 默认只能打包处理以 .js 后缀名结尾的模块，其他`非.js后缀名结尾的模块`，webpack 默认处理不了，`需要调用 loader 加载器`才可以正常打包，否则会报错！

> loader 加载器可以协助 webpack 打包处理特定的文件模块，比如：
> less-loader 可以打包处理 .less 相关的文件
> sass-loader 可以打包处理 .scss 相关的文件
> url-loader  可以打包处理 css 中与 url 路径相关的文件

### 2.webpack 中加载器的基本使用

① **打包时处理css文件** 

- 运行` npm i style-loader [需要处理的文件格式]-loader -D `命令，安装处理 css 文件的 loader
- 在 webpack.config.js modulerules数组中，添加 规则如下：

```js
// 所有第三方文件模块的匹配规则
module: {
    rules: [
        { test: /\.css$/, use: ['style-loader', 'css-loader'] },
        { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
        { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] },
        { test: /\.styl$/, use: ['style-loader', 'css-loader', 'stylus-loader'] }
    ]
}

```

-  在入口文件中引入对应的css
- 运行webpack-dev-server

>其中，test 表示匹配的文件类型， use 表示对应要调用的 loader
>注意：
>use 数组中指定的 loader 顺序是固定的
>多个 loader 的调用顺序是：从后往前调用

② **打包时将css文件打包成一个单独的文件**

- 运行 `npm i mini-css-extract-plugin -D `命令
- 在 webpack.config.js 的 plugins选项中添加以下插件

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin') 
plugins: [
    new MiniCssExtractPlugin({
        filename: 'static/css/[name].[chunkhash:8].css',
    })
]

```

- 在webpack.config.js 的module中替换规则

```js
rules: [
    { test: /\.css$/, use: [MiniCssExtractPlugin.loader, 'css-loader'] }
]
```

③ **打包css文件时自动添加兼容前缀**

- 运行` npm i postcss-loader autoprefixer -D` 命令

- 在项目根目录中创建 postcss 的配置文件 postcss.config.js，并初始化如下配置：

```js
const autoprefixer = require('autoprefixer') // 导入自动添加前缀的插件
module.exports = {
    plugins: [ autoprefixer ] // 挂载插件
}
```

- 在 webpack.config.js 的 module -> rules 数组中，修改 css 的 loader 规则如下：

```js
module: {
    rules: [
        // postcss-loader 要添加在less-loader之前
        { test:/\.less$/, use: ['style-loader', 'css-loader', 'postcss-loader','less-loader']}
    ]
}
```

④ **打包处理 js 文件中的高级语法**  

- 安装babel转换器相关的包：`npm i babel-loader @babel/core @babel/runtime -D`
- 安装babel语法插件相关的包：`npm i @babel/preset-env @babel/plugin-transform-runtime –D`
- 这个包不好安装 建议单独安装 `yarn add @babel/plugin-proposal-class-properties --dev`
- 在项目根目录中，创建 <u>babel 配置文件 babel.config.js</u> 并初始化基本配置如下：

```js
module.exports = {
    presets: [ '@babel/preset-env' ],
    plugins: [ '@babel/plugin-transform-runtime', '@babel/plugin-proposal-class-properties' ]
}
```

- 在 <u>webpack.config.js</u> 的 module -> rules 数组中，添加 loader 规则如下：

```js
// exclude 为排除项，表示 babel-loader 不需要处理 node_modules 中的 js 文件
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
```

## 五、webpack 自动清除构建文件夹

①运行 npm install clean-webpack-plugin –D 命令，安装生成预览页面的插件

②修改 webpack.config.js 文件头部区域，添加如下配置信息：

```js
// 导入自动清除构建文件夹的包
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
```

③修改 webpack.config.js 文件中向外暴露的配置对象，新增如下配置节点：

```js
module.exports = {
  plugins: [ new CleanWebpackPlugin ] 
}
```

## 六、webpack 配置vue和jsx

### 1. **配置vue**  

① 运行 yarn add vue-loader vue-template-compiler -D 命令

② 修改 webpack.config.js module中的规则

```js
const { VueLoaderPlugin } = require('vue-loader')
plugins: [
   new VueLoaderPlugin()
]  
module: {
    rules: [
      { test: /\.vue$/, use: 'vue-loader' } // 处理 .vue 文件的 loader
    ]
  }
```

### 2. 配置jsx

① 运行 npm install --save-dev @babel/preset-react –D 命令，安装jsx解析包

② 修改 webpack.config.js module中的规则

```js
{ test: /\.js|jsx$/, use: 'babel-loader', exclude: /node_modules/ }
```

③ 修改 babel.config.js 文件中向外暴露的配置对象，新增如下配置节点：

```js
module.exports = {
    presets: ['@babel/preset-react']
}
```



















