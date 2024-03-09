---
title: "Linux Tips"
date: 2023-09-27T15:34:30-04:00
categories:
  - Blog
tags:
  - Linux
---
1. du -sh *，显示文件大小
2. mii-tool -v，显示网卡工作状态
3. ifconfig eth0 192.168.3.20 netmask 255.255.255.0，配置IP地址
4. lsb_release -a, 列出系统版本
5. cat /proc/version, 查看内核版本
6. /etc/init.d/networking status|restart，查看网卡状态和重启网卡
7. top，看当前运行的程序
8. bash -x test.sh,以调试方式运行shell脚本
9.  Shell 脚本也可以接收参数;$# ：包含参数的数目；$0 ：包含被运行的脚本的名称 （我们的示例中就是 variable.sh ；$1：包含第一个参数；$2：包含第二个参数；$8 ：包含第八个参数。
10. 进程就是加载到内存中运行的程序;Google的Chrome浏览器，每开一个标签栏都会创建一个新的进程
11. more, less, head, tail，查看文件，不编辑文件的时候不需要用vi
12. ln -s,创建软链
13. adduser, 增加用户，passwd xxx，为xxx修改密码
14. -exec 运行， find . -name "*.c" -ok chmod +x {} \;可以用ok来代替，这样改变权限的时候就会询问
15. grep name file, -n : 显示行号，-i 忽略大小写, -r 递归
16. wc，（word count），显示行数，单词书，和字节数
17. zcat,预览压缩包中的内容
18. sudo apt-get install openssh-server，sudo service ssh start，开启SSH服务
19. wget + http or ftp 地址, 下载文件
20. scp，从远程计算机拷贝
21. 配置IP地址，ifconfig eth0 192.168.120.56 netmask 255.255.255.0 broadcast 192.168.120.255
