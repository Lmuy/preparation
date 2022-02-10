- ## js 脚本加载 async、defer 问题

1. 如果依赖其它脚本和 DOM 结果，使用 defer
2. 如果与 DOM 和其它脚本基本依赖不强时，使用 async

- ## 原型链相关

1. **js 原型和实例的关系**：每个构造函数(constructor)都有一个原型对象(prototype)，这个原型对象包含了一个指向此构造函数的指针属性，通过 new 进行构造函数调用生成的实例，此实例包含一个指向原型对象的指针们也就是通过`Prototype`链接到了这个原型对象。
2. **js 中属性的查找**：当我们试图引用实例对象的某个属性时，首先查找实例对象上是否有这个属性，如果没有找到，就去构造函数这个实例对象的构造函数的`prototype`所指向的对象上去查找，如果还是找不到，就从这个`prototype`对象所指向的构造函数的`prototype`原型对象上去查找。
3. **原型链**：当对象查找一个属性的时候，如果没有在自身找到，那么就会查找自身的原型，如果原型还没找到，那么就会继续查找原型的原型，直到找到`Object.prototype`的原型时，此时原型为`null`，查找停止。这样逐级查找形似一个链条，且通过`Prototype`属性链接，所以被称为原型链。
4. **原型链继承**：一个对象可以使用另外一个对象的属性或者方法，就称之为继承。具体是通过将这个对象的原型设置为另外一个对象，这样根据原型链的规则，如果查找一个对象属性且在自身不存在时，就会查找另外一个对象，相当于一个对象可以使用另外一个对象的属性和方法了。

- ## 数组能够调用的函数

1. **push**：在数组末尾添加一个或多个元素，并返回新数组长度。
2. **join**：将数组中的而所有元素都转化为字符串并连接在一起。
3. **reverse**： 将数组中的元素颠倒顺序。
4. **concat**： 数组拼接，返回新数组，原数组不受影响。
5. **slice**：截取数组生成新数组，原数组不受影响。
6. **splice**：从数组中删除、插入或者同时完成这两种操作，修改原数组。
7. **pop**： 从数组末尾删除一个元素，并返回被删除的元素，修改原数组。
8. **shift**： 在数组开始添加一给或多个元素，并返回新数组长度。
9. **unshift**： 在数组开始删除一个元素，并返回被删除的元素。
10. **toString()和 toLocaleString()**：将数组的每个元素转化为字符串，并且输入用逗号分隔的字符串列表。
11. **indexOf()和 lastIndexOf()**：从数组的开头和末尾开始查找，返回查找项索引值，没找到返回-1。
12. **map()**：调用数组的每一个元素传递给指定函数，并返回一个新数组，不修改原数组。

```js
let arr = [2, 3, 4, 5];
let b = arr.map((x) => {
  return x * x;
});
console.log(b); // [4,6,9,16,25]
```

13. **filter()**：过滤功能，数组中每一项运行给定函数，返回满足过滤条件组成的数组。

```js
let a = [1, 2, 3, 4, 5];
a.filter((item) => {
  return item > 2;
}); // [3,4,5]
```

14. **every()和 some()**：every() 判断数组中每一项都是否满足条件，只有所有项都满足条件，才会返回 true。some() 判断数组中是否存在满足条件的项，只要有一项满足条件，就会返回 true。
15. **reduce()和 reduceRight()**：reduce() 两个参数：函数和递归的初始值。从数组的第一项开始，逐个遍历到最后。reduceRight() 从数组的最后一项开始，向前遍历到第一项

```js
//可以用reduce快速求数组之和
var arr = [1, 2, 3, 4];
arr.reduce(function (a, b) {
  return a + b;
}); //10
```

- ## 箭头函数和普通函数系列

1. 普通函数通过`function`关键字定义，this 无法结合词法作用域使用，在运行时绑定，只取决于函数的调用方式，在哪里被调用，调用位置。
2. 箭头函数不应用普通函数 this 绑定的四种规则，而是根据外层的作用域来决定 this，且箭头函数的绑定无法被修改。
3. this 的绑定规则：new > 显示绑定 > 隐式绑定 > 默认绑定  
   **默认绑定**：当独立函数调用时，不管是否在调用栈中，this 都指向全局对象。

```js
var a = 2;
function foo() {
  console.log(this.a);
}
function bar() {
  var a = 5;
  foo();
}
bar(); // 2
```

**隐式绑定**：当函数引用有上下文对象时，隐式绑定规则会把函数调用中的 this 绑定到这个上下文对象。

```js
function foo() {
  console.log(this.a);
}
var obj2 = {
  a: 42,
  foo: foo,
};
var obj1 = {
  a: 2,
  obj2: obj2,
};
obj1.obj2.foo(); // 42
```

**显示绑定**：采用 call()和 apply()，通过传入一个对象（若为基本类型，会被封装函数转为对象—装箱），将 this 绑定到该对象。

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
};
var bar = function () {
  foo.call(obj);
};
bar(); // 2
setTimeout(bar, 100); // 2
// 硬绑定后bar无论怎么调用，都不会影响foo函数的this绑定
bar.call(window); // 2
```

**new 绑定**：任何函数都可能被用作构造函数，当函数被 new 操作符“构造调用”时，会执行下面操作：

1. 创建一个新对象（若该函数不是 JS 内置的，则创建一个新的 Object 对象）；
2. 将 this 绑定到这个对象；
3. 执行构造函数中的代码（为这个新对象添加属性）；
4. 若函数没有返回其他对象，则自动返回这个新对象；若函数有 return 返回的是非对象，则还是自动返回这个新对象，即覆盖那个非对象。

## 装饰器 decorator

### 1.类的装饰

许多面向对象的语言都有装饰器(Decorator)函数，用来修改类的行为。

```js
@testable
class MyClass {}

function testable(target) {
  target.isTestable = true;
}

MyClass.isTestable; // true
```

### 2.基本语法：

```js
@decorator
class A {}

// 等同于
class A {}
A = decorator(A) || A;
```

修饰器是一个对类进行处理的函数。注意，修饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。这意味着，修饰器能在编译阶段运行代码。也就是说，修饰器本质就是编译时执行的函数。  
**React 与 Redux 库结合使用时，常常需要写成下面这样**

```js
class MyConponent extends React.Conponent {}
export default connect(mapStateToProps, mapDispatchToProps)(Mycomponent)

// 可以通过装饰器，写成下面这样
@connect(mapStateToProps, mapDispatchToPtops)
export default class MyComponent extends React.Component {}
```

### 3.修饰类的方法

修饰器不仅可以修饰类，还可以修饰类的属性。

```js
class Person {
  @readonly
  name() {
    return `${this.name}`;
  }
}
```

## jsonp 原理及实现方法

JSONP 是 JSON with Padding 的略称。它是一个非官方的协议，它允许在服务器端集成 Script tags 返回至客户端，通过 JavaScript callback 的形式实现跨域访问。

```js
// 实现方式
// 例如：当前前端环境：localhost:8080     后端环境：localHost:8081
// 前端调用后端地址
<script type="text/javascript" src="http://localhost:8081/test.js"></script>
```

以上，可以成功访问到非同源的 server 地址。script 标签的 src 属性并不被同源策略所约束，所以可以获取任何服务器上的脚本并运行。

### 实现模式--CallBack

程序 A 中部分代码

```js
<script type="text/javascript">
// 回调函数
function callback(data) {
    alert(data.message);
}
</script>
<script type="text/javascript" src="http://localhost:20002/test.js"></script>
```

程序 B 中部分代码

```js
//调用callback函数，并以json数据形式作为阐述传递，完成回调

callback({ message: "success" });
```

这其实就是 JSONP 的实现模式，或者说 JSONP 的原型：**创建一个回调函数，然后再远程服务上调用这个函数并且将 JSON 数据形式作为参数传递，完成回调。将 JSON 数据填充进回调函数**

## promise 实例理解

```js
let p1 = new Promise((resolve, reject) => {
  resolve("数据");
});
p1.then((res) => {
  console.log(111, res);
})
  .then((res) => {
    console.log(222, res);
  })
  .catch((err) => {
    console.log(333, err);
  });
// 111 '数据'
// 222 undefined
let p1 = new Promise((resolve, reject) => {
  reject("错误数据");
});
p1.then((res) => {
  console.log(111, res);
})
  .then((res) => {
    console.log(222, res);
  })
  .catch((err) => {
    console.log(333, err);
  });
// 333 '错误数据'
let p1 = new Promise((resolve, reject) => {
  reject("错误数据");
});
p1.then((res) => {
  console.log(111, res);
})
  .then((res) => {
    console.log(222, res);
  })
  .catch((err) => {
    console.log(333, err);
  })
  .then((res) => {
    console.log(444, res);
  });
// '错误数据'
// 444 undefined
let p1 = new Promise((resolve, reject) => {
  reject("错误数据");
});
p1.then((res) => {
  console.log(111, res);
})
  .then((res) => {
    console.log(222, res);
  })
  .catch((err) => {
    console.log(333, err);
  })
  .then((res) => {
    console.log(444, res);
  })
  .catch((err) => {
    console.log(555, err);
  });
// 333 '错误数据'
// 444 undefined
```
