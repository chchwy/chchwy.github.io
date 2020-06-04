---
date: 2020-06-04
title: "Github Actions 更新部落格"
tags: [Blog]
isCJKLanguage: true
draft: true
---

現在部落格是 Hugo + Github Pages 的組合。

Hugo 這類靜態網站產生器的問題，就是一旦更新了內容，就要重新編譯上傳。
最早我會自己手動編譯上傳，但是每次小修改都要打一堆指令，太麻煩，反而讓我變得不愛寫部落格。

後來設定了 Travis CI，只要 Github Repo 有推送就會自動重新編譯和更新。只是需要兩個 Repo，一個保存原始的 markdown 檔案，一個保存編譯後的結果。

現在有了 Github Actions，可以直接在 Github 上一站搞定所有的事情:

1. 原始文件和編譯結果都保存在同一個 git repo 裡，只是不同 branch。原始文件放在 `blog` 分支，編譯結果放在 `master` 分支。
2. 設定
