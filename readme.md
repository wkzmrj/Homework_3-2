1、Webpack 的构建流程主要有哪些环节？如果可以请尽可能详尽的描述 Webpack 打包的整个过程。

1. 初始化参数。获取用户在webpack.config.js文件配置的参数

2. 开始编译。初始化compiler对象，注册所有的插件plugins，插件开始监听webpack构建过程的生命周期事件，不同环节会有相应的处理，然后开始执行编译。

3. 确定入口。根据webpack.config.js文件的entry入口，开始解析文件构建ast语法树，找抽依赖，递归下去。

4. 编译模块。递归过程中，根据文件类型和loader配置，调用相应的loader对不同的文件做转换处理，在找出该模块依赖的模块，递归本操作，直到项目中依赖的所有模块都经过了本操作的编译处理。

5. 完成编译并输出。递归结束，得到每个文件结果，包含转换后的模块以及他们之前的依赖关系，根据entry以及output等配置生成代码块chunk

6. 打包完成。根据output输出所有的chunk到相应的文件目录


2、Loader 和 Plugin 有哪些不同？请描述一下开发 Loader 和 Plugin 的思路
1. 开发一个loader:
首先要了解loader是将非javascript文件转换为webpack能处理的有效模块，那么loader是一个输入资源转换输出的过程。

loader.js需要到导出一个函数，这个函数对加载的资源进行处理
函数输入为加载到的资源，输出为加工的资源
输入的结果有两种形式：第一就是输出为标准的JS代码，让打包结果可以正常执行，第二就是输出的结果交给下一个loader继续执行
开发好的loader.js配置到webpack.config.js下面的module.rules

2. 开发一个plugin:
首先要了解plugin是通过webpack的钩子机制(hooks)实现的，开发的插件可以通过这些不同的任务节点上挂载不同的任务。

创建一个javascript命名函数
在插件函数的prototype上定义一个apply方法
指定一个绑定到webpack自身的事件钩子
处理webpack内部实例的特点数据
功能完成以后调用webpack提供的回调

