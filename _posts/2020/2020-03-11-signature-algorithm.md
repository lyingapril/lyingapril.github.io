---
layout: post
title:  "几种支付签名生成方法"
date:   2020-03-11 16:05:38 +0800
categories: Post
tags: [php, sign]
---

　　总结最近用过几个签名方法。

## 签名方法
定义生成 sign 字符串的方法：
1. 对所有传入的参数按照字段名 **ASCII** (美国信息交换标准代码)从小到大排序，
使用 **URL** 键值对的格式(即 key1=value1&key2=value2…)拼接成字符串 **string1** ；
2. 在 **string1** 最后拼接上 key=Key(商户支付密钥)得到 **stringSignTemp** 字符串，
并对 **stringSignTemp** 进行 **md5** 运算，再将得到的字符串所有字符转换为大写，
得到 **sign** 值 **signValue** 。

## PHP代码实现
```php
<?php
    //...
$commit_url = 'https://commit.url.test';    //提交地址
$md5_key = 'o5aHp3DY9Q7JlOycuiMWe6nyYKXYyxZ8';    //密钥
$pay_appid = 'Mhw4UtWuPuFyMHAg';    //商户ID
$pay_nonce_str = md5(uniqid(microtime(true), true));    //32位随机字符串
$pay_out_trade_no = date('YmdHis') . rand(100, 999);    //订单号
$pay_total_fee = $_GET['num'] * 100;    //交易金额
$pay_notifyurl = 'http://' . $_SERVER['HTTP_HOST'] . '/index/index/noticeUrl';   //服务端通知地址

$data = array(
    'appid' => $pay_appid,
    'nonce_str' => $pay_nonce_str,
    'out_trade_no' => $pay_out_trade_no,
    'total_fee' => $pay_total_fee,
    'notifyurl' => $pay_notifyurl
);
ksort($data);
$paras = urldecode(http_build_query($data)) . '&key=' . $md5_key;
$data['sign'] = strtoupper(md5($paras));

//表单方式提交

echo '<form class="form-inline" id="myForm" method="post" action="'.$commit_url.'">';

foreach ($data as $key => $val) {
    echo '<input type="hidden" name="' . $key . '" value="' . $val . '">';
}

echo '</form>';

echo "<script>document.getElementById('myForm').submit();</script>";

//Curl方式提交

$params = json_encode($data);

$ch = curl_init();
//设置超时
curl_setopt($ch, CURLOPT_TIMEOUT, 30);
curl_setopt($ch, CURLOPT_URL, $commit_url);

//设置header
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: text/plain; charset=utf-8',
    'Content-Length: ' . strlen($params)
));
//要求结果为字符串且输出到屏幕上
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);

//post提交方式
curl_setopt($ch, CURLOPT_POST, TRUE);
curl_setopt($ch, CURLOPT_POSTFIELDS, $params);
//运行curl
$data = curl_exec($ch);
if ($data) {
    curl_close($ch);
    $data = json_decode($data,true);
    echo ($data['RET_HTML']);
} else {
    $error = curl_errno($ch);
    curl_close($ch);
    print("curl出错，错误码:$error");
}
```