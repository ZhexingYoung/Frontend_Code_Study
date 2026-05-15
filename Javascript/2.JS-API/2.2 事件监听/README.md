# Web APIs 第二天：事件监听

本章主要学习浏览器中的事件机制：用户点击、鼠标移入移出、输入框聚焦、键盘输入等行为，都可以通过事件监听让页面做出响应。

学习主线：

1. 获取 DOM 元素。
2. 给元素绑定事件监听。
3. 在事件触发时执行回调函数。
4. 结合样式、内容、类名、定时器和事件对象完成页面交互。

## 1. 事件监听

对应文件：`1. 事件监听.html`

### 需求

让页面中的某个元素在用户操作时执行指定代码，例如点击按钮后弹窗、开始随机抽题、停止定时器等。

### 概念

事件监听就是“监听用户行为”。当用户触发某个事件时，浏览器会自动执行提前绑定好的事件处理函数。

事件监听由三部分组成：

- 事件源：触发事件的 DOM 元素。
- 事件类型：用户做了什么操作，比如 `click`、`mouseenter`、`keydown`。
- 事件处理函数：事件发生后要执行的代码。

### 语法

```js
元素对象.addEventListener('事件类型', function () {
  // 事件触发后执行的代码
})
```

示例：

```js
const btn = document.querySelector('button')

btn.addEventListener('click', function () {
  alert('点击事件')
})
```

### 使用场景

- 点击按钮后切换内容。
- 鼠标移入后显示提示。
- 输入框输入时实时统计字数。
- 键盘按下回车时发布评论。
- 鼠标移入轮播图时停止自动播放。

### 案例：随机问答

案例需求：

- 点击“开始”按钮后，每隔一段时间随机显示一个问题。
- 点击“结束”按钮后，停止随机切换。
- 已经抽中的问题从数组中删除，避免重复抽取。
- 当只剩最后一个问题时，直接显示并禁用按钮。

核心思路：

```js
const btnStart = document.querySelector('.start')
const btnEnd = document.querySelector('.end')
const qs = document.querySelector('.box1 p span')

let timeId = 0
let random = 0

btnStart.addEventListener('click', function () {
  timeId = setInterval(function () {
    random = Math.floor(Math.random() * questionList.length)
    qs.innerText = questionList[random].question
  }, 200)
})

btnEnd.addEventListener('click', function () {
  clearInterval(timeId)
  questionList.splice(random, 1)
})
```

### 重点

- `addEventListener()` 可以给同一个元素绑定多个事件，不会互相覆盖。
- 旧写法 `onclick` 后面的事件处理函数会覆盖前面的。
- 定时器返回值要用变量保存，后续才能通过 `clearInterval()` 清除。
- `disabled = true` 可以禁用按钮，避免用户重复点击。

### 易错点

- 方法名是 `addEventListener`，不要写成 `addEvenListener`。
- `setInterval()` 会持续执行，不需要时一定要清除。
- `Math.random()` 生成的是 `[0, 1)` 的小数，通常要配合 `Math.floor()` 获取数组下标。
- 删除数组元素前，要确认保存的随机下标仍然对应当前数组。

## 2. 事件类型

对应文件：`2. 事件类型.html`

### 需求

认识常见事件类型，知道不同用户行为应该使用哪一种事件。

### 语法

事件类型以字符串形式写在 `addEventListener()` 的第一个参数中：

```js
元素.addEventListener('click', function () {
  // 点击时执行
})
```

### 常见事件分类

鼠标事件：

- `click`：鼠标点击。
- `mouseenter`：鼠标移入。
- `mouseleave`：鼠标移出。
- `mousemove`：鼠标移动。
- `mousedown`：鼠标按下。
- `mouseup`：鼠标松开。

焦点事件：

- `focus`：元素获得焦点。
- `blur`：元素失去焦点。

键盘事件：

- `keydown`：键盘按下。
- `keyup`：键盘松开。

表单事件：

- `input`：表单内容输入时触发。
- `change`：表单内容改变后触发。
- `submit`：表单提交。
- `reset`：表单重置。

### 使用场景

- 按钮点击：`click`。
- 鼠标移入显示、移出隐藏：`mouseenter`、`mouseleave`。
- 输入框获得焦点时高亮：`focus`。
- 输入框失去焦点时隐藏提示：`blur`。
- 实时统计输入内容：`input`。
- 判断用户是否按下回车：`keyup` 或 `keydown`。

### 重点

- 事件类型必须写成字符串。
- 不同事件触发时机不同，选择事件时要看需求。
- 表单实时输入通常用 `input`，不要只依赖键盘事件，因为粘贴、删除、输入法等也会改变内容。

### 易错点

- `keydown` 触发时，输入框的最新内容不一定已经完全更新；实时获取文本更推荐 `input`。
- `focus` 和 `blur` 常用于输入框、文本域等可聚焦元素。

## 2. 案例：轮播图事件版本

对应文件：`2. X(案例)轮播图（事件版本）.html`

### 需求

实现一个可以点击切换、自动播放、鼠标移入暂停、鼠标移出继续播放的轮播图。

### 涉及语法

获取元素：

```js
const sliderWrapper = document.querySelector('.slider-wrapper')
const nextButton = document.querySelector('.next')
const prevButton = document.querySelector('.prev')
```

修改样式：

```js
sliderWrapper.style.backgroundImage = `url(${sliderData[i].url})`
sliderFooter.style.backgroundColor = sliderData[i].color
```

切换类名：

```js
document.querySelector('.slider-footer-dot ul .active').classList.remove('active')
document.querySelector(`.slider-footer-dot ul li:nth-child(${i + 1})`).classList.add('active')
```

绑定事件：

```js
nextButton.addEventListener('click', function () {
  i++
  if (i >= sliderData.length) {
    i = 0
  }
  render()
})
```

自动播放：

```js
let timerId = setInterval(function () {
  nextButton.click()
}, 1000)
```

### 案例思路

1. 准备轮播图数据数组 `sliderData`，每一项包含图片地址、标题和背景色。
2. 用变量 `i` 保存当前显示的图片下标。
3. 封装 `render()` 函数，根据 `i` 渲染图片、文字、背景色和小圆点状态。
4. 点击右按钮时 `i++`，超过最后一张就回到第一张。
5. 点击左按钮时 `i--`，小于 `0` 就回到最后一张。
6. 使用 `setInterval()` 自动触发右按钮点击。
7. 鼠标移入大盒子时清除定时器，鼠标移出时重新开启定时器。

### 使用场景

- 首页焦点图。
- 商品推荐轮播。
- 图片新闻切换。
- 带自动播放和手动切换的展示组件。

### 重点

- 数据驱动页面：页面显示内容来自 `sliderData` 数组。
- 重复渲染逻辑要封装成函数，比如 `render()`。
- 自动播放可以复用“下一张按钮”的点击逻辑：`nextButton.click()`。
- 小圆点和图片下标有关，DOM 的 `nth-child()` 从 `1` 开始，数组下标从 `0` 开始，所以要用 `i + 1`。

### 易错点

- 左按钮事件中也应该调用负责渲染的函数，保证图片、文字和小圆点同步更新。
- 重启定时器前要注意是否已经存在定时器，避免重复开启多个定时器。
- `classList.remove()` 前要确保页面中存在 `.active` 元素。
- CSS 中 `font-weight: solid` 不是常用有效值，常用值是 `normal`、`bold` 或数字。

## 3. 焦点事件

对应文件：`3. 焦点事件.html`

### 需求

输入框获得焦点时显示搜索建议并高亮边框，失去焦点时隐藏搜索建议并取消高亮。

### 概念

焦点表示用户当前正在操作某个可输入或可选择的元素。

- 获得焦点：用户点击输入框，准备输入内容。
- 失去焦点：用户点击页面其他位置，离开输入框。

### 语法

```js
元素.addEventListener('focus', function () {
  // 获得焦点时执行
})

元素.addEventListener('blur', function () {
  // 失去焦点时执行
})
```

案例代码：

```js
const ipt = document.querySelector('[type=search]')
const rlist = document.querySelector('.result-list')

ipt.addEventListener('focus', function () {
  rlist.style.display = 'block'
  ipt.classList.add('search')
})

ipt.addEventListener('blur', function () {
  rlist.style.display = 'none'
  ipt.classList.remove('search')
})
```

### 使用场景

- 搜索框获得焦点后显示联想词。
- 输入框获得焦点后改变边框颜色。
- 表单失去焦点后做格式校验。
- 登录框失去焦点后提示用户名或密码是否为空。

### 重点

- `focus` 适合做“进入输入状态”的效果。
- `blur` 适合做“离开输入状态”的效果。
- 控制样式既可以直接改 `style`，也可以通过 `classList` 添加或删除类名。

### 易错点

- 如果点击搜索建议列表导致输入框失焦，建议列表可能会立刻隐藏。
- `display = 'none'` 会让元素完全隐藏并不占位置。
- 类名操作时，CSS 中要提前写好对应类名的样式。

## 4. 键盘事件与输入事件

对应文件：`4. 键盘事件.html`

### 需求

监听用户在输入框中的键盘操作，并实时获取输入框内容。

### 概念

键盘事件用于判断用户按下或松开了键盘上的按键。输入事件用于监听表单内容是否发生变化。

### 语法

键盘按下：

```js
ipt.addEventListener('keydown', function () {
  console.log('键盘按下')
})
```

键盘松开：

```js
ipt.addEventListener('keyup', function () {
  console.log('键盘松开')
})
```

实时输入：

```js
ipt.addEventListener('input', function () {
  console.log(ipt.value)
})
```

### 使用场景

- 按下回车发布评论。
- 输入内容时实时搜索。
- 输入密码时实时显示强度。
- 文本域输入时实时统计字数。

### 重点

- 获取输入框内容使用 `元素.value`。
- `input` 事件更适合做内容实时监听。
- `keydown` 和 `keyup` 更适合判断具体按键。

### 易错点

- `innerHTML`、`innerText` 用于普通标签内容，表单输入值要用 `value`。
- 用户粘贴内容也会触发 `input`，但不一定符合键盘按键逻辑。
- 判断具体按键需要配合事件对象的 `e.key`。

## 4. 案例：字数统计

对应文件：`4. X(案例)字数统计.html`

### 需求

评论输入框获得焦点时显示字数统计，失去焦点时隐藏字数统计，输入内容时实时更新当前字数。

### 涉及语法

```js
const taipt = document.querySelector('#tx')
const wt = document.querySelector('.total')

taipt.addEventListener('focus', function () {
  wt.style.opacity = '1'
})

taipt.addEventListener('blur', function () {
  wt.style.opacity = '0'
})

taipt.addEventListener('input', function () {
  wt.innerHTML = `${taipt.value.length} / 200字`
})
```

### 案例思路

1. 获取文本域 `textarea` 和字数提示元素。
2. 聚焦时让提示元素显示。
3. 失焦时让提示元素隐藏。
4. 输入时通过 `taipt.value.length` 获取当前输入字数。
5. 把统计结果渲染到页面中。

### 使用场景

- 评论区字数统计。
- 微博、动态、留言板输入限制。
- 表单备注字段限制。

### 重点

- 字符串有 `length` 属性，可以获取字符数量。
- `maxlength="200"` 可以限制输入框最多输入 200 个字符。
- 使用 `input` 事件能实时响应内容变化。

### 易错点

- 代码中的 `i++` 对字数统计不是必须的，直接使用 `taipt.value.length` 更准确。
- 字数显示建议和 `maxlength` 保持一致，避免页面提示和实际限制不一致。
- `opacity = 0` 只是透明，元素仍然占位置；`display = none` 才是不占位置。

## 5. 事件对象

对应文件：`5. 事件对象.html`

### 需求

在事件触发时，获取本次事件的详细信息，比如按下了哪个键、点击了哪个元素、鼠标坐标是多少。

### 概念

事件对象是浏览器在事件触发时自动传入事件处理函数的对象，里面保存了本次事件相关的信息。

### 语法

事件处理函数的第一个参数通常写成 `e`：

```js
元素.addEventListener('click', function (e) {
  console.log(e)
})
```

判断是否按下回车：

```js
ipt.addEventListener('keydown', function (e) {
  if (e.key === 'Enter') {
    console.log('Enter键被按下')
  }
})
```

### 常见属性

- `e.target`：真正触发事件的元素。
- `e.currentTarget`：绑定事件的元素。
- `e.type`：事件类型。
- `e.timeStamp`：事件触发时间。
- `e.clientX`、`e.clientY`：鼠标在浏览器可视区域中的坐标。
- `e.offsetX`、`e.offsetY`：鼠标在目标元素内部的坐标。
- `e.pageX`、`e.pageY`：鼠标在页面中的坐标。
- `e.screenX`、`e.screenY`：鼠标在屏幕中的坐标。
- `e.key`：键盘事件中按下的按键名称。

### 使用场景

- 判断用户是否按下 `Enter`。
- 判断用户点击的是哪个元素。
- 获取鼠标位置实现拖拽或跟随效果。
- 根据事件类型做不同逻辑。

### 重点

- 事件对象由浏览器自动传入，不需要自己创建。
- 事件对象参数名可以自定义，但常用 `e`。
- 判断按键时，推荐使用 `e.key`。

### 易错点

- `e.target` 和 `this` 不一定永远相同，尤其是父元素监听子元素事件时。
- `e.key` 的值区分具体按键名称，例如回车是 `'Enter'`。
- 写事件对象参数后，才能在函数内部使用 `e.key`、`e.target` 等属性。

## 5. 案例：按下回车发布评论

对应文件：`5. X(案例)按下回车发布.html`

### 需求

用户在评论框中输入内容后，按下回车发布评论；如果输入的是空内容或纯空格，则不发布。

### 涉及语法

监听键盘松开并判断回车：

```js
taipt.addEventListener('keyup', function (e) {
  if (e.key === 'Enter') {
    // 发布逻辑
  }
})
```

去除字符串左右空格：

```js
taipt.value.trim()
```

显示评论内容：

```js
item.style.display = 'block'
text.innerHTML = `${taipt.value.trim()}`
```

清空输入框和字数统计：

```js
taipt.value = ''
wt.innerHTML = `0 / 200字`
```

### 案例思路

1. 获取评论输入框、字数提示、评论列表项和评论文本元素。
2. 使用 `focus` 和 `blur` 控制字数提示显示隐藏。
3. 使用 `input` 实时统计字数。
4. 使用 `keyup` 获取事件对象。
5. 判断 `e.key === 'Enter'`，确认用户是否按下回车。
6. 使用 `trim()` 判断输入内容是否为空。
7. 不为空时显示评论，并把内容写入页面。
8. 发布后清空输入框和字数统计。

### 使用场景

- 评论区回车发布。
- 聊天窗口发送消息。
- 搜索框回车搜索。
- 表单快捷提交。

### 重点

- `trim()` 可以去除字符串左右两边的空格。
- 空字符串 `''` 在条件判断中会被当成 `false`。
- 发布后要重置输入框内容和字数提示。

### 易错点

- 只判断 `taipt.value !== ''` 不能排除纯空格输入，应该用 `taipt.value.trim()`。
- 如果使用 `keyup` 监听回车，文本域中可能会留下换行符，发布后要及时清空。
- 把用户输入直接写入 `innerHTML` 有安全风险；真实项目中更推荐使用 `innerText` 或做内容转义。

## 6. 环境对象 this 与回调函数

对应文件：`6. 环境对象.html`

### 需求

理解函数执行时 `this` 指向谁，以及什么是回调函数。

### 概念

`this` 是函数运行时所在环境的对象。简单理解：谁调用函数，`this` 就指向谁。

普通函数直接调用时：

```js
function fn() {
  console.log(this)
}

fn() // window
```

作为事件处理函数时：

```js
btn.addEventListener('click', function () {
  console.log(this)
})
```

这里的 `this` 通常指向绑定事件的元素。

回调函数：把函数作为参数传递给另一个函数，这个函数在合适的时候再被调用。

```js
function fn() {
  console.log('我是回调函数')
}

setInterval(fn, 1000)
```

### 使用场景

- 事件监听中的事件处理函数。
- 定时器中的执行函数。
- 数组方法中的处理函数。
- 后续异步请求中的成功或失败处理函数。

### 重点

- 普通函数直接调用时，非严格模式下 `this` 通常指向 `window`。
- 事件处理函数中，普通函数的 `this` 通常指向绑定事件的 DOM 元素。
- 回调函数的本质仍然是函数，只是作为参数传递。

### 易错点

- 箭头函数没有自己的 `this`，它会使用外层作用域的 `this`。
- 需要使用当前触发元素时，普通函数中可以用 `this`。
- 如果不确定 `this` 指向，可以使用 `console.log(this)` 打印观察。
- `setInterval(fn, 1000)` 是把函数交给定时器，不要写成 `setInterval(fn(), 1000)`。

## 7. 综合案例：Tab 栏切换

对应文件：`7. 综合案例.html`

### 需求

鼠标经过不同的导航标签时，切换当前高亮标签，并显示对应的内容区域。

### 涉及语法

获取多个元素：

```js
const as = document.querySelectorAll('.tab-nav a')
```

循环绑定事件：

```js
for (let i = 0; i < as.length; i++) {
  as[i].addEventListener('mouseenter', function () {
    // 切换逻辑
  })
}
```

切换导航高亮：

```js
document.querySelector('.tab-nav .active').classList.remove('active')
this.classList.add('active')
```

切换内容区域：

```js
document.querySelector('.tab-content .active').classList.remove('active')
document.querySelector(`.tab-content .item:nth-child(${i + 1})`).classList.add('active')
```

### 案例思路

1. 使用 `querySelectorAll()` 获取所有导航链接。
2. 使用 `for` 循环给每个导航链接绑定 `mouseenter` 事件。
3. 鼠标移入时，先移除原来的导航高亮。
4. 使用 `this.classList.add('active')` 给当前鼠标移入的链接添加高亮。
5. 移除原来的内容区域高亮。
6. 根据循环下标 `i` 找到对应内容区域，并添加 `active` 类名。

### 使用场景

- 商品分类切换。
- 新闻频道切换。
- 用户中心菜单切换。
- 图片或内容面板切换。

### 重点

- `querySelectorAll()` 获取的是一组元素，需要遍历后逐个绑定事件。
- `let i` 有块级作用域，适合在循环事件中保存当前下标。
- `this` 在普通事件处理函数中指向当前绑定事件的元素。
- 导航项和内容项数量、顺序要保持一致。

### 易错点

- 如果使用 `var i`，事件触发时可能拿不到期望的当前下标。
- `nth-child()` 从 `1` 开始，数组或伪数组下标从 `0` 开始，所以要写 `i + 1`。
- 切换前要先移除旧的 `.active`，否则可能出现多个元素同时高亮。
- 如果事件处理函数写成箭头函数，`this` 不会指向当前导航链接。

## 本章知识点总览

### 核心 API

```js
document.querySelector('选择器')
document.querySelectorAll('选择器')
元素.addEventListener('事件类型', 回调函数)
元素.classList.add('类名')
元素.classList.remove('类名')
元素.style.样式名 = '样式值'
setInterval(回调函数, 间隔时间)
clearInterval(定时器编号)
```

### 常见事件选择

- 点击按钮：`click`
- 鼠标移入：`mouseenter`
- 鼠标移出：`mouseleave`
- 获得焦点：`focus`
- 失去焦点：`blur`
- 键盘按下：`keydown`
- 键盘松开：`keyup`
- 内容实时变化：`input`

### 学习重点

- 明确事件三要素：事件源、事件类型、事件处理函数。
- 掌握 `addEventListener()` 的基本写法。
- 会根据需求选择合适事件类型。
- 会使用事件对象 `e` 获取事件信息。
- 会用 `classList` 控制页面状态。
- 会用 `this` 获取当前触发事件的元素。
- 会把重复渲染逻辑封装成函数。

### 高频易错点

- `addEventListener` 拼写错误。
- 事件类型没有加引号。
- 表单内容用错属性，输入框内容应使用 `value`。
- 忘记清除定时器，导致多个定时器同时运行。
- `nth-child()` 和数组下标差 `1`。
- 使用箭头函数后，`this` 指向不符合预期。
- 没有先移除旧的 `.active`，导致多个元素同时处于选中状态。
- 直接使用 `innerHTML` 渲染用户输入，真实项目中可能有安全风险。

## 推荐复习顺序

1. 先记住事件监听三要素。
2. 再熟悉常用事件类型。
3. 用焦点事件和输入事件练习表单交互。
4. 用事件对象练习判断键盘按键。
5. 用轮播图和 Tab 栏案例练习“事件 + 数据 + 渲染函数 + 类名切换”。
