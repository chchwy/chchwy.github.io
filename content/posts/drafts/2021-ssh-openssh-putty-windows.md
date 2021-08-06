---
date: 2021-07-05
isCJKLanguage: true
title: "OpenSSH vs PuTTY: 搞清楚兩種 SSH 工具的差異"
tags: [開發日常]
draft: true
---

## 為什麼我搞不清楚 SSH

後來發現，我之前有很長時間搞不懂 SSH，是因為 Windows 上有兩套設定 SSH 的方法，一個叫做 PuTTY，一個叫 OpenSSH。這兩個方法雖然底層原理相同，但是工具名稱不一樣，私鑰的儲存格式也不一樣，互不相通。我在網路上搜尋教學，總是看的迷迷糊糊，一下這個一下那個，前後兜不起來。直到我自己親手把這兩種 SSH 設定方式各跑過一遍，才搞明白是怎麼回事。

## 兩套 SSH 工具簡介

- 第一種叫 PuTTY
- 第二種叫 OpenSSH

很多年以來，[PuTTY][0] 都是 Windows 上最多人用的老牌 SSH 連線軟體。TortoiseGit 小烏龜預設的就是走 PuTTY 格式。[OpenSSH][1] 則是由 BSD/Linux 世界開發維護，一直到 Win10 1809，微軟才把它帶入 Windows 系統內。

這兩套工具，各自有命令跟軟體來處理 SSH 連線。

[0]: https://www.putty.org/ "Putty website"
[1]: https://www.openssh.com/ "OpenSSH website"

## 啟始設定 SSH

How to Install SSH in Windows 10 (Quick)

Installing SSH functionality to the Windows 10 PowerShell is straightforward enough, but the menu options for it are somewhat hidden. Here's what you'll need to do:

    Open Settings.
    View Apps > Apps & features
    Go to Optional features
    Click Add a feature
    Select OpenSSH Client
    Wait, then reboot

Putty tools download:
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html


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
https://richardballard.co.uk/ssh-keys-on-windows-10/

## 連線

GIT_SSH

git config --global core.sshCommand C:/Windows/System32/OpenSSH/ssh.exe


Reference:
https://blog.alantsai.net/posts/2018/11/faq-start-sshagent-error-1058-on-windows-1803-cannot-use-ssh-agent
https://docs.microsoft.com/zh-tw/windows-server/administration/openssh/openssh_keymanagement