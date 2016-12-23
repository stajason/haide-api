###PHP样例

```php
/**
 * 海德流量充值接口DEMO封装
 * @since 2016-08-22
 */
class HDFlow extends HDFlowBase
{
    public function __construct()
    {
        $this->api = 'http://api.xunion.me/v1/';
    }
    public $clientID = 'flow3355e802d44ay95yeev2';
    public $clientSecret = 'eal0ycl5argvq8v3cthm5qnzq1ljqf1429506832';
    public $token = '$2y$10$N8VTTlOaxnn3x.4.pzuCK.tysjymi3lMS41eJL76jP86R81KBRTB.b40mp42kcs0f41atin4g';

    /**
     * [getToken 获取token]
     */
    public function getToken()
    {
        $resp = $this->post('auth/token', [], ['client_id' => $this->clientID, 'client_secret' => $this->clientSecret, 'grant_type' => 'client_credential']);
        return json_decode($resp, true);
    }

    /**
     * [getProductLists 获取可用的商品列表]
     */
    public function getProductLists()
    {
        $resp = $this->get('product/lists', ['access_token' => $this->token]);
        return json_decode($resp, true);
    }

    /**
     * [getBalance 获取账号余额]
     */
    public function getBalance()
    {
        $resp = $this->get('account/balance', ['access_token' => $this->token]);
        return json_decode($resp, true);
    }

    /**
     * [getDetail 获取某一订单详情]
     * @param string $orderid 海德订单号
     */
    public function getDetail($orderid)
    {
        $package = [
            'order_id' => $orderid,
            'client_id' => $this->clientID,
        ];
        $sign = $this->getSign($package);
        $resp = $this->get('order/detail', ['access_token' => $this->token,'sign' => $sign,'order_id' => $orderid]);
        return json_decode($resp, true);
    }
    
    /**
     * [getLists 获取订单列表]
     * @param string $start_time 开始时间，和$end_time 成对出现，格式为 UNIX 时间戳
     * @param string $end_time 结束时间，和$start_time 成对出现，格式为 UNIX 时间戳
     * @param integer $page 页数
     */
    public function getLists($start_time,$end_time,$page = 1)
    {
        $package = [
            'start_time' => $start_time,
            'end_time' => $end_time,
            'page' => $page,
            'client_id' => $this->clientID,
        ];
        $sign = $this->getSign($package);
        $resp = $this->get('order/lists', ['access_token' => $this->token,'sign' => $sign,'start_time' => $start_time,'end_time' => $end_time,'page' => $page]);
        return json_decode($resp, true);
    }

    /**
     * [recharge 提交充值订单]
     * @param string $number 充值号码
     * @param string $product 套餐编号
     * @param string $callBack 回调地址
     * @param string $userOrderId 商户订单号
     */
    public function recharge($number, $product, $callBack, $userOrderId)
    {
        $package = [
            'number' => $number,
            'product_id' => $product,
            'client_id' => $this->clientID,
            'callback_url' => $callBack,
            'user_order_id' => $userOrderId
        ];
        $sign = $this->getSign($package);
        $data = [
            'number' => $number,
            'product_id' => $product,
            'callback_url' => $callBack,
            'user_order_id' => $userOrderId,
            'sign' => $sign
        ];
        $resp = $this->post('flow/recharge/order', ['access_token' => $this->token], $data);
        return json_decode($resp, true);
    }

    /**
     * [getHaiDeCallBack 接收来自海德的回调]
     * @return string
     */
    public function getHaiDeCallBack()
    {
        try {
            $resp = file_get_contents('php://input');
            if (empty($resp)) {
                return;
            }
            $data = json_decode($resp, true);
        } catch (Exception $e) {
            Log::error($e);
        }
        echo '200';
    }
}

class HDFlowBase
{
    protected $api = 'http://api.xunion.me/v1/';

    /**
     * [get get提交]
     * @param string $url 提交路径
     * @param string $params 参数
     * @return string $response 返回参数
     */
    protected function get($url, $params = '')
    {
        if (strpos($url, 'http://') === false ) {
            $url = $this->api . $url;
        }
        if ($params) {
            $url .= '?' . http_build_query($params);
        }
        $ch = curl_init();
        $options = [
            CURLOPT_URL => $url,
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_TIMEOUT => 180,
            CURLOPT_CONNECTTIMEOUT => 30
        ];
        curl_setopt_array($ch, $options);
        $response = curl_exec($ch);
        $resp_status = curl_getinfo($ch);
        curl_close($ch);
        if (intval($resp_status["http_code"]) == 200) {
            //如果获取数据成功则返回数据内容
            return $response;
        } else {
            throw new \Exception('HDFlow curl get error');
        }
    }

    /**
     * [post post提交]
     * @param string $url 提交路径
     * @param array  $params 地址栏参数
     * @param array  $data post参数
     * @param array  $headers 数据流输出
     * @return string $response 返回参数
     */
    protected function post($url, $params = '', $data = '', $headers = [])
    {
        if (strpos($url, 'http://') === false) {
            $url = $this->api . $url;
        }
        if ($params) {
            $url .= '?'.http_build_query($params);
        }
        $ch = curl_init();
        $options = [
            CURLOPT_URL => $url,
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_POST => true,
            CURLOPT_TIMEOUT => 180,
            CURLOPT_CONNECTTIMEOUT => 30
        ];
        $headers[] = 'Content-Type: application/json';
        if ($headers) {
            $options[CURLOPT_HTTPHEADER] = $headers;
        }
        if ($data) {
            $options[CURLOPT_POSTFIELDS] = json_encode($data);
        }
        curl_setopt_array($ch, $options);
        $response = curl_exec($ch);
        $resp_status = curl_getinfo($ch);
        curl_close($ch);
        if (intval($resp_status["http_code"]) == 200) {
            return $response;
        } else {
            throw new \Exception('HDFlow curl post error');
        }
    }
    
    /**
     * [formatBizQueryParaMap 格式化参数，签名过程需要使用]
     * @param array $paraMap 升序排序后的数组
     * @return string $reqPar 未加密前的字符串
     */
    protected function formatBizQueryParaMap($paraMap)
    {
        $buff = '';
        ksort($paraMap);
        foreach ($paraMap as $k => $v){
            $buff .= $k . '=' . $v . '&';
        }
        $reqPar = '';
        if (strlen($buff) > 0) {
            $reqPar = substr($buff, 0, strlen($buff)-1);
        }
        return $reqPar;
    }
    
    /**
     * [getSign 生成签名]
     * @param array $data 传参数据
     * @return string $result_  加密后的签名
     */
    protected function getSign($data)
    {
        foreach ($data as $k => $v) {
            $Parameters[$k] = $v;
        }
        //签名步骤一：按字典序排序参数
        ksort($Parameters);
        $String = $this->formatBizQueryParaMap($Parameters, false);
        //签名步骤二：在$String后加入client_id
        $String = $String . $this->clientID;
        //签名步骤三：sha1加密
        $String = sha1($String);
        //签名步骤四：所有字符转为大写
        $result_ = strtoupper($String);
        return $result_;
    }
}
```
