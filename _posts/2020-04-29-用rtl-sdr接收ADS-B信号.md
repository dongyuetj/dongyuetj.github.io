---
layout:     post
mathjax:    true
title:      用rtl-sdr接收ADS-B信号
date:       2020-04-29
author:     DY
header-img: img/lebin.jpg
catalog: false
tags:
    - 技术
---
因为工作中要对ADS-B信号进行软解调，于是买了一块rtl28xx电视棒接收ADS-B信号。

首先安装rtl-sdr，具体的安装过程参照Ranous(<https://ranous.wordpress.com/rtl-sdr4linux/)>给出的教程。

```bash
git clone git://git.osmocom.org/rtl-sdr.git
cd rtl-sdr/
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make
sudo make install
sudo ldconfig
sudo cp ../rtl-sdr.rules /etc/udev/rules.d/
```

安装好rtl-sdr之后，需要增加黑名单（因为系统默认为RTL28xx为电视棒）。进入路径 /etc/modprobe.d，获得超级用户权限，

```bash
touch blacklist-rtl.conf
echo "blacklist dvb_usb_rtl28xxu" > blacklist-rtl.conf
```

由于我之前已将rtl28xxu电视棒插到电脑上了，我还需要重启机器。然后运行

```bash
rtl_test -t
```

运行结果如下：


![rtl_test](\img\rtl_test.jpg)

运行结果会出现PLL not locked！和 No E4000 tuner found, aborting的提示字样。这是正常的，不用担心。

接着，从github上下载antirez(<https://github.com/antirez/dump1090>)的代码。

```bash
git clone git://github.com/tedsluis/dump1090.git dump1090
cd dump1090
make
./dump1090 --interactive
```

应该就可以就接收到信号了。

但是，这个代码在我的台式机上编译报错。因为Makefile指向的库文件找不到（但是我感觉Makefile的设置没有问题），无奈之下，上网找解决方案。通过把库的绝对路径直接写进makefile里才顺利编译。

```
CFLAGS?=-O2 -g -Wall -W $(shell pkg-config --cflags librtlsdr)
LDLIBS+=$(shell pkg-config --libs librtlsdr) -lpthread -lm
CC?=gcc
PROGNAME=dump1090

all: dump1090

%.o: %.c
	$(CC) $(CFLAGS) -c $<

dump1090: dump1090.o anet.o
	$(CC) -g -o dump1090 dump1090.o anet.o $(LDFLAGS) $(LDLIBS)

clean:
	rm -f *.o dump1090
```

改成


```
CFLAGS?=-O2 -g -Wall -W $(shell pkg-config --cflags librtlsdr)
LDLIBS+=-lrtlsdr -lpthread -lm
CC?=gcc
PROGNAME=dump1090

all: dump1090

%.o: %.c
	$(CC) $(CFLAGS) -c $<

dump1090: dump1090.o anet.o
	$(CC) -g -o dump1090 dump1090.o anet.o $(LDFLAGS) $(LDLIBS)

clean:
	rm -f *.o dump1090
```


刚运行程序，就收到了一条消息：

![adsb_message9148](\img\adsb_message9148.jpg)

打开航旅纵横

![flight9148](\img\flignth9148.jpg)

再看一下现在的时间大概11点30左右，确认就是这架航班了。

其它结果：


![adsb_message4843](\img\adsb_message4843.jpg)

![flight4843](\img\flignth4843.jpg)

![adsb_message2822](\img\adsb_message2822.jpg)

由于我没有校正 rtl28xxu 的频偏，增益模式也是固定的，不是agc模式，因此能收到17675米海拔高度的信号，灵敏度就还算可以了。