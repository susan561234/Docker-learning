# Docker 學習筆記02
容器都是標準化!!!

就像是網路的HTTP協議或TCP協議,容器也有協議OCI(Open Container Initiative)
1. 運行時規範
2. 圖像規範
3. 分發規範

以前還沒有Docker時......
1. 套件版本差異
2. 執行流程很多
3. 環境不一會導致無法運行
4. 軟體都要安裝在本地電腦

Docker優點 為什麼使用Docker?
1. 上雲快速方便(移植性強)
2. 軟體套件版本清晰
3. 好交付
4. 測試執行快速
5. 環境腳本化

容器裡面裝了什麼?
![](https://i.imgur.com/M0OeOT0.png)

Docker採主從式架構
                    
docker engine 伺服器(主)<===>我們下指令 客戶端(從)

Image
1.  一種唯讀的模板，封裝了「執行特定環境所需要的資源」，在啟動 container 之後能夠使用。
2. 每個 image 都有獨一無二的 digest (sha256) ，凡 image有更動，digest 就會跟著改變。
3. 無法單獨執行，只能透過建立 container 間接執行。

Container
1. 基於 image 所建立的執行實例。
2. Docker 利用容器來執行應用。
3. 可以被創建、啟動、停止、刪除。
4. 每個容器都是相互隔離的
