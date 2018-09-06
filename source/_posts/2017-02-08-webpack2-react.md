---
title: webpack2 & react
date: 2017-02-08 17:32:41
tags:
categories:
description:
---



# webpack + React 搭建开发环境


webpack是当前如日中天的打包工具，React官方指定的搭配产品。这里记录一下具体的操作步骤。

## 使用方法
### 1. 初始化项目

```bash
mkdir react_tutorial
// 文件夹的名字是可以随便取的
cd react_tutorial
npm init
```
<!-- more -->

### 2. 安装webpack和webpack-dev-server

``` bash
npm install webpack@2.2.0 webpack-dev-server@2.2.0 html-webpack-plugin  --save-dev
```

`html-webpack-plugin` 插件的作用是动态生成一个网页文件。
`webpack-dev-server` 模块的作用是创建一个开发服务器，用来观看效果，服务器基于 *express*



### 3. 安装Babel
`Babel`是一个javascript编译器，可以将es6或者jsx的代码编译为浏览器可识别的代码，这里需要用到babel的核心模块和两个预设器（preset）。

```bash
//安装babel核心模块和babel加载器
npm install babel-core babel-loader --save-dev
//安装es6预设器和react中jsx的预设器
npm install babel-preset-es2015 babel-preset-react --save-dev
```

此外还需要在项目根目录中添加`.babelrc`文件用来设置预设器：
```json
{
    "presets":["es2015","react"]
}
```
### 4. 安装React
把react添加在项目中：
```bash
npm install react react-dom --save
```
> 这里要注意npm中 save  和 save-dev 的区别：
> save：代表所安装的模块是产品的依赖，最后会给用户使用
> save-dev：代表模块是开发过程中要使用的工具

### 5. 配置webpack
在项目根目录中建立`webpack.config.js`文件。
```javascript
const { resolve } = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  // 配置页面入口js文件
  entry: './src/index.js',

  // 配置打包输出相关
  output: {
    // 打包输出目录
    path: resolve(__dirname, 'dist'),

    // 入口js的打包输出文件名
    filename: 'index.js'
  },

  module: {
    /*
    配置各种类型文件的加载器, 称之为loader
    webpack当遇到import ... 时, 会调用这里配置的loader对引用的文件进行编译
    */
    rules: [
      {
        /*
        使用babel编译ES6/ES7/ES8为ES5代码
        使用正则表达式匹配后缀名为.js的文件
        */
        test: /\.js$/,

        // 排除node_modules目录下的文件, npm安装的包不需要编译
        exclude: /node_modules/,

        /*
        use指定该文件的loader, 值可以是字符串或者数组.
        使用数组可以让文件传入多个loader，loader的处理顺序是从最后一个开始
        babel-loader用来编译js文件.
        */
        use: ['babel-loader']
      }
    ]
  },

  /*
  配置webpack插件
  plugin和loader的区别是, loader是在import时根据不同的文件名, 匹配不同的loader对这个文件做处理,
  而plugin, 关注的不是文件的格式, 而是在编译的各个阶段, 会触发不同的事件, 让你可以干预每个编译阶段.
  */
  plugins: [
    /*
    html-webpack-plugin用来打包入口html文件
    entry配置的入口是js文件, webpack以js文件为入口, 遇到import, 用配置的loader加载引入文件
    但作为浏览器打开的入口html, 是引用入口js的文件, 它在整个编译过程的外面,
    所以, 我们需要html-webpack-plugin来打包作为入口的html文件
    */
    new HtmlWebpackPlugin({
      /*
      template参数指定入口html文件路径, 插件会把这个文件交给webpack去编译,
      webpack按照正常流程, 找到loaders中test条件匹配的loader来编译, 那么这里html-loader就是匹配的loader
      html-loader编译后产生的字符串, 会由html-webpack-plugin储存为html文件到输出目录, 默认文件名为index.html
      可以通过filename参数指定输出的文件名
      html-webpack-plugin也可以不指定template参数, 它会使用默认的html模板.
      */
      template: './src/index.html'
    })
  ],

  /*
  配置开发时用的服务器, 让你可以用 http://127.0.0.1:8080/ 这样的url打开页面来调试
  并且带有热更新的功能, 打代码时保存一下文件, 浏览器会自动刷新. 比nginx方便很多
  如果是修改css, 甚至不需要刷新页面, 直接生效. 这让像弹框这种需要点击交互后才会出来的东西调试起来方便很多.
  */
  devServer: {
    // 配置监听端口, 因为8080很常用, 为了避免和其他程序冲突, 我们配个其他的端口号
    port: 8100,

    /*
    historyApiFallback用来配置页面的重定向

    SPA的入口是一个统一的html文件, 比如
    http://localhost:8010/foo
    我们要返回给它
    http://localhost:8010/index.html
    这个文件

    配置为true, 当访问的文件不存在时, 返回根目录下的index.html文件
    */
    historyApiFallback: true
  }
}
```

### 6.配置 package.json 文件使用项目依赖的 webpack 来启动打包

在项目的 `package.json` 文件中插入找到 `scripts` 这段，添加如下内容：
``` bash
{
  "scripts": {
    "dev": "webpack-dev-server -d --hot",
    "build": "webpack -p"
  }
}
```

这样我们执行
```bash
npm run dev
```
就会开启一个用于开发测试的服务器，并且根据我们在`src`中各个文件的修改，还会自动刷新浏览器，使用起来效率高的不要不要的。
