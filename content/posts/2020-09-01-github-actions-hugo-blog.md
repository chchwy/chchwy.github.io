---
date: 2020-09-01
title: "用 Github Actions 更新部落格"
tags: [Blog]
isCJKLanguage: true
---

現在我的部落格是 Hugo + Github Pages 的組合。

跟一般的部落格平台不一樣，用 Hugo 這類靜態網站產生器，內容更新了需要全站重新編譯。所以更新部落格就變得稍微有點麻煩。

我怎麼處理部落格更新，總分成三個時期：

1. 自己手動編譯。每次小修改都要打指令，麻煩，讓我變得不太愛寫部落格。
2. [Travis-CI 自動編譯][travisci]。只要 Git Repo 推送就會自動重新編譯，節省我很多力氣。缺點是需要兩個 Git Repo，這個 chchwy.github.io 只保存編譯後的結果，需要另一個 Git 倉庫保存原始 markdown 檔案。
3. 現在。採用 Github Actions 部署。直接在 Github 上一站搞定所有的事情。
    - 只有一個 Git Repo，用 branch 區隔原檔和編譯結果。原檔放 `blog`，編譯結果放 `master`。
    - 任何 git push 都會觸發 Github Actions 自動全站編譯。

[travisci]: https://docs.travis-ci.com/user/deployment/pages/

## 設定 Github Action 

設定步驟如下：

一、先到你的 Git Repo 點選 Action 分頁，然後點 `New workflow`

![](/img/github-action-01.png)

二、接著點選 `set up a workflow yourself`

三、Github 會導引你創建一個 Workflow 設定檔，位於 `.github/workflows/main.xml`。

在文字框填入以下設定 (依個人情況修改適合自己的設定)

```yml
name: "Deploy my blog" # 自己取一個喜歡的名字
on:
  push:
    branches: [ blog ] # 設定哪些 branch 會觸發 Action

jobs:
  build:
    runs-on: ubuntu-latest # 執行環境

    steps:
    - name: Checkout Repo
      uses: actions/checkout@v2

    # 直接引用其他人寫好的 Hugo to Github Pages Action
    - name: "Publish Hugo Site"
      uses: chabad360/hugo-gh-pages@master 
      with:
        branch: "master"
        args: --gc --minify --cleanDestinationDir
        githubToken: ${{ secrets.PERSONAL_TOKEN }}
```

四、Github Action 需要 Access Token，它才可以讀寫你的 Git 倉庫。

照著官方文件的指示 [Creating a Personal Access Token][create-token] 建立 Access Token，記得要給予適當的讀寫權限。

接著到 Github 倉庫的 Setting 分頁，選 Secrets -> New Secrets。給這個 Secret 取名叫做 PERSONAL_TOKEN (基本上就是 workflow 設定檔的最後一行那個變數名)，貼上剛剛建立的 Access Token 字串。 

[create-token]: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token

五、測試 Github Action

推送修改到你的 Git 倉庫，到 Actions 分頁看看有沒有觸發 Workflow

![](/img/github-action-03.png)

看見 workflow 開始執行就表示大功告成，Github Action 設定完畢，可以射後不理盡情寫部落格了。

參考連結: <https://github.com/chabad360/hugo-gh-pages>



