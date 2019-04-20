# gulp 基础

gulp 是基于 nodejs 的用自动化构建工具，可以通过 gulp 和各种插件实现前端的开发自动化工作流程。

> gulp4.0已经发布，当前文章是gulp@3.9.1的版本的代码，如果需要升级请查看我的文章：[gulp4.0升级](/pages/gulp4.md)

## 快速入门

第一步： 全局安装 gulp：

```sh
# 安装gulp的控制台辅助工具。
$ npm install --global gulp-cli
$ npm install --global gulp@4.0.0
```

第二步： 作为项目的开发依赖（devDependencies）安装

```sh
# 由于gulp4.0.0发布后不兼容之前的api，所以本文以下所有的代码实例都是以3.9.1版本编写，请注意！
$ npm install --save-dev gulp@3.9.1
```

> **由于gulp4.0.0发布后不兼容之前的api，所以本文以下所有的代码实例都是以4.0.0版本编写，请注意！**

第三步：

在项目根目录下创建一个名为 gulpfile.js 的文件：

```js
var gulp = require('gulp');

gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});
```

第四步： 运行 gulp：

```sh
$ gulp
```

> 默认的名为 default 的任务（task）将会被运行，在这里，这个任务并未做任何事情。
> 想要单独执行特定的任务（task），请输入 gulp <task> <othertask>。

## gulp API 文档

`gulp`在3.9.1之前的版本只有一共就 4 个 api， `src` 、`dest` 、`task` 、`watch`。
在新的4.0.0的版本中增加了`gulp.parallel`和`gulp.series`等api。

- `gulp.src()` --全局匹配一个或多个文件
- `gulp.dest()` --将文件写入目录
- `gulp.symlink()` --与dest相似，但是使用软连接形式
- `gulp.task()` --定义任务
- `gulp.lastRun()` --获得上次成功运行的时间戳
- `gulp.parallel()` --并行运行任务
- `gulp.series()` --运行任务序列
- `gulp.watch()` --当文件发生变化时做某些操作
- `gulp.tree()` --获得任务书树
- `gulp.registry()` --获得或注册任务

具体细节请参考[官方文档](https://gulpjs.com/docs/en/api/concepts)

## gulp task的用法

task就代表一个任务，注册一个gulp的任务。

### 注册函数为一个任务，任务名就是函数的名字。

```js
const gulp = require('gulp');

// 定义任务的方法
function DemoTask(cb) {
  console.log('任务执行中');
  cb(); // cb就是告诉gulp，当前任务执行结束了。
}

// 注册任务名字为'DemoTask'的任务，可以使用gulp DemoTask 运行此任务。
gulp.task(DemoTask);
```

运行上面代码的任务：  

```sh
gulp DemoTask
```

### 注册指定任务名字的task任务

```js
const gulp = require('gulp');
// 注册任务名字为'DemoTask'的任务，可以使用gulp DemoTask 运行此任务。
gulp.task('DemoTask', function (cb) {  // 指定的第一个参数就是任务的名字
  console.log('任务执行中');
  cb(); // cb就是告诉gulp，当前任务执行结束了。
});
```

运行上面的任务：

```sh
gulp DemoTask
```

### 任务还可以是串行任务和并行任务

`gulp.series()`可以串行执行多个任务。
`gulp.parallel()`可以并行执行多个任务。

```js
const gulp = require('gulp');
function d1(cb) {
  console.log(1);
  cb();
}
function d2(cb) {
  console.log(2);
  cb();
}
function d3(cb) {
  console.log(3);
  cb();
}
function d4(cb) {
  console.log(4);
  cb();
}

gulp.task('s1', gulp.series(d1, d2)); // 串行执行任务d1和d2
gulp.task('p1', gulp.parallel(d1, d2)); // 并行执行任务d1和d2
gulp.task('default', gulp.series(d1, gulp.parallel(d2, d3, d4), 'p1')); // 嵌套串行和并行任务。
```

> 任务的名字要么是函数要么就是字符串。

### 设定函数任务的displayName

用js函数作为任务的时候，可以设置函数的`displayName`属性 来设定任务名字。

```js
function styles(){...}
styles.displayName = "pseudoStyles";
styles.description = "Does something with the stylesheets."
gulp.task(styles);

// 运行此任务的命令就是：   gulp pseudoStyles
```

### 任务的返回值和回调函数

- 1) Callback实现回调通知执行完成

```js
var del = require('del');
gulp.task('clean', function(done) {
    del(['.build/'], done);
});
```

- 2) Return a Stream

直接返回一个gulp的处理流

```js
gulp.task('somename', function() {
    return gulp.src('client/**/*.js')
        .pipe(minify())
        .pipe(gulp.dest('build'));
});
```

- 3) Return a Promise

返回一个promise对象。

```js
var promisedDel = require('promised-del');
gulp.task('clean', function() {
    return promisedDel(['.build/']);
});
```

## 串行注册任务

比如我们需要执行以下任务：

```js
gulp.task('clean-dev',  function() {});
gulp.task('clean-dist', function() {});
gulp.task('sprite', function() {});
gulp.task('compile-css', function() {});
gulp.task('compile-js', function() {});
gulp.task('copy-html', function() {});
gulp.task('reversion', function() {});
gulp.task('replace', function() {});
```

在之前我们是通过第三方的工具比如： `run-sequence`实现按顺序调用。
比如：

```js
gulp.task('dev',  ['clean-dev'],  function()  {
    runSecquence(['compile-css',  'compile-js',  'copy-html']);
});
```

新API中`gulp.series`可以帮助我们进行顺序执行任务。比如：

```js
function cleanDev()  {}
function cleanDist()  {}
function sprite()  {}
function compileCss()  {}
function compileJs()  {}
function copyHtml()  {}
function reversion()  {}
function replace()  { }
gulp.task('defualt', gulp.series(cleanDev, cleanDist, compileCss))
```

## 并行注册任务

有些任务可以同时进行的，不用等待其他任务的，可以放到并行任务中。gulp提供`gulp.parallel`方法执行并行任务。

比如：

```js
function cleanDev()  {}
function cleanDist()  {}
function sprite()  {}
function compileCss()  {}
function compileJs()  {}
function copyHtml()  {}
function reversion()  {}
function replace()  {   }
gulp.task('default', gulp.series(
    cleanDev,
    cleanDist,
    gulp.parallel(
      compileCss,
      compileJs
      copyHtml
    ),
    reversion,
    replace
  ));
```

## gulp的watch变化

`watch`之前的第二个参数是一个数组，当发生变化的时候需要执行的任务数组。
例如：

```js
gulp.task('dev', ['open'], function () {
  gulp.watch('src/style/**', ['style:dev']);
  gulp.watch('src/template/**', ['tpl']);
});
```

目前参数变更为：

```js
// 语法签名
watch(globs, [options], [task])
```

实例：

```js
const { watch } = require('gulp');

watch(['input/*.js', '!input/something.js'], {delay: 300}, function(cb) {
  // body omitted
  cb();
});
```

我们主要关心task的变化成了一个函数或者是`gulp.series`或者`gulp.parallel`的任务。
而且返回值是一个可以监控当前watch设置的一个对象实例，可以进行个性化定制监控的事件。

所以，开始的例子我们得改造成新的为：

```js
gulp.task('dev', ['open'], function () {
  gulp.watch('src/style/**', ['style:dev']);
  gulp.watch('src/template/**', ['tpl']);
});

// 改造后
function style(){}  // style的编译任务
function tpl(){}
function dev() {
  gulp.watch('src/style/**', gulp.series(style));
  gulp.watch('src/template/**', function(cb){
    gulp.series(tpl)
    cb();
  });
}
gulp.task('default', gulp.series(dev))
```

## 适合初学者的 gulp+requirejs 的项目模板

虽然，`webpack`已经大行其道。作为初学者，gulp 依然是最快的入门学习前端工作流的工具。本项目本着为中小型团队打造一个基于：gulp+sass+requirejs+es6+eslint+arttemplate 的基本的项目模板。

项目模板[在线 github 地址](https://github.com/malun666/gulptemp)

## gulp 相关应用

- js(用 requirejs 管理模块)
  - 压缩
  - 转码
  - 版本号
  - 自动修改注入 requirejs 的 paths
  - es6 转码
  - eslint 检测
  - 添加版本信息
- style
  - sass
  - 合并
  - 压缩
  - 加前缀
  - 打版本
  - sourcemap(开发)
- 图片压缩优化
- html
  - 压缩
  - 修改 url 地址
- 打包前清理
- 打包后拷贝目录

## 编辑器的一致性

- .editorconfig 文件控制编辑器的一致性
- eslint 校验一致性，并配置 vscode 自动修改符合 eslint 校验，可以安装 eslint 插件。

## 本地测试和代理配置

- 运行本地测试

```shell
$ npm run dev
```

本地启动服务器地址： http://localhost:8000

## 打包

```shell
$ npm run build

# 会把最终结果输出到dist目录
```

## 项目的目录结构

```diff
.
├── gulpfile.js
├── package.json
├── readme.md
├── src
│   ├── app.js
│   ├── asset
│   │   └── img
│   │       └── a.jpg
│   ├── controller
│   │   └── student
│   │       └── listController.js
│   ├── index.html
│   ├── js
│   │   ├── E.js
│   │   ├── index.js
│   │   ├── rev-manifest.json
│   │   └── tmpl
│   │       ├── header.js         // 自动生成
│   │       ├── student.js        // 自动生成
│   │       └── user.js
│   ├── lib
│   │   ├── arttemplate
│   │   │   ├── es5-sham.min.js
│   │   │   ├── es5-shim.min.js
│   │   │   ├── json3.min.js
│   │   │   └── template-web.js
│   │   └── require.js
│   ├── service
│   │   └── index.js              // 所有的服务
│   ├── style
│   │   ├── a.css
│   │   ├── main.css
│   │   ├── rev-manifest.json
│   │   └── scss
│   │       ├── b.scss
│   │       └── index.scss
│   ├── template                 //  art-template模板
│   │   ├── header
│   │   │   └── header.html
│   │   ├── student
│   │   │   ├── stuList.html
│   │   │   ├── t.html
│   │   │   └── userList.html
│   │   └── user
│   │       └── footer.html
│   └── view
│       ├── about.html
│       └── student
│           └── list.html
└── yarn.lock
```

## gulpfile 细节

```js
'use strict';
const fs = require('fs');
const path = require('path');

// 引入gulp包， nodejs代码
const gulp = require('gulp'),
  // 引入gulp-sass 包
  sass = require('gulp-sass'),
  // gulp版本定义
  rev = require('gulp-rev'),
  concat = require('gulp-concat'), // 合并文件 --合并只是放一起--压缩才会真正合并相同样式
  rename = require('gulp-rename'), // 设置压缩后的文件名
  // 替换版本url
  revCollector = require('gulp-rev-collector'),
  // css 压缩
  cleanCss = require('gulp-clean-css'),
  // 兼容处理css3
  autoprefixer = require('gulp-autoprefixer'),
  // css代码map
  sourcemaps = require('gulp-sourcemaps'),
  // js 压缩
  uglify = require('gulp-uglify'),
  // img 压缩
  imgagemin = require('gulp-imagemin'),
  // 清理文件夹
  clean = require('gulp-clean'),
  // 顺序执行
  runSequence = require('gulp-run-sequence'),
  // 给js文件替换文件路径
  configRevReplace = require('gulp-requirejs-rev-replace'),
  // 过滤
  filter = require('gulp-filter'),
  // js校验
  eslint = require('gulp-eslint'),
  // babel 转换es6
  babel = require('gulp-babel'),
  // 本地的测试服务器
  connect = require('gulp-connect'),
  // 本地测试服务器中间件
  proxy = require('http-proxy-middleware'),
  // 服务器的代理插件
  modRewrite = require('connect-modrewrite'),
  // 打开浏览器
  open = require('gulp-open'),
  // html 压缩
  htmlmin = require('gulp-htmlmin'),
  replace = require('gulp-replace'),
  // 自动编译artTemplate模板成js文件
  tmodjs = require('gulp-tmod');
// minifyHtml = require('gulp-minify-html'); 样式处理工作流：编译sass → 添加css3前缀 → 合并 →
// 压缩css →添加版本号
function style(cb) {
  // 过滤非sass文件
  var sassFilter = filter(['**/*.scss'], {restore: true});
  return gulp.src(['./src/style/**/*.{scss,css}', '!./src/style/main.css']) // 读取sass文件
    .pipe(sassFilter)
    .pipe(sass()) // 编译sass
    .pipe(sassFilter.restore)
    .pipe(autoprefixer({
      // 兼容css3
      browsers: ['last 2 versions'], // 浏览器版本
      cascade: true, // 美化属性，默认true
      add: true, // 是否添加前缀，默认true
      remove: true, // 删除过时前缀，默认true
      flexbox: true // 为flexbox属性添加前缀，默认true
    }))
    .pipe(concat('main.css')) // 合并css
    .pipe(cleanCss({
      // 压缩css
      compatibility: 'ie8',
      // 保留所有特殊前缀 当你用autoprefixer生成的浏览器前缀，如果不加这个参数，有可能将会删除你的部分前缀
      keepSpecialComments: '*'
    }))
    .pipe(rev()) // 文件名加MD5后缀
    .pipe(gulp.dest('./dist/style/')) // 输出目标文件到dist目录
    .pipe(rev.manifest()) // 生成一个rev-manifest.json
    .pipe(gulp.dest('./src/style/')); // 将 rev-manifest.json 保存到 src目录
}

// 替换目标html文件中的css版本文件名，js版本的文件名,html 压缩
function html(e) {
  return gulp.src(['./src/**/*.json', './src/**/*.html', '!./src/template/**']) // - 读取 rev-manifest.json 文件以及需要进行css名替换的文件
    .pipe(revCollector({replaceReved: true})) // - 执行html文件内css文件名的替换和js文件名替换
    .pipe(htmlmin({
    removeComments: true, // 清除HTML注释
    collapseWhitespace: true, // 压缩HTML
    // collapseBooleanAttributes: true, //省略布尔属性的值 <input checked="true"/> ==> <input />
    removeEmptyAttributes: true, // 删除所有空格作属性值 <input id="" /> ==> <input />
    removeScriptTypeAttributes: true, // 删除<script>的type="text/javascript"
    removeStyleLinkTypeAttributes: true, // 删除<style>和<link>的type="text/css"
    minifyJS: true, // 压缩页面JS
    minifyCSS: true // 压缩页面CSS
  }))
    .pipe(gulp.dest('./dist/')); // - 替换后的文件输出的目录
}

// 图片压缩
function imgmin(cb) {
  return gulp
    .src('./src/asset/**/*.{png,jpg,gif,ico}')
    .pipe(imgagemin({
      optimizationLevel: 5, // 类型：Number  默认：3  取值范围：0-7（优化等级）
      progressive: true, // 类型：Boolean 默认：false 无损压缩jpg图片
      interlaced: true,
      // 类型：Boolean 默认：false 隔行扫描gif进行渲染
      multipass: true // 类型：Boolean
      // 默认：false 多次优化svg直到完全优化
    }))
    .pipe(gulp.dest('./dist/asset'));
}

// js 压缩添加版本
function js(cb) {
  return gulp
    .src(['./src/**/*.js', '!./src/lib/**'])
    .pipe(eslint())
    .pipe(eslint.results(results => {
      // Called once for all ESLint results.
      console.log(`JS总校验文件: ${results.length}`);
      console.log(`JS警告个数：: ${results.warningCount}`);
      console.log(`JS错误个数: ${results.errorCount}`);
    }))
    .pipe(eslint.format())
    .pipe(eslint.failAfterError())
    .pipe(babel({presets: ['env']}))
    .pipe(uglify())
    .pipe(rev())
    .pipe(gulp.dest('./dist'))
    .pipe(rev.manifest())
    .pipe(gulp.dest('./src/js/'));
}

// 给requirejs引用的文件修改版本号的路径
function revjs() {
  return gulp
    .src('./dist/**/*.js')
    .pipe(configRevReplace({
      manifest: gulp.src('./src/js/rev-manifest.json')
    }))
    .pipe(uglify())
    .pipe(gulp.dest('dist/'));
}

// 打包要复制的路径2
var copyPathArr = ['./src/lib/**/*', './src/asset/**/*', './src/*.ico'];
// 拷贝gulp文件
function copy(cb) {
  return gulp
    .src(copyPathArr, {base: './src'})
    .pipe(gulp.dest('./dist/'));
}
// 清理dist目录下的历史文件
function cleanDist(cb) {
  return gulp.src([
    'dist/style/**', 'dist/js/**', 'dist/*.js'
  ], {read: false}).pipe(clean());
}


function tpl(cb) {
  // 拿到所有的路径
  let basePath = path.join(__dirname, 'src/template');
  let files = fs.readdirSync(basePath);
  files.forEach((val, index) => {
    let dirPath = path.join(basePath, val);
    let stat = fs.statSync(dirPath);
    if (!stat.isDirectory()) {
      return;
    }
    var fileter = 'src/template/' + val + '/**/*.html';
    console.log(fileter);
    gulp
      .src('src/template/' + val + '/**/*.html')
      .pipe(tmodjs({
        templateBase: 'src/template/' + val,
        runtime: val + '.js',
        compress: false
      }))
      // 自动生成的模板文件，进行babel转换，会报错，此转换插件已经停更，所以间接改这个bug
      // 参考bug：https://github.com/aui/tmodjs/issues/112 主要是this  →  window
      .pipe(replace('var String = this.String;', 'var String = window.String;'))
      .pipe(gulp.dest('src/js/tmpl/'));
  })
  cb();
}

// ============= 开发样式处理
function style_dev(e) {
  var sassFilter = filter(['**/*.scss'], {restore: true});
  return gulp.src(['./src/style/**/*.{scss,css}', '!./src/style/main.css']) // 读取sass文件
    .pipe(sourcemaps.init())
    .pipe(sassFilter)
    .pipe(sass()) // 编译sass
    .pipe(sassFilter.restore)
    .pipe(sass())
    .pipe(concat('main.css'))
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('./src/style/'));
}

// 开发监控sass文件变化编译sass到css文件 可以增加es6的编译，jslint
gulp.task('dev', gulp.series( devServer, openBrowser,  function() {
  gulp.watch('src/style/**', gulp.series(style_dev));
  gulp.watch('src/template/**', gulp.series(tpl));
}))

// 配置测试服务器
function devServer(cb) {
  connect.server({
    root: ['./src'], // 网站根目录
    port: 38900, // 端口
    livereload: true,
    middleware: function (connect, opt) {
      return [modRewrite([// 设置代理
          '^/api/(.*)$ http://140.143.64.118:8082/mockjsdata/1/api/$1 [P]'])];
    },
  });
  cb();
}

// 启动浏览器打开地址
function openBrowser() {
  return  gulp
  .src(__filename)
  .pipe(open({uri: 'http://localhost:38900/index.html'}));
}

gulp.task('dist', gulp.series(cleanDist, tpl, style, js, revjs, html, copy, imgmin))

```
