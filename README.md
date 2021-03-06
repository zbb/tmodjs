#	TmodJS-前端模板编译工具

######	像后端一样书写前端模板

##	关于 TmodJS

TmodJS（原名 atc）是一款前端模板编译工具，它可以让前端模板外置、实现类似后端模板一样按文件与目录组织前端模板，并且模板之间可以使用``include``语句相互包含。

###	像后端一样书写前端模板

相对与前端模板，后端模板有两个优秀的特征：

1.	模板按文件与目录组织
2.	模板之间可以相互引用

通过 TmodJS 预编译技术让前端模板突破浏览器的文本文件加载限制，支持模板按文件存放，并且支持多层目录组织模板，并且模板之间可通过``include``语句进行复用。

###	模板编译输出为 js 文件

它会将模板编译成 js 文件，编译后的代码体积非常小，不包含模板引擎也无需依赖脚本加载器。因为是预编译，省去前端模板客户端动态编译过程，也能够在移动设备中节省一定的系统资源。

编译成 js 后，原来字符串模板遇到的跨域加载、异步按需加载已经不是问题，甚至可以接入 GruntJS 等自动化工具进行部署。

###	支持接入 AMD 或者 CMD 部署工具

默认情况下，模板目录将会被打包成 js, 可以直接在页面中使用传统 Script 标签加载，简单且有效。除此之外还可以设置将单个模板文件输出为单个的 AMD 、CMD 异步模块，以便接入它们自动化部署工具进行深度定制化的优化，这样可以实现按需加载、合并等高级的优化手段。

###	横跨前后端运行

支持输出基于 NodeJS 的同步规范模块，前后端可轻松的共用同一套模板。

###	动态调试支持

支持输出调试版本，模板运行中错误可精确到模板源文件所在行。

###	即时编译

支持设置检测模板的修改进行即时编译，这样几乎可以忽略编译过程的存在，模板编写只需要这两个步骤：1、修改模板并保存 2、刷新浏览器预览效果

##	安装

先安装 [NodeJS](http://nodejs.org) 与 npm (最新版 NodeJS 已经附带 npm)，执行：

```
$ npm install tmodjs -g
```	

> Mac OSX 可能需要管理员权限运行： ``$ sudo npm install tmodjs -g``


##	快速入门

学习 TmodJS 只需要理解这四个关键点就好，7分钟可入门：

1. 建立模板目录：TmodJS 是基于目录编译，你至少要给项目建立一个专用的前端模板目录
2. 编写模板：你需要了解 TmodJS 的模板语句，如输出变量、条件判断、循环等
3. 编译模板：编译模板你需要知道如何使用命令参数
4. 调用模板：如何在项目中加载模板

###	一、建立模板目录

TmodJS 的前端模板不再耦合在业务页面中，而是和后端模板一样有专门的目录管理。目录名称支持英文、数字、下划线。

###	二、编写模板

一个模板对应一个文件，模板后缀可以是``.html``、``.htm``、``.tpl``。

模板支持输出变量、条件判断、循环、包含子模板，请查看：[模板语法参考](https://github.com/aui/tmodjs/wiki/模板语法)

###	三、编译模板

```
$ tmod [path] [options]
```

其中 path 是模板目录。TmodJS 基于目录进行处理。

####	options
            
*	``-w``或``--watch``设置监控模板修改触发编译
*	``-d``或``--debug``输出调试版本
*	``--charset value``定义模板编码，默认``utf-8``
*	``--output value``定义输出目录，默认``./build``
*	``--type value``定义输出模块格式，默认``templatejs``，可选``cmd``、``amd``、``commonjs``
*	``--version``显示 TmodJS 版本号
*	``--help``显示帮助信息

运行完成后，程序会将模板编译成 js 文件。如果你经常需要修改模板，可以开启``-w``参数，让它检测修改自动编译。

>	首次运行后，会对模板根目录进行初始化，生成 package.json，也可以编辑它进行更多配置，包括语法、公用辅助方法、压缩选项等，参考[配置](#配置)。
>	当模板目录初始化后，下次编译模板可以无需输入配置参数，将沿用上一次的参数进行编译（-w 与 -d 除外）。

###	四、调用模板

模板编译后，模板目录会生成 build 子目录，里面包含了所有的模板编译版本。其中 build/template.js 是压缩后的模板包，通常情况下你只需要在页面中引入它就好。例如：

```
<script src="tpl/build/template.js"></script>
```

这是默认的加载方式，除此之外还支持 RequireJS、SeaJS、NodeJS 加载。[示例](http://aui.github.io/tmodjs/test/index.html)
	
####	模板接口

```
template(path, data)
```

path 参数是**模板目录相对路径**，并且**不带后缀名**，例如 ：

```	
var html = template('news/list', {hot: [...]});
document.getElementById('list').innerHTML = html;
```

##	编译演示项目

源码包中 ./test 是一个演示项目，./test/tpl 是项目的模板目录，包含了若干模板。你可以通过这个演示项目快速了解 TmodJS 用法以及模板语法、模板加载方式。

首先，使用 cd 命令切换到 TmodJS 目录后，你可以编译这个目录模板：

```
$ tmod ./test/tpl
```

编译完毕后你可以在浏览器中打开 [test/index.html](http://aui.github.io/tmodjs/test/index.html) 查看如何加载模板。

## 对外接口

若想集成 TModJS 到其它自动化工具中（如 GruntJS），可以使用 TModJS 提供的 API：

```
var TmodJS = require('tmodjs');

// 模板目录
var path = './demo/templates';

// 配置
var options = {
	output: './build',
	charset: 'utf-8',
	debug: false // 此字段不会保存在配置中
};

// 初始化 TmodJS
// path {String}	模板目录
// options {Object} 选项
TmodJS.init(path, options);

// 编译模板
// file {String} 参数可选，无则编译整个模板目录，否则编译指定的模板文件
// recursion {Boolean} 若为 false 则不编译依赖的模板
TmodJS.compile(file, recursion);

// 监控模板修改
TmodJS.watch();

// 保存用户设置到模板目录 package.json 文件中
TmodJS.saveUserConfig();

// 监听编译过程的事件
// 支持：change、load、compileError、combo
TmodJS.on('compile', function (data) {});
```	

##	配置

配置最终会保存在模板目录的 package.json 文件中，可以对它直接修改。

```
{
    // 编译输出目录设置
    output: './build',

    // 模板使用的编码。（注意：非 utf-8 编码的模板缺乏测试）
    charset: 'utf-8',

    // 模板合并规则
    // 注意：type 参数的值为 templatejs 才会生效
    combo: ['*'],

    // 定义模板采用哪种语法，可选：
    // simple: 默认语法，易于读写。可参看语法文档
    // native: 功能丰富，灵活多变。语法类似微型模板引擎 tmpl
    syntax: 'simple',

    // 自定义辅助方法路径
    helpers: null,

    // 是否输出为压缩的格式
    minify: true,

    // 是否内嵌异步加载插件（beta）
    // 可以支持 template.async(path, function (render) {}) 方式异步载入模板
    // 注意：type 参数是 templatejs 的时候才生效
    async: false,

    // 是否嵌入模板引擎，否则编译为不依赖引擎的纯 js 代码
    // 通常来说，模板不多的情况下，编译为原生的 js 打包后体积更小，因为不必嵌入引擎
    // 当模板很多的时候，内置模板引擎，模板使用字符串存储的方案会更能节省空间
    engine: false,

    // 输出的模块类型（不区分大小写），可选：
    // templatejs:  模板目录将会打包后输出，可使用 script 标签直接引入，也支持 NodeJS/RequireJS/SeaJS。
    // cmd:         这是一种兼容 RequireJS/SeaJS 的模块（类似 atc v1版本编译结果）
    // amd:         支持 RequireJS 等流行加载器
    // commonjs:    编译为 NodeJS 模块
    type: 'templatejs'     
}
```
	
##	常见问题

**问**：TmodJS 需要部署到服务器中吗？

> 不需要，这是本地工具，基于 NodeJS 编写是为了实现跨平台。

**问**：如何将每个模板都编译成单独的 amd/cmd 模块输出？

> 指定 type 参数即可，如``--type cmd``则可以让每个模板都支持 RequireJS/SeaJS 调用。

**问**：如何将模板编译成 NodeJS 的模块？

> 指定 type 参数即可，如``--type commonjs``。

**问**：线上运行的模板报错了如何调试？

> 开启 debug 模式编译，如``-d``，这样会输出调试版本，可以让你快速找到模板运行错误的语句以及数据。

**问**：如何不压缩输出 js？

> 编辑配置文件，设置``"minify": false``。

**问**：如何修改默认的输出目录？

> 指定 output 参数即可，如``--output ../../build``。

**问**：如何让模板访问全局变量？

> 请参考：[辅助方法](https://github.com/aui/tmodjs/wiki/辅助方法)。

**问**：可以使用使用类似 tmpl 那种的 js 原生语法作为模板语法吗？

> 可以。编辑配置文件，设置``"syntax": "native"``即可，这也是模板引擎 artTemplate 的默认语法，目前 TmodJS 默认使用的是 simple 语法。

**问**：如何兼容旧版本 atc 的项目？

> 编辑配置文件，分别设置``"type": "cmd"``、``"syntax": "native"``、``"output": "./"``

**问**：如何迁移原来写在页面中的 artTemplate 模板，改为 TmodJS 这种按按文件存放的方式？

> 请参考：[页面中的模板迁移指南](https://github.com/aui/tmodjs/wiki/页面中的模板迁移指南)。


##	更新日志

###	TmodJS v0.0.1

这是一个革命性的版本！同时项目更名为 **TmodJS**，内部版本号收归到 0.0.1，这是一个新的开始，未来将稳步更新。

使用 TemplageJS 格式的模块作为默认输出的类型：它包含模板目录中所有模板，除了支持页面 Script 直接引入之外还支持 RequireJS、SeaJS、NodeJS 加载，并且接口统一，跨架构与前后端运行！

详细更新列表：

*	吸收了来自业务的一些建议，编译方案的大调整，内部进行无数次优化，编译后的代码更小。
*	编译后的脚本使用统一的接口：``template(path, data)`` 其中 path 相对于 template.js 所在目录
*	自动打包目录与子目录的模板
*	可选支持异步载入模板功能
*	可选嵌入完整模板引擎（使用字符串存储模板）
*	可选支持 RequireJS/SeaJS/NodeJS 模块
*	保存模板配置文件（方便多人协作中使用版本管理工具共享配置）
*	可选编译调试版本
*	编译后的函数体优化
*	错误处理优化
*	``compile(file)``接口可递归编译依赖
*	增加``saveUserConfig``接口保存用户设置
*	默认语法变更：默认使用简洁语法，取消``--define-syntax``，并使用新的界定符 ``{{``与``}}``。[模板语法参考](https://github.com/aui/tmodjs/wiki/模板语法)
*	取消``--clone-helpers``参数

###	atc v1.0.3

*	默认使用简洁语法，取消--define-syntax，增加--no-define-syntax参数恢复原生语法
*	增加-t, --type设置输出的模块类型，默认 CMD ，可选：CMD | AMD | CommonJS。让模板可以前后端共用
*	优化无逻辑语句的模板编译后的函数体积
*	因 Windows 批处理无法模拟 NodeJS 的高级特性，atc 不再包含 Windows 批处理脚本，若需要可用批处理调用 NodeJS
*	模板语法的界定符有变更，请参考：模板语法

###	atc v1.0.2

NodeJS 版本：

*	增加``-o path``或``--output path``定义输出目录
*	修复``-d``或``--define-syntax``可能失效的问题
*	修改``-w``或``--watch``参数启动后的规则：只监控模板修改，而不再编译所有模板
*	增强调试特性：模板语法错误将在控制台输出调试源码，并停止进程

###	atc v1.0.1

NodeJS 版本：

*	支持监控目录，即时编译
*	使用命令行传递参数
*	使用 npm 管理包
*	支持设置简洁语法

###	atc v1.0.0

*	支持前端模板按文件与目录组织，自动处理 include 依赖
*	NodeJS 与 Windows 批处理版本同时发布

=========

如果你也认同 TmodJS 的理念、它能让你在开发中体会到书写模板的快乐，那么我希望你也能参与到 TmodJS 这个开源项目中来，无论是贡献代码、完善文档（目前文档还比较笼统）或者撰写博文推广它等。

特别感谢 [@TooBug](https://github.com/TooBug) 的建议，以及 NodeJS 代码迁移支持；还有 [@warmhug](https://github.com/warmhug) 同学在编译器雏形阶段的热心的测试与反馈，正是因为你们的支持 TmodJS 才能逐步健壮。