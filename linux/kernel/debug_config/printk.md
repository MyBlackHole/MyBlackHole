# printk

```shell
printk(日志級別 "消息文本")；這里的日志級別通俗的說指的是對文本信息的一種輸出范圍上的指定。

日志級別一共有8個級別，printk的日志級別定義如下(在linux26/includelinux/kernel.h中)：

#defineKERN_EMERG"<0>"/*緊急事件消息，系統崩潰之前提示，表示系統不可用*/

#defineKERN_ALERT"<1>"/*報告消息，表示必須立即采取措施*/

#defineKERN_CRIT"<2>"/*臨界條件，通常涉及嚴重的硬件或軟件操作失敗*/

#defineKERN_ERR"<3>"/*錯誤條件，驅動程序常用KERN_ERR來報告硬件的錯誤*/

#defineKERN_WARNING"<4>"/*警告條件，對可能出現問題的情況進行警告*/

#defineKERN_NOTICE"<5>"/*正常但又重要的條件，用於提醒。常用於與安全相關的消息*/

#defineKERN_INFO"<6>"/*提示信息，如驅動程序啟動時，打印硬件信息*/

#defineKERN_DEBUG"<7>"/*調試級別的消息*/

沒有指定日志級別的printk語句默認采用的級別是 DEFAULT_ MESSAGE_
```
