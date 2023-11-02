# isort

```shell
ARG VERSION=3
FROM python:$VERSION

# Install pip and poetry
RUN python -m pip install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple && python -m pip install --user isort==5.10.1 -i https://pypi.tuna.tsinghua.edu.cn/simple

CMD ["bash"]

<!-- build -->
docker build -f ./Dockerfile -t isort:5.10.1 .

<!-- run -->
docker run -it --rm isort:5.10.1 /root/.local/bin/isort --help
```
