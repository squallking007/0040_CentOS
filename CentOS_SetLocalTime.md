#CentOS 设置本地时间
<<<<<<< HEAD
## 第一步：安装NTP
### 确认是否安装了ntp
`#yum list installed | grep ntp`
`#rpm -qa ntp`
### 如果没有安装则安装ntp
`yum install ntp`
### 确认正确安装了ntp
`#yum list installed | grep ntp`
## 第二步：设置本地时区
`#ln -sf /usr/share/zoneinfo/Asia/ToKyo /etc/localtime`
这里把本地时区设置为亚洲的东京
## 第三步：设置硬件时间和系统时间一致并校准
`#/sbin/hwclock --systohc`
## 第四步：重启系统，确认本地时间
`#shutdown -r now`
`#timedatectl`
=======
## 第一步：安装NTP服务器
### 确认是否安装了ntp服务器
`# yum list installed | grep ntp`

### 如果没有安装则安装ntp服务器
`# yum install ntp`

### 确认正确安装了ntp服务器
`# yum list installed | grep ntp`

## 第二部：同步时间
`# ntpdate time.nist.gov`

## 第三：设置本地时区
`# ln -sf /usr/share/zoneinfor/Asia/ToKyo /etc/localtime`

这里把本地时区设置为亚洲的东京

## 第四步：设置硬件时间和系统时间一致并校准
`# /sbin/hwclock --systohc`

## 第四步：重启系统，确认本地时间
`# shutdown -r now`

登录后确认是否设置成功

`# timedatectl`
>>>>>>> origin/master
