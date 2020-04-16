---
title: CSS 原理 - position
categories: web
tags: CSS
toc: true
date: 2020-03-31 20:03:24
thumbnail: https://i.imgur.com/qmupIlt.png
---
這篇要介紹的是 position，顧名思義，它與元素的位置或定位方式有關，了解它的各種屬性值肯定是必要的，像是常見的彈跳視窗或固定導覽列，也都需要它。
<!-- more -->

<br>


## position 語法
position 的屬性值有 static、relative、absolute、fixed 與 sticky。

```css
position: static | relative | absolute | fixed | sticky
```
<br>


### static
```css
position: static;
```
* 為預設值。

* 元素為 in-flow。

* top、right、bottom 與 left 屬性皆無效。

* z-index 屬性無效。 

<br>

![](https://i.imgur.com/Z8A5Qln.png)

<br>

### relative
```css
position: relative;
```
* 元素仍為 in-flow，為元素`預留原本的空間`。

* top、right、bottom 與 left 屬性可指定元素相對於`自身原本的位置`做偏移，`不影響`其他元素的位置。


![](https://i.imgur.com/PzvtHCj.png)

* 此屬性值對 display 值為 table-row-group、table-header-group、table-footer-group、table-row、table-column-group、 table-column、table-cell 與 table-caption 的元素無效。

<br>

### absolute
```css
position: absolute;
```

* 元素 out-of-flow，`不為`元素預留原本的空間。

![](https://i.imgur.com/N7nm4d3.png)

* 相對於祖譜中`最接近`且 `position` 值`非 static` 的`containing block(包含塊)`做定位，`若無`，則追溯至 initial containing block (初始包含塊)，在連續媒體下即為 veiwport (視口)。

* top、right、bottom 與 left 屬性可指定其對於`containing block(包含塊)`的偏移量，不影響其他元素的位置。

* `不會`與其他元素發生 `margin collapsing(外距重疊)`，因為會建立一個新的 BFC。

* 建立 `Block Formatting Context(BFC)`。


<br>

### fixed
```css
position: fixed;
```
<br>

* 元素 out-of-flow，`不為`元素預留原本的空間。

![](https://i.imgur.com/BIxGB5g.gif)

* `containing block(包含塊)`為 `veiwport`，所以會以 veiwport 做定位，滾動時，元素相對於 viewport 仍處於同一位置。

* 上層元素中若有 `transform` 屬性`非 none `的祖先時，`containing block(包含塊)`由 veiwport `改為該祖先`，即針對該祖先定位。

* top、right、bottom 與 left 屬性可指定其對於`containing block`的偏移量。

* `不會`與其他元素發生 margin collapsing(外距重疊)。

* 建立 `Block Formatting Context(BFC)`。


<br>

### sticky

為相對定位(relative)和固定定位(fixed)的混合體。元素在跨越`特定門檻(specified threshold)`之前屬於相對定位，之後屬於固定定位。
```css
position: sticky;
```
- 元素為 in-flow。

- 必須指定 top、right、bottom 或 left 其中一個做為`特定門檻(specified threshold)`，sticky 才有效，`即使是 top: 0`。

- 相對於最近的可滾動祖先和 containing block 做定位。

- top、right、bottom 與 left 屬性可指定其偏移量，不影響其他元素的位置。

- 建立 `Block Formatting Context(BFC)`。

<br>

例如:

```html
<div class="container">
  <div class="box">1</div>
  <div class="box">2</div>
  <div class="box">3</div>
</div>
```

```css
.container{
  width: 280px;
  height: 1850px;  /*  可滾動   */
}

.box{
  position: sticky;
  top: 150px;  /*  門檻值(必要條件)  */ 
  width: 70px;
  height: 70px;
  margin-bottom: 100px;
}
```

![](https://i.imgur.com/ZoLNZ9R.gif)


<br>
<br>

<br>參考資源</br>
[W3C-hoosing a positioning scheme: 'position' property](https://www.w3.org/TR/CSS22/visuren.html#choose-position)
[MDN-position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)
[MDN-Block formatting context](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)

