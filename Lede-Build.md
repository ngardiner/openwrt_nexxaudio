# LEDE 17.01.3 Build

## Audio Reciever / Sender

```
cd lede-imagebuilder-17.01.3-ramips-mt7620.Linux-x86_64

make image PROFILE="wt3020-8M" PACKAGES="-dnsmasq -iptables -ip6tables -ppp -ppp-mod-pppoe -firewall -wpad-mini alsa-lib alsa-utils hostapd kmod-sound-core kmod-usb-audio kmod-usb-core kmod-usb-ohci kmod-usb2 monit pulseaudio openssh-sftp-server sshfs uhttpd usbutils zabbix-agentd zabbix-extra-mac80211 zabbix-extra-network zabbix-extra-wifi zabbix-get zabbix-sender" FILES="ledecustom_files/"
```

On the device:

   * Add /etc/pulse to /etc/sysupgrade.conf
   * ``` sysupgrade -v [image].bin ```

   * Comes up with default 192.168.1.1 and 1.5M available on overlay
   * Perform the networking config (LAN is vlan 3, remove WAN, all ports on VLAN 3)
   * Set root password and device hostname
   * Add sshfs mount to /etc/rc.local
```
mkdir -p /mnt/external

sshfs -o allow_other,nonempty,reconnect,directport=77XX,idmap=user,Compression=no,ServerAliveInterval=15,ServerAliveCountMax=5,noatime 192.168.29.130:/mnt/local/external/audio /mnt/external
```
   * Add external target to /etc/opkg.conf
      * This is not necessary, it will be done by Ansible
      * ```dest ext /mnt/external```
      * ```opkg install -dest ext bash python simplejson```

   * Add paths to /etc/profile
      * Not required. Now automatic
```
export PATH="/usr/sbin:/usr/bin:/sbin:/bin:/mnt/external/usr/sbin:/mnt/external/usr/bin:/mnt/external/sbin:/mnt/external/bin"
export LD_LIBRARY_PATH="/mnt/external/usr/lib"
```

   * Add /etc/dropbear/authorized_keys
      * Not required. Now automatic

The device is now ready to be managed by Ansible.

## Storage Server

### Introduction

This profile will be used to share USB attached media with the network.

### Image Builder

make image PROFILE="wt3020-8M" PACKAGES="-dnsmasq -iptables -ip6tables -ppp -ppp-mod-pppoe -firewall -wpad-mini blkid block-mount coreutils coreutils-dd e2fsprogs fdisk file fstools -hostapd -hostapd-common -iw -kmod-cfg80211 kmod-crypto-crc32c kmod-crypto-hash kmod-crypto-manager kmod-crypto-pcompress kmod-dm kmod-fs-ext4 kmod-fuse kmod-nbd kmod-scsi-core kmod-usb-storage kmod-usb-storage-extras kmod-usb-core kmod-usb-ohci kmod-usb-uhci kmod-usb2 kmod-usb3 kmod-usbip kmod-usbip-server lsof lvm2 monit ntfs-3g openssh-sftp-server resize2fs rsync socat usbutils zabbix-agentd zabbix-extra-network zabbix-get zabbix-sender"
