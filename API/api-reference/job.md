# 任务

## Create job

POST /v2/team/jobs

分析或探索您的数据。您可以通过此接口根据您的数据进行提问，立即获取洞察。

在 MAXIR AI 中，**任务（Job）** 即 MAXIR AI 根据您的请求（例如提示词或其他工作流）而生成并执行的问答过程。

> Body 请求参数

```json
{
  "session_id": "cxxdgegeegeg3433fff",
  "user_id": "tmm-dafasdfasdfasdf",
  "stream": true,
  "question": "Hello World",
  "dataset_id": "cm1gjmg8e0057r3x22v1fdu8m",
  "datasource_ids": [
    "cm1gjmmoo0001h0x24uk1xgu9"
  ],
  "output_language": "AUTO",
  "job_mode": "AUTO"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 ||none|
|» session_id|body|string| 是 ||会话 ID。|
|» stream|body|boolean| 否 ||是否使用流式返回。如果设置为 `true`，MAXIR AI 将实时向客户端发送回答更新，持续传输实时可用的数据。如果设置为 `false`，则在整个回答完成后，一次性返回完整的响应。|
|» question|body|string| 是 ||您的问题（即提示词）。|
|» dataset_id|body|string| 否 ||任务关联的数据集 ID。 |
|» datasource_ids|body|string| 否 ||指定在任务中使用的数据源的 ID。最多可以指定 1,000 个数据源。|
|» output_language|body|string| 否 ||MAXIR AI 的回复语言。例如，当设置为 `ZH-CN` 时，则 MAXIR AI 以简体中文回复。可选值包括：|
|» job_mode|body|string| 否 ||任务类型。可选值包括：|
|» user_id|body|string| 是 ||用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**» session_id**: 会话 ID。

如需查看项目中存在的会话，请调用 [GET /v2team/sessions](/maxirai/API/api-reference/session?id=list-sessions) 接口。

**» stream**: 是否使用流式返回。如果设置为 `true`，MAXIR AI 将实时向客户端发送回答更新，持续传输实时可用的数据。如果设置为 `false`，则在整个回答完成后，一次性返回完整的响应。

如不指定，则使用默认值 `false`。

关于如何理解流式返回的内容，请参考 [流式返回中的内容说明](/maxirai/API/introduction/streaming?id=如何理解流式响应)。

**» dataset_id**: 任务关联的数据集 ID。 

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

**» output_language**: MAXIR AI 的回复语言。例如，当设置为 `ZH-CN` 时，则 MAXIR AI 以简体中文回复。可选值包括：

- `AUTO`：根据提示词自动识别
- `EN`：英语
- `ES`：西班牙语
- `AR`：阿拉伯语
- `PT`：葡萄牙语
- `ID`：印尼语
- `JA`：日语
- `RU`：俄语
- `HI`：印地语
- `FR`：法语
- `DE`：德语
- `VI`：越南语
- `TR`：土耳其语
- `PL`：波兰语
- `IT`：意大利语
- `KO`：韩语
- `ZH-CN`：简体中文
- `ZH-TW`：繁体中文

**» job_mode**: 任务类型。可选值包括：

- `AUTO`：MAXIR AI 根据你的提问进行意图判断，决定该任务为数据分析任务或信息检索任务。
- `DATA_ANALYTICS`：MAXIR AI 会根据你的输入进行数据分析。

如不指定，则使用会话的 `job_mode` 设置。

#### 枚举值

|属性|值|
|---|---|
|» output_language|AUTO|
|» output_language|EN|
|» output_language|ES|
|» output_language|AR|
|» output_language|PT|
|» output_language|ID|
|» output_language|JA|
|» output_language|RU|
|» output_language|HI|
|» output_language|FR|
|» output_language|DE|
|» output_language|VI|
|» output_language|TR|
|» output_language|PL|
|» output_language|IT|
|» output_language|KO|
|» output_language|ZH-CN|
|» output_language|ZH-TW|
|» job_mode|AUTO|
|» job_mode|DATA_ANALYTICS|

> 返回示例

```json
{
  "code": 0,
  "data": {
    "job_id": "job-cm3ikdeuj02zk01l1yeuirt77",
    "blocks": [
      {
        "type": "CODE",
        "content": "```python\n\nimport pandas as pd\n\ndef invoke(input_0: pd.DataFrame) -> pd.DataFrame:\n    '''\n    input_0: pd.DataFrame  makeovermonday-a-century-of-global-deaths-from-disasters_decadal-deaths-disasters-type.csv\n    '''\n    # Group by 'Year' and sum the deaths for each type of disaster\n    aggregated_data = input_0.groupby('Year').sum().reset_index()\n    \n    # Select only the columns related to deaths\n    death_columns = [\n        'Deaths - Drought (decadal)', 'Deaths - Flood (decadal)', \n        'Deaths - Earthquake (decadal)', 'Deaths - Extreme weather (decadal)', \n        'Deaths - Extreme temperature (decadal)', 'Deaths - Volcanic activity (decadal)', \n        'Deaths - Wildfire (decadal)', 'Deaths - Glacial lake outburst flood (decadal)', \n        'Deaths - Dry mass movement (decadal)', 'Deaths - Wet mass movement (decadal)', \n        'Deaths - Fog (decadal)'\n    ]\n    \n    # Create a new DataFrame with the aggregated results\n    output = aggregated_data[['Year'] + death_columns]\n    \n    # Rename columns to be more descriptive\n    output.columns = ['Decade'] + [col.replace('Deaths - ', '').replace(' (decadal)', '') for col in death_columns]\n    \n    return output\n\n```",
        "group_id": "33063572-6e88-4912-8e2d-4166bcc8caee",
        "group_name": "Analyze the dataset to observe the trend of deaths caused by different types of natural disasters over the past century. This involves aggregating the data by decade and calculating the total number of deaths for each type of disaster to identify any changes in trends.",
        "stage": "Analyze"
      },
      {
        "type": "TABLE",
        "content": {
          "url": "https://static.xxx.ai/tmp_datasource_cache/code_result/cm37bchx106e301l1v9yf67yc/e24b6a5f-fdb8-48ca-ae35-dc91ac8e8ef7.csv",
          "name": "trend_data.csv",
          "expires_at": "2024-11-21T09:56:34.290544Z"
        },
        "group_id": "33063572-6e88-4912-8e2d-4166bcc8caee",
        "group_name": "Analyze the dataset to observe the trend of deaths caused by different types of natural disasters over the past century. This involves aggregating the data by decade and calculating the total number of deaths for each type of disaster to identify any changes in trends.",
        "stage": "Analyze"
      },
      {
        "type": "IMAGE",
        "content": {
          "url": "https://static.xxx.ai/tmp_datasource_cache/code_result/cm37bchx106e301l1v9yf67yc/81b75a33-a223-4954-9680-9f397872c8ad.png",
          "name": "Trend of Deaths from Natural Disasters Over the Century",
          "expires_at": "2024-11-21T09:56:34.290544Z"
        },
        "group_id": "7501680b-5879-441b-bd96-f58b1029ae17",
        "group_name": "Visualize the trend data to show how the number of deaths from different types of natural disasters has changed over the past century. Use line charts to represent the trends for each disaster type, which will help in understanding the impact of measures and technological advancements on reducing deaths.",
        "stage": "Analyze"
      },
      {
        "type": "MESSAGE",
        "content": "\n\n`Analyzing Conclusions` \n\n### Analysis of Trends in the Number of Deaths from Natural Disasters \n\n#### Data Analysis\n\n",
        "group_id": "b842aca7-6fd5-4190-85fa-97085e473877",
        "group_name": "Conclusions",
        "stage": "Respond"
      },
      {
        "type": "TABLE",
        "content": {
          "url": "https://static.xxx.ai/tmp_datasource_cache/code_result/cm37bchx106e301l1v9yf67yc/e24b6a5f-fdb8-48ca-ae35-dc91ac8e8ef7.csv",
          "name": "trend_data.csv",
          "expires_at": "2024-11-21T09:56:34.290544Z"
        },
        "group_id": "b842aca7-6fd5-4190-85fa-97085e473877",
        "group_name": "Conclusions",
        "stage": "Respond"
      },
      {
        "type": "MESSAGE",
        "content": "\n\n- **Droughts and Floods**: In the early 20th century, droughts and floods caused extremely high death tolls, particularly during the 1920s and 1930s.\n- **Earthquakes and Extreme Weather**: Earthquakes and extreme weather also led to significant death tolls throughout the century, especially in the 1970s and 1990s.\n- **Extreme Temperatures and Volcanic Activity**: These disasters had relatively lower death tolls, but in certain decades, such as the 2000s, deaths caused by extreme temperatures increased.\n\n#### Trend Visualization\n\n",
        "group_id": "b842aca7-6fd5-4190-85fa-97085e473877",
        "group_name": "Conclusions",
        "stage": "Respond"
      },
      {
        "type": "IMAGE",
        "content": {
          "url": "https://static.xxx.ai/tmp_datasource_cache/code_result/cm37bchx106e301l1v9yf67yc/81b75a33-a223-4954-9680-9f397872c8ad.png",
          "name": "Trend of Deaths from Natural Disasters Over the Century",
          "expires_at": "2024-11-21T09:56:34.290544Z"
        },
        "group_id": "b842aca7-6fd5-4190-85fa-97085e473877",
        "group_name": "Conclusions",
        "stage": "Respond"
      },
      {
        "type": "MESSAGE",
        "content": "\n\n- **Overall Trend**: The chart shows that although certain decades experienced spikes in death tolls caused by natural disasters, the overall trend is declining.\n- **Impact of Technology and Measures**: Over time, advancements in technology and the implementation of disaster prevention measures are likely key factors in reducing death tolls.\n\n#### Conclusions and Insights\n- **Technological Advancements**: Modern technological progress, such as improved early warning systems and better construction techniques, may have reduced the fatalities caused by earthquakes and extreme weather.\n- **Disaster Prevention Measures**: The enhancement of disaster prevention measures and emergency response capabilities on a global scale has likely contributed to the decreased fatality rates of natural disasters.",
        "group_id": "b842aca7-6fd5-4190-85fa-97085e473877",
        "group_name": "Conclusions",
        "stage": "Respond"
      },
      {
        "type": "SOURCES",
        "content": [
          {
            "source": "makeovermonday-a-century-of-global-deaths-from-disasters_decadal-deaths-disasters-type.csv",
            "datasource_id": "clxin6l9200oo01l1457bolx3",
            "dataset_id": "clxin6l8400ok01l1ff2m0s25",
            "file_type": "csv"
          }
        ],
        "group_id": "",
        "group_name": "",
        "stage": "Respond"
      },
      {
        "type": "QUESTIONS",
        "content": [
          "Analyze the trends in death tolls from different types of natural disasters over the past century and explore which disaster types have shown the most significant reduction in fatalities.",
          "Study the technological advancements and measures in responding to natural disasters across different regions globally, and analyze how these differences have influenced changes in death tolls in each region.",
          "Explore how future technological advancements and policy measures could further reduce fatalities caused by natural disasters, and assess their feasibility and potential impacts."
        ],
        "group_id": "-1",
        "stage": "Respond"
      }
    ]
  }
}
```

```json
{
  "code": 210021,
  "msg": "Job quota exceeded"
}
```

```json
{
  "code": 1003,
  "msg": "insufficient.authentication"
}
```

```json
{
  "code": 210020,
  "msg": "Something went wrong during job execution. Please try again."
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|none|Inline|
|403|[Forbidden](https://tools.ietf.org/html/rfc7231#section-6.5.3)|none|Inline|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|true|none||任务对象。|
|»» job_id|string|true|none||任务 ID，即会话中该任务的唯一标识。|
|»» blocks|object|true|none||构成完整答案的答案块列表。|
|»»» type|string|true|none||答案块（Block）的内容类型。可能值为：<br /><br />- `MESSAGE`：表示内容是文本。<br />- `CODE`：表示内容是代码片段。<br />- `TABLE`：表示内容是表格。<br />- `IMAGE`：表示内容是图片。<br />- `SOURCE`：表示内容是答案块的参考来源。<br />- `QUESTIONS`：即 MAXIR AI 生成的建议问题，帮助引导您后续的数据探索和分析。|
|»»» content|string|true|none||答案块的内容，随 `type` 值而异：<br /><br />- 当 `type` 为 `MESSAGE` 时，内容为一段文本。<br />- 当 `type` 为 `CODE` 时，内容为 Markdown 格式表示的代码片段。<br />- 当 `type` 为 `TABLE` 时，内容为表格，包含如下构成参数：<br />    - `name`：`.csv` 文件的名称。<br />    - `url`：文件的 S3 Key 或 URL。<br />    - `expires_at`：`url` 的过期时间。如需保存表格方便后续使用，请确保在 URL 过期之前完成下载。<br />- 当 `type` 为 `IMAGE` 时，内容为一张图片，包含如下构成参数：<br />    - `name`：图片的名称。<br />    - `url`：图片的 S3 Key 或 URL。<br />    - `expires_at`：`url` 的过期时间。如需保存图片方便后续使用，请确保在 URL 过期之前完成下载。<br />- 当 `type` 为 `SOURCE` 时，内容为答案块的参考来源，包含如下构成参数：<br />    - `source`：数据源的文件名。<br />    - `datasource_id`：数据源 ID。<br />    - `dataset_id`：数据集 ID。<br />    - `file_type`：数据源的文件扩展名。<br />- 当 `type` 为 `QUESTIONS` 时，内容为 MAXIR AI 生成的建议问题，帮助您引导后续的数据探索与分析。|
|»»» group_id|string|true|none||答案块所在的分组 ID。|
|»»» group_name|string|true|none||答案块所在的分组名称。|
|»»» stage|string|true|none||MAXIR AI 生成答案的过程分为两个阶段：`Analyze` 和 `Respond`。  <br /><br />- `Analyze` 阶段的答案块并非最终答案的构成部分，为分析过程中的输出，旨在帮助您理解答案的生成方式。<br />- `Respond` 阶段的答案块是 MAXIR AI 基于您的问题生成的最终答复。|

#### 枚举值

|属性|值|
|---|---|
|type|MESSAGE|
|type|CODE|
|type|TABLE|
|type|SOURCES|
|type|QUESTIONS|
|stage|Analyze|
|stage|Respond|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
