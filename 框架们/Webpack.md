# Webpack

## 踩坑

`Cannot assign to read only property 'exports' of object '#<Object>'`
在webpack打包的时候，可以在js文件中混用require和export。但是不能混用import 以及module.exports。
因为webpack 2中不允许混用import和module.exports

## 常用配置

无需指定入口文件 默认./src/index.js

### 转换es6

依赖: @babel/core @babel/preset-env babel-loader

```js
module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
```

### 复制并压缩html文件

自动生成打包好并自动添加js入口文件
依赖: html-webpack-plugin html-loader

```js
module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
```

### 编译css，自动添加前缀，抽取css到独立文件

`npm i mini-css-extract-plugin css-loader --save-dev`

`npm i style-loader postcss-loader  --save-dev`

使用 mini-css-extract-plugin  报错`window is not defined` 移除style-loader

```js
    // webpack.config.js
    const MiniCssExtractPlugin = require("mini-css-extract-plugin");
    module.exports = (env, argv) => {
      const devMode = argv.mode !== 'production'
      return {
        module: {
          rules: [
            // ...,
            {
                test: /\.css$/,
                use: [
                    devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
                    'css-loader',
                    'postcss-loader'
                ]
            },
            ]
          },
          plugins: [
            // ...,
            new MiniCssExtractPlugin({
              filename: "[name].css",
              chunkFilename: "[id].css"
            })
          ]
      }
    }
```

## 有趣的用处

导出 `variables.scss` 里定义的变量到 js 中使用

```scss
$primary: #5A69F3 !default;
$white: #FFFFFF !default;

:export {
  primaryColor: $primary;
  whiteColor: $white;
}

// index.vue
import variables from 'variables.scss'
computed: {
  variables() {
    return vatiables
  }
 }

```

## 优化

后编译
按需加载
