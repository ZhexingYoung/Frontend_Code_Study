# Web APIs 第一天知识点总结

> 学习目标：掌握 DOM 对象的获取、内容修改、属性操作、样式控制、表单属性、自定义属性和定时器，并能用这些语法完成简单页面交互案例。

## 目录

- [1. const 常量声明](#1-const-常量声明)
- [2. Web API 与 DOM 基础](#2-web-api-与-dom-基础)
- [3. 操作 DOM 对象内容](#3-操作-dom-对象内容)
- [4. 操作 DOM 常用属性](#4-操作-dom-常用属性)
- [5. 操作 DOM 样式属性](#5-操作-dom-样式属性)
- [6. 操作表单元素属性](#6-操作表单元素属性)
- [7. 自定义属性](#7-自定义属性)
- [8. 定时器：间歇函数](#8-定时器间歇函数)
- [9. 案例模块](#9-案例模块)

## 1. const 常量声明

### 核心概念

`const` 用来声明常量，声明后不能重新赋值，也不能重复声明。它适合保存不会被整体替换的数据。

```js
const PI = 3.14
```

### 注意点

- `const` 声明基本数据类型时，值不能被修改。
- `const` 声明对象或数组时，不能把变量整体重新赋值，但可以修改对象属性或数组内容。

```js
const obj = { name: '张三', age: 18 }
obj.name = '李四'

const arr = [1, 2, 3]
arr.push(4)
```

### 易错点

对象和数组使用 `const` 时，不是“里面的内容完全不能变”，而是“变量保存的地址不能变”。

## 2. Web API 与 DOM 基础

### Web API 是什么

API 是 `Application Programming Interface` 的缩写，意思是应用程序编程接口。浏览器提前提供了很多功能，JavaScript 可以通过这些接口操作网页和浏览器。

### Web API 的组成

- `DOM`：Document Object Model，文档对象模型，用来操作页面元素。
- `BOM`：Browser Object Model，浏览器对象模型，用来控制浏览器行为。

### DOM 相关概念

- `document`：整个 HTML 文档的根对象。
- `DOM 树`：浏览器会把 HTML 结构解析成树形结构。
- `DOM 对象`：页面中的标签被浏览器解析后形成的对象，JS 可以通过对象操作标签。

## 3. 获取 DOM 对象

### 需求

想要修改页面内容或样式，第一步通常是先获取页面中的元素对象。

### 常用语法

#### 获取匹配的第一个元素

```js
document.querySelector('选择器')
```

示例：

```js
document.querySelector('#id')
document.querySelector('.class')
document.querySelector('div')
document.querySelector('[data-id="1"]')
document.querySelector('.nav li')
```

如果没有匹配到元素，返回 `null`。

#### 获取匹配的所有元素

```js
document.querySelectorAll('选择器')
```

返回的是 `NodeList`，它是伪数组：

- 有长度 `length`
- 有索引号
- 可以用 `for` 循环遍历
- 不能直接使用数组的 `push`、`pop` 等方法

```js
const lis = document.querySelectorAll('li')

for (let i = 0; i < lis.length; i++) {
  console.log(lis[i])
}
```

### 了解即可的旧方法

```js
document.getElementById('id')
document.getElementsByClassName('class')
document.getElementsByTagName('tag')
```

## 4. 操作 DOM 对象内容

### 需求

获取元素后，经常需要修改标签中的文字或 HTML 结构。

### 核心语法

#### innerText

`innerText` 用来获取或设置元素的纯文本内容，不会解析 HTML 标签。

```js
const box = document.querySelector('.box')
box.innerText = '我是新的文字内容'
```

如果写入 `<span>文字</span>`，页面会把它当普通文本显示。

#### innerHTML

`innerHTML` 用来获取或设置元素的 HTML 内容，会解析标签。

```js
const box = document.querySelector('.box')
box.innerHTML = '<span>我是新的文字内容</span>'
```

### 使用建议

- 只改文字，用 `innerText`。
- 需要插入标签结构，用 `innerHTML`。
- 多标签拼接时，可以配合模板字符串。

## 5. 操作 DOM 常用属性

### 需求

通过 JS 修改标签自带属性，例如图片地址、链接地址、标题等。

### 核心语法

```js
对象.属性 = 值
```

常见属性：

- `src`
- `href`
- `title`
- `alt`

示例：随机切换图片。

```js
const img = document.querySelector('img')
img.src = `./assets/0${random}.jpg`
```

### 注意点

属性名通常和 HTML 标签上的属性名一致。例如 HTML 中是 `src`，JS 中也用 `img.src`。

## 6. 操作 DOM 样式属性

### 需求

通过 JS 控制元素样式，让页面产生动态变化。

### 方式一：通过 style 修改行内样式

```js
元素.style.样式属性 = '样式值'
```

示例：

```js
const box = document.querySelector('.box')
box.style.backgroundColor = 'red'
box.style.border = '1px solid black'
```

注意：

- CSS 中多单词属性，在 JS 中要写成小驼峰命名。
- `background-color` 写成 `backgroundColor`。
- 通过 `style` 设置的是行内样式，权重比较高。
- 适合修改少量样式。

### 方式二：通过 className 修改类名

```js
元素.className = '类名'
```

示例：

```js
const box2 = document.querySelector('.box2')
box2.className = 'boxchange'
```

注意：`className` 会覆盖原来的全部类名。如果想保留原来的类名，需要手动拼接。

```js
box2.className = 'box2 boxchange'
```

### 方式三：通过 classList 操作类名

```js
元素.classList.add('类名')
元素.classList.remove('类名')
元素.classList.toggle('类名')
元素.classList.contains('类名')
```

含义：

- `add`：添加类名
- `remove`：删除类名
- `toggle`：有就删除，没有就添加
- `contains`：判断是否包含某个类名

优点：不会覆盖原有类名，更适合控制多个类名。

## 7. 操作表单元素属性

### 需求

获取和修改表单元素的值、类型、选中状态、禁用状态。

### 常用属性

```js
input.value
input.type
input.checked
button.disabled
select.selected
```

示例：

```js
const ipt = document.querySelector('input')
ipt.checked = false

const button = document.querySelector('button')
button.disabled = false
```

### 注意点

`checked`、`disabled`、`selected` 这类属性通常使用布尔值：

```js
ipt.checked = false
button.disabled = true
```

不要写成字符串：

```js
ipt.checked = 'false'
```

字符串 `'false'` 也可能被当成真值处理，容易产生误解。

## 8. 自定义属性

### 需求

当标准属性不够用时，可以给标签添加自定义数据，例如商品 id、用户 id、索引值等。

### HTML5 推荐写法

在标签上一律以 `data-` 开头：

```html
<div data-id="1" data-num="8">1</div>
```

### JS 获取方式

通过 DOM 对象的 `dataset` 获取：

```js
const one = document.querySelector('div')
console.log(one.dataset)
console.log(one.dataset.id)
```

### 注意点

- HTML 中写 `data-id`，JS 中通过 `dataset.id` 获取。
- `dataset` 得到的是一个对象，里面保存所有 `data-` 开头的自定义属性。

## 9. 定时器：间歇函数

### 需求

让一段代码每隔一段时间自动执行一次，例如倒计时、轮播图自动切换。

### 开启定时器

```js
setInterval(函数, 间隔时间)
```

示例：

```js
setInterval(function () {
  console.log('一秒执行一次')
}, 1000)
```

也可以传入函数名：

```js
function fn() {
  console.log('一秒执行一次')
}

setInterval(fn, 1000)
```

### 注意点

传入函数名时不要加小括号：

```js
setInterval(fn, 1000)
```

如果写成下面这样，函数会立即执行一次，而不是交给定时器调用：

```js
setInterval(fn(), 1000)
```

### 定时器编号

`setInterval` 会返回一个定时器编号：

```js
let timerId = setInterval(fn, 1000)
```

### 清除定时器

```js
clearInterval(timerId)
```

如果后续需要重新开启并修改定时器编号，建议用 `let` 声明：

```js
let timerId = setInterval(fn, 1000)
clearInterval(timerId)
timerId = setInterval(fn, 1000)
```

## 10. 案例模块

## 案例一：年会抽奖

### 需求

从姓名数组中随机抽取一等奖、二等奖、三等奖，并显示到页面对应位置。每个人只能中奖一次。

### 用到的语法

- 数组保存姓名
- `Math.random()` 生成随机数
- `Math.floor()` 向下取整
- `document.querySelector()` 获取元素
- `innerText` 修改文本
- `splice()` 删除已中奖的人

### 实现步骤

1. 准备姓名数组。
2. 封装随机整数函数 `getRandom(N, M)`。
3. 随机抽取一个下标。
4. 把对应姓名写入页面。
5. 使用 `splice()` 从数组中删除已抽中的人。
6. 重复抽取二等奖和三等奖。

### 关键代码

```js
function getRandom(N, M) {
  return Math.floor(Math.random() * (M - N + 1) + N)
}

let random = getRandom(0, arrName.length - 1)
document.querySelector('#first').innerText = arrName[random]
arrName.splice(random, 1)
```

## 案例二：随机版轮播图

### 需求

页面打开时，随机展示一张轮播图，并同步修改标题、底部背景色和小圆点激活状态。

### 用到的语法

- 数组对象保存轮播图数据
- `querySelector()` 获取 DOM 元素
- `style.backgroundImage` 修改背景图
- `style.backgroundColor` 修改背景色
- `innerHTML` 修改标题内容
- `classList.add()` 添加激活类名
- `:nth-child()` 选择对应的小圆点

### 数据结构

```js
const sliderData = [
  { url: './assets/slider01.jpg', title: '标题', color: 'rgb(100, 67, 68)' }
]
```

### 实现步骤

1. 准备轮播图数据数组，每一项包含图片地址、标题和主题色。
2. 生成随机下标。
3. 获取图片区域、底部区域、标题区域和对应小圆点。
4. 根据随机下标更新背景图、标题、底部颜色。
5. 给对应的小圆点添加 `active` 类名。

### 关键代码

```js
const random = getRandom(0, sliderData.length - 1)

sliderWrapper.style.backgroundImage = `url(${sliderData[random].url})`
sliderFooter.style.backgroundColor = sliderData[random].color
sliderFooterP.innerHTML = sliderData[random].title

const li = document.querySelector(`.slider-footer-dot ul li:nth-child(${random + 1})`)
li.classList.add('active')
```

### 易错点

`:nth-child()` 从 `1` 开始计数，而数组下标从 `0` 开始，所以要写 `random + 1`。

## 案例三：阅读注册协议倒计时

### 需求

用户进入页面后，按钮默认禁用。倒计时结束后，按钮变为可点击，并修改按钮文字。

### 用到的语法

- `setInterval()` 开启定时器
- `clearInterval()` 清除定时器
- `disabled` 控制按钮禁用状态
- `innerHTML` 修改按钮文字
- `if` 判断倒计时是否结束

### 实现步骤

1. 获取按钮元素。
2. 设置倒计时变量。
3. 每隔 1 秒更新按钮文字。
4. 倒计时为 0 时清除定时器。
5. 修改 `disabled` 为 `false`，恢复按钮点击。

### 关键代码

```js
const btn = document.querySelector('.btn')
let i = 5

function fn() {
  if (i === 0) {
    clearInterval(timerId)
    btn.disabled = false
    btn.innerHTML = '我同意用户协议'
  } else {
    btn.innerHTML = `我已阅读用户协议(${i})`
    i--
  }
}

let timerId = setInterval(fn, 1000)
```

## 案例四：定时器版轮播图

### 需求

轮播图不再随机显示，而是每隔一段时间自动切换下一张图片，并同步切换标题、背景色和小圆点。

### 用到的语法

- 数组对象保存多张图片数据
- 变量 `i` 保存当前轮播图下标
- `setInterval()` 周期性切换
- `style` 修改图片和颜色
- `innerHTML` 修改标题
- `classList.remove()` 移除旧小圆点样式
- `classList.add()` 添加新小圆点样式
- `if` 判断下标是否需要归零

### 实现步骤

1. 准备轮播图数据数组。
2. 定义变量 `i = 0` 表示当前显示第几张。
3. 封装切换函数 `fn()`。
4. 在函数中根据 `i` 更新图片、标题和底部颜色。
5. 先移除原来的 `active` 类名，再给当前小圆点添加 `active`。
6. 每次切换后让 `i++`。
7. 当 `i >= sliderData.length` 时，把 `i` 重置为 `0`。
8. 使用 `setInterval(fn, 1000)` 自动执行。

### 关键代码

```js
let i = 0

function fn() {
  sliderWrapper.style.backgroundImage = `url(${sliderData[i].url})`
  sliderFooter.style.backgroundColor = sliderData[i].color
  sliderFooterP.innerHTML = sliderData[i].title

  document.querySelector('.slider-footer-dot ul .active').classList.remove('active')
  document.querySelector(`.slider-footer-dot ul li:nth-child(${i + 1})`).classList.add('active')

  i++
  if (i >= sliderData.length) {
    i = 0
  }
}

setInterval(fn, 1000)
```

### 易错点

- 删除旧的 `active` 后，再给新的小圆点添加 `active`。
- `i++` 后要判断是否超出数组长度。
- 小圆点选择器仍然需要 `i + 1`，因为 `nth-child` 从 `1` 开始。

## 11. 本章重点回顾

- 获取元素：`querySelector()`、`querySelectorAll()`。
- 修改内容：`innerText` 修改纯文本，`innerHTML` 可以解析标签。
- 修改属性：`对象.属性 = 值`。
- 修改样式：少量样式用 `style`，多个样式优先考虑 `classList`。
- 表单属性：`value`、`type`、`checked`、`disabled`、`selected`。
- 自定义属性：HTML 中写 `data-xxx`，JS 中用 `dataset.xxx`。
- 定时器：`setInterval()` 开启，`clearInterval()` 清除。
- 案例思路：先准备数据，再获取元素，最后根据数据更新 DOM。
