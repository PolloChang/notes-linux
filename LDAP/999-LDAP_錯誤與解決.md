# 我的linux日常生活-LDAP篇-LDAP_錯誤與解決

###### tags: `我的linux日常生活` `LDAP`

## ldap_bind: Invalid credentials (49)

1. 可能忘記管理員密碼了
2. 請不要用鍵盤的9功能數字鍵

## additional info: no global superior knowledge

```bash
jameschang@debian:~$ ldapadd -x -D cn=admin,dc=nodomain -W -f basedn.ldif
Enter LDAP Password: 
adding new entry "ou=people,dc=computingforgeeks,dc=com"
ldap_add: Server is unwilling to perform (53)
        additional info: no global superior knowledge
```

## 忘記管理密碼


* debian

```bash
/etc/ldap/slapd.d
sudo slappasswd
```