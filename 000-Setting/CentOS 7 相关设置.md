### CentOS 7 相关设置
***

#### 新建，删除用户

1 创建用户
``` sh
# useradd -m user_name
```
参数m是在 `/home`下创建同名目录。

2 设置密码
``` sh
# passwd user_name
```

3 删除用户
``` sh
# userdel -r user_name
```
参数r是在删除该用户的同时删除该用户在`/home`下的同名目录。

#### IP地址设置

1 确认网络配置
``` sh
# ip addr
```

2 备份配置文件
``` sh
# cp -p /etc/sysconfig/network-scripts/ifcfg-ens33 /etc/sysconfig/network-scripts/ifcfg-ens33.bak
```

3 修改配置文件
``` sh
# vi /etc/sysconfig/network-scripts/ifcfg-ens33
```
将<font color="blue">ONBOOT</font>行的<font color="red">no</font>修改为<font color="red">yes</font>
> ONBOOT=yes

4 重启网络服务或者重启系统
``` sh
# 重启网络服务
# systemctl restart nework.service
```

``` sh
# 重启系统
# shutdown -r now
```

5 重新查看网络配置
``` sh
# ip addr
```

#### 允许root用户通过ssh连接

1 备份配置文件
``` sh
# cp -p /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

2 修改配置文件
``` sh
# vi /etc/ssh/sshd_config

# 修改前
# PermitRootLogin no

# 修改后
# PermitRootLogin yes
```

3 重启sshd服务
``` sh
# systemctl restart sshd.service
```


