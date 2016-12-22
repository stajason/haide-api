###获取号码信息
* 请求方式：GET
* 请求地址：http://api.xunion.me/v1/phone/data?access_token=access_token&sign=sign&phone_no=phone_no
* 数据格式：key=value
* 请求参数：

参数名称|是否必须|注释
:------------|:------------|:------------
access_token|必须|由2.1获取的access_token字段信息
phone_no|必须|11位手机号码
sign|必须|签名


* 请求报文demo

```
GET http://api.xunion.me/v1/phone/data?access_token=$2y$10$k0x84QjJavDgYagOoQogo.Zl1uts8KKWFXd2RO2QZi5gYxJtVDCj6ew05atsbqj8klxteh875pny2tdgde5s7c2rrbgty&sign=33B9B8B70296D48211A80256C4B27C964A2F76AE&phone_no=15876598724 HTTP/1.1
User-Agent: Fiddler
Host: api.xunion.me
```

* 返回JSON数据:

```json
{
    "errcode": 0,
    "errmsg": "success",
    "phone_no": "15876598724",
    "phone_type": "中国移动",
    "province": "广东 广州市"
}
```
* 返回说明:

参数名称|类型|注释
:------------|:------------|:------------
phone_no|string 11|手机号码
phone_type|string 32|运营商 [中国移动，中国联通，中国电信]
province|string 32|省份，城市