# ollama

```shell
paru -S ollama

<!--7B 代码专用模型-->
ollama pull codellama:7b-code

<!--或通用模型-->
ollama pull llama2:13b

systemctl enable ollama

<!--启动服务（默认端口 11434）-->
<!--ollama serve-->

systemctl start ollama

<!--测试 API 是否响应（新终端窗口）-->
curl http://localhost:11434/api/generate -d '{
  "model": "codellama:7b-code",
  "prompt": "Hello"
}'

ollama list

<!--<!--生成密钥-->-->
<!--export OLLAMA_API_KEY=your_api_key-->
```

```shell
ollama pull qwen2:72b
ollama pull deepseek-r1:8b
```
