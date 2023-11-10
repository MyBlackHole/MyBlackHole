# mirror

```shell
--overwrite 覆盖同名
标志：
   --overwrite 如果目标与源不同，则覆盖目标上的对象
   --dry-run 执行假镜像操作
   --watch, -w 监视并同步更改
   --remove 删除目标上的无关对象
   --region 值指定在目标上创建新存储桶时的区域（默认值：“us-east-1”）
   --preserve, -a 保留目标上的文件/对象属性和存储桶策略/锁定配置桶
   --md5 强制所有上传计算 md5sum 校验和
   --active-active 启用主动-主动多站点设置
   --disable-multipart 禁用分段上传功能
   --exclude 值排除与指定对象名称模式匹配的对象
   --exclude-storageclass 值排除与指定存储类匹配的对象
   --older-than value 过滤器对象早于持续时间字符串中的值（例如 7d10h31s）
   --newer-than value 过滤对象比持续时间字符串中的值更新（例如 7d10h31s）
   --storage-class value, --sc value 指定目标上新对象的存储类
   --attr value 为所有对象添加自定义元数据
   --monitoring-address 值如果指定，将创建一个新的 prometheus 端点来报告镜像活动。 （例如：本地主机:8081)
   --retry 如果指定，将在发生错误时基于每个对象启用重试
   --encrypt-key 值加密/解密对象（使用服务器端加密和客户提供的密钥）[$MC_ENCRYPT_KEY]
   --加密值加密/解密对象（使用服务器端加密和服务器管理密钥）[$MC_ENCRYPT]
   --config-dir 值，-C 值配置文件夹路径（默认值：“/home/black/.mc”）[$MC_CONFIG_DIR]
   --quiet, -q 禁用进度条显示 [$MC_QUIET]
   --no-color 禁用颜色主题 [$MC_NO_COLOR]
   --json 启用 JSON 行格式化输出 [$MC_JSON]
   --debug 启用调试输出 [$MC_DEBUG]
   --insecure 禁用 SSL 证书验证 [$MC_INSECURE]
   --limit-upload 值将上传限制为最大速率（以 KiB/s、MiB/s、GiB/s 为单位）。 （默认：无限制）[$MC_LIMIT_UPLOAD]
   --limit-download 值将下载限制为最大速率（以 KiB/s、MiB/s、GiB/s 为单位）。 （默认：无限制）[$MC_LIMIT_DOWNLOAD]
   --help, -h 显示帮助
```

- 同步(默认不替换已存在文件)
```shell
mc mirror BH/test/ BH/test2

mc mirror --overwrite BH/test/ BH/test2
```
