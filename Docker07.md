# Docker 學習筆記07
**Docker-Compose**

**-f** $pwd/lab3/docker-compose.yml 指定了要使用的 Docker Compose 配置文件的路徑和文件名
**up** 命令用於啟動容器
```
docker-compose -f $pwd/lab3/docker-compose.yml up -d 
```

```
version: '3' #yml版本
services:    #容器服務
  flask:
    build:
      context: ./  #dockerfile路徑
    container_name: python_test #flask容器名稱
    tty: true        # -t
    stdin_open: true # -i
    ports:           # -p
      - "5000:5000"
#Developer use    
    #volumes:   # -v 綁定資料夾
    #  - ./web:/app/
    #command:    容器啟動後指令 可下在dockerfile or yml
    #  - app.py
```
```
FROM python:3.9
# add project
ADD ./web /app

WORKDIR /app  #工作目錄

# install flask
RUN pip install --upgrade pip
RUN pip install Flask requests line-bot-sdk PyMYSQL


##run flask
##Developer use
#CMD ["python"]

#Production use
CMD ["python", "app.py"]  #預設執行指令
```
刪除容器 只刪除容器 image 還存在
```
docker-compose -f $pwd/lab3/docker-compose.yml down
```
ENTRYPOINT 與 CMD 區別
ENTRYPOINT:一定要執行
CMD: 可以改掉
![](https://i.imgur.com/KaDMv8R.png)
