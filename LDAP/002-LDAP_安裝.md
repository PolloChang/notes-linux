# 我的linux日常生活-LDAP篇-LDAP_安裝

###### tags: `我的linux日常生活` `LDAP`

## 安裝 slapd

可以想像 slapd 是 資料庫

### CemtOS

#### 安裝

```bash
yum install openldap compat-openldap openldap-clients openldap-servers openldap-servers-sql openldap-devel
```

#### 啟動服務

```bash
systemctl start slapd && systemctl enable slapd
```

#### 檢查有正常服務

```bash
netstat -tunlp | grep 389
```

### Debian

```bash
sudo apt install slapd
```

```bash
~$ sudo apt install slapd
[sudo] password for jameschang: 
Sorry, try again.
[sudo] password for jameschang: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following package was automatically installed and is no longer required:
  libfwupdplugin1
Use 'sudo apt autoremove' to remove it.
The following additional packages will be installed:
  libodbc1
Suggested packages:
  libmyodbc odbc-postgresql tdsodbc unixodbc-bin ldap-utils libsasl2-modules-gssapi-mit | libsasl2-modules-gssapi-heimdal
The following NEW packages will be installed:
  libodbc1 slapd
0 upgraded, 2 newly installed, 0 to remove and 0 not upgraded.
Need to get 1,586 kB of archives.
After this operation, 16.9 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://tw.archive.ubuntu.com/ubuntu focal/main amd64 libodbc1 amd64 2.3.6-0.1build1 [189 kB]
Get:2 http://tw.archive.ubuntu.com/ubuntu focal-updates/main amd64 slapd amd64 2.4.49+dfsg-2ubuntu1.8 [1,397 kB]
Fetched 1,586 kB in 11s (143 kB/s)  
Preconfiguring packages ...
Selecting previously unselected package libodbc1:amd64.
(Reading database ... 72030 files and directories currently installed.)
Preparing to unpack .../libodbc1_2.3.6-0.1build1_amd64.deb ...
Unpacking libodbc1:amd64 (2.3.6-0.1build1) ...
Selecting previously unselected package slapd.
Preparing to unpack .../slapd_2.4.49+dfsg-2ubuntu1.8_amd64.deb ...
Unpacking slapd (2.4.49+dfsg-2ubuntu1.8) ...
Setting up libodbc1:amd64 (2.3.6-0.1build1) ...
Setting up slapd (2.4.49+dfsg-2ubuntu1.8) ...
  Creating new user openldap... done.
  Creating initial configuration... done.
  Creating LDAP directory... done.
Processing triggers for ufw (0.36-6ubuntu1) ...
Processing triggers for systemd (245.4-4ubuntu3.15) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for libc-bin (2.31-0ubuntu9.7) ...
```

安裝過程或出現要輸入管理者密碼，類似這個畫面

```bash
 +--------------------------| Configuring slapd |-------------------------+
 | Please enter the password for the admin entry in your LDAP directory.  |
 |                                                                        |
 | Administrator password:                                                |
 |                                                                        |
 | ********______________________________________________________________ |
 |                                                                        |
 |                                 <Ok>                                   |
 |                                                                        |
 +------------------------------------------------------------------------+
```

## 檢查 LDAP 有啟動服務

如果有看到TCP有在監聽連接埠389，就表示slapd安裝成功了！

```bash
jameschang@ldap1:~$ sudo netstat -tulnp | grep slapd
tcp        0      0 0.0.0.0:389             0.0.0.0:*               LISTEN      20077/slapd         
tcp6       0      0 :::389                  :::*                    LISTEN      20077/slapd 
```

## 重新產生slapd的設定檔

```bash
jameschang@ldap1:~$ sudo dpkg-reconfigure slapd
Backing up /etc/ldap/slapd.d in /var/backups/slapd-2.4.49+dfsg-2ubuntu1.8... done.
Moving old database directory to /var/backups:
- directory unknown... done.
Creating initial configuration... done.
Creating LDAP directory... done.
```

1. 忽略OpenLDAP伺服器的配置? N

```bash
┌───────────────────────────────────┤ Configuring slapd ├───────────────────────────────────┐
│                                                                                           │ 
│ If you enable this option, no initial configuration or database will be created for you.  │ 
│                                                                                           │ 
│ Omit OpenLDAP server configuration?                                                       │ 
│                                                                                           │ 
│                          <Yes>                             <No>                           │ 
│                                                                                           │ 
└───────────────────────────────────────────────────────────────────────────────────────────┘ 
```

2. 設定基本的DN(Base DN)。

```bash
┌────────────────────────────────────────────────────┤ Configuring slapd ├─────────────────────────────────────────────────────┐
│ The DNS domain name is used to construct the base DN of the LDAP directory. For example, 'foo.example.org' will create the   │ 
│ directory with 'dc=foo, dc=example, dc=org' as base DN.                                                                      │ 
│                                                                                                                              │ 
│ DNS domain name:                                                                                                             │ 
│                                                                                                                              │ 
│ pollochang.org______ │ 
│                                                                                                                              │ 
│                                                            <Ok>                                                              │ 
│                                                                                                                              │ 
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘ 
```

3. 織名稱。這個雖然是在設定O欄位，但它不會被當成dn的一部份，純粹記錄用。

```bash
┌──────────────────────────────────┤ Configuring slapd ├───────────────────────────────────┐
│ Please enter the name of the organization to use in the base DN of your LDAP directory.  │ 
│                                                                                          │ 
│ Organization name:                                                                       │ 
│                                                                                          │ 
│ pollodict_______________________________________________________________________________ │ 
│                                                                                          │ 
│                                          <Ok>                                            │ 
│                                                                                          │ 
└──────────────────────────────────────────────────────────────────────────────────────────┘ 
```

4. 管理員的密碼。

```bash
┌─────────────────────────┤ Configuring slapd ├──────────────────────────┐
│ Please enter the password for the admin entry in your LDAP directory.  │ 
│                                                                        │ 
│ Administrator password:                                                │ 
│                                                                        │ 
│ ______________________________________________________________________ │ 
│                                                                        │ 
│                                 <Ok>                                   │ 
│                                                                        │ 
└────────────────────────────────────────────────────────────────────────┘
```

5. 再次輸入密碼

```bash
┌───────────────────────────────────────────┤ Configuring slapd ├────────────────────────────────────────────┐
│ Please enter the admin password for your LDAP directory again to verify that you have typed it correctly.  │ 
│                                                                                                            │ 
│ Confirm password:                                                                                          │ 
│                                                                                                            │ 
│ __________________________________________________________________________________________________________ │ 
│                                                                                                            │ 
│                                                   <Ok>                                                     │ 
│                                                                                                            │ 
└────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

6. 決定LDAP資料庫是否要在乾淨移除(purge)slapd時也跟著被移除。

```bash
┌─────────────────────┤ Configuring slapd ├─────────────────────┐
│                                                               │ 
│                                                               │ 
│                                                               │ 
│ Do you want the database to be removed when slapd is purged?  │ 
│                                                               │ 
│                <Yes>                   <No>                   │ 
│                                                               │ 
└───────────────────────────────────────────────────────────────┘
```

7. 再來決定是否要備份舊的LDAP資料庫，備份檔會被放在/var/backups/目錄中。

```bash
┌────────────────────────────────────────────────────┤ Configuring slapd ├─────────────────────────────────────────────────────┐
│                                                                                                                              │ 
│ There are still files in /var/lib/ldap which will probably break the configuration process. If you enable this option, the   │ 
│ maintainer scripts will move the old database files out of the way before creating a new database.                           │ 
│                                                                                                                              │ 
│ Move old database?                                                                                                           │ 
│                                                                                                                              │ 
│                                     <Yes>                                        <No>                                        │ 
│                                                                                                                              │ 
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘ 
```

## 安裝 ldap-utils

可以想像 ldap-utils 是資料庫操作工具如：SQL

```bash
jameschang@ldap1:~$ sudo apt install ldap-utils
[sudo] password for jameschang: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  libsasl2-modules-gssapi-mit | libsasl2-modules-gssapi-heimdal
The following NEW packages will be installed:
  ldap-utils
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 122 kB of archives.
After this operation, 745 kB of additional disk space will be used.
Get:1 http://tw.archive.ubuntu.com/ubuntu focal-updates/main amd64 ldap-utils amd64 2.4.49+dfsg-2ubuntu1.8 [122 kB]
Fetched 122 kB in 10s (11.7 kB/s)       
Selecting previously unselected package ldap-utils.
(Reading database ... 72365 files and directories currently installed.)
Preparing to unpack .../ldap-utils_2.4.49+dfsg-2ubuntu1.8_amd64.deb ...
Unpacking ldap-utils (2.4.49+dfsg-2ubuntu1.8) ...
Setting up ldap-utils (2.4.49+dfsg-2ubuntu1.8) ...
Processing triggers for man-db (2.9.1-1) ...
```

## linux 驗證

Debian 系列一直無法驗證成功，原因待查

### CentOS-LDAP-Server

### CentOS-Client

```bash
yum install -y openldap-clients nss-pam-ldapd
authconfig --enableldap --enableldapauth --ldapserver=192.168.56.229 --ldapbasedn="dc=example=org,dc=tw" --enablemkhomedir --update
systemctl restart nslcd
```

cat pollo.ldif 

```bash
dn: uid=pollo,ou=People,dc=example=org,dc=tw
objectClass: top
objectClass: account
objectClass: posixAccount
objectClass: shadowAccount
cn: pollo
uid: pollo
uidNumber: 2003
gidNumber: 2003
homeDirectory: /home/pollo
loginShell: /bin/bash
gecos: pollo
userPassword: {crypt}x
shadowLastChange: 17058
shadowMin: 0
shadowMax: 99999
shadowWarning: 7
```

```bash
ldapadd -x -W -D "cn=ldapadm,dc=example=org,dc=tw" -f pollo.ldif
ldappasswd -s 4321 -W -D "cn=ldapadm,dc=example=org,dc=tw" -x "uid=pollo,ou=People,dc=example=org,dc=tw"
```

```bash
getent passwd
```