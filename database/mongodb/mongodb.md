# mongodb

## install 

### linux
```shell
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add - 

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list 

sudo apt-get update 

sudo apt-get install -y mongodb-org 

sudo systemctl start mongod.service
```

|   |   |
|---|---|
|sudo service mongodb start|启动|
|sudo service mongodb stop|停止|
|sudo service mongodb restart|重启|
|sudo apt install mongodb-server|服务|
|sudo npm install -g mongo-express web|可视界面|

