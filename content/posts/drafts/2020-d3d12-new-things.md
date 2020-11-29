---
date: 2020-09-01
title: " D3D12 新觀念"
tags: [DirectX, D3D12]
isCJKLanguage: true
draft: true
---

這裡紀錄我學習 D3D12 (或者叫 DirectX 12) 中學到新概念。

D3D12 是微軟新一代的圖形 API，和 Vulkan、Metal、WebGPU 屬同一個世代。

這一代 API 的特色就是為了高效能，所以 API 刻意曝露了出更多的細節，更「低階」。好處是程式設計師有更大的權力，可以控制更多的細節，但是壞處就是「權力越大，責任越大」，

這樣子顯示卡製造商就可以甩鍋了。

1. Command Queue & Command List
2. CPU/GPU syncrhonization & Fence
3. Root Signature
4. Pipeline State Object
5. Resource & Resource Heap
6. Descriptor & Descriptor Heap


