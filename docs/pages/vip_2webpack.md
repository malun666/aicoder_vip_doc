# webpack 入门教程

## webpack 是什么？

本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

![webpack](../images/webpack.png)

## 快速了解几个基本的概念

### mode 开发模式

webpack 提供 mode 配置选项，配置 webpack 相应模式的内置优化。

```diff
// webpack.production.config.js
module.exports = {
+  mode: 'production',
}
```

### 入口文件(entry)

入口文件，类似于其他语言的起始文件。比如：c 语言的 main 函数所在的文件。

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

可以在 webpack 的配置文件中配置入口，配置节点为： `entry`,当然可以配置一个入口，也可以配置多个。

### 输出(output)

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。

```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

### loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

### 插件(plugins)

loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

## webpack 的安装

请确保安装了 `Node.js` 的最新版本。而且已经在您的项目根目录下已经初始化好了最基本的`package.json`文件

### 本地安装 webpack

```sh
$ npm install --save-dev webpack

# 如果你使用 webpack 4+ 版本，你还需要安装 CLI。
npm install --save-dev webpack-cli
```

安装完成后，可以添加`npm`的`script`脚本

```js
// package.json
"scripts": {
    "start": "webpack --config webpack.config.js"
}
```

### 全局安装 webpack（不推荐)

将使 webpack 在全局环境下可用：

```sh
npm install --global webpack
```

> 注意：不推荐全局安装 webpack。这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。

## 快速入门完整 demo

- 第一步：创建项目结构

首先我们创建一个目录，初始化 npm，然后 在本地安装 webpack，接着安装 webpack-cli（此工具用于在命令行中运行 webpack）：

```sh
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```

项目结构

```diff
  webpack-demo
+ |- package.json
+ |- /dist
+   |- index.html
+ |- /src
+   |- index.js
```

- 第二步：安装 loadash 依赖和编写 js 文件

```sh
npm install --save lodash
```

编写：src/index.js 文件

```js
import _ from 'lodash';

function createDomElement() {
  let dom = document.createElement('div');
  dom.innerHTML = _.join(['aicoder', '.com', ' wow'], '');
  return dom;
}

document.body.appendChild(createDomElement());
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>起步</title>
</head>
<body>
  <script src="./main.js"></script>
</body>
</html>
```

- 第三步：编写 webpack 配置文件

根目录下添加 `webpack.config.js`文件。

```diff
  webpack-demo
  |- package.json
+ |- webpack.config.js
  |- /dist
    |- index.html
  |- /src
    |- index.js
```

webpack.config.js 内容如下：

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, './dist')
  }
};
```

- 执行构建任务

直接执行构建任务：

```sh
npx webpack
```

打开： dist/index.html 可以查看到页面的结果。

## 加载非 js 文件

webpack 最出色的功能之一就是，除了 JavaScript，还可以通过 loader 引入任何其他类型的文件

### 加载 CSS 文件

- 第一步： 安装 css 和 style 模块解析的依赖 `style-loader` 和 `css-loader`

```sh
npm install --save-dev style-loader css-loader
```

- 第二步： 添加 css 解析的 loader

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
};
```

- `css-loader`： 辅助解析 js 中的 import './main.css'
- `style-loader`: 把 js 中引入的 css 内容 注入到 html 标签中，并添加 style 标签.依赖 `css-loader`

> 你可以在依赖于此样式的 js 文件中 导入样式文件，比如：import './style.css'。现在，当该 js 模块运行时，含有 CSS 字符串的 `<style>` 标签，将被插入到 html 文件的 `<head>`中。

- 第三步： 编写 css 文件和修改 js 文件

在 src 目录中添加 `style.css`文件

```diff
 webpack-demo
  |- package.json
  |- webpack.config.js
  |- /dist
    |- bundle.js
    |- index.html
  |- /src
+   |- style.css
    |- index.js
  |- /node_modules
```

src/style.css

```css
.hello {
  color: red;
}
```

修改 js 文件

```diff
  import _ from 'lodash';
+ import './style.css';

  function createDomElement() {
    let dom = document.createElement('div');
    dom.innerHTML = _.join(['aicoder', '.com', ' wow'], '');
+   dom.className = 'hello';
    return dom;
  }

  document.body.appendChild(createDomElement());
```

最后重新打开 dist 目录下的 index.html 看一下文字是否变成了红色的了。

### module 配置补充

模块(module): 这些选项决定了如何处理项目中的不同类型的模块。

#### module.noParse

值的类型： RegExp | [RegExp] | function

防止 webpack 解析那些任何与给定正则表达式相匹配的文件。忽略的文件中不应该含有 import, require, define 的调用，或任何其他导入机制。忽略大型的 library 可以提高构建性能。

```js
module.exports = {
  mode: 'devleopment',
  entry: './src/index.js',
  ...
  module: {
    noParse: /jquery|lodash/,
    // 从 webpack 3.0.0 开始,可以使用函数，如下所示
    // noParse: function(content) {
    //   return /jquery|lodash/.test(content);
    // }
  }
  ...
};
```

#### module.rules

创建模块时，匹配请求的规则数组。这些规则能够修改模块的创建方式。这些规则能够对模块(module)应用 loader，或者修改解析器(parser)。

```js
module.exports = {
  ...
  module: {
    noParse: /jquery|lodash/,
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
  ...
};
```

#### module Rule

- Rule 条件详解
  - 字符串：匹配输入必须以提供的字符串开始。是的。目录绝对路径或文件绝对路径。
  - 正则表达式：test 输入值。
  - 函数：调用输入的函数，必须返回一个真值(truthy value)以匹配。
  - 条件数组：至少一个匹配条件。
  - 对象：匹配所有属性。每个属性都有一个定义行为。

#### Rule.test

- { test: Condition }：匹配特定条件。一般是提供一个正则表达式或正则表达式的数组，但这不是强制的。

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
  ...
};
```

其他的条件比如：

- `{ include: Condition }`:匹配特定条件。一般是提供一个字符串或者字符串数组，但这不是强制的。
- `{ exclude: Condition }`:排除特定条件。一般是提供一个字符串或字符串数组，但这不是强制的。
- `{ and: [Condition] }`:必须匹配数组中的所有条件
- `{ or: [Condition] }`:匹配数组中任何一个条件
- `{ not: [Condition] }`:必须排除这个条件

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        include: [
          path.resolve(__dirname, "app/styles"),
          path.resolve(__dirname, "vendor/styles")
        ],
        use: ['style-loader', 'css-loader']
      }
    ]
  }
  ...
};
```

#### Rule.use

应用于模块指定使用一个 loader。

Loaders can be chained by passing multiple loaders, which will be applied from right to left (last to first configured).

加载器可以链式传递，从右向左进行应用到模块上。

```js
use: [
  'style-loader',
  {
    loader: 'css-loader'
  },
  {
    loader: 'less-loader',
    options: {
      noIeCompat: true
    }
  }
];
```

> 传递字符串（如：use: [ "style-loader" ]）是 loader 属性的简写方式（如：use: [ { loader: "style-loader "} ]）。

### 加载 Sass 文件

加载 Sass 需要`sass-loader`。

安装

```sh
npm install sass-loader node-sass webpack --save-dev
```

使用：

```js
// webpack.config.js
module.exports = {
  ...
  module: {
    rules: [{
      test: /\.scss$/,
      use: [{
        loader: "style-loader"
      }, {
        loader: "css-loader"
      }, {
        loader: "sass-loader"
      }]
    }]
  }
};
```

为sass文件注入内容：

如果你要将 Sass 代码放在实际的入口文件(entry file)之前，可以设置 data 选项。此时 sass-loader 不会覆盖 data 选项，只会将它拼接在入口文件的内容之前。

```js
{
    loader: "sass-loader",
    options: {
        data: "$env: " + process.env.NODE_ENV + ";"
    }
}
```

> 注意：由于代码注入, 会破坏整个入口文件的 source map。通常一个简单的解决方案是，多个 Sass 文件入口。

### 创建Source Map

`css-loader`和`sass-loader`都可以通过该options设置启用sourcemap。

```js
// webpack.config.js
module.exports = {
  ...
  module: {
    rules: [{
      test: /\.scss$/,
      use: [{
        loader: "style-loader"
      }, {
        loader: "css-loader",
        options: {
          sourceMap: true
        }
      }, {
        loader: "sass-loader",
        options: {
          sourceMap: true
        }
      }]
    }]
  }
};
```

### PostCSS处理loader（附带：添加css3前缀）

[PostCSS](https://postcss.org/)是一个CSS的预处理工具，可以帮助我们：给CSS3的属性添加前缀，样式格式校验（stylelint），提前使用css的新特性比如：表格布局，更重要的是可以实现CSS的模块化，防止CSS样式冲突。

我们常用的就是使用PostCSS进行添加前缀，以此为例：

安装

```sh
npm i -D postcss-loader
npm install autoprefixer --save-dev

# 以下可以不用安装
# cssnext可以让你写CSS4的语言，并能配合autoprefixer进行浏览器兼容的不全，而且还支持嵌套语法
$ npm install postcss-cssnext --save-dev

# 类似scss的语法，实际上如果只是想用嵌套的话有cssnext就够了
$ npm install precss --save-dev

# 在@import css文件的时候让webpack监听并编译
$ npm install postcss-import --save-dev
```

```js
const path = require('path');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          'style-loader', {
            loader: 'css-loader',
            options: {
              sourceMap: true
            }
          }, {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              sourceMap: true,
              plugins: (loader) => [
                require('autoprefixer')({browsers: ['> 0.15% in CN'] }) // 添加前缀
              ]
            }
          }, {
            loader: 'sass-loader',
            options: {
              sourceMap: true
            }
          }
        ]
      }
    ]
  }
};

```

### 样式表抽离成专门的单独文件并且设置版本号

首先以下的 css 的处理我们都把 mode 设置为 `production`。

webpack4 开始使用： `mini-css-extract-plugin`插件, 1-3 的版本可以用： `extract-text-webpack-plugin`

> 抽取了样式，就不能再用 `style-loader`注入到 html 中了。

```sh
npm install --save-dev mini-css-extract-plugin
```

```js
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const devMode = process.env.NODE_ENV !== 'production'; // 判断当前环境是开发环境还是 部署环境，主要是 mode属性的设置值。

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader','postcss-loader', 'sass-loader']
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: devMode ? '[name].css' : '[name].[hash].css', // 设置最终输出的文件名
      chunkFilename: devMode ? '[id].css' : '[id].[hash].css'
    })
  ]
};
```

再次运行打包：

在 dist 目录中已经把 css 抽取到单独的一个 css 文件中了。修改 html，引入此 css 就能看到结果了。

### 压缩 CSS

webpack5 貌似会内置 css 的压缩，webpack4 可以自己设置一个插件即可。

压缩 css 插件：`optimize-css-assets-webpack-plugin`

安装

```sh
npm i -D optimize-css-assets-webpack-plugin
```

```js
const path = require('path');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const autoprefixer = require('autoprefixer');

module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    filename: 'main.[hash].js',
    path: path.resolve(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          MiniCssExtractPlugin.loader, {
            loader: 'css-loader'
          }, {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              plugins: (loader) => [
                autoprefixer({browsers: ['> 0.15% in CN']}),
              ]
            }
          }, {
            loader: 'sass-loader',
          }
        ]
      }
    ]
  },
  plugins: [new MiniCssExtractPlugin({filename: '[name][hash].css', chunkFilename: '[id][hash].css'})],
  optimization: {
    minimizer: [
      new OptimizeCSSAssetsPlugin({})
    ]
  }
};
```

### JS 压缩

压缩需要一个插件： `uglifyjs-webpack-plugin`, 此插件需要一个前提就是：`mode: 'production'`.

安装

```sh
npm i -D uglifyjs-webpack-plugin
```

```js
const path = require('path');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const autoprefixer = require('autoprefixer');

module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    filename: 'main.[hash].js',
    path: path.resolve(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          MiniCssExtractPlugin.loader, {
            loader: 'css-loader'
          }, {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              plugins: (loader) => [
                autoprefixer({browsers: ['> 0.15% in CN']}),
              ]
            }
          }, {
            loader: 'sass-loader',
          }
        ]
      }
    ]
  },
  plugins: [new MiniCssExtractPlugin({filename: '[name][hash].css', chunkFilename: '[id][hash].css'})],
  optimization: {
    minimizer: [
      new UglifyJsPlugin({
        cache: true,
        parallel: true,
        sourceMap: true // set to true if you want JS source maps
      }),
      new OptimizeCSSAssetsPlugin({})
    ]
  }
};
```

### 解决CSS文件或者JS文件名字哈希变化的问题

`HtmlWebpackPlugin`插件，可以把打包后的CSS或者JS文件引用直接注入到HTML模板中，这样就不用每次手动修改文件引用了。

安装

```sh
npm install --save-dev html-webpack-plugin
```

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const autoprefixer = require('autoprefixer');

module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    filename: 'main.[hash].js',
    path: path.resolve(__dirname, './dist')
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          MiniCssExtractPlugin.loader, {
            loader: 'css-loader'
          }, {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              plugins: (loader) => [
                autoprefixer({browsers: ['> 0.15% in CN']}),
              ]
            }
          }, {
            loader: 'sass-loader',
          }
        ]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({filename: '[name][hash].css', chunkFilename: '[id][hash].css'}),
    new HtmlWebpackPlugin({
      title: 'AICODER 全栈线下实习', // 默认值：Webpack App
      filename: 'main.html',  // 默认值： 'index.html'
      template: path.resolve(__dirname, 'src/index.html'),
      minify: {
        collapseWhitespace: true,
        removeComments: true,
        removeAttributeQuotes: true, // 移除属性的引号
      }
    })
  ],
  optimization: {
    minimizer: [
      new UglifyJsPlugin({
        cache: true,
        parallel: true,
        sourceMap: true // set to true if you want JS source maps
      }),
      new OptimizeCSSAssetsPlugin({})
    ]
  }
};

```