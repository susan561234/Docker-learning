# Docker 學習筆記04
**實作1

1. 運行一個 nginx 伺服器容器 (2 種方法) 
2. 進入容器進行操作，更改容器內的檔案
3. 打開瀏覽器，驗證是否更改成功
4. 將容器保留成鏡像檔並再次驗證

拉image
```
docker pull nginx
```
創建容器
```
docker create -p 7999:80 --name mynginx nginx
```
啟動
```
docker start mynginx
```

網頁輸入http://localhost:7999/ nginx啟動成功

容器刪除
```
docker container rm -f <ID>
```
已創建並且啟動容器
```
docker run -d -p 7000:80 --name secondn nginx
```
進入容器
```
docker exec -it 60e bash
```
移至資料夾
```
cd /usr/share/nginx/html
```
對html做修改
```
echo "<h1>Hello Docker</h1>" > index.html
```
存成image
```
docker commit 60e mynginx:0.0.1
```
已創建並且啟動容器且可以做互動
```
docker run -d -it -p 8888:80 mynginx:0.0.1
```
-aq只顯示所有容器ID
```
docker ps -aq
```
停止所有容器狀態並且將停止的容器全部刪除=>全部刪除
```
docker stop $(docker ps -aq); docker container prune -f
```
![](https://i.imgur.com/bi8C3HL.png)


```
docker container rm -f $(docker ps -a -q)
```
**進階實作**
-p :本機port:容器port (讓外人接觸容器內的service)
-v :本機目錄:容器目錄 雙向同步
1. commit 保留 image
2. volume 卷積->Bind mount dir/file

讓本機與容器目錄同步
```
docker run -itd -v $pwd/web:/usr/share/nginx/html -p 7999:80 nginx
```
html寫入
```
echo "<h1>Hello, Bind Mount</h1>" > web/index.html
```
進入容器內
```
docker exec -it <container id> bash
```
移至剛剛對映的目錄
```
cd /usr/share/nginx/html
```
讀取內容
```
cat index.html
```
修改html內容
```
echo "<h1>Good Bye, Bind Mount</h1>" > index.html
```
```
exit
```
再次訪問7999端口內容以改變
