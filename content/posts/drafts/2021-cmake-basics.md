---
date: 2021-03-10
title: "CMake 極簡教學: 建立基本 Windows 視窗程式"
tags: [CMake]
isCJKLanguage: true
draft: true
---

CMake 是一個跨平台的編譯工具，廣泛應用在許多 C++ 專案裡。這篇文章是我學習 CMake 的紀錄。

## 為什麼需要 CMake?

簡單講，當 C++ 專案脫離學校作業的等級，從兩三個 cpp 成長到成千上百個檔案之後，直接用 g++ 編譯程式就變得相當不切實際，需要工具來管理整個專案編譯的流程，設定編譯參數，管理外部依賴等等。

大部分人在學 C++ 時可能已經用過某些建構系統，像是 Linux 的 Makefile 或者 Visual Studio (底層是 MSBuild)。問題是這些建構系統互不相通，所以即使撰寫了完全平台獨立的程式碼，要在 Linux 上重新編譯 Windows 專案依舊困難重重。

CMake 的跨平台特性，剛好可以解決這個問題。

CMake 專案可以轉換成目標平台的專案再編譯。簡單講，CMake 是一個間接工具，先寫一份 CMake 專案腳本，當我想在 Windows 平台上編譯時，CMake 就轉換成 Visual Studio 專案。同理，當我想在 macOS 上編譯時，就可以轉換成 Xcode 專案。

即使不跨平台，CMake 也是一個好工具，因為 CMake 腳本容易編寫，純文字格式方便版本管理，而且節省學習成本，不需要每個平台再學習一次建構系統。

## Hello CMake!

第一步，在專案目錄下建立一個檔案名叫 `CMakeLists.txt` 檔案，這是 CMake 的規定。所以如果看到某專案目錄裡有這個檔案，你就知道它用了 CMake。

下一步就是編輯 `CMakeLists.txt` 的內容：

```cmake
cmake_minimum_required(VERSION 3.10) # 設定最低版本要求
project(HelloCMake)                  # 專案名稱
add_executable(HelloCMake main.cpp)  # 輸出執行檔叫做 HelloCMake 輸入原始碼 main.cpp
```

這三行設定了一個最基本簡單的 CMake 專案。版本要求、專案名稱。輸入的原始檔和輸出的執行檔。

那要怎麼編譯這個 Cmake 專案呢? 用以下指令生成目標平台的專案檔

```cmd
mkdir build  # 在專案目錄下建立一個 build 子目錄
cd build     # 進入子目錄
cmake ../    # 呼叫 CMake 指令，參數是頂層 CMakeLists.txt 的所在位置
```

我的環境是 Windows 10 + Visual Studio 2019 的組合，所以 CMake 生出了 VS2019 的專案檔:

```cmd
D:\HelloCMake\build>cmake ..
-- Building for: Visual Studio 16 2019
-- The CXX compiler identification is MSVC 19.28.29910.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: C:/Program Files (x86)/Microsoft Visual Studio/2019/Community/VC/Tools/MSVC/14.28.29910/bin/Hostx64/x64/cl.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: D:/HelloCMake/build
```

如果到子目錄裡頭去看一眼，
CMake 的慣例是切割源碼目錄和建構目錄。所有編譯產生的中間檔都會跑去 build 子目錄，保持原本的專案目錄乾淨。





[0]: https://gist.github.com/Rod-Persky/e6b93e9ee31f9516261b