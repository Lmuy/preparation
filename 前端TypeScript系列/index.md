## `Partial<T>`

将 T 中所有的属性转换为可选属性。返回的类型可以是 T 的任意子集。

```js
export interface UserModel {
  name: string;
  age?: number;
  sex: number;
}

type JUserModel = Partial<UserModel>
// =
type JUserModel = {
    name?: string | undefined;
    age?: number | undefined;
    sex?: number | undefined;
}
// 源码解析
type Partial<T> = { [P in keyof T]?: T[P] }
```

## `Required<T>`

通过将 T 的所有属性设置为必选属性来构造一个新的类型。与 Partial 相反。

```js
type JUserModel2 = Required<UserModel>;
// =
type JUserModel2 = {
  name: string,
  age: number,
  sex: number,
};
```

## `Readonly<T>`

将 T 中所有属性设置为只读

```js
type JUserModel3 = Readonly<UserModel>

// =
type JUserModel3 = {
    readonly name: string;
    readonly age?: number | undefined;
    readonly sex: number;
}
```

## `Record<K, T>`

构造一个类型，该类型具有一组属性 K，每个属性的类型为 T。可用于将一个类型的属性映射为另一个类型。Record 后面的泛型就是对象键和值的类型。  
简单理解为：K 对应对象的 key，T 对应对象的 value，返回的就是一个声明好的对象。

```js
type TodoProperty = "title" | "description";

type Todo = Record<TodoProperty, string>;
// =
type Todo = {
  title: string,
  description: string,
};

interface IGirl {
  name: string;
  age: number;
}

type allGirls = Record<string, IGirl>;
```

## `Pick<T, K>`

在一个声明好的对象中，挑选一部分出来组成一个新的声明对象。

```js
interface Todo {
  title: string;
  description: string;
  done: boolean;
}

type TodoBase = Pick<Todo, "title" | "done">;

// =
type TodoBase = {
  title: string,
  done: boolean,
};
```

## `Omit<T, K>`

在 T 中取出除去 K 的其它所有属性。与 Pick 相对

## `Exclude<T, U>`

从 T 中排除可分配给 U 的属性，剩余的属性构成新的类型

```js
type T0 = Exclude<"a" | "b" | "c", "a">;

// =

type T0 = "b" | "c";
```

## `NonNullable<T>`

去除 T 中 null 和 undefined 类型

## `Parameters<T>`

返回类型为 T 的函数的参数类型所组成的数组

```js
type T0 = Parameters<() => string>; // []

type T1 = Parameters<(s: string) => void>; // [string]
```

## `ReturnType<T>`

function T 的返回类型

```js
type T0 = ReturnType<() => string>; // string

type T1 = ReturnType<(s: string) => void>; // void
```

## `InstanceType<T>`

返回构造函数类型 T 的实力类型，相当于 js 中的，不过返回的是对应的实例

```js
class C {
  x = 0;
  y = 0;
}

type T0 = InstanceType<typeof C>; // C
```
