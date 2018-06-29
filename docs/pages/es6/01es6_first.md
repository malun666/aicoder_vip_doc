# ES6 的概述

首先，感谢阮一峰老师的[《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)开源的 JavaScript 语言教程，此教程全面介绍 ECMAScript 6 新引入的语法特性。建议大家购买纸质版，支持阮老师。

## ECMAScript 和 JavaScript 的关系是

ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现（另外的 ECMAScript 方言还有 Jscript 和 ActionScript）。日常场合，这两个词是可以互换的。

## js 历史

- 1997 年 ECMA 发布 262 号标准文件（ECMA-262）,也就是 ECMAScript1.0
- ECMAScript 2.0（1998 年 6 月）
- ECMAScript 3.0（1999 年 12 月）
- 2000 年，ECMAScript 4.0，社区分裂，没有形成统一的标准
- 2008 年 7 月，ECMA 中止 ECMAScript 4.0 的开发，发布 ECMAScript 3.1，后来改名为 ECMAScript 5
- 2011 年 6 月，ECMAscript 5.1 版发布，并且成为 ISO 国际标准
- 2015 年 6 月，ECMAScript 6 正式通过，成为国际标准。

关于 ES2015 和 ES6 的区别：

ES6 的后续小改的版本，不通过 ES6.0 、ES6.1、ES6.2 ... 来命名,而是通过年份来命名：

- ES6 的第一个版本是在 2015 年 6 月发布的 ES2015。
- 2016 年 6 月，小幅修订的《ECMAScript 2016 标准》（简称 ES2016）如期发布，这个版本可以看作是 ES6.1
- 2017 年 6 月发布 ES2017 标准。

## 参与 ES6 的标准贡献

**任何人**都可以向标准委员会（又称 TC39 委员会）提案。

一个提案成为标准经历如下阶段，每个阶段的变动都需要由 TC39 委员会批准。

- Stage 0 - Strawman（展示阶段）
- Stage 1 - Proposal（征求意见阶段）
- Stage 2 - Draft（草案阶段）
- Stage 3 - Candidate（候选人阶段）
- Stage 4 - Finished（定案阶段）

可以在 TC39 的官方网站 [Github.com/tc39/ecma262](https://github.com/tc39/ecma262) 查看。

## 最新的 ES 的兼容情况

目前最新的浏览器已经支持 ES6 达到 96%以上了。

查看各个浏览器及 babel 和服务器端的兼容情况汇总：[http://kangax.github.io/compat-table/es6/](http://kangax.github.io/compat-table/es6/)

## 参考

[《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)

# [回到ES6知识列表首页](/pages/vip_2ES6.md)