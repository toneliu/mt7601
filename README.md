## MT7601U Linux driver
支持小度WIFI、360WIFI、小米WiFi。
Many cheap USB wifi dongles use the MT7601U chip.
Including 360widi xiaodu wifi and xiaomi wifi.
Now they are all drived by this driver.

### Usage

MT7601 Liunx驱动支持小度WIFI、360WIFI、小米WiFi。

使用lsusb命令可以获取USB设备ID。 小度wifi为2955:0001或2955:1001 　360WIFI2为148F:760B 二者均使用Mediatek Ralink MT7601芯片，都是在Mediatek官网下载MT7601U USB驱动源码包. 修改common/rtusb_dev_id.c文件，在 

{USB_DEVICE(0x148f,0x7601)}, /* MT 6370 /

下面加上 

{USB_DEVICE(0x2955,0x0001)}, / XiaoDu Wifi */

{USB_DEVICE(0x2955,0x1001)}, /* XiaoDu Wifi */

{USB_DEVICE(0x148f,0x760b)}, /* 360 Wifi */

使用make 命令编译后，执行make install 。 根据iwpriv_usage.txt，执行初始化或重启系统，网卡就可以使用了

First install kernel-devel for your Linux distro

```
git clone https://github.com/porjo/mt7601.git
cd mt7601/src
make
mkdir -p /etc/Wireless/RT2870STA/
cp RT2870STA.dat /etc/Wireless/RT2870STA/
insmod os/linux/mt7601Usta.ko
```

If the module has loaded OK, you should see `mt7601Usta` listed in the output of `lsmod` and a new network interface `ra0` should be present in the output of `ip link`.

If all goes well, you can permanently install the driver with `make install`.

### History

On 26 Aug, 2014 user **@poma** posted to the [linux-wireless](http://wireless.kernel.org/en/developers/MailingLists) mailing list discussing the poor state of driver support for this chipset. Thread can be seen here:

http://www.spinics.net/lists/linux-wireless/msg126115.html

An inital patch was released on 28 Aug, 2014 with the following comment:
```
A patch[1] is composed partly from the RT3573 source code patched by ashaffer, from Andreas work, some of the ideas are from the beagleboard community, and some of my. :)
Debug(trace) is turned off.

Device now works more or less OK but slow, max. 10 Mbit, although connectable is only within the "N" & "N/G" modes.
What is important is the system no longer crashes, and disconnection are rare.
Generally better than before.

Tested with kernels:
3.15.10-200.fc20.x86_64
3.16.1-301.fc21.x86_64
3.17.0-0.rc2.git0.1.fc22.x86_64

and with debug kernel:
3.17.0-0.rc2.git1.1.fc22.x86_64
```

A second patch was released on 31 Aug, 2014 with the following comment:

```
A new patch[1] mainly based on patches at 
https://github.com/ashaffer/rt3573sta
and several network throughput tests via the Iperf.
Tested with kernels:
- 3.15.10-200.fc20.x86_64
- 3.16.1-301.fc21.x86_64
- 3.17.0-0.rc2.git3.1.fc22.x86_64
- 3.16.1-301.fc21.i686
- 3.17.0-0.rc2.git3.1.fc22.i686
```

### Credits

**Source code:** (c) Copyright 2002-2013, MediaTek Inc. (released under GPLv2)

**Patch:** @poma at [linux-wireless](http://wireless.kernel.org/en/developers/MailingLists) mailing list
