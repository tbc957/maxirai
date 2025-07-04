# 数据源

## Create data source

POST /v2/team/datasets/{id}/datasources

在指定数据集中创建数据源。您只能在自己创建的数据集中创建数据源。

数据源可以为以下任意格式：**.csv**、**.tsv**、**.md**、**.mdx**、**.json**、**.txt**、**.pdf**、**.pptx**、**.ppt**、**.doc**、**.docx**、**.xls** 或 **.xlsx**。

> Body 请求参数

```json
{
  "name": "test.csv",
  "type": "FILE",
  "user_id": "tmm-dsfasdfasdfa",
  "url": "https://s3.amazonaws.com/xxxtest/user/clvl4cad2001q01l1m522hxlu/upload/f9773f1e-cd68-489a-8121-d566ca9218b1.csv?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240924T143419Z&X-Amz-SignedHeaders=host&X-Amz-Expires=599&X-Amz-Credential=AKIARLSQLXURHEIDN4OZ%2F20240924%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=9ca0c58d508926a5811818041d557ffb53c64025dae94c0855280d457c7089a2"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|id|path|string| 是 ||目标数据集 ID。|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 ||none|
|» name|body|string| 是 ||数据源名称，需包含文件扩展名（例如 `example.csv`），至多可支持 128 个字符。如超过此限制，名称将被截断展示。|
|» type|body|string| 是 | FILE|数据源的类型。设置为 **FILE**。|
|» url|body|string| 否 ||文件 URL，用于公网访问该文件。|
|» file_object_key|body|string| 否 ||您本地上传文件的对象存储路径。|
|» user_id|body|string| 是 ||用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**id**: 目标数据集 ID。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

**» url**: 文件 URL，用于公网访问该文件。

`url` 和 `file_object_key` 之间必须且只能指定其一。

仅支持以下扩展名的文件：.csv、.tsv、.md、.mdx、.json、.txt、.pdf、.pptx、.ppt、.doc、.docx、.xls 或 .xlsx。

**» file_object_key**: 您本地上传文件的对象存储路径。

`url` 和 `file_object_key` 之间必须且只能指定其一。

支持的文件扩展名包括：**.csv**、**.tsv**、**.md**、**.mdx**、**.json**、**.txt**、**.pdf**、**.pptx**、**.ppt**、**.doc**、**.docx**、**.xls** 或 **.xlsx**。

如何获取文件的 `file_object_key`：

当使用 [Upload file](/maxirai/API/api-reference/file?id=upload-local-file) 接口完成文件上传后，会返回该文件的`file_object_key`。

> 返回示例

```json
{
  "code": 0,
  "data": {
    "id": "datasource-cadsgfsdagasgadsg",
    "dataset_id": "dataset-dagasdgasgasg",
    "name": "test.csv",
    "type": "FILE",
    "status": "synching"
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
|» data|object|true|none||数据源对象。|
|»» id|string|true|none||数据源 ID，即该数据源在数据集中的唯一标识。|
|»» dataset_id|string|true|none||数据源所在的数据集 ID。|
|»» name|string|true|none||数据源名称。|
|»» type|string|true|none||数据源类型，固定为 **FILE**。|
|»» status|string|true|none||数据源的处理状态。可能值为：<br /><br />- `invalid`：等待处理中。<br />- `synching`：正在处理中。<br />- `synched`：已成功同步。|

#### 枚举值

|属性|值|
|---|---|
|type|FILE|
|status|synching|
|status|synched|
|status|invalid|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## List data sources

GET /v2/team/datasets/{id}/datasources

返回指定数据集中的数据源列表。使用此接口时，请注意：

- 确保指定数据集和您的 API Key 归属于同一项目。
- 如需查看您在该项目中有访问权限的数据集，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|id|path|string| 是 ||目标数据集 ID。|
|page_number|query|integer| 否 ||分页返回的起始页码。如不指定，则使用默认值 `1`。|
|page_size|query|integer| 否 ||每页返回的记录数量。如不指定，则使用默认值 `10`。|
|status|query|string| 否 ||数据源状态。指定该参数后，只有处于指定状态的数据源才会被返回。可选值包括：|
|user_id|query|string| 是 ||用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

#### 详细说明

**id**: 目标数据集 ID。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

**status**: 数据源状态。指定该参数后，只有处于指定状态的数据源才会被返回。可选值包括：

- `invalid`：等待处理中。
- `synching`：正在处理中。
- `synched`：已成功同步。

如果未指定，将返回所有数据源。

可以通过英文逗号分隔的列表指定多个状态，匹配任何状态的数据源将被返回。

#### 枚举值

|属性|值|
|---|---|
|status|synching|
|status|invalid|
|status|synched|

> 返回示例

```json
{
  "code": 0,
  "data": {
    "total_items": 1,
    "page_size": 10,
    "page_number": 1,
    "records": [
      {
        "id": "datasource-cadsgfsdagasgadsg",
        "dataset_id": "dataset-dagasdgasgasg",
        "name": "test.csv",
        "type": "FILE",
        "status": "synching"
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
|» data|object|true|none||返回的数据源分页列表。|
|»» total_items|integer|true|none||返回的数据源总数量。|
|»» page_size|integer|true|none||每页返回的数据源数量。|
|»» page_number|integer|true|none||当前页面的页码。|
|»» records|object|true|none||当前页面返回的数据源列表。|
|»»» id|string|true|none||数据源 ID，即该数据源在数据集中的唯一标识。|
|»»» dataset_id|string|true|none||数据源所在的数据集 ID。|
|»»» name|string|true|none||数据源名称。|
|»»» type|string|true|none||数据源类型，固定为 **FILE**。|
|»»» status|string|true|none||数据源的处理状态。可能值为：<br /><br />- `invalid`：等待处理中。<br />- `synching`：正在处理中。<br />- `synched`：已成功同步。|

#### 枚举值

|属性|值|
|---|---|
|type|FILE|
|status|synching|
|status|synched|
|status|invalid|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Delete data source

DELETE /v2/team/datasets/{dataset_id}/datasources/{datasource_id}

从指定的数据集中删除数据源。一旦删除，数据源将无法恢复。

您只能删除自己创建的数据集中的数据源。

> Body 请求参数

```json
{
  "user_id": "tmm-dafasdfasdfasdf"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|dataset_id|path|string| 是 ||目标数据集 ID。|
|datasource_id|path|string| 是 ||要删除的数据源 ID。|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 ||none|
|» user_id|body|string| 是 ||用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**dataset_id**: 目标数据集 ID。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

**datasource_id**: 要删除的数据源 ID。

如需查询指定数据集中的数据源，请调用 [GET /v2/team/datasets/{id}/datasources](/maxirai/API/api-reference/data-source?id=list-data-sources) 接口。

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
|» data|object¦null|false|none||操作成功则返回 null。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Get data source

GET /v2/team/datasets/{dataset_id}/datasources/{datasource_id}

获取指定数据源信息。

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|dataset_id|path|string| 是 ||目标数据源所在的数据集 ID。|
|datasource_id|path|string| 是 ||目标数据源 ID。|
|user_id|query|string| 是 ||用户 ID，即您在组织中的唯一身份标识。|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

#### 详细说明

**dataset_id**: 目标数据源所在的数据集 ID。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

**datasource_id**: 目标数据源 ID。

如需查询指定数据集中的数据源，请调用 [GET /v2/team/datasets/{id}/datasources](/maxirai/API/api-reference/data-source?id=list-data-sources) 接口。

> 返回示例

```json
{
  "code": 0,
  "data": {
    "id": "datasource-cadsgfsdagasgadsg",
    "dataset_id": "dataset-dagasdgasgasg",
    "name": "test.csv",
    "type": "FILE",
    "status": "synching"
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
|» data|object|true|none||数据源对象。|
|»» id|string|true|none||数据源 ID，即该数据源在数据集中的唯一标识。|
|»» dataset_id|string|true|none||数据源所在的数据集 ID。|
|»» name|string|true|none||数据源名称。|
|»» type|string|true|none||数据源类型，固定为 **FILE**。|
|»» status|string|true|none||数据源的处理状态。可能值为：<br /><br />- `invalid`：等待处理中。<br />- `synching`：正在处理中。<br />- `synched`：已成功同步。|

#### 枚举值

|属性|值|
|---|---|
|type|FILE|
|status|synching|
|status|synched|
|status|invalid|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Create data source without specifying a dataset

POST /v2/team/datasources

该接口用于在不指定数据集的情况下，直接创建数据源。

调用此接口时，MAXIR AI 会自动为该数据源创建一个数据集。请保存返回中的数据集 ID，便于后续操作，如 [关联数据集至任务](/maxirai/API/api-reference/job?id=create-job) 进行数据分析与探索。

> Body 请求参数

```json
{
  "name": "test.csv",
  "type": "FILE",
  "user_id": "tmm-dafasdfasdfasdf",
  "file_object_key": "/tmp/sdgsagdsgsadgasdg"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 ||none|

> 返回示例

```json
{
  "code": 0,
  "data": {
    "id": "datasource-cadsgfsdagasgadsg",
    "dataset_id": "dataset-dagasdgasgasg",
    "name": "test.csv",
    "type": "FILE",
    "status": "synching"
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
|» data|object|true|none||数据源对象。|
|»» id|string|true|none||数据源 ID，即该数据源在数据集中的唯一标识。|
|»» dataset_id|string|true|none||数据源所在的数据集 ID。|
|»» name|string|true|none||数据源名称。|
|»» type|string|true|none||数据源类型，固定为 **FILE**。|
|»» status|string|true|none||数据源的处理状态。可能值为：<br /><br />- `invalid`：等待处理中。<br />- `synching`：正在处理中。<br />- `synched`：已成功同步。|

#### 枚举值

|属性|值|
|---|---|
|type|FILE|
|status|synching|
|status|synched|
|status|invalid|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|

## Presign data source 

POST /v2/team/datasets/{dataset_id}/datasources/{datasource_id}/presign

该接口用于为指定数据源生成一个预签名 URL（Presigned URL），从而您可以通过该 URL 下载对应的数据源。

预签名 URL 具有有效期，请确保在 URL 过期之前完成数据源的下载。

> Body 请求参数

```json
{
  "expires_in": 600,
  "user_id": "tmm-dafasdfasdfasdf"
}
```

### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|dataset_id|path|string| 是 ||目标数据源所在的数据集 ID。|
|datasource_id|path|string| 是 ||目标数据源 ID。|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 ||none|
|» expires_in|body|integer| 否 ||预签名 URL 的过期时间，单位为秒（s）。最小值为 `60`，默认值为 `600`。|
|» user_id|body|string| 是 ||用户 ID，即您在组织中的唯一身份标识。|

#### 详细说明

**dataset_id**: 目标数据源所在的数据集 ID。

如需查询您有访问权限的数据集列表，请调用 [GET /v2/team/datasets](/maxirai/API/api-reference/datasets?id=list-datasets) 接口。

**datasource_id**: 目标数据源 ID。

如需查询指定数据集中的数据源，请调用 [GET /v2/team/datasets/{id}/datasources](/maxirai/API/api-reference/data-source?id=list-data-sources) 接口。

> 返回示例

```json
{
  "code": 0,
  "data": {
    "presigned_url": "string",
    "expires_at": "2024-11-13T14:15:22.123Z"
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
|»» presigned_url|string|true|none||预签名 URL，用于下载对应数据源。|
|»» expires_at|string(date-time)|true|none||预签名 URL 的过期日期和时间。|

### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
