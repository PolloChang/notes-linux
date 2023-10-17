# 032-history時間戳記

history 時間戳記
   * 未配置前

```bash
pollochang@debian-12-template:~$ history
    1  sudo apt update
    2  sudo apt install qemu-guest-agent vim firewalld wget curl autofs rsync
    3  sudo vim /etc/profile.d/ssh-login-info.sh
    4  sudo chmod +x /etc/profile.d/ssh-login-info.sh
    5  sudo apt install tmux
    6  history
```

   * 配置後

```bash
pollochang@debian-12-template:~$ history
    1  2023-09-18 16:10:55 sudo -i
    2  2023-09-18 16:10:55 sudo apt update
    3  2023-09-18 16:10:55 sudo apt install qemu-guest-agent vim firewalld wget curl autofs rsync
    4  2023-09-18 16:10:55 sudo vim /etc/profile.d/ssh-login-info.sh
    5  2023-09-18 16:10:55 sudo chmod +x /etc/profile.d/ssh-login-info.sh
    6  2023-09-18 16:10:55 sudo apt install tmux
    7  2023-09-18 16:10:55 history
    8  2023-09-18 16:10:55 sudo vim /etc/profile.d/history.sh
    9  2023-09-18 16:10:58 history
```

  * 配置腳本: /etc/profile.d/history.sh

```bash
# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTCONTROL=ignoredups:ignorespace
HISTSIZE=50000
HISTFILESIZE=50000
HISTTIMEFORMAT='%F %T '
```

```bash
sudo chmod 0755 /etc/profile.d/history.sh
```