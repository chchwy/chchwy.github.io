---
date: 2021-03-10
title: "CMake 極簡教學: 建立基本 Windows 視窗程式"
tags: [CMake]
isCJKLanguage: true
draft: true
---

CMake 是一個跨平台的建構系統，廣泛使用在 C++ 領域裡。

## 為什麼需要 CMake?

為什麼需要建構系統？簡單講，當 C++ 專案成長到某個數量級，比如說上百個 .h/cpp 檔案之後，直接下 g++ 命令來編譯程式就變得有點不切實際，需要有某種工具來管理專案編譯的流程，設定編譯參數，管理外部依賴等等。

大部分人在學 C++ 時很可能已經用過一些建構系統，像是 Linux 的 Makefile 或者 Visual Studio (底下使用 MSBuild)。但是這些建構系統格式互不相通，所以即使程式碼是完全平台獨立的，但是要在 macOS 上重新編譯一個 Windows 專案就是困難重重。

而 CMake 的跨平台特性，剛好可以解決這個問題。

CMake 跨平台的方法，就是透過轉換，把 CMake 腳本轉換成特定平台的專案格式。簡單講，CMake 是一個間接工具，只要寫一份 CMake 腳本，當我想在 Windows 平台上編譯時，CMake 就幫我把腳本轉換成 Visual Studio 專案。同理，當我想在 macOS 上編譯時，就轉換成 Xcode 專案。 

## Hello CMake!

我已經用 CMake 編譯過好多開源專案了，但是從來沒有自己寫過