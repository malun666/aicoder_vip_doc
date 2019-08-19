# TypeScript 入门教程

`TypeScript`是由微软推出并推动的一个静态类型检查的编译型编程语言，可以完美兼容ES6的JavaScript，目前大量前端库和应用使用TS开发，已然成为前端的主流开发语言。

优势：

- 静态编译，减少JS动态语言特性带来了的非常多的隐藏bug。
- 类型安全检查在编译阶段完成
- 大型项目的更好的进行管理和向后约定维护
- 为前端的彻底的静态安全的类型检查、类型继承、抽象设计体系铺好了基石
- 更加友好的智能提示和文档说明
- 大团的的开发协助更友好
- 社区活跃，大厂推动

## 安装TypeScript

有两种主要的方式来获取`TypeScript`工具：

- 通过`npm`（`Node.js`包管理器）
- 安装`Visual Studio`的`TypeScript`插件

> Visual Studio 2017和Visual Studio 2015 Update 3默认包含了TypeScript。 如果你的Visual Studio还没有安装TypeScript，你可以[下载](https://www.tslang.cn/#download-links)它。

针对使用`npm`的用户：

```sh
npm install -g typescript
```

## 构建你的第一个`TypeScrip`t文件

### 编写第一个ts文件

在编辑器，将下面的代码输入到`greeter.ts`文件里：

```js
function greeter(person) {
    return "Hello, " + person;
}

let user:String = "Jane User"; // 此时设定user变量为String类型

document.body.innerHTML = greeter(user);
```

### 编译代码

在命令行上，运行TypeScript编译器：

```sh
tsc greeter.ts
```

输出结果为一个`greeter.js`文件，它包含了和输入文件中相同的`JavsScript`代码。 一切准备就绪，我们可以运行这个使用`TypeScript`写的`JavaScript`应用了！

> 其实，TypeScript完全兼容ES6的语法，所以使用TS完全可以无缝从JS切入。

## 编译高级

### 命令行编译

命令行编译的语法：

```sh
tsc [options] [file ...]
```

#### 启动监听编译

启动编译后，可以添加`-w`参数，让编译工具一直监听文件的变化，ts文件变化后自动编译。

```sh
tsc -w greeter.ts
# 或者

tsc --watch greeter.ts
```

> 如果编译的是一个目录，也会全部监听整个目录。

#### 编译多个文件

```sh
# 编译当前目录下的所有的ts文件
tsc -w *.ts

# 编译当前目录及子目录的ts文件
tsc -w ./**/*.ts
```

#### 构建SourceMap文件

```sh
tsc -w --sourceMap ./**/*.ts
```

#### 指定输出文件

可以通过选项： `--outFile FILE`控制编译完成后所有代码合并到一个输出文件中。

```sh
tsc --sourceMap --outFile main.js -m amd ./**/*.ts
```

> `-m amd`是设置编译输出的js符合amd的模块标准规范。

#### 其他各种编译选项配置表

当然还有其他各种参数，见下表：

## 编译选项

选项                                     | 类型      | 默认值                    | 描述
----------------------------------------|-----------|--------------------------|----------------------------------------------------------------------
`--allowJs`                             | `boolean` |  `false`                 | 允许编译javascript文件。
`--allowUnreachableCode`                | `boolean` | `false`                  | 不报告执行不到的代码错误。
`--allowUnusedLabels`                   | `boolean` | `false`                  | 不报告未使用的标签错误。
`--alwaysStrict`                        | `boolean` | `false`                  | 以严格模式解析并为每个源文件生成`"use strict"`语句
`--baseUrl`                             | `string`  |                          | 解析非相对模块名的基准目录。查看
`--build`<br/>`-b`                      | `boolean` | `false`                  | 
`--charset`                             | `string`  | `"utf8"`                 | 输入文件的字符集。
`--checkJs`                             | `boolean` | `false`                  | 在.js文件中报告错误。与`--allowJs`配合使用。
`--composite`                           | `boolean` | `true`                   | 确保TypeScript能够找到编译当前工程所需要的引用工程的输出位置。
`--declaration`<br/>`-d`                | `boolean` | `false`                  | 生成相应的`.d.ts`文件。
`--declarationDir`                      | `string`  |                          | 生成声明文件的输出路径。
`--diagnostics`                         | `boolean` | `false`                  | 显示诊断信息。
`--disableSizeLimit`                    | `boolean` | `false`                  | 禁用JavaScript工程体积大小的限制
`--emitBOM`                             | `boolean` | `false`                  | 在输出文件的开头加入BOM头（UTF-8 Byte Order Mark）。
`--extendedDiagnostics`                 | `boolean` | `false`                  | 显示详细的诊段信息。
`--help`<br/>`-h`                       |           |                          | 打印帮助信息。
`--importHelpers`                       | `string`  |                          | 从[`tslib`](https://www.npmjs.com/package/tslib)导入辅助工具函数（比如`__extends`，`__rest`等）
`--inlineSourceMap`                     | `boolean` | `false`                  | 生成单个sourcemaps文件，而不是将每sourcemaps生成不同的文件。
`--inlineSources`                       | `boolean` | `false`                  | 将代码与sourcemaps生成到一个文件中，要求同时设置了`--inlineSourceMap`或`--sourceMap`属性。
`--init`                                |           |                          | 初始化TypeScript项目并创建一个`tsconfig.json`文件。
`--isolatedModules`                     | `boolean` | `false`                  | 将每个文件作为单独的模块（与“ts.transpileModule”类似）。
`--jsx`                                 | `string`  | `"preserve"`             | 在`.tsx`文件里支持JSX：`"react"`或`"preserve"`或`"react-native"`。
`--jsxFactory`                          | `string`  |   | 指定生成目标为react JSX时，使用的JSX工厂函数，比如`React.createElement`或`h`。
`--lib`                                 | `string[]`|                          | 编译过程中需要引入的库文件的列表。<br/>可能的值为：  <br/>► `ES5` <br/>► `ES6` <br/>► `ES2015` <br/>► `ES7` <br/>► `ES2016` <br/>► `ES2017`  <br/>► `ES2018` <br/>► `ESNext` <br/>► `DOM` <br/>► `DOM.Iterable` <br/>► `WebWorker` <br/>► `ScriptHost` <br/>► `ES2015.Core` <br/>► `ES2015.Collection` <br/>► `ES2015.Generator` <br/>► `ES2015.Iterable` <br/>► `ES2015.Promise` <br/>► `ES2015.Proxy` <br/>► `ES2015.Reflect` <br/>► `ES2015.Symbol` <br/>► `ES2015.Symbol.WellKnown` <br/>► `ES2016.Array.Include` <br/>► `ES2017.object` <br/>► `ES2017.Intl` <br/>► `ES2017.SharedMemory` <br/>► `ES2017.String` <br/>► `ES2017.TypedArrays` <br/>► `ES2018.Intl` <br/>► `ES2018.Promise` <br/>► `ES2018.RegExp` <br/>► `ESNext.AsyncIterable` <br/>► `ESNext.Array` <br/>► `ESNext.Intl` <br/>► `ESNext.Symbol` <br/><br/> 注意：如果`--lib`没有指定默认注入的库的列表。默认注入的库为：<br/> ► 针对于`--target ES5`：`DOM，ES5，ScriptHost`<br/>  ► 针对于`--target ES6`：`DOM，ES6，DOM.Iterable，ScriptHost`
`--listEmittedFiles`                    | `boolean` | `false`                  | 打印出编译后生成文件的名字。
`--listFiles`                           | `boolean` | `false`                  | 编译过程中打印文件名。
`--locale`                              | `string`  | *(platform specific)*    | 显示错误信息时使用的语言，比如：en-us。
`--mapRoot`                             | `string`  |                          | 为调试器指定指定sourcemap文件的路径，而不是使用生成时的路径。当`.map`文件是在运行时指定的，并不同于`js`文件的地址时使用这个标记。指定的路径会嵌入到`sourceMap`里告诉调试器到哪里去找它们。
`--maxNodeModuleJsDepth`                | `number`  | `0`                      | node_modules依赖的最大搜索深度并加载JavaScript文件。仅适用于`--allowJs`。
`--module`<br/>`-m`                     | `string`  |  "commonjs"| 指定生成哪个模块系统代码：`"None"`，`"CommonJS"`，`"AMD"`，`"System"`，`"UMD"`，`"ES6"`或`"ES2015"`。<br/>► 只有`"AMD"`和`"System"`能和`--outFile`一起使用。<br/>►`"ES6"`和`"ES2015"`可使用在目标输出为`"ES5"`或更低的情况下。
`--newLine`                             | `string`  | *(platform specific)*    | 当生成文件时指定行结束符：`"crlf"`（windows）或`"lf"`（unix）。
`--noEmit`                              | `boolean` | `false`                  | 不生成输出文件。
`--noEmitHelpers`                       | `boolean` | `false`                  | 不在输出文件中生成用户自定义的帮助函数代码，如`__extends`。
`--noEmitOnError`                       | `boolean` | `false`                  | 报错时不生成输出文件。
`--noErrorTruncation`                   | `boolean` | `false`                  | 不截短错误消息。
`--noImplicitAny`                       | `boolean` | `false`                  | 在表达式和声明上有隐含的`any`类型时报错。
`--noImplicitReturns`                   | `boolean` | `false`                  | 不是函数的所有返回路径都有返回值时报错。
`--noImplicitThis`                      | `boolean` | `false`                  | 当`this`表达式的值为`any`类型的时候，生成一个错误。
`--noImplicitUseStrict`                 | `boolean` | `false`                  | 模块输出中不包含`"use strict"`指令。
`--noLib`                               | `boolean` | `false`                  | 不包含默认的库文件（`lib.d.ts`）。
`--noResolve`                           | `boolean` | `false`                  | 不把`/// <reference``>`或模块导入的文件加到编译文件列表。
`--noStrictGenericChecks`               | `boolean` | `false`                  | 禁用在函数类型里对泛型签名进行严格检查。
`--noUnusedLocals`                      | `boolean` | `false`                  | 若有未使用的局部变量则抛错。
`--noUnusedParameters`                  | `boolean` | `false`                  | 若有未使用的参数则抛错。
~~`--out`~~                             | `string`  |                          | 弃用。使用 `--outFile` 代替。
`--outDir`                              | `string`  |                          | 重定向输出目录。
`--outFile`                             | `string`  |                          | 将输出文件合并为一个文件。合并的顺序是根据传入编译器的文件顺序和`///<reference``>`和`import`的文件顺序决定的。查看输出文件顺序文档[了解详情](https://github.com/Microsoft/TypeScript/wiki/FAQ#how-do-i-control-file-ordering-in-combined-output---out-)。
`paths`<sup>[2]</sup>                   | `Object`  |                          | 模块名到基于`baseUrl`的路径映射的列表。
`--preserveConstEnums`                  | `boolean` | `false`                  | 保留`const`和`enum`声明。查看[const enums documentation](https://github.com/Microsoft/TypeScript/blob/master/doc/spec.md#94-constant-enum-declarations)了解详情。
`--preserveSymlinks`                    | `boolean` | `false`                  | 不把符号链接解析为其真实路径；将符号链接文件视为真正的文件。
`--preserveWatchOutput`                 | `boolean` | `false`                  | 保留watch模式下过时的控制台输出。
`--pretty`<sup>[1]</sup>                | `boolean` | `false`                  | 给错误和消息设置样式，使用颜色和上下文。
`--project`<br/>`-p`                    | `string`  |                          | 编译指定目录下的项目。这个目录应该包含一个`tsconfig.json`文件来管理编译。
`--reactNamespace`                      | `string`  | `"React"`                | 当目标为生成`"react"` JSX时，指定`createElement`和`__spread`的调用对象
`--removeComments`                      | `boolean` | `false`                  | 删除所有注释，除了以`/!*`开头的版权信息。
`--rootDir`                             | `string`  | | 仅用来控制输出的目录结构`--outDir`。
`rootDirs`<sup>[2]</sup>                | `string[]`|                          | <i>根（root）</i>文件夹列表，表示运行时组合工程结构的内容。
`--showConfig`                          | `boolean` | `false`                  | 不真正执行build，而是显示build使用的配置文件信息。
`--skipDefaultLibCheck`                 | `boolean` | `false`                  | 忽略[库的默认声明文件]的类型检查。
`--skipLibCheck`                        | `boolean` | `false`                  | 忽略所有的声明文件（`*.d.ts`）的类型检查。
`--sourceMap`                           | `boolean` | `false`                  | 生成相应的`.map`文件。
`--sourceRoot`                          | `string`  |                          | 指定TypeScript源文件的路径，以便调试器定位。当TypeScript文件的位置是在运行时指定时使用此标记。路径信息会被加到`sourceMap`里。
`--strict`                              | `boolean` | `false`                  | 启用所有严格检查选项。 <br/>包含`--noImplicitAny`, `--noImplicitThis`, `--alwaysStrict`, `--strictBindCallApply`, `--strictNullChecks`, `--strictFunctionTypes`和`--strictPropertyInitialization`.
`--strictFunctionTypes`                 | `boolean` | `false`                  | 禁用函数参数双向协变检查。
`--strictNullChecks`                    | `boolean` | `false`                  | 在严格的`null`检查模式下，`null`和`undefined`值不包含在任何类型里，只允许用它们自己和`any`来赋值（有个例外，`undefined`可以赋值到`void`）。
`--target`<br/>`-t`                     | `string`  | `"ES3"`                  | 指定ECMAScript目标版本`"ES3"`（默认），`"ES5"`，`"ES6"`/`"ES2015"`，`"ES2016"`，`"ES2017"`或`"ESNext"`。<br/><br/> 注意：`"ESNext"`最新的生成目标列表为[ES proposed features](https://github.com/tc39/proposals)
`--traceResolution`                     | `boolean` | `false`                  | 生成模块解析日志信息
`--types`                               | `string[]`|                          | 要包含的类型声明文件名列表。
`--typeRoots`                           | `string[]`|                          | 要包含的类型声明文件路径列表。
`--version`<br/>`-v`                    |           |                          | 打印编译器版本号。
`--watch`<br/>`-w`                      |           |                          | 在监视模式下运行编译器。会监视输出文件，在它们改变时重新编译。监视文件和目录的具体实现可以通过环境变量进行配置。

* <sup>[1]</sup> 这些选项是试验性的。
* <sup>[2]</sup> 这些选项只能在`tsconfig.json`里使用，不能在命令行使用。

## 相关信息

* 在[`tsconfig.json`](./tsconfig.json.md)文件里设置编译器选项。
* 在[MSBuild工程](./Compiler%20Options%20in%20MSBuild.md)里设置编译器选项。

### 编译上下文

编译上下文算是一个比较花哨的术语，它用来给文件分组，告诉 TypeScript 哪些文件是有效的，哪些是无效的。除了有效文件所携带信息外，编译上下文也包含了有哪些编译选项正在使用。定义这种逻辑分组，一个比较好的方式是使用 `tsconfig.json` 文件。

#### 初始化ts编译配置文件

```sh
tsc --init
```

> 项目根目录下执行以上命令，会在根目录下创建一个完整的配置文件。例如：

```json
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    // "lib": [],                             /* Specify library files to be included in the compilation. */
    // "allowJs": true,                       /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    // "outDir": "./",                        /* Redirect output structure to the directory. */
    // "rootDir": "./",                       /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    // "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                 /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */
    // "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true                   /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,          /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */
  }
}
```

你可以通过 `compilerOptions` 来定制你的编译选项：

```js
{
  "compilerOptions": {

    /* 基本选项 */
    "target": "es5",                       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
    "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "lib": [],                             // 指定要包含在编译中的库文件
    "allowJs": true,                       // 允许编译 javascript 文件
    "checkJs": true,                       // 报告 javascript 文件中的错误
    "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
    "declaration": true,                   // 生成相应的 '.d.ts' 文件
    "sourceMap": true,                     // 生成相应的 '.map' 文件
    "outFile": "./",                       // 将输出文件合并为一个文件
    "outDir": "./",                        // 指定输出目录
    "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
    "removeComments": true,                // 删除编译后的所有的注释
    "noEmit": true,                        // 不生成输出文件
    "importHelpers": true,                 // 从 tslib 导入辅助工具函数
    "isolatedModules": true,               // 将每个文件做为单独的模块 （与 'ts.transpileModule' 类似）.

    /* 严格的类型检查选项 */
    "strict": true,                        // 启用所有严格类型检查选项
    "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
    "strictNullChecks": true,              // 启用严格的 null 检查
    "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
    "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

    /* 额外的检查 */
    "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
    "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
    "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
    "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

    /* 模块解析选项 */
    "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
    "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
    "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
    "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
    "typeRoots": [],                       // 包含类型声明的文件列表
    "types": [],                           // 需要包含的类型声明文件名列表
    "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

    /* Source Map Options */
    "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
    "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
    "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
    "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

    /* 其他选项 */
    "experimentalDecorators": true,        // 启用装饰器
    "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
  }
}

#### 启动编译文件配置编译

好的 IDE 支持对 TypeScript 的即时编译。但是，如果你想在使用 `tsconfig.json` 时从命令行手动运行 TypeScript 编译器，你可以通过以下方式：

- 运行 tsc，它会在当前目录或者是父级目录寻找 `tsconfig.json` 文件。
- 运行 `tsc -p ./path-to-project-directory` 。当然，这个路径可以是绝对路径，也可以是相对于当前目录的相对路径。

你甚至可以使用 `tsc -w` 来启用 TypeScript 编译器的观测模式，在检测到文件改动之后，它将重新编译。

#### 编译文件配置

你也可以显式指定需要编译的文件：

```js
{
  "files": [
    "./some/file.ts"
  ]
}
```

或者，你可以使用 `include` 和 `exclude` 选项来指定需要包含的文件，和排除的文件：

```js
{
  "include": [
    "./folder"
  ],
  "exclude": [
    "./folder/**/*.spec.ts",
    "./folder/someSubFolder"
  ]
}
```

> tip 注意
> 使用 `globs`：`**/*` （一个示例用法：`some/folder/**/*`）意味着匹配所有的文件夹和所有文件（扩展名为 `.ts/.tsx`，当开启了 `allowJs: true` 选项时，扩展名可以是 `.js/.jsx`）。

## 基础类型

简单的数据类型：数字，字符串，结构体，布尔值等。 `TypeScript`支持与`JavaScript`几乎相同的数据类型，此外还提供了枚举类型。

### 布尔值

最基本的数据类型就是简单的`true/false`值，在`JavaScript`和`TypeScript`里叫做`boolean`（其它语言中也一样）。

```js
let isDone: boolean = false; // 冒号后面是对当前变量类型的声明，isDone只能赋值布尔类型值，否则会编译失败。
```

### 数字

和`JavaScrip`t一样，`TypeScript`里的所有数字都是浮点数。 这些浮点数的类型是 `number`。 除了支持十进制和十六进制字面量，`TypeScript`还支持`ECMAScript 2015`中引入的二进制和八进制字面量。

```js
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

### 字符串

`JavaScript`程序的另一项基本操作是处理网页或服务器端的文本数据。 像其它语言里一样，我们使用 `string`表示文本数据类型。 和`JavaScript`一样，可以使用双引号（ "）或单引号（'）表示字符串。

```js
let name: string = "bob";
name = "smith";
```

你还可以使用模版字符串，它可以定义多行文本和内嵌表达式。 这种字符串是被反引号包围（ `），并且以${ expr }这种形式嵌入表达式

```js
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.`;
```

这与下面定义`sentence`的方式效果相同：

```js
let sentence: string = "Hello, my name is " + name + ".\n\n" +
    "I'll be " + (age + 1) + " years old next month.";
```

### 数组

`TypeScript`像`JavaScript`一样可以操作数组元素。 有两种方式可以定义数组。 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组：

```js
let list: number[] = [1, 2, 3];
```

第二种方式是使用数组泛型，Array<元素类型>：

```js
let list: Array<number> = [1, 2, 3];
```

### 元组 Tuple

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 `string`和`number`类型的元组。

```js
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

当访问一个已知索引的元素，会得到正确的类型：

```js
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
```

当访问一个越界的元素，会使用联合类型替代：

```js
x[3] = 'world'; // OK, 字符串可以赋值给(string | number)类型

console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString

x[6] = true; // Error, 布尔不是(string | number)类型
联合类型是高级主题，我们会在以后的章节里讨论它。
```

### 枚举

`enum`类型是对`JavaScript`标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```js
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1开始编号：

```js
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
```

或者，全部都采用手动赋值：

```js
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
```

枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字：

```js
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```

### Any类型

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any类型来标记这些变量：

```js
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

在对现有代码进行改写的时候，any类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。 你可能认为 Object有相似的作用，就像它在其它语言中那样。 但是 Object类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法：

```js
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

当你只知道一部分数据的类型时，any类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```js
let list: any[] = [1, true, "free"];

list[1] = 100;
```

### Void

某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void：

```js
function warnUser(): void {
    console.log("This is my warning message");
}
```

声明一个void类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`：

```js
let unusable: void = undefined;
```

### Null 和 Undefined

`TypeScript`里，undefined和null两者各自有自己的类型分别叫做undefined和null。 和 void相似，它们的本身的类型用处不是很大：

```js
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。

然而，当你指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自。 这能避免 很多常见的问题。 也许在某处你想传入一个 string或null或undefined，你可以使用联合类型string | null | undefined。 再次说明，稍后我们会介绍联合类型。

注意：我们鼓励尽可能地使用--strictNullChecks，但在本手册里我们假设这个标记是关闭的。

### Never

never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。

下面是一些返回never类型的函数：

```js
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

### Object

object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。

使用object类型，就可以更好的表示像Object.create这样的API。例如：

```js
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### 类型断言

有时候你会遇到这样的情况，你会比`TypeScript`更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 `TypeScript`会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```js
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一个为as语法：

```js
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在`TypeScript`里使用JSX时，只有 as语法断言是被允许的。

### 关于let

你可能已经注意到了，我们使用let关键字来代替大家所熟悉的`JavaScript`关键字var。 let关键字是`JavaScript`的一个新概念，`TypeScript`实现了它。 我们会在以后详细介绍它，很多常见的问题都可以通过使用 let来解决，所以尽可能地使用let来代替var吧。
