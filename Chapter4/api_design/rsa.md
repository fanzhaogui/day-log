# RSA对称加密



## 获取公私钥

rsa 公私钥生成： http://web.chacuo.net/netrsakeypair



## 示例代码

~~~php
<?php
/**
 * rsa 对称加密 demo
 */

$param = [
    "name" => "fan",
    "age" => "33",
    "sex" => "man",
];
$request = json_encode($param);

$rsa_config = [
    'public_key' => '-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDEzGDKrgJQbkUhE3B4A29QyZB7
diX1cartkLS6cjOuVmUyYCkU9lKkpBL7V9ZhhNNEo20V7cNyYF625i1VbAog5+s/
JAK+us67mS73mUJfjD8D3fWLdY0gYrjB5x10qPtA75RJFDVPEIyv9qhbRmnefUnN
saznizUjblYna59WeQIDAQAB
-----END PUBLIC KEY-----',

    'private_key' => '-----BEGIN PRIVATE KEY-----
MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBAMTMYMquAlBuRSET
cHgDb1DJkHt2JfVxqu2QtLpyM65WZTJgKRT2UqSkEvtX1mGE00SjbRXtw3JgXrbm
LVVsCiDn6z8kAr66zruZLveZQl+MPwPd9Yt1jSBiuMHnHXSo+0DvlEkUNU8QjK/2
qFtGad59Sc2xrOeLNSNuVidrn1Z5AgMBAAECgYEAw7eid09Q+95+n6NukdyQox6i
szZQD+ZooHTuWBJldXd0kwHxOWizzgti2VaE3V00oymIvmtEmgZfi7Qk17Rn6Iwn
zjy6J00z0pgWEM7LOvUouEZAJi0123c5Q6rChuauMJshaSvYX6REU4fxkrcW6gev
KsBdURrYxrsqcHhirQECQQDx8c3K8QnF5DQpoF6D4A6vI6PjKcKng5a9eESpUqc9
IA5QTg1YgycwTo0UY9B4yIrYsBWIid2bRNcXP24mC3rhAkEA0DspHT1qOMGKKHc+
cGjtYjZbYmGfgMIh1ZeMcBQglkaX8QngyujyOV0lksT0OMz8wCYnqfikAtbsLUrw
3LWmmQJBAMttxNseUFlTx4g4jz/S5IXeMa6PLlwrCFPHC/RSystaaK6c8hu6Kvkz
EuxSALeN5zDK3VAwm2QyPagObU8P2AECQDiu4hJDlZa9mI5LZ4PSDEyf32B4kqLK
Ncue5WvdDsHZlaLXvYl+v/E9mezKEHEl2+eyezmZpYvgVYo+zDJIgIECQFOAZjcI
WhFtCZO9rb/g3wLGhNJlCHPdeZNA+3WDNGIsnferq9ko9EIrD55iKgRkFzp9d14d
CQ73VmGQ+KvvC84=
-----END PRIVATE KEY-----',
];
// 模拟 - 公钥加密
$rsa = new ProviderRsa($rsa_config);
$pubEncStr = $rsa->publicKeyEncode($request);
var_dump($pubEncStr);

// 模拟 - 私钥解密
$deEncData = $rsa->decodePublicEncode($pubEncStr);
var_dump($deEncData);


/**
 * @desc    RSA 公钥 私钥加密 解密
 */
class ProviderRsa
{
    private $_config;
    public function __construct($rsa_config= [])
    {
        $this->setRsaConfig();
        
        if(!empty($rsa_config)) $this->_config= $rsa_config; 
    }
    
    /**
     * @method ras 加密
     * @return void 
     */
    private function setRsaConfig(){
        $this->_config['public_key'] = '-----BEGIN PUBLIC KEY-----
MIGeMA0GCSqGSIb3DQEBAQUAA4GMADCBiAKBgGAY7QN/HN2guiY4gta2MUdtHi0+
hWHWPcnSvFK3vKxmRT6f3S48ETlu71P8X9M/5c0vTpyxqLZSdYLwoWq9390+cQyq
9cLPeWGi836K7MZaFwVZMJGWQmcf8QdmeJqA18Vin8QUVhbJE1hMo8HyfAPyqQS+
vh3OpTkTOIaBqn8FAgMBAAE=
-----END PUBLIC KEY-----';
        
        $this->_config['private_key']= '-----BEGIN RSA PRIVATE KEY-----
MIICWwIBAAKBgGAY7QN/HN2guiY4gta2MUdtHi0+hWHWPcnSvFK3vKxmRT6f3S48
ETlu71P8X9M/5c0vTpyxqLZSdYLwoWq9390+cQyq9cLPeWGi836K7MZaFwVZMJGW
Qmcf8QdmeJqA18Vin8QUVhbJE1hMo8HyfAPyqQS+vh3OpTkTOIaBqn8FAgMBAAEC
gYAM5QtYvsPG0XxpCIg1+3idVv0HoS4QtMjRvh9bEiCVGZwNDTKGs7Sz+jjPEjxh
gl95qvFngUdcP7BZA6UFR7k0Mq4rCKuztcPg8jAs8XmobFTKMmOO8EmrRTOaizaF
0CZeouESKJsUVlidDq3SRCoWFIStUZYCvgbK/y0qXZhfAQJBAKfXAPsYvf9BsZTz
J4e/mohWFVrzMRCMGnUqXU+NGjBlDhziwnEh6ttGhF1hoRtKxqo/F55QUkyOKpJM
BcQXNSUCQQCSkuSJpgvukBsBc3gXpyXwdZVduuwVcX6Yhvo8nc3Jmgz/EN+WW+jQ
8+lwXjjFIickHqaN9ow4ZSj+y0WQuSxhAkEAgh/FGOez1kSeYzapPSulqXHkGKFX
NtcIZDI2KcjhtweCC48a5Q9AwERJtwRMHZa5s6A6tXjcdZH7G3VpOwArKQJAG/Vj
HJKM0huw2w0Ailp61SxIqpFeORTmFgghMXDUcTEua3T3gUHU3g64p5OBdrD2EGC8
WnX99z/smvWBNoLr4QJAGGqScfkDthNQt/vDXeWFSYLVlZ4eCUQEDPWKOYtthKzr
29WEehzgX8XFFVcdnpX+LSLZA4aAzDiJAVJb8V3APw==
-----END RSA PRIVATE KEY-----';
        
    }
    /**
     * @desc 私钥加密
     * @param string $data 要加密的数据
     * @return string 加密后的字符串
     */
    public function privateKeyEncode($data)
    {
        $encrypted = '';
        $this->_needKey(2);
        $private_key = openssl_pkey_get_private($this->_config['private_key']);
        $fstr = array();
        $array_data = $this->_splitEncode($data); //把要加密的信息 base64 encode后 等长放入数组
        foreach ($array_data as $value) {//理论上是可以只加密数组中的第一个元素 其他的不加密 因为只要一个解密不出来 整体也就解密不出来 这里先全部加密
            openssl_private_encrypt($value, $encrypted, $private_key); //私钥加密
            $fstr[] = $encrypted; //对数组中每个加密
        }
        return base64_encode(serialize($fstr)); //序列化后base64_encode
    }

    /**
     * @desc 公钥加密
     * @param string $data 要加密的数据
     * @return string 加密后的字符串
     */
    public function publicKeyEncode($data)
    {
        $encrypted = '';
        $this->_needKey(1);
        $public_key = openssl_pkey_get_public($this->_config['public_key']);
        $fstr = array();
        $array_data = $this->_splitEncode($data);
        foreach ($array_data as $value) {
            openssl_public_encrypt($value, $encrypted, $public_key); //私钥加密
            $fstr[] = $encrypted;
        }
        return base64_encode(serialize($fstr));
    }

    /**
     * 用公钥解密私钥加密内容
     * @param string $data 要解密的数据
     * @return string 解密后的字符串
     */
    public function decodePrivateEncode($data)
    {
        $decrypted = '';
        $this->_needKey(1);
        $public_key = openssl_pkey_get_public($this->_config['public_key']);
        $array_data = $this->_toArray($data); //数据base64_decode 后 反序列化成数组
        $str = '';
        foreach ($array_data as $value) {
            openssl_public_decrypt($value, $decrypted, $public_key); //私钥加密的内容通过公钥可用解密出来
            $str .= $decrypted; //对数组中的每个元素解密 并拼接
        }
        return base64_decode($str); //把拼接的数据base64_decode 解密还原
    }

    /**
     * 用私钥解密公钥加密内容 
     * @param string $data  要解密的数据
     * @return string 解密后的字符串
     */
    public function decodePublicEncode($data)
    {
        $decrypted = '';
        $this->_needKey(2);
        $private_key = openssl_pkey_get_private($this->_config['private_key']);
        $array_data = $this->_toArray($data);
        $str = '';
        foreach ($array_data as $value) {
            openssl_private_decrypt($value, $decrypted, $private_key); //私钥解密
            $str .= $decrypted;
        }
        return base64_decode($str);
    }

    /**
     * 检查是否 含有所需配置文件
     * @param int 1 公钥 2 私钥
     * @return int 1
     * @throws \Exception
     */
    private function _needKey($type)
    {
        switch ($type) {
            case 1:
                if (empty($this->_config['public_key'])) {
                    throw new \Exception('请配置公钥');
                }
                break;
            case 2:
                if (empty($this->_config['private_key'])) {
                    throw new \Exception('请配置私钥');
                }
                break;
        }
        return 1;
    }

    /**
     * @param string  $data
     * @return array
     */
    private function _splitEncode($data)
    {
        $return = [];
        $data = base64_encode($data); //加上base_64 encode  便于用于 分组
        $total_lenth = strlen($data);
        $per = 96; // 能整除2 和 3 RSA每次加密不能超过100个
        $dy = $total_lenth % $per;
        $total_block = $dy ? ($total_lenth / $per) : ($total_lenth / $per - 1);
        $total_block = intval($total_block + 1);
        for ($i = 0; $i < $total_block; $i++) {
            $return[] = substr($data, $i * $per, $per); //把要加密的信息base64 后 按64长分组
        }
        return $return;
    }

    /**
     * @method 公钥加密并用 base64 serialize 过的 data
     * @param string $data base64 serialize 过的 data
     * @return array 
     */
    private function _toArray($data)
    {
        $data = base64_decode($data);
        $array_data = unserialize($data);
        if (!is_array($array_data)) {
            throw new \Exception('数据加密不符');
        }
        return $array_data;
    }
}
~~~

