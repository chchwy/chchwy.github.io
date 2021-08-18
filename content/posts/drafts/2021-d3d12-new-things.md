---
date: 2021-04-15
title: "D3D12：新觀念"
tags: [DirectX, D3D12]
isCJKLanguage: true
draft: true
---

紀錄我學習 D3D12 (或者叫 DirectX 12) 中學到的新概念。

D3D12 是微軟的次世代低階圖形 API。

這一代 API 的特色就是更「低階」，給予程式設計師更多細部資源控制的權力。
但是壞處就是「權力越大，責任越大」，顯示卡驅動負責的事情少了，程式設計師需要處理的事就多了，擔負的責任也多了。

主題:

## 一、Command Queue & Command List

別忘了，Graphics 裡面有兩個處理器: CPU & GPU。

工作是由 CPU 發起，交給 GPU 去做。
以前 D3D11 沒有這麼明顯的區別，但是 D3D12 就可以明顯的感受到這個動作。

GPU 有一個命令序列 CommandQueue
透過 Command List 錄製 GPU 命令。然後再丟進 CommandQueue

Command allocator

## 二、CPU/GPU syncrhonization & Fence

最理想的狀態就是 CPU 跟 GPU 都很忙。

- Fence
- Resource Barriers

ID3D12CommandQueue::Signal(fence, n + 1); 

塞入一個指令，叫 GPU 更新柵欄值
之後可以問 GPU 柵欄值就知道跑完了沒

要怎麼問柵欄值? `ID3D12Fence::GetCompletedValue()` 的回傳值

如果要在柵欄到達某個數字 N 的時候通知 CPU，可以設定 Event

`ID3D12Fence::SetEventOnCompletion(N, eventHandle);`

接著用 `WaitForSingleObject(eventHandle, INFINITE);` 那麼 CPU 就會停下來等事件發生
當然這段時間 CPU 就閒置了，我們要盡量縮小 CPU 閒置的時間


## 三、Resource Binding 

資源綁定概觀
  - 各種資源現在都統一叫做 ID3D12Resource，因為他們實際上就是一塊記憶體。
  - 使用資源，則要先建立 Descriptor
  - Descriptor 一個持有特殊資訊的物件。包含顯示卡硬體需要知道的資訊，還有該怎麼使用這個資源。
  - Descriptor 有四種 1. CBV/SRV/UAV 2.  Sampler 3. RTV 4. DSV 這四種要分別放在不同的 Descriptor Heap 裡面
  - Descriptor 都保存在 Descriptor Heap 裡頭
  - typeless resource 可以透過 descriptor 來指定顯示卡怎麼解讀這塊 buffer

4. Root Signature
4. Pipeline State Object
5. Descriptor & Descriptor Heap
6. Resource Residency


