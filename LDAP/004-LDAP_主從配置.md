# 我的linux日常生活-LDAP篇-LDAP_主從配置

###### tags: `我的linux日常生活` `LDAP`

## 主機配置

下列為規劃的主機配置

```
IP Address	    OS	        Node
192.168.56.229	CentOS 7	LDAP-Master v
192.168.56.236	CentOS 7	LDAP-Slave v
192.168.56.166	Debian 11	LDAP-Client
```

## Master 配置

rpuser.ldif

```conf
dn: uid=rpuser,dc=example=org,dc=tw
objectClass: simpleSecurityObject
objectClass: account
uid: rpuser
description: Replication User
userPassword: rpuser123
```

```bash
[root@localhost slapd.d]# ldapadd -x -W -D "cn=ldapadm,dc=example=org,dc=tw" -f rpuser.ldif
Enter LDAP Password: 
adding new entry "uid=rpuser,dc=example=org,dc=tw"
```

* 啟用 syncprov
  
syncprov_mod.ldif

```bash
[root@localhost slapd.d]# ldapadd -Y EXTERNAL -H ldapi:/// -f syncprov_mod.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
adding new entry "cn=module,cn=config"
```

* 將每個⽬錄都啟⽤ syncprov

vim syncprov.ldif

```conf
dn: olcOverlay=syncprov,olcDatabase={2}hdb,cn=config
objectClass: olcOverlayConfig
objectClass: olcSyncProvConfig
olcOverlay: syncprov
olcSpSessionLog: 100
```

```bash
[root@localhost slapd.d]# ldapadd -Y EXTERNAL -H ldapi:/// -f syncprov.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
adding new entry "olcOverlay=syncprov,olcDatabase={2}hdb,cn=config"
```

## Slave 配置

* 現在，我們將配置 Slave，將最重要的配置（如LDAP服務器URI，LDAP⽤⼾和密碼）填⼊每個從屬節點的⽂件中。

vim rp.ldif

```conf
dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcSyncRepl
olcSyncRepl: rid=001
    provider=ldap://192.168.56.229:389/
    bindmethod=simple
    binddn="uid=rpuser,dc=example=org,dc=tw"
    credentials=zxcv
    searchbase="dc=example=org,dc=tw"
    scope=sub
    schemachecking=on
    type=refreshAndPersist
    retry="30 5 300 3"
    interval=00:00:05:00
```

```bash
ldapmodify -Y EXTERNAL -H ldapi:/// -f rp.ldif
```