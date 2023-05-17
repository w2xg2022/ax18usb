## 背景

在海鲜市场买了一台已经硬改1G内存和USB3.0和目AX18（兆能M2理论上也适用），一直苦于找不到同时兼具**无线速度**、**USB驱动**和**可靠软件源**的固件，于是开始收集资料、学习如何编译固件，才有了这个仓库。

目前已实现：

1. 5G无线贴脸测试，千兆带宽可跑上920Mbps。
2. USB可正常使用，通过无线存取samba共享的U盘（Class10TF卡+USB3.0读卡器），读取速度约55MB、写入约20MB。【外接的M2移动硬盘无法识别】
3. 大部分插件可安装和使用。【kmod类、lib类插件容易失败，信息为Cannot satisfy the following dependencies for luci-app-xxx、But that file is already provided by package libxxx，部分可通过--nodeps参数完成安装】


## 代码来源和主要调整

主要代码来自coolsnowwolf/lean、kiddin9等2位大佬，特此感谢。仓库地址如下：

1. https://github.com/coolsnowwolf/openwrt-gl-ax1800
2. https://github.com/kiddin9/OpenWrt_x86-r2s-r4s-r5s-N1/tree/master/devices/ipq60xx_generic

相关调整如下：

1. 参考360V6，改动target/linux/ipq60xx/files-4.4/arch/arm64/boot/dts
/qcom下的qcom-ipq6018-cmiot.dtsi，306~317行加上USB支持。
2. 无线驱动取用kiddin9库里的board-2.bin.IPQ6018。
3. 通过.vermagic，对齐固件和kiddin9软件源的内核版本：4.4.60-1-1。
4. 更新dnsproxy、hostapd版本，解决编译报错问题。


## 编译环境和命令

1. 在PC虚拟机使用Ubuntu x64 22.04.2、Github Actions使用Ubuntu 22.04成功编译过。

2. 安装编译依赖

	```bash

	sudo apt-get update
	sudo apt-get upgrade -y
	sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pyelftools libpython3-dev qemu-utils rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev

	```

3. 下载源代码，更新和安装feeds、选择配置、下载dl库，开始编译

	```bash
	git clone https://github.com/w2xg2022/ax18usb
	cd ax18usb
	
	#代码在Win10里整理的，权限都乱了，直接强制提权
	chmod -R 775 *

	./scripts/feeds update -a
	./scripts/feeds install -a

	#第一次编译最好直接用提供的default.config
	cp default.config .config
	make menuconfig

	#下载<a href ="https://pan.baidu.com/s/1JkQlDtkj0UPLUFpu73A64g?pwd=wb85">ax18usb_dl.rar</a>解压缩到dl 目录，比较快；保守起见都只用单线程，-j1可用-j$(nproc)替换为多线程
	
	make download -j1 V=s

	make -j1 V=s   
	```


## 云编译和固件下载

1.固件的默认IP、账号密码、插件清单、更换软件源等信息，请参考https://github.com/w2xg2022/actions4ax18usb。

2.固件下载页面：https://github.com/w2xg2022/actions4ax18usb/releases/tag/2023.05.16-1844，或直接<a href="https://github.com/w2xg2022/actions4ax18usb/releases/download/2023.05.16-1844/openwrt-ipq60xx-generic-cmiot_ax18-squashfs-nand-factory.ubi">下载</a>。
	

## 打赏

如果你觉得这个仓库、说明文档和固件等对你有帮助，能够激发和目AX18/兆能M2的潜能，欢迎通过微信打赏，谢谢。

 ![star](pic_star.png)
