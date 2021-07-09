---
date: 2021-07-05
isCJKLanguage: true
title: "OpenSSH vs PuTTY: 搞清楚兩種 SSH 工具的差異"
tags: [開發日常]
draft: true
---

> 本篇文章是專門為了 Windows 系統而寫的

## Git 身份認證

Git 提供了帳號密碼以外的另一種驗證身份的方式，叫做 SSH。

從 git 網址可以看出這兩種身份驗證方法：

```bash
https://github.com/pencil2d/pencil.git   # https:// 開頭的網址就是用密碼驗證
git@github.com:pencil2d/pencil.git       # git@ 開頭的網址就是走 SSH 驗證
```

作為另一種身份驗證工具，SSH 連線有相當多優點：不需要每次 push 時重打密碼 (或者把密碼存在電腦某處)。還有私鑰不容易被破解的特性。即使私鑰被偷了，還有一層驗證密碼，傷害往往比帳號密碼被偷來的小。

缺點呢，就是 SSH 用起來不像普通密碼那樣直觀，需要一些額外設定步驟。這也是我以前不喜歡用 SSH 的原因。

## 為什麼我搞不清楚 SSH

後來發現，我之前有很長時間搞不懂 SSH，是因為 Windows 作業系統上有兩套設定 SSH 的方法，一個叫做 PuTTY，一個叫 OpenSSH。這兩個方法雖然底層原理相同，但是工具名稱不一樣，私鑰的儲存格式也不一樣，互不相通。我在網路上搜尋教學，總是看的迷迷糊糊，一下這個一下那個，前後兜不起來。直到我自己親手把這兩種 SSH 設定方式各跑過一遍，才搞明白是怎麼回事。

## 兩套 SSH 工具簡介

- 第一種叫 PuTTY
- 第二種叫 OpenSSH

很多年以來，[PuTTY][0] 都是 Windows 上最多人用的老牌 SSH 連線軟體。TortoiseGit 小烏龜預設的就是走 PuTTY 格式。[OpenSSH][1] 則是由 BSD/Linux 世界開發維護，一直到 Win10 1809，微軟才把它帶入 Windows 系統內。

這兩套工具，各自有命令跟軟體來處理 SSH 連線。

[0]: https://www.putty.org/ "Putty website"
[1]: https://www.openssh.com/ "OpenSSH website"

## 產生金鑰

SSH 連線的第一步就是產生金鑰。

Putty 產生金鑰的程式叫做 **PuTTYgen**，OpenSSH 是 **ssh-keygen** 命令。

PuTTYgen 是一支視窗程式，打開來按 Generate 就可以產生金鑰。

Putty 私鑰檔的副檔名為 **.ppk**。OpenSSH 私鑰則沒有副檔名，但不知為何，有個慣例命名私鑰為 `id_rsa`，公鑰則叫做 `id_rsa.pub`，但是實際上你可以隨意取名。

值得一提的是，PuTTYgen 可以雙向轉換金鑰格式，把 ppk 私鑰轉成 OpenSSH 格式，或者從 OpenSSH 格式轉回來。方式：主選單 -> **Conversions** -> **Import Key** 或 **Export OpenSSH Key**

## 啟動 SSH 服務

```bash
pageant.exe key.ppk
```

```bash
ssh-add ~/.ssh/id_key
```

## 開機常駐 SSH agent 的方式

寫一支 .bat 檔

寫 PowerShell 

## 連線

GIT_SSH
