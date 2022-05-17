# 我的linux日常生活-LDAP篇-LDAP_CRUD

###### tags: `我的linux日常生活` `LDAP`

## 新增


* 需要先新增「資料庫」再新增「群組」最後是「使用者」

```
dn: cn=Barbara J Jensen,dc=example,dc=com
cn: Barbara J Jensen
cn: Babs Jensen
objectclass: person
description:< file:///tmp/babs
sn: Jensen

dn: cn=Bjorn J Jensen,dc=example,dc=com
cn: Bjorn J Jensen
cn: Bjorn Jensen
objectclass: person
sn: Jensen

```

```bash
ldapadd -h 192.168.56.103 -W -D "cn=admin,dc=magiclen,dc=org" -f 'cn=Ron,dc=magiclen,dc=org.ldif'

ldapadd -h 192.168.56.103 -W -D "cn=admin,dc=pollochang,dc=org" -f 'cn=JamesChang,dc=pollochang,dc=org.ldif'

ldapadd -h 192.168.56.45 -W -D "dc=dic,dc=ksu" -f 'add.ldif'

ldapadd -h 192.168.56.45 -x -W -D "cn=admin,dc=pollochang,dc=org" -f 'adam.ldif'
```

* `-h` 參數可以指定LDAP伺服器位址。
* `-W` 參數可以指定要使用簡易驗證機制，且密碼是在執行指令後才輸入。如果要直接在指令中輸入密碼，需用-w參數；如果要從檔案中讀取密碼，需用-y參數。
* `-D` 參數可以指定「Bind DN」，也就是管理員的DN。
* `-f` 可以指定一個LDIF檔來匯入。

## 修改

### 準備修改文件

test1.ldif

```conf
dn: uid=test1,ou=People,dc=example=org,dc=tw ### >> 修改目標
changetype: modify ### >> 狀態
replace: gidNumber ### >> 要修改的欄位
gidNumber: 2001 ### >> 修改目標欄位值
```

### 執行修改

```bash
ldapmodify -x -W -D "cn=ldapadm,dc=example=org,dc=tw" -f test1.ldif
```

## 查詢

```bash
[root@localhost slapd.d]# ldapsearch -x dc=example,dc=com,dc=tw
# extended LDIF
#
# LDAPv3
# base <> (default) with scope subtree
# filter: dc=example,dc=com,dc=tw
# requesting: ALL
#

# search result
search: 2
result: 32 No such object

# numResponses: 1
```

```bash
[root@localhost slapd.d]# ldapsearch -x cn=test2 -b dc=example,dc=com,dc=tw
# extended LDIF
#
# LDAPv3
# base <dc=example,dc=com,dc=tw> with scope subtree
# filter: cn=test2
# requesting: ALL
#

# test2, People, example.com.tw
dn: uid=test2,ou=People,dc=example,dc=com,dc=tw
objectClass: top
objectClass: account
objectClass: posixAccount
objectClass: shadowAccount
cn: test2
uid: test2
uidNumber: 2002
gidNumber: 100
homeDirectory: /home/test2
loginShell: /bin/bash
gecos: test2
shadowLastChange: 17058
shadowMin: 0
shadowMax: 99999
shadowWarning: 7
userPassword:: e1NTSEF9UW0wb2d1Y0dtRFJOTW1yQnhXMHA0U3pOZ3U2ME5DRlY=

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

ldapsearch -x -o ldif-wrap=no -H "ldap://10.1.2.5" -b "uid=jameschang,ou=people,dc=example,dc=com,dc=tw" -w ""

ldapsearch -x -o ldif-wrap=no -H "ldap://192.168.56.11" -b "ou=people,dc=mail,dc=werisetek,dc=com" -w "12341234"

ldapsearch -x -o ldif-wrap=no -H "ldap://10.1.2.5" -b "uid=jameschang,ou=people,dc=example,dc=com,dc=tw" -w ""
