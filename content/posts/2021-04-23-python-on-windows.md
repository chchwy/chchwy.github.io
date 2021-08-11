---
isCJKLanguage: true
date: 2021-04-23
title: "Windows 電腦切換 Python 2/3 版本"
tags: [python]
---

目前看來，Python 2 和 3 應該還會共存相當長一段時間，所以電腦上免不了要同時安裝兩個版本。以下是我找到 Windows 作業系統下可以輕鬆切換版本的方法。

## py Launcher

Python 3.3 開始內建 `py` 啟動器，可用參數選擇 Python 版本。

py 指令使用方法如下:
```shell
py -2 myscript2.py # 指定 Python 2
py -3 myscript3.py # 指定 Python 3
```

列出所有已安裝的 Python 版本
```shell
py --list
```

安裝模組
```shell
py -2 -m pip install SomePackage  # 指定 Python 2
py -3 -m pip install SomePackage  # 指定 Python 3
```

## 在檔案中指定版本

使用 py 啟動器的話，也可以在 Python 腳本檔案的第一行加入以下語句來指定版本
```py
#! python2.7
#! python3
```

這樣 py 啟動器就不用下版本參數，會根據檔案第一行啟動對應的版本
```shell
py myScript.py # 使用檔案裡指定的版本
```

## 原始碼偵測語言版本

在 Python 腳本裡面偵測目前語言版本的方法: 

```py
>>> import sys
>>> print(sys.version_info)
sys.version_info(major=2, minor=7, micro=18, releaselevel='final', serial=0)
```

## 參考連結

- [Python 官方文件][1],
- [知乎：在同一台电脑下如何进行 Python 2 与 3 的切换？][2]

[1]: https://docs.python.org/3/installing/index.html#install-scientific-python-packages
[2]: https://www.zhihu.com/question/22846291/answer/22928449
