---
layout:     post
title:      在petalinux的嵌入式系统中增加开机启动脚本
subtitle:   
date:       2023-10-28
author:     DY
catalog: false
tags:
    - linux
---
开机脚本 init.sh 内容如下：
```
#!/bin/sh
echo "begin initial process" >> /home/root/log.txt

case "$1" in
    start)
        echo "init.sh start" >> /home/root/log.txt
        /home/root/9361_init.elf
        ;;

    stop)
        echo "init.sh stop" >> /home/root/log.txt
        ;;
    *)
        echo "Usage: /etc/init.d/init.sh {start|stop}"
        exit 1
        ;;
esac

exit 0
```

在系统中增加开机启动项

```
 #在ubuntu环境认可的命令、成功后自动添加到对应的rc.d文件夹中
sudo update-rc.d /etc/init.d/init.sh defaults 90   #若报错用下面命令
#或
cd /etc/init.d;update-rc.d init.sh defaults 90 

#在centos环境认可的命令
sudo chkconfig --add init.sh  && chkconfig init.sh on 
```

