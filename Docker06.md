# Docker 學習筆記06

**Dockerfile**==>建立image

1. 透過學習編寫 dockerfile，可以 build 客製化image。
2. 透過學習編寫 docker-compose.yml，運用相關指令去編排及管理單主機上多個 container。
3. 透過以上兩樣工具，可以迅速雲端建置及部署容器服務。

拉教材
```
git clone https://github.com/Whaleman0423/docker_advanced2.git
```
切換目錄
```
cd docker_advanced2
```
-t (tag) image名稱:版本 Dockerfile 路徑
```
docker build -t aiclass/custom-jupyter:v1 ./lab1/1_1/
```

**Dockerfile 結構基本分為四部分：**
• 基底映像檔資訊
•~~維護者資訊~~(已拿掉)
• 映像檔操作指令(安裝套件指令)
• 容器啟動時執行指令

dockerfile內容
```
FROM jupyter/base-notebook
```

```
docker run --name lab1-container -d -p 8888:8888 -v $pwd/lab1:/home/jovyan/work aiclass/custom-jupyter:v1 start-notebook.sh --NotebookApp.token=''
```
失敗import

dockerfile內容
```
FROM jupyter/base-notebook
RUN pip install boto3 requests
```
創建image
```
docker build -t aiclass/custom-jupyter:v2 ./lab1/1_2/
```
啟動容器
```
docker run --name lab1-container -d -p 8888:8888 -v $pwd/lab1/:/home/jovyan/work aiclass/custom-jupyter:v2 start-notebook.sh --NotebookApp.token=''
```
![](https://i.imgur.com/fdZCqHd.png)
成功import

---
dockerfile內容
```
FROM nginx
MAINTAINER aeon33system@gmail.com
COPY ./html /usr/share/nginx/html
LABEL name="Nginx Web Server" \
    build-date="2019-7-12"
VOLUME ["/usr/share/nginx/html"]
EXPOSE 80
```
有無指定-p -v
```
docker build -t aiclass/custom-nginx:v1 ./lab2/1_1/
```

```
docker run -itd --name without-option-nginx aiclass/custom-nginx:v1
```

```
docker run -itd -p 80:80 --name nginx aiclass/custom-nginx:v1
```
訪問 http://localhost/  看到下圖就是成功啦~~~~ 
![](https://i.imgur.com/fsWWx4l.jpg)


