###签名
* 在提交订单的时候需要将提交的数据签名加密
* access_token和sign不参与加密
* 加密/校验流程如下：

>1. 将提交的参数按照参数名进行字典序排序<font color='red'>(参数排序中需要加入client_id)</font>
>2. 将参数字符串拼接成一个字符串进行sha1加密，拼接的方式例子：callback_url=xxx&client_id=xxx&number=xxx&product_id=xxx
>3. 将获得的字符串转换成大写

* 说明：若接口返回签名错误点击以下链接进行验证<font color='red'>（签名测试工具）</font>http://www.xunion.me/admin/open/checksign

* PHP demo：

```php
    /**
     * 格式化参数，签名过程需要使用
     * @param array $paraMap 签名必需参数
     * @param boolen $urlencode 中文字符转换为十六进制
     * @return string $reqPar 拼接后的字符串
     */
    protected function formatBizQueryParaMap($paraMap, $urlencode)
    {
        $buff = "";
        ksort($paraMap);
        foreach ($paraMap as $key => $val) {
            if($urlencode) {
               $val = urlencode($val);
            }
            $buff .= $key ."=". $val ."&";
        }
        $reqPar = "";
        if (strlen($buff) > 0) {
            $reqPar = substr($buff, 0, -1);
        }
        return $reqPar;
    }

    /**
     * 生成签名
     * @param array $data 生成签名所需参数
     * @return string $result_  加密后的签名
     */
    protected function getSign($data)
    {
        foreach ($data as $key => $val) {
            $Parameters[$key] = $val;
        }
        // 签名步骤一：按字典序排序参数
        ksort($Parameters);
        $String = $this->formatBizQueryParaMap($Parameters, false);
        //签名步骤二：在$String后加入client_id
        $String = $String . $this->client_id;
        //签名步骤三：sha1加密
        $String = sha1($String);
        //签名步骤四：所有字符转为大写
        $result_ = strtoupper($String);
        return $result_;
    }
    
    /**
     * 调用加密签名方法
     */
    $signature = $this->getSign(
        array(
            'client_id' =>$this->client_id,
            'number' =>'15876598724', 
            'product_id' =>'2_10'
        )
    );
```
