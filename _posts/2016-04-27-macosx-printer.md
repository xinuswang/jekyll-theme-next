---
title: Mac系统连接Windows打印机
date: 2016-04-27 09:23:03
tags:
- printer
categories:
- macos
---

公司的网络是组了域的，Windows PC访问打印机很容易，直接再地址栏键入打印机地址，就可以访问到，比如`\\GROUP_PRINTER_SERVER\CTU_F88W167_HP_m706n_PRT1001`。

我的Mac并没有加入域，所以配置时，需要用IP地址来代替主机名称。

1. 打开`System Preference...`，找到`Printers & Scanners`并打开。
![image](/assets/images/macosx-printer-0.png)

2. 如下图，点击`+`添加新的打印机。
![image](/assets/images/macosx-printer-1.png)

3. 如下图，在`Add`页面中，如果工具栏中没有`Advanced`，可以再工具栏区域点击右键，在弹出菜单中选择`Customize Toolbar...`，出现入下图的界面后，把`Advanced`拖拽到工具栏，然后确认。
![image](/assets/images/macosx-printer-2.png)

4. 如下图，在`Add`页面中，切换到`Advanced`，填入打印机的相关信息，因为没有加入域，需要用IP地址替代主机名，最后点`Add`添加，如果一切正常，会提示是否双面打印，然后确认。
![image](/assets/images/macosx-printer-3.png)

5. 完成上述步骤后，可以打开一份`Word`文档来测试打印，打印时会询问用户名和密码，正确填写即可。
