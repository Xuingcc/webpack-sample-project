# webpack-sample-project
# webpack-sample-project

webpack 打包练习demo
webpack起步
1、新建空文件夹 =》 
npm init =>
设置淘宝源 npm config set registry https://registry.npm.taobao.org/
查看源 npm config get registry
    最后npm install --save-dev webpack
2、设置项目目录如图


、使用webpack打包
# {extry file}出填写入口文件的路径，本文中就是上述main.js的路径，
# {destination for bundled file}处填写打包文件的存放路径可以不需要，则默认生成dist文件夹，在index.html中引用
# 填写路径的时候不用添加{}
使用命令
webpack {entry file} {destination for bundled file}
# webpack非全局安装的情况使用
node_modules/.bin/webpack app/main.js

4、升级，使用文件配置在项目根目录新建webpack.config.js
// 注：“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
有了这个配置之后，再打包文件，只需在终端里运行webpack(非全局安装需使用node_modules/.bin/webpack)

5、更便捷方式
在package.json中对scripts对象进行相关设置即可，设置方法如下。
"scripts": {
    "start": "webpack"
  },
注：package.json中的script会按照一定顺序寻找命令对应位置，本地的node_modules/.bin路径就在这个寻找清单中，所以无论是全局还是局部安装的Webpack，你都不需要写前面那指明详细的路径了。

6、webpack配置Source Maps
配置前

配置后能定位到哪个文件哪行

在webpack的配置文件中配置source maps，需要配置devtool，它有以下四种不同的配置选项，各具优缺点，描述如下：
（1）source-map 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度；
 （2）cheap-module-source-map 在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便； 
(3)eval-source-map 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项； 
(4)cheap-module-eval-source-map 这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点；

7、热更新======
配置devserver
inline设置为true，当源文件改变时会自动刷新页面

8、Loaders
Loaders需要单独安装并且需要在webpack.config.js中的modules关键字下进行配置，Loaders的配置包括以下几方面：
  ● test：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
  ● loader：loader的名称（必须）
  ● include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
  ● query：为loaders提供额外的设置选项（可选）

9、Babel
Babel其实是一个编译JavaScript的平台，它可以编译代码帮你达到以下目的：
（1）让你能使用最新的JavaScript代码（ES6，ES7...），而不用管新标准是否被当前使用的浏览器完全支持；
（2）让你能使用基于JavaScript进行了拓展的语言，比如React的JSX；
babel-core： babel核心的npm包
babel-preset-envbabel用于解析es6的包
babel-preset-reactbabel用于解析jsx的包

babel配置
Babel其实可以完全在 webpack.config.js 中进行配置，但是考虑到babel具有非常多的配置选项，在单一的webpack.config.js文件中进行配置往往使得这个文件显得太复杂，因此一些开发者支持把babel的配置选项放在一个单独的名为 ".babelrc" 的配置文件中。我们现在的babel的配置并不算复杂，不过之后我们会再加一些东西，因此现在我们就提取出相关部分，分两个配置文件进行配置（webpack会自动调用.babelrc里的babel配置选项）

10、一切皆模块
Webpack有一个不可不说的优点，它把所有的文件都都当做模块处理，JavaScript代码，CSS和fonts以及图片等等通过合适的loader都可以被处理。

CSS
webpack提供两个工具处理样式表，css-loader 和 style-loader，二者处理的任务不同，css-loader使你能够使用类似@import 和 url(...)的方法实现 require()的功能,style-loader将所有的计算后的样式加入页面中，二者组合在一起使你能够把样式表嵌入webpack打包后的JS文件中。

请注意这里对同一个文件引入多个loader的方法。

通常情况下，css会和js打包到同一个文件中，并不会打包为一个单独的css文件，不过通过合适的配置webpack也可以把css打包为单独的文件的。

CSS module

通过CSS模块，所有的类名，动画名默认都只作用于当前模块。Webpack对CSS模块化提供了非常好的支持，只需要在CSS loader中进行简单配置即可，然后就可以直接把CSS的类名传递到组件的代码中，这样做有效避免了全局污染

// css-loader3.0以上版本移除了localIndentName命令
需要配置成


CSS预处理器
Sass 和 Less 之类的预处理器是对原生CSS的拓展，CSS预处理器可以这些特殊类型的语句转化为浏览器可识别的CSS语句，
  ● Less Loader
  ● Sass Loader
  ● Stylus Loader
不过其实也存在一个CSS的处理平台-PostCSS，它可以帮助你的CSS实现更多的功能，在其官方文档可了解更多相关知识。
举例来说如何使用PostCSS，我们使用PostCSS来为CSS代码自动添加适应不同浏览器的CSS前缀。
首先安装postcss-loader 和 autoprefixer（自动添加前缀的插件）

npm install --save-dev postcss-loader autoprefixer

插件（Plugins）
使用插件的方法
我们需要通过npm安装它，然后要做的就是在webpack配置中的plugins关键字

 npm install banner-plugin

