---
date: 2020-06-04
title: "用 Github Actions 更新部落格"
tags: [Blog]
isCJKLanguage: true
draft: true
---

現在我的部落格是 Hugo + Github Pages 的組合。

跟一般的部落格平台不一樣，使用 Hugo 這類靜態網站產生器，內容更新了需要全站重新編譯。

更新部落格分成三個時期：

1. 自己手動編譯，但是每次小修改都要打指令，太麻煩了，反而讓我變得不愛寫部落格。
2.  Travis CI，只要 Github Repo 有推送就會自動重新編譯。只是需要兩個 Git Repo，一個保存原始 markdown 檔案，一個保存編譯後的結果。

現在有了 Github Actions，直接在 Github 上一站搞定所有的事情:

1. 只用一個 Git Repo，原始文件和編譯結果用 branch 區隔。原始文件放在 `blog` 分支，編譯結果放在 `master` 分支。
2. 設定 Github Actions，只要推送修改，就自動重新編譯。


