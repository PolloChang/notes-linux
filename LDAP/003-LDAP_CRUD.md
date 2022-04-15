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