+++
date=2024-12-15
title="用命令列操作FTP上傳檔案"
draft = false
[taxonomies]
tags = ["開發日常", "code", "Troubleshooting"]
+++

最近碰到一個需求，要在 CI 流程的最後一步將檔案上傳到 FTP 伺服器上。我研究了一下，發現有好幾種作法，順手紀錄一下我試過可行的兩招。

## 方法一：Windows內建的ftp指令

原來 Windows 作業系統本身內建了 `ftp` 指令，可以操作 FTP 上傳下載。

這個 `ftp` 指令原本是互動式的，但是它也可以接收一個指令檔，檔案裡預先寫好一連串操作步驟，非常適合給 CI 使用。

比如說，我想要上傳檔案到 IP 位址為 192.168.99.99 的 FTP 伺服器：
```bat
echo open 192.168.99.99> ftp_commands.txt
echo %FTP_USER%>> ftp_commands.txt
echo %FTP_PASSWORD%>> ftp_commands.txt
echo binary>> ftp_commands.txt
echo cd /Your/Server/Directory>> ftp_commands.txt
echo send YOUR_LOCAL_FILE>> ftp_commands.txt
echo bye>> ftp_commands.txt

ftp -d -s:ftp_commands.txt
del ftp_commands.txt
```

拆解這段程式碼的運作：

首先用`echo`指令把每個FTP操作步驟寫入`ftp_commands.txt`檔案，一行一個指令，就像自動化腳本：
- `open IP` 連線到 FTP 伺服器，
- 接下來兩行是帳號密碼，用環境變數`%FTP_USER%` `%FTP_PASSWORD%`保護敏感資訊
- `binary` 設定傳輸模式為二進位模式
- `cd` 切換到指定目錄
- `send` 指令執行檔案上傳
- `bye` 最後關閉連線。

讀取指令檔並執行: `ftp -d -s:ftp_commands.txt` <br/>
清理檔案 `del ftp_commands.txt`

## 方法二：使用 cURL

萬能瑞士刀 `curl` 也可以操作FTP上傳：

```bat
curl -T "YOUR_LOCAL_FILE"^
    --user "FTP_USER:FTP_PASSWORD"^
    "ftp://192.168.99.99/Your/Directory/UPLOADED_FILE"^
    --ftp-create-dirs
```

這個指令做的事情是一樣的：
- `-T YOUR_LOCAL_FILE` 指定要上傳的本地端檔案
- `--user` 帳號密碼
- `ftp:/SERVER_IP/Your/Directory/UPLOADED_FILE` 指定伺服器 IP 位址以及上傳路徑
- `--ftp-create-dirs` 如果上傳路徑中缺了任何目錄，自動建立目錄 

## 結論

這兩個方法都可以用於 FTP 檔案上傳。`ftp` 是遠古時代就存在的指令，`cURL` 則在Windows 10 17063之後成為作業系統的預裝指令之一，所以兩者都內建。

跨平台方面：`ftp`指令是Windows限定，`curl`可以跨平台。

我最終採用 curl，指令更簡潔，且 Windows & macOS 可以用同一招上傳。




