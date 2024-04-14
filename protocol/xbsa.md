# xbsa

## 创建对象
![[imgs/Pasted image 20240413095103.png]]

- BSAQueryApiVersion：该接口用于确定Netbackup XBSA接口的当前版本。
- BSAInit：该接口用于对XBSA应用程序进行身份验证，与NetBackup XBSA接口建立session会话，并为调用者的后续API调用建立环境。注意，BSAInit不支持嵌套创建session会话。
- BSABeginTxn：

该接口用于创建一个事物，这里的事物和数据库事物概念相似，BSABeginTxn（）调用向NetBackup XBSA接口指示作为原子单位执行的一个或多个操作的开始，即所有操作将成功或没有成功。可以将一个动作假定为为特定目的而进行的单个函数调用或一系列函数调用。

例如，一个BSACreateObject（）调用后跟多个BSASendData（）调用并以BSAEndData（）调用终止可以被视为单个操作。 在正常使用中，BSABeginTxn（）调用总是与随后的BSAEndTxn（）调用耦合。如果在事务期间调用BSATerminate（），则事务中止。

注意，BSABeginTxn不支持嵌套创建事物。

- BSACreateObject：

BSACreateObject调用在NetBackup中创建一个XBSA对象。 该调用将启动NetBackup XBSA接口与NetBackup服务器之间的通信。然后可以将XBSA对象数据传递到内存缓冲区中。BSACreateObject调用中的dataBlockPtr参数允许调用者获取有关NetBackup XBSA接口所需的缓冲区结构的信息。

- BSASendData：

BSASendData（）将字节数据流发送到缓冲区中的NetBackup XBSA接口。如果要发送的字节数据流很大，则可以多次调用BSASendData（）。此调用只能在BSACreateObject（）或另一个BSASendData（）调用之后使用。

- BSAEndData：

调用方在调用BSACreateObject之后调用零次或多次BSASendData，当前备份文件传输完毕后调用BSAEndData，用于通知Netbackup服务器当前文件传输结束

- BSAEndTxn：

BSAEndTxn与BSABeginTxn耦合使用，以标识将被视为事务的API调用或一组API调用。。

- BSATerminate：

BSATerminate调用终止与NetBackup XBSA接口的会话，该接口由BSAInit调用对应，释放当前session会话获取的所有资源。如果在事务内调用BSATerminate（），则事务中止。


## 获取对象

![[imgs/Pasted image 20240413095132.png]]

- BSAQueryObject：

BSAQueryObject调用从NetBackup XBSA界面启动有关NetBackup XBSA对象文件的信息请求。查询结果由查询描述符中指定的搜索条件确定。在BSA_ObjectDescriptor（由objectDescriptorPtr参数引用）中返回满足查询搜索条件的第一个XBSA对象的XBSA对象描述符。

- BSAGetData：

BSAGetData从NetBackup XBSA接口请求XBSA对象文件数据。在BSAGetObject（）调用之后或在其他BSAGetData调用之后使用此调用。
