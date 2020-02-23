---
title: CSS 原理 - Line box
categories: web
toc: true
tags: 
  - CSS
  - w3HexSchool
date: 2020-02-23 11:58:29
thumbnail: https://i.imgur.com/z4N9CzB.jpg
---

Line box 就像筆記本上的線框，一行一行的垂直堆疊，裡面裝著文字或是 inline-level boxes，而我們可以利用 text-align 與 vertical-align 屬性操控 inline-level boxes 在 line box 裡的水平與垂直對齊。

<!-- more -->

## 什麼是 line box 

![](./LineBox/notebook.jpg)
圖片來源：[visualhunt](https://visualhunt.com/photo2/1605/)

line box 是用來裝文字與所有 inline-level box 的，換句話說，`只要有文字或是 inline-level box 的地方，就會出現 line box`，就像「行」的概念，一個 line box 就是一行。這篇將說明 line box 的範圍是如何決定的。

若對於哪些元素會生成 inline-level box 不太清楚，可複習[上一篇 display](https://yachen168.github.io/article/dispaly.html#more) 文章。

<br>

## line box 寬度
line box 是「行」的概念，也是一個矩形範圍，正常情況下，line box 的寬度取決於 [Containing block(包含塊)](https://yachen168.github.io/article/Containing-block.html)，但若有 float 元素，則 line box 會受到壓縮(shrink)。

直接用實例來看會較具體一點，以下將比較正常情況下與在有 float 元素下，line box 的寬度會如何變化。

<br>

### Case: 無 float 元素時
以 display: inline-block 為例，該元素屬於 inline-level box，故會產生 line box，而 line box 寬度取決於其 containing block(包含塊)。

```html
<div class="block-container">
  <div class="inline-level-box">
    display: inline-block
  </div>
</div>

```
```css
.block-container {
  width: 600px;
  height: 250px;
  border: 10px solid #333;
}

.inline-level-box {
  display: inline-block; /* 為一種 inline-level-box */
  height: 100px;
  color: white;
  font-size: 20px;
  background-color: orange;
}
```

![](./LineBox/width-2.png)

<br>

此時設定 text-align: center，則橘色的 inline-level box 會水平置中於 line box 裡，這結果應該毫不令人意外。

```css
.block-container {
  text-align: center;
}
```
![](./LineBox/text-align-2.png)

<br>

### Case: 有 float 元素時

當有 float 元素時，line box 會受到 float 元素的擠壓，若擠壓到 line box 無法容納裡面的文字或是 inline-level box 時，line box 就會自動「`換行`」。

```css
.block-container {
  width: 600px;
  height: 250px;
  border: 10px solid #333;
  margin: 100px;
}

.float {
  float: left;   /* float 會擠壓 line box */
  width: 100px;
  height: 150px;
  color: white;
  background-color: #F75848;
}

.inline-level-box {
  display: inline-block;   /* 為一種 inline-level-box */
  color: white;
  font-size: 20px;
  height: 100px;
  background-color: orange;
}
```
<br>

此時 line box 已經被 float 元素壓縮了。[範例連結](https://codepen.io/yachen/pen/dyoOLWR?editors=1100)

![](./LineBox/float-1.png)


設定 text-align: center，讓橘色的 inline-level box 水平置中於被壓縮後的 line box 裡。
```css
.block-container {
  text-align: center;
}
```
![](./LineBox/float-2.png)

<br>

接著，若 float 元素變更寬，擠壓 line box 更多，多到該行無法再容納橘黃色的 inline-level box 時，line box 就會「換行」。
```css
.float {
  width: 500px;  /* 變超胖 */
}
```

![](./LineBox/float-4.png)


<br>

## line box 高度

如同上述，line box 是用來裝文字或 inline-levle box 的，所以一個 line box (同一行)高度由位置`最高`的 `inline-level box 頂部`與位置`最低`的 `inline-level box 底部`的距離。

其中 inline-level box 又可二分為 inline box 與 atomic inline-level box 兩種，像是`替換元素 <img>` 或 `display: inline-block` 皆屬於 atomic inline-level box，下一篇文章會說明兩種有何差別(應該會吧)。

<br>

### Case: 若是 inline box
> Inline box 僅有 box model 中的 content area 會影響 line box 高度。

Line box 僅取決於 inline box 的 content area (像是 line-height 或 font-size 皆會影響 content area)，`不`包含 padding、border、margin。

#### 例子

\<span> 預設為 display: inline，且非替換元素(nonreplaced elememt)，為 inline-level element 中的 inline element，故其僅有 line-height 會影響 line box，padding、border、margin 皆不會影響 line box 高度。

```html
<div>Lorem ipsum, dolor sit add
    <span>我在 inline box 裡面</span>
    Lorem ipsum, dolor sit amet consectetur .
</div>
```
```css
div {
  width: 220px;
  margin: 200px;
  border: 1px solid #000;
}

span {
  background-color: orange;
  padding: 10px;
}
```


為方便觀察，給 \<span> 背景橘色的顏色，並給 \<span> 上下左右 padding 各 10px。
`反白區域為一個 line box 高度`。可以清楚看見，`line box 並沒有被撐高`，上下方的文字沒有被推開(但左右有)。

![](./LineBox/inline-box.png)


<br></br>


### Case: 若是 atomic inline-level box
> Atomic inline-level box 的整個 box model 會影響 line box 高度。

Line box 取決於 atomic inline-level box 的整個 `box model 高度`，即包含 padding、border 與 margin 部分。
  
<br>

#### 例子

現在將 \<span> 設定為 display: inline-block，\<span> 仍然屬於 inline-level element，但由 inline element 變成 `atomic inline-level element`。

```css
span {
  display: inline-block; /* 變成 atomic inline-level */
  background-color: orange;
  padding: 10px;
}
```
<br></br>

`反白區域為一個 line box 高度`。可以清楚看見，`line box 長高了`，所以上下方文字的距離也隔開了！

![](./LineBox/atomic-box.png)



<br>

註：
① box model 高度為內容區高度 + 上下內距 +上下邊框 + 上下外距，可參考[先前文章](https://yachen168.github.io/article/box-model.html#more)。

② [其它替換元素](https://developer.mozilla.org/zh-TW/docs/Web/CSS/Replaced_element)





<br>
<br>




<b>參考資源</b>
[W3C - Inline formatting contexts](https://www.w3.org/TR/CSS21/visuren.html#inline-formatting)
[W3C - Line height calculations](https://www.w3.org/TR/CSS21/visudet.html#line-height)
[W3C - Floats](https://www.w3.org/TR/CSS2/visuren.html#floats)
[鉄人28号FX　鉄人2号「文本士」content area](https://ithelp.ithome.com.tw/articles/10216486)

