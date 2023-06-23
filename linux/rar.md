# rar

## 选项

|   |   |
|---|---|
|a|添加文件到操作文档|
|c|对操作文档添加说明注释|
|d|从文档中删除文件|
|e|将文件解压到当前目录|
|x|带路径解压文档中内容到当前目录|
|s|转换文档成自解压文档|
|r|修复文档|
|t|检测文档|
|k|锁定文档|

## 例子

|   |   |
|---|---|
|rar e test.rar|不保持压缩前的目录结构|
|rar x test.rar|保持压缩前目录结构|
|rar a t.rar f.txt|t.rar存在则追加f.txt,否则f.txt压缩成t.rar,但是t.rar中有f.txt则更新f.txt|
|rar c test.rar|添加注释|
|rar cf test.rar|对每一个文件添加注释|
|rar cw test.rar comment.txt|将文档注释写入文件|
|rar d test.rar file1.txt|将file.txt从test.rar中删除|
|rar k test.rar|锁定文档，使其无法进行任何更新操作|
|rar r test.rar|尝试修复文件|
|rar s test.rar|生成 test.sfx 的可执行文档，运行效果相当于 rar x test.rar|
|rar t test.rar|检查完整性|
