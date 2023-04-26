# Docker 學習筆記03
**基本指令/image指令**

版本
```
docker version
```
如何操作
```
docker
```
從docker hub 抓取image 後 啟動 container 若是有東西打印在終端機
```
docker run hello-world
```
完整元件指令 操作資源+子指令(pull、create......)
```
docker image
docker container
docker volume
docker network
```
列出所有image/查看功能
```
docker image ls -a /docker images -a 
docker image ls --help
```
查詢是否有這個image
```
docker search python
```
拉ubuntu image
```
docker pull ubuntu
```
刪除image報錯 不能砍有運行container的image
```
docker image rm hello-world
```
刪除ubuntu
```
docker image rm 08d22(Image ID 不用完整輸入若是有一樣的會顯示找不到)
```
**container指令**

```
docker ps -a/docker container ls -a
```

情境:老闆希望測試Redis緩存DB
```
docker pull redis
docker run -d -it --name myRedis redis 
```
運行myRedis容器  簡單來說與容器互動就是加-it
-d  detach後台執行 
-a  attach前台執行 輸入輸出頁面 終端印出啟用狀態
-i  interactive 讓我們可以在容器輸入指令    
-t  tty 一個介面

進入myRedis容器redis-cli開啟cli 可以用redis指令
```
docker exec -it myRedis redis-cli
```
刪除容器 -f force 強制砍運行中容器
```
docker rm -f <container ID>
```
**熟悉docker-cli操作**
1. 建立ubuntu容器
2. 啟動ubuntu容器
3. 使用exec外部執行指令,進入容器
4. docker logs
5. 關閉並重起容器
6. 使用attach進入容器
7. docker log
8. 砍掉容器再練,透過ps認識exec/attach

```
docker container create -it --name myubuntu ubuntu
docker start myubuntu 開啟容器
docker exec  -it myubuntu bash 進入類似終端畫面
exit
*容器狀態依舊運行*
docker stop myubuntu 關掉容器
docker start myubuntu
docker attach myubuntu 進入容器
exit
*退出後容器狀態直接關閉*
docker logs <container ID> 操作過程皆記錄下來
```
使用attach方法進入logs會記錄下來 
attach 是主線第一個PID 
exec 會另外建立執行序run bash
追蹤logs -f fllow
```
docker logs -f myubuntu
```
容器資訊
```
docker inspect <ID or name>container or image都
可以查看
```
容器使用資源狀態有CPU/記憶體...
```
docker stats <containerID or name>
```
安裝壓力測試套件
```
apt-get update
apt-get install -y stress-ng
stress-ng --cpu 4 --timeout 60s
可以看到第一個終端機的畫面cpu有跳動
```