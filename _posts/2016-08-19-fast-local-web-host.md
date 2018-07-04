---
title: 快速开启HTTP服务器
date: 2016-08-19 14:50:11
tags:
- httpserver
categories:
- http
---

### Python快速快速搭建Web服务器
确保已经有Python环境，使用命令行快速搭建Web服务器
```bash
python -m SimpleHTTPServer 80
```
当前所在的文件夹设置为默认的Web目录，后面的80端口是可选的，不填会采用缺省端口8000。浏览器敲入地址
```
http://localhost:80
```
如果当前文件夹有index.html文件，会默认显示该文件。

### 参考文档
1. [SimpleHTTPServer — Simple HTTP request handler](https://docs.python.org/2/library/simplehttpserver.html)
