---
title: 手把手帶你架 hexo 部落格
tags: test
date: 2020-02-14 15:05:36
---


### 在 github 開一個新的 repository
如果希望以後自己的 hexo 網址為 <自己的 GitHub 用戶名>.github.io，那麼 repository 應該直接命名為
```
<自己的 GitHub 用戶名>.github.ioｃ
```

例如若我的 github 用戶名為 yachen16888，那麼新建立的 repository 就命名為 yachen16888.github.io。

![repository 命名](./hexo/newRepo.png)

<br>
<br>

### 安裝 Hexo

在安裝 hexo 之前，先確認自己的電腦是否已經安裝好：
- [Node.js](https://nodejs.org/en/) (Node.js 版本需不低於8.10，建議使用 Node.js 10.0 及以上版本)

- [Git](https://git-scm.com/)

Hexo 的安裝需要用到 npm，而 npm 與 Node.js 綁在一起，下載 Node.js 的同時就會有 npm。

接著透過 npm 全域(-g)安裝 Hexo，在終端機輸入指令：

```
npm install -g hexo-cli
```

### 建立 Hexo

```
hexo init 資料夾名稱
```
接著進入該資料夾，輸入指令：

```
hexo cd 資料夾名稱
```
安裝 npm 相關套件
```
npm install
```

```
hexo s
```






<br>
<br>
<br>


參考資料
[hexo 官方文件](https://hexo.io/zh-tw/docs/)


