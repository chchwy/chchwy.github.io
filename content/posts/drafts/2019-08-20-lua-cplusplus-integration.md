---
date: 2019-08-20
isCJKLanguage: true
title: "Lua/C++ 整合筆記"
tags: [Lua, C++]
draft: true
---

Lua 語言是門輕巧的直譯式動態語言。它的與眾不同之處在於 Lua 語言很少單獨存在，通常是作為一個腳本引擎，依附在現有的大型 C/C++ 軟體系統上。
最有名的例子要算是魔獸世界 WOW 的 UI 擴展系統了。魔獸世界的遊戲本體是用 C++ 寫成的，但是遊戲同時提供了 Lua 腳本引擎和一系列的 Lua API，玩家可以藉由撰寫 Lua 腳本來客製化自己的 UI 界面。這些 Lua API 會呼叫預先綁定好的 C++ API 來驅動遊戲系統。
因為 Lua 腳本不需要事先編譯，可以隨寫隨跑。這讓魔獸世界達成了一個相當了不起的成就：玩家們不需要取得 C++ 源始碼，也不需要重新編譯遊戲程式，就能自己發揮創意，把 UI 系統修改成自己想要的樣子。我記得以前有一個非常有名的插件叫做 "Auctioneer"，大幅強化了遊戲內建的拍賣場介面，如果在魔獸世界裡面要搞大量買賣，這個插件是必不可少的。
不只是遊戲，其他大型系統也是一樣。大型 C/C++ 系統編譯動輒耗費數十分鐘，開發效率很差。如果把系統中不常變動的核心部份用 C++ 來實現，然後時常變動的業務部份用 Lua 實現，就能取得開發效率跟執行效率的平衡。

## 整合 Lua 腳本引擎

要在合 Lua 相當容易。