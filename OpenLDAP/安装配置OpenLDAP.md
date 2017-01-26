## CentOS7 安装配置 OpenLDAP

### 1. 确认OpenLDAP的安装情况
```sh
# rpm -qa | grep openldap
```
    openldap-2.4.40-13.el7.x86_64

### 2. 安装OpenLDAP服务器及客户端
```sh
# yum install openldap openldap-servers openldap-clients
```

### 3. 确认安装的OpenLDAP服务器及客户端
```sh
# rpm -qa | grep openldap
```
    openldap-servers-2.4.40-13.el7.x86_64
    openldap-2.4.40-13.el7.x86_64
    openldap-clients-2.4.40-13.el7.x86_64

### 4. 复制slapd.conf
###### ※CentOs7下没有[slapd.conf](./slapd.conf.obsolete)文件，需要自行下载。
``` sh
# cp slapd.conf.obsolete /etc/openldap/slapd.conf
# vi /etc/sysconfig/slapd

# Any custom options
# SLAPD_OPTIONS=""
SLAPD_OPTIONS="-f /etc/openldap/slapd.conf -s 512"  # 添加
```

### 5. DB_CONFIG的设定
```sh
# 复制DB_CONFIGファイル文件
# cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
```

### 6. 修改系统日志配置文件
```sh
# cp /etc/rsyslog.conf /etc/rsyslog.conf.bak
# vi /etc/rsyslog.conf

# 修改前
*.info;mail.none;authpriv.none;cron.none                /var/log/messages

# 修改后
*.info;mail.none;authpriv.none;cron.none;share4.none    /var/log/messages
share4.*                                                /var/log/messages

# 保存后重启服务
# systemctl restart  rsyslog.service
```
### 7. OpenLDAPの基本设定
```sh
# slappasswd
New password:[输入密码]
Re-enter new password:[再次输入密码]
{SSHA}+fsjhjnMSUBAaJc1Am3BnDG4C1DvMmlR    #加密后的密码，需要保存

# cp /etc/openldap/slapd.conf /etc/openldap/slapd.conf.bak
# vi /etc/openldap/slapd.conf
# 修改前
database        bdb
suffix          "dc=my-domain,dc=com"
checkpoint      1024 15
rootdn          "cn=Manager,dc=my-domain,dc=com"

# 修改后
database        bdb
suffix          "dc=openldap,dc=com"
checkpoint      1024 15
rootdn          "cn=Manager,dc=openldap,dc=com"
rootpw          {SSHA}+fsjhjnMSUBAaJc1Am3BnDG4C1DvMmlR

# 保存退出后进行确认
# slaptest -u -f /etc/openldap/slapd.conf -v
config file testing succeeded
```

### 8. 启动slapd
```sh
# systemctl start  slapd.service

# 查看slapd状态
# systemctl status slapd.service
```

### 9. 管理员导入
```sh
# vi init.ldif

dn: dc=openldap,dc=com
objectclass: dcObject
objectclass: organization
o: VIRTUAL CORPORATION
dc: openldap

dn: cn=Manager,dc=openldap,dc=com
objectclass: organizationalRole
cn: Manager

# 保存退出后，导入管理员信息
# ldapadd -x -W -D "cn=Manager,dc=openldap,dc=com" -f init.ldif
Enter LDAP Password: [输入密码]
adding new entry "dc=openldap,dc=com"

adding new entry "cn=Manager,dc=openldap,dc=com"
```

### 10. 检索用户导入
```sh
# 生成密码
# slappasswd -h '{CRYPT}'
New password: [输入密码]
Re-enter new password: [再次输入密码]
{CRYPT}ami7gG.nkaI26

# vi search.ldif
dn: cn=search001,dc=openldap,dc=com
objectclass: organizationalRole
objectclass: simpleSecurityObject
cn: search001
userPassword: {CRYPT}ami7gG.nkaI26

dn: cn=search002,dc=openldap,dc=com
objectclass: organizationalRole
objectclass: simpleSecurityObject
cn: search002
userPassword: {CRYPT}ami7gG.nkaI26

dn: cn=search003,dc=openldap,dc=com
objectclass: organizationalRole
objectclass: simpleSecurityObject
cn: search003
userPassword: {CRYPT}ami7gG.nkaI26

# 保存退出后，导入检索用户
# ldapadd -x -W -D "cn=Manager,dc=openldap,dc=com" -f search.ldif
Enter LDAP Password: [输入密码]
adding new entry "cn=search001,dc=openldap,dc=com"

adding new entry "cn=search002,dc=openldap,dc=com"

adding new entry "cn=search003,dc=openldap,dc=com"
```

### 11. OU ( Organization Unit )导入
```sh
# vi ou.ldif
dn: ou=Users,dc=openldap,dc=com
objectclass: organizationalUnit
ou: Users

dn: ou=Groups,dc=openldap,dc=com
objectclass: organizationalUnit
ou: Groups

dn: ou=Computers,dc=openldap,dc=com
objectclass: organizationalUnit
ou: Computers

# 保存退出后，导入OU信息
# ldapadd -x -W -D "cn=Manager,dc=openldap,dc=com" -f ou.ldif
Enter LDAP Password: [输入密码]
adding new entry "ou=Users,dc=openldap,dc=com"

adding new entry "ou=Groups,dc=openldap,dc=com"

adding new entry "ou=Computers,dc=openldap,dc=com"
```

### 12. Group导入
```sh
# vi groups.ldif
dn: cn=develop,ou=Groups,dc=openldap,dc=com
objectClass: posixGroup
cn: develop
gidNumber: 1000

dn: cn=sales,ou=Groups,dc=openldap,dc=com
objectClass: posixGroup
cn: sales
gidNumber: 1001

dn: cn=market,ou=Groups,dc=openldap,dc=com
objectClass: posixGroup
cn: market
gidNumber: 1002

# 保存退出后，导入Group信息
# ldapadd -x -W -D "cn=Manager,dc=openldap,dc=com" -f groups.ldif
Enter LDAP Password: [输入密码]
adding new entry "cn=develop,ou=Groups,dc=openldap,dc=com"

adding new entry "cn=sales,ou=Groups,dc=openldap,dc=com"

adding new entry "cn=market,ou=Groups,dc=openldap,dc=com"
```

### 13. 用户信息导入
```sh
# 生成密码
# slappasswd -h '{CRYPT}'
New password: [输入密码]
Re-enter new password: [再次输入密码]
{CRYPT}3vtK/jKg7hb0s

# vi useradd.ldif
dn: uid=takeda,ou=Users,dc=openldap,dc=com
objectclass: posixAccount
objectclass: inetOrgPerson
sn: takeda
cn: kazuma
displayName: Takeda Kazuma
uid: takeda
uidNumber: 1000
gidNumber: 1000
homeDirectory: /home/takeda
loginShell: /bin/bash
userPassword: {CRYPT}3vtK/jKg7hb0s
mail: takeda@openldap.net

dn: uid=suzuki,ou=Users,dc=openldap,dc=com
objectclass: posixAccount
objectclass: inetOrgPerson
sn: suzuki
cn: hajime
displayName: Suzuki Hajime
uid: suzuki
uidNumber: 1001
gidNumber: 1000
homeDirectory: /home/suzuki
loginShell: /bin/bash
userPassword: {CRYPT}3vtK/jKg7hb0s
mail: suzuki@openldap.net

dn: uid=tanaka,ou=Users,dc=openldap,dc=com
objectclass: posixAccount
objectclass: inetOrgPerson
sn: tanaka
cn: takuya
displayName: Tanaka Takuya
uid: tanaka
uidNumber: 1002
gidNumber: 1000
homeDirectory: /home/tanaka
loginShell: /bin/bash
userPassword: {CRYPT}3vtK/jKg7hb0s
mail: tanaka@openldap.net

# 保存退出后，导入用户信息
# ldapadd -x -W -D "cn=Manager,dc=openldap,dc=com" -f useradd.ldif
Enter LDAP Password: [输入密码]
adding new entry "uid=takeda,ou=Users,dc=openldap,dc=com"

adding new entry "uid=suzuki,ou=Users,dc=openldap,dc=com"

adding new entry "uid=tanaka,ou=Users,dc=openldap,dc=com"

# 设置目录权限
# chown -R ldap:ldap /var/lib/ldap/
# chown -R ldap:ldap /etc/openldap/
```

### 14. phpldapadmin设置
```sh
# 安装Apache
# yum -y install httpd

# 安装php
# yum -y install php

# 下载phpldapadmin(先安装wget)
# yum -y install wget
# yum -y install php-ldap
# wget http://sourceforge.net/projects/phpldapadmin/files/phpldapadmin-php5/1.2.3/phpldapadmin-1.2.3.tgz
# tar xzvf phpldapadmin-1.2.3.tgz
# mv phpldapadmin-1.2.3 phpldapadmin
# cp phpldapadmin/config/config.php.example phpldapadmin/config/config.php
# cp -rp phpldapadmin /usr/local/

# ln -s /usr/local/phpldapadmin/ /var/www/html/phpldapadmin
# ln -s /usr/local/phpldapadmin/ /var/www/html/ldapadmin

# 修改phpldapadmin的配置文件config.php
# vi /usr/local/phpldapadmin/config/config.php

# 修改前
$servers->setValue('server','name','My LDAP Server');
// $servers->setValue('server','host','127.0.0.1');
// $servers->setValue('server','port',389);
// $servers->setValue('server','base',array(''));
// $servers->setValue('login','auth_type','session');
// $servers->setValue('login','bind_id','');
// $servers->setValue('server','tls',false);
// $servers->setValue('login','attr','dn');

# 修改后
$servers->setValue('server','name','OpenLDAP Server');
$servers->setValue('server','host','127.0.0.1');
$servers->setValue('server','port',389);
$servers->setValue('server','base',array('dc=openldap,dc=com'));
$servers->setValue('login','auth_type','session');
$servers->setValue('login','bind_id','cn=Manager,dc=openldap,dc=com');
$servers->setValue('server','tls',false);
$servers->setValue('login','attr','dn');

# 保存退出

# 修改Apache的配置文件
# cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.bak
# vi /etc/httpd/conf/httpd.conf

# 追加前
#ServerName www.example.com:80

# 追加后
#ServerName www.example.com:80
Alias /phpldapadmin /usr/local/phpldapadmin/htdocs
Alias /ldapadmin /usr/local/phpldapadmin/htdocs

<Directory /usr/local/phpldapadmin/htdocs>
    AllowOverride all
    Require all granted
</Directory>

# 保存退出

# 保存退出后重启Apache
# systemctl restart httpd

# 输入URL确认phpldapadmin
http://ipAddress/ldapadmin/
```
