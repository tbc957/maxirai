# 数据模型
## CreateSessionRequest
<h2 id="tocS_CreateSessionRequest">CreateSessionRequest</h2>

<a id="schemacreatesessionrequest"></a>
<a id="schema_CreateSessionRequest"></a>
<a id="tocScreatesessionrequest"></a>
<a id="tocscreatesessionrequest"></a>

```json
{
  "name": "string",
  "output_language": "AUTO",
  "job_mode": "AUTO",
  "max_contextual_job_history": 0,
  "agent_id": "DATA_ANALYSIS_AGENT",
  "user_id": "string"
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|name|string|true|none||会话名称，至多可支持 128 个字符。如超过此限制，名称将被截断展示。|
|output_language|string|false|none||MAXIR AI 的回复语言。例如，当设置为 `EN` 时，则 MAXIR AI 使用英语进行回复。如不指定，将使用默认值 `AUTO`，即 MAXIR AI 会根据提示词自动选择合适的回复语言。可选值包括：<br /><br />- `AUTO`：根据提示词自动识别<br />- `EN`：英语<br />- `ES`：西班牙语<br />- `AR`：阿拉伯语<br />- `PT`：葡萄牙语<br />- `ID`：印尼语<br />- `JA`：日语<br />- `RU`：俄语<br />- `HI`：印地语<br />- `FR`：法语<br />- `DE`：德语<br />- `VI`：越南语<br />- `TR`：土耳其语<br />- `PL`：波兰语<br />- `IT`：意大利语<br />- `KO`：韩语<br />- `ZH-CN`：简体中文<br />- `ZH-TW`：繁体中文|
|job_mode|string|false|none||任务类型。可选值包括：<br /><br />- `AUTO`：MAXIR AI 根据你的提问进行意图判断，决定该任务为数据分析任务或信息检索任务。<br />- `DATA_ANALYTICS`：MAXIR AI 会根据你的输入进行数据分析。<br /><br />如不指定，则采用默认值 `AUTO`。如果您明确地想要进行数据分析，我们建议您将值设置为 **DATA_ANALYTICS**，从而 MAXIR AI 可以跳过意图识别步骤，加快问题处理速度。|
|max_contextual_job_history|integer|false|none||作为下一个任务上下文的历史任务数，取值范围为 `0` 到 `10`，默认值为 `10`。如设置为 `0`，则当前任务将不使用任何历史任务作为上下文信息。|
|agent_id|string|false|none||AI 智能体的 ID。该参数为保留参数，无需制定或设置为 **DATA_ANALYSIS_AGENT**。|
|user_id|string|true|none||用户 ID，即您在组织中的唯一身份标识。|

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

## SessionDTO
<h2 id="tocS_SessionDTO">SessionDTO</h2>

<a id="schemasessiondto"></a>
<a id="schema_SessionDTO"></a>
<a id="tocSsessiondto"></a>
<a id="tocssessiondto"></a>

```json
{
  "id": "string",
  "name": "string",
  "output_language": "AUTO",
  "job_mode": "AUTO",
  "max_contextual_job_history": 0,
  "agent_id": "DATA_ANALYSIS_AGENT"
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none||会话 ID，会话在当前项目中的唯一标识。|
|name|string|true|none||会话名称。如名称长度超过 128 个字符，将仅返回前 128 个字符。|
|output_language|string|false|none||MAXIR AI 的返回语言。可能值包括：<br /><br />- `AUTO`：根据提示词自动识别<br />- `EN`：英语<br />- `ES`：西班牙语<br />- `AR`：阿拉伯语<br />- `PT`：葡萄牙语<br />- `ID`：印尼语<br />- `JA`：日语<br />- `RU`：俄语<br />- `HI`：印地语<br />- `FR`：法语<br />- `DE`：德语<br />- `VI`：越南语<br />- `TR`：土耳其语<br />- `PL`：波兰语<br />- `IT`：意大利语<br />- `KO`：韩语<br />- `ZH-CN`：简体中文<br />- `ZH-TW`：繁体中文|
|job_mode|string|false|none||任务类型。可能值包括：<br /><br />- `AUTO`：MAXIR AI 根据您的提问进行意图判断，决定任务是数据分析任务还是信息检索任务。<br />- `DATA_ANALYTICS`：MAXIR AI 根据您的输入进行数据分析。|
|max_contextual_job_history|integer|false|none||在会话中保留作为下一个任务上下文的任务数量，取值范围为 `0` 到 `10`，默认值为 `10`。值为 `0` 时表示会话中的任务在执行时不会使用历史任务执行结果作为上下文信息。|
|agent_id|string|false|none||AI 智能体的 ID。该参数为保留参数，无实际意义，请忽略。|

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

## DatasourceDTO
<h2 id="tocS_DatasourceDTO">DatasourceDTO</h2>

<a id="schemadatasourcedto"></a>
<a id="schema_DatasourceDTO"></a>
<a id="tocSdatasourcedto"></a>
<a id="tocsdatasourcedto"></a>

```json
{
  "id": "string",
  "dataset_id": "string",
  "name": "string",
  "type": "FILE",
  "status": "synching"
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none||数据源 ID，即该数据源在数据集中的唯一标识。|
|dataset_id|string|true|none||数据源所在的数据集 ID。|
|name|string|true|none||数据源名称。|
|type|string|true|none||数据源类型，固定为 **FILE**。|
|status|string|true|none||数据源的处理状态。可能值为：<br /><br />- `invalid`：等待处理中。<br />- `synching`：正在处理中。<br />- `synched`：已成功同步。|

#### 枚举值

|属性|值|
|---|---|
|type|FILE|
|status|synching|
|status|synched|
|status|invalid|

## DatasetDTO
<h2 id="tocS_DatasetDTO">DatasetDTO</h2>

<a id="schemadatasetdto"></a>
<a id="schema_DatasetDTO"></a>
<a id="tocSdatasetdto"></a>
<a id="tocsdatasetdto"></a>

```json
{
  "id": "string",
  "name": "string",
  "description": "string"
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|id|string|true|none||数据集 ID，即该数据集在项目中的唯一标识。|
|name|string|true|none||数据集名称。|
|description|string|true|none||数据集描述。|

## 返回模型

<h2 id="tocS_返回模型">返回模型</h2>

<a id="schema返回模型"></a>
<a id="schema_返回模型"></a>
<a id="tocS返回模型"></a>
<a id="tocs返回模型"></a>

```json
{
  "code": 0
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|

## BlockDTO
<h2 id="tocS_BlockDTO">BlockDTO</h2>

<a id="schemablockdto"></a>
<a id="schema_BlockDTO"></a>
<a id="tocSblockdto"></a>
<a id="tocsblockdto"></a>

```json
{
  "type": "MESSAGE",
  "content": "string",
  "group_id": "string",
  "group_name": "string",
  "stage": "Analyze"
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|type|string|true|none||答案块（Block）的内容类型。可能值为：<br /><br />- `MESSAGE`：表示内容是文本。<br />- `CODE`：表示内容是代码片段。<br />- `TABLE`：表示内容是表格。<br />- `IMAGE`：表示内容是图片。<br />- `SOURCE`：表示内容是答案块的参考来源。<br />- `QUESTIONS`：即 MAXIR AI 生成的建议问题，帮助引导您后续的数据探索和分析。|
|content|string|true|none||答案块的内容，随 `type` 值而异：<br /><br />- 当 `type` 为 `MESSAGE` 时，内容为一段文本。<br />- 当 `type` 为 `CODE` 时，内容为 Markdown 格式表示的代码片段。<br />- 当 `type` 为 `TABLE` 时，内容为表格，包含如下构成参数：<br />    - `name`：`.csv` 文件的名称。<br />    - `url`：文件的 S3 Key 或 URL。<br />    - `expires_at`：`url` 的过期时间。如需保存表格方便后续使用，请确保在 URL 过期之前完成下载。<br />- 当 `type` 为 `IMAGE` 时，内容为一张图片，包含如下构成参数：<br />    - `name`：图片的名称。<br />    - `url`：图片的 S3 Key 或 URL。<br />    - `expires_at`：`url` 的过期时间。如需保存图片方便后续使用，请确保在 URL 过期之前完成下载。<br />- 当 `type` 为 `SOURCE` 时，内容为答案块的参考来源，包含如下构成参数：<br />    - `source`：数据源的文件名。<br />    - `datasource_id`：数据源 ID。<br />    - `dataset_id`：数据集 ID。<br />    - `file_type`：数据源的文件扩展名。<br />- 当 `type` 为 `QUESTIONS` 时，内容为 MAXIR AI 生成的建议问题，帮助您引导后续的数据探索与分析。|
|group_id|string|true|none||答案块所在的分组 ID。|
|group_name|string|true|none||答案块所在的分组名称。|
|stage|string|true|none||MAXIR AI 生成答案的过程分为两个阶段：`Analyze` 和 `Respond`。  <br /><br />- `Analyze` 阶段的答案块并非最终答案的构成部分，为分析过程中的输出，旨在帮助您理解答案的生成方式。<br />- `Respond` 阶段的答案块是 MAXIR AI 基于您的问题生成的最终答复。|

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

## DataSourceConfig

<h2 id="tocS_DataSourceConfig">DataSourceConfig</h2>

<a id="schemadatasourceconfig"></a>
<a id="schema_DataSourceConfig"></a>
<a id="tocSdatasourceconfig"></a>
<a id="tocsdatasourceconfig"></a>

```json
{
  "name": "string",
  "type": "string",
  "url": "string",
  "file_object_key": "string",
  "user_id": "string"
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|name|string|true|none||数据源名称，需包含文件扩展名（例如 `example.csv`），至多可支持 128 个字符。如超过此限制，名称将被截断展示。|
|type|string|true|none|FILE|数据源的类型。设置为 **FILE**。|
|url|string|false|none||文件 URL，用于公网访问该文件。<br /><br />`url` 和 `file_object_key` 之间必须且只能指定其一。<br /><br />仅支持以下扩展名的文件：.csv、.tsv、.md、.mdx、.json、.txt、.pdf、.pptx、.ppt、.doc、.docx、.xls 或 .xlsx。|
|file_object_key|string|false|none||您本地上传文件的对象存储路径。<br /><br />`url` 和 `file_object_key` 之间必须且只能指定其一。<br /><br />支持的文件扩展名包括：**.csv**、**.tsv**、**.md**、**.mdx**、**.json**、**.txt**、**.pdf**、**.pptx**、**.ppt**、**.doc**、**.docx**、**.xls** 或 **.xlsx**。<br /><br />如何获取文件的 `file_object_key`：<br /><br />当使用 [Upload file](/maxirai/API/api-reference/file?id=upload-file) 接口完成文件上传后，会返回该文件的`file_object_key`。|
|user_id|string|true|none||用户 ID，即您在组织中的唯一身份标识。|




