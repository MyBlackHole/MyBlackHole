# ossutil
[细节](https://help.aliyun.com/zh/oss/developer-reference/copy-objects-4?spm=a2c4g.11186623.0.0.70a343a3hGth1B)

## cp
可以直接上传到指定目录下,即使没有对应上级目录

- 普通上传(小于 5 G)
```shell
<!-- -e 端点  -->
<!-- -i AccessKey ID(当成用户名)  -->
<!-- -k AccessKey Secret(当成密码)  -->
<!-- 上传 -->
ossutil64 -e http://127.0.0.1:9000 -i bNkZp8WKDMziff8x6hOz -k OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3 cp ./init.2.0.5.x86_64.tar oss://mybucket
 
<!-- 下载 -->
ossutil64 -e http://127.0.0.1:9000 -i bNkZp8WKDMziff8x6hOz -k OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3 cp oss://mybucket ./init.2.0.5.x86_64.tar

<!-- 增量(针对文件,文件由修改重新发送) -->
ossutil64 -e http://127.0.0.1:9000 -i bNkZp8WKDMziff8x6hOz -k OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3 cp -r TestS3/ oss://mybucket -u
```


## ls

- 列举所有桶
```shell
ossutil64 -e http://127.0.0.1:9000 -i bNkZp8WKDMziff8x6hOz -k OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3 ls
```

- 列举所有桶内文件
```shell
ossutil64 -e http://127.0.0.1:9000 -i bNkZp8WKDMziff8x6hOz -k OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3 ls oss://mybucket
```

## mkdir


## rm 

- 删除目录
```shell
ossutil64 -e http://127.0.0.1:80 -i bNkZp8WKDMziff8x6hOz -k OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3 rm -r oss://mybucket/wdg80140141142
```

## mb
- 创建桶
```shell
ossutil64 -e http://127.0.0.1:80 -i bNkZp8WKDMziff8x6hOz -k OvygWaWLqxDBmCeMth3PovD12p4DZL1I5eSLNGD3  mb oss://backup
```
