## computed 和 watch 原理

## nextTick 原理

## 响应式原理(双向绑定原理)

[参考这里](https://www.cnblogs.com/canfoo/p/6891868.html)

## 总结 vue 和 react 的区别

react 和 vue 都是提倡组件化，整体的功能都类似，主要由于设计思路的区别，导致出现一些不同。

### 1.数据是否可变

`react`是单向数据流，推崇数据不可变。  
`vue`的思想是响应式，数据是可变的，通过对每个属性建立 watcher 监听，当属性变化时，触发响应式进而更新 dom。

### 2.开发方式

`react`是 all in js，使用 jsx 开发。  
`vue`使用模板语法。

### 3.手动优化/自动优化

### 4.框架功能（框架+社区）

## vue 重写数组操作方法实现响应式

[参考这里](https://blog.csdn.net/wytraining/article/details/114321846?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1.pc_relevant_default&utm_relevant_index=1)
