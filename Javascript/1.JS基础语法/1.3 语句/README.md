# 1.3 语句

## 学习目标

本章主要学习分支语句、三元运算符、`switch`、`while` 循环、`for` 循环、`continue`、`break` 和循环嵌套。学习完后，应能根据条件执行不同代码，并能重复执行一段逻辑完成累加、遍历、菜单和乘法表等案例。

## 核心知识点

### if 单分支语句

```javascript
if (条件) {
  // 条件为 true 时执行
}
```

`if` 的条件会自动转换成布尔值。常见会被转换成 `false` 的值有：

- `false`
- `0`
- `''`
- `null`
- `undefined`
- `NaN`

### if 双分支语句

```javascript
if (条件) {
  // 条件成立
} else {
  // 条件不成立
}
```

适合只有两种结果的判断，例如登录成功/失败、闰年/平年。

### if 多分支语句

```javascript
if (score >= 90) {
  alert('优秀')
} else if (score >= 80) {
  alert('良好')
} else if (score >= 60) {
  alert('及格')
} else {
  alert('不及格')
}
```

适合多个区间判断，比如成绩等级。

### 三元运算符

三元运算符适合简单的二选一逻辑。

```javascript
条件 ? 条件成立时的结果 : 条件不成立时的结果
```

示例：

```javascript
let num = +prompt('请输入一个数字')
num = num < 10 ? '0' + num : num
alert(num)
```

### switch 分支

`switch` 适合判断一个变量是否等于多个固定值。

```javascript
switch (operator) {
  case '+':
    alert(num1 + num2)
    break
  case '-':
    alert(num1 - num2)
    break
  default:
    alert('未知操作')
}
```

注意：`case` 比较时要求值和类型都匹配，类似 `===`。

### while 循环

循环三要素：

1. 初始化变量。
2. 终止条件。
3. 更新变量。

```javascript
let i = 1

while (i <= 10) {
  document.write(i + '<br>')
  i++
}
```

`while` 更适合循环次数不明确的场景，例如一直询问用户直到输入正确。

### for 循环

```javascript
for (let i = 1; i <= 10; i++) {
  document.write(i + '<br>')
}
```

`for` 更适合循环次数明确的场景，例如打印 1 到 100、遍历数组。

### continue 和 break

```javascript
continue // 跳过本次循环，继续下一次循环
break // 直接退出整个循环
```

示例：

```javascript
for (let i = 0; i < 10; i++) {
  if (i === 5) {
    continue
  }
  document.write(i + '<br>')
}
```

### 循环嵌套

循环嵌套就是循环里面再写循环，常用于打印矩形、三角形、九九乘法表等。

```javascript
for (let row = 1; row <= 9; row++) {
  for (let col = 1; col <= row; col++) {
    document.write(`${col}*${row}=${row * col} `)
  }
  document.write('<br>')
}
```

## 常见案例

### 登录判断

```javascript
let userId = prompt('请输入用户名')
let userPassword = prompt('请输入密码')

if (userId === 'pink' && userPassword === '123456') {
  alert('恭喜你登录成功')
} else {
  alert('用户名或密码错误')
}
```

### 闰年判断

```javascript
let year = +prompt('请输入年份')

if (year % 4 === 0 && year % 100 !== 0 || year % 400 === 0) {
  alert(`${year} 是闰年`)
} else {
  alert(`${year} 不是闰年`)
}
```

### 求 1 到 100 的和

```javascript
let sum = 0

for (let i = 1; i <= 100; i++) {
  sum += i
}

document.write(sum)
```

### 求 1 到 100 的偶数和

```javascript
let sum = 0

for (let i = 1; i <= 100; i++) {
  if (i % 2 === 0) {
    sum += i
  }
}

document.write(sum)
```

### 银行菜单案例

银行案例综合使用了 `while`、`switch`、变量更新和分支判断。

```javascript
let money = 1000
let action

while (action !== 4) {
  action = +prompt('请输入操作：1.取钱 2.存钱 3.查询余额 4.退出')

  switch (action) {
    case 1:
      let withdraw = +prompt('请输入取款金额')
      if (withdraw > money) {
        alert('余额不足')
      } else {
        money -= withdraw
        alert(`取款成功，余额是${money}`)
      }
      break
    case 2:
      let deposit = +prompt('请输入存款金额')
      money += deposit
      alert(`存款成功，余额是${money}`)
      break
    case 3:
      alert(`余额是${money}`)
      break
    case 4:
      alert('谢谢使用')
      break
    default:
      alert('请输入正确编号')
  }
}
```

### 九九乘法表

```javascript
for (let row = 1; row <= 9; row++) {
  for (let col = 1; col <= row; col++) {
    document.write(`<span>${col}*${row}=${row * col}</span>`)
  }
  document.write('<br>')
}
```

## 易错点

- `if` 条件不是只能写布尔值，其他值会自动转成布尔值。
- `switch` 的每个 `case` 通常要写 `break`，否则会继续向下执行。
- `prompt()` 返回字符串，做数字比较或计算前建议用 `+prompt()`。
- `while` 循环如果忘记更新变量，容易变成死循环。
- `continue` 是跳过本次循环，`break` 是结束整个循环。
- 明确循环次数时优先用 `for`，不明确次数时可以用 `while`。

## 复习清单

- 能写出单分支、双分支、多分支 `if`。
- 能用三元运算符完成简单二选一。
- 能用 `switch` 写简单计算器。
- 能说明 `while` 循环三要素。
- 能使用 `for` 完成累加、遍历数组。
- 能区分 `continue` 和 `break`。
- 能写出九九乘法表。
