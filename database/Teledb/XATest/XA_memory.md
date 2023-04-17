# xa storage
XA 事务状态存储

- 测试目标
1. 测试内存 XA 是否落盘
2. 高并发下是否预期插入与世纪插入是否一致
3. 测试 XA 事务过程中拦截其中一个分片请求事务是否可以回滚


## 设置方式
[[teledb#^transaction]]
实例列表 -- 管理 -- 分组管理 -- 属性设置 -- 查询属性**xaStorage** -- 修改属性值 **memory**
重启 dbproxy

### 测试修改内存存放 xa 后 file 模式是否还有效
修改内存模式后将不记录到 rocksdb 
- rocksdb 日志状态
```shell
udaltest_1(dbproxy-b2d4b50e-69943-udaltest_1-3082946

udaltest_2(dbproxy-b2d4b50e-69943-udaltest_2-307476dbproxy-b2d4b50e-69943r6

udaltest_1(dbproxy-b2d4b50e-69943-udaltest_1-3082946

udaltest_2(dbproxy-b2d4b50e-69943-udaltest_2-307476
                                                   dbproxy-b2d4b50e-69943
```
- 执行输入插入
```sql
MySQL test@192.168.90.207:UDALTEST> UDAL XA START;
Reconnecting...
+--------------------+
| Xid                |
+--------------------+
| dbproxy-83454ffe-1 |
+--------------------+
1 row in set
Time: 0.019s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, name) values(908768, RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.112s
MySQL test@192.168.90.207:UDALTEST> insert into user (id, score, name) values(908769, RAND() * 100, 'test1');
Query OK, 1 row affected
Time: 0.068s
MySQL test@192.168.90.207:UDALTEST> COMMIT;
Query OK, 0 rows affected
Time: 0.146s
```

- 执行后日志状态
```shell
udaltest_1(dbproxy-b2d4b50e-69943-udaltest_1-3082946

udaltest_2(dbproxy-b2d4b50e-69943-udaltest_2-307476dbproxy-b2d4b50e-69943r6

udaltest_1(dbproxy-b2d4b50e-69943-udaltest_1-3082946

udaltest_2(dbproxy-b2d4b50e-69943-udaltest_2-307476
                                                   dbproxy-b2d4b50e-69943
```
