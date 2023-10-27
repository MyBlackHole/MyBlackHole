# cp

```shell
--rewind value 在指定时间将对象回滚到当前版本
--version-id value, --vid value 选择要复制的对象版本
--recursive, -r 递归复制
--older-than value 复制比持续时间字符串中的值更早的对象（例如 7d10h31s）
--newer-than value 复制比持续时间字符串中的值更新的对象（例如 7d10h31s）
--storage-class value, --sc value 设置目标上新对象的存储类别
--attr value 添加对象的自定义元数据
--Continue, -c 创建或恢复复制会话
--preserve, -a 保留文件系统属性（模式、所有权、时间戳）
--disable-multipart 禁用分段上传功能
--md5 强制所有上传计算 md5sum 校验和
--tags value 对上传的对象应用一个或多个标签
--retention-mode 应用于对象的值保留模式（治理、合规性）
--retention-duration 值对象的保留期限，以 d 天或 y 年为单位
--legal-hold value 对复制的对象应用合法保留（开、关）
--zip 从远程 zip 文件中提取（仅限 MinIO 服务器源）
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
