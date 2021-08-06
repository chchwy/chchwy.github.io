---
date: 2021-08-06
title: "實用 CMake 入門：跨平台 C++ 專案建置"
tags: [CMake]
isCJKLanguage: true
draft: true
---

## 為什麼需要 CMake?

簡單講，C++ 語言本身雖然跨平台。但是編譯 C++ 的工具鏈卻不跨平台。

像是 Linux Makefile 和 Windows Visual Studio 互不相通，即使撰寫了完全平台獨立的程式碼，想在 Linux 上編譯 Windows 專案往往相當困難。

針對這個問題，除了同時維護兩套專案之外，還可以考慮 CMake 這類跨平台工具來解決。

藉由 CMake ，我們可以寫一份完全中立、跟平台無關的專案腳本，需要編譯時才轉換成當下平台的工具鏈。比如當我想在 Windows 平台上編譯時，CMake 就產生一個 Visual Studio 專案。同理，當我需要在 macOS 上編譯時，就產生 Xcode 專案。

CMake 是一個節省時間精力的好物，避開每個平台都需再學習一次建構系統的成本。

## Hello CMake!

馬上開始第一個 CMake 專案。

在專案目錄下創建兩個檔案：`CMakeLists.txt`、`main.cpp`。

CMake 規定，專案腳本的入口一定叫做 `CMakeLists.txt`。不用說 `main.cpp` 就是 C++ 原始碼。

給 `CMakeLists.txt` 添加內容：
```cmake
cmake_minimum_required(VERSION 3.10) # 設定最低版本要求
project(HelloCMake)                  # 專案名稱
add_executable(MyApp main.cpp)       # 執行檔名叫 MyApp，原始碼 main.cpp
```
三行完成一個最簡單的 CMake 專案。原始碼只有 main.cpp，編譯後會產生一個可執行檔 MyApp.exe。

## 編譯

該怎麼編譯專案呢? 我們剛剛提過 CMake可以生成本機專案。用以下指令生成真正可編譯的專案：

```cmd
mkdir build   >> 建立名為 build 子目錄
cd build      >> 進入子目錄
cmake ..      >> 讀取 CMakeLists.txt 並生成本機專案
```

我的開發環境是 Windows 10 + Visual Studio 2019 的組合，所以 CMake 產生了 VS2019 專案。

看一眼 build 子目錄就能看見生成的 VS2019 專案：

![CMake VS2019](/img/cmake-vs.png)

`cmake` 命令的第一個參數是給 `CMakeLists.txt` 所在的路徑，CMake 解析腳本後會在當前目錄輸出專案檔。以本例子來說，CMakeLists.txt 和原始碼位於 build 的上層目錄 (`..`)，而自動產生的 VS2019 檔案就在子目錄 `build` 內。

這是 CMake 的慣例，將產生的檔案和原始專案區分開來，不污染原始專案。一旦該子目錄放進版控忽略清單後 (如 `.gitignore`) 就不會影響版控，相當方便。

最後打指令 `cmake --build .` 編譯程式。

當然，徑直打開 Visual Studio 或者任何對應的 IDE 也可以，不一定要透過 CMake 編譯。

## CMakeLists.txt 範例

一個比較完整的 CMake 範例：
```cmake
cmake_minimum_required(VERSION 3.16)

project(MyProject)          # 專案名稱
set(CMAKE_CXX_STANDARD 11)  # 要求 C++11 標準

add_executable(MyExe main.cpp work.cpp header.h pch.h)    # 執行檔和原始碼
include_directories("C:/path/to/include")                 # 設定 include 目錄
target_link_libraries(MyExe "C:/path/to/lib")             # 設定 lib 連結
```
這個範例示範了一些東西，包括多個標頭檔跟原始檔，指定 C++ 版本標準，引用第三方程式庫的 include 路徑和 link 路徑。應該足以應付大多數的簡單需求。

注意 include_directories() 和 target_link_libraries() 要給絕對路徑

```cmake
include_directories("D:\freeglut\include")
target_link_libraries(Target1 "D:\freeglut\lib\freeglut.lib)
```


## Debug/Release 編譯組態

編譯時用 `--config` 指定編譯組態
```cmd
cmake --build . --config Release
cmake --build . --config Debug
```

可指定 `Debug`, `Release`, `RelWithDebInfo`, `MinSizeRel`。

## 指定編譯器

生成專案時，有多個編譯器可選的話，用 `-G` 指定編譯器
```cmd
cmake -G "Visual Studio 16 2019" /path/to/source
cmake -G "Visual Studio 15 2017" /path/to/source
cmake -G "MinGW Makefiles"       /path/to/source
```

參考連結：[所有可用編譯器](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#manual:cmake-generators(7))

## 指定 32/64-bits

用 `-A` 指定架構
```cmd
cmake -G "Visual Studio 16 2019" -A Win32 /path/to/source
cmake -G "Visual Studio 16 2019" -A x64   /path/to/source
cmake -G "Visual Studio 16 2019" -A ARM   /path/to/source
```

## 視窗程式

如果寫 Win32 視窗程式，需要特別注意一些事項：
```cmake
cmake_minimum_required(VERSION 3.16)

project(MyWindowApp)        # 專案名字是 MyWindowApp
set(CMAKE_CXX_STANDARD 11)  # 指定 C++11 標準

add_compile_definitions(UNICOD _UNICODE)  # 寬字元設定
add_executable(MyApp WIN32 main.cpp work.cpp header.h pch.h) # 注意關鍵字 WIN32

target_precompile_headers(MyApp PRIVATE pch.h)
```

- 用 `WIN32` 關鍵字指明是視窗程式，這樣程式入口才會變成 `WinMain()`
- 定義 `UNICODE` 和 `_UNICODE` 告訴 VC++ 怎麼處理寬字元
- `target_precompile_headers()` 預編譯標頭檔 (放 `#include<windows.h>` 好用) 

## 編譯後複製資源 (如 shader)

我自己寫 Diret3D 程式時，會希望每次編譯後都複製 shaders 到執行檔目錄。以下 CMake 腳本可以達成：

```cmake
add_custom_target(
    copy_shader_files
    ${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/shaders.hlsl ${CMAKE_BINARY_DIR}
)
add_dependencies(Target1 copy_shader_files)
```

## 參考連結

- [CMake 官方教學](https://cmake.org/cmake/help/latest/guide/tutorial/)
- [CLion Quick CMake tutorial](https://www.jetbrains.com/help/clion/quick-cmake-tutorial.html)
- [An introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/)
- [Qt5 Projects with CMake](https://gist.github.com/Rod-Persky/e6b93e9ee31f9516261b)
- [用CMake在建置時複製檔案到輸出執行檔的目錄 ][CopyShader]

[CopyShader]: https://minecraftxwinp.github.io/2017/11/27/%E7%94%A8CMake%E5%9C%A8%E5%BB%BA%E7%BD%AE%E6%99%82%E8%A4%87%E8%A3%BD%E6%AA%94%E6%A1%88%E5%88%B0%E8%BC%B8%E5%87%BA%E5%9F%B7%E8%A1%8C%E6%AA%94%E7%9A%84%E7%9B%AE%E9%8C%84/