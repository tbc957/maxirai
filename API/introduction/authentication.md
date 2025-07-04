# 身份认证

MAXIR AI Open API 使用 API Key 进行身份验证。每个 API 请求都必须包含你的 API Key。该 API Key 用于请求认证与使用配额跟踪。

<Tip>
通过 API 来访问特定项目中的资源时，必须使用该项目中的 API Key 进行认证。
</Tip>

请注意，**你的 API Key 是非常重要的机密信息**。请勿与他人共享，也不要在浏览器、应用程序或其他客户端代码中暴露。

所有 API 请求必须在 `x-pd-api-key` HTTP 头中包含您的 API Key，例如：

```shell
x-pd-api-key: API_KEY
```

---

## 发起请求

您可以将以下命令复制并粘贴到终端中，执行您的第一个 API 请求。执行请求前，请将 `$PROJECT_API_KEY` 替换为您的 API 密钥，`$USER_ID` 替换为您的实际用户 ID，URL 中的 `<domain_name>` 替换为真实的域名。

<CodeGroup>

```curl cURL
curl --request POST \
  --url https://<domain_name>/v2/team/sessions \
  --header 'Content-Type: application/json' \
  --header 'x-pd-api-key: $PROJECT_API_KEY' \
  --data '{
  "name": "My session",
  "output_language": "FR",
  "job_mode": "AUTO",
  "max_contextual_job_history": 10,
  "agent_id": "DATA_ANALYSIS_AGENT",
  "user_id": "$USER_ID"
}'
```

```python Python
import requests

url = "https://<domain_name>/app/api/v2/team/sessions"

payload = {
    "name": "My session",
    "output_language": "FR",
    "job_mode": "AUTO",
    "max_contextual_job_history": 10,
    "agent_id": "DATA_ANALYSIS_AGENT",
    "user_id": "tmm-dafasdfasdfasdf"
}
headers = {
    "x-pd-api-key": "$PROJECT_API_KEY",
    "Content-Type": "application/json"
}

response = requests.request("POST", url, json=payload, headers=headers)

print(response.text)
```

</CodeGroup>

