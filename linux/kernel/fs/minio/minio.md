# minio 

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

5Ua3CRN75mh3wk3kDT26
RdXdJas318BXz9CW9W5zttKRehItYYtzyRXyjfqE


docker run -p 9000:9000 -p 9090:9090 \
     --net=host \
     --name minio \
     -d --restart=always \
     -e "MINIO_ACCESS_KEY=minioadmin" \
     -e "MINIO_SECRET_KEY=minioadmin" \
     -v /home/minio/data:/data \
     -v /home/minio/config:/root/.minio \
     minio/minio server \
     /data --console-address ":9090" -address ":9000"
```

- podman
```shell
podman run -p 9000:9000 -p 9090:9090 \
     --name minio \
     -d --restart=always \
     -e "MINIO_ACCESS_KEY=minioadmin" \
     -e "MINIO_SECRET_KEY=minioadmin" \
     minio server \
     /data --console-address ":9090" -address ":9000"
```

- docker-compose.yml
```shell
# Copyright VMware, Inc.
# SPDX-License-Identifier: APACHE-2.0

version: '2'

services:
  minio:
    image: docker.io/bitnami/minio:2023
    ports:
      - '9000:9000'
      - '9001:9001'
    environment:
      - MINIO_ROOT_USER=admin1234
      - MINIO_ROOT_PASSWORD=admin1234
    volumes:
      - 'minio_data:/bitnami/minio/data'

volumes:
  minio_data:
    driver: local
```
