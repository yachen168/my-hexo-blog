---
title: CSS 原理 - Flex(上)
categories: web
tags: CSS
toc: true
date: 2020-05-08 13:32:37
thumbnail: https://i.imgur.com/qmupIlt.png
---

接下來將介紹 flex，從先前 [formatting context](https://yachen168.github.io/article/Formatting-context.html) 的觀念開始，再詳細介紹每個屬性的用法以及 flex box 伸縮的計算方式。

<!-- more -->

<br>

## display: flex | inline-flex
flex container 顧名思義就是一個容器(container)，描述 flex 元素與其後裔元素之間的關係，當對一個元素設定 `display: flex` 時，`此元素`稱為 `flex container`，而其`直接子元素`則稱為 `flex item`。

![](https://i.imgur.com/nWyrjIY.png)
<br></br>

`display: flex` 的元素會生成 `block-level box` 與 `flex container`，所以該元素本身會`參與 BFC` 佈局，呈現垂直排列，另一方面，為其內容`建立 FFC`。

`display: inline-flex` 的元素會生成 `inline-level box` 與 `flex container`，所以該元素本身會`參與 IFC` 佈局，且會為其內容`建立 FFC`。

![](https://i.imgur.com/bkFfnzR.png)


<br>

### flex container
* display 為 `flex` 或 `inline-flex` 的元素。

* flex container 會建立 flex formatting contex(FFC)，所以此元素`不會`與 float 元素重疊。
    
* column-* 屬性`不`適用。

<br>

### flex item
display 為 flex 或 inline-flex 元素的`子`元素稱為 `flex item`。
 * `建立` BFC，所以 `flex items` 之間`不會`發生 `margin collapsing`，也不會與其父元素(flex container)發生 margin collapsing。
 
 * `參與` FFC。
 
 * vertical-align `不`適用。

 * float 與 clear `不`適用。
 
 * 即使 flex item 是 display: inline 的元素，仍然`可以`透過 `width` 與 `height` 屬性調整寬高。因 flex item 是 [blockified](https://www.w3.org/TR/css-display-3/#blockify)。
 

<br>
<br>
 
## 主軸(main axis)與副軸(cross axis)

flex items 可以透過 `flex-direction` 屬性來決定`排列方向`，flex-direction 同時也會決定`主軸(main axis)`與`副軸(cross axis)`。

![](https://i.imgur.com/8G9pzzZ.png)
圖片來源：[W3C](https://www.w3.org/TR/css-flexbox-1/#flex-direction-property)

<br>

flex-direction 共有四個屬性值，會受到`書寫方向 writing-mode` 影響。
<br>

![](https://i.imgur.com/3ow0Yie.png)
![](https://i.imgur.com/EkZ9GhU.png)


<br>

<b>以`橫向`且`由左至右`的書寫方式來說，此時 row 為橫向，column 為直向。</b>

### flex-direction: row
* 主軸為 row 方向。
* 副軸為 column 方向。
* 由 `main start` 至 `main end` 排列。

### flex-direction: row-reverse
* 主軸為 row 方向。
* 副軸為 column 方向。
* 由 `main end` 至 `main start` 排列。

### flex-direction: column
* 主軸為 column 方向。
* 副軸為 row 方向。
* 由 `main start` 至 `main end` 排列。

### flex-direction:column-reverse
* 主軸為 column 方向。
* 副軸為 row 方向。
* 由 `main end` 至 `main start` 排列。

<br>

## flex-wrap

`flex-wrap` 屬性適用於 `flex container`，有 `nowrap`、`wrap` 與 `wrap-reverse` 三種屬性值。
<br>

![](https://i.imgur.com/BNQ3Lbt.png)
圖片來源：[W3C](https://www.w3.org/TR/css-flexbox-1/#flex-wrap-property)

<br>

###  nowrap
```
flex-wrap: nowrap
```
* `不`換行，為預設值。 

![](https://i.imgur.com/UA89lTf.png)
<br>

### wrap
```css
flex-wrap: wrap;
```
* `換行`，由 `cross start` 開始向 `cross end` 堆疊。

![](https://i.imgur.com/zstxoLd.png)


<br>

### wrap-reverse
```css
flex-wrap: wrap-reverse;
```
* `可換行`，由 `cross end` 開始向 `cross start` 堆疊。

![](https://i.imgur.com/OkMGviF.png)

<br>

## flex-flow 屬性
`flex-direction` 屬性與 `flex-wrap` 屬性的`縮寫`。


例如： 
```css
flex-flow: row wrap;
```
上式等同於
```css
flex-direction: row;   /* 預設值 */
flex-wrap: wrap;
```
<br>

又例如：
```css
flex-flow: row-reverse wrap-reverse;
```
上式等同於
```css
flex-direction: row-reverse;
flex-wrap: wrap-reverse;
```
<br>

## order 屬性

`order` 屬性可以控制 `flex items` 的`順序`，會從 order 最小的開始排序。
僅適用於 `flex items`，屬性值須為`整數(可為負數)`， 預設下皆為 `0`。

![](https://i.imgur.com/8WuzGEP.png)
圖片來源：[W3C](https://www.w3.org/TR/css-flexbox-1/#order-property)


<br>

直接看圖比較快～
例如在書寫方向為橫向且由左至右的前提下，若主軸(main axis)為 row，即 flex-direction: row，則 flex items 的會依照其 order 由左至右排序(order 愈小愈優先)。

* **1 < 3 < 4**

![](https://i.imgur.com/GCfs4Aw.png)
<br></br>

* **-5 < -2 < 4 < 8**

![](https://i.imgur.com/lrWZ1qB.png)

<br>

若主軸(main axis)為 column，即 flex-direction: column，則 flex items 會依照其 order 由上至下排序(order 愈小愈優先)。
<br>

* **1 < 3 < 4**

![](https://i.imgur.com/jrz2iEn.png)
<br></br>

* **-10 < 4 < 7 < 13**

![](https://i.imgur.com/owoYoSz.png)


<br>
<br>





#### 參考資料
1. [W3C-CSS Flexible Box Layout Module Level 1](https://www.w3.org/TR/css-flexbox-1/#flexibility)
2. [MDN-Controlling Ratios of Flex Items Along the Main Axis](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax)
3. [CSS TRICKS-A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)