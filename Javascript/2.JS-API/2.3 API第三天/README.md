# JS API 第三天：DOM 事件总结

本章主要学习 DOM 事件体系：如何监听事件、事件如何在页面元素之间流动、如何解绑事件、如何利用事件委托减少事件绑定次数，以及如何处理表单默认行为、页面加载和滚动事件。

## 章节目录

1. [全选复选框](#1-全选复选框)
2. [事件流](#2-事件流)
3. [解绑事件](#3-解绑事件)
4. [事件委托](#4-事件委托)
5. [案例：Tab 栏切换](#5-案例tab-栏切换)
6. [阻止默认行为](#6-阻止默认行为)
7. [其他事件：加载与滚动](#7-其他事件加载与滚动)
8. [本章 API 速查](#本章-api-速查)
9. [易错点清单](#易错点清单)
10. [复习问题](#复习问题)

## 1. 全选复选框

对应文件：`1. 全选复选框.html`

### 学习目标

掌握复选框 `checked` 属性的读写，理解“大复选框控制小复选框”和“小复选框反向控制大复选框”的实现思路。

### 核心知识点

- `checkbox.checked` 可以读取或设置复选框是否被选中。
- `querySelector()` 获取单个元素，`querySelectorAll()` 获取一组元素。
- `querySelectorAll()` 返回的是类数组节点集合，可以用 `for` 循环遍历。
- CSS 伪类选择器 `:checked` 可以选中当前已勾选的表单元素。

### 代码逻辑

大复选框控制所有小复选框：

```js
checkAll.addEventListener('click', function () {
  for (let i = 0; i < cks.length; i++) {
    cks[i].checked = this.checked
  }
})
```

这里的 `this` 指向绑定事件的 `checkAll`，所以 `this.checked` 就是大复选框当前的选中状态。把每个小复选框的 `checked` 都设置成这个值，就能实现全选或全不选。

小复选框反向控制大复选框：

```js
checkAll.checked = document.querySelectorAll('.ck:checked').length === cks.length
```

这句代码的意思是：如果被选中的小复选框数量等于所有小复选框数量，说明全部选中，大复选框也应该被勾选；否则大复选框取消勾选。

### 易错点

- `checked` 是布尔值，不是字符串。
- `this.checked` 只能在普通函数中按当前写法稳定指向事件源；如果换成箭头函数，`this` 不再指向当前 DOM 元素。
- `.ck:checked` 表示同时满足 class 为 `ck` 且被选中的元素。

## 2. 事件流

对应文件：`2. 事件流.html`

### 学习目标

理解事件从发生到被处理的完整过程，掌握捕获阶段、目标阶段、冒泡阶段，以及如何阻止事件继续传播。

### 事件流三个阶段

1. 捕获阶段：事件从外层向内层传递，例如 `document -> html -> body -> father -> son`。
2. 目标阶段：事件到达真正触发事件的目标元素。
3. 冒泡阶段：事件从目标元素向外层父级逐级传递，例如 `son -> father -> body -> html -> document`。

### `addEventListener` 的第三个参数

```js
element.addEventListener('click', function () {
  // 事件处理函数
}, true)
```

- 第三个参数为 `true`：在捕获阶段触发。
- 第三个参数为 `false` 或省略：在冒泡阶段触发，默认就是冒泡阶段。

### 阻止事件传播

```js
son.addEventListener('click', function (e) {
  e.stopPropagation()
  alert('我是儿子')
})
```

`stopPropagation()` 可以阻止事件继续传播。它既可以阻止冒泡，也可以阻止捕获中继续传递，关键取决于事件当前处于哪个阶段。

### 易错点

- 不写第三个参数时，默认是冒泡阶段。
- 点击子元素时，父元素的同名事件也可能被触发，这是冒泡造成的。
- 如果同一个元素重复绑定多个相同事件处理函数，它们可能都会执行。示例中 `son` 对 `click` 有多次绑定，学习时要注意区分每段代码的演示目的。

## 3. 解绑事件

对应文件：`3. 解绑事件.html`

### 学习目标

掌握传统事件绑定和 `addEventListener` 方式的解绑方法，理解为什么匿名函数不能直接解绑。

### 传统事件解绑

```js
btn.onclick = function () {
  alert('点击事件')
}

btn.onclick = null
```

通过 `onclick` 这种方式绑定的事件，可以把对应属性设置为 `null` 来解绑。

### `addEventListener` 解绑

错误示例：

```js
btn.addEventListener('click', function () {
  alert('点击事件')
})

btn.removeEventListener('click', function () {
  alert('点击事件')
})
```

这两处看起来代码一样，但它们是两个不同的函数对象，所以无法解绑成功。

正确写法：

```js
function fn() {
  alert('点击事件')
}

btn.addEventListener('click', fn)
btn.removeEventListener('click', fn)
```

解绑时必须传入和绑定时同一个函数引用。

### 鼠标经过相关事件

- `mouseover`：鼠标经过元素时触发，会冒泡。
- `mouseout`：鼠标离开元素时触发，会冒泡。
- `mouseenter`：鼠标移入元素时触发，不会冒泡。
- `mouseleave`：鼠标移出元素时触发，不会冒泡。

### 易错点

- 匿名函数无法用另一段匿名函数解绑。
- `removeEventListener()` 的事件类型、函数引用、捕获参数需要和绑定时对应。
- 如果不希望鼠标移入子元素时影响父级事件，优先考虑 `mouseenter` / `mouseleave`。

## 4. 事件委托

对应文件：`4 .事件委托.html`

### 学习目标

理解事件委托的原理，学会把事件绑定到父元素上，再通过 `e.target` 判断真正触发事件的子元素。

### 核心思想

事件委托本质上是利用事件冒泡：子元素触发事件后，事件会冒泡到父元素，因此可以只给父元素绑定一次事件。

优点：

- 减少事件注册次数。
- 提高程序性能。
- 对动态新增的子元素也更友好，因为事件绑定在父元素上。

### 代码逻辑

```js
const ul = document.querySelector('ul')

ul.addEventListener('click', function (e) {
  if (e.target.tagName === 'LI') {
    e.target.style.color = 'red'
  }
})
```

这里点击 `li` 时，事件会冒泡到 `ul`。事件处理函数绑定在 `ul` 上，但真正被点击的元素可以通过 `e.target` 获取。

### `target` 和 `tagName`

- `e.target`：真正触发事件的元素。
- `e.currentTarget` 或 `this`：绑定事件处理函数的元素。
- `tagName` 返回大写标签名，例如 `LI`、`A`、`DIV`。

### 易错点

- 判断标签名时要写大写：`e.target.tagName === 'LI'`。
- 事件委托需要依赖冒泡，不适合所有不冒泡的事件。
- 如果父元素里有不需要响应的子元素，必须做条件判断。

## 5. 案例：Tab 栏切换

对应文件：`4 .X（案例）Tab栏切换（条件委托版）.html`

### 学习目标

综合使用事件委托、排他思想、`classList`、自定义属性 `data-*` 和 `dataset` 完成 Tab 栏切换。

### 页面结构

- 导航区域：多个 `a` 标签，每个标签都有 `data-id`。
- 内容区域：多个 `.item`，默认只有一个 `.item.active` 显示。
- CSS 通过 `.active` 控制当前选中的导航样式和内容显示状态。

### 实现步骤

1. 把点击事件绑定到 `.tab-nav ul` 上，使用事件委托。
2. 判断点击目标必须是 `A` 标签。
3. 移除当前导航项的 `active` 类。
4. 给被点击的导航项添加 `active` 类。
5. 读取当前导航项的 `data-id`。
6. 移除当前内容项的 `active` 类。
7. 根据索引找到对应内容项，并添加 `active` 类。

### 关键代码

```js
ul.addEventListener('click', function (e) {
  if (e.target.tagName === 'A') {
    document.querySelector('.tab-nav .active').classList.remove('active')
    e.target.classList.add('active')

    const i = +e.target.dataset.id

    document.querySelector('.tab-content .active').classList.remove('active')
    list[i].classList.add('active')
  }
})
```

### 关键知识点

- `classList.add('active')`：添加类名。
- `classList.remove('active')`：移除类名。
- `dataset.id`：读取 `data-id` 自定义属性。
- `+e.target.dataset.id`：把字符串类型的 `data-id` 转成数字，方便作为数组索引使用。
- `list[i]`：通过索引找到对应内容面板。

### 两种内容切换思路

第一种：使用 `nth-child()` 拼接选择器。

```js
document.querySelector(`.tab-content .item:nth-child(${i + 1})`).classList.add('active')
```

第二种：提前获取所有内容项，再用索引访问。

```js
const list = document.querySelectorAll('.tab-content .item')
list[i].classList.add('active')
```

当前示例使用第二种方式，更直观，也避免频繁拼接选择器。

### 易错点

- 点击事件绑定在 `ul` 上，但真正要处理的是 `a`，所以必须判断 `e.target.tagName === 'A'`。
- `dataset` 读取到的是字符串，作为索引时建议转成数字。
- 每次切换前要先移除旧的 `active`，再添加新的 `active`，这就是排他思想。
- `href="javascript:;"` 的作用是让链接点击后不跳转到新页面。

## 6. 阻止默认行为

对应文件：`5 .阻止默认行为.html`

### 学习目标

掌握 `preventDefault()` 的使用，理解“默认行为”和“事件处理逻辑”的区别。

### 表单默认提交

表单里的提交按钮默认会触发表单提交，浏览器会根据 `form` 的 `action` 地址跳转或发送请求。

```html
<form action="https://www.baidu.com">
  <input type="text" name="username" placeholder="请输入用户名">
  <input type="password" name="password" placeholder="请输入密码">
  <button type="submit">登录</button>
</form>
```

### 阻止默认提交

```js
const form = document.querySelector('form')

form.addEventListener('submit', function (e) {
  e.preventDefault()
})
```

`preventDefault()` 只阻止浏览器的默认行为，不会阻止事件冒泡。如果既要阻止默认行为又要阻止传播，需要分别调用 `preventDefault()` 和 `stopPropagation()`。

### 常见默认行为

- 表单提交后跳转。
- `a` 标签点击后跳转。
- 鼠标右键打开菜单。
- 某些键盘快捷键触发浏览器默认操作。

### 易错点

- `preventDefault` 是方法，调用时要写括号：`e.preventDefault()`。
- 监听表单提交时，通常绑定 `submit` 事件到 `form`，而不是只绑定按钮的 `click`。
- 阻止默认行为不等于阻止冒泡。

## 7. 其他事件：加载与滚动

对应文件：`6 .其他事件.html`

### 学习目标

了解页面加载事件、DOM 加载事件、图片加载事件和滚动事件，掌握 `scrollTop` 与 `scrollTo()` 的基本用法。

### 页面加载事件 `load`

```js
window.addEventListener('load', function () {
  console.log('页面加载完成')
})
```

`load` 会等页面所有资源加载完成后再执行，包括 HTML、CSS、JS、图片等资源。适合处理依赖完整资源的逻辑。

### DOM 加载事件 `DOMContentLoaded`

```js
document.addEventListener('DOMContentLoaded', function () {
  console.log('DOM加载完成')
})
```

`DOMContentLoaded` 只需要 DOM 结构加载完成，不需要等待图片等外部资源加载完成。它通常比 `load` 更早触发。

### 图片加载和失败事件

```js
img.addEventListener('load', function () {
  console.log('图片加载完成')
})

img.addEventListener('error', function () {
  console.log('图片加载失败')
})
```

图片资源可以单独监听 `load` 和 `error`，用于判断图片是否加载成功。

### 元素滚动事件

```js
const div = document.querySelector('div')

div.addEventListener('scroll', function () {
  div.scrollTop > 100 ? console.log('滚动条滚了100px') : console.log('滚动条没滚')
})
```

`scrollTop` 表示元素内容向上滚动出去的距离，也就是滚动条距离顶部的距离。它返回不带单位的数字，默认单位可以理解为 `px`。

### 页面滚动事件

```js
window.addEventListener('scroll', function () {
  const n = document.documentElement.scrollTop

  if (n >= 100) {
    div.style.display = 'block'
  } else {
    div.style.display = 'none'
  }
})
```

页面滚动距离通常通过 `document.documentElement.scrollTop` 获取。示例中，当页面滚动超过 100px 时显示 `div`，否则隐藏。

### 让页面滚动到指定位置

```js
window.scrollTo(0, 0)
```

`scrollTo(x, y)` 可以让页面滚动到指定坐标。第一个参数是水平滚动距离，第二个参数是垂直滚动距离。

### 易错点

- 页面必须有足够高度，才会出现滚动条。
- 监听元素滚动时，元素本身需要有固定高度和 `overflow: scroll` 或类似样式。
- `document.documentElement.scrollTop` 可以读，也可以写。
- `load` 等全部资源加载完成，`DOMContentLoaded` 只等 DOM 结构加载完成。

## 本章 API 速查

### DOM 获取

| API | 作用 |
| --- | --- |
| `document.querySelector(selector)` | 获取匹配选择器的第一个元素 |
| `document.querySelectorAll(selector)` | 获取匹配选择器的所有元素 |

### 事件监听与解绑

| API | 作用 |
| --- | --- |
| `addEventListener(type, fn)` | 注册事件监听 |
| `addEventListener(type, fn, true)` | 在捕获阶段注册事件监听 |
| `removeEventListener(type, fn)` | 解绑事件监听 |
| `onclick = null` | 解绑传统方式绑定的事件 |

### 事件对象

| API / 属性 | 作用 |
| --- | --- |
| `e.target` | 获取真正触发事件的元素 |
| `e.stopPropagation()` | 阻止事件继续传播 |
| `e.preventDefault()` | 阻止浏览器默认行为 |

### 类名与自定义属性

| API | 作用 |
| --- | --- |
| `element.classList.add(className)` | 添加类名 |
| `element.classList.remove(className)` | 移除类名 |
| `element.dataset.xxx` | 读取元素的 `data-xxx` 自定义属性 |

### 表单与状态

| API / 属性 | 作用 |
| --- | --- |
| `checkbox.checked` | 读取或设置复选框选中状态 |
| `:checked` | CSS 选择器，匹配被选中的表单元素 |

### 加载与滚动

| API / 事件 | 作用 |
| --- | --- |
| `window.addEventListener('load', fn)` | 页面所有资源加载完成后执行 |
| `document.addEventListener('DOMContentLoaded', fn)` | DOM 结构加载完成后执行 |
| `window.addEventListener('scroll', fn)` | 页面滚动时触发 |
| `element.addEventListener('scroll', fn)` | 元素内部滚动时触发 |
| `document.documentElement.scrollTop` | 获取或设置页面垂直滚动距离 |
| `element.scrollTop` | 获取或设置元素内部垂直滚动距离 |
| `window.scrollTo(x, y)` | 滚动到指定页面位置 |

## 易错点清单

- `addEventListener()` 默认在冒泡阶段执行。
- 如果要解绑 `addEventListener()` 绑定的事件，必须使用同一个函数引用。
- 匿名函数看起来代码一样，也不是同一个函数。
- 事件委托要通过 `e.target` 找真正触发事件的子元素。
- `tagName` 返回的是大写标签名，例如 `LI`、`A`。
- `preventDefault()` 只阻止默认行为，不阻止冒泡。
- `stopPropagation()` 只阻止事件传播，不阻止默认行为。
- `dataset` 读取到的值是字符串，需要数字时可以用 `+` 转换。
- `classList` 操作的是类名，不需要写点号。
- `scrollTop` 返回数字，不带 `px` 单位。
- 页面滚动事件要生效，页面本身需要有足够高度。
- 元素滚动事件要生效，元素需要能产生内部滚动。

## 复习问题

1. 大复选框如何控制所有小复选框的选中状态？
2. 小复选框如何反向判断是否应该勾选大复选框？
3. 事件流分为哪三个阶段？
4. `addEventListener()` 的第三个参数有什么作用？
5. `stopPropagation()` 和 `preventDefault()` 有什么区别？
6. 为什么匿名函数不能直接解绑事件？
7. `mouseover` 和 `mouseenter` 的区别是什么？
8. 事件委托为什么能减少事件注册次数？
9. `e.target` 和 `this` 在事件委托里分别表示什么？
10. Tab 栏切换为什么需要先移除旧的 `active`？
11. `dataset.id` 读取到的是什么类型？
12. `load` 和 `DOMContentLoaded` 哪个触发更早？为什么？
13. 页面滚动距离应该通过哪个属性读取？
14. `scrollTo(0, 0)` 的作用是什么？

## 本章学习主线

本章可以按照这条线来理解：

```text
事件监听 -> 事件流 -> 事件解绑 -> 事件委托 -> 默认行为 -> 加载和滚动事件
```

先学会给元素绑定事件，再理解事件为什么会影响父子元素；接着学习如何解绑事件、如何借助冒泡做事件委托；最后补充实际开发中常见的表单提交、页面加载和页面滚动处理。
