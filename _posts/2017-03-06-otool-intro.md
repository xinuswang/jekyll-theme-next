---
title: macOS下面查看执行文件依赖信息
date: 2017-03-06 14:49:39
tags:
- otool
categories:
- macos
---

### 查看动态链接库信息
通过otool查看：
```
$ otool -L CppRest
CppRest:
	/usr/local/opt/cpprestsdk/lib/libcpprest.2.9.dylib (compatibility version 2.9.0, current version 0.0.0)
	/usr/local/opt/openssl/lib/libssl.1.0.0.dylib (compatibility version 1.0.0, current version 1.0.0)
	/usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib (compatibility version 1.0.0, current version 1.0.0)
	/usr/local/opt/boost/lib/libboost_system.dylib (compatibility version 0.0.0, current version 0.0.0)
	/usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 307.4.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.0.0)

```
<!-- more -->

### 查看符号表
通过nm查看:
```
nm CppRest
000000010000a21c s GCC_except_table138
000000010000a234 s GCC_except_table148
000000010000a254 s GCC_except_table156
000000010000a280 s GCC_except_table159
000000010000a2bc s GCC_except_table166
...
0000000100001370 t __ZN5boost4asio5error16get_ssl_categoryEv
00000001000012f0 t __ZN5boost4asio5error17get_misc_categoryEv
00000001000011f0 t __ZN5boost4asio5error18get_netdb_categoryEv
...
0000000100009970 t ___cxx_global_var_init
0000000100009990 t ___cxx_global_var_init.1
0000000100009a90 t ___cxx_global_var_init.12
0000000100009af0 t ___cxx_global_var_init.13
0000000100009b50 t ___cxx_global_var_init.14
0000000100009bb0 t ___cxx_global_var_init.15
0000000100009c10 t ___cxx_global_var_init.16
00000001000099b0 t ___cxx_global_var_init.2
00000001000099d0 t ___cxx_global_var_init.3
...
```
