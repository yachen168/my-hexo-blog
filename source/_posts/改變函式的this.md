---
title: 三種改變函式內部 this 的方式：apply()、call()、bind()
tags: 
      - JavaScript
      - w3HexSchool
date: 2020-06-04 17:29:48
categories: web
toc: true
thumbnail: ./images/cover-js.png
---


在 JavaScript 中，改變函數內部 this 的指向常見的方式有 `call()`、`apply()`、`bind()`，而這三種方式又存在些微差異。

<!-- more -->

<br>

## call()
`call()` 方法會`呼叫(執行)一個函數`，且可以同時改變函數內部的 `this` 指向。

語法：
```javascript=
fun.call(thisArg ,arg1, arg2, ...);
```
參數說明：
- **thisArg**：在函式運行時的 `this` 指向，若省略則僅執行函式而不改變 `this` 指向。
- **arg1, arg2, ...**：傳遞的參數(可省略)。
<br>

例如：

在非嚴格模式下，函式 foo 中的 `this` 指向的原本是 `window`，因在此例子中，`foo` 其實是屬於 `window` 物件的一個方法。(若在嚴格模式下，則為 `undefined`)

```javascript=
var obj = {
    name: 'yachen'
}

function foo(){
    console.log(this);
}   

foo();  // window，其實這裡的 foo() 是 window.foo()
```
<br>

剛提到，`call()` 會呼叫(執行)函式，但若無給第一個參數，則僅會呼叫函式而不會改變 `this` 指向，所以除了用 `foo()` 執行 `foo` 函式之外，也可以透過 `call()` 來呼叫(當然不需要多此一舉)。
```javascript=
foo.call();  // window
```

<br>

現在除了呼叫 foo 函式，同時想要將函式 foo 裡的 `this 指向改為 obj`，可以使用 `call()` 來達成。

```javascript=
foo.call(obj);  // obj
```
<br>


還可以傳遞一些參數。

```javascript=
var obj = {
    name: 'yachen'
}

function foo(a, b){
    console.log(this);
    console.log(a + b);
}   

foo.call(obj, 1, 2);     
// obj 
// 3
```


<br>

### 實現繼承

**`call()` 也可以用來實現繼承。**

```javascript=
function Person(name, age){
    this.name = name;
    this.age = age;
    this.say = function(){
          console.log(`${this.name} is ${this.age} years old`);
}};

function Taiwanese(name, age){
    // this 指向 me
    Person.call(this, name, age);
}

var me = new Taiwanese('yachen', 18);

me.say();  //  "yachen is 18 years old"
```

<br>
<br>

## apply()

使用 `apply()` 會呼叫一個函數，同時可以改變函數內部 `this` 指向，`apply()` 與 `call()` 幾乎一樣，最大的不同是 `call()` 接受一連串的參數，而 `apply()` 接受一組陣列(或類陣列)形式的參數。


<br>

語法：
```javascript=
fun.apply(thisArg, [argsArray]);
```
參數說明：
- **thisArg**：在函式運行時的 `this` 指向。
- **[argsArray]**：傳遞的參數必須為==陣列或類陣列==(可省略)。



<br>
<br>



 ## bind()
 `bind()` 能改變 `this` 指向，但`不會呼叫`(執行)函數，會 copy 並返回一個新的函數。

<br>


語法：
```javascript=
fun.bind(thisArg, arg1, arg2, ...)
```
參數說明：
- **thisArg**：在 fun 函數運行時指定的 `this` 值。
- **arg1, arg2, ...**：傳遞其他參數(可省略)。

<br>


例如：

使用 `bind()` 將函式 foo 裡的 `this` 指向改為 `obj`，同時傳遞 `1` 和 `2` 兩個引數，有別於 `call()` 與 `apply()`，`bind()` 並不會呼叫執行 `foo` 函式，僅會返回一個新的函式。
```javascript=
var obj = {
    name: 'yachen'
}

function foo(a, b){
    console.log(this);
    console.log(a + b);
}

foo.bind(obj, 1, 2);   // 並未執行，而使返回一個新的函式
```
<br>

因 `bind()` 並不會呼叫執行 foo 函式，而是返回一個新的函式，若想要同時執行 `foo`，則需手動加上 ()。

```javascript=
foo.bind(obj, 1, 2)();  
// obj
// 3
```

<br>

現在來看看返回的新函式長什麼樣子，可以打開瀏覽器，並輸入以下代碼。
```javascript=
var obj = {
    name: 'yachen'
}

function foo(a, b){
    console.log(this);
    console.log(a + b);
}

var newFoo = foo.bind(obj, 1, 2); // 並未執行，而使返回一個新的函式
console.log(newFoo);
```
印出 `newFoo`：

![](https://i.imgur.com/cOC7AnG.png)


<br>

咦？ `newFoo` 不就是 `foo` 嗎？

再試著輸入以下代碼，事實證明 `newFoo` 不等於 `foo`。

```javascript=
console.log(newFoo === foo);   // false
```





<br>
<br>

在開發中，最常使用的應該是 `bind()`，因為很多情況下我們只是想改變 `this`，不想同時執行函數。

例如現在有一個按鈕，想要在用戶點擊後，禁用此按鈕 3 秒。

```htmlmixed=
<button>按鈕</button>
```
<br>

若直接在定時器裡使用匿名 `function(){}` 形式的 callback，定時器裡的 `this` 指向的是 `window`。 

```javascript=
const btn = document.querySelector('button');
btn.addEventListener('click', eventHandler);

function eventHandler(){
    // this 指向按鈕 btn
    this.disabled = true;
    
    setTimeout(function(){
        // this 指向 window
        this.disabled = false;
    },3000)
}
```

<br>

這時候 `bind()` 就可以派上用場了，因為不想要在改變 `this` 的同時執行 `this.disabled = false`，而是希望透過定時器 3 秒後執行，此時就不適用 `call()` 與 `apply()`。

```javascript=
const btn = document.querySelector('button');
btn.addEventListener('click', eventHandler);

function eventHandler(){
    // this 指向按鈕 btn
    this.disabled = true;
    
    setTimeout(function(){
        this.disabled = false;
    }.bind(this),3000)  // this 改指向按鈕 btn
}
```

<br>

當然，也可以直接將定時器裡的 callback function 改成`箭頭函式(arrow function)`的形式來解決。

```javascript=
const btn = document.querySelector('button');
btn.addEventListener('click', eventHandler);

function eventHandler(){
    this.disabled = true;
    
    // 改為箭頭函式
    setTimeout(()=>{
        // this 指向 btn
        this.disabled = false;
    },3000)
}
```


<br>
<br>
 
 ## 總結
 `call()`、`apply()`、`bind()` 皆可以改變函數內部的 `this` ，三者的差異在於：
 
 

|         | 自動呼叫函數 |    參數(可省略)     |
| ------- |:------------:|:-----------:|
| call()  |     ⭕️      |  arg1, arg2, ...  |
| apply() |     ⭕️      | [arg1, arg2, ...] |
| bind()  |     ❌      |  arg1, arg2, ...  |

 


<br>






<br>
<br>
<br>
<br>

參考資料：
1. [MDN - Function.prototype.call](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
2. [MDN - Function.prototype.apply()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
3. [MDN - Function.prototype.bind()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
4. [MDN - setTimeout](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout)