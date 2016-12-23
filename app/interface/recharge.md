###提交充值订单
* 请求方式：POST
* 请求地址：http://api.xunion.me/v1/flow/recharge/order?access_token=access_token
* 数据格式：JSON
* 请求参数：

参数名称|是否必须|注释
:------------|:------------|:------------
number|是|手机号码
product_id|是|套餐编号
callback_url|否|回调地址，可选参数，填则回调，不填则不回调（设置回调地址有两种方式：1.接口传入回调地址 2.商务在后台配置固定的回调地址。如果同时在接口传入回调地址和后台配置了固定回调地址，那么优先选择接口传入的回调地址）
user_order_id|否|商户订单号，可选参数，非空则判断唯一性，最多支持40位的字符串
sign|是|签名 <a href="../../app/explain/signature.md">详情见1.3</a>

* 请求报文demo

```json
POST http://api.xunion.me/v1/flow/recharge/order?access_token=$2y$10$SKvmSE.VRPP8mliy4uhwg.yrkQXzMmwfP1mLTVL6YwHqh5JVJBPOy7w9azplhccpawfqmnyw1 HTTP/1.1
User-Agent: Fiddler
Host: api.xunion.me
Content-Length: 229
Content-Type: application/json

{
    "number": "13593868168",
    "user_order_id": "20161122112033770940",
    "product_id": "2_10",
    "callback_url": "http://api.xunion.me/cgi/haide/callback/",
    "sign": "7084774BA53FD251CE170ED2EA87333F721A167C"
}
```

* 返回JSON数据：

    * 1.提交成功demo：

        ```json
        {
            "errcode": 0,
            "errmsg": "success",
            "order": {
                "transaction_id": "201511181328427155909265",
                "number": "15876598724",
                "product_id": "2_30",
                "fee": "5.00",
                "deduction_fee": "4.90"
            }
        }
        ```
    * 2.提交失败demo：

        ```json
        {
            "errcode": 50008,
            "errmsg": "签名有误"
        }
        ```

* 返回说明：

参数名称|类型|注释
:------------|:------------|:------------
order|object|订单对象
transaction_id|string 32|平台生成订单号
number|string 11|充值号码
product_id|string 16|套餐编号
fee|float 4,2|原价
deduction_fee|float 4,2|扣费金额
