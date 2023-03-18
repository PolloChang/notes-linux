# 30-LAB新建VM安裝

## guest's XML

```xml
<channel type='unix'>
   <target type='virtio' name='org.qemu.guest_agent.0'/>
</channel>
```

## linux

### Debain

```bash
sudo apt install -y qemu-guest-agent nmon curl wget vim autofs
```

```bash
sudo systemctl start qemu-guest-agent
sudo systemctl enable qemu-guest-agent
```

* /etc/profile

```
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
```

```bash
source /etc/profile
```

### redhat

```bash
yum install qemu-guest-agent nmon curl wget vim autofs
```

```bash
systemctl start qemu-guest-agent
```

### CentOS6

```bash
service qemu-ga start
chkconfig qemu-ga on
```

## 參考資料

[Chapter 11. Enhancing Virtualization with the QEMU Guest Agent and SPICE Agent](https://access.redhat.com/documentation/zh-tw/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/chap-qemu_guest_agent)