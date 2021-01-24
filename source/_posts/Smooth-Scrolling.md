---
title: 讓網頁平滑滾動!
categories: web
tags: 
      - CSS
toc: true
date: 2021-01-05 16:09:05
thumbnail: ./images/cover-css.png
---
在網頁開發中，錨點算是個常見的需求，例如：在點擊按鈕後，畫面想要蝦趴的「滑」到頂部。

這種平滑的滾動效果，其實只要加一行 CSS 就可以辦得到，無需勞動到 JS，也就是今天要介紹的 `scroll-behavior` 屬性，直接進入實作吧!

<!-- more -->

## 實作範例
<b>情境：</b>
現在頁面上有很多區塊(達到可滾動的狀態)，而不管畫面在哪，只要`點擊右下角橘色按鈕，就要滑動到最上方`。
[codepen 原始碼範例](https://codepen.io/yachen/pen/gOwzmVo?editors=1100)

![](./Smooth-Scrolling/situation.png)

<br>

HTML 如下:

```html
<a id="top"></a>
<section>1</section>
<section>2</section>
<section>3</section>
<section>4</section>
<section>5</section>
<section>6</section>
<section>7</section>
<section>8</section>
<section>9</section>
<section>10</section>
<a class="btn-back" href="#top">
  ↑
</a>
```

CSS 的部分，在 html 加上 `scroll-behavior: smooth` 即可。

```css
html {
  scroll-behavior: smooth;
}
```

實際效果如下：

![](./Smooth-Scrolling/result.gif)


<br>

最後，順帶看一下瀏覽器兼容性:

![](./Smooth-Scrolling/browser-compatibility.png)
圖片來源: [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scroll-behavior)


<br>

結束XD

<br>

<br/>
<br/>


<b>參考資源</b>
[MDN-scroll-behavior](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scroll-behavior)

