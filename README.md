# QTUpdate
專題軟體使用的更新程式

## 特點：
* 盡可能壓縮更新所需總時間
* 文件均經壓縮處理，僅須下載差異文件
* 使用多線程下載&解壓
* 配製簡易(一份版本訊息、兩份patch包)
        
 ## 打包：
#### 將指定目錄下的所有文件打包成兩份文件
* patch.res (經處理&合併後的資料包)
* patch.resdiff  (紀錄文件，紀錄所有文件的：路徑、crc32值、Offset、Size)

#### patch.res
    考量到時間成本(解壓+下載時間總長)
    預設情況使用Zstd壓縮，並加密前16bytes，小於1kb的文件則加密整個文件
    以下情況使用lzma壓縮：
        文件大於100mb
        壓縮比不小於0.9
    以下情況不做壓縮：
        文件小於1kb
        壓縮比小於0.9
#### patch.resdiff
    0
#### 更新
    下載前，比對VersionConfig與軟體紀載的版本是否相同
    若不匹配，開始分析本地的所有文件與記錄文件(patch.resdiff)的crc32值
    接著下載&解壓差異文件
    


https://github.com/DMCDN/QTUpdate/assets/128150279/fc788936-c14f-48de-9668-670f0414a4e5

