# 本地文件

## Upload local file

POST /v2/team/file/upload-datasource

上传本地文件。支持的文件格式包括：**.csv**、**.tsv**、**.md**、**.mdx**、**.json**、**.txt**、**.pdf**、**.pptx**、**.ppt**、**.doc**、**.docx**、**.xls** 或 **.xlsx**。

您可以使用此接口上传本地文件，然后使用获得的 `file_object_key` 来创建数据源。

> Body 请求参数

```yaml
file: ""
user_id: ""

```


### 请求参数

|名称|位置|类型|必选|中文名|说明|
|---|---|---|---|---|---|
|x-pd-external-trace-id|header|string| 否 ||您本地系统中设置的 Trace ID，至多支持 128 个字符。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
|body|body|object| 否 ||none|
|» file|body|string(binary)| 是 ||要上传的文件。该字段必填，且应当包含文件数据。例如，`--form 'file=@"/Users/jiaoqi/Downloads/0f9a7ebd-7a2a-454a-8cd9-96accffa3107.csv"'`。|
|» user_id|body|string| 是 ||用户 ID，即您在组织中的唯一身份标识。|

> 返回示例

```json
{
  "code": 0,
  "data": {
    "file_object_key": "/tmp/sdgsagdsgsadgasdg.csv"
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
|»» file_object_key|string|true|none||文件的对象存储路径。|



### 返回头部 Header

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|x-pd-trace-id|string||MAXIR AI 返回的 Trace ID。当请求发生错误时，可以将此 ID 提供给 MAXIR AI 团队，协助进行故障排查。|
