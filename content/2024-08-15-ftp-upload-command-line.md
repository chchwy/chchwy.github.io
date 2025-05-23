+++
date=2024-12-15
title="命令列上傳檔案到FTP"
draft = false
[taxonomies]
tags = ["開發日常", "code", "Troubleshooting"]
+++

最近需要在 CI 流程中上傳檔案到 FTP 伺服器，順手紀錄一下。

## 方法一：使用 cURL 上傳 FTP

原來萬能瑞士刀 `curl` 可以操作FTP檔案上傳。

在 Windows 上使用 `curl` 指令上傳檔案到 FTP 伺服器的範例：

```bat
curl -T "YOUR_LOCAL_FILE"^
    --user "FTP_USER:FTP_PASSWORD"^
    "ftp://192.168.99.99/Your/Directory/UPLOADED_FILE"^
    --ftp-create-dirs
```

這段指令的意思是：

- `-T YOUR_LOCAL_FILE` 指定要上傳的本地端檔案
- `--user` 帳號密碼
- `ftp:/SERVER_IP/Your/Directory/UPLOADED_FILE` 指定伺服器的 IP 位址以及上傳路徑
- `--ftp-create-dirs` 如果上傳的目的地路徑中有缺任何目錄，自動建立目錄

## 方法二：Windows 內建的 ftp 指令

原來 Windows 作業系統本身內建了 `ftp` 指令，可以操作 FTP 上傳下載。

這個 `ftp` 指令原本是互動式的，但是它也可以接收一個指令檔，裡面預先寫好一串操作步驟，適合 CI 使用。

比如說，我想要上傳檔案到 IP 位址為 192.168.99.99 的 FTP 伺服器，以下就是操作範例：
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

用`echo`指令把每個FTP操作步驟寫入`ftp_commands.txt`檔案，一行一個指令：
- `open IP` 連線到 FTP 伺服器，
- 接下來兩行是帳號密碼，用環境變數`%FTP_USER%` `%FTP_PASSWORD%`保護敏感資訊
- `binary` 設定傳輸模式為二進位模式
- `cd` 切換到指定目錄
- `send` 指令執行檔案上傳
- `bye` 最後關閉連線。

讀取指令檔並執行: `ftp -d -s:ftp_commands.txt` <br/>
清理檔案 `del ftp_commands.txt`

## 結論

這兩個方法都可以用於 FTP 檔案上傳。`ftp` 是遠古時代就存在的指令，`cURL` 則在Windows 10 17063之後成為作業系統的預裝指令之一，所以兩者都內建。

跨平台方面：`ftp`指令是Windows限定，`curl`可以跨平台。

我最終採用 curl，指令更簡潔，且 Windows & macOS 可以用同一招上傳。




