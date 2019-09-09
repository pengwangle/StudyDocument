## 安装hadoop环境

1.上传hadoop安装包

~~~

~~~

2.解压hadoop文件

~~~
[pw@hadoop010 software]$ tar -zxvf hadoop-2.7.2.tar.gz -C /opt/module/

~~~

3.配置hadoop环境

~~~
[pw@hadoop010 software]$ sudo vim /etc/profile
##HADOOP_HOME
export HADOOP_HOME=/opt/module/hadoop-2.7.2
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

刷新文件：
[pw@hadoop010 software]$ source /etc/profile
~~~

4.测试安装的hadoop

~~~
hadoop version
~~~

