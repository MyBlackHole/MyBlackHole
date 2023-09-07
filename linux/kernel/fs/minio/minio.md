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
```

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
    volumes:
      - 'minio_data:/bitnami/minio/data'

volumes:
  minio_data:
    driver: local
```
