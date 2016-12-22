###获取某一订单详情
* 请求方式：GET
* 请求地址：http://api.xunion.me/v1/order/detail?access_token=access_token&sign=sign&order_id=order_id
* 数据格式：key=value
* 请求参数：

参数名称|是否必须|注释
:------------|:------------|:------------
access_token|必须|由2.1获取的access_token字段信息
order_id|否|平台生成订单号，即transaction_id（user_order_id 和 order_id 至少传一个值，同时传优先选择 order_id)
user_order_id|否|商户订单号（user_order_id 和 order_id 至少传一个值，同时传优先选择 order_id)
sign|必须|签名

* 请求报文demo

```
GET http://api.xunion.me/v1/order/detail?access_token=$2y$10$ERbTKqULxWmzNxRcJgfgyuOGdUE8OCaSv1zFuv6b9fWPCl592vOeb7h1ngrf626ohm6wggd&sign=6831B8805F9D7BA8247B6A1BE895F662FF8F8484&user_order_id=2016112220335962054699 HTTP/1.1
User-Agent: Fiddler
Host: api.xunion.me
```

* 返回JSON数据：

```json
{
    "errcode": 0,
    "errmsg": "success",
    "order": {
        "transaction_id": "201504281450333991412661",
        "user_order_id": "123",
        "number": "18026286598",
        "product_id": "10",
        "product_no": "12003",
        "recharge_fee": "1.00",
        "created_at": "2015-04-28 14:50:33",
        "status": 2,
        "msg": "余额不足"
    }
}
```
* 返回说明：查询订单成功与否可判断status字段状态。

参数名称|类型|注释
:------------|:------------|:------------
order|object|订单对象
transaction_id|string 32|订单号
user_order_id|string 32|商户订单号
number|string 11|手机号码
product_id|string 16|套餐编号
product_no|string 16|商品编号
recharge_fee|float 6,2|充值费用
created_at|string|充值时间
status|int|订单状态，0：未充值，1：充值中，2：充值失败，3：充值成功，4：测试订单
msg|string|订单状态信息
