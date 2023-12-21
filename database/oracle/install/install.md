# install

```shell
docker pull docker.io/gvenzl/oracle-xe:21-slim
docker build -t pyo --build-arg PYO_PASSWORD=a_secret .
docker run -d --name pyo -p 1521:1521 -it -e ORACLE_PASSWORD=a_secret_password pyo
docker exec -it pyo bash
python setup.py
python bind_insert.py
```
