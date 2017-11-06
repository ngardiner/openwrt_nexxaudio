# OpenWRT NEXX WT3020 Custom Image
NEXX Audio Server customizations for OpenWRT

It seems that WT3020 support was added with the following patchset: https://dev.openwrt.org/browser/trunk/0001-ramips-add-support-for-Nexx-WT3020-devices.patch?rev=42983

This is after the chaos calmer buidld?

## Platform
   * Nexx WT3020 - OpenWRT Chaos Calmer 15.05.1 / LuCI 15.05-149-g0d8bbd2 Release (git-15.363.78009-956be55)
      * https://wiki.openwrt.org/toh/nexx/wt3020
      * WT3020F 	Yes 	8 MB 	Wireless router with USB port and 3G dongle support¹ 
      * MediaTek MT7620n
      * https://downloads.openwrt.org/chaos_calmer/15.05-rc3/ramips/mt7620/openwrt-15.05-rc3-ramips-mt7620-wt3020-8M-squashfs-factory.bin

### Officially Supported

Although there are some options potentially available to us to backport support for the Nexx WT3020 (see below), official support exists for the following:
   * OpenWRT 15.05 - 15.05.1
   * LEDE 17.01.0 - 17.01.3

## Issues
| Release         | Kernel  | Pulse Version  | Behaviour             |
| --------------- | ------- | -------------- | --------------------- |
| OpenWRT 15.05.1 | 3.18.23 | pulseaudio 6.0 | 48K RTP does not work |
| LEDE 17.01.3    | 4.4.89  | pulseaudio 9.0 | Random Drops          |

### LEDE 17.01.3 Random Drops

W: [alsa-sink-USB Audio] alsa-util.c: Could not recover from POLLERR|POLLNVAL|POLLHUP and XRUN: Broken pipe

Bus 001 Device 003: ID 0c76:161f JMTek, LLC.

Unfortunately, when this behaviour occurs it does not cause the pulseaudio daemon to stop running or signal any issue, which will leave the system without audio until the daemon is manually restarted.

## Packages
   * Add
      * alsa-lib
      * alsa-utils
      * kmod-sound-core
      * kmod-usb-audio
      * kmod-usb-core
      * kmod-usb-ohci
      * kmod-usb2
      * libncurses
      * libopenssl
      * libsndfile
      * libspeexdsp
   * Remove
      * dnsmasq
      * firewall
      * ip6tables
      * iptables
      * kmod-ip6tables
      * kmod-ipt-conntrack
      * kmod-ipt-core
      * kmod-ipt-nat
      * kmod-nf-conntrack
      * kmod-nf-conntrack6
      * kmod-nf-ipt
      * kmod-nf-ipt6
      * kmod-nf-nat
      * kmod-nf-nathelper
      * kmod-ppp
      * kmod-pppoe
      * kmod-pppox
      * libuci
      * libuci-lua
      * luci*
      * wpad-mini

```
Architecture: MIPS 24KEc V5.0 (ramips)
Vendor: MediaTek
Bootloader: U-boot 1.1.3
System-On-Chip: MediaTek MT7620n
CPU Speed: 580MHz
RAM memory: 64 MB DDR
RAM chip: EtronTech EM6AB160TSE-5G
Flash memory: 8 MB
Flash chip: Winbond W25Q32BV (4MB), W25Q64BV (8MB) or cFeon Q64-104HIP (8MB) ¹
Wired network: 2 x Ethernet 100 Mbps (switched)
Ethernet chip: MediaTek MT7530 (SoC)
Ethernet switch: MediaTek MT7530 (integrated)
Wireless network: 2.4 GHz 802.11n MiMo 2x2:2
Wireless chip: MediaTek RT5390 (SoC)
Wireless antennas: 2 x printed on-board
USB: No / 1 x USB 2.0 host (A-type)
Serial port: Yes (TTL pins)
```

### Installation

#### Barrier Breaker (r42625)

Haven't tried this yet, but since the below patch was applied to r42983 (2014-10-20) and Barrier Breaker is a close revision, it's expected that this should apply with some encouragement. Will try it and see what needs to be changed.

```
git clone -b barrier_breaker git://github.com/openwrt/openwrt.git

cd openwrt

get https://dev.openwrt.org/export/42983/trunk/0001-ramips-add-support-for-Nexx-WT3020-devices.patch
patch -p1 < 0001-ramips-add-support-for-Nexx-WT3020-devices.patch
```

#### Chaos Calmer 15.05-rc3

I have no intention to try this version unless there is a compelling reason for it. WT3020 support was introduced in 15.05-rc3 but since it is supported in 15.05.1, this is preferred

```
git reset --hard 171f0fd10830acd3259f7c229f1b65b95595f388
```

#### Chaos Calmer 15.05.1

Support for NEXX WT3020 was introduced in Chaos Calmer 15.05 RC3 (yes, confusing, there's a Chaos Calmer 15.05 and a Chaos Calmer 15.05.1) and for some currently unknown reason, it was dropped out of the build target support in the 15.05.1 Github repository, but if you follow the instructions below, you can restore it.

```
git clone -b chaos_calmer git://github.com/openwrt/openwrt.git

cd openwrt
./scripts/feeds update -a
./scripts/feeds install -a

wget --no-check-certificates https://dev.openwrt.org/browser/branches/chaos_calmer/target/linux/ramips/image/Makefile?format=txt -O target/linux/ramips/image/Makefile

rm -rf tmp

make defconfig
make menuconfig
```

#### LEDE 17.01

```
git clone -b lede-17.01 https://git.lede-project.org/source.git lede-17.01
cd lede-17.01
```

#### LEDE Trunk

The following instructions will clone the latest LEDE trunk repository, 

```
git clone https://git.lede-project.org/source.git lede
cd lede

./scripts/feeds update -a
./scripts/feeds install -a

make defconfig
make menuconfig

make
```

The menuconfig actions which were used to generate the config file are <a href="ledetrunkmc.txt">here</a>

#### OpenWRT Trunk

The following instructions will clone the latest OpenWRT trunk repository, 

```
git clone git://github.com/openwrt/openwrt.git
cd openwrt

./scripts/feeds update -a
./scripts/feeds install -a

make defconfig
make menuconfig
```
