###获取access_token
* 请求方式：POST
* 请求地址：http://api.xunion.me/v1/auth/token
* 数据格式：JSON
* 请求参数：

参数名称|是否必须|注释
:------------|:------------|:------------
client_id|必须|第三方用户唯一凭证
client_secret|必须|第三方用户唯一凭证密钥
grant_type | 必须 | 值固定为client_credential

* 请求报文demo

```json
POST http://api.xunion.me/v1/auth/token HTTP/1.1
User-Agent: Fiddler
Host: api.xunion.me
Content-Length: 151
Content-Type: application/json

{
    "client_id": "flow3355e802d44ay95yeev2",
    "client_secret": "eal0ycl5argvq8v3cthm5qnzq1ljqf1429506832",
    "grant_type": "client_credential"
}
```

* 返回JSON数据：

```json
{
    "errcode": 0,
    "errmsg": "success",
    "access_token": "token",
    "expires_in": 7200
}
```
* 返回说明：

参数名称|类型|注释
:------------|:------------|:------------
access_token|string 128|获取到的凭证，获取新token，旧token依然在原来的7200秒内过期
expires_in|int 4|凭证有效时间，单位：秒
