# 1.5 函数

## 学习目标

本章主要学习函数声明、函数调用、参数、默认参数、返回值、作用域、匿名函数、立即执行函数、逻辑中断和布尔型转换。学习完后，应能把重复逻辑封装成函数，并能通过参数和返回值复用代码。

## 核心知识点

### 函数声明与调用

函数可以把一段代码封装起来，需要时再调用。

```javascript
function getSum(start = 0, end = 0) {
  let sum = 0

  for (let i = start; i <= end; i++) {
    sum += i
  }

  console.log(sum)
}

getSum(1, 100)
```

基本结构：

```javascript
function 函数名(参数1, 参数2) {
  // 函数体
}

函数名(实参1, 实参2)
```

### 参数和默认参数

参数用于把外部数据传入函数。

```javascript
function fn(x = 0, y = 0) {
  console.log(x + y)
}

fn(1) // 1
```

默认参数可以避免参数缺失时出现 `undefined`，从而减少 `NaN` 问题。

### 返回值

`return` 用于把函数内部的结果返回到函数外部。

```javascript
function getMax(a = 0, b = 0) {
  if (a > b) {
    return a
  }

  return b
}

let max = getMax(10, 20)
console.log(max)
```

注意：

- `console.log(fn)` 打印的是函数本身。
- `console.log(fn())` 打印的是函数执行后的返回值。
- `return` 后面的代码不会继续执行。

### 封装数组处理函数

```javascript
function getArrSum(arr = []) {
  let sum = 0

  for (let i = 0; i < arr.length; i++) {
    sum += arr[i]
  }

  return sum
}

console.log(getArrSum([1, 2, 3, 4, 5]))
```

函数封装的重点是：把变化的数据作为参数传入，把计算结果通过 `return` 返回。

### 函数特征

- 函数名相同时，后面的声明会覆盖前面的声明。
- 实参多于形参时，多出来的实参会被忽略。
- 实参少于形参时，没接收到值的形参是 `undefined`。
- 没有声明变量直接赋值，会变成全局变量，不推荐。

### 作用域

```javascript
let num = 10

function fn() {
  let num = 20
  console.log(num) // 20
}

fn()
console.log(num) // 10
```

作用域访问遵循“就近原则”：先找当前作用域，找不到再向外层作用域查找。

### 匿名函数

匿名函数没有函数名，常见写法是赋值给变量。

```javascript
let fn = function () {
  console.log('fn')
}

fn()
```

函数声明可以先调用后声明，但函数表达式通常不能在声明前调用。

### 立即执行函数

立即执行函数声明后会立刻执行。

```javascript
(function () {
  console.log('立即执行')
})()

(function (a, b) {
  console.log(a + b)
})(1, 2)
```

### 逻辑中断

```javascript
console.log(1 && 2) // 2
console.log(1 || 2) // 1
```

- `&&`：如果前面为假，直接返回前面的值；如果都为真，返回最后一个值。
- `||`：如果前面为真，直接返回前面的值；如果前面为假，继续看后面。

常用于设置默认值：

```javascript
function fn(x, y) {
  x = x || 0
  y = y || 0
  console.log(x + y)
}
```

### 布尔型转换

```javascript
Boolean(1) // true
Boolean(0) // false
Boolean('') // false
Boolean('pink') // true
Boolean(null) // false
Boolean(undefined) // false
Boolean(NaN) // false
Boolean(Infinity) // true
```

常见假值：`false`、`0`、`''`、`null`、`undefined`、`NaN`。

## 常见案例

### 求任意区间的和

```javascript
function getSum(start = 0, end = 0) {
  let sum = 0

  for (let i = start; i <= end; i++) {
    sum += i
  }

  return sum
}

console.log(getSum(1, 100)) // 5050
```

### 求任意数组的最大值和最小值

```javascript
function getArrMax(arr = []) {
  let max = arr[0]
  let min = arr[0]

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i]
    }

    if (arr[i] < min) {
      min = arr[i]
    }
  }

  return [max, min]
}

let result = getArrMax([3, 1, 7, 10, 28])
console.log(`最大值是${result[0]}，最小值是${result[1]}`)
```

### 秒数转时分秒

```javascript
function getTime(seconds) {
  let rest = seconds % 3600
  let hours = parseInt(seconds / 3600)
  let mins = parseInt(rest / 60)
  let secs = parseInt(rest % 60)

  hours = hours < 10 ? '0' + hours : hours
  mins = mins < 10 ? '0' + mins : mins
  secs = secs < 10 ? '0' + secs : secs

  return `${seconds}秒转换为${hours}小时${mins}分钟${secs}秒`
}

console.log(getTime(3661))
```

### 余额计算函数

```javascript
function getBalance(balance, foodCost, lifeCost) {
  return balance - foodCost - lifeCost
}

let balance = +prompt('请输入银行卡余额')
let foodCost = +prompt('请输入当月食宿消费金额')
let lifeCost = +prompt('请输入当月生活消费金额')

document.write(`银行卡剩余金额：${getBalance(balance, foodCost, lifeCost)}元`)
```

### 数组求和或平均值

```javascript
function handleData(arr = [], isSum = true) {
  let sum = 0

  for (let i = 0; i < arr.length; i++) {
    sum += arr[i]
  }

  if (isSum) {
    return sum
  }

  return sum / arr.length
}

console.log(handleData([1, 2, 3])) // 6
console.log(handleData([1, 2, 3], false)) // 2
```

### 拓展案例：some 函数

需求：判断数组中是否存在某个元素。

```javascript
function some(ele, arr = []) {
  for (let i = 0; i < arr.length; i++) {
    if (ele === arr[i]) {
      return true
    }
  }

  return false
}

console.log(some('荔枝', ['苹果', '香蕉', '橘子', '荔枝', '梨子'])) // true
console.log(some('榴莲', ['苹果', '香蕉', '橘子', '荔枝', '梨子'])) // false
```

### 拓展案例：findIndex 函数

需求：返回元素在数组中的索引，找不到返回 `-1`。

```javascript
function findIndex(ele, arr = []) {
  for (let i = 0; i < arr.length; i++) {
    if (ele === arr[i]) {
      return i
    }
  }

  return -1
}

console.log(findIndex(10, [1, 5, 10, 22, 8, 7])) // 2
console.log(findIndex(8, [1, 5, 10, 22, 8, 7])) // 4
console.log(findIndex(88, [1, 5, 10, 22, 8, 7])) // -1
```

## 排错总结

### 参数缺失导致 NaN

```javascript
function fn(x, y = 0) {
  console.log(x + y)
}

fn(1) // 1
```

原因：原本 `y` 没有传值时是 `undefined`，`1 + undefined` 得到 `NaN`。

### 数组求和常见错误

```javascript
function getSumArr(arr = []) {
  let sum = 0

  for (let i = 0; i < arr.length; i++) {
    sum += arr[i]
  }

  return sum
}

console.log(getSumArr([10, 20, 30, 40])) // 100
```

注意：

- `length` 不要拼写成 `legnth`。
- 累加要写 `sum += arr[i]`，不能只写 `sum + arr[i]`。

## 易错点

- 函数声明后不会自动执行，必须调用。
- 参数默认值可以减少 `undefined` 参与计算的问题。
- 想拿到函数结果时，要调用函数并接收 `return`。
- 函数内部用 `let` 声明的变量只能在函数内部访问。
- 不写 `let` 直接赋值会污染全局作用域。
- 函数表达式不能在声明前调用。
- 使用逻辑中断设置默认值时，`0` 也会被当成假值。

## 复习清单

- 能声明并调用函数。
- 能理解形参、实参和默认参数。
- 能用 `return` 返回函数执行结果。
- 能封装求和、取最大值、数组求和等函数。
- 能说明全局作用域和局部作用域。
- 能使用匿名函数和立即执行函数。
- 能理解逻辑中断和布尔型转换。
- 能完成 `some`、`findIndex` 这类数组工具函数。
