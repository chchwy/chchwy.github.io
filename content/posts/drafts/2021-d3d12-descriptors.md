---
date: 2021-04-15
title: "D3D12: Descriptors"
tags: [DirectX, D3D12]
isCJKLanguage: true
draft: true
---


## 什麼是 Descriptor

操作 D3D API 的時候，我們經常需要存取各樣的資源，比如說寫入 BackBuffer，或者讀取貼圖等等。
在讀寫資源之前，一定都需要先綁定資源 (bind)，然後 shader 才有辦法操作這些資源。

在 D3D12 裡面，資源 (ID3D12Resource*) 是沒辦法直接跟 Rendering Pipeline 綁定的。

所有的綁定都要透過 Descriptor

> 如果你有 D3D11 的經驗，以前叫做某某 View。
> 比如讀貼圖要先建立 Shader Resource View。
> 寫入的話要建 Render Target View，背後可能指向同一張貼圖。

## 怎麼建立 Descriptor

1. 首先要先知道各種類 Descriptor 的大小。

```
ID3D12Device::GetDescriptorHandleIncrementSize(D3D12_DESCRIPTOR_HEAP_TYPE);
```

```
UINT rtvSize = device->GetDescriptorHandleIncrementSize(D3D12_DESCRIPTOR_HEAP_TYPE_RTV);
UINT dsvSize = device->GetDescriptorHandleIncrementSize(D3D12_DESCRIPTOR_HEAP_TYPE_DSV);
UINT srvSize = device->GetDescriptorHandleIncrementSize(D3D12_DESCRIPTOR_HEAP_TYPE_CBV_SRV_UAV);
```

2. 配置記憶體

跟 GPU 挖一塊記憶體來放 Descriptor
`ID3D12Device::CreateDescriptorHeap();`

拿到 `ID3D12DescriptorHeap` 物件

3. 在記憶體上建立 Descriptor

呼叫 `ID3D12DescriptorHeap::GetCPUDescriptorHandleForHeapStart();` 取得記憶體的起始位置。

`D3D12_CPU_DESCRIPTOR_HANDLE` 這個就代表一個記憶體位址。

然後呼叫 CreateXXXView() 系列 API 

```
ID3D12Device::CreateConstantBufferView()
ID3D12Device::CreateShaderResourceView()
ID3D12Device::CreateUnorderedAccessView()
ID3D12Device::CreateRenderTargetView()
ID3D12Device::CreateDepthStencilView()
```

傳入
1. 想要綁定的 ID3D12Resource
2. 設定 Desc
3. 配置好的空白 Descriptor 的記憶體位置

拿到 D3D12_CPU_DESCRIPTOR_HANDLE

可以用 CD3DX12_CPU_DESCRIPTOR_HANDLE 來協助計算正確位址

## 接著就可以在各處使用這些 Descriptor 了

```
ID3D12GraphicsCommandList::OMSetRenderTargets() 可以綁定 RTV 跟 DSV
ID3D12GraphicsCommandList::ClearRenderTargetView();
ID3D12GraphicsCommandList::ClearDepthStencilView();
```
