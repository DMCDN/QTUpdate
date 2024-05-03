# QTUpdate
專題軟體使用的更新程式

使用PyQt6製作UI，文件壓縮使用Zstd與LZMA

## 特點：
* 盡可能壓縮更新所需總時間
* 文件均經壓縮處理，僅須下載差異文件
* 使用多線程下載&解壓
* 配製簡易(一份版本訊息、兩份patch包)
        
 ## 打包：
#### 將指定目錄下的所有文件打包成兩份文件
* patch.res (經處理&合併後的資源包)
* patch.resdiff  (紀錄文件，紀錄所有文件的：路徑、crc32值、Offset、Size)

## patch.res
#### 考量到時間成本(解壓+下載時間總長)
* 預設情況使用Zstd壓縮，~~並加密前16bytes~~ 小於1kb的文件則加密整個文件

* 以下情況使用lzma壓縮：
文件大於100mb
壓縮比不小於0.9
* 以下情況不做壓縮：
文件小於1kb
壓縮比小於0.9

* 最後將壓縮/加密方法寫於頭部後再加入至包內

#### 文件結構
* File Signature(2bytes)
* 加密方法 (1byte)
* 壓縮方法 (1byte)
* 文件原始大小 (4bytes)
* 其餘則為加密/壓縮處理後資料

 ![螢幕擷取畫面 2024-05-03 184810](https://github.com/DMCDN/QTUpdate/assets/128150279/6bb7267d-d009-4fec-a619-31fbcdb1c0a8)
 
## patch.resdiff
![image](https://github.com/DMCDN/QTUpdate/assets/128150279/6f70f552-7dab-4378-a40b-56b8f7c5a42e)
* 紀錄每個文件在patch.res中的訊息
* 根據紀錄文件提供的路徑、指針(Offset)與大小(size)開始創建下載程序
* requests header的參數則為 ({"Range": "bytes={offset}-{offset+size-1}"})
  
## 更新
* 下載前，比對VersionConfig與軟體紀載的版本是否相同
* 若不匹配，開始分析本地的所有文件與記錄文件(patch.resdiff)的crc32值
* 接著下載&解壓差異文件
    


https://github.com/DMCDN/QTUpdate/assets/128150279/fc788936-c14f-48de-9668-670f0414a4e5



# 額外功能
## 檢查軟體啟用狀態

網頁使用Flask與SQLite製作一個檢查序號用的API

首次使用有1小時試用時間，到期則會要求將程式顯示的序號，輸入至網站的啟用頁面

透過進入 https://lwork.pythonanywhere.com 購買啟用權限輸入(購買流程開發中)

https://github.com/DMCDN/QTUpdate/assets/128150279/cb95f7f4-4fb5-471c-a339-59f108c79881

