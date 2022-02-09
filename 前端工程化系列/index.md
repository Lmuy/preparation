1. ## babel 的工作过程

- 解析：将代码转换为 AST
  - 词法分析：将代码分割为 token 流，即语法单元的数组
  - 语法分析：分析 token 流并生成 AST
- 转换：访问 AST 的节点进行变换操作生成新的 AST
  - Taro 就是利用 babel 完成的小程序语法转换
- 生成：以新的 AST 为基础生成代码

2. ## webpack 理解
   打包的原理：本质上是创建了导入和导出的环境，没有改变代码的执行逻辑，执行顺序和模块加载顺序也完全一致。  
   webpack 是一个模块化的打包机，分析项目结构，处理模块化依赖，将代码转换成浏览器可运行的代码：

- 代码转换：TypeScript 编译成 JS，SaSS/LESS 等编译为 css
- 文件优化：压缩 JS/CSS/HTML、压缩合并图片
- 代码分割：提取多个页面的公共代码，提取首屏不需要的执行代码让它异步加载
- 模块合并：在模块化项目里面会有很多个模块和文件，需要构建功能将多个模块合并成一个文件。
- 自动刷新：监听本地源代码的变化，自动重新构建、刷新浏览器。

3. ## webpack 系列优化

- https://juejin.cn/post/6844903862898262024#heading-32

4. ## webpack 打包过程

- 初始化参数：从配置文件和 shell 语句中读取并合并参数，得到最终参数。
- 开始编译：使用得到的参数初始化 Compiler 对象，然后加载所有配置的插件，接着调用对象上的 run 方法开始执行编译。
- 编译模块：从入口文件开始，调用所有配置的 loader 来翻译模块，接着该模块依赖的其它模块，递归调用本步骤，直到入口文件依赖的所有模块都运行了本步骤。
- 完成模块编译：经过上一步 Loader 翻译完所有模块之后，得到了模块翻译之后的内容以及模块之间的依赖。
- 输出资源：根据入口与其它模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，然后将 Chunk 转换为文件加载到输出列表，这不是改变输出内容的最后机会。
- 输出完后：确定好输出内容之后，根据配置确定输出的路径和文件名，将输出内容写入文件系统。

## webpack 动态加载原理

[参考 1](https://blog.csdn.net/qq_17175013/article/details/119350311)、[参考二](https://juejin.cn/post/6952703369135800350)

## webpack 热更新原理

大概流程是我们用 webpack-dev-server 启动一个服务之后，浏览器和服务端是通过 websocket 进行长连接，webpack 内部实现的 watch 就会监听文件修改，只要有修改就 webpack 会重新打包编译到内存中，然后 webpack-dev-server 依赖中间件 webpack-dev-middleware 和 webpack 之间进行交互，每次热更新都会请求一个携带 hash 值的 json 文件和一个 js，websocker 传递的也是 hash 值，内部机制通过 hash 值检查进行热更新， 至于内部原理，因为水平限制，目前还看不懂。

## webpack 文件路径问题（path.join(\_\_dirname, 'index.js')）

[参考一](https://blog.csdn.net/weixin_43972437/article/details/88378035)

## webpack 4 和 5 的差别

[参考一](https://www.imgeek.org/article/825357975)
