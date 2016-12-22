###通道实时消息推送
* 请求方式：POST
* 回调地址：请联系商务配置回调地址
* 数据格式：JSON
* 请求数据：

```json
{
    "notify_type": "channel_notify",
    "errcode": 0,
    "errmsg": "success",
    "channel": [
        {
            "product_id": "1_1",
            "product_name": "中国电信1元1MB流量套餐",
            "limit_status": true,
            "limit_price": "95",
            "provider_type": "1",
            "origin_price": "1.00",
            "deduction_fee": "1.00",
            "product_status": "1",
            "recharge_status": "1",
            "flow_value": "1",
            "eft_type": "0"
        },
        {
            "product_id": "1_5",
            "product_name": "中国电信1元5MB流量套餐",
            "limit_status": true,
            "limit_price": "95",
            "provider_type": "1",
            "origin_price": "1.00",
            "deduction_fee": "1.00",
            "product_status": "1",
            "recharge_status": "1",
            "flow_value": "5",
            "eft_type": "0"
        }
    ]
}
```

* 请求参数说明

参数名称|类型|注释
:------------|:------------|:------------
notify_type|string 32|值固定为channel_notify
errcode|int 6|推送结果错误码，0代表请求成功。
errmsg|string 128|推送结果详细信息
channel|object|通道消息对象
product_id|string 16|套餐编号
product_name|string 64|套餐说明
limit_status|boolen|通道是否限价（true：限价 false：不限价）
limit_price|string 16|限价折扣（取值0~100，若通道不限价limit_price将不参与请求）
provider_type|string 16|运营商类型 1：电信 2：移动 3：联通
origin_price|string 16|原价
deduction_fee|string 16|代理商价格
product_status|string 8|套餐状态（1：正常 2：暂停）
recharge_status|string 8|套餐充值状态（1：正常 2：压单）
flow_value|string 16|流量值（单位MB）
eft_type|string 8|有效范围（0：全国有效 1：省内有效）

* 返回说明:
业务处理完毕后返回 http 状态码200的响应报头，超时或者非200状态码的 http 响应系统会判断回调失败，并且重新发起回调，共四次回调，间隔为30秒、60秒、90秒，120秒。