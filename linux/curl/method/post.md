# post

```shell
curl \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2OTkxOTUwODYsImlhdCI6MTY5ODU5MDI4Nn0.jpEKig2tHRcJn0feryGf47rIdQw6UsaexMPpWJzXLi8" \
    --data '{"userId":"2"}'  \
    -v 127.0.0.1:8888/user/info

curl -X POST --data "data=xxx" example.com/form.cgi 

curl -H "Content-Type: application/json" -X POST -d "{\"abc\":123}" "https://httpbin.org/post"  : POST BODY data 

curl -H "Content-Type: application/json" -X POST -d @test.json URL : json 数据放到文件 test.json 里 

curl -X POST --data-urlencode "date=April 1" example.com/form.cgi  : 数据没有经过表单编码 

curl -X POST 127.0.0.1:8080/post -F "username=aaaa" -F "password=aaaaaaa"  : POST FORM 
```
