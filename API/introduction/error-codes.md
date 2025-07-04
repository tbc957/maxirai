# 错误码

MAXIR AI 使用标准的 HTTP 响应码来指示 API 请求的结果。一般来说，`2xx` 范围内的代码表示请求成功；`4xx` 范围内的代码表示客户端错误（例如，参数无效）；`5xx` 范围内的代码表示服务器端错误（此类错误较少发生）。

---

## API 错误

| 错误码 | 错误信息 | 说明 |
| :- | :- | :- |
| `400` | Bad Request | 无效参数。 |
| `401` | Unauthorized | 无效 API Key。 |
| `402` | Request Failed | 参数有效，但请求无法完成。 |
| `403` | Forbidden | 使用的 API Key 缺少执行此操作所需的权限。 |
| `404` | Not Found | 请求的资源不存在。 |
| `409` | Conflict | 请求与其他操作冲突（例如，使用相同的幂等密钥）。 |
| `429` | Too Many Requests | 在一定时间内，发送的请求过多，超出上限。 |
| `500`、`502`、`503`、`504` | Server Errors | MAXIR AI 端发生错误（较少见）。 |

---

## 错误类型

| 错误类型 | 描述 |
| :- | :- |
| `authentication_error` | 认证错误，API Key 无效、已过期或已被撤销。可能是由于输入错误、格式问题或潜在的安全问题。 |
| `invalid_request_error` | 请求错误，请求中包含无效参数。 |
| `internal_server_error` | 内部服务器错误，表示处理请求时发生了服务端的问题，可能是临时问题、Bug 或系统故障。 |
| `idempotency_error` | 幂等性错误，同一幂等密钥被用于不同的 API 接口或参数集时发生冲突。 |
| `rate_limit_error` | 限速错误，你所在的团队超出了每秒 20 次 API 请求的上限。 |
| `permission_error` | 您无权执行此操作。（操作：%s，资源：%s。） |

---

## 客户端错误码

| 错误码 | 错误信息 | HTTP 状态码 | 描述 |
| :- | :- | :- | :- |
| `300001` | Invalid parameters *`error_details>`* | 400 | 缺少必需的参数或参数配置错误。请检查所有参数是否设置正确。 |
| `300002` | No permissions *`<error_details>`* | 200 | 您没有执行此操作所需的权限。 |
| `300003` | *`<resource_name>`* not found | 200 | 项目中不存在指定资源。请确认资源 ID 是否正确。 |
| `300004` | Invalid file extension | 200 | 文件扩展名不支持。支持的扩展名包括 **.csv**、**.tsv**、**.md**、**.mdx**、**.json**、**.txt**、**.pdf**、**.pptx**、**.ppt**、**.doc**、**.docx**、**.xls** 和 **.xlsx**。 |
| `300005` | Empty file | 200 | 文件为空，请检查上传的文件是否正确。 |
| `300006` | Insufficient storage space | 200 | 没有足够的可用存储空间上传文件。请先升级容量套餐。 |
| `300007` | Failed to upload the file | 200 | 上传文件时发生错误。请检查文件格式是否受支持，然后再试。 |
| `300008` | An error occurred while generating the presigned URL | 200 | 生成预签名 URL 时发生错误。请检查所有参数是否正确设置，然后重试。 |
| `300009` | The number of sessions has reached the upper limit | 200 | 已达到最大会话数。请删除不必要的会话，然后再试。 |
| `300011` | Fail to create datasource | 200 | 因内部错误导致无法创建数据源。 |
| `210020` | Something went wrong during job execution. Please try again. | 400 | 任务执行失败，请稍后重试。 |
| `210021` | Job quota exceeded | 400 | 任务配额不足，请先升级任务套餐。 |
| `210022` | Question too long | 400 | 问题过长，最多支持个 6000 字符。 |
| `210023` | Selected files are not all ready | 400 | 选择的文件中有尚未完成同步的文件。 |
| `210024` | Text too long for TTS service, limit is 5k characters. | 400 | 输入的文本超过了最大限制 5000 字符，请缩短文本。 |
| `210025` | Too many selected files in the query | 400 | 选择的文件数量过多。 |
| `210026` | Invalid analysis | 400 | 数据分析失败，请稍后重试或将 `stream` 设置为 true 后重新发起任务。 |

---

## 服务器端错误码

| 错误码 | 错误信息 | HTTP 状态码 | 描述 |
| :- | :- | :- | :- |
| `9999` | Internal server error | `500` | 因未知错误导致请求无法处理。 |
| `201` | Rate limit reached for requests | `429` | 请求超出分配的限制。当前每个团队每秒最多可发送 20 个 API 请求。 |
| `1002` | Expired credentials | `403` | 提供的凭证已过期且无效，您可能需要更新或刷新凭证。 |
| `1003` | Insufficient authentication | `403` | 提供的认证信息不足或不完整，请确保包含所有必需的认证信息。 |
| `1004` | Bad credentials | `403` | 提供的凭证错误或格式不正确，请确认凭证信息是否正确。 |

