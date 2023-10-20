# curl
网络工具

## install
```shell
./configure --prefix=/media/black/Data/lib/curl/curl-7_32_0/  --disable-ldap
```

## 选项

### -A 
指定客户端的用户代理标头，即User-Agent。curl 的默认用户代理字符串是curl/[version] curl -A '' https://google.com 
移除User-Agent标头 

### -b 
用来向服务器发送 Cookie 
curl -b 'foo1=bar;foo2=bar2' https://google.com 
发送两个 Cookie 
curl -b cookies.txt https://www.google.com 
读取本地文件cookies.txt 

### -c 
将服务器设置的 Cookie 写入一个文件 
curl -c cookies.txt https://www.google.com 
将服务器的 HTTP 回应所设置 Cookie 写入文本文件cookies.txt 

### -d 
用于发送 POST 请求的数据体 
curl -d'login=emma＆password=123'-X POST https://google.com/login 
curl -d 'login=emma' -d 'password=123' -X POST  https://google.com/login 
使用-d参数以后，HTTP 请求会自动加上标头Content-Type : application/x-www-form-urlencoded。并且会自动将请求转为 POST 方法，因此可以省略-X POST 
curl -d '@data.txt' https://google.com/login 
读取data.txt文件的内容，作为数据体向服务器发送 

### --data-urlencode  
等同于 -d 发送 POST 请求的数据体，区别在于会自动将发送的数据进行 URL 编码 
curl --data-urlencode 'comment=hello world' https://google.com/login 
发送的数据hello world之间有一个空格，需要进行 URL 编码 

### -e 
用来设置 HTTP 的标头Referer，表示请求的来源 
curl -e 'https://google.com?q=example' https://www.example.com 
将Referer标头设为https://google.com?q=example 

### -F 
用来向服务器上传二进制文件 
curl -F 'file=@photo.png' https://google.com/profile 
给 HTTP 请求加上标头Content-Type: multipart/form-data，然后将文件photo.png作为file字段上传 
curl -F 'file=@photo.png;type=image/png' https://google.com/profile 
指定 MIME 类型为image/png，否则 curl 会把 MIME 类型设为application/octet-stream 
curl -F 'file=@photo.png;filename=me.png' https://google.com/profile 
原始文件名为photo.png，但是服务器接收到的文件名为me.png 

### -G 
用来构造 URL 的查询字符串 
curl -G -d 'q=kitties' -d 'count=20' https://google.com/search 
发出一个 GET 请求，实际请求的 URL 为https://google.com/search?q=kitties&count=20。如果省略--G，会发出一个 POST 请求 

### -H 
添加 HTTP 请求的标头 
curl -H 'Accept-Language: en-US' https://google.com 
添加 HTTP 标头Accept-Language: en-US 
curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' https://google.com 
添加两个 HTTP 标头 
curl -d '{"login": "emma", "pass": "123"}' -H 'Content-Type: application/json' https://google.com/login 
添加 HTTP 请求的标头是Content-Type: application/json，然后用-d参数发送 JSON 数据 

### -i 
打印出服务器回应的 HTTP 标头 
curl -i https://www.example.com 
收到服务器回应后，先输出服务器回应的标头，然后空一行，再输出网页的源码 

### -I 
向服务器发出 HEAD 请求，然会将服务器返回的 HTTP 标头打印出来 
curl -I https://www.example.com 
输出服务器对 HEAD 请求的回应 

### -k 
跳过 SSL 检测 
curl -k https://www.example.com 
不会检查服务器的 SSL 证书是否正确 

### -L 
会让 HTTP 请求跟随服务器的重定向。curl 默认不跟随重定向 
curl -L -d 'tweet=hi' https://api.twitter.com/tweet 

### --limit-rate 
用来限制 HTTP 请求和回应的带宽，模拟慢网速的环境 
curl --limit-rate 200k https://google.com 
将带宽限制在每秒 200K 字节 

### -o 
将服务器的回应保存成文件，等同于wget命令 
curl -o example.html https://www.example.com 
将www.example.com保存成example.html 

### -O 
将服务器回应保存成文件，并将 URL 的最后部分当作文件名 
curl -O https://www.example.com/foo/bar.html 
将服务器回应保存成文件，文件名为bar.html 

### -s 
将不输出错误和进度信息 
curl -s https://www.example.com 
上面命令一旦发生错误，不会显示错误信息。不发生错误的话，会正常显示运行结果 
curl -s -o /dev/null https://google.com 
让 curl 不产生任何输出，可以使用下面的命令 

### -S 
指定只输出错误信息，通常与-s一起使用 
curl -s -o /dev/null https://google.com 
命令没有任何输出，除非发生错误 

### -u 
用来设置服务器认证的用户名和密码 
curl -u 'bob:12345' https://google.com/login 
设置用户名为bob，密码为12345，然后将其转为 HTTP 标头Authorization: Basic Ym9iOjEyMzQ1 
curl https://bob:12345@google.com/login 
命令能够识别 URL 里面的用户名和密码，将其转为上个例子里面的 HTTP 标头 
curl -u 'bob' https://google.com/login 
只设置了用户名，执行后，curl 会提示用户输入密码 

### -v 
输出通信的整个过程，用于调试 
curl -v https://www.example.com 
curl --trace - https://www.example.com 
--trace参数也可以用于调试，还会输出原始的二进制数据 

### -x 
指定 HTTP 请求的代理 
curl -x socks5://james:cats@myproxy.com:8080 https://www.example.com 
指定 HTTP 请求通过myproxy.com:8080的 socks5 代理发出。 
如果没有指定代理协议，默认为 HTTP 
curl -x james:cats@myproxy.com:8080 https://www.example.com 
请求的代理使用 HTTP 协议 

### -X 
指定 HTTP 请求的方法 
curl -X POST https://www.example.com 
对https://www.example.com发出 POST 请求 


## 例子
curl -o [文件名] www.sina.com : 保存响应内容 

curl -X GET 127.0.0.1:8080/\?username=1\&age=2\&page=2 : Get 

curl -X POST 127.0.0.1:8080/post -F "username=aaaa" -F "password=aaaaaaa"  : POST FORM 

curl --cookie "name=xxx" www.example.com  :cookie 

curl -L www.sina.com  :自动跳转 

curl --header "Content-Type:application/json" http://example.com : header 

curl --user name:password example.com : http 认证 

curl -i www.sina.com  : 显示响应头信息 

curl -v www.sina.com 

curl --trace output.txt www.sina.com (更加详细) 

curl --trace-ascii output.txt www.sina.com : 显示通信过程 

curl -X POST --data "data=xxx" example.com/form.cgi 

curl -H "Content-Type: application/json" -X POST -d "{\"abc\":123}" "https://httpbin.org/post"  : POST BODY data 

curl -H "Content-Type: application/json" -X POST -d @test.json URL : json 数据放到文件 test.json 里 

curl -X POST--data-urlencode "date=April 1" example.com/form.cgi  : 数据没有经过表单编码 

curl --user-agent "[User Agent]" [URL] : user-agent 

curl --trace output.txt www.sina.com : 保存通行过程 output.txt 

