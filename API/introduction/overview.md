# Team Open API Version 2 中文版本

MAXIR AI 提供了一系列功能强大的 API 接口，支持与系统的无缝集成。所有接口均需使用您的 API Key 进行身份验证。


# 域名
生产环境 URLs: https://<替换成生产域名></a>
域名指向的IP获取方式：
* 首先在 UCloud 云平台上创建 MAXIR AI 实例，并在详情页获取OpenAPI地址;
* 外网访问的情况下，客户需通过ALB（应用负载均衡器）获取并绑定外网地址,可参考[相关文档](https://docs.ucloud.cn/maxirai/introduction/access)；
* 将自己的业务域名指向该外网地址，以完成配置。


# 授权

验证方式 ：API Key
传参方式：通过请求头（Header）传递
参数名称：x-pd-api-key
获取API key：请参考 [身份认证](/maxirai/API/introduction/authentication)。

