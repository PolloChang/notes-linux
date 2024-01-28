# 我的Linux生活日記 08-LVM-新增硬碟-LVM

## Physical Drivers / Partitions / PV(Physical Volume) / VG(Volume Group) / LV(Logical Volume) 的相互關係

![LVM](./pic/lvm.jpg.crdownload)

## 情境敘述

新增一顆5GB的硬碟，並以 LVM ( Logical Volume Management ) 管理空間。硬碟檔案格式為 ext4。

## 新增硬碟步驟

1. 虛擬機新增硬碟
2. 取得硬碟資訊，確認實際要格式化那一顆硬碟
3. 計劃分割表
4. 新增 physical volume
5. 新增 Volume Group
6. 檢查 Volume Group 可以配置的大小
7. 將 Volume Group 可用的空間加入 Logical Volume
8. 硬碟分割
9. 掛載硬碟

## 虛擬機新增硬碟

首先未虛擬機新增一顆5 GB 的硬碟吧！

虛擬機這邊我是用 kvm/qemu，下面是新增的步驟。

![https://ithelp.ithome.com.tw/upload/images/20220921/20128528zx3RzOZ9iB.png](https://ithelp.ithome.com.tw/upload/images/20220921/20128528zx3RzOZ9iB.png)

![https://ithelp.ithome.com.tw/upload/images/20220921/201285281wFaXQkDc9.png](https://ithelp.ithome.com.tw/upload/images/20220921/201285281wFaXQkDc9.png)

## 取得硬碟資訊，確認實際要格式化那一顆硬碟

1. 取得目前掛載資訊

```bash
$ sudo df -h # 檢查目前的硬碟狀況
Filesystem      Size  Used Avail Use% Mounted on
udev            479M     0  479M   0% /dev
tmpfs            99M  3.0M   96M   4% /run
/dev/vda1        19G  1.5G   17G   8% /
tmpfs           494M     0  494M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           494M     0  494M   0% /sys/fs/cgroup
tmpfs            99M     0   99M   0% /run/user/1000
```

2. 列出硬碟所有資訊

```bash
sudo fdisk -l # 列出所有的硬碟狀況
Disk /dev/vda: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x1d8cb10f

Device     Boot    Start      End  Sectors  Size Id Type
/dev/vda1  *        2048 39942143 39940096   19G 83 Linux
/dev/vda2       39944190 41940991  1996802  975M  5 Extended
/dev/vda5       39944192 41940991  1996800  975M 82 Linux swap / Solaris


Disk /dev/vdb: 5 GiB, 5368709120 bytes, 10485760 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

```

由 第一項 與 第二項 可以得知這次要處理的硬碟為 `/dev/vdb`。

## 計劃分割表

1. 執行硬碟分割

```bash
sudo gdisk /dev/vdb
```

詳細資訊

```bash
sudo gdisk /dev/vdb
GPT fdisk (gdisk) version 1.0.3

Partition table scan:
  MBR: not present
  BSD: not present
  APM: not present
  GPT: not present

Creating new GPT entries.

Command (? for help): ?
b       back up GPT data to a file
c       change a partition's name
d       delete a partition
i       show detailed information on a partition
l       list known partition types
n       add a new partition
o       create a new empty GUID partition table (GPT)
p       print the partition table
q       quit without saving changes
r       recovery and transformation options (experts only)
s       sort partitions
t       change a partition's type code
v       verify disk
w       write table to disk and exit
x       extra functionality (experts only)
?       print this menu

Command (? for help): n
Partition number (1-128, default 1): 1
First sector (34-10485726, default = 2048) or {+-}size{KMGTP}: 
Last sector (2048-10485726, default = 10485726) or {+-}size{KMGTP}: 
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300): 
Changed type of partition to 'Linux filesystem'

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): y 
OK; writing new GUID partition table (GPT) to /dev/vdb.
The operation has completed successfully.
```

2. 檢查硬碟資訊

```bash
❯ sudo fdisk -l # 列出所有的硬碟狀況
Disk /dev/vda: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x1d8cb10f

Device     Boot    Start      End  Sectors  Size Id Type
/dev/vda1  *        2048 39942143 39940096   19G 83 Linux
/dev/vda2       39944190 41940991  1996802  975M  5 Extended
/dev/vda5       39944192 41940991  1996800  975M 82 Linux swap / Solaris


Disk /dev/vdb: 5 GiB, 5368709120 bytes, 10485760 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: A03E9FA4-9352-451B-A944-0D32AAD6D44B

Device     Start      End  Sectors Size Type
/dev/vdb1   2048 10485726 10483679   5G Linux filesystem
```

### 錯誤訊息: gdisk: command not found

* 錯誤資訊

```bash
❯ sudo gdisk /dev/vdb
sudo: gdisk: command not found
```

* 解決方式

```bash
sudo apt install gdisk
```

## 新增 physical volume

```bash
sudo pvcreate /dev/vdb1
```

* 執行結果

```bash
$ sudo pvcreate /dev/vdb1
Physical volume "/dev/vdb1" successfully created.
```

### 錯誤訊息: pvcreate: command not found

* 錯誤資訊

```bash
sudo pvcreate /dev/vdb1
sudo: pvcreate: command not found
```

* 解決方式

```bash
sudo apt install lvm2
```

## 新增 Volume Group

```bash
sudo vgcreate -s 16M test-pv-1 /dev/vdb1
```

* 執行結果

```bash
$ sudo vgcreate -s 16M test-pv-1 /dev/vdb1
Volume group "test-pv-1" successfully created
```

## 檢查 Volume Group 可以配置的大小

```bash
sudo vgdisplay test-pv-1
```

* 執行結果

```bash
❯ sudo vgdisplay test-pv-1
  --- Volume group ---
  VG Name               test-pv-1
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               4.98 GiB
  PE Size               16.00 MiB
  Total PE              319
  Alloc PE / Size       0 / 0   
  Free  PE / Size       319 / 4.98 GiB
  VG UUID               1LkB0A-Perm-1vIV-NsMr-DjSc-Cww0-BB7XsY
```

## 將 Volume Group 可用的空間加入 Logical Volume

```bash
sudo lvcreate -L 4.98G -n test-lv test-pv-1
```

* 執行結果

```bash
$ sudo lvcreate -L 4.98G -n test-lv test-pv-1
  Rounding up size to full physical extent 4.98 GiB
  Logical volume "test-lv" created.
```

## 硬碟分割

1. 檢查路徑

```bash
sudo lvdisplay
```

* 執行結果

```bash
❯ sudo lvdisplay
  --- Logical volume ---
  LV Path                /dev/test-pv-1/test-lv
  LV Name                test-lv
  VG Name                test-pv-1
  LV UUID                T0V5dm-tnLT-EpyZ-LnWM-FSfL-u2r1-xzZ2cs
  LV Write Access        read/write
  LV Creation host, time pollochangVM, 2022-09-20 23:37:16 +0800
  LV Status              available
  # open                 0
  LV Size                4.98 GiB
  Current LE             319
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
```

1. 執行分割

```bash
sudo mkfs.ext4 /dev/test-pv-1/test-lv
```

* 執行結果

```bash
$ sudo mkfs.ext4 /dev/test-pv-1/test-lv
mke2fs 1.44.5 (15-Dec-2018)
Creating filesystem with 1306624 4k blocks and 327040 inodes
Filesystem UUID: 9d305794-a97f-46a8-b484-6a59748915d7
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done 
```

## 掛載硬碟

1. 新增掛載目錄

```bash
sudo mkdir -p /test-disk
```

2. 嘗試掛載硬碟

```bash
sudo mount /dev/test-pv-1/test-lv /test-disk
```

3. 檢查掛載狀況

```bash
$ df -h
Filesystem                        Size  Used Avail Use% Mounted on
udev                              479M     0  479M   0% /dev
tmpfs                              99M  3.0M   96M   4% /run
/dev/vda1                          19G  1.5G   17G   9% /
/dev/mapper/test--pv--1-test--lv  4.9G   24K  4.6G   1% /test-disk
```

4. 查詢硬碟`UUID`

```bash
sudo blkid
```

* 執行結果

```bash
❯ sudo blkid 
/dev/mapper/test--pv--1-test--lv: UUID="9d305794-a97f-46a8-b484-6a59748915d7" TYPE="ext4"
```

5. 寫入fstab

```shell
UUID=ce5d3900-4b84-4ee9-bca5-a9af99ee67c6 /centos-repo ext4 defaults 0 1
```

6. 重開機檢查


如果文章內容有錯，請不吝色請教 m)(.__.)(m)Thank you.
