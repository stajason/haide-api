###充值结果回调
* 请求方式：POST
* 回调地址：充值订单接口提交的callback_url或者后台配置的回调地址（优先选择接口的callback_url）
* 数据格式：JSON
* 请求数据：

```json
{
    "notify_type": "recharge_result",
    "errcode": 0,
    "errmsg": "测试请求",
    "order": {
        "transaction_id": "201504210952082292343482",
        "user_order_id": "201504210952123",
        "number": 15876598724,
        "product_id": "2_10",
        "recharge_fee": "3.00",
        "deduction_fee": "3.00",
        "status": 4,
        "msg": "success"
    }
}
```

* 请求参数说明

参数名称|类型|注释
:------------|:------------|:------------
notify_type|string 32|值固定为recharge_result
errcode|int 6|充值结果错误码，0代表成功，非0代表订单失败。
errmsg|string 128|充值结果详细信息
order|object|订单对象
transaction_id|string 32|订单号
user_order_id|string 40|用户提交的订单号
number|int 11|手机号码
product_id|string 16|套餐编号
recharge_fee|string|充值费用
deduction_fee|string|实际扣款金额
status|int|订单状态 [2：充值失败 3：充值成功]，若返回4，则该订单为测试订单。
msg|string 128|充值结果详细信息

* 返回说明:
业务处理完毕后返回 http 状态码200的响应报头，超时或者非200状态码的 http 响应系统会判断回调失败，并且重新发起回调，共四次回调，间隔为30秒、60秒、90秒，120秒,查询订单成功与否可判断status字段状态，errcode非0时订单失败。
