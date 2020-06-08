---
title: 解決 Vue.js-devtools 無法開啟問題
categories: web
date: 2020-06-09 01:18:59
tags: Vue
thumbnail: ./images/cover-vue.png
---

相信用過 Vue.js 的應該都有裝 Vue.js-devtools (吧)。

最近發現若是單純以引入 `CDN` 的方式使用 Vue，而非用 [Vue CLI](https://cli.vuejs.org/zh/)，會出現 [Vue.js-devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) 無法正常開啟的情況。

<!-- more -->

![](./vue-devtools無法開啟/devtools.png)

<br>

查了一下，解決辦法是需要手動進行設定，需要在 js 檔案中添加以下程式碼：
```javascript=
Vue.config.devtools = true;
```

<br>

像這樣：

```javascript=
Vue.config.devtools = true;   // 手動添加這行程式碼

Vue.component('todo-item', {
  template: '<li>...</li>'
})

var app = new Vue(...)
```
<br>


然後重新開啟 live server，就可以看到 `Vue.js-devtools` 正常運作囉！

![](./vue-devtools無法開啟/success.png)
![](./vue-devtools無法開啟/success-open.png)




<br>
<br>
<br>
<br>
<br>

**參考資料** <br>
[GitHub issues](https://github.com/vuejs/vue-devtools/issues/190)
