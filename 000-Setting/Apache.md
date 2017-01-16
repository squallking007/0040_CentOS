### Apache Install and Setting
***
- #### 安装Apache

1 确认Apache
``` sh
# yum list installed | grep httpd
```

2 安装Apache
``` sh
# yum install httpd
```

3 确认安装
``` sh
# yum list installed | grep httpd
```
4 设置Apache服务的启动级别
``` sh
# chkconfig --levels 235 httpd on
```
5 启动Apache
``` sh
# systemctl start httpd
```
6 验证Apache
``` sh
http://ipAddress:80
```
7 重新启动Apache
``` sh
# systemctl restart httpd
```
8 停止Apache
``` sh
# systemctl stop httpd
```
9 安装目录  
Apache默认的安装目录
```
/var/www/html 目录  
```
默认的主配置文件是
```
/etc/httpd/conf/httpd.conf
```
默认欢迎页面
```
/etc/httpd/conf.d/
```

* 设置Apache服务的启动级别  
> Lelvel 指等级 235是分开的2、3、5 这三个等级，  
> 系统启动有6个等级  
> 等级0表示：表示关机  
> 等级1表示：单用户模式  
> 等级2表示：无网络连接的多用户命令行模式  
> 等级3表示：有网络连接的多用户命令行模式  
> 等级4表示：不可用  
> 等级5表示：带图形界面的多用户模式  
> 等级6表示：重新启动  
