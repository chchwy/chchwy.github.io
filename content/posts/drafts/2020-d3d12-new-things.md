---
date: 2020-09-01
title: " D3D12 新觀念"
tags: [DirectX, D3D12]
isCJKLanguage: true
draft: true
---

這裡紀錄我學習 D3D12 (或者叫 DirectX 12) 中學到新概念。

D3D12 是微軟新一代的低階圖形 API，和 Vulkan、Metal、WebGPU 屬同一個世代。

這一代 API 的特色就是更「低階」，更接近硬體。好處是賦予程式設計師更大的權力，充分的利用各種硬體資源，榨出更多效能。
但是壞處就是「權力越大，責任越大」，程式設計師需要掌握的細節變多了，需要負擔的責任也變多了。（相對來講，顯示卡驅動負責的事情就變少了，以後就不能把 BUG 推給顯示卡驅動了，陰謀阿！)

1. Command Queue & Command List
2. CPU/GPU syncrhonization & Fence
3. Root Signature
4. Pipeline State Object
5. Resource & Resource Heap
6. Descriptor & Descriptor Heap


