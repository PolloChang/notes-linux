# 我的linux日常生活-16.連接iPhone讀取照片

###### tags: `我的linux日常生活`

## 安裝 iFuse

```shell
jameschang@debian:~$ sudo apt-get install ifuse
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following NEW packages will be installed:
  ifuse
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 15.7 kB of archives.
After this operation, 48.1 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bullseye/main amd64 ifuse amd64 1.1.4~git20181007.3b00243-1 [15.7 kB]
Fetched 15.7 kB in 2s (7,099 B/s)
Selecting previously unselected package ifuse.
(Reading database ... 335631 files and directories currently installed.)
Preparing to unpack .../ifuse_1.1.4~git20181007.3b00243-1_amd64.deb ...
Unpacking ifuse (1.1.4~git20181007.3b00243-1) ...
Setting up ifuse (1.1.4~git20181007.3b00243-1) ...
Processing triggers for man-db (2.9.4-2) ...

```

### 掛載

```shell
# 1. 選擇或新造要掛載的目錄

jameschang@debian:~$ mkdir ~/iphone


# 2. 掛載
jameschang@debian:~$ ifuse ~/iphone
```

[How To Access Photos & Videos On Your iPhone On Linux](https://www.addictivetips.com/ubuntu-linux-tips/access-photos-videos-on-your-iphone-on-linux/)