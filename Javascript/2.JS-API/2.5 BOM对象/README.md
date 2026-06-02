# BOM 对象学习笔记

本目录主要学习浏览器对象模型 BOM、浏览器运行机制、页面跳转、浏览器信息、历史记录、本地存储，以及 `map()` / `join()` 在页面渲染中的使用。综合案例通过“学生就业统计表”串联了本地存储、数组渲染、表单提交、事件委托和删除回写等业务逻辑。

> 注意：本 README 只总结当前章节知识点，不修改原 HTML/CSS/资源文件。个别原笔记中容易误解的地方会在“注意”中修正。

## 1. BOM 对象

### 核心概念

BOM，全称 Browser Object Model，浏览器对象模型。它提供了一组对象，让 JavaScript 可以操作浏览器窗口本身，例如弹窗、定时器、地址栏、历史记录、浏览器信息等。

常见 BOM 相关对象：

- `window`：浏览器窗口顶级对象，也是 JavaScript 在浏览器中的全局对象。
- `document`：页面文档对象，属于 DOM，但也挂在 `window` 上。
- `location`：当前页面 URL 地址信息。
- `history`：当前窗口的历史记录。
- `navigator`：浏览器和运行环境信息。
- `screen`：屏幕信息。

### 关键代码

```js
document.querySelector('div')
window.document.querySelector('div')
```

`window` 是顶级对象，所以很多属性和方法可以省略 `window.`。实际开发中一般直接写 `document.querySelector()`、`setTimeout()`，不用刻意写 `window.document`。

### 定时器

延时定时器：指定时间后执行一次。

```js
let timeout = setTimeout(function () {
  console.log('延时函数')
}, 1000)

clearTimeout(timeout)
```

间歇定时器：每隔指定时间重复执行。

```js
const timer = setInterval(function () {
  console.log('间歇函数')
}, 1000)
```

### 业务逻辑拆解

以“倒计时后跳转”为例：

1. 页面显示剩余秒数。
2. 使用 `setInterval()` 每秒减少一次。
3. 秒数归零后清除定时器。
4. 执行页面跳转。

### ERP 开发场景

- 登录页验证码倒计时，例如“60 秒后重新发送验证码”。
- 操作成功后倒计时跳转到列表页。
- 仪表盘页面定时刷新库存、订单、审批数量。
- 长时间无操作时触发登录过期提醒。

## 2. JS 执行机制

### 核心概念

JavaScript 是单线程语言，同一时间只能执行一个主线程任务。如果所有任务都同步执行，耗时任务会阻塞页面渲染和用户交互。

任务大致分为：

- 同步任务：立即进入主线程，按顺序执行。
- 异步任务：先交给浏览器环境处理，满足条件后把回调放入任务队列。
- 事件循环 Event Loop：主线程执行完同步任务后，不断从任务队列中取出可执行的异步回调。

常见异步任务：

- `setTimeout`
- `setInterval`
- 用户事件：`click`、`input`、`submit`
- 网络请求回调
- 资源加载事件：`load`、`error`

### 关键代码

```js
console.log('A')

setTimeout(function () {
  console.log('B')
}, 0)

console.log('C')
```

执行顺序是：

```text
A
C
B
```

原因是 `setTimeout` 的回调不会立刻执行，而是进入任务队列，等主线程同步代码执行完之后再执行。

### 业务逻辑拆解

以“点击按钮保存数据”为例：

1. 用户点击按钮。
2. 浏览器捕获点击事件。
3. 点击回调进入任务队列。
4. 主线程空闲后执行回调。
5. 回调里完成表单校验、数据提交、页面更新。

### ERP 开发场景

- 提交采购单、销售单时，请求后端接口属于异步任务。
- 表格搜索、筛选、分页需要等待用户事件触发。
- 大屏数据刷新不能阻塞用户点击和页面渲染。
- 批量导入时要避免同步大量计算导致页面卡死。

## 3. location 对象

### 核心概念

`location` 表示浏览器地址栏中的 URL 信息，可以读取当前地址，也可以控制页面跳转、刷新。

常见属性和方法：

- `location.href`：读取或设置完整 URL，常用于页面跳转。
- `location.search`：获取查询字符串，例如 `?id=1001&type=order`。
- `location.hash`：获取 hash 值，例如 `#detail`。
- `location.reload()`：重新加载当前页面。
- `location.assign(url)`：跳转到新页面，会保留历史记录。
- `location.replace(url)`：替换当前页面，不保留当前历史记录。

> 注意：`back()` 和 `forward()` 是 `history` 对象的方法，不是标准的 `location` 方法。页面前进后退应使用 `history.back()`、`history.forward()`。

### 关键代码

```js
let i = 5
const span = document.querySelector('span')

const timer = setInterval(function () {
  i--
  span.innerHTML = i

  if (i === 0) {
    clearInterval(timer)
    location.href = 'https://www.baidu.com'
  }
}, 1000)
```

这段逻辑的重点：

1. 使用变量 `i` 保存倒计时状态。
2. 每秒更新页面上的 `span`。
3. 倒计时结束后清除定时器。
4. 使用 `location.href` 跳转页面。

### 查询参数和 hash

```js
console.log(location.search)
console.log(location.hash)
```

`search` 常用于页面之间传递参数，`hash` 常用于锚点、单页应用路由。

### ERP 开发场景

- 从订单列表跳转到订单详情：`order-detail.html?id=1001`。
- 通过 URL 查询参数保留筛选条件：`?status=pending&page=2`。
- 登录过期后跳转到登录页。
- 操作完成后跳转回列表页。
- 使用 `replace()` 跳转登录页，避免用户点击后退又回到需要权限的页面。

## 4. Navigator 对象

### 核心概念

`navigator` 用来获取浏览器和运行环境相关信息。

常见属性：

- `navigator.userAgent`：浏览器、系统、设备等信息字符串。
- `navigator.platform`：运行平台信息。

### 关键代码

```js
const userAgent = navigator.userAgent
const android = userAgent.match(/(Android);?[\s/]+([\d.]+)?/)
const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)

if (android || iphone) {
  location.href = 'https://www.baidu.com'
}
```

这段逻辑的作用：

1. 读取浏览器 `userAgent`。
2. 判断是否包含 Android 或 iPhone 信息。
3. 如果是移动设备，就跳转到移动端页面。

### 立即执行函数

```js
;(function () {
  // 初始化逻辑
})()
```

立即执行函数会在定义后马上执行，适合包裹一次性初始化逻辑，减少变量污染。

### ERP 开发场景

- 判断当前是否移动端，跳转到移动审批页面。
- 根据设备类型展示不同布局，例如 PC 端表格、移动端卡片。
- 统计用户使用的浏览器环境，辅助排查兼容性问题。
- 对低版本浏览器提示升级。

> 注意：`userAgent` 可以被伪造，不能作为权限判断依据。ERP 中权限必须由后端校验。

## 5. history 对象

### 核心概念

`history` 表示浏览器当前窗口的历史记录栈，常用于页面前进、后退、刷新当前页面。

常见方法：

```js
history.back()
history.forward()
history.go(1)
history.go(-1)
history.go(0)
```

含义：

- `history.back()`：后退一页。
- `history.forward()`：前进一页。
- `history.go(1)`：前进一步。
- `history.go(-1)`：后退一步。
- `history.go(0)`：刷新当前页面。

### 业务逻辑拆解

以“详情页返回列表”为例：

1. 用户从列表页进入详情页。
2. 点击“返回”按钮。
3. 调用 `history.back()` 返回上一页。

### ERP 开发场景

- 审批详情页点击“返回”回到审批列表。
- 表单保存后，返回上一个页面。
- 多步骤流程中支持回到上一步。

### 注意

如果用户不是从系统内部页面进入详情页，而是直接打开详情链接，`history.back()` 可能无法回到预期列表页。ERP 中更稳妥的方式通常是明确跳转：

```js
location.href = '/orders/list'
```

## 6. 本地存储

### 核心概念

浏览器本地存储用于在浏览器端保存少量数据，常见对象有：

- `localStorage`：长期存储，除非手动删除，否则关闭页面后仍然存在。
- `sessionStorage`：会话存储，页面会话结束后通常会丢失。

二者都是以键值对形式存储，并且值会被保存为字符串。

### localStorage 常用方法

```js
localStorage.setItem('name', '张三')
localStorage.getItem('name')
localStorage.removeItem('name')
localStorage.clear()
```

### sessionStorage 常用方法

```js
sessionStorage.setItem('name', '张三')
sessionStorage.getItem('name')
sessionStorage.removeItem('name')
sessionStorage.clear()
```

### 业务逻辑拆解

以“保存筛选条件”为例：

1. 用户在列表页选择筛选条件。
2. 把筛选条件存入 `localStorage` 或 `sessionStorage`。
3. 页面刷新或重新进入时读取存储。
4. 自动恢复用户上一次的筛选状态。

### ERP 开发场景

- 保存用户偏好的表格列显示、每页条数。
- 暂存搜索条件、筛选条件。
- 暂存未提交的表单草稿。
- 保存主题、语言、侧边栏展开状态。

### 注意

本地存储不适合保存敏感信息，例如密码、身份证号、完整 token、财务敏感数据等。ERP 系统中涉及权限和敏感业务数据，应该优先依赖后端和安全机制。

## 7. 本地存储复杂数据类型

### 核心概念

`localStorage` 只能直接保存字符串。如果要保存对象或数组，需要先使用 `JSON.stringify()` 转成 JSON 字符串；读取时再使用 `JSON.parse()` 转回对象或数组。

### 关键代码

```js
const user = { name: '张三', age: 18 }

localStorage.setItem('user', JSON.stringify(user))

const result = JSON.parse(localStorage.getItem('user'))
console.log(result.name)
```

逻辑说明：

1. JavaScript 对象不能直接作为结构化数据存入 `localStorage`。
2. `JSON.stringify()` 把对象转成字符串。
3. `JSON.parse()` 把字符串还原成对象。

### 业务逻辑拆解

以“保存表单草稿”为例：

1. 收集表单字段，组成对象。
2. 把对象转成 JSON 字符串。
3. 存入 `localStorage`。
4. 用户再次进入页面时读取 JSON 字符串。
5. 解析后回填表单。

### ERP 开发场景

- 复杂查询条件暂存，例如日期范围、状态数组、组织 ID。
- 暂存单据草稿，例如采购申请、报销申请。
- 保存用户表格配置，例如列宽、排序字段、是否冻结列。

### 注意

`JSON.parse()` 解析空值或非法 JSON 会报错。实际项目中读取本地存储时通常要给默认值：

```js
const list = JSON.parse(localStorage.getItem('list')) || []
```

如果数据来源不确定，还可以使用 `try...catch` 做容错。

## 8. map 方法和 join 方法

### map()

`map()` 用于遍历数组，并返回一个新数组。它适合把“数据数组”转换成“展示数组”。

```js
const arr = ['red', 'green', 'blue']

const newArr = arr.map(function (ele, index) {
  return ele + '颜色'
})
```

结果：

```js
['red颜色', 'green颜色', 'blue颜色']
```

### join()

`join()` 用于把数组转换成字符串，可以指定连接符。

```js
newArr.join()
newArr.join('-')
newArr.join('')
newArr.join('|')
```

在页面渲染中，`join('')` 很常用，因为它可以把多个 HTML 字符串拼接成一整段 HTML。

### 业务逻辑拆解

以“表格渲染”为例：

1. 后端或本地拿到一组数组数据。
2. 使用 `map()` 把每条数据转成一行 HTML 字符串。
3. 使用 `join('')` 拼成完整 HTML。
4. 使用 `innerHTML` 插入到页面中。

### ERP 开发场景

- 把订单列表数据渲染成表格行。
- 把菜单权限数组渲染成侧边栏菜单。
- 把审批记录数组渲染成时间线。
- 把商品明细数组渲染成销售单、采购单的明细行。

### 注意

如果数据来自用户输入或后端接口，直接拼接 `innerHTML` 可能带来 XSS 风险。真实 ERP 项目中通常使用框架模板能力，或者对内容进行转义。

## 综合案例：本地存储学生就业统计表

### 案例目标

实现一个学生就业统计表，支持：

- 初始化学生数据。
- 渲染学生列表到表格。
- 表单新增学生。
- 删除指定学生。
- 使用 `localStorage` 持久化数据。

### 数据结构

每个学生对象包含：

```js
{
  stuId: 1,
  userName: '张三',
  userAge: 18,
  userGender: '男',
  userSalary: 10000,
  userCity: '北京',
  createTime: new Date().toLocaleString()
}
```

这个结构类似 ERP 中一条业务单据或一条主数据记录。

### 1. 初始化数据

```js
const tempdata = JSON.parse(localStorage.getItem('data')) || initdata
```

逻辑拆解：

1. 优先从 `localStorage` 读取历史数据。
2. 如果本地没有数据，就使用 `initdata` 作为默认数据。
3. 后续新增、删除都操作 `tempdata`。

ERP 场景：

- 页面初始化时优先读取缓存的筛选条件。
- 如果没有缓存，就使用默认查询条件。
- 离线或临时页面可以先读取本地暂存数据。

### 2. 渲染表格

```js
function render() {
  const trArr = tempdata.map(function (ele, index) {
    return `
      <tr>
        <td>${ele.stuId}</td>
        <td>${ele.userName}</td>
        <td>${ele.userAge}</td>
        <td>${ele.userGender}</td>
        <td>${ele.userSalary}</td>
        <td>${ele.userCity}</td>
        <td>${ele.createTime}</td>
        <td>
          <a href="javascript:" data-id="${index}">删除</a>
        </td>
      </tr>`
  })

  document.querySelector('tbody').innerHTML = trArr.join('')
  document.querySelector('.title span').innerHTML = tempdata.length
}
```

逻辑拆解：

1. `map()` 遍历数据数组。
2. 每条学生数据生成一个 `<tr>`。
3. 删除按钮上通过 `data-id` 保存当前数组索引。
4. `join('')` 把多行 HTML 拼成字符串。
5. `innerHTML` 更新表格内容。
6. 同步更新总数量。

ERP 场景：

- 渲染客户列表、商品列表、订单列表。
- 渲染采购单或销售单明细。
- 渲染审批记录、操作日志、库存流水。

### 3. 新增数据

```js
formInput.addEventListener('submit', function (e) {
  e.preventDefault()

  if (!uname.value.trim() || !uage.value.trim() || !usalary.value.trim()) {
    return alert('输入不能为空')
  }

  tempdata.push({
    stuId: tempdata.length ? tempdata[tempdata.length - 1].stuId + 1 : 1,
    userName: uname.value,
    userAge: uage.value,
    userGender: ugender.value,
    userSalary: usalary.value,
    userCity: ucity.value,
    createTime: new Date().toLocaleString()
  })

  localStorage.setItem('data', JSON.stringify(tempdata))
  render()
  this.reset()
})
```

逻辑拆解：

1. 监听表单 `submit` 事件。
2. 使用 `e.preventDefault()` 阻止表单默认刷新页面。
3. 对必填项做非空校验。
4. 把表单字段组装成一条新学生数据。
5. 使用 `push()` 添加到数组。
6. 使用 `JSON.stringify()` 回写本地存储。
7. 调用 `render()` 刷新页面。
8. 使用 `reset()` 清空表单。

ERP 场景：

- 新增客户、供应商、商品档案。
- 新增订单明细行、采购明细行。
- 新增费用报销明细。
- 临时保存单据草稿。

### 4. 删除数据

```js
tbody.addEventListener('click', function (e) {
  if (e.target.tagName === 'A') {
    if (confirm('你确定要删除这条数据吗？')) {
      tempdata.splice(e.target.dataset.id, 1)
      localStorage.setItem('data', JSON.stringify(tempdata))
      render()
    }
  }
})
```

逻辑拆解：

1. 删除按钮是动态渲染出来的，所以给 `tbody` 绑定点击事件。
2. 通过事件委托判断点击目标是否是删除按钮。
3. 使用 `confirm()` 二次确认。
4. 通过 `dataset.id` 获取要删除的数组索引。
5. 使用 `splice()` 删除数组中的对应项。
6. 删除后重新写入 `localStorage`。
7. 调用 `render()` 重新渲染表格。

ERP 场景：

- 删除订单明细行。
- 删除草稿中的商品行。
- 删除临时上传的附件记录。
- 删除表格中的未提交数据。

### 案例中的完整数据流

```text
页面加载
  -> 读取 localStorage
  -> 没有数据则使用 initdata
  -> render() 渲染表格

用户新增
  -> 表单校验
  -> push 新数据
  -> 写入 localStorage
  -> render() 重新渲染

用户删除
  -> 事件委托捕获点击
  -> confirm 二次确认
  -> splice 删除数组项
  -> 写入 localStorage
  -> render() 重新渲染
```

### 可以继续优化的点

- 删除时建议使用稳定的 `stuId`，而不是数组索引。因为数组索引会随着排序、筛选、删除而变化。
- 表单中的年龄、薪资可以做数字校验。
- 渲染用户输入内容时要注意 XSS 风险。
- 真实 ERP 项目中数据最终应提交到后端数据库，本地存储更适合草稿、偏好设置或离线临时数据。
- `localStorage` 写入失败、解析失败时，可以增加异常处理。

## 本章知识串联

本章内容之间的关系：

```text
BOM
  -> window 顶级对象
  -> location 控制地址和跳转
  -> history 控制前进后退
  -> navigator 获取浏览器环境
  -> setTimeout / setInterval 处理定时任务

JS 执行机制
  -> 理解同步、异步和事件循环
  -> 理解用户事件、定时器为什么不会立即执行

本地存储
  -> localStorage / sessionStorage 保存数据
  -> JSON.stringify / JSON.parse 保存复杂数据

数组方法
  -> map 把数据转换为 HTML
  -> join 把 HTML 数组拼成字符串

综合案例
  -> 本地存储 + 表单事件 + 数组操作 + 页面渲染
```

## ERP 开发中的整体应用

这些知识在 ERP 前端中经常组合使用：

- 使用 `location.search` 获取详情页 ID，再根据 ID 查询订单详情。
- 使用 `localStorage` 保存用户表格偏好、筛选条件和草稿。
- 使用 `map()` 渲染订单、客户、库存、审批列表。
- 使用事件委托处理动态表格中的编辑、删除、查看按钮。
- 使用 `setInterval()` 刷新待办数量、库存预警、消息提醒。
- 使用 `history.back()` 或明确 URL 跳转处理返回列表页。
- 使用 `navigator.userAgent` 做移动端适配，但不做权限判断。

本章重点不是记住每个 API，而是理解“浏览器提供了哪些能力”，以及如何把这些能力组合成实际业务流程。
