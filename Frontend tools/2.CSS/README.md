# CSS 速查手册

> 用途：复习 CSS 时先看这里，遇到细节再跳转到对应章节的详细课堂笔记。

## 学习路线

1. [样式选择器](./1.样式选择器/知识点总结.md)：CSS 引入方式、基础选择器、关系选择器、文字样式、伪类伪元素和权重。
2. [盒子模型](./2.盒子模型/知识点总结.md)：边框、圆角、内外边距、盒子尺寸、背景、渐变、阴影、字体图标和精灵图。
3. [网页布局](./3.网页布局/知识点总结.md)：显示模式、浮动、弹性布局、定位、网格布局、多列布局和鼠标样式。
4. [动画效果](./4.动画效果/知识点总结.md)：2D/3D 变换、过渡、关键帧动画和动效案例。
5. [小兔鲜儿案例](./EX（小兔鲜儿案例）/知识点总结.md)：综合页面结构、公共样式、首页布局和组件复用。

## CSS 基础速查

### 引入方式

| 方式 | 写法 | 使用建议 |
| --- | --- | --- |
| 行内样式 | `style="color:red"` | 临时测试，不适合维护 |
| 内部样式 | `<style>...</style>` | 单文件练习常用 |
| 外部样式 | `<link rel="stylesheet" href="./css/index.css">` | 项目开发推荐 |

### 选择器

| 选择器 | 示例 | 说明 |
| --- | --- | --- |
| 类型选择器 | `p {}` | 选中同名标签 |
| 类选择器 | `.box {}` | 最常用，可复用 |
| ID 选择器 | `#app {}` | 唯一性强，权重高 |
| 通配符选择器 | `* {}` | 常用于初始化 |
| 后代选择器 | `.nav a {}` | 选中所有后代 |
| 子代选择器 | `.nav > li {}` | 只选中直接子元素 |
| 并集选择器 | `h1, h2 {}` | 多个选择器共用样式 |
| 交集选择器 | `div.active {}` | 同时满足多个条件 |

### 权重速记

| 类型 | 权重理解 |
| --- | --- |
| 行内样式 | 最高常规权重 |
| ID 选择器 | 高 |
| 类、伪类、属性选择器 | 中 |
| 标签、伪元素选择器 | 低 |
| 通配符、继承 | 很低 |

遇到样式不生效，优先检查：选择器是否选中、属性是否写错、权重是否被覆盖、后写的样式是否覆盖前面的样式。

## 盒子模型速查

```css
.box {
  width: 200px;
  height: 100px;
  padding: 20px;
  border: 1px solid #ccc;
  margin: 20px;
  box-sizing: border-box;
}
```

| 属性 | 作用 |
| --- | --- |
| `width` / `height` | 内容区域宽高 |
| `padding` | 内边距 |
| `border` | 边框 |
| `margin` | 外边距 |
| `box-sizing` | 控制盒子尺寸计算方式 |

项目中常用：

```css
* {
  box-sizing: border-box;
}
```

## 布局速查

### Flex

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 16px;
}
```

常用属性：

- `flex-direction`：主轴方向。
- `justify-content`：主轴对齐。
- `align-items`：交叉轴单行对齐。
- `align-content`：多行交叉轴对齐。
- `flex-wrap`：是否换行。
- `flex`：子项放大、缩小和基础尺寸。

### Grid

```css
.list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(210px, 1fr));
  gap: 20px;
}
```

Grid 适合二维布局，例如卡片列表、后台面板、图片墙。

### Position

| 值 | 说明 |
| --- | --- |
| `relative` | 相对自身位置偏移，不脱离文档流 |
| `absolute` | 相对最近的定位祖先定位，脱离文档流 |
| `fixed` | 相对浏览器窗口固定 |
| `sticky` | 粘性定位，滚动到阈值后固定 |

常见口诀：子绝父相。子元素 `absolute`，父元素设置 `relative` 作为定位参照。

## 动效速查

### 过渡

```css
.card {
  transition: transform .3s;
}

.card:hover {
  transform: translateY(-8px);
}
```

### 动画

```css
@keyframes move {
  from { transform: translateX(0); }
  to { transform: translateX(100px); }
}

.box {
  animation: move 1s linear infinite;
}
```

### 变换

| 属性 | 作用 |
| --- | --- |
| `translate()` | 位移 |
| `rotate()` | 旋转 |
| `scale()` | 缩放 |
| `skew()` | 斜切 |
| `perspective` | 3D 透视 |

## 工程实践速查

- 类名尽量表达结构和用途，例如 `.header`、`.nav-list`、`.product-card`。
- 公共样式放 `base.css` 或 `common.css`，页面样式放独立文件。
- 优先使用 Flex 和 Grid 做布局，浮动主要用于理解历史布局和文字环绕。
- 不要轻易使用 `!important`，多数情况下应该调整选择器或结构。
- 动画优先改变 `transform` 和 `opacity`，比频繁改变宽高、位置更利于性能。
- 大项目中要注意 CSS 作用域、命名规范和样式覆盖问题。

