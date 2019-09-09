## jdk安装配置

1.上传jdk包

~~~

~~~

2.检查是否安装了jdk

~~~
[pw@hadoop010 software]$ rpm -qa|grep java
如果安装完了，就要卸载jdk
sudo rpm -e 软件包

~~~



3.解压jdk包

~~~
[pw@hadoop010 software]$ tar -zxvf jdk-8u144-linux-x64.tar.gz -C /opt/module/
~~~

4.配置jdk环境

~~~
sudo vim /etc/profile
配置环境信息：
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin
刷新配置文件：
[pw@hadoop010 software]$ source /etc/profile
~~~

5.测试java安装是否成功

~~~
[pw@hadoop010 software]$ java -version
~~~

