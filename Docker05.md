# Docker 學習筆記05
**實作2**

使用power shell!!!
```
mkdir -p jupyter/app
cd jupyter
```
注意名子有重複需要改 若是端口被占用使用其他端口ex:7777:8888/4000:5000 

start-notebook.sh --NotebookApp.token='' 啟動notebook
```
docker run --name jupyter -d -p 8888:8888 -p 5000:5000 -v $pwd/app:/home/jovyan/work jupyter/base-notebook start-notebook.sh --NotebookApp.token=''
```
進入jupyter http://localhost:8888/
在jupyter 輸入
```
!pip install Flask
```
```
from flask import Flask, request, abort

app = Flask(__name__)

@app.route('/', methods=['GET'])
def hello_world():
    return "網頁測試成功~~~~"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```
http://localhost:5000/ 網頁測試成功~~

**實作3  將jupyter與MySQL 串接**  

容器與容器間做溝通 link network

-e 環境變數 設定值
```
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -v $pwd/sqldb:/var/lib/mysql -d mysql:latest
```
--link 連接mysql容器 單向(只能jupyter to mysql)
```
docker run --link mysql --name jupyter -d -p 7777:8888 -v $pwd/app:/home/jovyan/work jupyter/base-notebook start-notebook.sh --NotebookApp.token=''
```

```
!pip install pymysql

import pymysql
# 開啟資料庫連線
db = pymysql.connect(host="mysql", user="root", passwd="123456", db="")
# 使用 cursor() 方法建立一個客戶端游標物件 cursor
cursor = db.cursor()
# 使用 execute() 方法執行 SQL 查詢
cursor.execute("SELECT VERSION()")
# 使用 fetchone() 方法獲取單條資料
data = cursor.fetchone()
print("Database version : %s" % data)
# 關閉資料庫連線
db.close()

```

![](https://i.imgur.com/wNNoayu.png) 成功~~~


**network**

```
docker network create test-network
```
查看有哪些網路
```
docker network ls
```
啟動容器
```
docker run --network test-network --name mysql -e MYSQL_ROOT_PASSWORD=123456 -v $pwd/sqldb:/var/lib/mysql -d mysql:latest
```
把2個容器指定在network下面/2個容器就可以做連結(雙向溝通)
```
docker run --network test-network --name jupyter -d -p 7777:8888 -v $pwd/app:/home/jovyan/work jupyter/base-notebook start-notebook.sh --NotebookApp.token=''
```
開啟jupyter work下有先前程式碼 一樣看到版本號 成功~~~~