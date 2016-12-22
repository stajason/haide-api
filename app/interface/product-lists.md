###获取可用的商品列表
* 请求方式：GET
* 请求地址：http://api.xunion.me/v1/product/lists?access_token=access_token
* 数据格式：key=value
* 请求参数：

参数名称|是否必须|注释
:------------|:------------|:------------
access_token|必须|由2.1获取的access_token字段信息

* 请求报文demo

```
GET http://api.xunion.me/v1/product/lists?access_token=$2y$10$SKvmSE.VRPP8mliy4uhwg.yrkQXzMmwfP1mLTVL6YwHqh5JVJBPOy7w9azplhccpawfqmnyw1 HTTP/1.1
User-Agent: Fiddler
Host: api.xunion.me
User-Agent: Fiddler
```

* 返回JSON数据：

```json
{
    "errcode": 0,
    "errmsg": "success",
    "products": [
        {
            "product_id": "1_10",
            "name": "中国电信2元10MB流量套餐",
            "price": "2.00",
            "cost": "1.50",
            "flow_value": "10",
            "provider_type": "1"
        },
        {
            "product_id": "1_50",
            "name": "中国电信7元50MB流量套餐",
            "price": "7.00",
            "cost": "6.50",
            "flow_value": "50",
            "provider_type": "1"
        },
        {
            "product_id": "1_100",
            "name": "中国电信10元100MB流量套餐",
            "price": "10.00",
            "cost": "9.50",
            "flow_value": "100",
            "provider_type": "1"
        }
    ]
}
```
* 返回说明：

参数名称|类型|注释
:------------|:------------|:------------
products|array|商品列表
product_id|string 16|套餐编号
name|string 64|商品说明
price|string 16|原价
cost|string 16|成本
flow_value|string 16|流量值（单位MB）
provider_type|string 16|运营商类型 1：电信 2：移动 3：联通
