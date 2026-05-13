# JavaScript 基础语法 - 1.6 对象

本章主要学习 JavaScript 中对象的创建、属性操作、方法、遍历、内置对象 `Math`、随机数案例，以及引用类型的基本理解。

## 1. 对象的基本使用

对象（object）是一种引用数据类型，也可以理解为一组无序的键值对集合。对象适合用来描述一个具体事物的多个特征。

常见写法：

```js
let obj = {
  uname: '张三',
  age: 18,
  gender: '男'
}
```

也可以使用构造函数创建对象：

```js
let obj = new Object({
  uname: '张三',
  age: 18,
  gender: '男'
})
```

对象中可以嵌套数组、对象，用来表示更复杂的数据结构：

```js
let musician = {
  name: 'Bill Evans',
  age: 65,
  instrument: 'piano',
  style: 'coolJazz',
  albums: [
    {
      name: 'Bill Evans Live At The Village Vanguard',
      year: 1961
    }
  ]
}
```

## 2. 对象属性的查询、修改、新增和删除

查询对象属性有两种常见方式：

```js
obj.uname
obj['uname']
```

当属性名包含特殊符号、空格，或者属性名来自变量时，必须使用中括号写法：

```js
let goods = {
  'name-id': '100012',
  name: '小米10青春版'
}

console.log(goods['name-id'])
```

修改属性就是重新赋值：

```js
goods.name = '小米10Plus'
```

新增属性也是通过赋值完成：

```js
goods.color = 'pink'
```

删除属性使用 `delete`：

```js
delete goods.color
```

## 3. 对象的方法

对象中的属性如果保存的是函数，这个函数就可以叫做对象的方法。

```js
let obj = {
  uname: '张三',
  age: 18,
  song: function (x = 0, y = 0) {
    console.log(x + y)
  }
}

obj.song(1, 2)
```

方法调用格式：

```js
对象名.方法名()
```

轻微拓展：在对象方法中，后续还会经常用到 `this`，它通常指向“调用这个方法的对象”。

## 4. 对象的遍历

对象不能像数组一样直接用普通 `for` 循环按下标遍历，常见方式是使用 `for...in`。

```js
let obj = {
  uname: '张三',
  age: 18,
  gender: '男'
}

for (let key in obj) {
  console.log(key)      // 属性名
  console.log(obj[key]) // 属性值
}
```

注意：`key` 是变量，所以访问属性值时要写 `obj[key]`，不能写成 `obj.key`。`obj.key` 表示访问对象中名字就叫 `key` 的属性。

### 案例：学生信息表格渲染

案例中使用“数组里面放对象”的结构保存多个学生信息：

```js
let students = [
  { name: '小明', age: 18, gender: '男', hometown: '北京' },
  { name: '小红', age: 19, gender: '女', hometown: '上海' }
]
```

外层循环遍历学生数组，内层 `for...in` 遍历每个学生对象的属性，然后使用 `document.write()` 把数据写入表格。

核心思路：

1. 准备数据：数组保存多名学生，每名学生用对象表示。
2. 遍历数组：每次拿到一个学生对象。
3. 遍历对象：取出学生的姓名、年龄、性别、家乡。
4. 拼接表格行：把数据渲染到页面中。

## 5. 内置对象 Math

`Math` 是 JavaScript 提供的内置对象，常用于数学计算。

常见方法：

```js
Math.random() // 生成 [0, 1) 之间的随机小数
Math.ceil()   // 向上取整
Math.floor()  // 向下取整
Math.round()  // 四舍五入
Math.max()    // 求最大值
Math.min()    // 求最小值
Math.abs()    // 求绝对值
Math.pow()    // 求幂
```

示例：

```js
console.log(Math.floor(3.9)) // 3
console.log(Math.ceil(3.1))  // 4
console.log(Math.round(3.5)) // 4
console.log(Math.max(1, 2, 3)) // 3
```

`parseInt()` 不是 `Math` 的方法，它是一个全局函数，常用于把字符串开头能解析成整数的部分转成数字：

```js
parseInt('12.33px') // 12
```

## 6. 随机数

`Math.random()` 返回范围是：

```js
0 <= Math.random() < 1
```

生成 `N` 到 `M` 之间的随机整数，常用公式：

```js
function getRandom(N, M) {
  return Math.floor(Math.random() * (M - N + 1)) + N
}
```

例如：

```js
getRandom(1, 100) // 可能得到 1 到 100 之间的整数，包含 1 和 100
```

### 案例：随机点名

用数组保存名字，然后通过随机下标取出一个元素：

```js
let arr = ['赵云', '关羽', '张飞', '马超']
let random = Math.floor(Math.random() * arr.length)
document.write(arr[random])
```

如果希望抽中过的人不再参与下一轮，可以使用 `splice()` 删除：

```js
arr.splice(random, 1)
```

### 案例：猜数字游戏

核心逻辑：

1. 生成一个 1 到 10 的随机整数。
2. 用户最多猜 3 次。
3. 判断输入是否为有效数字。
4. 根据大小提示“猜大了”或“猜小了”。
5. 猜中后用 `break` 结束循环。
6. 使用开关变量 `flag` 判断最终是否失败。

示例中的重点知识：

```js
let flag = true

if (randomNum === num) {
  flag = false
  break
}
```

`flag` 用来记录游戏状态：如果猜中了，就改成 `false`；循环结束后如果仍然是 `true`，说明次数用完也没有猜中。

### 案例：随机颜色

十六进制颜色由 `#` 加 6 位字符组成，每一位可以从 `0-9` 和 `a-f` 中随机取：

```js
let arr = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f']
```

RGB 颜色可以随机生成 3 个 `0-255` 之间的整数：

```js
rgb(123, 45, 67)
```

这个案例练习了：

1. 函数封装。
2. 随机整数公式。
3. 字符串拼接。
4. 数组存储和数组取值。
5. 根据参数返回不同格式的结果。

## 7. 基本数据类型与引用数据类型

JavaScript 中常见的数据类型可以先粗略分为两类：

基本数据类型：

```js
string
number
boolean
undefined
null
symbol
bigint
```

引用数据类型：

```js
object
array
function
```

基本类型保存的是值本身，引用类型保存的是对对象的引用。

```js
let obj1 = { name: '张三', age: 18 }
let obj2 = obj1

obj2.name = '李四'

console.log(obj1.name) // 李四
console.log(obj2.name) // 李四
```

原因是：`obj1` 和 `obj2` 指向同一个对象，所以通过其中任何一个变量修改对象，另一个变量访问到的也是修改后的结果。

## 勘误与建议

下面是本章代码中需要注意或修正的地方：

1. `1.对象的使用.html` 中 `name: BillEvans` 会被当成变量，如果没有提前定义 `BillEvans`，会报错。应该写成字符串：`name: 'Bill Evans'`。
2. `2.对象的查询.html` 中同一个 `<script>` 里重复使用 `let obj` 声明对象，会导致语法错误。可以改成不同变量名，例如 `goods`。
3. `2.对象的查询.html` 中先执行 `console.log(obj['name-id'])`，但当时的 `obj` 里没有这个属性，所以结果是 `undefined`。如果要查询，应该先定义该属性。
4. `4.对象的遍历.html` 中内层循环里写 `console.log(student[i].name)`，会导致每个学生的姓名被重复输出多次。如果要输出每个属性值，应写 `console.log(student[i][key])`。
5. `4.X(案例)学生信息表格渲染.html` 中 `<h2>` 放在 `<table>` 里面不太规范，标题更适合放在表格外面，或者使用 `<caption>`。
6. `5.内置对象.html` 中 `parseInt()` 不属于 `Math` 对象，它是全局函数。
7. `5.内置对象.html` 中 `Math.ceil(Math.random() * 100)` 不适合作为常规的 `0-100` 随机整数写法。更推荐用 `Math.floor(Math.random() * 101)` 生成 `0-100`。
8. `6.内置对象-随机数.html` 中 `Math.random() * 100 + 1` 的范围是 `[1, 101)`，不是严格的 `1-100` 整数。生成 `1-100` 随机整数建议使用 `Math.floor(Math.random() * 100) + 1`。
9. `6.X2（案例）生成随机颜色.html` 中注释“所有函数都得有 return”不准确。JavaScript 函数可以没有 `return`，没有返回值时默认返回 `undefined`。
10. `7.额外补充.html` 中“堆内存不会自动释放，需要手动释放”对 JavaScript 来说不准确。JavaScript 有垃圾回收机制，不需要手动释放对象内存。
11. 猜数字案例中注释掉的 `1 <= num <= 10` 不是 JavaScript 中判断区间的正确写法。应该写成：`num >= 1 && num <= 10`。

## 本章复习重点

1. 对象是无序键值对集合，适合描述复杂数据。
2. 点语法适合普通属性名，中括号语法适合特殊属性名和变量属性名。
3. 对象方法本质上是保存在对象属性里的函数。
4. 遍历对象常用 `for...in`，取值时使用 `obj[key]`。
5. 数组对象结合是前端渲染列表数据的常见结构。
6. `Math.random()` 的范围是 `[0, 1)`。
7. 生成指定区间随机整数推荐使用 `Math.floor(Math.random() * (M - N + 1)) + N`。
8. 引用类型赋值复制的是引用，不是复制一个全新的对象。
