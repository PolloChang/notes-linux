# 002-LDAP_安裝_CentOS7

```sh
sudo yum -y openldap compat-openldap openldap-clients openldap-servers openldap-servers-sql openldap-devel net-tools
sudo systemctl start slapd
sudo netstat -tunlp | grep 389
```

```sh
# slappasswd -h {SSHA} -s nchc1234
{SSHA}z2S8NXYqHGKeFaD9CNiGFKqElkYFmzk2
```

* /etc/openldap/slapd.d

## 新增DB

* /home/jameschang/ldap/db.ldif

```conf
dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=example,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=ldapadm,dc=example,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}dwUZKfM3DiKpau+A4aacMUSnM6UhJBsb
```

```
sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f /home/jameschang/ldap/
sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f db.ldif
```


## 新增管理員

* /home/jameschang/ldap/monitor.ldif

```conf
dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external, cn=auth" read by dn.base="cn=admin,dc=example,dc=com" read by * none
```

```sh
sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f /home/jameschang/ldap/monitor.ldif
ldapmodify -Y EXTERNAL -H ldapi:/// -f monitor.ldif
```

## 建立憑證

```bash
sudo openssl req -new -x509 -nodes -out /etc/openldap/certs/exampleldapcert.pem -keyout /etc/openldap/certs/exampleldapkey.pem -days 365
sudo chown -R ldap:ldap /etc/openldap/certs/*.pem
```

* /home/jameschang/ldap/certs.ldif

```conf
dn: cn=config
changetype: modify
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/openldap/certs/exampleldapkey.pem

dn: cn=config
changetype: modify
replace: olcTLSCertificateFile
olcTLSCertificateFile: /etc/openldap/certs/exampleldapcert.pem
```

* 設定憑證

```
sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f /home/jameschang/ldap/certs.ldif
```

* 驗證

```bash
[jameschang@localhost ldap]$ sudo slaptest -u
config file testing succeeded
```

## 設定 DB Confg

```
sudo cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
sudo chown -R ldap:ldap /var/lib/ldap/
```

## 導入 schema

```bash
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
```

## 建立 樹狀目錄結構

/home/jameschang/ldap/base.ldif

```conf
dn: dc=example,dc=com
dc: example
objectClass: top
objectClass: domain

dn: cn=admin ,dc=example,dc=com
objectClass: organizationalRole
cn: admin
description: LDAP Manager

dn: ou=People,dc=example,dc=com
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=example,dc=com
objectClass: organizationalUnit
ou: Group
```

```bash
sudo ldapadd -x -W -D "cn=ldapadm,dc=example,dc=com" -f /home/jameschang/ldap/base.ldif
```

## 新增使用者


```bash
ldapadd -x -W -D "cn=ldapadm,dc=example,dc=com" -f /home/jameschang/ldap/ldap.bak
```

## 查詢

```bash
ldapsearch -x -b "dc=example,dc=com" | grep dn
```