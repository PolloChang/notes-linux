# 安裝作業系統完後建議處理事項-Debian

## 新增軟體

```bash
sudo apt install vim rsync resolvconf
```

## 雙網卡處理

* /etc/network/interfaces

```conf
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
# allow-hotplug enp1s0
auto enp1s0
iface enp1s0 inet dhcp

auto enp2s0
iface enp2s0 inet dhcp
```

### dhcp


如果是使用 dhcp 設定IP DNS 預設無法自行設定，需要另外安裝軟體

/etc/resolv.conf 會被重置

```bash
sudo apt install resolvconf
```

/etc/resolvconf/resolv.conf.d/base

```conf
nameserver 8.8.8.8
nameserver 114.114.114.114
```