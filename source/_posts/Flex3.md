---
title: CSS 原理 - Flex(下)
categories: web
tags: CSS
toc: true
date: 2020-05-11 10:53:54
thumbnail: ./images/cover-css.png
---

vertical-align、text-align 甚至 float 都是控制不了 flex items 的，flex 有自己專屬的對齊屬性，而因對齊分為水平對齊與垂直對齊，所以首先要先認清楚誰是主軸(main axis)誰是副軸(cross axis)，才不會精神分裂。
<!-- more -->

<br>

先稍微複習一下，主軸(main axis)與副軸(cross axis)的方向取決於 flex direction 屬性與書寫模式 writing-mode，可參考先前文章 [CSS 原理 - Flex(上)](https://yachen168.github.io/article/Flex.html#more)。

![](https://i.imgur.com/3ow0Yie.png)

![](https://i.imgur.com/EkZ9GhU.png)

<br>

## flex 的對齊
要小心的是，有些屬性是適用於 flex container，有些屬性則適用於 flex items，用錯地方是沒反應的唷。

**適用於 flex container 的對齊屬性**

  * justify-content
  * align-items
  * align-content

**適用於 flex items 的對齊屬性**

  * align-self


<br>

### justify-content 屬性
控制 `flex items` 在`主軸(main axis)`方向的對齊方式，僅`適用於 flex container`。以下為幾個常用且瀏覽器支援度較高的屬性值：


#### • flex-start
  為預設值，flex items 由`主軸`的`始端`開始排列。
<br>

#### • flex-end
  flex items 由`主軸`的`末端`開始排列。
<br>

#### • center
  flex items `置中`於`主軸`。
<br>

#### • space-between
  `第一個` flex item 對齊`主軸`的`始端`，`最後一個` flex item 對齊`主軸`的`末端`，其餘空間`平均`分佈於 flex items 之間。
<br>

#### • space-around
  以 flex-direction: row 來看，每個 flex item 左右像自備 x 空間，第一個與最後一個 flex item 與 container 的之間有 `x` 空間，而 flex items 兩兩之間有 `2x` 空間。
<br>

#### • pace-evenly
  以 flex-direction: row 來看，每個 flex item 左右各有 `x` 空間，第一個與最後一個 flex item 與 container 的之間的空間亦為 `x`。

<br>

#### 圖形輔助
用圖形非常好理解，在水平且由左至右的書寫模式下，若`主軸`為 `row` (flex-direction：row)，使用此六種屬性值會呈現下圖中的結果。

![](https://i.imgur.com/7p2W0W4.png)
[圖片來源：CSS TRICKS](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

<br>

### align-items 屬性
控制 `flex items` 在`副軸(cross axis)`方向上的對齊方式，僅`適用於 flex container`。以下為幾個常用且瀏覽器支援度較高的值。


#### • flex-start
  flex items 由`副軸`的`始端`開始排列。
<br>

#### • flex-end
  flex items 由`副軸`的`末端`開始排列。
<br>

#### • center
  flex items `置中`於`副軸`。
<br>

#### • stretch
  為`預設值`，這也是為什麼 flex item 在`預設`下會`撐滿`容器在`副軸`上的空間。
<br>

#### • baseline
 flex items 依照 `baseline` 對齊。

<br>

#### 圖形輔助
在水平且由左至右的書寫模式下，若`副軸`為 `column` (flex-direction：row)，使用此五種屬性值會呈現下圖中的結果。

![](https://i.imgur.com/sx5hce7.png)
[圖片來源：CSS TRICKS](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)


<br>

### align-content 屬性
控制「`多行(multi-line)主軸`」在`副軸`上的對齊方式，也就是前提為 flex container 的 `flex-wrap` 屬性值須為 `wrap` 或 `wrap-reverse`，而非預設的 flex-wrap: nowrap。僅適用於 flex container。

以下圖形範例以主軸(main axis)為 row(橫向)，副軸(cross axis)為 column(縱向)來說，即 flex-direction: row 下：

![](https://i.imgur.com/3ow0Yie.png)

<br>

#### • flex-start
「多行(multi-line)主軸」對齊`副軸`的`始端`。
![](https://i.imgur.com/srlWDT2.png)

<br>

#### • flex-end
「多行(multi-line)主軸」對齊`副軸`的`末端`。

![](https://i.imgur.com/sFvkK2O.png)

<br>

#### • center
「多行(multi-line)主軸」`置中`於`副軸`。

![](https://i.imgur.com/4zVdwuE.png)

<br>

#### • space-between
`第一個`與`最後一個` line 對齊`副軸`的`始端`與`末端`，其餘空間`平均`分佈於 flex items 之間。

![](https://i.imgur.com/scRqyPx.png)

<br>

#### • stretch
`延伸撐滿副軸`。

![](https://i.imgur.com/i3vPqQ9.png)

<br>

#### • space-around
每個 line 兩旁像自備 x 空間，第一個與最後一個 line 與 container 的之間有 `x` 空間，而 lines 兩兩之間有 `2x` 空間。

![](https://i.imgur.com/k5GAD3M.png)

<br>

#### • space-evenly
每個 line 左右各有 `x` 空間，第一個與最後一個 line 與 container 的之間的空間亦為 `x`。

![](https://i.imgur.com/GRJBloJ.png)


<br>
<br>


### align-self 屬性
也可以單獨控制`個別` `flex item` 在副軸上的對齊方式。僅適用於 flex items。

以主軸(main axis)為 row(橫向)，副軸(cross axis)為 column(縱向)來說，即 flex-direction: row 下：

![](https://i.imgur.com/3ow0Yie.png)
 
<br>

#### • flex-start
flex item 由`副軸`的`始端`開始排列。
![](https://i.imgur.com/QCt1SRV.png)

<br>

#### • flex-end
flex item 由`副軸`的`末端`開始排列。
![](https://i.imgur.com/0u8vWak.png)
<br>

#### • center
flex item `置中`於`副軸`。
![](https://i.imgur.com/JGqJAn0.png)

<br>

#### • stretch
flex item 撐滿`副軸`。
![](https://i.imgur.com/S5WlRPy.png)

<br>

#### • baseline
flex item 對齊 `baseline`。


<br>
<br>
<br>
<br>

### 總整理
* **適用於 flex container**

  * justify-content
      控制`主軸`上所有 flex items 的對齊。
  * align-items
  控制`副軸`上所有 flex items 的對齊。
  * align-content
  控制「多行(multi-line)主軸」在`副軸`上的對齊方式。
<br>

* **適用於 flex items**

  * align-self
  控制`副軸`上個別 flex item 的對齊。



<br>
<br>
<br>
<br>
<br>


<b>參考資料</b>
1. [W3C - Flexible Box Layout Module Level 1](https://www.w3.org/TR/css-flexbox-1/#justify-content-property)
2. [MDN - Aligning Items in a Flex Container](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Aligning_Items_in_a_Flex_Container)
3. [CSS TRICKS - A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

