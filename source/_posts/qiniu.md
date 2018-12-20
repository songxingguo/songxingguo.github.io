title: 七牛云自定义域名配置
author: songxingguo
tags: []
categories: []
date: 2018-12-20 15:30:00
---
## 创建域名

[创建域名](https://portal.qiniu.com/cdn/domain/create?bucket=wecqut&fixBucket)

> 创建普通域名。

<!-- more -->

![创建域名](https://graphbed.qiniu.songxingguo.com/qiniu/%E5%88%9B%E5%BB%BA%E5%9F%9F%E5%90%8D.png)

## 使用 HTTPS

### 申请证书

[申请证书](https://console.cloud.tencent.com/ssl)

> 登录腾讯云，点击 SSL 证书管理 > 申请证书。

### 证书格式转换

[格式转换](https://myssl.com/cert_convert.html)

#### 原文件

![原文件](https://graphbed.qiniu.songxingguo.com/qiniu/%E5%8E%9F%E6%96%87%E4%BB%B6.png)

#### 目标文件

![目标文件](https://graphbed.qiniu.songxingguo.com/qiniu/%E7%9B%AE%E6%A0%87%E6%96%87%E4%BB%B6.png)

[查看 cer、crt 格式文件](https://myssl.com/cert_decode.html)

> 打开 cer 文件获取 PEM 格式代码。

![PEM格式](https://graphbed.qiniu.songxingguo.com/qiniu/PEM%E6%A0%BC%E5%BC%8F.png)

### 上传证书

[上传证书](https://portal.qiniu.com/certificate/ssl)

> 登录七牛云，SSL证书服务 > 证书管理 > 上传自有证书

![上传证书](https://graphbed.qiniu.songxingguo.com/qiniu/%E4%B8%8A%E4%BC%A0%E8%AF%81%E4%B9%A6.png)

PEM格式如下：

```
-----BEGIN CERTIFICATE-----
MIIEkjCCA3qgAwIBAgIQCgFBQgAAAVOFc2oLheynCDANBgkqhkiG9w0BAQsFADA/
MSQwIgYDVQQKExtEaWdpdGFsIFNpZ25hdHVyZSBUcnVzdCBDby4xFzAVBgNVBAMT
DkRTVCBSb290IENBIFgzMB4XDTE2MDMxNzE2NDA0NloXDTIxMDMxNzE2NDA0Nlow
SjELMAkGA1UEBhMCVVMxFjAUBgNVBAoTDUxldCdzIEVuY3J5cHQxIzAhBgNVBAMT
GkxldCdzIEVuY3J5cHQgQXV0aG9yaXR5IFgzMIIBIjANBgkqhkiG9w0BAQEFAAOC
AQ8AMIIBCgKCAQEAnNMM8FrlLke3cl03g7NoYzDq1zUmGSXhvb418XCSL7e4S0EF
q6meNQhY7LEqxGiHC6PjdeTm86dicbp5gWAf15Gan/PQeGdxyGkOlZHP/uaZ6WA8
SMx+yk13EiSdRxta67nsHjcAHJyse6cF6s5K671B5TaYucv9bTyWaN8jKkKQDIZ0
Z8h/pZq4UmEUEz9l6YKHy9v6Dlb2honzhT+Xhq+w3Brvaw2VFn3EK6BlspkENnWA
a6xK8xuQSXgvopZPKiAlKQTGdMDQMc2PMTiVFrqoM7hD8bEfwzB/onkxEz0tNvjj
/PIzark5McWvxI0NHWQWM6r6hCm21AvA2H3DkwIDAQABo4IBfTCCAXkwEgYDVR0T
AQH/BAgwBgEB/wIBADAOBgNVHQ8BAf8EBAMCAYYwfwYIKwYBBQUHAQEEczBxMDIG
CCsGAQUFBzABhiZodHRwOi8vaXNyZy50cnVzdGlkLm9jc3AuaWRlbnRydXN0LmNv
bTA7BggrBgEFBQcwAoYvaHR0cDovL2FwcHMuaWRlbnRydXN0LmNvbS9yb290cy9k
c3Ryb290Y2F4My5wN2MwHwYDVR0jBBgwFoAUxKexpHsscfrb4UuQdf/EFWCFiRAw
VAYDVR0gBE0wSzAIBgZngQwBAgEwPwYLKwYBBAGC3xMBAQEwMDAuBggrBgEFBQcC
ARYiaHR0cDovL2Nwcy5yb290LXgxLmxldHNlbmNyeXB0Lm9yZzA8BgNVHR8ENTAz
MDGgL6AthitodHRwOi8vY3JsLmlkZW50cnVzdC5jb20vRFNUUk9PVENBWDNDUkwu
Y3JsMB0GA1UdDgQWBBSoSmpjBH3duubRObemRWXv86jsoTANBgkqhkiG9w0BAQsF
AAOCAQEA3TPXEfNjWDjdGBX7CVW+dla5cEilaUcne8IkCJLxWh9KEik3JHRRHGJo
uM2VcGfl96S8TihRzZvoroed6ti6WqEBmtzw3Wodatg+VyOeph4EYpr/1wXKtx8/
wApIvJSwtmVi4MFU5aMqrSDE6ea73Mj2tcMyo5jMd6jmeWUHK8so/joWUoHOUgwu
X4Po1QYz+3dszkDqMp4fklxBwXRsW10KXzPMTZ+sOPAveyxindmjkW8lGy+QsRlG
PfZ+G6Z6h7mjem0Y+iWlkYcV4PIWL1iwBi8saCbGS5jN2p8M+X+Q7UNKEkROb3N6
KOqkqm57TH2H3eDJAkSnh6/DNFu0Qg==
-----END CERTIFICATE-----
```
## CNAME 配置

[CNAME 配置](https://developer.qiniu.com/fusion/kb/1322/how-to-configure-cname-domain-name)

### 获取CNAME值

[域名管理](https://portal.qiniu.com/cdn/domain)

> 登录七牛云， 进入域名管理复制 CNAME 。

![获取CNAME值](https://odum9helk.qnssl.com/FnnFpqz11MlQbpZsSlw1v2xPqRTD)

### 添加CNAME记录

![添加CNAME记录](https://odum9helk.qnssl.com/Fn5LvRsKMEUgBQM4PvvAccI-ISyO)

## 写在最后

[证书格式后缀说明](https://blog.csdn.net/qq_30698633/article/details/77895151)
