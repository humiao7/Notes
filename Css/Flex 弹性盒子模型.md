# Flex 弹性盒子模型

## 一、简介

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为 Flex 布局：

```css
.box {
    display: flex;
}
```

Webkit 内核的浏览器，必须加上 ```-webkit``` 前缀：

```css
.box {
    display: -webkit-flex; /* Safari */
    display: flex;
}
```

> 注意：设为 Flex 布局以后，子元素的 **float、clear** 和 **vertical-align** 属性将失效。

## 二、flex 基本概念

采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称 "**项目**"

![](./assets/flex-layout.png)

容器默认存在两根轴：水平的**主轴（main axis）**和垂直的**交叉轴（cross axis）**。主轴的开始位置（与边框的交叉点）叫做 main start，结束位置叫做 main end；交叉轴的开始位置叫做 cross start，结束位置叫做 cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做 main size，占据的交叉轴空间叫做 cross size。

## 三、容器的属性

容器有以下 6 个主要属性：

* ```flex-direction```

* ```flex-wrap```

* ```flex-flow```

* ```justify-content```

* ```align-items```

* ```align-content```

### 1、flex-direction 属性

flex-direction 属性决定主轴的方向（容器中项目的排列方向）。

```css
.box {
    flex-direction: row | row-reverse | column | column-reverse;
}
```

它可能取 4 个值：

|         值         |            描述            |                            效果                            |
| :----------------: | :------------------------: | :--------------------------------------------------------: |
|      **row**       | 主轴为水平方向，起点在左端 |      <img src="/assets/row.png" style="zoom:67%;" />       |
|  **row-reverse**   | 主轴为水平方向，起点在右端 |  <img src="/assets/row-reverse.png" style="zoom:67%;" />   |
|     **column**     | 主轴为垂直方向，起点在上沿 |     <img src="/assets/column.png" style="zoom:67%;" />     |
| **column-reverse** | 主轴为垂直方向，起点在下沿 | <img src="/assets/column-reverse.png" style="zoom:67%;" /> |

### 2、flex-wrap 属性

默认情况下，项目都排在一条线（又称"轴线"）上，flex-wrap 属性定义，如果一条轴线排不下，如何换行。

![](/assets/flex-wrap.png)

```css
.box {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

它可能取 3 个值：

**nowrap**（默认），不换行：

![](./assets/nowrap.png)

**wrap**，换行，第一行在上方：

![](./assets/wrap.jpg)

**wrap-reverse**，换行，第一行在下方：

![](./assets/wrap-reverse.jpg)

### 3、flex-flow 属性

flex-flow 属性是 **flex-direction** 属性和 **flex-wrap** 属性的简写形式，默认值为 ```row nowrap```

```css
.box {
    flex-flow: <flex-direction> || <flex-wrap>;
}
```

### 4、justify-content 属性

justify-content 属性定义了项目在主轴上的对齐方式:

```css
.box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

它可能取 5 个值，具体对齐方式与轴的方向有关。下面**假设主轴为从左到右**：

|            值            |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
| **flex-start**（默认值） |                            左对齐                            |
|       **flex-end**       |                            右对齐                            |
|        **center**        |                             居中                             |
|    **space-between**     |                两端对齐，项目之间的间隔都相等                |
|     **space-around**     | 每个项目两侧的间隔相等，项目之间的间隔比项目与边框的间隔大一倍 |

![](./assets/justify-content.png)

### 5、align-items 属性

align-items 属性定义主轴未出现换行时，项目在交叉轴上的对齐方式

```css
.box {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下

|         属性          |                        描述                        |
| :-------------------: | :------------------------------------------------: |
|    **flex-start**     |                  交叉轴的起点对齐                  |
|     **flex-end**      |                  交叉轴的终点对齐                  |
|      **center**       |                  交叉轴的中点对齐                  |
|     **baseline**      |             项目的第一行文字的基线对齐             |
| **stretch**（默认值） | 如果项目未设置高度或设为auto，将占满整个容器的高度 |

![](./assets/align-items.png)

### 6、align-content 属性

align-content 属性定义了主轴出现换行时，行与行之间的对齐方式。如果项目只有一行，则该属性不起作用

```css
.box {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

该属性可能取6个值

|         属性          |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
|    **flex-start**     |                      与交叉轴的起点对齐                      |
|     **flex-end**      |                      与交叉轴的终点对齐                      |
|      **center**       |                      与交叉轴的中点对齐                      |
|   **space-between**   |           与交叉轴两端对齐，轴线之间的间隔平均分布           |
|   **space-around**    | 每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍 |
| **stretch**（默认值） |                      轴线占满整个交叉轴                      |

![](./assets/align-content.png)

## 三、项目属性

项目有以下 5 个属性：

- ```flex-grow```
- ```flex-shrink```
- ```flex-basis```
- ```flex```
- ```order```

### 1、flex-grow 属性

flex-grow 属性决定项目的拉伸因子，当所处行存在空白的时候会根据该行的项目的 flex-grow 值分配多余的空间。默认值为0，即不会自动拉伸。

### 2、flex-shrink 属性

flex-shrink 属性决定项目的收缩因子，当所处行空间不足时候会根据该行的项目的 flex-shrink 按比例进行收缩。默认值为1，即等比例收缩。

### 3、flex-basis 属性

flex-basis 属性指定了 flex 项目在主轴方向上的初始大小。如果不设置一般会读取 width;

### 4、flex 属性

flex 属性是 flex-grow、flex-shrink、flex-basis 3 个属性的简写形式

```css
.item {
    flex: <flex-grow> || <flex-shrink> || <flex-basis>;
}
```

> 注：```flex : 1 ``` == ```flex: 1 1 auto``` == ```flex-grow:1; flex-shrink:1; flex-basis:auto;```

### 5、order 属性

order 属性可以更改元素出现的顺序，即会优先根据设置的order来排序。默认为0;

