# postman

```js
pm.sendRequest(
  {
    method: "POST",
    url: `${pm.environment.get("protocal")}://${pm.environment.get("host")}:${pm.environment.get("port")}${pm.environment.get("base-path")}/authentication/login-name`,
    header: { 
       'Content-Type': 'application/json'
    },
    body: {
        mode: "raw",
        raw: JSON.stringify({
            loginName: pm.environment.get('loginName'), //从环境变量中获取登录名和密码
            password: pm.environment.get('password')
        })
    },
  },
  function (err, response) {
    pm.environment.set("Authorization", response.json().data);
  }
);
//添加请求头
pm.request.headers.add({ 
    // These keys appears when you set a header by hand. Just for fun they are here
    disabled: false,
    description:{
        content: "DescriptionTest",
        type: "text/plain"
    },
    // Your header, effectively
    key: 'KeyTest', 
    name: 'NameTest', 
    // If you set a variable you can access it
    // HeaderTest here has value="ValueHeaderTest"
    value: pm.collectionVariables.get("HeaderTest")
}); 
```

```js
var jsonData = pm.response.json();
// console.info(jsonData)
// console.info(pm.environment)
pm.environment.set("apiKey", jsonData.data.token)
```
