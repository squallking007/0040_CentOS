### JDK Install and Setting
***
#### 安装JDK
1 确认安装的JDK
``` sh
# yum list installed | grep jdk
```
2 安装wget
``` sh
# yum -y install wget
```
3 下载JDK
``` sh
# mkdir /root/tools/
# cd /root/tools
# wget --no-cookies --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u102-b14/jdk-8u102-linux-x64.rpm -O jdk-8u102-linux-x64.rpm
```
4 安装JDK
``` sh
# rpm -ivh /root/tools/jdk-8u102-linux-x64.rpm
# //参数 ivh 是 Install Verbose Hash, 即显示安装进度
```
5 确认安装的JDK
``` sh
# yum list installed | grep jdk
```
6 设置环境变量  
6.1 确认安装路径
``` sh
# ll /usr/java/
```
6.2 备份配置文件
``` sh
# cp /etc/profile /etc/profile.bak
```
6.3 修改配置文件
``` sh
# vi /etc/profile
# //开始部分加入下列代码
export JAVA_HOME=/usr/java/jdk1.8.0_102/
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin  
```
6.4 刷新配置文件
``` sh
# source /etc/profile
```
6.4 确认环境变量配置是否成功
``` sh
# javac -version
```
