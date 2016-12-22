###1.请求
* 正式账号的client_id与client_secret由海德信息科技有限公司提供给合作商
* 所有POST请求http header设置Content-Type为application/json，charset为utf-8
* POST请求的所有数据格式均为JSON

* 测试账号

参数名称|值
:------------|:------------
client_id|flow3355e802d44ay95yeev2
client_secret|eal0ycl5argvq8v3cthm5qnzq1ljqf1429506832

###2.响应
* 响应的所有数据格式均为JSON
* 所有接口都会统一返回errcode和errmsg两个参数
* 正常的返回：
```json
{
    "errcode": 0,
    "errmsg": "success"
}
```
* 错误的返回：
```json
{
    "errcode": 50001,
    "errmsg": "client_id或者client_secret参数有误"
}
```
