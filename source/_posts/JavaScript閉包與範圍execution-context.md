---
title: JavaScript 閉包與範圍 ── Execution Context 
date: 2020-06-21 21:27:57
tags: 
      - JavaScript
      - w3HexSchool
categories: web
toc: true
thumbnail: ./images/cover-js.png
---

閉包(closure)在 JavaScript 中佔著重要的地位，但閉包本人其實不太好搞，先從與閉包密不可分的 ── 作用範圍(Scope)開始下手吧！這篇將從執行背景空間(execution context)開始介紹，一步步看 JavaScript 引擎是如何追蹤程式碼的執行，然後......等時機到了就會知道什麼是閉包了(吧)。
<!-- more -->

如果不相信閉包本人很難搞的話，可以打開 MDN，你會看到**閉包的定義**為：

[英版 MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)：
> A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.

[中版 MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Closures)：
> 閉包（Closure）是函式以及該函式被宣告時所在的作用域環境（lexical environment）的組合。

<br>

<img src="https://i.imgur.com/045RJSL.png" width="320px">

<br>
<br>


如果你看不懂 MDN 在說什麼，別鐵齒了~還是先繞過閉包本人吧！


<br>

## Execution Context (執行背景空間)
由於 JavaScript 屬於單執行緒(Single Thread)，也就是它「`一次只能做一件事`」，所以當它要去執行另一件任務 B 時，就勢必得先暫停手上正在進行的任務 A，等待任務 B 執行完畢後，再回過頭繼續完成任務 A。若是在執行任務 B 時，又被叫去做任務 Ｃ，這時又得停下手邊的任務 B，先去執行完任務 C，接著回頭處理完任務 B，最後再回到任務 A。以此類推，因此所有一連串的任務都需要被追蹤。

每個任務都有一個`執行背景空間(execution context)`，JavaScript 引擎用 call stack 來追蹤它們。

在 JavaScript 中，有兩種執行背景空間(execution context)：
1. **全域執行背景空間**
> 只有一個，且總是最早被建立的，負責處理全域中的程式碼。

2. **函式執行背景空間**
> 每`呼叫`函式一次，就會建立一個新的函式執行背景空間，負責處理函式中的程式碼。

<br>
<br>

## call stack (呼叫堆疊)

JavaScript 引擎利用`call stack(呼叫堆疊)`來追蹤所有任務，

現在用個例子來看看執行背景空間(execution context)究竟是如何堆疊(被追蹤)的。
```javascript=
function a(){
  b();    // 呼叫函式 b
}

function b(){
  console.log('函式 b 被執行了')
}

a();    // 第一次呼叫函式 a
a();    // 第二次呼叫函式 a
```

<br>

![](https://i.imgur.com/1aoMHGq.png)
![](https://i.imgur.com/CTDEBoE.png)

1. 最先被創建的永遠是全域執行背景空間。
<br>

2. 在全域下，定義了函式 a 與函式 b，接著**呼叫了函式 a** (第 9 行)，於是建立 a 函式執行背景空間，並堆放至 stack 中，由於 JavaScript 一次只能做一件事，所以此時全域執行背景空間會被暫停。
<br>

3. 因在函式 a 中又**呼叫了函式 b**，於是建立 b 函式執行背景空間，並堆放至 stack 中，此時 a 函式執行背景空間會被暫停。
<br>

4. 印出「函式 b 被執行了」後，b 函式執行完畢，於是 b 函式執行背景空間從 stack 中移出(pop)，回到了 a 函式執行背景空間繼續執行。
<br>

5. a 函式執行完畢(a 函式中也已經沒有其他程式碼需要被執行了)，a 函式執行背景空間從 stack 中移出，回到全域執行背景空間繼續執行。
<br>

6. 在全域下，第 10 行中又再次**呼叫函式 a**，以上 2~5 的過程又重複一次。

<br>


可以發現，stack 中的執行背景空間會「後進先出」，也就是較晚被堆疊進來的執行背景空間，會先被執行完然後 pop 出去。

我們也可以在瀏覽器的除錯工具中觀察 call stack 的變化，來驗證一下過程是不是如上述一樣。


首先在 VScode 中開啟 live server，接著打開開發者工具(按 F12)，點選 `Source`，然後選擇要觀察的 js 檔案。

![](https://i.imgur.com/9QtZBy6.png)

<br>

點擊左邊的行數來下中斷點，一個紅點代表一個中斷點(範例中將中斷點下在第 9 行)。

然後重新整理一下頁面。

會發現在 call stack 欄位中，出現一個 (anonymous)，它就是全域執行背景空間(global execution context)。要注意的是，此時第 9 行的程式碼尚未被執行。

![](https://i.imgur.com/Y4Lvx2w.png)


接著按下一步，開始執行第 9 行程式碼，也就是**呼叫函式 a**。
會發現 call stack 中被疊加了 a 函式執行背景空間。

![](https://i.imgur.com/sJb1wuF.png)
![](https://i.imgur.com/5EYxFyc.png)

接著繼續按下一步，執行函式 a 中的程式碼，也就是**呼叫函式 b**。會發現 call stack 中又被疊加了 b 函式執行背景空間，此時 a 函式執行背景空間被暫停。

![](https://i.imgur.com/YpH3TcJ.png)

接著再按下一步，執行完函式 b 中的程式碼(console.log)，b 函式執行背景空間從 call stack 中彈出，恢復執行 a 函式執行背景空間。

![](https://i.imgur.com/WpiFoKg.png)


接著繼續按下一步，因函式 a 中，在呼叫函式 b 之後，已經無其他程式碼需要被執行，所以函式 a 也執行完畢，a 函式執行背景空間彈出 call stack，恢復執行全域執行背景空間。

![](https://i.imgur.com/MgZCMNx.png)

<br>

因全域執行背景空間中，第 10 行程式碼還沒被執行(再次呼叫函式 a)，所以繼續按下一步，會發現剛剛的過程又會重複一遍。

<br>
<br>

除了 call stack 之外， Web API、Task Queue、Event Loop 也扮演著重要的角色，推薦參考影片 [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)，該影片將 JS 引擎的運作流程解釋得非常淺顯易懂。

<br>

執行背景空間(execution context) 就簡單的介紹到此，在這篇文章中，瞭解到 JavaScript 引擎如何透過執行背景空間來追蹤程式碼的執行，下一篇將繼續介紹 JavaScript 如何透過字彙環境(lexical environment)來追蹤變數和函式的作用範圍。

<br>
<br>
<br>
<br>

<b>參考資料</b>
1. [忍者：JavaScript開發技巧探秘第二版](https://www.books.com.tw/products/0010773867)
2. [MDN - closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
3. [JavaScript: Understanding the Weird Parts](https://www.youtube.com/watch?v=Bv_5Zv5c-Ts)
4. [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)









