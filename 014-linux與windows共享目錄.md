# 我的linux日常生活-14.linux與windows共享目錄

###### tags: `我的linux日常生活`

## 安裝samba

```shell
sudo apt install samba
```

## 設定共享目錄


sudo vim /etc/samba/smb.conf

```shell
[localgit]
    comment = dadatong
    path = /home/pollochang/data/localGit
    browsable = yes
    guest ok = no
    read only = no
    create mask = 0755

[software]
    comment = dadatong
    path = /home/pollochang/data/software
    browsable = yes
    guest ok = no
    read only = yes
    create mask = 0755
    
[data]
    comment = dadatong
    path = /home/jameschang/data
    browsable = yes
    guest ok = no
    read only = no
    create mask = 0755
    
[tes1]
    comment = tes1
    path = /home/jc/tes1
    browsable = yes
    guest ok = no
    read only = no
    workgroup = oinstall
    create mode = 0775 #從Client端建立檔案後，建立之檔案權限會設定為create mode的值
	directory mode = 2770 #從Client端建立目錄後，建立之目錄權限會設定為directory mode的值
    valid users = @oinstall #可登入SAMBA的帳號白名單列表，可直接指定帳號清單，或是以 @開頭表示可使用的群組，指定的user一定要是系統上存在的帳號，否則是無效的。如 root, @user 表示允許root帳號與 @user群組中的帳號使用
```

## 新增 samba 的使用者

```
pollochang@pollochang-GL72-7RDX:~$ sudo smbpasswd -a jameschang
New SMB password:
Retype new SMB password:
Failed to add entry for user jameschang.
```

注意：新增的使用者必須是要linux 的現有的使用者

## 重啟 samba 服務

```
pollochang@pollochang-GL72-7RDX:~$ sudo service smbd restart
```

## 防火牆

```bash
sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="10.192.1.0/24" port protocol="tcp" port="137" accept' &&\
sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="10.192.1.0/24" port protocol="tcp" port="138" accept' &&\
sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="10.192.1.0/24" port protocol="tcp" port="139" accept' &&\
sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="10.192.1.0/24" port protocol="tcp" port="445" accept'
```