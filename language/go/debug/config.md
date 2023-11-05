# config

- golang的dlv调试工具print打印字符串显示more,无法显示更多
```shell
(dlv) config -list
aliases                   map[]
substitute-path           []
max-string-len            <not defined>
max-array-values          <not defined>
max-variable-recurse      <not defined>
disassemble-flavor        <not defined>
show-location-expr        false
source-list-line-color    <nil>
source-list-arrow-color   ""
source-list-keyword-color ""
source-list-string-color  ""
source-list-number-color  ""
source-list-comment-color ""
source-list-line-count    <not defined>
debug-info-directories    [/usr/lib/debug/.build-id]
position                  ""
(dlv) config max-string-len 99999
```
