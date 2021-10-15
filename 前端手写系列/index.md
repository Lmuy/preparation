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
