---
title: 用純 CSS 做定格動畫
date: 2020-06-13 12:54:55
tags: 
      - CSS
      - w3HexSchool
categories: web
toc: true
thumbnail: ./images/cover-css.png
---


什麼是定格動畫？簡單來說，是一種動畫技術，它的原理是將每幀不同的圖像快速播放，因人眼有視覺暫留，所以會產生動畫效果。小時候玩的手翻書就是以這樣的原理來達成動畫的效果！

<!-- more -->


<br>

## 事前準備
要用 CSS 完成定格動畫，首先需要有雪碧圖(Sprite)素材，當然，還需要會 CSS animation 的基本操作。

- 雪碧圖(Sprite)
- CSS animation

<br>

## 什麼是雪碧圖(Sprite)

將多張圖像整合為單一圖像，然後利用定位的方式顯示各部分圖的技術，早期網頁常以這種方式來減少圖片加載的次數，以便提高網頁顯示速度。

<br>



## CSS animation
在 CSS animation 包含了以下幾種屬性，其中，若要做定格動畫，`timing-function` 的設定為關鍵，需要將 timing-function 設定為 `steps`。

* 動畫名稱 animation-name
* 持續時間 animation-duration
* `時間函數 animation-timing-function`
  * ease (預設值)
  * ease-in
  * ease-in-out
  * ease-out
  * linear
  * `steps(n, step position)`

* 延遲時間 animation-delay
* 播放次數 animation-iteration-count
* 播放方向 animation-direction
* 前後狀態 animation-fill-mode
* 運行停止 animation-play-state

<br>


### steps(n, step position)
steps 第一個參數 n 為一正整數，至於第二個參數 step position，這裡僅介紹 `start` 與 `end(預設值)` 兩個值。
* **n**： 正整數

* **step position**
    * start：動畫一開始就跳一階
    * `end (預設值)`
<br>

與[其他多數 timing-function](https://developer.mozilla.org/zh-CN/docs/Web/CSS/timing-function) 不一樣的是，`step` 像是階梯(step) 一樣，一階一階，而非完全連續型函數。

下圖左方為 `start` 的 timing-function，右方為 `end` 的 timing-function，若看不懂 timing-function 其實也沒關係，會用就好XD

![](https://i.imgur.com/REHYirr.png)

![](https://i.imgur.com/pi70fox.png)

<br>
<br>

用一個簡單的小範例可看出 `start` 與 `end` 的差異。
若要做定格動畫，我們需要利用 `end` 的特性。

<iframe height="439" style="width: 100%;" scrolling="no" title="PowbBxq" src="https://codepen.io/yachen/embed/PowbBxq?height=439&theme-id=default&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yachen/pen/PowbBxq'>PowbBxq</a> by yachen
  (<a href='https://codepen.io/yachen'>@yachen</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br>
<br>


### 開始做定格動畫
有了一些預備知識之後，就可以來動手做定格動畫了！只要依照以下 3 個步驟就可以完成。

<br>

#### step1
> 確認雪碧圖有幾幀 ⇒ 決定 steps(n) 中的 n，完整寫法為 steps(n, end)，可省略 end 不寫(因為是預設值)。


例如：
下方雪碧圖有 8 幀 ⇒ steps(8)。

![](https://i.imgur.com/wiVwOhb.png=80%)

<br>

#### step2
> 確認雪碧圖的總寬度(或總高度) ⇒ 決定總共要移動多少距離。

例如：下方雪碧圖的總寬度為 2880px ⇒ 圖片將移動(translateX) 2880px。

![](https://i.imgur.com/vRkxFb8.png)


<br>

#### step3
> 計算每一幀的寬度或高度 ⇒ 決定容器元素的寬度或高度。

每一幀的寬度 ＝ 總寬度 / 幀數

所以每一幀的寬度為 2280px / 8，等於 360px  ⇒ 將容器元素寬度設定為 360px

<br>

#### 實現結果
```html
<div class="container">
  <!-- 雪碧圖(總寬度 2880px，共 8 幀)-->
  <img src="https://i.imgur.com/5aXROc2.png" alt=""/>
</div>
```

```css=
.container {
  width: 360px;    // 總寬度 2880px / 8幀
  overflow: hidden;
  animation: moveContainer 5s infinite linear;
}

img {
  animation: run 0.5s infinite steps(8);  // 8 幀
}

@keyframes run {
  0% {
      transform: translateX(0px);
  }
  100% {
      transform: translateX(-2880px);    // 總寬度 2880px
  }
}

// 移動 container，製造「向前跑」效果
@keyframes moveContainer {
  0% {
      transform: translateX(0px);
  }
  100% {
      transform: translateX(1600px);
  }
}
```

<iframe height="631" style="width: 100%;" scrolling="no" title="jOEMBzQ" src="https://codepen.io/yachen/embed/jOEMBzQ?height=631&theme-id=default&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yachen/pen/jOEMBzQ'>jOEMBzQ</a> by yachen
  (<a href='https://codepen.io/yachen'>@yachen</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br>
<br>
<br>
<br>

可以在網路上找自己喜歡的雪碧圖素材，運用相同的原理，做出各種有趣的定格動畫。

<iframe height="572" style="width: 100%;" scrolling="no" title="YzPGNaK" src="https://codepen.io/yachen/embed/YzPGNaK?height=572&theme-id=default&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yachen/pen/YzPGNaK'>YzPGNaK</a> by yachen
  (<a href='https://codepen.io/yachen'>@yachen</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br>
<br>

<iframe height="640" style="width: 100%;" scrolling="no" title="VwYmVYd" src="https://codepen.io/yachen/embed/VwYmVYd?height=640&theme-id=light&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yachen/pen/VwYmVYd'>VwYmVYd</a> by yachen
  (<a href='https://codepen.io/yachen'>@yachen</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>



<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


參考資源：
[CSS Easing Functions Level 1](https://www.w3.org/TR/css-easing-1/#step-position)

[MDN-timing-function](https://developer.mozilla.org/zh-CN/docs/Web/CSS/timing-function)
