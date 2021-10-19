1. ## 设计模式系列

- ### 设计模式有那些？
  单例模式、装饰器模式、发布-订阅模式、工厂模式、适配器模式、迭代器模式、策略模式、代理模式、职责链模式、中介者模式、组合模式、享元模式、模板方法模式、命令模式、状态模式。
- ### 发布-订阅模式
  发布-订阅模式其实是一种对象间一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到状态改变的通知。订阅者把自己想订阅的事件注册到调度中心，当发布者发布该事件到调度中心，也就是该事件触发时，由调度中心统一调度，订阅者注册到调度中心的处理代码。（js 事件系统就是一种发布-订阅模式）

```js
let eventEmitter = {
  // 缓存列表
  list: {},
  // 订阅
  on(event, fn) {
    let _this = this;
    // 如果对象中没有对应的event值，也就是说明没有订阅过，就给event创建个缓存列表
    // 如果对象中有相应的event值，把fn添加到对应的event缓存列表中
    (_this.list[event] || (_this.list[event] = [])).push(fn)
    return _this;
  }
  // 发布
  emit() {
    let _this = this;
    // 第一个参数是对应的event的值，直接用数组的shift方法取出
    let event = [].shift.call(arguments),
        fns = [..._this.list[event]]
    // 如果缓存列表没有fn就返回false
    if (!fns || fns.length === 0) {
      return false
    }
    // 遍历event值对应的缓存列表，依次执行fn
    fns.forEach(fn => {
      fn.apply(_this, arguments)
    })
    return _this
  }
}
function user1(content) {
  console.log('用户1订阅了', content)
}

// 订阅
eventEmitter.on('article1', user1)

// 发布
eventEmitter.emit('article1', 'Javascript 发布-订阅模式')
```
