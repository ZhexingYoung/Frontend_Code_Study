# 1.4 数组

## 学习目标

本章主要学习数组的声明、取值、遍历、求和、取极值、增删改查、数组筛选、柱状图渲染和排序。学习完后，应能使用数组保存一组数据，并通过循环对数组进行处理。

## 核心知识点

### 数组声明

```javascript
let arr = [1, 2, 3, 4, 5]
let arr2 = new Array(1, 2, 3, 4, 5)
```

当前阶段建议优先使用数组字面量 `[]`，写法更简单。

### 数组取值

数组通过索引取值，索引从 `0` 开始。

```javascript
let nums = [10, 20, 30]

console.log(nums[0]) // 10
console.log(nums[1]) // 20
console.log(nums.length) // 3
```

### 数组遍历

```javascript
let nums = [0, 1, 2, 3, 4, 5]

for (let i = 0; i < nums.length; i++) {
  console.log(nums[i])
}
```

遍历数组时，循环条件通常写成 `i < arr.length`。

### 数组求和与平均值

```javascript
let arr = [2, 6, 1, 7, 4]
let sum = 0

for (let i = 0; i < arr.length; i++) {
  sum += arr[i]
}

console.log(sum)
console.log(sum / arr.length)
```

### 数组取最大值和最小值

```javascript
let arr = [2, 6, 1, 77, 52, 25, 7]
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

console.log(max, min)
```

取极值时，推荐先把 `max` 或 `min` 初始化为数组的第一个元素。

### 修改数组元素

```javascript
let arr = ['pink', 'red', 'green']

arr[0] = 'pink老师'

for (let i = 0; i < arr.length; i++) {
  arr[i] = arr[i] + '老师'
}
```

### 添加数组元素

```javascript
let arr = ['pink', 'red', 'green']

arr.push('blue') // 末尾添加，返回新长度
arr.unshift('orange') // 开头添加，返回新长度
```

### 删除数组元素

```javascript
let arr = [2, 0, 6, 1, 77]

arr.pop() // 删除最后一个元素，返回被删除元素
arr.shift() // 删除第一个元素，返回被删除元素
arr.splice(2, 1) // 从索引 2 开始，删除 1 个元素
```

`splice(起始索引, 删除个数)` 可以删除指定位置的数据。

### 数组排序

冒泡排序的核心思想是：相邻两个元素两两比较，如果顺序不对就交换。

```javascript
let arr = [2, 5, 3, 4, 1]

for (let i = 0; i < arr.length - 1; i++) {
  for (let j = 0; j < arr.length - 1 - i; j++) {
    if (arr[j] > arr[j + 1]) {
      let temp = arr[j]
      arr[j] = arr[j + 1]
      arr[j + 1] = temp
    }
  }
}

console.log(arr)
```

开发中也常用 `sort()`：

```javascript
let arr = [2, 3, 7, 2, 4, 1]

arr.sort(function (a, b) {
  return a - b // 升序
})

arr.sort(function (a, b) {
  return b - a // 降序
})
```

## 常见案例

### 筛选数组中大于等于 10 的数据

```javascript
let arr = [2, 0, 6, 1, 77, 0, 52, 0, 25, 7]
let result = []

for (let i = 0; i < arr.length; i++) {
  if (arr[i] >= 10) {
    result.push(arr[i])
  }
}

console.log(result) // [77, 52, 25]
```

这个案例练习了数组遍历、条件判断和 `push()` 添加元素。

### 根据输入渲染柱状图

柱状图案例综合使用了数组、循环、`prompt()`、`document.write()` 和模板字符串。

```javascript
let arr = []

for (let i = 0; i < 4; i++) {
  arr.push(+prompt(`请输入第${i + 1}季度的销售额`))
}

document.write('<div class="box"><ul>')

for (let i = 0; i < arr.length; i++) {
  document.write(`
    <li style="height: ${arr[i]}px;">
      <span>${arr[i]}</span>
      <p>第${i + 1}季度</p>
    </li>
  `)
}

document.write('</ul></div>')
```

## 易错点

- 数组索引从 `0` 开始，最后一个元素的索引是 `arr.length - 1`。
- 遍历数组时不要写成 `i <= arr.length`，否则会访问到 `undefined`。
- `push()` 和 `unshift()` 返回的是新数组长度，不是新数组。
- `pop()`、`shift()`、`splice()` 会修改原数组。
- 取最大值或最小值时，不建议直接把初始值写成 `0`，如果数组都是负数会出错。
- `sort()` 默认按字符串规则排序，数字排序需要传比较函数。

## 复习清单

- 能声明数组并通过索引取值。
- 能使用 `for` 遍历数组。
- 能完成数组求和、平均值、最大值、最小值。
- 能使用索引修改数组元素。
- 能使用 `push()`、`unshift()` 添加元素。
- 能使用 `pop()`、`shift()`、`splice()` 删除元素。
- 能用循环筛选数组。
- 能理解冒泡排序的双层循环。
