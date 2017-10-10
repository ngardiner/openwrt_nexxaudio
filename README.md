# openwrt_nexxaudio
NEXX Audio Server customizations for OpenWRT

## Platform
   * Nexx WT3020 - OpenWRT Chaos Calmer 15.05.1 / LuCI 15.05-149-g0d8bbd2 Release (git-15.363.78009-956be55)
      * https://wiki.openwrt.org/toh/nexx/wt3020
      * WT3020F 	Yes 	8 MB 	Wireless router with USB port and 3G dongle support¹ 
      * MediaTek MT7620n
      * https://downloads.openwrt.org/chaos_calmer/15.05-rc3/ramips/mt7620/openwrt-15.05-rc3-ramips-mt7620-wt3020-8M-squashfs-factory.bin
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

git clone -b chaos_calmer git://github.com/openwrt/openwrt.git

cd openwrt
./scripts/feeds update -a
./scripts/feeds install -a

make menuconfig
