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
    compat-openldap-2.3.43-5.el7.x86_64
    openldap-2.4.40-13.el7.x86_64
    openldap-devel-2.4.40-13.el7.x86_64
    openldap-clients-2.4.40-13.el7.x86_64



未完待续。。。。。。
