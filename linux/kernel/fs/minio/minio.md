# 

# 安装

- docker 
```
curl -sSL https://raw.githubusercontent.com/bitnami/containers/main/bitnami/minio/docker-compose.yml > docker-compose.yml

lvim docker-compose.yml
添加
    environment:
      - MINIO_ROOT_USER=admin1234
      - MINIO_ROOT_PASSWORD=admin1234

docker-compose up -d --build
```
