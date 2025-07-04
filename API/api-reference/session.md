# 会话

## Create session

POST /v2/team/sessions

创建会话。
会话即用户与 MAXIR AI 在单次对话中的持续交互过程。每个用户至多可以同时保留 150 个会话。达到此上限后，如需创建新会话，可以删除不再使用的会话后，再进行创建。

> Body 请求参数

```json
{
  "name": "My session",
  "output_language": "FR",
  "job_mode": "AUTO",
  "max_contextual_job_history": 10,
  "agent_id": "DATA_ANALYSIS_AGENT",
  "user_id": "tmm-dafasdfasdfasdf"
}
```

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 |none|
|» name|body|string| 是 |会话名称，至多可支持 128 个字符。如超过此限制，名称将被截断展示。|
|» output_language|body|string| 否 |MAXIR AI 的回复语言。例如，当设置为 `EN` 时，则 MAXIR AI 使用英语进行回复。如不指定，将使用默认值 `AUTO`，即 MAXIR AI 会根据提示词自动选择合适的回复语言。可选值包括：|
|» job_mode|body|string| 否 |任务类型。可选值包括：|
|» max_contextual_job_history|body|integer| 否 |作为下一个任务上下文的历史任务数，取值范围为 `0` 到 `10`，默认值为 `10`。如设置为 `0`，则当前任务将不使用任何历史任务作为上下文信息。|
|» agent_id|body|string| 否 |AI 智能体的 ID。该参数为保留参数，无需制定或设置为 **DATA_ANALYSIS_AGENT**。|
|» user_id|body|string| 是 |用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**» output_language**: MAXIR AI 的回复语言。例如，当设置为 `EN` 时，则 MAXIR AI 使用英语进行回复。如不指定，将使用默认值 `AUTO`，即 MAXIR AI 会根据提示词自动选择合适的回复语言。可选值包括：

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

如不指定，则采用默认值 `AUTO`。如果您明确地想要进行数据分析，我们建议您将值设置为 **DATA_ANALYTICS**，从而 MAXIR AI 可以跳过意图识别步骤，加快问题处理速度。

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
|» agent_id|DATA_ANALYSIS_AGENT|

> 返回示例

```json
{
  "code": 0,
  "data": {
    "id": "session-dasfasdgasdgfasdgasdg"
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|true|none||返回的数据对象。|
|»» id|string|true|none||会话 ID。<br /><br />如后续要使用该会话来 [执行任务](/maxirai/API/api-reference/job?id=create-job)，请保存该 ID。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## List sessions

GET /v2/team/sessions

返回您创建的会话列表。

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|page_number|query|integer| 否 |分页返回的起始页码。如不指定，则使用默认值 `1`。|
|page_size|query|integer| 否 |每页返回的记录数量。如不指定，则使用默认值 `10`。|
|search|query|string| 否 |搜索关键词，最多支持 128 个字符。所有名称中包含该关键词的会话将被返回。|
|user_id|query|string| 是 |用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

#### 详细说明

**search**: 搜索关键词，最多支持 128 个字符。所有名称中包含该关键词的会话将被返回。

如果省略此项，将列出所有会话。

> 返回示例

```json
{
  "code": 0,
  "data": {
    "total_items": 8,
    "page_size": 10,
    "page_number": 1,
    "records": [
      {
        "id": "04b69928-532d-408a-8716-8f5e70fcacc6",
        "name": "Finding Your Inner Spark",
        "output_language": "AUTO",
        "job_mode": "AUTO",
        "max_contextual_job_history": 10,
        "agent_id": "DATA_ANALYSIS_AGENT"
      },
      {
        "id": "7e9941af-ad1b-4834-a421-4b37b54aae19",
        "name": "session_trade_analyze_001",
        "output_language": "EN",
        "job_mode": "AUTO",
        "max_contextual_job_history": 1,
        "agent_id": "DATA_ANALYSIS_AGENT"
      },
      {
        "id": "a137ecc8-9554-42c3-b042-2458dd2aeb36",
        "name": "My first session",
        "output_language": "AUTO",
        "job_mode": "AUTO",
        "max_contextual_job_history": 10,
        "agent_id": "DATA_ANALYSIS_AGENT"
      },
      {
        "id": "4440ab38-3df0-465b-a66c-bf6acb0f1bc2",
        "name": "New name",
        "output_language": "EN",
        "job_mode": "AUTO",
        "max_contextual_job_history": 10,
        "agent_id": "DATA_ANALYSIS_AGENT"
      },
      {
        "id": "3eb5e294-11eb-4bf0-8854-7177c415d15c",
        "name": "trade_analyze_001",
        "output_language": "ZH-CN",
        "job_mode": "AUTO",
        "max_contextual_job_history": 10,
        "agent_id": "DATA_ANALYSIS_AGENT"
      },
      {
        "id": "ccca6d34-894f-4f6f-a8b5-c9df93126309",
        "name": "lhtest-old",
        "output_language": "ZH-CN",
        "job_mode": "AUTO",
        "max_contextual_job_history": 5,
        "agent_id": "DATA_ANALYSIS_AGENT"
      },
      {
        "id": "c28a1f47-da68-4456-bb0b-e85bf5a0e98e",
        "name": "lhtest-newest",
        "output_language": "ZH-TW",
        "job_mode": "AUTO",
        "max_contextual_job_history": 6,
        "agent_id": "DATA_ANALYSIS_AGENT"
      },
      {
        "id": "fbde9b45-69cd-408e-97d1-f874236aadac",
        "name": "General-session",
        "output_language": "ZH-CN",
        "job_mode": "AUTO",
        "max_contextual_job_history": 10,
        "agent_id": "DATA_ANALYSIS_AGENT"
      }
    ]
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|true|none||会话的分页列表。|
|»» total_items|integer|true|none||返回的会话总数量。|
|»» page_number|integer|true|none||当前页面的页码。|
|»» page_size|integer|true|none||每页返回的会话数量。|
|»» records|object|true|none||当前页面返回的会话列表。|
|»»» id|string|true|none||会话 ID，会话在当前项目中的唯一标识。|
|»»» name|string|true|none||会话名称。如名称长度超过 128 个字符，将仅返回前 128 个字符。|
|»»» output_language|string|false|none||MAXIR AI 的返回语言。可能值包括：<br /><br />- `AUTO`：根据提示词自动识别<br />- `EN`：英语<br />- `ES`：西班牙语<br />- `AR`：阿拉伯语<br />- `PT`：葡萄牙语<br />- `ID`：印尼语<br />- `JA`：日语<br />- `RU`：俄语<br />- `HI`：印地语<br />- `FR`：法语<br />- `DE`：德语<br />- `VI`：越南语<br />- `TR`：土耳其语<br />- `PL`：波兰语<br />- `IT`：意大利语<br />- `KO`：韩语<br />- `ZH-CN`：简体中文<br />- `ZH-TW`：繁体中文|
|»»» job_mode|string|false|none||任务类型。可能值包括：<br /><br />- `AUTO`：MAXIR AI 根据您的提问进行意图判断，决定任务是数据分析任务还是信息检索任务。<br />- `DATA_ANALYTICS`：MAXIR AI 根据您的输入进行数据分析。|
|»»» max_contextual_job_history|integer|false|none||在会话中保留作为下一个任务上下文的任务数量，取值范围为 `0` 到 `10`，默认值为 `10`。值为 `0` 时表示会话中的任务在执行时不会使用历史任务执行结果作为上下文信息。|
|»»» agent_id|string|false|none||AI 智能体的 ID。该参数为保留参数，无实际意义，请忽略。|

#### 枚举值

|属性|值|
|---|---|
|output_language|AUTO|
|output_language|EN|
|output_language|ES|
|output_language|AR|
|output_language|PT|
|output_language|ID|
|output_language|JA|
|output_language|RU|
|output_language|HI|
|output_language|FR|
|output_language|DE|
|output_language|VI|
|output_language|TR|
|output_language|PL|
|output_language|IT|
|output_language|KO|
|output_language|ZH-CN|
|output_language|ZH-TW|
|job_mode|AUTO|
|job_mode|DATA_ANALYTICS|
|agent_id|DATA_ANALYSIS_AGENT|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Modify session

POST /v2/team/sessions/{id}

修改会话配置。

> Body 请求参数

```json
{
  "name": "hello",
  "output_language": "EN",
  "job_mode": "DATA_ANALYSIS",
  "max_contextual_job_history": 3,
  "user_id": "tmm-ddsdfsfgs"
}
```

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |要修改的会话 ID。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 |none|
|» name|body|string| 是 |会话名称，至多可支持 128 个字符。如超过此限制，名称将被截断展示。|
|» output_language|body|string| 否 |MAXIR AI 的回复语言。例如，当设置为 `EN` 时，则 MAXIR AI 使用英语进行回复。如不指定，将使用默认值 `AUTO`，即 MAXIR AI 会根据提示词自动选择合适的回复语言。可选值包括：|
|» job_mode|body|string| 否 |任务类型。可选值包括：|
|» max_contextual_job_history|body|integer| 否 |作为下一个任务上下文的历史任务数，取值范围为 `0` 到 `10`，默认值为 `10`。如设置为 `0`，则当前任务将不使用任何历史任务作为上下文信息。|
|» user_id|body|string| 是 |用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**id**: 要修改的会话 ID。

如需查看项目中存在的会话，请调用 [GET /v2/team/sessions](/maxirai/API/api-reference/session?id=list-sessions) 接口。

**» output_language**: MAXIR AI 的回复语言。例如，当设置为 `EN` 时，则 MAXIR AI 使用英语进行回复。如不指定，将使用默认值 `AUTO`，即 MAXIR AI 会根据提示词自动选择合适的回复语言。可选值包括：

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

如不指定，则采用默认值 `AUTO`。如果您明确地想要进行数据分析，我们建议您将值设置为 **DATA_ANALYTICS**，从而 MAXIR AI 可以跳过意图识别步骤，加快问题处理速度。

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

> 200 Response

```json
{
  "code": 0,
  "data": {}
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|true|none||操作成功则返回 null。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Delete session

DELETE /v2/team/sessions/{id}

删除会话。一旦删除，该会话及其任务历史记录将被永久删除，且无法恢复。

> Body 请求参数

```json
{
  "user_id": "tmm-dafasdfasdfasdf"
}
```

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |要删除的会话 ID。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 |none|
|» user_id|body|string| 是 |用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**id**: 要删除的会话 ID。

如需查看项目中存在的会话，请调用 [GET /v2/team/sessions](/api-reference/list-sessions) 接口。

> 返回示例

> 200 Response

```json
{
  "code": 0,
  "data": {}
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|false|none||操作成功则返回 null。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Get session

GET /v2/team/sessions/{id}

获取指定会话的配置信息。

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |目标会话的 ID。|
|user_id|query|string| 是 |用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

#### 详细说明

**id**: 目标会话的 ID。

如需查看项目中存在的会话，请调用 [GET /v2/team/sessions](/api-reference/list-sessions) 接口。

> 返回示例

```json
{
  "code": 0,
  "data": {
    "id": "cckrXXg68P8lmb59dg4yO",
    "name": "Analyze performance",
    "output_language": "HI",
    "job_mode": "AUTO",
    "max_contextual_job_history": 76,
    "agent_id": "DATA_ANALYSIS_AGENT"
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|true|none||会话对象。|
|»» id|string|true|none||会话 ID，会话在当前项目中的唯一标识。|
|»» name|string|true|none||会话名称。如名称长度超过 128 个字符，将仅返回前 128 个字符。|
|»» output_language|string|false|none||MAXIR AI 的返回语言。可能值包括：<br /><br />- `AUTO`：根据提示词自动识别<br />- `EN`：英语<br />- `ES`：西班牙语<br />- `AR`：阿拉伯语<br />- `PT`：葡萄牙语<br />- `ID`：印尼语<br />- `JA`：日语<br />- `RU`：俄语<br />- `HI`：印地语<br />- `FR`：法语<br />- `DE`：德语<br />- `VI`：越南语<br />- `TR`：土耳其语<br />- `PL`：波兰语<br />- `IT`：意大利语<br />- `KO`：韩语<br />- `ZH-CN`：简体中文<br />- `ZH-TW`：繁体中文|
|»» job_mode|string|false|none||任务类型。可能值包括：<br /><br />- `AUTO`：MAXIR AI 根据您的提问进行意图判断，决定任务是数据分析任务还是信息检索任务。<br />- `DATA_ANALYTICS`：MAXIR AI 根据您的输入进行数据分析。|
|»» max_contextual_job_history|integer|false|none||在会话中保留作为下一个任务上下文的任务数量，取值范围为 `0` 到 `10`，默认值为 `10`。值为 `0` 时表示会话中的任务在执行时不会使用历史任务执行结果作为上下文信息。|
|»» agent_id|string|false|none||AI 智能体的 ID。该参数为保留参数，无实际意义，请忽略。|

#### 枚举值

|属性|值|
|---|---|
|output_language|AUTO|
|output_language|EN|
|output_language|ES|
|output_language|AR|
|output_language|PT|
|output_language|ID|
|output_language|JA|
|output_language|RU|
|output_language|HI|
|output_language|FR|
|output_language|DE|
|output_language|VI|
|output_language|TR|
|output_language|PL|
|output_language|IT|
|output_language|KO|
|output_language|ZH-CN|
|output_language|ZH-TW|
|job_mode|AUTO|
|job_mode|DATA_ANALYTICS|
|agent_id|DATA_ANALYSIS_AGENT|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Get job history in session

GET /v2/team/sessions/{id}/history

获取指定会话中的任务记录。

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |目标会话的 ID。|
|page_number|query|integer| 否 |分页返回的起始页码。如不指定，则使用默认值 `1`。|
|page_size|query|integer| 否 |每页返回的记录数量。如不指定，则使用默认值 `10`。|
|user_id|query|string| 是 |用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

#### 详细说明

**id**: 目标会话的 ID。

如需查看项目中存在的会话，请调用 [GET /v2/team/sessions](/maxirai/API/api-reference/session?id=list-sessions) 接口。

> 返回示例

```json
{
  "code": 0,
  "data": {
    "total_items": 1,
    "page_number": 1,
    "page_size": 10,
    "records": [
      {
        "job_id": "job-1dsfasddfasgddsaffds",
        "question": {
          "blocks": [
            {
              "type": "MESSAGE",
              "content": "Check for negative values across all sales columns"
            }
          ]
        },
        "answer": {
          "blocks": [
            {
              "type": "MESSAGE",
              "content": "- Check for negative values across all sales columns.\n- Filter the DataFrame to retain only rows with no negative sales values.",
              "group_id": "ba582fc9-bb36-4c5d-a8e8-d35bda6389cd",
              "group_name": "Identify the channels with negative sales values by examining each day's sales data. Filter out the rows where any sales value is negative.",
              "stage": "Analyze"
            },
            {
              "type": "CODE",
              "content": "```python\n\nimport pandas as pd\n\ndef invoke(input_0: pd.DataFrame) -> pd.DataFrame:\n    '''\n    input_0: pd.DataFrame  SalesByChannelByDay_Summary_Demo.Sheet1_0_table_1.csv\n    '''\n    # Identify columns that represent sales data (all except the first column)\n    sales_columns = input_0.columns[1:]\n    \n    # Filter rows where any sales value is negative\n    filtered_df = input_0[~(input_0[sales_columns] < 0).any(axis=1)]\n    \n    # Assign the result to the output variable\n    output = filtered_df\n    \n    return output\n\n```",
              "group_id": "ba582fc9-bb36-4c5d-a8e8-d35bda6389cd",
              "group_name": "Identify the channels with negative sales values by examining each day's sales data. Filter out the rows where any sales value is negative.",
              "stage": "Analyze"
            },
            {
              "type": "TABLE",
              "content": "https://s3.amazonaws.com/xxxtest/tmp_datasource_cache/code_result/clvl4cad2001q01l1m522hxlu/baf7d6d1-fb81-4fdb-bcdd-32923d214c7b.csv?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20241018T104617Z&X-Amz-SignedHeaders=host&X-Amz-Expires=599&X-Amz-Credential=AKIARLSQLXURHEIDN4OZ%2F20241018%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=9bcb5af552793f162e35f41d62fb9306cf90888924bfbdce81ea687265fddf83",
              "group_id": "ba582fc9-bb36-4c5d-a8e8-d35bda6389cd",
              "group_name": "Identify the channels with negative sales values by examining each day's sales data. Filter out the rows where any sales value is negative.",
              "stage": "Analyze"
            },
            {
              "type": "MESSAGE",
              "content": "- Sum the sales across all days for each channel.\n- Create a new DataFrame with the channel names and their corresponding total sales.",
              "group_id": "47183fd1-307b-4408-9986-e9238d952ec1",
              "group_name": "Calculate the overall sales trend for the identified channels with negative sales values. This involves summing up the sales across all days for each channel and analyzing the trend.",
              "stage": "Analyze"
            },
            {
              "type": "CODE",
              "content": "```python\n\nimport pandas as pd\n\ndef invoke(negative_sales_channels: pd.DataFrame) -> pd.DataFrame:\n    '''\n    negative_sales_channels: pd.DataFrame  negative_sales_channels.csv\n    '''\n    # Sum the sales across all days for each channel\n    total_sales = negative_sales_channels.iloc[:, 1:].sum(axis=1)\n    \n    # Create a new DataFrame with the channel names and their corresponding total sales\n    output = pd.DataFrame({\n        'Channel': negative_sales_channels.iloc[:, 0],\n        'Total Sales': total_sales\n    })\n    \n    return output\n\n```",
              "group_id": "47183fd1-307b-4408-9986-e9238d952ec1",
              "group_name": "Calculate the overall sales trend for the identified channels with negative sales values. This involves summing up the sales across all days for each channel and analyzing the trend.",
              "stage": "Analyze"
            },
            {
              "type": "TABLE",
              "content": "https://s3.amazonaws.com/xxxtest/tmp_datasource_cache/code_result/clvl4cad2001q01l1m522hxlu/10cffac2-8bf3-45f4-86e6-1ed8457329f2.csv?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20241018T104617Z&X-Amz-SignedHeaders=host&X-Amz-Expires=600&X-Amz-Credential=AKIARLSQLXURHEIDN4OZ%2F20241018%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=c6f5b522d2ddceea730304b86a45d5f5165f05f9fda3c1d275d11e9022c1e7ac",
              "group_id": "47183fd1-307b-4408-9986-e9238d952ec1",
              "group_name": "Calculate the overall sales trend for the identified channels with negative sales values. This involves summing up the sales across all days for each channel and analyzing the trend.",
              "stage": "Analyze"
            },
            {
              "type": "MESSAGE",
              "content": "- Replace any negative sales values with zero in the data.\n- Sum the sales across all days for each channel.\n- Create a new data structure with the summed sales values.",
              "group_id": "6b93c2b1-8908-4c2b-afb2-2a81f2d24739",
              "group_name": "Calculate the overall sales trend for the same channels but excluding the negative sales values. This involves setting negative values to zero or removing them and then summing up the sales across all days for each channel.",
              "stage": "Analyze"
            },
            {
              "type": "CODE",
              "content": "```python\n\nimport pandas as pd\n\ndef invoke(negative_sales_channels: pd.DataFrame) -> pd.DataFrame:\n    '''\n    negative_sales_channels: pd.DataFrame  negative_sales_channels.csv\n    '''\n    # Replace negative values with zero\n    negative_sales_channels.iloc[:, 1:] = negative_sales_channels.iloc[:, 1:].clip(lower=0)\n    \n    # Sum the sales across all days for each channel\n    sales_sum = negative_sales_channels.iloc[:, 1:].sum(axis=1)\n    \n    # Create a new DataFrame with the summed sales values\n    output = pd.DataFrame({\n        'Channel': negative_sales_channels.iloc[:, 0],\n        'Total Sales': sales_sum\n    })\n    \n    return output\n\n```",
              "group_id": "6b93c2b1-8908-4c2b-afb2-2a81f2d24739",
              "group_name": "Calculate the overall sales trend for the same channels but excluding the negative sales values. This involves setting negative values to zero or removing them and then summing up the sales across all days for each channel.",
              "stage": "Analyze"
            },
            {
              "type": "TABLE",
              "content": "https://s3.amazonaws.com/xxxtest/tmp_datasource_cache/code_result/clvl4cad2001q01l1m522hxlu/f4c99616-dd7c-48b1-8a35-d3141d732c36.csv?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20241018T104617Z&X-Amz-SignedHeaders=host&X-Amz-Expires=600&X-Amz-Credential=AKIARLSQLXURHEIDN4OZ%2F20241018%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=d21a8d939ab09547bc201754ba253ca6c0d1da5361752f2228237e3ff59be256",
              "group_id": "6b93c2b1-8908-4c2b-afb2-2a81f2d24739",
              "group_name": "Calculate the overall sales trend for the same channels but excluding the negative sales values. This involves setting negative values to zero or removing them and then summing up the sales across all days for each channel.",
              "stage": "Analyze"
            },
            {
              "type": "MESSAGE",
              "content": "- Merge the two datasets on the 'Channel' column to align sales data for comparison.\n- Calculate the difference in 'Total Sales' between the datasets for each channel.\n- Store the results, including channel name and calculated difference, in a new dataset.",
              "group_id": "3488f538-f7fc-4c0e-a265-b66e3a38d41e",
              "group_name": "Compare the sales trends with and without negative sales values to determine the impact of negative sales on the overall sales trend for the affected channels.",
              "stage": "Analyze"
            },
            {
              "type": "CODE",
              "content": "```python\n\nimport pandas as pd\n\ndef invoke(sales_trend_with_negatives: pd.DataFrame, sales_trend_without_negatives: pd.DataFrame) -> pd.DataFrame:\n    # Merge the two DataFrames on the 'Channel' column\n    merged_df = pd.merge(sales_trend_with_negatives, sales_trend_without_negatives, on='Channel', suffixes=('_with_negatives', '_without_negatives'))\n    \n    # Calculate the difference in 'Total Sales' between the two DataFrames\n    merged_df['Sales Difference'] = merged_df['Total Sales_without_negatives'] - merged_df['Total Sales_with_negatives']\n    \n    # Create a new DataFrame to store the results\n    output = merged_df[['Channel', 'Sales Difference']]\n    \n    return output\n\n```",
              "group_id": "3488f538-f7fc-4c0e-a265-b66e3a38d41e",
              "group_name": "Compare the sales trends with and without negative sales values to determine the impact of negative sales on the overall sales trend for the affected channels.",
              "stage": "Analyze"
            },
            {
              "type": "TABLE",
              "content": "https://s3.amazonaws.com/xxxtest/tmp_datasource_cache/code_result/clvl4cad2001q01l1m522hxlu/aaf4f2f7-e2db-4f2e-98ae-0bdd18f42333.csv?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20241018T104617Z&X-Amz-SignedHeaders=host&X-Amz-Expires=600&X-Amz-Credential=AKIARLSQLXURHEIDN4OZ%2F20241018%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=19fcfe97c70bf40292b920ecbf0299c101e1de11e49b54fb8f62934b6e874f52",
              "group_id": "3488f538-f7fc-4c0e-a265-b66e3a38d41e",
              "group_name": "Compare the sales trends with and without negative sales values to determine the impact of negative sales on the overall sales trend for the affected channels.",
              "stage": "Analyze"
            },
            {
              "type": "MESSAGE",
              "content": "\n\n`Analyzing Conclusions` \n\n### The impact of negative sales values on overall sales trends\n\n#### Sales variance analysis\n\n",
              "group_id": "fd1a62e6-48cf-4ac1-8bac-025665444710",
              "group_name": "Conclusions",
              "stage": "Respond"
            },
            {
              "type": "TABLE",
              "content": "https://s3.amazonaws.com/xxxtest/tmp_datasource_cache/code_result/clvl4cad2001q01l1m522hxlu/aaf4f2f7-e2db-4f2e-98ae-0bdd18f42333.csv?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20241018T104617Z&X-Amz-SignedHeaders=host&X-Amz-Expires=600&X-Amz-Credential=AKIARLSQLXURHEIDN4OZ%2F20241018%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=19fcfe97c70bf40292b920ecbf0299c101e1de11e49b54fb8f62934b6e874f52",
              "group_id": "fd1a62e6-48cf-4ac1-8bac-025665444710",
              "group_name": "Conclusions",
              "stage": "Respond"
            },
            {
              "type": "MESSAGE",
              "content": "\n\n- **Sales variance**：In all channels (including EC, JD, Tmall, WeChat, retail, corporate stores, outlets, and total), the sales difference is 0.0. This indicates that regardless of the presence of negative sales values, the sales trend has not changed.\n\n#### Conclusion and Insights\n- **The impact of negative sales values**：Based on the provided data, negative sales values have no impact on the overall sales trend of the affected channels, as the sales variance for all channels is 0.0.\n- **Data consistency**：The sales discrepancies across all channels are consistent, indicating that there are no anomalies or deviations caused by negative sales values during data processing or analysis.",
              "group_id": "fd1a62e6-48cf-4ac1-8bac-025665444710",
              "group_name": "Conclusions",
              "stage": "Respond"
            },
            {
              "type": "SOURCES",
              "content": [
                {
                  "source": "SalesByChannelByDay_Summary_Demo.xlsx",
                  "datasource_id": "cm2ej4wmo000001fcdkwbdrml",
                  "dataset_id": "cm2ej4vx900hp01l1o378zr9o",
                  "file_type": "xlsx",
                  "external_id": ""
                }
              ],
              "group_id": "",
              "group_name": "",
              "stage": "Respond"
            },
            {
              "type": "QUESTIONS",
              "content": [
                "Analyze the specific channels with negative sales values on different dates and discuss whether the sales strategies of these channels might lead to negative values.",
                "Study the long-term impact of negative sales values on overall sales trends and assess whether adjustments to data analysis methods are needed to more accurately reflect the actual situation.",
                "Investigate the source of negative sales values, whether they are related to returns, discounts, or other factors, and propose possible solutions to reduce the occurrence of negative values."
              ],
              "group_id": "-1",
              "stage": "Respond"
            }
          ]
        }
      }
    ]
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|true|none||返回的任务记录的分页列表。|
|»» total_items|integer|true|none||返回的任务记录总数量。|
|»» page_number|integer|true|none||当前页面的页码。|
|»» page_size|integer|true|none||每页返回的记录数量。|
|»» records|object|true|none||当前页面返回的任务记录列表。|
|»»» question|object|true|none||您的提问（即提示词）。|
|»»»» blocks|object|true|none||构成整个问题的内容块列表。|
|»»»»» type|string|true|none||The content type of the question block. Possible values are:<br />- `MESSAGE`: The content is a piece of text.<br />- `CODE`: The content a code snippet in Markdown format.|
|»»»»» content|string|true|none||The block content. It is a piece of text when the `type` is `MESSAGE`, and a code snippet when the `type` is `CODE`.|
|»»» answer|object|true|none||MAXIR AI 的回答。|
|»»»» blocks|object|true|none||MAXIR AI 的答案。|
|»»»»» type|string|true|none||答案块（Block）的内容类型。可能值为：<br /><br />- `MESSAGE`：表示内容是文本。<br />- `CODE`：表示内容是代码片段。<br />- `TABLE`：表示内容是表格。<br />- `IMAGE`：表示内容是图片。<br />- `SOURCE`：表示内容是答案块的参考来源。<br />- `QUESTIONS`：即 MAXIR AI 生成的建议问题，帮助引导您后续的数据探索和分析。|
|»»»»» content|string|true|none||答案块的内容，随 `type` 值而异：<br /><br />- 当 `type` 为 `MESSAGE` 时，内容为一段文本。<br />- 当 `type` 为 `CODE` 时，内容为 Markdown 格式表示的代码片段。<br />- 当 `type` 为 `TABLE` 时，内容为表格，包含如下构成参数：<br />    - `name`：`.csv` 文件的名称。<br />    - `url`：文件的 S3 Key 或 URL。<br />    - `expires_at`：`url` 的过期时间。如需保存表格方便后续使用，请确保在 URL 过期之前完成下载。<br />- 当 `type` 为 `IMAGE` 时，内容为一张图片，包含如下构成参数：<br />    - `name`：图片的名称。<br />    - `url`：图片的 S3 Key 或 URL。<br />    - `expires_at`：`url` 的过期时间。如需保存图片方便后续使用，请确保在 URL 过期之前完成下载。<br />- 当 `type` 为 `SOURCE` 时，内容为答案块的参考来源，包含如下构成参数：<br />    - `source`：数据源的文件名。<br />    - `datasource_id`：数据源 ID。<br />    - `dataset_id`：数据集 ID。<br />    - `file_type`：数据源的文件扩展名。<br />- 当 `type` 为 `QUESTIONS` 时，内容为 MAXIR AI 生成的建议问题，帮助您引导后续的数据探索与分析。|
|»»»»» group_id|string|true|none||答案块所在的分组 ID。|
|»»»»» group_name|string|true|none||答案块所在的分组名称。|
|»»»»» stage|string|true|none||MAXIR AI 生成答案的过程分为两个阶段：`Analyze` 和 `Respond`。  <br /><br />- `Analyze` 阶段的答案块并非最终答案的构成部分，为分析过程中的输出，旨在帮助您理解答案的生成方式。<br />- `Respond` 阶段的答案块是 MAXIR AI 基于您的问题生成的最终答复。|
|»»» job_id|string|true|none||任务 ID，即任务在当前会话中的唯一标识。|

#### 枚举值

|属性|值|
|---|---|
|type|MESSAGE|
|type|CODE|
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
