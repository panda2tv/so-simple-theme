---
title: "把网站从http换成https一文搞定"
excerpt: "从申请CA证书到设置nginx一文搞定怎样把http换成https"
last_modified_at: 2019-02-25
---

 # 记录一下把http转成https的整个过程  

现在打开http的网页，浏览器总会提示‘不安全链接’，让人很不舒服，而且https在搜索引擎权重更高，所以虽然是静态网站，我也把我的网址换成https的。  
  
分为以下几步  
1. 申请CA证书  
在这个网址申请CA证书 https://freessl.cn/  这个网站可以免费申请一年；
网站里面的说明不清不楚的，浪费了不少时间；  

首先要创建免费证书，输入网站域名，邮件，点击创建；  

2. 设置域名解析  
拿到创建的记录值，到域名提供商的网页填写解析信息； 这里记录必须是TXT类型，然后用"*"做主机记录，这样才能匹配xxx.com 和 www.xxx.com;  

3. 验证证书  
最长10分钟后回到freessl点击验证，通过的话会下载一个压缩包，里面2个文件  
公钥和私钥, full_chain.pem, private.key  

4. 修改nginx设置并重启nginx服务  
这里分享一下我的nginx config设置；  

```
server{
    listen     80;
    listen     443 ssl; # 监听端口
    # 开启 ssl
    ssl on;
    # 指定 ssl 证书路径
    ssl_certificate /etc/pki/tls/certs/full_chain.pem;
    # 指定私钥文件路径
    ssl_certificate_key /etc/pki/tls/certs/private.key;

    server_name www.yousite.com yousite.com;    # 站点域名
    root  /var/www/yourwebsite.com;              # 站点根目录
    index index.html;   # 默认导航页

    # www to non www
    if ( $host = "www.yourwebsite.com" ){
        return 301 https://yourwebsite.com$request_uri;
    }

    #http to https
    if ( $scheme = http ){
        return 301 https://$server_name$request_uri;
    }

}

```
改好设置重启nginx服务  
```
nginx -s reload  
```
nginx轻量好用功能强，用来替换了Apache
