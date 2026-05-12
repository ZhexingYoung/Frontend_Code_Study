# 1.1 基础语法数据

## 学习目标

本章主要学习 JavaScript 的基础书写方式、输入输出、变量、常量、数组、基本数据类型以及类型转换。学习完后，应能完成简单的数据接收、存储、计算和页面输出。

## 核心知识点

### JavaScript 的书写位置

JavaScript 常见有三种写法：

```html
<!-- 内部 JS -->
<script>
  alert('Hello World')
</script>

<!-- 外部 JS -->
<script src="./xxx.js"></script>

<!-- 行内 JS -->
<button onclick="alert('Hello World')">点击我</button>
```

学习时重点掌握内部 JS 和外部 JS。外部 JS 更适合项目维护，内部 JS 更适合当前阶段快速练习。

### 注释和结束符

```javascript
// 单行注释

/*
  多行注释
*/

alert('Hello World')
```

JavaScript 语句末尾可以写分号，也可以不写，但建议一个项目里保持统一风格。

### 输入和输出

```javascript
document.write('写入页面')
alert('弹窗输出')
console.log('控制台输出')
prompt('请输入内容')
```

- `document.write()`：把内容写到页面中。
- `alert()`：弹出提示框。
- `console.log()`：输出到浏览器控制台，适合调试。
- `prompt()`：接收用户输入，返回值默认是字符串。

### 变量

变量是临时存储数据的容器。

```javascript
let age
age = 18

let userName = '张三'
console.log(age, userName)
```

变量命名规则：

- 不能使用关键字作为变量名。
- 可以由字母、数字、下划线、`$` 组成，但不能以数字开头。
- 严格区分大小写，`pink` 和 `PINK` 是两个变量。
- 建议使用小驼峰命名法，例如 `userName`。

### 常量

常量使用 `const` 声明，声明时必须赋值，之后不能重新赋值。

```javascript
const PI = 3.14
```

适合保存不会变化的数据，比如圆周率、固定配置等。

### 数组基础

数组可以一次保存多个数据，索引从 `0` 开始。

```javascript
let arr = ['张三', '李四', '王五']

console.log(arr[0]) // 张三
console.log(arr.length) // 3

arr[0] = '猪八戒'
```

### 数据类型

常见基础数据类型：

- `number`：数字，例如 `10`、`3.14`。
- `string`：字符串，例如 `'hello'`。
- `boolean`：布尔值，只有 `true` 和 `false`。
- `undefined`：声明了变量但没有赋值。
- `null`：主动表示空值。

常见引用数据类型：

- `object`：对象、数组等复杂数据。

检测数据类型：

```javascript
console.log(typeof 18) // number
console.log(typeof 'pink') // string
console.log(typeof true) // boolean
console.log(typeof undefined) // undefined
console.log(typeof null) // object
```

### 字符串和模板字符串

字符串可以使用单引号、双引号或反引号。

```javascript
let name = '张三'
let age = 18

console.log('我叫' + name + '，今年' + age + '岁')
console.log(`我叫${name}，今年${age}岁`)
```

模板字符串使用反引号，可以通过 `${}` 插入变量，更适合拼接复杂内容。

### 类型转换

显式转换：

```javascript
Number('10') // 10
parseInt('12.33px') // 12
parseFloat('12.33px') // 12.33
String(10) // '10'
```

隐式转换：

```javascript
'pink' + 1 // 'pink1'
+'123' // 123
'10' - 1 // 9
```

`prompt()` 得到的是字符串，如果要做数学运算，通常需要先转成数字。

```javascript
let num1 = +prompt('请输入第一个数')
let num2 = +prompt('请输入第二个数')
document.write(num1 + num2)
```

## 常见案例

### 交换两个变量的值

```javascript
let num1 = 10
let num2 = 20

let temp = num1
num1 = num2
num2 = temp

console.log(num1, num2) // 20 10
```

### 计算圆的面积

```javascript
let radius = +prompt('请输入半径')
let area = radius * radius * 3.14
document.write(`圆的面积是：${area}`)
```

### 商品订单表格

综合案例中用到了输入、数字转换、变量、模板字符串和 `document.write()`：

```javascript
let price = +prompt('请输入商品价格')
let num = +prompt('请输入商品数量')
let address = prompt('请输入收货地址')
let total = price * num

document.write(`商品总价：${total}元，收货地址：${address}`)
```

## 易错点

- `prompt()` 返回字符串，直接相加可能会变成字符串拼接。
- 数组索引从 `0` 开始，不是从 `1` 开始。
- `const` 声明常量时必须立即赋值，且不能重新赋值。
- `NaN` 表示“不是一个数字”，并且 `NaN` 和任何值比较都不相等，包括它自己。
- `typeof null` 的结果是 `object`，这是 JavaScript 的历史遗留问题。

## 复习清单

- 能说出 JavaScript 的三种书写位置。
- 能使用 `alert`、`console.log`、`document.write`、`prompt`。
- 能用 `let` 声明变量，并理解变量可以更新。
- 能用 `const` 声明常量，并知道常量不能重新赋值。
- 能理解数组索引和数组长度。
- 能区分 `number`、`string`、`boolean`、`undefined`、`null`。
- 能完成字符串拼接和模板字符串输出。
- 能使用 `Number()`、`parseInt()`、`parseFloat()` 做类型转换。
