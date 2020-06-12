# Webpack篇

webpack是现代JavaScript应用程序的静态模块打包器。webpack处理应用程序时，会递归地构建一个依赖关系图，包含应用需要的每个模块，然后将这些模块打包成一个或者多个bundle（包）；

## 核心概念：

1. 入口（entey）
2. 输出（output）
3. loader
4. plugins（插件）

## 入口{entry}

入口起点（entry point）表示webpack应该使用哪个模块，来作为构建内部依赖关系图的开始。进入起点后，webpack会找出那些模块和库是入口起点依赖的。可以指定一个或者多个需要打包的入口js文件

```js
build/webpack.base.conf.js
// 单个入口（简写）语法
module.exports = {
  entry: './src/main.js'
};
// 对象语法
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```

## 出口{output}

output属性告诉webpack在哪里输出它所创建的bundles，以及如何命名这些文件，默认值是`./dist`。只可指定一个输出配置。

```javascript
build/webpack.base.conf.js
const path = require('path');

module.exports = {
  output: {
    // 打包后的文件存放的路径和文件夹名称
    path: path.resolve(__dirname, '../dist'),
    // 打包后的js文件放到dist/文件下。需要通过占位符才能打包为多个js文件，否则会依次覆盖文件
    filename: '[name].js',
    // 打包后的路径前面加上动态地址
    publicPath: process.env.NODE_ENV === 'production' ?
      config.build.assetsPublicPath :
      config.dev.assetsPublicPath
  }
};
```

output.path：表示webpack生成的bundle生成（emit）到哪里和生成的文件夹name（dist）；

output.filename：表示生成的bundle的名称，`[xxx].js`xxx代表变量；

## loader

loader让webpack能够处理非JavaScript文件（webpack自身只能理解JS）。webpack可以将所有类型文件转换成webpack能够处理的有效模块，然后你就可以利用webpack的打包能力，对他们进行处理。本质上，是转换成依赖图可以直接引用的模块。

```js
build/webpack.base.conf.js
// loader的三种写法
module: {
  rules: [
    {
      test: /\.css$/,
      use: ['style-loader','css-loader']
    }
  ]
},
module: {
  rules: [
    {
      test: /\.css$/,
      loader: ['style-loader','css-loader']
    }
  ]
},
module: {
  rules: [
    {
      test: /\.css$/,
      use: [
        {loader:'style-loader'},
        {loader:'css-loader'}
      ]
    }
  ]
},
```

1. test属性：用于标识出应该被对应的loader进行转换的某个或某些文件；正则匹配到的
2. use属性：表示进行转换时，应该使用哪个loader。

# 插件{plugins}

loader被用于转换某些类型的模块，而插件则可以执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中否变量。插件接口极其强大，可以处理各种各样的任务。

如果想使用一个插件，你只需要`require()`它，然后添加到`plugins`数组中。多说插件可以通过选项（options）自定义。也可在一个配置文件中因为不同目的而多次使用一个插件，这时需要通过使用`new`操作符来创建一个实例。

``````
build/webpack.base.conf.js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

const config = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'}),
    new webpack.optimize.CommonsChunkPlugin('common.js'),
    new webpack.ProvidePlugin({
      jQuery: "jquery",
      $: "jquery"
    })
  ]
};

module.exports = config;
``````

