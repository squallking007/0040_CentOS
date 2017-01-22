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
###### ※CentOs7下没有[slapd.conf]文件，需要自行下载。
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
未完待续。。。。。。
