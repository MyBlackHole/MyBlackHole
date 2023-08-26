# obd 

prepare [-p DATA_PATH -h HOST]	环境准备，在部署前执行
deploy -c YAML_CONF [-n DEPLOY_NAME]	部署一个集群，在prepare之后运行
redeploy [-c YAML_CONF -n DEPLOY_NAME]	重新部署一个集群
start [-n DEPLOY_NAME]	启动一个已经停止的集群
stop [-n DEPLOY_NAME]	停止一个集群
restart [-n DEPLOY_NAME]	重新启动一个集群
destroy [-n DEPLOY_NAME]	销毁一个集群
list [-n DEPLOY_NAME]	列出当前环境中的所有集群
create_tenant [-n DEPLOY_NAME]	创建一个租户，默认会使用当前ObServer所有可用的资源
drop_tenant [-n DEPLOY_NAME]	删除一个租户
mysqltest [-n DEPLOY_NAME]	运行MysqlTest
