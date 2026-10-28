### API 与 SDK 

API : Application Programming Interface（应用程序编程接口）的缩写。它指的是不同软件系统之间进行交互和通信的一套约定或规则。（通常基于 HTTP + JSON）

SDK : Software Development Kit（软件开发工具包）围绕某个 API 提供的开发工具包，把调用细节封装成函数，方便使用。

在大模型领域，OpenAI 和 Anthropic 的接口风格是两种常见的 API 约定，很多服务会兼容其中一种。对应的官方 Python SDK 分别是 openai 和 anthropic。由于 OpenAI 风格接口被广泛兼容，openai SDK 也常被用来调用其他兼容服务（比如 DeepSeek）。

### 以一次ds的API调用为例
调用 DeepSeek 的 API，本质是发一个 HTTP 请求：

```text
POST https://api.deepseek.com/chat/completions
Header: Authorization: Bearer xxx
Body: {"model": "...", "messages": [...]}
```

如果不用 SDK，就得自己用 Python/JS 的 HTTP 库（比如 requests、fetch）手写：

```python
import requests, json
r = requests.post(
    "https://api.deepseek.com/chat/completions",
    headers={"Authorization": f"Bearer {key}", "Content-Type": "application/json"},
    data=json.dumps({"model": "...", "messages": [...]})
)
result = r.json()
```
用了 SDK，就变成：

```python
client = OpenAI(api_key=..., base_url="https://api.deepseek.com")
response = client.chat.completions.create(model="...", messages=[...])
```

服务器地址、请求头、JSON 序列化、错误处理、流式解析……SDK 内部全部处理好了，只要管传参数、拿结果就行。

### SDK 内容结构
代码库（你 pip install / npm install 装的那个包）
文档、示例代码;工具/命令行程序;有时还有配置文件、调试工具

### 区分
|概念|是什么|例子
|--|--|--|
|SDK|一整套"开发工具包"，帮你对接某个平台/服务|微信 SDK、AWS SDK、OpenAI SDK
|API|服务方提供的"接口约定"（怎么发请求、返回什么）|POST /chat/completions 这个端点
|库 (Library)|一段可复用的代码，SDK 通常包含库|requests、numpy
|Framework |框架	|规定你代码怎么组织的"骨架"	|Django、React

### 对比curl与SDK
|对比项	|curl	|Python (openai SDK)	|Node.js (openai SDK)
|--|--|--|--|
|本质	|命令行工具发 HTTP	|用 Python 代码发 HTTP	|用 JS/TS 代码发 HTTP
|适用场景	|手动测试、调试、脚本	|Python 项目集成	|前端/Node 后端集成  
|能否写业务逻辑	|× 基本不行	|√	|√
|需要安装	|系统自带/装一下	|pip install openai	|npm install openai
|参数写法	|手写 JSON 字符串	|传 Python 字典	|传 JS 对象

