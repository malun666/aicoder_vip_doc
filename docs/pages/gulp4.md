# gulp升级到4.0指南

早在 2015 年 5 月 `gulpjs` 开发团队就已经释出了 4.0 分支，但是直到最近才发布到npm，之前默认安装的是`gulp@3.9.1`。然而 `gulp@3.9.1` 存在高危漏洞，而 `npm audit fix` 时就升级到了 `gulp@4`。

## 任务task的api的变化

之前用 gulp 3.x 时指定 Default Task 的写法一般是这样：

```js
gulp.task('open', ['devServer'], function() {
  gulp.src(__filename).pipe(open({ uri: 'http://localhost:38900/index.html' }));
});
```

`task`接受三个参数，其中第二个参数是依赖的前置任务。但是 `gulp@4.0` 取消了 `gulp.task` 的三个参数的用法，所以在 `gulp@4.0` 时再这么写就会遇到如下报错：

```sh
AssertionError [ERR_ASSERTION]: Task function must be specified
```

为了解决任务的依赖问题，引入了`gulp.parallel`和`gulp.series`分别执行并行任务和串行任务。

注册task的方法更新为：

```js
gulp.task('open', function() {
  // ...
});
```

在 `gulp@4.0` 中，`gulp.task` 又增加了一种新的用法，可以传递一个具名函数作为参数，`gulp` 会自动为其注册一个 `taskname`。

```js
function compile()  {
    gulp.src('./src/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./dist/js'))
}
gulp.task(compile);
```

> 除非你确实需要使用`gulp taskname`在命令行执行单独的任务命令。否则不需要再显示的给定taskname。
> 另外带来的是：一个任务变成了一个函数，可以直接在任务中作为一个函数调用。异常方便。

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

## 其他迁移

gulp@4.0 带来许多焕然一新的改动，除了 gulp.series 和 gulp.parallel 两个革命性的 API、支持使用具名函数注册 task 以外，还带来了 gulp.tree gulp.registry ，加上 gulp 4.0 还修复了一大堆 gulp 3.x 遗留下来的高危漏洞，意味着如果你现在的项目还在使用 gulp 3.x，是时候更新到 gulp 4.0 了。

迁移旧有的项目到 gulp 4.0 并不困难，因为除了 gulp.task 不再支持三参数传递以外，gulp 4.0 向下兼容。所以你只需要像前文简单修改一下 gulp.task 的注册方法即可完成对现有的项目的迁移。但是为了发挥出 gulp 4.0 的强大能力，还是建议阅读一下 官方升级日志 和 gulp 4.0 API docs，然后重新编写你的 gulpfile.js 吧！