1. ## React16 生命周期

### 挂载阶段：constructor -> getDerivedStateFromProps -> render -> componentDidMount

- **constructor**：组件构造函数，第一个被执行。如果没有显示定义它，我们会拥有一个默认的构造函数。如果显示定义了构造函数，我们必须在构造函数第一行执行`super(props)`，否则我们无法在构造函数里拿到 this 对象。

```js
constructor(props) {
    super(props)

    this.state = {
      select,
      height: 'atuo',
      externalClass,
      externalClassText
    }

    this.handleChange1 = this.handleChange1.bind(this)
    this.handleChange2 = this.handleChange2.bind(this)
}
```

在构造函数里面我们一般会做两件事：1.初始化 state 对象；2.给自定义方法绑定 this

- **getDerivedStateFromProps**：一个静态方法，所以不能在这个函数里面使用 this，这个函数有两个参数 props 和 state，分别指接收到的新参数和当前的 state 对象，这个函数会返回一个对象用来更新当前这个 state 对象，如果不需要更新可以返回 null。  
  该函数会在挂载时和接收到新的 props 时被调用。
- **render**：render 函数是纯函数，里面只做了一件事，就是返回需要渲染的东西，不应该包含其它的业务逻辑。
- **componentDidMount**：组件装载之后调用，此时我们可以获取到 DOM 节点并操作。在 componentDidMount 中调用 setState 会触发一次额外的渲染，多调用一次 render 函数，但是用户对此没有感知，因为他是在浏览器刷新屏幕前执行的，但是我们应该在开发中避免它，因为他会带俩一定的性能问题，我们应该在 constructor 中初始化我们的 state 对象，而不应该在 componentDidMount 调用 state 方法。

### 更新阶段：getDerivedStateFromProps -> shouldComponentUpdate -> render -> getSnapshotBeforeUpdate -> componetDidUpdate

- **getDerivedStateFromProps**：这个方法在装载阶段已经讲过了，这里不再赘述，记住在更新阶段，无论我们接收到新的属性，调用了 setState 还是调用了 forceUpdate，这个方法都会被调用。
- **shouldComponetUpdate**：有两个参数`nextProps`和`nextState`，表示新的属性和变化之后的 state，返回一个布尔值，true 表示会触发重新渲染，false 表示不会触发重新渲染，默认返回 true。（当我们调用 forceUpdate 并不会触发此方法）  
  因为默认返回 true，也就是只要接收到新的属性和调用了 setState 都会触发重新渲染，这会带来一定的性能问题，所以我们需要将 this.props 与 nextProps，以及 this.state 与 nextState 进行比较来决定是否返回 false，来减少重新渲染。
- **render**：同上。
- **getSnapshotBeforeUpdate**：这个方法在 render 之后 componentDidUpdate 之前调用，有两个参数 prevProps 和 prevState，表示之前的属性和之前的 state，这个函数有一个返回值，会作为第三个参数传给 componentDidUpdate，如果你不想要返回值请返回 null，不写的话控制台会有警告。还有这个方法一定要和 componentDidUpdate 一起使用，否则控制台也会有警告。典型场景：获取 render 之前的 dom 状态。
- **componentDidUpdate**：该方法在 getSnapshotBeforUpdate 方法之后被调用，有三个参数 prevPops，prevState，snapshot，表示之前的 props，之前的 state 和 snapshot。第三个参数是 getSnapshotBeforeUpdate 返回的。
  在这个函数里我们可以操作 dom，和发起服务器请求，还可以 setState，但是注意一定要用 if 语句控制，否则会导致无限循环。

### 卸载阶段：componentWillUnmount

- **componentWillUnmount**：当我们的组件被卸载或者销毁了就会调用，我们可以在这个函数里去清除一些定时器，取消网络请求，清理无效的 DOM 元素等垃圾清理工作。

2. ## React 虚拟 dom 和 diff 算法

- **虚拟 dom**：直接使用 dom 进行操作时，会触发重排与重绘，直接创建 dom 也会消耗很多时间。虚拟 dom 就是在首次渲染 dom 时，多了一层虚拟 dom 的计算，在 dom 的基础上建立了一个抽象层。当有改动时，会生成一个新的虚拟 dom 与上一次生成的虚拟 dom 去 diff，等到一个差异化部分的 patch，然后将 patch 打到浏览器的 dom 上去。
- **diff 算法**：两个树完全的 diff 算法时间复杂度时 O(n^3)。react 应用了三种策略，使得时间复杂度从 O(n^3)降到了 O(n)。  
  2.1 **tree diff 层级**：逐层比较，假设跨层级操作节点的概率不大，那么如果遇到在比较旧的虚拟 dom 的时候，如果此节点在旧的里面有，在新的里面没有，那么就删除这个节点，而不会去跨层级找到这个节点。  
  2.2 **component diff 组件级**：两个相同类型的组件具有相同的 dom 结构，两个不同类型的组件具有不同的 dom 结果，如果组件不同，那么直接进行删除和插入操作。  
  2.3 **element diff 元素级**：对于同一父节点下的一组节点，通过 key 值来标志，如果 key 不一样那么进行删除和插入操作，如果 key 一样，那么进行移动操作。

3. ## React 中 setState 是否是异步？
   setState 看似异步的，但是实际上不是真正意义上的异步，只是模拟了异步的行为。  
   3.1 在原生 dom 事件和 setTimeout 等原生里面，setState 是同步更新的。  
   3.2 在合成事件或者钩子函数里面会遵循一定的策略，不会马上更新，因为合成事件和钩子函数在更新之前就运行了。

- React 会去维护一个 isBatchingUpdates 标志，如果为 false 直接更新，如果是 true，那么会暂存状态进队列。
- 且 setState 本身不是异步的，本身执行过程和代码是同步的，不会马上更新，因为合成事件和钩子函数在更新之前就运行了，导致没法马上拿到更新的值，所以形成了所谓的‘异步’，当然 setState 的第二个参数可以拿到更新之后的结果，或者接收函数调用，在函数调用里面可以拿到更新之后的结果。
- 而且 React 的批量更新也有一定的规则，如果在合成事件或者钩子函数中，多次 setState 同一个属性会用最新的覆盖，如果多次 setState 不同的属性，会进行合并批量更新。

## React Class 组件有哪些周期函数？分别有什么作用？

## React Class 组件中请求可以在 componentWillMount 中发起吗？为什么？

## React Class 组件和 React Hook 的区别有哪些？

## React 中高阶函数和自定义 Hook 的优缺点？

## 简要说明 React Hook 中 useState 和 useEffect 的运行原理？

## React 如何发现重渲染、什么原因容易造成重渲染、如何避免重渲染？

## React Hook 中 useEffect 有哪些参数，如何检测数组依赖项的变化？

## React 的 useEffect 是如何监听数组依赖项的变化的？

## React Hook 和闭包有什么关联关系？

## React 中 useState 是如何做数据初始化的？

## 列举你常用的 React 性能优化技巧？
