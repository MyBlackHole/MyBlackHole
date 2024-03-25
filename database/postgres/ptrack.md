# ptrack

```shell
git clone https://github.com/postgrespro/ptrack.git
git clone https://github.com/postgres/postgres.git -b REL_14_STABLE && cd postgres
git apply -3 ../ptrack/patches/REL_14_STABLE-ptrack-core.diff
gmake world
gmake install-world


<!-- 设置ptrack.map_size参数 -->
echo "shared_preload_libraries='ptrack'">> /pgdata/14/data/postgresql.conf
echo "ptrack.map_size=64">>/pgdata/14/data/postgresql.conf


<!-- root编译安装ptrack扩展，安装完成，需要重启postgres，reload 不生效。 -->
USE_PGXS=1 make -C /root/ptrack/ install

<!-- 登陆重启后的postgres，创建ptrack扩展 -->
CREATE EXTENSION ptrack;


<!-- ptrack相关SQL API -->
ptrack_version() ：返回ptrack 版本

ptrack_init_lsn() ：返回最后一个Ptrack映射初始化的LSN。

ptrack_get_pagemapset(start_lsn pg_lsn) ：返回自指定start_lsn始具有多个更改块及其位图的一组更改的数据文件。

ptrack_get_change_stat(start_lsn pg_lsn) ：返回自指定START_LSN以来更改统计（文件数，页数和大小MB）。
```
