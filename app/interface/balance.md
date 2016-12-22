###获取账号余额
* 请求方式：GET
* 请求地址：http://api.xunion.me/v1/account/balance?access_token=access_token
* 数据格式：key=value
* 请求参数：

参数名称|是否必须|注释
:------------|:------------|:------------
access_token|必须|由2.1获取的access_token字段信息

* 请求报文demo

```
GET http://api.xunion.me/v1/account/balance?access_token=$2y$10$SKvmSE.VRPP8mliy4uhwg.yrkQXzMmwfP1mLTVL6YwHqh5JVJBPOy7w9azplhccpawfqmnyw1 HTTP/1.1
User-Agent: Fiddler
Host: api.xunion.me
```

* 返回JSON数据：

```json
{
    "errcode": 0,
    "errmsg": "success",
    "balance": "20.05"
}
```
* 返回说明：

参数名称|类型|注释
:------------|:------------|:------------
balance|string|余额，单位元
