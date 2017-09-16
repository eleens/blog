---
layout: post
title:  用python调用阿里云API
date:   2017-05-25 13:12:01 +0800
categories: python
tag: 阿里云
---

* content
{:toc}

1、调用格式
------------

协议 + 服务地址 + 请求参数

协议：http 或者https

服务地址：ecs.aliyuncs.com （ECS API服务接入地址），cdn.aliyuncs.com（CDN API服务接入地址），slb.aliyuncs.com（SLB API服务接入地址），ess.aliyuncs.com（弹性计算 API服务接入地址），rds.aliyuncs.com（RDS API服务接入地址），vpc.aliyuncs.com（VPC API服务接入地址）（更多的可参见阿里云官网）

请求参数：这里列出公共请求参数

公共请求参数是指每个接口都需要使用到的请求参数。


| 名称               |类型    |是否必须  |描述|
| --------           | ----- |--------|------|
| Format             |String |否    |返回值的类型，支持 JSON 与 XML。默认为 XML。
| Version            |String |是    |API 版本号，为日期形式：YYYY-MM-DD，本版本对应为 2014-05-26。
| AccessKeyId        |String |是    |阿里云颁发给用户的访问服务所用的密钥 ID。
|Signature           |String |是    |签名结果串，关于签名的计算方法，请参见<签名机制>。
|SignatureMethod     |String |是    |签名方式，目前支持 HMAC-SHA1。
|Timestamp           |String |是    |请求的时间戳。日期格式按照 ISO8601 标准表示，并需要使用 UTC 时间。格式为： YYYY-MM-DDThh:mm:ssZ  例如，2014-05-26T12:00:00Z（为北京时间 2014 年 5 月 26 日 20 点 0 分 0 秒）。
|SignatureVersion    |String |是    |签名算法版本，目前版本是 1.0。
|SignatureNonce      |String |是    |唯一随机数，用于防止网络重放攻击。用户在不同请求间要使用不同的随机数值
|ResourceOwnerAccount|String |否    |本次 API 请求访问到的资源拥有者账户，即登录用户名。此参数的使用方法，详见< 借助 RAM 实现子账号对主账号的 ECS 资源访问 >，（只能在 RAM 中可对 ECS 资源进行授权的 Action 中才能使用此参数，否则访问会被拒绝）

调用方法：GET

编码：UTF-8

例如：http://ecs.aliyuncs.com/?SignatureVersion=1.0&Format=JSON&TimeStamp=2017-05-18T01%3A42%3A55Z&AccessKeyId=LTAIATZLOUhWulMG&SignatureMethod=HMAC-SHA1&Version=2014-05-26&Signature=kSaXuBaImvpiah1DE6DqYx9O8%2Fo%3D&Action=DescribeRegions&SignatureNonce=505581d1-3b6b-11e7-9a22-b0c090572a4b

2、签名机制
------------

这里签名是根据AccessKeyId和Access Key Secret用特定的算法计算出Signature

1）参数规范化

参数按照参数名称的字典顺序对参数进行排序，然后对参数和参数名分别用 UTF-8 字符集进行 URL 编码，然后用=连接，把编码之后的进行排序，然后各个参数之间用&连接，即得到规范化字符串。

编码的规则如下：

+ 对于字符 A~Z、a~z、0~9 以及字符“-”、“_”、“.”、“~”不编码；
+ 对于其它字符编码成 %XY 的格式，其中 XY 是字符对应 ASCII 码的 16 进制表示。比如英文的双引号（”）对应的编码为 %22；
+ 对于扩展的 UTF-8 字符，编码成 %XY%ZA… 的格式；
+ 英文空格（ ）要编码成 %20，而不是加号（+）。
+ 实现时，可以先用标准库的方式进行编码，然后把编码后的字符串中加号（+）替换成 %20、星号（*）替换成 %2A、%7E 替换回波浪号（~），即可得到上述规则描述的编码字符串。

2）构造待签名的字符串StringToSign

StringToSign= HTTPMethod + “&” + 编码方法(“/”) + ”&” + 编码方法(第一步得到的字符串)

3）计算签名值

计算待签名字符串 StringToSign 的 HMAC 值，用的是SHA1，key是Access Key Secret。按照 Base64 编码规则把上面的 HMAC 值编码成字符串，即得到签名值（Signature）。

3、调用API代码
------------
以调用ECS的DescribeRegions API为例，计算签名值并调用API

1）拼装参数


    import time
    import uuid
    accessKeyId = 'testid'
    timestamp = time.strftime("%Y-%m-%dT%H:%M:%SZ", time.gmtime())
    params = {
        'Format': 'XML',
        'Version': '2014-05-26',
        'AccessKeyId': accessKeyId,
        'SignatureVersion': '1.0',
        'SignatureMethod': 'HMAC-SHA1',
        'SignatureNonce': str(uuid.uuid1()),
        'TimeStamp': timestamp,
        'Action': 'DescribeRegions',
    }

2）参数排序

    sortedParams = sorted(params.items(), key=lambda params: params[0])

3）定义编码方法，对参数进行编码，并用=和&进行连接

    import sys
    import urllib,urllib2
    #编码方法
    def percent_encode(str):
        res = urllib.quote(str.decode(sys.stdin.encoding).encode('utf8'), '')
        res = res.replace('+', '%20')
        res = res.replace('*', '%2A')
        res = res.replace('%7E', '~')
        return res
    #对参数进行编码并用=和&进行连接
    canonicalizedQueryString = ''
    for (k, v) in sortedParams:
        canonicalizedQueryString += '&' + percent_encode(k) + '=' + percent_encode(v)

4）构造待签名的字符串StringToSign

    stringToSign = 'GET&'+ percent_encode('/') + '&' + percent_encode(canonicalizedQueryString[1:])

5）计算哈希值

    import hmac
    from hashlib import sha1
    accessKeySecret='testsecret'
    h = hmac.new(accessKeySecret + "&", stringToSign, sha1)

6）base64编码

    import base64
    signature = base64.encodestring(h.digest()).strip()

7）得到签名，封装url

    server_address = 'http://ecs.aliyuncs.com'
    params['Signature'] = signature
    url = server_address + "/?" + urllib.urlencode(params)

8）得到url

    http://ecs.aliyuncs.com/?SignatureVersion=1.0&Version=2014-05-26&TimeStamp=2017-05-18T06%3A11%3A33Z&Format=XML&Action=DescribeRegions&SignatureNonce=d76e02cf-3b90-11e7-a775-b0c090572a4b&Signature=RZ2OdTwnBtgD3q9Sf7OmCIRgADU%3D&SignatureMethod=HMAC-SHA1&AccessKeyId=testid

9）通过url调用的结果

    <DescribeRegionsResponse>
    <RequestId>0B861D6C-1CF6-485C-AF0F-35159A7CC947</RequestId>
    <Regions>
    <Region>
    <RegionId>cn-shenzhen</RegionId>
    <LocalName>华南 1</LocalName>
    </Region>
    <Region>
    <RegionId>ap-southeast-1</RegionId>
    <LocalName>亚太东南 1 (新加坡)</LocalName>
    </Region>
    <Region>
    <RegionId>us-west-1</RegionId>
    <LocalName>美国西部 1 (硅谷)</LocalName>
    </Region>
    </Regions>
    </DescribeRegionsResponse>