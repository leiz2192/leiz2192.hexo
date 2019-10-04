---
title: Flask使用https
date: 2019-10-04 23:39:14
tags: python
categories: python
---

# Flask使用https
## 概述
Flask在run时可以通过参数ssl_context指定证书来使得运行的web app使用较为安全的协议https.
## 安装pyOpenSSL
pyOpenSSL使Python的openssl库.
通过pip安装:
```
pip install pyOpenSSL
```
<!--more-->
## 默认证书
pyOpenSSL安装完成后就可以使用默认证书将Flask切换为https.
```python
from flask import Flask
app = Flask(__name__)
app.run('0.0.0.0', debug=True, port=5000, ssl_context='adhoc')
```
**ssl_context**参数指明使用的证书, **adhoc**是默认证书, 是pyOpenSSL自带的.
## 自定义证书
### Generate a private key
```
openssl genrsa -des3 -out server.key 1024
```
### Generate a CSR
```
openssl req -new -key server.key -out server.csr
```
### Remove Passphrase from key
```
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key
```
### Generate self signed certificate
```
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
```
**days**是指证书有效时间, **-days 3650**即证书有效期是3650天.

### 使用
```python
from flask import Flask
app = Flask(__name__)
app.run('0.0.0.0', debug=True, port=5000, ssl_context=('server.crt', 'server.key'))
```
**ssl_context**中配置的crt和key文件使用绝对路径或相对路径.
