# evince
文档查看器 

## 参数
```shell
-p, --page-label=页面 :     要显示的文档的页标签。 

-i, --page-index=页号  :   要显示的文档的页编号。 

-n, --named-dest=目标   :  要显示的已命名目标。 

-f, --fullscreen        :  以全屏模式运行 evince 

-s, --presentation     :   以放映模式运行 evince 

-w, --preview         :    以预览程序模式运行 evince 

-l, --find=字符串     :    文档中待查找的单词或短语 

--display=显示       :     要使用的 X 显示 
```

## 例子
- 打开PDF
```shell
evince 文件.pdf 
```

- 打开PDF的第2页
```shell
evince 文件.pdf --page-index=2 
```

- 打开PDF的第2页的第3个字母
```shell
evince 文件.pdf --page-index=2 --find=字母 
```

- 打开PDF的第2页的第3个字母，并以全屏模式打开
```shell
evince 文件.pdf --page-index=2 --find=字母 --fullscreen 
```

- 打开PDF的第2页的第3个字母，并以放映模式打开
```shell
evince 文件.pdf --page-index=2 --find=字母 --presentation 
```

- 打开PDF的第2页的第3个字母，并以预览程序模式打开
```shell
evince 文件.pdf --page-index=2 --find=字母 --preview 
```
