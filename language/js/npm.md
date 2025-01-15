# npm

## install

```shell
paru -S npm

<!--mkdir ~/.npm-global-->
<!--npm config set prefix '~/.npm-global'-->
```

## usage
- 源查询
```shell
npm config get registry
```

- 设置源
```shell
# 官方 https://registry.npmjs.org/
npm config set registry https://registry.npm.taobao.org
```

- 查询包信息
```shell
npm view pyright
npm info pyright
```

