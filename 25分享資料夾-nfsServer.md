# 我的Linux生活日記 25.分享資料夾-nfs Server

最近工作上有做目錄共用的功能，主要是 linux 主機間共享目錄而已，所以這邊就順便整理之前的筆記吧！

另外我自己工作主機及自己件的環境是以 Debian為主，所以這邊就附上 Debian 與 CentOS 的用法，原則上使用差異沒有很大。

## 安裝

* CentOS7

```bash
sudo yum install nfs-utils
```

* Debian 10

```shell
sudo apt install nfs-kernel-server nfs-common
```

## 啟動服務
* CentOS6

```bash
service nfs start
chkconfig nfs on
chkconfig rpcbind on
```

* CentOS7

```bash
sudo systemctl start rpcbind
```

```bash
sudo systemctl restart nfs
```

* Debian 10,Oracle Linux 9

```bash
sudo systemctl start nfs-server
```

## 設定要分享的目錄

假設是 `/share` 要分享

```
sudo mkdir -p /share
sudo chmod 777 /share
```

* 設定文件: `/etc/exports`

```bash
# 目標目錄      要分享的IP       使用權限
/share         192.168.56.0/24(rw)
```

## 設定防火牆

我實做後發現只需要 新增 `nfs` 就可以了，但是看網路很多都寫到需要多開 `rpc-bind` 跟 `mountd` 所以我一樣記著，但是原則上能少開就少開。

```bash
sudo firewall-cmd --permanent --add-service=nfs
# sudo firewall-cmd --permanent --add-service=rpc-bind
# sudo firewall-cmd --permanent --add-service=mountd
sudo firewall-cmd --reload
```

```bash
sudo firewall-cmd --permanent --zone=libvirt --add-service=nfs
sudo firewall-cmd --permanent --zone=libvirt --add-service=rpc-bind
sudo firewall-cmd --permanent --zone=libvirt --add-service=mountd
```



## NFS


### NFS 4.0 之前：

NFS 啟動時會向 rpcbind 註冊

NFS 的 port 是 rpcbind(TCP 111) 所配發的

rpcbind restart，NFS 也要跟著 restart

查詢遠端主機開放的目錄：sudo showmount -e [remote_host_name or IP]

掛載：sudo mount -t nfs [remote_host_name or IP]:/content /mnt

### NFS 4.0：

固定使用 TCP port 2049

無法使用 showmount，因此必須預先知道開放的目錄

同時掛載所有開放的目錄：mount -t nfs [remote_host_name or IP]:/ /mnt (確定路徑也可以 mount 特定目錄)

NFSv2, NFSv3, NFSv4 預設是同時開啟的


介紹完架設 nfs-server 之後接下來就是在 nfs-client 設定啦！

明天我們繼續詳細解說～

## 參考資料

[[RHCE7] RH134 Chapter 11. Accessing Network Storage with Network File System (NFS) 學習筆記](https://godleon.github.io/blog/RHCE/RHCE7-RH134-LearningNotes-CH11_AccessingNetworkStorageWithNetworkFileSystem(NFS)/)
