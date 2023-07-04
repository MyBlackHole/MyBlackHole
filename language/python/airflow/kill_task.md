# kill task

关闭 task

```plantuml
@startuml
|web|
start
:调用接口 /dagrun_failed;
:查询运行中的 TI;
:修改 TI 为 FAILED;
end
|task run heartbeat|
fork
:运行 task;
:运行 task;
endfork
end
@enduml
```
