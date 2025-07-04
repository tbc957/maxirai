# 流式传输

MAXIR AI Open API 支持将流式响应发送给客户端，从而为特定请求提供部分结果。此功能通过 [服务器推送事件（SSE）](https://html.spec.whatwg.org/multipage/server-sent-events.html#server-sent-events) 标准实现。

## 如何理解流式响应

每个请求的响应由一系列响应块组成。当为请求启用流式传输模式时，MAXIR AI 将实时向客户端发送更新，持续传送响应块，直到所有数据完全返回。 

响应块的结构如下：

```json
{
    "id": "<group_id>",
    "model": "",
    "choices": [
        {
            "delta": {
                "content": "<content>"
            },
            "index": 0
        }
    ],
    "created": 1731664172,
    "group_id": "<group_id>",
    "group_name": "<group_name>",
    "stage": "<block_stage>"
}
```

每个流式响应块包含以下字段：

- `id` 和 `group_id`：响应块所属组的 ID。

    流式响应中的组是响应块的集合。例如，在一个常规任务中，`Analyze` 阶段的每个步骤都是一个组，而整个 `Respond` 阶段是另一个组。

- `content`：响应块的内容，具体内容会根据块的类型有所不同。有关详细信息，请参阅 [内容说明](#内容说明)。

- `created`：表示内容创建时间的时间戳。

- `group_id`：响应块所属组的 ID。

- `group_name`：组的名称，例如 `Conclusions`。

- `stage`：响应块所属的阶段。共有两个阶段：`Analyze` 和 `Respond`。


## 内容说明

每个响应块中 `content` 的值根据块的内容类型不同而有所变化：

- 当块的内容类型为 `MESSAGE` 时：

    内容为一段文本。

- 当块的内容类型为 `CODE` 时：

    内容为一个 Markdown 格式的代码片段。

- 当块的内容类型为 `TABLE` 时：

    内容表示一个表格，包含以下内容：
    
    - `name`：`.csv` 文件名。
    
    - `url`：文件的 S3 键或 URL。
    
    - `expired_at`：`url` 的过期时间。为了将表格保留供以后使用，请确保在其过期之前下载它。

- 当块的内容类型为 `IMAGE` 时：

    内容表示一张图片，包含以下内容：
    
    - `name`：图片名称。
    
    - `url`：图片的 S3 键或 URL。
    
    - `expired_at`：`url` 的过期时间。为了将图片保留供以后使用，请确保在其过期之前下载它。

- 当块的内容类型为 `SOURCES` 时：

    内容表示响应块的来源，包含以下内容：

    - `source`：数据源的文件名。

    - `datasource_id`：数据源的 ID。

    - `dataset_id`：数据集的 ID。

    - `file_type`：数据源文件的扩展名。

- 当块的内容类型为 `QUESTIONS` 时：

    内容表示 MAXIR AI 提示的后续问题。

让我们来看一个示例。

有关 `POST /v2/jobs` 端点的详细信息，请参阅 [创建任务](/maxirai/API/api-reference/job?id=create-job)。


```python Python request
import requests

url = "http://<domain_name>/app/api/v2/team/jobs"

payload = {
    "session_id": "cxxdgegeegeg3433fff",
    "user_id": "tmm-dafasdfasdfasdf",
    "stream": True,
    "question": "出发城市和到达城市的组合中，哪一对组合最为常见",
    "dataset_id": "cm1gjmg8e0057r3x22v1fdu8m",
    "datasource_ids": ["cm1gjmmoo0001h0x24uk1xgu9"],
    "output_language": "ZH-CN",
    "job_mode": "AUTO"
}
headers = {
    "x-pd-api-key": "<api-key>",
    "Content-Type": "application/json"
}

response = requests.request("POST", url, json=payload, headers=headers)

print(response.text)
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```

响应示例如下【流式响应】:

```
event:JOB_ID
data:job-cm7391oea00cb01l1auht64zg

id:a928b4fd-39b3-4da3-af21-43b4469cc3b8
event:TASK
data:{"id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","model":"","choices":[{"delta":{"content":{"name":"分析","id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","status":"running","parent_id":null,"stage":"Analyze","properties":{}}},"index":0,"finish_reason":null}],"created":1739445679,"group_id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","group_name":"分析","stage":"Analyze"}

id:a928b4fd-39b3-4da3-af21-43b4469cc3b8
event:TASK
data:{"id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","model":"","choices":[{"delta":{"content":{"name":"分析","id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","status":"running","parent_id":null,"stage":"Analyze","properties":{"files":""}}},"index":0,"finish_reason":null}],"created":1739445680,"group_id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","group_name":"分析","stage":"Analyze"}

id:a928b4fd-39b3-4da3-af21-43b4469cc3b8
event:TASK
data:{"id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","model":"","choices":[{"delta":{"content":{"name":"分析","id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","status":"done","parent_id":null,"stage":"Analyze","properties":{"files":""}}},"index":0,"finish_reason":null}],"created":1739445680,"group_id":"a928b4fd-39b3-4da3-af21-43b4469cc3b8","group_name":"分析","stage":"Analyze"}

id:86dd1800-cfd2-41da-b200-c910d1a14676
event:TASK
data:{"id":"86dd1800-cfd2-41da-b200-c910d1a14676","model":"","choices":[{"delta":{"content":{"name":"数据理解","id":"86dd1800-cfd2-41da-b200-c910d1a14676","status":"running","parent_id":null,"stage":"Analyze","properties":{}}},"index":0,"finish_reason":null}],"created":1739445684,"group_id":"86dd1800-cfd2-41da-b200-c910d1a14676","group_name":"数据理解","stage":"Analyze"}

id:fdd54fab-c86c-4296-875e-dd2fd726ef05
event:TASK
data:{"id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","model":"","choices":[{"delta":{"content":{"name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","status":"running","parent_id":null,"stage":"Analyze","properties":{"files":"junlan.csv"}}},"index":0,"finish_reason":null}],"created":1739445684,"group_id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","group_name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","stage":"Analyze"}

id:fdd54fab-c86c-4296-875e-dd2fd726ef05
event:TASK
data:{"id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","model":"","choices":[{"delta":{"content":{"name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","status":"running","parent_id":null,"stage":"Analyze","properties":{"files":"junlan.csv"}}},"index":0,"finish_reason":null}],"created":1739445684,"group_id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","group_name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","stage":"Analyze"}

id:-1
event:TASK
data:{"id":"-1","model":"","choices":[{"delta":{"content":{"name":"搜索参考文献","id":"3cd06d50-3035-48c5-9d4c-b78a36b8cba5","status":"running","parent_id":null,"stage":"Analyze","properties":{}}},"index":0,"finish_reason":null}],"created":1739445684,"group_id":"-1","group_name":"","stage":"Analyze"}

id:-1
event:TASK
data:{"id":"-1","model":"","choices":[{"delta":{"content":{"name":"搜索参考文献","id":"3cd06d50-3035-48c5-9d4c-b78a36b8cba5","status":"done","parent_id":null,"stage":"Analyze","properties":{}}},"index":0,"finish_reason":null}],"created":1739445687,"group_id":"-1","group_name":"","stage":"Analyze"}

id:86dd1800-cfd2-41da-b200-c910d1a14676
event:TASK
data:{"id":"86dd1800-cfd2-41da-b200-c910d1a14676","model":"","choices":[{"delta":{"content":{"name":"数据理解","id":"86dd1800-cfd2-41da-b200-c910d1a14676","status":"done","parent_id":null,"stage":"Analyze","properties":{}}},"index":0,"finish_reason":null}],"created":1739445687,"group_id":"86dd1800-cfd2-41da-b200-c910d1a14676","group_name":"数据理解","stage":"Analyze"}

id:fdd54fab-c86c-4296-875e-dd2fd726ef05
event:CODE
data:{"id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","model":"","choices":[{"delta":{"role":null,"content":"```python\n\nimport pandas as pd\n\ndef invoke(input_0: pd.DataFrame) -> pd.DataFrame:\n    # Group by '出发城市(城市三字码)' and '到达城市(城市三字码)' and count the occurrences\n    city_combinations = input_0.groupby(['出发城市(城市三字码)', '到达城市(城市三字码)']).size().reset_index(name='组合频次')\n    \n    # Sort the combinations by frequency in descending order\n    city_combinations = city_combinations.sort_values(by='组合频次', ascending=False).reset_index(drop=True)\n    \n    # Assign the result to the output variable\n    output = city_combinations\n    \n    return output\n\n```"},"index":0,"finish_reason":null}],"created":1739445687,"group_id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","group_name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","stage":"Analyze"}

id:fdd54fab-c86c-4296-875e-dd2fd726ef05
event:TABLE
data:{"id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","model":"","choices":[{"delta":{"content":{"name":"city_combination_data.csv","url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/e016fbc5-1aa4-4758-969e-21bd488639a3.csv","expired_at":"2025-02-13T11:31:28.255119Z"}},"index":0,"finish_reason":null}],"created":1739445688,"group_id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","group_name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","stage":"Analyze"}

id:fdd54fab-c86c-4296-875e-dd2fd726ef05
event:TASK
data:{"id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","model":"","choices":[{"delta":{"content":{"name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","status":"done","parent_id":null,"stage":"Analyze","properties":{"files":"junlan.csv"}}},"index":0,"finish_reason":null}],"created":1739445688,"group_id":"fdd54fab-c86c-4296-875e-dd2fd726ef05","group_name":"分析数据集中'出发城市(城市三字码)'和'到达城市(城市三字码)'的组合，统计每一对组合出现的频次，以找出最常见的组合。","stage":"Analyze"}

id:2175a00c-6474-40c5-9f2c-9759f9b5e30f
event:TASK
data:{"id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","model":"","choices":[{"delta":{"content":{"name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","status":"running","parent_id":null,"stage":"Analyze","properties":{"files":""}}},"index":0,"finish_reason":null}],"created":1739445688,"group_id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","group_name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","stage":"Analyze"}

id:2175a00c-6474-40c5-9f2c-9759f9b5e30f
event:TASK
data:{"id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","model":"","choices":[{"delta":{"content":{"name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","status":"running","parent_id":null,"stage":"Analyze","properties":{"files":""}}},"index":0,"finish_reason":null}],"created":1739445688,"group_id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","group_name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","stage":"Analyze"}

id:2175a00c-6474-40c5-9f2c-9759f9b5e30f
event:TASK
data:{"id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","model":"","choices":[{"delta":{"content":{"name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","status":"running","parent_id":null,"stage":"Analyze","properties":{"files":"city_combination_data.csv"}}},"index":0,"finish_reason":null}],"created":1739445688,"group_id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","group_name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","stage":"Analyze"}

id:2175a00c-6474-40c5-9f2c-9759f9b5e30f
event:CODE
data:{"id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","model":"","choices":[{"delta":{"role":null,"content":"```python\n\nimport pandas as pd\nimport matplotlib.pyplot as plt\nimport io\n\ndef invoke(city_combination_data: pd.DataFrame) -> io.BytesIO:\n    # Combine the departure and arrival city codes into a single column for easier plotting\n    city_combination_data['城市组合'] = city_combination_data['出发城市(城市三字码)'] + '-' + city_combination_data['到达城市(城市三字码)']\n    \n    # Sort the data by frequency and take the top 100 combinations\n    top_combinations = city_combination_data.sort_values(by='组合频次', ascending=False).head(100)\n    \n    # Plotting\n    fig, ax = plt.subplots(figsize=(12, 8))\n    ax.bar(top_combinations['城市组合'], top_combinations['组合频次'])\n    \n    # Set labels and title\n    ax.set_xlabel('城市组合')\n    ax.set_ylabel('组合频次')\n    ax.set_title('Top 100 城市组合频次')\n    \n    # Rotate x-axis labels for better readability\n    plt.xticks(rotation=90)\n    \n    # Use tight layout to ensure everything fits without overlap\n    plt.tight_layout()\n    \n    # Save the plot to a BytesIO object\n    output = io.BytesIO()\n    plt.savefig(output, format='png')\n    plt.close(fig)\n    \n    # Seek to the beginning of the BytesIO object\n    output.seek(0)\n    \n    return output\n\n```"},"index":0,"finish_reason":null}],"created":1739445693,"group_id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","group_name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","stage":"Analyze"}

id:2175a00c-6474-40c5-9f2c-9759f9b5e30f
event:IMAGE
data:{"id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","model":"","choices":[{"delta":{"content":{"url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/314a04f6-9724-49a1-89c9-58d93d089928.png","name":"visualization.png","expiredAt":"2025-02-13T11:31:36.619369Z"}},"index":0,"finish_reason":null}],"created":1739445696,"group_id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","group_name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","stage":"Analyze"}

id:2175a00c-6474-40c5-9f2c-9759f9b5e30f
event:TASK
data:{"id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","model":"","choices":[{"delta":{"content":{"name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","status":"done","parent_id":null,"stage":"Analyze","properties":{"files":""}}},"index":0,"finish_reason":null}],"created":1739445696,"group_id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","group_name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","stage":"Analyze"}

id:2175a00c-6474-40c5-9f2c-9759f9b5e30f
event:TASK
data:{"id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","model":"","choices":[{"delta":{"content":{"name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","status":"done","parent_id":null,"stage":"Analyze","properties":{"files":""}}},"index":0,"finish_reason":null}],"created":1739445696,"group_id":"2175a00c-6474-40c5-9f2c-9759f9b5e30f","group_name":"对分析得到的城市组合频次数据进行可视化，使用柱状图展示每对城市组合的出现频次，以便直观地识别最常见的组合。","stage":"Analyze"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:TASK
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"content":{"name":"结论","id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","status":"running","parent_id":null,"stage":"Respond","properties":{}}},"index":0,"finish_reason":null}],"created":1739445696,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"\n\n`Analyzing Conclusions` \n\n"},"index":0,"finish_reason":null}],"created":1739445696,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"### 最常见的城市组合\n\n"},"index":0,"finish_reason":null}],"created":1739445701,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"#### 数据分析\n"},"index":0,"finish_reason":null}],"created":1739445701,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":""},"index":0,"finish_reason":null}],"created":1739445701,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"\n"},"index":0,"finish_reason":null}],"created":1739445701,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:TABLE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"content":{"name":"city_combination_data.csv","url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/e016fbc5-1aa4-4758-969e-21bd488639a3.csv","expired_at":"2025-02-13T11:31:41.357771Z"}},"index":0,"finish_reason":null}],"created":1739445701,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"\n\n"},"index":0,"finish_reason":null}],"created":1739445701,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"- **最常见的组合**: 出发城市为BKK，到达城市为HKT的组合最为常见。\n"},"index":0,"finish_reason":null}],"created":1739445702,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"- **组合频次**: 该组合的出现频次为116次。\n"},"index":0,"finish_reason":null}],"created":1739445702,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"- **其他高频组合**: 例如，HKT到BKK的组合频次为99次，TYO到SPK的组合频次为86次。\n\n"},"index":0,"finish_reason":null}],"created":1739445703,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"#### 数据可视化\n"},"index":0,"finish_reason":null}],"created":1739445703,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":""},"index":0,"finish_reason":null}],"created":1739445703,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"\n"},"index":0,"finish_reason":null}],"created":1739445703,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:IMAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"content":{"url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/314a04f6-9724-49a1-89c9-58d93d089928.png","name":"visualization.png","expiredAt":"2025-02-13T11:31:43.749427Z"}},"index":0,"finish_reason":null}],"created":1739445703,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"\n\n"},"index":0,"finish_reason":null}],"created":1739445703,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"- **柱状图展示**: 图中显示了前100个城市组合的频次，BKK到HKT的组合频次最高。\n"},"index":0,"finish_reason":null}],"created":1739445704,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"- **频次分布**: 频次从最高的116次逐渐递减，显示出明显的长尾分布。\n\n"},"index":0,"finish_reason":null}],"created":1739445704,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"#### 结论与见解\n"},"index":0,"finish_reason":null}],"created":1739445704,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"- **关键组合**: BKK到HKT是最常见的城市组合，可能由于旅游或商务活动频繁。\n"},"index":0,"finish_reason":null}],"created":1739445704,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:MESSAGE
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"role":null,"content":"- **数据应用**: 这些信息可以用于优化航班安排和资源分配，以提高运营效率。"},"index":0,"finish_reason":null}],"created":1739445705,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:QUESTIONS
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"content":["分析出发城市和到达城市组合的变化趋势，是否在不同日期有显著差异。","探讨出发城市和到达城市组合与航空公司名称之间的关系，是否某些航空公司更倾向于特定的城市组合。","研究出发城市和到达城市组合与出票数量和行李数量之间的相关性，是否某些组合会导致更高的出票或行李数量。"]},"index":0,"finish_reason":null}],"created":1739445705,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:TRIGGER
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"content":{"name":"conclusion_slice","arguments":{"answer":"$answer"}}},"index":0,"finish_reason":null}],"created":1739445705,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:6792407b-daab-4acc-9d3f-6eb8e0d29ae2
event:TASK
data:{"id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","model":"","choices":[{"delta":{"content":{"name":"结论","id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","status":"done","parent_id":null,"stage":"Respond","properties":{}}},"index":0,"finish_reason":null}],"created":1739445705,"group_id":"6792407b-daab-4acc-9d3f-6eb8e0d29ae2","group_name":"结论","stage":"Respond"}

id:-1
event:SOURCES
data:{"id":"-1","model":"","choices":[{"delta":{"content":[{"id":"1","source":"junlan.csv","page_no":null,"content":null,"datasource_id":"ds-cm5clulg2000101fcumw33r4k","dataset_id":"dset-cm5clulda02s401l133osoja3","file_type":"csv","external_id":"4444"}]},"index":0,"finish_reason":null}],"created":1739445705,"group_id":"-1","group_name":"","stage":"Analyze"}

event:END_MARK
data:[DONE]
```
</Accordion>

如果不启用流式传输，MAXIR AI 仅在整个响应准备好后才返回响应。

我们以相同的请求为例进行清晰的对比。唯一的不同是将 `stream` 设置为 `False`。


```python
import requests

url = "https://<domain_name>/app/api/v2/team/jobs"

payload = {
    "session_id": "cxxdgegeegeg3433fff",
    "user_id": "tmm-dafasdfasdfasdf",
    "stream": False,
    "question": "出发城市和到达城市的组合中，哪一对组合最为常见？",
    "dataset_id": "cm1gjmg8e0057r3x22v1fdu8m",
    "datasource_ids": ["cm1gjmmoo0001h0x24uk1xgu9"],
    "output_language": "ZH-CN",
    "job_mode": "AUTO"
}
headers = {
    "x-pd-api-key": "$PROJECT_API_KEY",
    "Content-Type": "application/json"
}

response = requests.request("POST", url, json=payload, headers=headers)

print(response.text)
```


响应示例如下【非流式响应】：
```json   
{"code":0,"msg":null,"data":{"job_id":"job-cm738zrbt00bv01l13uy02cdk","blocks":[{"type":"CODE","content":"```python\n\nimport pandas as pd\n\ndef invoke(input_0: pd.DataFrame) -> pd.DataFrame:\n    # Group by the combination of departure and arrival cities and count occurrences\n    city_combinations = input_0.groupby(['出发城市(城市三字码)', '到达城市(城市三字码)']).size().reset_index(name='组合出现次数')\n    \n    # Sort the combinations by the count in descending order to find the most common ones\n    city_combinations_sorted = city_combinations.sort_values(by='组合出现次数', ascending=False).reset_index(drop=True)\n    \n    # Assign the result to the output variable\n    output = city_combinations_sorted\n    \n    return output\n\n```","group_id":"327b3b90-c947-4efb-be74-845b6edf29e2","group_name":"分析数据中出发城市(城市三字码)和到达城市(城市三字码)的组合，统计每一对组合出现的次数，以找出最常见的组合。","stage":"Analyze"},{"type":"TABLE","content":{"url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/e8a4d119-edc1-4877-a8e4-379ca9a99dc6.csv","name":"city_combination_data.csv","expired_at":"2025-02-13T11:30:15.453397Z"},"group_id":"327b3b90-c947-4efb-be74-845b6edf29e2","group_name":"分析数据中出发城市(城市三字码)和到达城市(城市三字码)的组合，统计每一对组合出现的次数，以找出最常见的组合。","stage":"Analyze"},{"type":"CODE","content":"```python\n\nimport pandas as pd\nimport matplotlib.pyplot as plt\nimport io\n\ndef invoke(city_combination_data: pd.DataFrame) -> io.BytesIO:\n    # Sort the data by '组合出现次数' in descending order and take the top 100\n    top_combinations = city_combination_data.sort_values(by='组合出现次数', ascending=False).head(100)\n    \n    # Create a new column for the city combination\n    top_combinations['城市组合'] = top_combinations['出发城市(城市三字码)'] + '-' + top_combinations['到达城市(城市三字码)']\n    \n    # Plotting\n    fig, ax = plt.subplots(figsize=(12, 8))\n    ax.bar(top_combinations['城市组合'], top_combinations['组合出现次数'])\n    \n    # Set labels and title\n    ax.set_xlabel('城市组合')\n    ax.set_ylabel('组合出现次数')\n    ax.set_title('最常见的城市组合出现频率')\n    \n    # Rotate x-axis labels for better readability\n    plt.xticks(rotation=90)\n    \n    # Use tight layout for better spacing\n    plt.tight_layout()\n    \n    # Save the plot to a BytesIO object\n    output = io.BytesIO()\n    plt.savefig(output, format='png')\n    plt.close(fig)\n    \n    # Seek to the beginning of the BytesIO object\n    output.seek(0)\n    \n    return output\n\n```","group_id":"f443de5b-1960-42be-bef8-3c4d4c5c011e","group_name":"对出发城市和到达城市的组合出现频率进行可视化，使用柱状图展示最常见的城市组合。","stage":"Analyze"},{"type":"IMAGE","content":{"url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/30c8fa22-d666-4e82-8682-352b8dc1982d.png","name":"visualization.png","expired_at":"2025-02-13T11:30:15.453397Z"},"group_id":"f443de5b-1960-42be-bef8-3c4d4c5c011e","group_name":"对出发城市和到达城市的组合出现频率进行可视化，使用柱状图展示最常见的城市组合。","stage":"Analyze"},{"type":"MESSAGE","content":"\n\n`Analyzing Conclusions` \n\n### 最常见的城市组合\n\n#### 数据分析\n\n","group_id":"2ddd5382-f302-4f33-917e-124d9c925eab","group_name":"结论","stage":"Respond"},{"type":"TABLE","content":{"url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/e8a4d119-edc1-4877-a8e4-379ca9a99dc6.csv","name":"city_combination_data.csv","expired_at":"2025-02-13T11:30:15.453397Z"},"group_id":"2ddd5382-f302-4f33-917e-124d9c925eab","group_name":"结论","stage":"Respond"},{"type":"MESSAGE","content":"\n\n- **最常见的组合**: 出发城市为BKK，到达城市为HKT，出现次数为116次。\n- **其他高频组合**: HKT到BKK出现99次，TYO到SPK出现86次，CNX到BKK出现76次，BKK到CNX出现73次。\n\n#### 可视化分析\n\n","group_id":"2ddd5382-f302-4f33-917e-124d9c925eab","group_name":"结论","stage":"Respond"},{"type":"IMAGE","content":{"url":"https://static.xxx.ai/tmp_datasource_cache/code_result/tmm-cm5ao3yoe00zm01l1u1e7p3pj/30c8fa22-d666-4e82-8682-352b8dc1982d.png","name":"visualization.png","expired_at":"2025-02-13T11:30:15.453397Z"},"group_id":"2ddd5382-f302-4f33-917e-124d9c925eab","group_name":"结论","stage":"Respond"},{"type":"MESSAGE","content":"\n\n- **柱状图展示**: 图中显示了各城市组合的出现频率，BKK到HKT的组合明显高于其他组合。\n\n#### 结论与见解\n- **最常见的城市组合**: BKK到HKT是最常见的城市组合。\n- **高频航线**: 这些高频组合可能反映了热门航线或旅游目的地的趋势。","group_id":"2ddd5382-f302-4f33-917e-124d9c925eab","group_name":"结论","stage":"Respond"},{"type":"SOURCES","content":[{"source":"junlan.csv","datasource_id":"ds-cm5clulg2000101fcumw33r4k","dataset_id":"dset-cm5clulda02s401l133osoja3","file_type":"csv","external_id":"4444"}],"group_id":"","group_name":"","stage":"Respond"},{"type":"QUESTIONS","content":["分析出发城市和到达城市组合的变化趋势，是否在不同日期有显著差异。","探讨出发城市和到达城市组合与航空公司名称之间的关系，是否某些航空公司更倾向于特定的城市组合。","研究出发城市和到达城市组合与出票数量和行李数量之间的相关性，是否某些组合会导致更高的出票或行李数量。"],"group_id":"-1","group_name":null,"stage":"Respond"}]}}

```
</Accordion>


​    
