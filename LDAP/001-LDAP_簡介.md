# 我的linux日常生活-LDAP篇-LDAP 簡介

###### tags: `我的linux日常生活` `LDAP`

輕型目錄存取協定

## 文件配置

* 目錄: /etc/ldap/slapd.d

* 預設文件

`cn=config`：這個是slapd在執行時會產生並持續維護的設定檔目錄，裡面也會存放多個.ldif設定檔。

`cn=config.ldif`這個檔案表示用來管理整個LDAP伺服器的物件，算是權限等級最高的物件，這個物件又稱為OLC。

### /etc/ldap/slapd.d/"cn=config.ldif" 預設文件內容

```conf
# AUTO-GENERATED FILE - DO NOT EDIT!! Use ldapmodify.
# CRC32 99036c02
dn: cn=config
objectClass: olcGlobal
cn: config
olcArgsFile: /var/run/slapd/slapd.args
olcLogLevel: none
olcPidFile: /var/run/slapd/slapd.pid
olcToolThreads: 1
structuralObjectClass: olcGlobal
entryUUID: 58f3eba0-3f01-103c-83ca-5500624a39a3
creatorsName: cn=config
createTimestamp: 20220323142909Z
entryCSN: 20220323142909.753689Z#000000#000#000000
modifiersName: cn=config
modifyTimestamp: 20220323142909Z
```

* 資料格式: 欄位名稱:欄位值

### 欄位意含

* dn

識別名稱, Distinguish Name

* objectClass

用來表示指定這個物件所代表的意義，以及它必須要有的欄位，或是可能會有的欄位。可以使用多個objectClass欄位，來指定該物件屬於多個類別，進而擴充其可以使用的欄位。

### RDN和DN

根據目錄服務的定義(X.500)，常用的目錄節點(node)的屬性類型(AttributeType)如下：

* CN(Common Name)：通用名稱。
* L(Locality Name)：地方名稱。
* ST(State Or Province Name)：洲、省名稱。
* O(Organization Name)：組織名稱。
* OU(Organization Unit Name)：組織單位名稱。
* C(Country Name)：國家名稱。
* STREET(Street Name)：街道名稱。
* DC(Domain Component)：網域部件。如google.com，有com和google兩個部件。
* UID(User ID)：使用者的ID。

使用者

網域中的使用者，會顯示在所屬機構單位下方：

```
objectClass：top、person、organizationalPerson、inetOrgPerson、posixAccount
uid：使用者名稱，也就是使用者電子郵件地址的使用者名稱部分。
googleUid：與 uid 相同，這個屬性是為了與 posixUid 明確區分而設。
posixUid：使用者名稱，或是其 POSIX 使用者名稱 (如已設定)。
cn：「常用名稱」的英文縮寫。其中包含兩個值：使用者名稱和使用者的顯示名稱。
sn：使用者的姓氏。
givenName：使用者的名字。
displayName：使用者的顯示名稱 (全名)。
mail：使用者的電子郵件地址。
memberOf：使用者所屬群組的完整名稱清單。
title：使用者職稱。
employeeNumber：使用者的員工 ID。
employeeType：使用者在機構中的角色。
departmentNumber：使用者所屬部門名稱 (不一定是號碼)。
physicalDeliveryOfficeName：使用者所在地區或住址。
jpegPhoto：使用者的個人資料相片。
entryUuid：使用者專屬的固定通用 ID。
objectSid：使用者專屬的固定通用 ID，與 Windows 安全性 ID 相容。
uidNumber：使用者的 POSIX UID 號碼。如果使用者的 POSIX ID 已設定完畢，這裡就會顯示該 ID；如果沒有，則會顯示使用者的專屬固定 ID。
gidNumber：使用者主要群組的 POSIX GID 號碼。如果使用者的 POSIX GID 已設定完畢，這裡就會顯示該 ID；如果沒有，則會顯示使用者的 UID 號碼。
```

注意：除非管理員已使用 Admin SDK API 設定使用者的 posixAccounts 屬性，否則您無法利用「uidNumber」或「gidNumber」搜尋使用者。

```
homeDirectory：使用者的 POSIX 主目錄，預設為「/home/<使用者名稱>」。
loginShell：使用者的 POSIX 登入殼層，預設為「/bin/bash」。
gecos：使用者的 GECOS (歷來) 屬性。
hasSubordinates：FALSE
​群組
objectClass：top、groupOfNames、posixGroup
cn：群組的網域專屬名稱。
displayName：使用者可理解的群組顯示名稱。
description：使用者可理解的群組詳細說明。
gidNumber：群組的 POSIX GID 號碼。這是固定的唯一識別碼，但您無法透過這組號碼快速找出群組。
entryUuid：群組專屬的固定通用 ID。
objectSid：群組專屬的固定通用 ID，與 Windows 安全性 ID 相容。
member：群組成員的完整名稱清單。
memberUid：群組成員的使用者名稱清單。
googleAdminCreated：如果群組是由管理員建立，則這裡的值為 True。
hasSubordinates：FALSE
```

## slapcat

可以查看目前slapd資料庫中的資料

```bash
jameschang@ldap1:~$ sudo slapcat
dn: dc=pollochang,dc=org
objectClass: top
objectClass: dcObject
objectClass: organization
o: pollodict
dc: pollochang
structuralObjectClass: organization
entryUUID: 401b304e-3f05-103c-8d86-b7bbf61b4c36
creatorsName: cn=admin,dc=pollochang,dc=org
createTimestamp: 20220323145706Z
entryCSN: 20220323145706.054898Z#000000#000#000000
modifiersName: cn=admin,dc=pollochang,dc=org
modifyTimestamp: 20220323145706Z

dn: cn=admin,dc=pollochang,dc=org
objectClass: simpleSecurityObject
objectClass: organizationalRole
cn: admin
description: LDAP administrator
userPassword:: e1NTSEF9Z0Vmc2QrTmpqakc1NEk3TnBiWlo2QTQ0d3plTFdmL2M=
structuralObjectClass: organizationalRole
entryUUID: 401ce1d2-3f05-103c-8d87-b7bbf61b4c36
creatorsName: cn=admin,dc=pollochang,dc=org
createTimestamp: 20220323145706Z
entryCSN: 20220323145706.066031Z#000000#000#000000
modifiersName: cn=admin,dc=pollochang,dc=org
modifyTimestamp: 20220323145706Z
```