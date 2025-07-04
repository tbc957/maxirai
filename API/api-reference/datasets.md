# 数据集

## Get dataset overview

GET /v2/team/datasets/{id}/overview

获取数据集的基本信息，包括关键词、描述以及预生成的问题。

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |目标数据集的 ID。|
|user_id|query|string| 是 |用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

#### 详细说明

**id**: 目标数据集的 ID。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

> 返回示例

```json
{
  "code": {
    "code": 0
  },
  "data": {
    "id": "dset-cm5axptyyxxx298",
    "name": "sales_indicators_2024",
    "description": "A dataset comprising 373 travel bookings with 15 attributes, offering insights into booking patterns, pricing strategies, and more",
    "summary": "This dataset contains 373 travel bookings with 15 attributes, enabling analysis of booking trends, pricing strategies, and travel agency dynamics.",
    "exploration_questions": [
      "How does the booking price trend over time based on the BookingTimestamp?",
      "How does the average booking price change with respect to the TravelDate?",
      "Are there any significant outliers in the booking prices, and what might be causing them?",
      "How does the average price vary between one-way and round-trip bookings?"
    ],
    "keywords": [
      "Travel Bookings",
      "Booking Trends",
      "Travel Agencies"
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
|» code|object|true|none||none|
|»» code|integer|true|none||状态码。**0** 表示操作成功。其它值则表示操作失败。如需进行错误排查，请参阅 [错误码](/maxirai/API/introduction/error-codes)。|
|» data|object|true|none||数据集对象。|
|»» id|string|true|none||数据集 ID，即数据集在项目中的唯一标识。|
|»» name|string|true|none||数据集名称。|
|»» description|string|true|none||数据集描述。|
|»» summary|string|true|none||数据集中数据源的概述。|
|»» exploration_questions|string|true|none||MAXIR AI 预生成的问题，可以帮助更好地探索数据集中的数据。|
|»» keywords|string|true|none||数据集关键词，可以帮助更好地理解数据集内容。|

## Create dataset

POST /v2/team/datasets

创建数据集。

数据集是相关数据源的集合。数据源可以是 Excel、CSV、PDF、Word、网页、Markdown、纯文本等格式。使用数据集可以更高效地访问和利用您的数据进行分析。您可以创建一个数据集，将所有用于特定分析的必要数据源存储其中。然后，在进行问答时，您只需将数据集与会话关联，即可访问数据集中的所有数据源。

> Body 请求参数

```json
{
  "name": "My dataset",
  "description": "my default dataset",
  "user_id": "tmm-dafasdfasdfasdf"
}
```

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 |none|
|» name|body|string| 是 |数据集名称，至多可支持 128 个字符。如超过此限制，名称将被截断展示。|
|» description|body|string| 否 |数据集描述，至多可支持 5000 个字符。如果超过此限制，描述将被截断展示。|
|» user_id|body|string| 是 |用户 ID，即您在组织中的唯一身份标识。|

> 返回示例

```json
{
  "code": 0,
  "data": {
    "id": "dataset-adsdfasafdsfasdgasd"
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
|» data|object|true|none||none|
|»» id|string|true|none||数据集 ID。<br /><br />如需 [上传](/maxirai/API/api-reference/data-source?id=create-data-source) 数据源到该数据集或将其 [关联至任务](/maxirai/API/api-reference/job?id=create-job) 进行数据分析和探索，请保存该数据集 ID。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## List datasets

GET /v2/team/datasets

返回数据集列表。

该接口仅会返回与您的 API 密钥所在同一项目中的数据集。

您可以指定搜索关键词，通过名称或描述进行数据集过滤。

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|page_number|query|integer| 否 |分页返回的起始页码。如不指定，则使用默认值 `1`。|
|page_size|query|integer| 否 |每页返回的记录数量。如不指定，则使用默认值 `10`。|
|search|query|string| 否 |搜索关键词，最多支持 128 个字符。所有名称或描述中包含该关键词的数据集将被返回。|
|user_id|query|string| 是 |用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

#### 详细说明

**search**: 搜索关键词，最多支持 128 个字符。所有名称或描述中包含该关键词的数据集将被返回。

如果省略此项，将列出项目中的所有数据集。

> 返回示例

```json
{
  "code": 0,
  "data": {
    "page_number": 1,
    "page_size": 10,
    "total_items": 1,
    "records": [
      {
        "id": "dataset-dasfadsgadsgas",
        "name": "mysql",
        "description": "mysql databases"
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
|» data|object|true|none||返回的数据对象。|
|»» page_number|integer|true|none||当前页面的页码。|
|»» page_size|integer|true|none||每页返回的数据集数量。|
|»» total_items|integer|true|none||返回的数据集总数量。|
|»» records|object|true|none||当前页面返回的数据集列表。|
|»»» id|string|true|none||数据集 ID，即该数据集在项目中的唯一标识。|
|»»» name|string|true|none||数据集名称。|
|»»» description|string|true|none||数据集描述。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Delete dataset

DELETE /v2/team/datasets/{id}

删除数据集。一旦删除，该数据集中的所有数据源也将被永久删除，且无法恢复。

> Body 请求参数

```json
{
  "user_id": "tmm-dafasdfasdfasdf"
}
```

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |目标数据集 ID。您只能删除自己创建的数据集。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 |none|
|» user_id|body|string| 是 |用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**id**: 目标数据集 ID。您只能删除自己创建的数据集。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

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

## Modify dataset

POST /v2/team/datasets/{id}

修改指定数据集的名称或描述。

> Body 请求参数

```json
{
  "name": "sales_data",
  "description": "sales data of all regions",
  "user_id": "tmm-dsadfdsafasdf"
}
```

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |目标数据集 ID。您只能修改自己创建的数据集。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 |none|
|» name|body|string| 否 |数据集的新名称，至多可支持 128 个字符。如超过此限制，名称将被截断展示。|
|» description|body|string| 否 |数据集的新描述，至多可支持 5000 个字符。如果超过此限制，描述将被截断展示。|
|» user_id|body|string| 是 |用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**id**: 目标数据集 ID。您只能修改自己创建的数据集。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

**» name**: 数据集的新名称，至多可支持 128 个字符。如超过此限制，名称将被截断展示。

必须指定 `name` 或 `description` 中的至少一个。

**» description**: 数据集的新描述，至多可支持 5000 个字符。如果超过此限制，描述将被截断展示。

必须指定 `name` 或 `description` 中的至少一个。

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

## Get status summary of data sources in dataset

GET /v2/team/datasets/{id}/status

此接口会计算并返回指定数据集中处于每个状态的数据源数量。您可以使用该接口来检查数据集中的所有数据源是否已完成同步并可用于数据分析任务。

只有当返回中的 `invalid_count` 和 `synching_count` 均为 `0`时，才表示该数据集中的所有数据源都能用于问答。否则，MAXIR AI 将无法使用未完成同步的数据源进行数据分析与探索。

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|id|path|string| 是 |需要了解数据源状态的数据集 ID。|
|user_id|query|string| 是 |用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 |您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

> 返回示例

```json
{
  "code": 0,
  "data": {
    "synched_count": 5,
    "invalid_count": 0,
    "synching_count": 0
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
|» data|object|true|none||处于每个状态的数据源数量。<br /><br />请注意，只有当 `invalid_count` 和 `synching_count` 都为 `0` 时，使用 MAXIR AI 进行问答时才能使用数据集中的所有数据源。否则，MAXIR AI 将无法使用未完成同步的数据源进行数据分析与探索。|
|»» synched_count|integer|true|none||已同步的数据源数量。|
|»» invalid_count|integer|true|none||同步失败的数据源数量。|
|»» synching_count|integer|true|none||等待同步或正在同步的数据源数量。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
