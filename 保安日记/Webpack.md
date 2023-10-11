![image](assets/72333201 tailored-20231011214740-3ubmmys.jpg)

# Webpack

# [Webpack](https://www.webpackjs.com/)

## 基本设置

* 测试打包模板

  ```js
  git clone https://gitee.com/westblueflower/webpack-lesson.git//拉取基本模板，可以用来测试webpack
  ```
* ​`webpack.config.js`​：是`Webpack`​的配置文件，在项目根目录里。**vue中是vue.config.js**

  ```js
  const path = require('path')// 导入node.js的path模块
  const HtmlWebpackPlugin = require('html-webpack-plugin')//导入插件
  //webpack配置
  module.exports = {
    devtool: 'source-map',// 是否开启source map，生成独立的map文件，开发使用，上线删除
    mode: 'development',// 打包模式 development 开发 / production 生产
    entry: path.resolve(__dirname, 'src', 'index.js'),//设置打包入口文件，path模块用在这里
    // 开发服务器
    devServer: {//npm run serve 实时打包，打包文件在内存中，需要安装插件
      port: 9000, // 实时打包所用的端口号
      open: true, // 初次打包完成后，自动打开浏览器
      static: path.join(__dirname, './dist'),// 服务器的根目录
      hot: false,//是否开启热替换
    },
    // 打包到哪里
    output: {
      path: path.resolve(__dirname, 'dist'),// 使用绝对路径
      filename: 'bundle.js',// 打包出来的文件名
      clean: true,// 每次打包时清除上一次的dist目录
      assetModuleFilename: 'images/[hash][ext][query]',
    },
    // 模块配置，默认只能打包js，安装插件，可以打包css，less等其他格式文件
    module: {
      // 解析规则
      rules: [
        {
          test: /\.css$/i, // .css结尾的文件
          use: ['style-loader', 'css-loader'], // 通过 这2个 loader进行解析
        },
        {
          test: /\.less$/i, // .less结尾的文件
          use: [
            // 使用如下的loader进行解析
            'style-loader',
            'css-loader',
            'less-loader',
          ],
        },
        {
          test: /\.(png|svg|jpg|jpeg|gif)$/i, // 图片资源
          type: 'asset/resource', // 生成一个单独的文件，并设置url
        },
      ],
    },
    // 插件配置，wbepack默认只打包js，HtmlWebpackPlugin可以生成index.html文件并把js放进去
    plugins: [
      new HtmlWebpackPlugin({
        inject: 'body',// js插入的位置
        filename: 'index.html',// 打包之后的文件名
        template: path.resolve(__dirname, './public/index.html'),// 使用哪个文件作为模板
      }),
    ],
  }
  ```
* [package.json](https://juejin.cn/post/6870426598605062152)：可以设置打包所使用的插件和插件设置(`scripts`​中设置)，以及选择打包环境

  ```js
  {
    "name": "test-project", // 名称，通常是github仓库名称
    "author": "xxx", // 作者的信息
    "contributors": ["xxx", "xxxx"], // 贡献者信息数组
    "bugs": "https://github.com/nodejscn/node-api-cn/issues", // bug信息，通常是github的issue页面
    "homepage": "http://nodejs.cn", // 发布项目时，项目的主页
    "version": "1.0.0", // 当前版本, 遵循semver语义版本控制规范，具体含义将在后面详细解释
    "license": "MIT", // 许可证信息
    "keywords": ["xxx", "xxxx"], // 关键字数组
    "description": "A Vue.js project", // 描述信息
    "repository": "git://github.com/xxxx.git", // 仓库地址
    "main": "src/main.js", // 当引用这个包时，应用程序会在该位置搜索模块的导出
    "private": true,  // 防止包意外的发布到npm上，如果是true，npm将拒绝发布
    "scripts": {
      "dev": "webpack --config webpack.config.js",//配置开发模式打包
      "watch": "webpack watch --config webpack.config.js",//配置watch模式
      "build": "webpack  --config webpack.config.js --mode production",//配置生产模式打包
      "serve": "webpack serve --config webpack.config.js"//配置虚拟服务器

    }, // 可运行的node脚本，通常命令是npm run serve
    "dependencies": {
      "core-js": "^3.6.5",
      "vue": "^3.0.0-0",
      "vue-router": "^4.0.0-0",
      "vuex": "^4.0.0-0"
    }, // 生产环境所依赖的安装包
    "devDependencies": {
      "@vue/cli-plugin-babel": "~4.5.0",
      ...
    } // 开发环境所依赖的安装包
    "engines": {
      "node": ">= 6.0.0",
      "npm": ">= 3.0.0"
    }, // 要运行的 Node.js 或其他命令的版本，但似乎没卵用，可参考https://github.com/nodejs/node/issues/29249
    "browserslist": ["> 1%", "last 2 versions", "not ie <= 8"]  //支持的浏览器及其版本号，polyfill时会用到
  }

  ```
* 安装`webpack`​命令

  ```js
  //安装webpack---两个命令是一个意思：将安装包记录成开发阶段的依赖
  //  --save 保存到 package.json 中
  //  -dev 开发依赖，只在开发阶段使用
  //dependences: 项目依赖, 项目上线也要用的
  //devDependencies: 开发依赖, 只在开发中使用, 上线时不要用的
  npm i webpack --save-dev
  npm i webpack -D
  ```
* 打包命令

  ```js
  npm run dev//开发环境打包
  npm run build//生产环境打包
  ```

## 设置说明

### 模式设置

* 概念

  ```js
  开发环境development:不会对打包生成的文件进行代码压缩和性能优化，打包速度快
  生产环境production:默认设置，会对打包生成的文件进行代码压缩和性能优化，打包速度很慢
  ```
* 设置开发或生产模式：设置`/webpack.config.js`​，增加设置`mode`​

  ```js
  // 导入path模块
  const path = require('path')

  module.exports = {
    mode: 'development',// 打包模式 development 开发 / production 生产

    // 打包的入口文件
    entry: path.resolve(__dirname, 'src', 'index.js'),
    // 打包到哪里
    output: {
      // 使用绝对路径
      path: path.resolve(__dirname, 'dist'),
      // 打包出来的文件名
      filename: 'bundle.js',
    },
  }
  ```
* 设置打包命令：`package.json`​ 中增加打包命令

  ```js
  "scripts": {
    "dev": "webpack --config webpack.config.js",//配置开发模式打包
    "watch": "webpack watch --config webpack.config.js",//配置watch模式
    "build": "webpack  --config webpack.config.js --mode production",//配置生产模式打包
    "serve": "webpack serve --config webpack.config.js"//配置虚拟服务器

  },
  ```
* 打包命令：看你怎么设置得

  ```js
  npm run dev//开发环境打包
  npm run build//生产环境打包
  ```

### 清理设置

清理dist目录：`output.clean`​默认不开启，那么上一次打包生成的dist文件不会删除，这次打包的文件还是在上一次的dist中，导致可能原先的文件存在导致后面看代码文件造成混乱。

开启后会把文件出口文件删除后在继续打包，重新改出口名，原文件不在新出口，是不会删除的。

* ​`/webpack.config.js`​中设置**增加清理** `dist`​ 目录的配置

  ```js
  module.exports = {
    ...
    output: {
      // 使用绝对路径
      path: path.resolve(__dirname, 'dist'),
      // 打包出来的文件名
      filename: 'bundle.js',
      // 每次打包时清除dist目录
      clean: true,
    },
    ...
  }
  ```

### 热替换设置

​`HMR`​：修改了代码并保存时，`HMR`​ 会在应用程序运行过程中替换、添加或删除模块，而**无需重新加载整个页面（入口代码修改还是会刷新）**。

开启了 `dev-server`​ 之后，会**默认开启**​`HMR(hot module replacement)`​。

* ​`HMR`​ 主要通过以下几种方式，来显著加快开发速度：

  * **保留在完全重新加载页面时丢失的应用程序状态；**
  * **只更新变更内容，以节省宝贵的开发时间；**
  * **调整样式更加快速 - 几乎相当于在浏览器调试器中更改样式。**

在`webpack.config.js`​中进行设置

```js
module.exports = {
  // 开发服务器
  devServer: {
    port: 9000, // 实时打包所用的端口号
    open: true, // 初次打包完成后，自动打开浏览器
    // 服务器的根目录
    static: path.join(__dirname, './dist'),
    hot: true,// 模块热替换 true / false 开启 / 关闭
  },
}
```

### 映射设置

就是一个信息文件，里面储存着位置信息。存储着压缩混淆后的代码，所对应的**转换前**的位置。

设置开启，会在项目打包后会生成`.map`​文件，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码，能够极大的方便后期的调试。

注意:开发阶段辅助调错，**上线一定要移除，避免源码泄露**。

在`webpack.config.js`​中进行设置

```js
//开发中需要排错，需要准确的定位错误行号
module.exports = {
  // source-map 仅限在开发模式中使用
  devtool: 'source-map',//（生产环境，我们不希望别人看到我们的源码，注释该配置即可）
}
```

### watch设置

开启 `watch`​ 模式启动自动化打包：检测打包入口，以及打包入口引用的文件是否发生变动，如果发生变动，就重新打包。

​`watch`​ 是内置的，不需要下载，文件发生变动后，文件自动打包，但是网页需要自己刷新。

调整 `package.json`​ 中的 `scripts`​ 开启`watch`​ 模式，然后启动命令：`npm run watch`​

```js
"scripts": {
  "dev": "webpack --config webpack.config.js",
  "watch": "webpack watch --config webpack.config.js",
}
```

## 相关插件

### html-webpack-plugin

​`Webpack`​默认只打包`js`​，这个插件可以打包时会自动生成`index.html`​页面到`dist`​文件夹中，自动把打包生成的`js`​文件插入到页面中。

* 下载插件

  ```js
  npm i--save-dev html-webpack-plugin //下载插件
  ```
* 设置设置模块页面

  ```js
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>测试用模板</title>
    </head>
    <body>
      <div class="box"></div>
    </body>
  </html>

  ```
* **调整** `/webpack.config.js`​，**增加插件实例化的配置**

  ```js
  module.exports = {
    ...
    plugins: [
      new HtmlWebpackPlugin({//实例化
        inject: 'body',// js插入的位置
        filename: 'index.html',// 打包之后的文件名
        template: path.resolve(__dirname, './public/index.html'),//设置页面样本地址
      }),
    ],
  }
  ```

### loader

​`loader`​ 是文件加载器，能够加载资源文件，并对这些文件进行特定的处理，然后打包在指定文件中，**简单来说就是将** `webpack`​ 传入的字符串做出特定的处理修改。

​`webpack`​ 默认只能解析 `js`​ 模块，如果是其他的文件需要安装对应的 `loader`​。

​![image](assets/image-20220731205739-uc0rxf9.png)​

1. 下载所需的loader,第三方模块

    ```js
    npm i --save-dev style-loader css-loader//css
    npm i less less-loader --save-dev//less
    ```
2. 调整 `webpack.config.js`​ 的配置

    ```js
    module.exports = {
      ...
      // 模块配置
      module: {
        // 解析规则
        rules: [
          {
            test: /\.css$/i, // .css结尾的文件
            use: ['style-loader', 'css-loader'], // 通过 这2个 loader进行解析
          },
          {
            test: /\.less$/i, // .less结尾的文件
            use: [
              // 使用如下的loader进行解析
              'style-loader',
              'css-loader',
              'less-loader',
            ],
          },
        ],
      },
    }
    ```
3. 在打包的`index.js`​中导入所要使用的`css`​、`less`​文件

    ```js
    // 导入css 
    // import './assets/base.css'
    // 导入less
    import './assets/less/index.less'
    ```
4. 运行打包

    ```bash
    npm run dev
    ```

### webpack-dev-server

​`webpack-dev-server`​ 的本质是开启了一个测试用服务器，`dev-server`​，开启一个服务器，并自动监测文件变更，刷新页面。

凡是修改了`webpack.config.js ​`​配置文件，或修改了`​ package.json ​`​配置文件，必须重启实时打包的服务器，否则最新的配置文件无法生效。

1. 下载`webpack-dev-server`​：

    ```bash
    npm i --save-dev webpack-dev-server
    ```
2. 修改 `/webpack.config.js`​，**增加** `devServer`​ 的配置

    ```js
    module.exports = {
      ...
      // 开发服务器
      devServer: {
        port: 9000, // 实时打包所用的端口号
        open: true, // 初次打包完成后，自动打开浏览器
        // 服务器的根目录
        static: path.join(__dirname, './dist'),//即文件打包后放的位置
      },
      ....
    }
    ```
3. 设置 `package.json`​，增加一条命令

    ```js
    "scripts": {
      "dev": "webpack --config webpack.config.js",//设置打包模式
      "watch": "webpack watch --config webpack.config.js",//开启watch模式
      "serve": "webpack serve --config webpack.config.js"//开启虚拟服务器
    },
    ```
4. 执行命令，运行项目

    ```bash
    npm run serve
    ```
5. 注意点

    1. 配置了 webpack-dev-server 之后，打包生成的文件存放到了内存中

        1. 不再根据 output 节点指定的路径，存放到实际的物理磁盘上
        2. 提高了实时打包输出的性能，因为内存比物理磁盘速度快很多
        3. **看不到** `dist`​ 目录
    2. ​`http://localhost:9000/`​

        1. 访问的是 `devServer`​ 配置中 `static`​ 的根目录
        2. ​`/index.html`​ 可以省略不写

# 包管理工具

node.js安装完成之后，自带有npm了，可以使用npm下载使用依赖包。

## npm

1. 初始化命令：初始化之后，会在项目目录中生成 `package.json`​ 的文件。

    ```js
    npm init -y
    # 或
    npm init
    # 然后一路回车
    ```
2. 切换镜像地址：下载速度可以快一点

    ```js
    下面的命令只需要执行一次即可（不管以后重启vscode还是重启电脑，都不需要执行第二次）
    npm config set registry https://registry.npmmirror.com
    ```
3. 安装第三方包

    ```js
    # 正常的下载安装
    npm install 模块名

    # 简写install为i
    npm i 模块名

    # 一次性安装多个模块
    npm i 模块名 模块名 模块名
    # 可以通过@版本号,不设置默认是最新
    npm i 模块名@版本号
    ```
4. 卸载第三方包

    ```js
    npm uninstall 模块名
    npm un 模块名
    npm un 模块名 模块名 模块名
    ```

## yarn

​`npm`​是官方的管理工具，`yarn`​是第三方的包管理工具 , 和`npm`​相比有**缓存机制**，可以直接取缓存没必要重新下载，速度比`npm`​快一些。

* 基本命令

  ```js
  1.安装yarn
  npm i yarn -g    // 全局安装使用yarn

  2.初始化
  yarn init  //  yarn init -y

  3.安装插件
  npm:   npm i 插件名
  yarn:  yarn add 插件名
         yarn global add 插件名//全局安装

  4.安装所有依赖
  npm:   npm i
  yarn:  yarn

  5.删除插件
  npm:   npm uninstall  插件名
  yarn:  yarn remove 插件名
         yarn global remove 插件名//全局卸载

  6.运行项目
  npm:   npm run serve
  yarn:  yarn serve

  7.设置淘宝镜像
  npm:   npm config set registry http://registry.npm.taobao.org/
  yarn:  yarn config set registry http://registry.npm.taobao.org/

  8.npm换成yarn：删掉node_module,package-lock.json然后安装使用yarn
  ```
