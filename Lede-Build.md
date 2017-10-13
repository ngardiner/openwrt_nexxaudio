# LEDE

```
cd lede-imagebuilder-17.01.3-ramips-mt7620.Linux-x86_64

make image PROFILE="wt3020-8M" PACKAGES="-dnsmasq -iptables -ip6tables -ppp -ppp-mod-pppoe -firewall -wpad-mini alsa-lib alsa-utils hostapd kmod-sound-core kmod-usb-audio kmod-usb-core kmod-usb2 monit pulseaudio openssh-sftp-server sshfs uhttpd usbutils zabbix-agentd zabbix-extra-mac80211 zabbix-extra-network zabbix-extra-wifi zabbix-get zabbix-sender"
```

On the device:

   * Add /etc/pulse to /etc/sysupgrade.conf

```
sysupgrade -v [image].bin
```

   * Comes up with default 192.168.1.1 ans 1.7M available on overlay
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

The device is now ready to be managed by Ansible.


## Todo

   * Check that it works with 48k RTP audio in study
   * Perform upgrade again with new monit image and check that it works
