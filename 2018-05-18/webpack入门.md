#webpack是什么
webpack是一个现代javascript应用程序的静态模块打包器（module bundler）。当webpack处理应用程序时，它会递归的构建一个关系依赖图，其中包含应用程序需要的每个模块，比如javascript，CSS,sass,img/png等，然后将这些模块进行组合和打包成一个或多个bundle，使其变成能够被浏览器使用的静态资源。
![wbepack](https://user-gold-cdn.xitu.io/2018/5/17/1636d95b281d7baa?imageslim)

# 核心概念
## entry入口
entry是配置模块的入口，它指示webpack执行构建的第一步。     
可以在webpack.config.js配置文件中配置entry属性，可以指定一个或多个入口点，如果配置多个入口点，可以将entry定义成一个对象             
entry类型有三种：字符串，数组，对象。 
- String ："./src/entry" 入口模块的文件路径，可以是相对路径
- array : ["./src/entry1", "./src/entry2"] 入口模块的文件路径，可以是相对路径。与字符串类型不同的是数组可将多个文件打包为一个文件
- object ： { a: './src/entry-a', b: ['./src/entry-b1', './app/entry-b2']} 配置多个入口，每个入口有一个 Chunk
## Output 输出结果
output 配置如何输出最终想要的代码。output 是一个 object，里面包含一系列配置项。

filename 用于输出文件的文件名。
path 目标输出目录的绝对路径，必须是绝对路径。
## Module 模块和Loader 模块转换器
module 配置如何处理模块

Loader 可以看作具有文件转换功能的翻译员，配置里的 module.rules 数组配置了一组规则，告诉 Webpack 在遇到哪些文件时使用哪些 Loader 去加载和转换。

use 属性的值需要是一个由 Loader 名称组成的数组，Loader 的执行顺序是由后到前的；
每一个 Loader 都可以通过 URL querystring 的方式传入参数，例如 css-loader?minimize 中的 minimize 告诉 css-loader 要开启 CSS 压缩。

作者：AdityaSui
链接：https://juejin.im/post/5afd556ef265da0ba76ff5c6
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
## Chunk 代码块
Chunk，代码块，即打包后输出的文件。
Webpack 会为每个 Chunk 取一个名称，可以根据 Chunk 的名称来区分输出的文件名。

    filename: '[name].js'

一个入口文件，默认 chunkname 为 main。
除了内置变量 name，与 chunk 相关的变量还有：

- `id` 	Chunk 的唯一标识，从0开始
- `name` Chunk 的名称
- `hash` Chunk 的唯一标识的 Hash 值
- `chunkhash` Chunk 内容的 Hash 值

## plugin 插件

Plugin 用于扩展 Webpack 功能，各种各样的 Plugin 几乎让 Webpack 可以做任何构建相关的事情。

Plugin 的配置很简单，plugins 配置项接受一个数组，数组里每一项都是一个要使用的 Plugin 的实例，Plugin 需要的参数通过构造函数传入。
配置插件例子：
```js
plugins: [
        new webpack.BannerPlugin('版权所有，翻版必究'),
        new HtmlWebpackPlugin({
            template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
        }),
        new webpack.HotModuleReplacementPlugin()//热加载插件
    ],
```
>Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

# Webpack的强大功能

生成Source Maps（使调试更容易）

开发总是离不开调试，方便的调试能极大的提高开发效率，不过有时候通过打包后的文件，你是不容易找到出错了的地方，对应的你写的代码的位置的，Source Maps就是来帮我们解决这个问题的。

通过简单的配置，webpack就可以在打包时为我们生成的source maps，这为我们提供了一种对应编译文件和源文件的方法，使得编译后的代码可读性更高，也更容易调试。

在webpack的配置文件中配置source maps，需要配置devtool，

它有以下四种不同的配置选项


## 面试问题
1. gulp/grunt 与 webpack的区别是什么?
Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack在很多场景下可以替代Gulp/Grunt类的工具。

Grunt和Gulp的工作方式是：在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，工具之后可以自动替你完成这些任务。

Webpack的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。

2. webpack是解决什么问题而生的?
现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，前端社区涌现出了很多好的实践方法

模块化，让我们可以把复杂的程序细化为小的文件;
类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能转换为JavaScript文件使浏览器可以识别；
Scss，less等CSS预处理器
...
这些改进确实大大的提高了我们的开发效率，但是利用它们开发的文件往往需要进行额外的处理才能让浏览器识别,而手动处理又是非常繁琐的，这就为WebPack类的工具的出现提供了需求。

3. 如何配置多入口文件?
entry: {
    bundle1: './main1.js',
    bundle2: './main2.js'
    ...
  },
4. 你是如何提高webpack构件速度的?

5. 利用webpack如何优化前端性能? 