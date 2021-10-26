## 1.手写 Promise

```js
class MyPromise {
  constructor(fn) {
    this.resolvedCallbacks = [];
    this.rejectedCallbacks = [];

    this.state = "PENDING";
    this.value = "";

    fn(this.resolve.bind(this), this.reject.bind(this));
  }

  resolve(value) {
    if (this.state === "PENDING") {
      this.state = "RESOLVED";
      this.value = value;

      this.resolvedCallbacks.map((cb) => cb(value));
    }
  }

  reject(value) {
    if (this.state === "PENDING") {
      this.state = "REHECTED";
      this.value = value;

      this.rejectedCallbacks.map((vb) => vb(value));
    }
  }

  then(onFulfilled, onRejected) {
    if (this.state === "PENDING") {
      this.resolvedCallbacks.push(onFilfilled);
      this.rejectedCallbacks.push(onRejected);
    }

    if (this.state === "RESOLVED") {
      onFulfilled(this.value);
    }

    if (this.state === "REJECTED") {
      onRejected(this.value);
    }
  }
}
```

## 2.手写数组扁平化

```js
function flatten(arr) {
  let result = [];

  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      result = result.concat(flatten(arr[i]));
    } else {
      result = result.concat(arr[i]);
    }
  }

  return result;
}
```

## 3.手写实现柯里化

**柯里化**：是指这样一个函数，它接受函数 A，并且能返回一个新的函数，这个新的函数能够处理函数 A 的剩余参数。

```js
function createCurry(func, args) {
  var argity = func.length;
  var args = args || [];

  return function () {
    var _args = [].slice.apply(arguments); // 将参数转化为数组
    args.push(..._args);

    if (args.length < argity) {
      return createCurry.call(this, func, args);
    }

    return func.apply(this, args);
  };
}
```

## 4.手写防抖和节流

**防抖**：高频事件触发时，只有等 n 秒内不再触发事件了，才执行函数，如果 n 秒内继续触发事件，那么就以新事件为准，等新的事件触发之后的 n 秒后才执行。

```js
function dobounce(func, delay) {
  var timeout;

  return function (e) {
    clearTimeout(timeout);

    var context = this;
    var args = arguments;

    timeout = setTimeout(function () {
      func.apply(context, args);
    }, delay);
  };
}
```

**节流**：高频事件触发时，不管触发多少次，每 n 秒执行一次。

```js
function throttle(func, delay) {
  var flag = true;

  return () => {
    if (!flag) return;
    flag = false;
    timer = setTimeout(() => {
      func();
      flag = true;
    }, delay);
  };
}
```

## 5.手写 new 操作符的实现

```js
function myNew(fn, ...args) {
  // 创建一个新对象（若该函数不是 JS 内置的，则创建一个新的 Object 对象）
  let obj = Object.create(fn.prototype);
  // 将 this 绑定到这个对象；执行构造函数中的代码（为这个新对象添加属性）
  let res = fn.call(obj, ...args);
  //若函数没有返回其他对象，则自动返回这个新对象；若函数有 return 返回的是非对象，则还是自动返回这个新对象，即覆盖那个非对象。
  if (res && (typeof res === "object" || typeof res === "function")) {
    return res;
  }
  return obj;
}
```

## 6.手写 call、apply、bind

```js
Function.prototype.myCall = function (context, ...args) {
  if (!context || context === null) {
    context = window;
  }
  // 创造唯一的key值，作为我们构造的context内部方法名
  let fn = Symbol();
  context[fn] = this; // this指向调用call的函数
  // 执行函数并返回结果，相当于把自身作为传入的context的方法进行调用了
  return context[fn](...args);
};

// apply原理一致，只是第二个参数是传入的数组
Function.prototype.myApply = function (context, args) {
  if (!context || context === null) {
    context = window;
  }
  // 创造唯一的key值，作为我们构造的context内部方法名
  let fn = Symbol();
  context[fn] = this;
  // 执行函数并返回结果
  return context[fn](...args);
};

// bind实现较为复杂
Function.prototype.myBind = function (context, ...args) {
  if (!context || context === null) {
    context = window;
  }
  // 创造唯一的key值，作为我们构造的context内部方法名
  let fn = Symbol();
  context[fn] = this;
  let _this = this;
  // bind情况较为复杂
  const result = function (...innerArgs) {
    if (this instanceof _this === true) {
      // 此时this指向指向result的实例  这时候不需要改变this指向
      this[fn] = _this;
      this[fn](...[...args, ...innerArgs]); //这里使用es6的方法让bind支持参数合并
    } else {
      // 如果只是作为普通函数调用  那就很简单了 直接改变this指向为传入的context
      context[fn](...[...args, ...innerArgs]);
    }
    // 如果绑定的是构造函数 那么需要继承构造函数原型属性和方法
    // 实现继承的方式: 使用Object.create
    result.prototype = Object.create(this.prototype);
    return result;
  };
};
```

## 7.手写深拷贝

```js
function deepClone(target) {
  // 定义一个变量
  let result;
  // 如果当前需要深拷贝的是一个对象的话
  if (typeof target === "object") {
    // 如果是一个数组的话
    if (Array.isArray(target)) {
      result = []; // 将result赋值为一个数组，并且执行遍历
      for (let i in target) {
        // 递归克隆数组中的每一项
        result.push(deepClone(target[i]));
      }
      // 判断如果当前的值是null的话；直接赋值为null
    } else if (target === null) {
      result = null;
      // 判断如果当前的值是一个RegExp对象的话，直接赋值
    } else if (target.constructor === RegExp) {
      result = target;
    } else {
      // 否则是普通对象，直接for in循环，递归赋值对象的所有值
      result = {};
      for (let i in target) {
        result[i] = deepClone(target[i]);
      }
    }
  } else {
    // 如果不是对象的话，就是基本数据类型，那么直接赋值
    result = target;
  }
  // 返回最终结果
  return result;
}
```

## 8.手写 instanceof 操作符实现

```js
function myInstanceof(left, right) {
  while (true) {
    if (left === null) {
      return false;
    }
    if (left.__proto__ === right.prototype) {
      return true;
    }
    left = left.__proto__;
  }
}
```

## 9.常用排序系列

```js
// 冒泡排序（时间复杂度n^2）
function bubbleSort(arr) {
  const len = arr.length;
  for (let i = 0; i < len; i++) {
    for (let j = 0; j < len - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  return arr;
}

// 选择排序（时间复杂度n^2）
function selectSort(arr) {
  const len = arr.length;
  let minIndex;
  for (let i = 0; i < len - 1; i++) {
    minIndex = i;
    for (let j = i; j < len; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    if (minIndex !== i) {
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
    }
  }
  return arr;
}

// 插入排序（时间复杂度n^2）
function insetSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    let j = i;
    let target = arr[j];
    while (j > 0 && arr[j - 1] > target) {
      arr[j] = arr[j - 1];
      j--;
    }
    arr[j] = target;
  }
  return arr;
}

// 快排（时间复杂度nlogn ~ n^2之间）
function quickSort(arr) {
  if (arr.length < 2) {
    return arr;
  }
  const cur = arr[arr.length - 1];
  const left = arr.filter((v, i) => v <= cur && i !== arr.length - 1);
  const right = arr.filter((v) => v > cur);
  return [...quickSort(left), cur, ...quickSort(right)];
}

// 二分查找（时间复杂度log2(n)）
function search(arr, target, start, end) {
  let targetIndex = -1;

  let mid = Math.floor((start + end) / 2);

  if (arr[mid] === target) {
    targetIndex = mid;
    return targetIndex;
  }

  if (start >= end) {
    return targetIndex;
  }

  if (arr[mid] < target) {
    return search(arr, target, mid + 1, end);
  } else {
    return search(arr, target, start, mid - 1);
  }
}
```

## 10.类数组转数组

```js
// 扩展运算符
[...arrayLike];
// Array.from
Array.from(arrayLike);
// Arry.prototype.slice
Array.prototype.slice.call(arrayLike);
// array.apply
Array.apply(null, arrayLike);
// Array.prototype.concat
Array.prototype.concat.aaply([], arrayLike);
```

## 11.虚拟 DOM 转为真实 DOM

```js
function _render(vnode) {
  // 如果是数字类型转化为字符串
  if (typeof vnode === "number") {
    vnode = String(vnode);
  }
  // 字符串类型直接就是文本节点
  if (typeof vnode === "string") {
    return document.createTextNode(vnode);
  }
  // 普通DOM
  const dom = document.createElement(vnode.tag);
  if (vnode.attrs) {
    // 遍历属性
    Object.keys(vnode.attrs).forEach((key) => {
      const value = vnode.attrs[key];
      dom.setAttribute(key, value);
    });
  }
  // 子数组进行递归操作
  vnode.children.forEach((child) => dom.appendChild(_render(child)));
  return dom;
}
```
