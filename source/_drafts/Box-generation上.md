---
title: CSS 原理 - 你所不知道的 display
categories: web
tags: CSS
---
Visual formatting model (視覺格式化模型) 對於排版來說是很重要的觀念，但不是那麼平易近人且有點抽象，在初次看[W3C 規範](https://www.w3.org/TR/CSS2/visuren.html) 時一定是霧煞煞，強烈建議先釐清幾個重要名詞的定義，會發現繞來繞去，講的其實都是相同的概念。

<!-- more -->

## display

為什麼 display: block 的元素們會呈現垂直排列？為什麼 display: inline 的元素們會呈現水平排列？又為什麼 display: inline-block 的元素是呈現水平排列呢？其實答案都與元素生成(generation)何種類型的 box 有關。 
<br>

在先前 [box model 文章](https://yachen168.github.io/article/box-model.html)中曾介紹，元素就像一個個的盒子(box)，這篇將介紹在 normal flow 下，這些「 盒子如何排列」，或說是「`元素如何佈局`」。

元素在 normal flow 中如何佈局的主要關鍵為該元素「生成 (generate)了什麼類型的 box」，而這會取決於元素的 `display 屬性`。

其實我們所熟悉的 display 同時定義了元素的兩種 display type：
- outer display type：決定元素「`本身`」在 normal flow 下如何佈局，即元素本身`參與`的是何種 formatting context。

- inner display type：決定元素為它的後裔元素`建立`何種 formatting context，與其`後裔元素的佈局`有關。

<br></br>

### outer display type

在 outer display type 方面，box 類型可分成 inline-level box 與 block-level box 兩大類，所有 inline-level box 皆會參與 IFC，呈現水平排列，而所有 block-level box 皆會參與 BFC，呈現垂直排列。

其中 inline-level box 又可以細分為 inline box 與 atomic inline-level box 兩種。
  * inline-level box (行內級盒子)
    * inline box (行內盒子)

    * atomic inline-level box (原子行內級盒)
  * block-level box (塊級盒子)

<br>

### inner display type
在 inner display type 方面，因其描述的是元素本身與其內容或後裔元素的關係，可以想像成元素像個容器(container)，裝著文字內容或後裔元素。常見的 container box 類型有 block container box、flex container box 與 grid container box 等等。

而什麼類型的 container，就會為其內容或後裔元素`建立`什麼類型的 formatting context，例如：

- flex container 建立 flex formatting context (FFC)

- grid container 建立 grid formatting context (GFC)
- block container 可建立 block formatting context (BFC) 或 inline formatting context (IFC)

順帶一提，替換元素 (replaced element)，例如 \<img>，display 的預設值為 inline，不論將其 display 屬性值改成什麼，皆不會有 container box，因為它就是路徑來源的圖片，不是用來裝像是 \<span>、\<div> 或其他元素的容器。

<br>
<br>

## 了解 display 的好處

講了這麼多，了解 display 到底有什麼好處？
個人認為最大的兩個優點

### 秒殺元素的排列方式

### 秒殺屬性的適用對象








<br>

## Box generation

### inline-level box (行內級盒)

![inline-level box](./box-generation上/inline-level-box.png)

inline-level box 又可以細分為



<br></br>

### block-level box (塊級盒)

摘自 [W3C](https://www.w3.org/TR/CSS22/visuren.html)
> Block-level boxes are boxes that participate in a block formatting context.

block-level box 會參與`塊格式化上下文(Block Formatting Context，BFC)`。

<br></br>
![](https://i.imgur.com/Lh4m2J7.png)

<br></br>

* block-level box(塊級盒)
> 描述`元素`與`其父母元素`和`兄弟姐妹元素`之間的關係，有時候也同時為 block container(塊容器盒)。

* block container box
> 描述`元素`與其`子元素`之間的關係，確定其`子元素`的定位與佈局，有時候也同時是 block-level box(塊級盒)。

<br></br>
若一個塊級盒`同時`是個塊級容器，則`又稱為塊盒`(block box)，此時亦可稱為 `block`。

如`下圖`所示，有些塊級盒並不是塊容器盒，例如 `display` 為 `table` 時，會生成`塊級盒`，但不會生成`塊容器盒`，而有些塊容器盒也不是塊級盒，例如當`非替換元素` `display` 值為 `inline-block` 時，其內部會形成一個`塊容器盒`，而`元素本身`因成為`行內級元素`(inline-level element)而生成一個`行內級盒`(inline-level box)，而`非`成為塊級元素且生成塊級盒。

註： `替換元素`(例如 \<img>)，設定為 display: inline-block 時，它並`不會`生成`塊容器盒`，因它是連結外部的圖片，而不是用來裝塊級盒或行內級盒。
<br></br>
> 會生成何種盒子取決於元素的 display 屬性，`平時所使用的 display 屬性僅是縮寫`，完整內容在 [W3C 的 display 表格](https://www.w3.org/TR/css-display-3/#the-display-properties) 中有列出，包括其會`生成哪些盒子`皆有詳細列出。
<br></br>

![](https://i.imgur.com/y1GaXGE.png)
![](https://i.imgur.com/obvcCeZ.png)


圖片來源：[W3C](https://www.w3.org/TR/css-display-3/#the-display-properties)

<br></br>
<br></br>
<br></br>
<br></br>
<br></br>
<br></br>

參考資源
[W3C-Controlling box generation](https://www.w3.org/TR/CSS22/visuren.html)
[W3C-Appendix A: Glossary](https://www.w3.org/TR/css-display-3/#glossary)
[W3C-Box Layout Modes: the display property](https://www.w3.org/TR/css-display-3/#the-display-properties)
[MDN-Visual formatting model](https://developer.mozilla.org/en-US/docs/Web/CSS/Visual_formatting_model)