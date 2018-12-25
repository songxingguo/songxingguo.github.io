title: Vue-TodoList项目实战
author: songxingguo
tags:
  - Vue
categories: []
date: 2018-12-06 16:02:00
---
## 写在前面

[课程地址](https://www.imooc.com/video/16402)、[源码地址](https://github.com/songxingguo/vue-todoList)、[预览地址](https://songxingguo.github.io/vue-todoList/)

## 前端开发

### 网络优化

减少http请求、压缩静态文件、使用浏览器长缓存、应用浏览编写、加快访问速度

<!-- more -->

### 工程化

搭建前端工程、网络优化、API 定制、Node.js

## 项目开发

### 项目结构

![项目结构](https://graphbed.qiniu.songxingguo.com/vue-todolist/%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84.png)

### 核心代码

#### package.json

```json
{
  "name": "vue-todolist",
  "version": "1.0.0",
  "description": "",
  "main": "src/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "node_modules/.bin/webpack --config webpack.config.js --mode production",
    "start": "node_modules/.bin/webpack-dev-server --config webpack.config.js --mode development --open ",
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config webpack.config.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "autoprefixer": "^9.4.2",
    "babel-core": "^6.26.0",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-loader": "^7.1.2",
    "babel-plugin-transform-vue-jsx": "^3.7.0",
    "babel-preset-env": "^1.6.1",
    "css-loader": "^1.0.1",
    "file-loader": "^2.0.0",
    "postcss": "^7.0.7",
    "postcss-import": "^12.0.1",
    "postcss-load-config": "^2.0.0",
    "postcss-loader": "^3.0.0",
    "postcss-url": "^8.0.0",
    "readable-stream": "^2.3.6",
    "style-loader": "^0.23.1",
    "stylus": "^0.54.5",
    "stylus-loader": "^3.0.2",
    "url-loader": "^1.1.2",
    "vue": "^2.5.17",
    "vue-loader": "^15.4.2",
    "webpack": "^4.27.1"
  },
  "devDependencies": {
    "babel": "^6.23.0",
    "babel-plugin-syntax-jsx": "^6.18.0",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-stage-2": "^6.24.1",
    "clean-webpack-plugin": "^1.0.0",
    "cross-env": "^5.2.0",
    "extract-text-webpack-plugin": "^3.0.2",
    "html-webpack-plugin": "^3.2.0",
    "vue-template-compiler": "^2.5.17",
    "webpack-cli": "^3.1.2",
    "webpack-dev-server": "^3.1.10"
  }
}
```
#### webpack.config.js

```js
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
const isDev = process.env.NODE_ENV === "development";

const config = {
    context: __dirname,
    entry: path.resolve(__dirname, 'src/index.js'),
    output: {
        filename: '[name].js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [{
            test: /\.vue$/,
            use: ['vue-loader']
        }, {
            test: /\.css$/,
            use: [ 'style-loader', 'css-loader' ]
        }, {
            test: /\.(png|gif|jpg|jpeg|svg|xml|json)$/,
            use: [ 'url-loader' ]
        }, {
            test: /\.jsx$/,
            loader: "babel-loader"
        }, {
            test: /\.styl/,
            use: [{
                loader: 'style-loader' // creates style nodes from JS strings
            }, {
                loader: 'css-loader' // translates CSS into CommonJS
            }, {
                loader: "postcss-loader",
                options: {
                    sourceMap: true
                }
            }, {
                loader: 'stylus-loader' // compiles Less to CSS
            }]
        }]
    },
    plugins: [
        // make sure to include the plugin for the magic
        new VueLoaderPlugin(),
        new webpack.DefinePlugin({
            'process.env': {
                NODE_ENV: isDev ? '"development"' : '"production"'
            }
        }),
        new HtmlWebpackPlugin({
            template: 'index.html'
        })
    ]
}

if (isDev) {
    config.devtool = '#cheap-module-eval-source-map'
    config.devServer = {
        port: 8000,
        host: '0.0.0.0',
        // overlay: {
        //     errors: true,
        // },
        open: true,
        hot: true,
    }
    config.plugins.push(
        new webpack.HotModuleReplacementPlugin(),
        // new webpack.NormalModuleReplacementPlugin()
    )
}

module.exports = config
```
#### index.js

```js
import Vue from 'vue'
import App from './App.vue'
import  './assets/styles/global.styl'

const root = document.createElement('div');
document.body.appendChild(root)

new Vue({
    render: (h) => h(App)
}).$mount(root)
```
#### .travis.yml

```yml
language: node_js
node_js:
  - 8.12.0

# S: Build Lifecycle
install:
  - npm install

before_script:
# - npm install -g gulp

script:
  - npm run build

after_script:
  - cd ./dist
  - git init
  - git config user.name "songxingguo"
  - git config user.email "1328989942@qq.com"
  - git add .
  - git commit -m "Update Blog By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  # Github Pages
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:gh-pages
  # Add Tag
  - git tag v0.0.$TRAVIS_BUILD_NUMBER -a -m "Auto Taged By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  # Github Pages
  - git push --quiet "https://${GH_TOKEN}@${GH_REF}" master:gh-pages --tags
# E: Build LifeCycle

branches:
  only:
    - master
env:
  global:
    # Github Pages
    - GH_REF: github.com/songxingguo/vue-todoList.git
```

### CSS 分离打包

webpack.config.js

```js
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
const isDev = process.env.NODE_ENV === "development";
const ExtractPlugin = require('extract-text-webpack-plugin');

const config = {
    context: __dirname,
    entry: path.resolve(__dirname, 'src/index.js'),
    output: {
        filename: 'bundle.[hash:8].js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [{
            test: /\.vue$/,
            use: ['vue-loader']
        }, {
            test: /\.css$/,
            use: [ 'style-loader', 'css-loader' ]
        }, {
            test: /\.(png|gif|jpg|jpeg|svg|xml|json)$/,
            use: [ 'url-loader' ]
        }, {
            test: /\.jsx$/,
            loader: "babel-loader"
        }]
    },
    plugins: [
        // make sure to include the plugin for the magic
        new VueLoaderPlugin(),
        new webpack.DefinePlugin({
            'process.env': {
                NODE_ENV: isDev ? '"development"' : '"production"'
            }
        }),
        new HtmlWebpackPlugin({
            template: 'index.html'
        })
    ]
}

if (isDev) {
    config.module.rules.push({
        test: /\.styl/,
        use: [{
            loader: 'style-loader' // creates style nodes from JS strings
        }, {
            loader: 'css-loader' // translates CSS into CommonJS
        }, {
            loader: "postcss-loader",
            options: {
                sourceMap: true
            }
        }, {
            loader: 'stylus-loader' // compiles Less to CSS
        }]
    })
    config.devtool = '#cheap-module-eval-source-map'
    config.devServer = {
        port: 8000,
        host: '0.0.0.0',
        // overlay: {
        //     errors: true,
        // },
        open: true,
        hot: true,
    }
    config.plugins.push(
        new webpack.HotModuleReplacementPlugin(),
        // new webpack.NormalModuleReplacementPlugin()
    )
} else {
    config.output.filename = '[name].[chunkhash:8].js';
    config.module.rules.push({
        test: /\.styl/,
        use: ExtractPlugin.extract({
            fallback: 'style-loader',
            use: [{
                loader: 'css-loader' // translates CSS into CommonJS
            }, {
                loader: "postcss-loader",
                options: {
                    sourceMap: true
                }
            }, {
                loader: 'stylus-loader' // compiles Less to CSS
            }]
        })
    })
    config.plugins.push(
        new ExtractPlugin('styles.[hash:8].css')
    )
}

module.exports = config
```
### webpack区分打包类库代码及hash优化 

webpack.config.js

```js
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
const isDev = process.env.NODE_ENV === "development";
const ExtractPlugin = require('extract-text-webpack-plugin');

const config = {
    context: __dirname,
    entry: path.resolve(__dirname, 'src/index.js'),
    output: {
        filename: 'bundle.[hash:8].js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [{
            test: /\.vue$/,
            use: ['vue-loader']
        }, {
            test: /\.css$/,
            use: [ 'style-loader', 'css-loader' ]
        }, {
            test: /\.(png|gif|jpg|jpeg|svg|xml|json)$/,
            use: [ 'url-loader' ]
        }, {
            test: /\.jsx$/,
            loader: "babel-loader"
        }]
    },
    plugins: [
        // make sure to include the plugin for the magic
        new VueLoaderPlugin(),
        new webpack.DefinePlugin({
            'process.env': {
                NODE_ENV: isDev ? '"development"' : '"production"'
            }
        }),
        new HtmlWebpackPlugin({
            template: 'index.html'
        })
    ]
}

if (isDev) {
    config.module.rules.push({
        test: /\.styl/,
        use: [{
            loader: 'style-loader' // creates style nodes from JS strings
        }, {
            loader: 'css-loader' // translates CSS into CommonJS
        }, {
            loader: "postcss-loader",
            options: {
                sourceMap: true
            }
        }, {
            loader: 'stylus-loader' // compiles Less to CSS
        }]
    })
    config.devtool = '#cheap-module-eval-source-map'
    config.devServer = {
        port: 8000,
        host: '0.0.0.0',
        // overlay: {
        //     errors: true,
        // },
        open: true,
        hot: true,
    }
    config.plugins.push(
        new webpack.HotModuleReplacementPlugin(),
        // new webpack.NormalModuleReplacementPlugin()
    )
} else {
    config.entry = {
        app: path.join(__dirname, 'src/index.js'),
        vendor: ['vue']
    }
    config.output.filename = '[name].[chunkhash:8].js';
    config.module.rules.push({
        test: /\.styl/,
        use: ExtractPlugin.extract({
            fallback: 'style-loader',
            use: [{
                loader: 'css-loader' // translates CSS into CommonJS
            }, {
                loader: "postcss-loader",
                options: {
                    sourceMap: true
                }
            }, {
                loader: 'stylus-loader' // compiles Less to CSS
            }]
        })
    })
    config.plugins.push(
        new ExtractPlugin('styles.[hash:8].css')
    )
    config.optimization = {
        splitChunks: {
            name: 'vendor'
        }
    }
}

module.exports = config
```

## 问题

1. vue-template-compiler must be installed as a peer dependency, or a compatible compiler implementation must be passed via options.

  [参考地址](https://segmentfault.com/q/1010000015986575)

  解决方法： npm install vue-template-compiler -D

2. Module parse failed: Unexpected character '#' (16:0) You may need an appropriate loader to handle this file type.

  [参考地址](https://segmentfault.com/q/1010000014770071?sort=created)

  解决方法： npm install style-leader --save
  
    ```
      {
          test: /\.css$/,
              use: ['style-loader','css-loader']
      }
    ```