- ## Flex 布局系列

1. **flex 属性**：flex 是 flex-grow，flex-shrink 和 flex-basis 的缩写。  
   `flex-grow`用于设置各 item 项按对应比例划分剩余空间，若 flex 中没有指定 flex-grow，则相当于设置了 flex-grow：1  
   `flex-shrink`用于设置 item 的总和超出容器空间时，各 item 的收缩比例，若 flex 中没有指定 flex-shrink，则等同于设置了 flex-shrink：1
   `flex-basis`用于设置各 item 项的伸缩基准值，可以取值为长度或百分比，如果 flex 中省略了该属性，则相当于设置了 flex-basis:0  
   当 flex 为 none 时,等同于 flex: 0 0 auto。  
   当 flex 为 auto 时，等同于 flex: 1 1 auto。  
   当 flex 取值为一个数字，则该数字是设置的 flex-grow 值，其它两个属性用初始值，如 flex:1 时，等同于 flex: 1 1 0%。  
   当 flex 取值为 2 个数字时，相当于设置的 flex-grow 和 flex-shrink 值，flex-basis 取值为初始值，如 flex:1 0 时，等同于 flex: 1 0 0%。  
   当 flex 取值为 1 个数字和 1 个长度或百分比时，设置的是 flex-grow 和 flex-basis 的值，flex-shrink 值是初始值，如 flex:1 20%,等同于 flex: 1 1 20%。

- ## 布局系列

1. **css div 垂直水平居中，并完成 div 高度永远是宽度的一半**

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      html,
      body {
        width: 100%;
        height: 100%;
      }
      .outer {
        width: 400px;
        height: 100%;
        background: blue;
        margin: 0 auto;
        display: flex;
        align-items: center;
      }
      .inner {
        position: relative;
        width: 100%;
        height: 0;
        padding-bottom: 50%;
        background: red;
      }
      .box {
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner">
        <div class="box">hello</div>
      </div>
    </div>
  </body>
</html>
```

2. **CSS 怎么画一个大小为父元素宽度一半的正方形？**

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        width: 400px;
        height: 600px;
        background: red;
      }
      .inner {
        width: 50%;
        padding-bottom: 50%;
        background: blue;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner"></div>
    </div>
  </body>
</html>
```

3. **display:flow-root 可以设置为 BFC**

- ## CSS 动画系列

1. `animation`：用于设置动画属性，它是一个简写属性，包含 6 个属性。

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <title>animation</title>
  <style>
    .box {
      height: 100px;
      width: 100px;
      border: 15px solid black;
      animation: changebox 1s ease-in-out 1s infinite alternate running forwards;
    }
    .box:hover {
      animation-play-state: paused;
    }
    @keyframes changebox {
      10% {
        background: red;
      }
      50% {
        width: 80px;
      }
      70% {
        border: 15px solid yellow;
      }
      100% {
        width: 180px;
        height: 180px;
      }
    }
  </style>
</head>
<body>
  <div class="box"></div>
</body>
</html>
```

2. `transition`：用于设置元素的样式过度，和 animation 类似，但细节有很大不同。

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <title>transition</title>
  <style>
    #box {
      height: 100px;
      width: 100px;
      background: green;
      transition: transform 1s ease-in 1s;
    }
    #box:hover {
      transform: rotate(180deg) scale(.5, .5);
    }
  </style>
</head>
<body>
  <div id="box"></div>
</body>
</html>
```

3. `transform`：用于元素进行旋转、缩放、移动或倾斜，和设置样式的动画没什么关系。
4. `translate`：只是 transform 的一个属性值，即移动。

- ## 盒模型

1. **标准模型**：宽度=内容的宽度（content）+ border + padding，对应`box-sizing`: `content-box`;
2. **IE 盒子模型**：宽度=内容宽度（content+border+padding），对应`box-sizing`: `border-box`;
