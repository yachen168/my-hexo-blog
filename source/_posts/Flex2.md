---
title: CSS 原理 - Flex(中)
categories: web
tags: CSS
toc: true
date: 2020-05-10 15:54:33
thumbnail: ./images/cover-css.png
---

這篇將介紹 flex 屬性，並且深入探討 flex 究竟是如何計算伸縮的比例。

<!-- more -->

<br>

## 名詞介紹

若要理解 flex 是如何計算伸縮比例，首先需要了解一下 positive free space 與 negative free space 這兩個名詞。

### positive free space 
若 `flex items` 在`主軸(main axis)`方向上的尺寸總和`小於` `flex container` 的尺寸，此時會出現 flex container 的空間沒有被填滿，這些剩餘空間就稱為 `positive free space`。

例如，在主軸(main axis)為 row 下，若 flex container 的寬度為 500px，而 a、b、c 三個 flex items 寬度各為 100px，此時 flex container 還有 200px 的 positive free space。

<br>

![](https://i.imgur.com/lNhLpcs.png)
[圖片來源 MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax)

<br>

### negative free space
若 `flex items` 在`主軸(main axis)`方向上的尺寸總和`大於` `flex container` 的尺寸，此時 flex container 的空間不夠用，而 flex items 的尺寸總和與 flex container 尺寸的`差額`就稱為 `negative free space`。


例如，在主軸(main axis)為 row 下，若 flex container 的寬度為 500px，而 a、b、c 三個 flex items 寬度各為 200px，此時 flex container 的寬度 500px 小於 flex items 的寬度總和 600px，negative free space 為 600px 減 500px，等於 100px。

<br>

![](https://i.imgur.com/OugSaLG.png)
[圖片來源 MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax)

<br>

## flex 屬性

![](https://i.imgur.com/MkKEK9o.png)
圖片來源：[W3C](https://www.w3.org/TR/css-flexbox-1/#flex-property)

```css
flex: flex-grow ｜ flex-shrink ｜ flex-basis;
```

* 為三個屬性 `flex-grow`、`flex-shrink` 與 `flex-basis` 的縮寫。
  * flex-grow: 決定 `flex item` 將`得到`多少`比例`的 `positive free space`。
  * flex-shrink: 決定 `flex item` 將`得到`多少`比例`的 `negative free space`。
  * flex-basis: 決定 `flex item` 在尚未 grow 與 shrink 前的`原始尺寸`。

* 預設值為 `flex: 0 1 auto`

<br>

### flex-grow 
* 以 `flex-basis` 為基礎，決定 `positive free space` 的`分配比例`，所以有 positive free space 才有作用。
     * 必須為 `≥ 0` 的值。
     * 若為 `0` (`預設值`)，則`不會延伸`。

#### 計算方式

在 flex-direction: row 下，有一個寬度為 500px 的 flex container，與三個寬度各為 100px、80px、70px 的 flex items，則在預設下會有 500px - 250px = 250px 的 positive free space 。

```html

<div class="container">
  <div class="item1">item1</div>
  <div class="item2">item2</div>
  <div class="item3">item3</div>
</div>
```
```css
.container {
  display: flex;
  outline: 3px solid #444;
  width: 500px;
  height: 120px;
  }  

.item1 {
  width: 100px;
  flex: 0 1 auto;    /* 此為預設值 */
}

.item2 {
  width: 80px;
  flex: 0 1 auto;    /* 此為預設值 */
}

.item3 {
  width: 70px;
  flex: 0 1 auto;    /* 此為預設值 */
}
```
![](https://i.imgur.com/UyGZoAz.png)


<br>

現在將三個 flex items 的 `flex-grow` 分別設定為 `1`、`2`、`2`。
* item1 將分配到 1/(1+2+2) = `1/5` 的 positive free space。最終寬度為 150px。
* item2 將分配到 2/(1+2+2) = `2/5` 的 positive free space。最終寬度為 180px。
* item3 將分配到 2/(1+2+2) = `2/5` 的 positive free space。最終寬度為 170px。

```css
.item1 {
  width: 100px;
  flex: 1 1 auto; /* 將 flex-grow 改為 1，將佔 1/5 等分 */
}

.item2 {
  width: 80px;
  flex: 2 1 auto; /* 將 flex-grow 改為 2，將佔 2/5 等分 */
}

  
.item3 {
  width: 70px;
  flex: 2 1 auto;  /* 將 flex-grow 改為 2，將佔 2/5 等分 */
}
```

![](https://i.imgur.com/xOlD3h8.gif)
<br>

### flex-shrink 
* 以 `flex-basis` 為基礎，決定 `negative free space` 的`分配比例`，所以有 negative free space 才有作用。
  
   * 必須為 `≥ 0` 的值。
   * 若為 `0`，則`不會`收縮，此時若 container 空間不夠，會發生 `overflow`。
   * `預設值`為 `1`。

* flex items `不會`縮短至`小於` `min-content` 的尺寸，除非設定 min-width 或 min-height 屬性。

<br>

#### 計算方式


flex container 寬度為 500px，其三個 flex items 寬度各為 200px，故此時有 100px 的 negative free space，若將所有 flex items 的 flex-shrink 皆設定為 0，則將發生 overflow。

```html
<div class="container">
  <div class="item1">item1</div>
  <div class="item2">item2</div>
  <div class="item3">item3</div>
</div>
```
```css
.container { 
    display: flex;
    width: 500px;
    height: 120px;
    outline: 3px solid #333;
}

.container > * {      
  width: 200px;       /* 所有 flex items 寬度皆為 200px */ 
  flex: 0 0 auto;     /* 將所有的 flex-shrink 設定為 0 */
}
```
![](https://i.imgur.com/QmDyzSx.png)

<br>

現在將第一個與第二個 flex item 的 `flex-shrink` 分別設定為 `1` 與 `4`，第三個 flex item 的 flex-shrink 依然為 `0`。

<b>注意，shrink 與 grow 的計算方法並不相同，公式可參考 [前端新手村 flex grow & shrink 演算法](https://ithelp.ithome.com.tw/articles/10194694)。</b>

negative free space = 500px - 200 \*3 = -100px

* item 1 將分配到 `1/5` 的 negative free space，所以最後寬度為 180px。
  > 算法： 1 \* 200/(1 \* 200 + 4 \* 200 + 0 \* 200)= 1/5

* item 2 將分配到 `4/5` 的 negative free space，所以最後寬度為 120px。
  > 算法： 4 \* 200/(1 \* 200 + 4 \* 200 + 0 \* 200)= 4/5 

* item 3 將分配到 `0/5` 的 negative free space，所以最後寬度為 200px。
  > 算法： 0 \* 200/(1 \* 200 + 4 \* 200 + 0 \* 200)= 0

<br>

```css
.item1 {
  flex: 0 1 auto;      /* 佔 1*200/(1*200+4*200+0*200)= 1/5 等份 */
}

.item2 {
  flex: 0 4 auto;     /* 佔 4*200/(1*200+4*200+0*200)= 4/5 等份 */
}

.item3 {
  flex: 0 0 auto;     /* 佔 0*200/(1*200+4*200+0*200)= 0 等份 */
}
```

![](https://i.imgur.com/3pxK0LD.gif)



<br>

### flex-basis
* 決定 flex item 在尚未 grow 或 shrink 前的`原始尺寸`。
* `預設值`為 `auto`。
    * 若`有`指定尺寸 width、height (視主軸 main axis 而定)，則依照`所指定的尺寸`。
    * 若`無`指定尺寸 width、height (視主軸 main axis 而定)，則由其`內容大小`決定。
    
* 若為 `0`，則 flex item `不被納入空間計算`。例如有一個 container 寬度 400px，裡面有 item1 與 item2，寬度各為 200px，此時 flex-basis 為預設的 auto，故沒有剩餘空間。但若將 item2 的 flex-basis 設為 0，則會有 200px 的剩餘空間，原因是 flex-basis 優先級高於 width 與 height。
        
* 除了 auto 之外，尚有 content、max-content、min-content 等屬性值，不過多數瀏覽器不支援。

* 可給定`有單位`的`數值`，例如 100px 或 10%，若`同時`設定 `flex-basis (非 auto)` 和`尺寸`(width 或 height，視主軸 main axis 而定)，則以 `flex-basis` 為`優先`。

例如：

有三個 flex items，指定第一個 flex item 的 width 為 200px，
在 flex-direction: row 且 flex-grow 與 flex-shrink 皆為 0 的前提下，設定 flex-basis 值為 auto，此時
* 第一個 flex item 的寬度為 200px。
* 第二與第三個 flex item 的寬度為其內容尺寸。

![](https://i.imgur.com/zY5dqky.png)
```html
<div class="container">
  <div>flex item 1</div>
  <div>flex item 2</div>
  <div>flex item 3</div>
</div>
```
```css
div {
  outline: 1px solid #333;
}

.container {
  display: flex;
}

.container :first-child {
  width: 200px;    /* 第一個子元素給定寬度 200px */
}

.container > * {
  flex: 0 0 auto;  /* 所有子元素的 flex basis 為 auto*/
}
```

<br>
<br>

## flex 屬性另類寫法

flex 屬性除了直接給定 flex-grow、flex-shrink、flex-basis 三個值之外，還有以下幾種寫法。

* flex: `initial`
* flex: `auto`
* flex: `none`
* flex: `<正數>`

註：<正數> 可不必為整數，例如可為 0.5。

<br>

以下分別說明這四種屬性值的意義。


<br>

### flex: initial
```css
flex： initial;
```
等同於
```css
flex： 0 1 auto;
```

flex item 會依照 width 或 height (視主軸 main axis 而定)屬性決定其尺寸，即使 container 中`有剩餘空間`，flex item 仍`無法延伸`，但當`空間不足`時，元素`可收縮`。

<br>

### flex: auto
```css
flex: auto;
```
等同於
```css
flex: 1 1 auto;
```

flex item `可延伸`與`收縮`，會依照 width 或 height (視主軸 main axis 而定)屬性決定其尺寸。若所有 flex items 均設定 flex: auto 或 flex: none，則在 flex items 尺寸決定後，剩餘空間會被平分給 flex: auto 的 flex items。

<br>

### flex: none 
```css
flex: none;
```
等同於 
```css
flex: 0 0 auto;
```

flex item `不可延伸`與`收縮`，會依照 width 或 height (視主軸 main axis 而定)屬性決定其尺寸。

<br>

### flex: positive-number
```css
flex: <正數>;
```
等同於 
```css
flex: <正數> 1 0;
```

flex item `可延伸`與`收縮`，flex-basis 為 `0`，故 flex item 會依據所設定的比例佔用 container 中的剩餘空間。

例如

```css
flex: 2;
```
等同於

```css
flex: 2 1 0;
```

可利用此種屬性值的指定方式，輕易地決定 `flex items` 在 `flex container` 中所佔的尺寸`比例` (width 或 height，視主軸 main axis 而定)。


<br>

例如，一個 flex container 裡面有三個 flex items，希望不管 container 如何變化，這三個 items 的`尺寸皆一樣大`，則可設定每個 items 的 flex 屬性有相同的正數(positive-number)。

```html
<div class="container">
  <div class="item1"></div>
  <div class="item2"></div>
  <div class="item3"></div>
</div>
```
```css
.container {
  display: flex;
  height: 120px;
}

.item1 {
  flex: 1;   /* 將得到容器中剩餘空間的 1/(1+1+1)＝1/3 等份 */
}

.item2 {
  flex: 1;   /* 將得到容器中剩餘空間的 1/(1+1+1)＝1/3 等份 */
}

.item3 {
  flex: 1;   /* 將得到容器中剩餘空間的 1/(1+1+1)＝1/3 等份 */
}
```

 註：只要是`相同正數`即可，其僅代表`比例`關係。

![](https://i.imgur.com/wWL2qtn.png)

且不論容器如何變動(例如縮放視窗時)，比例皆為 `1 : 1 : 1`。

![](https://i.imgur.com/xNoC0bS.gif)


<br>

同理，若希望這三個 flex items 的尺寸比例分別為 `3 : 2 : 1`，則可設定：

```css
.item1 {
  flex: 3;   /* 將得到容器中剩餘空間的 3/(3+2+1)＝3/6 等份 */
}

.item2 {
  flex: 2;   /* 將得到容器中剩餘空間的 2/(3+2+1)＝2/6 等份 */
}

.item3 {
  flex: 1;   /* 將得到容器中剩餘空間的 1/(3+2+1)＝1/6 等份 */
}
```
![](https://i.imgur.com/L1azPUh.png)

且不論容器如何變動(例如縮放視窗時)，比例皆為 `3 : 2 : 1`。

![](https://i.imgur.com/qIBQMX1.gif)


<br>
<br>
<br>

有關於 flex 屬性以及伸縮的計算方式就介紹到此，下一篇將繼續介紹 flex 的各種對齊方式。


<br>
<br>
<br>
<br>


<b>參考資源</b>
1. [W3C-CSS Flexible Box Layout Module Level 1](https://www.w3.org/TR/css-flexbox-1/#flexibility)
2. [MDN-Controlling Ratios of Flex Items Along the Main Axis](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Controlling_Ratios_of_Flex_Items_Along_the_Main_Ax)
3. [前端新手村 flex grow & shrink 演算法](https://ithelp.ithome.com.tw/articles/10194694)

