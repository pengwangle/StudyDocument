# hadoop编译环境搭建

##  1 .前期准备工作

### CentOS联网 

配置CentOS能连接外网。Linux虚拟机ping [www.baidu.com](http://www.baidu.com) 是畅通的

注意：采用root角色编译，减少文件夹权限出现问题

### jar包准备(hadoop源码、JDK8、maven、ant 、protobuf)

（1）hadoop-2.7.2-src.tar.gz

（2）jdk-8u144-linux-x64.tar.gz

（3）apache-ant-1.9.9-bin.tar.gz（build工具，打包用的）

（4）apache-maven-3.0.5-bin.tar.gz

（5）protobuf-2.5.0.tar.gz（序列化的框架）

## 2 jar包安装

注意：所有操作必须在root用户下完成

JDK解压、配置环境变量 JAVA_HOME和PATH，验证[java](http://lib.csdn.net/base/javase)-version(如下都需要验证是否配置成功)

~~~
[root@hadoop101 software] # tar -zxf jdk-8u144-linux-x64.tar.gz -C /opt/module/
[root@hadoop101 software]# vi /etc/profile

#JAVA_HOME：
export JAVA_HOME=/opt/module/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin

[root@hadoop101 software]#source /etc/profile
验证命令：java -version
~~~

2. Maven解压、配置  MAVEN_HOME和PATH

   ~~~
   [root@hadoop101 software]# tar -zxvf apache-maven-3.0.5-bin.tar.gz -C /opt/module/
   
   [root@hadoop101 apache-maven-3.0.5]# vi conf/settings.xml
   <mirrors>
       <!-- mirror
       | Specifies a repository mirror site to use instead of a given repository. The repository that
       | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
       |
   <mirror>
         <id>mirrorId</id>
         <mirrorOf>repositoryId</mirrorOf>
         <name>Human Readable Name for this Mirror.</name>
         <url>http://my.repository.com/repo/path</url>
         </mirror>
        -->
          <mirror>
                  <id>nexus-aliyun</id>
                   <mirrorOf>central</mirrorOf>
                   <name>Nexus aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
          </mirror>
   </mirrors>
   
   
   
   ~~~

   ~~~
   [root@hadoop101 apache-maven-3.0.5]# vi /etc/profile
   #MAVEN_HOME
   export MAVEN_HOME=/opt/module/apache-maven-3.0.5
   export PATH=$PATH:$MAVEN_HOME/bin
   
   [root@hadoop101 software]#source /etc/profile
   验证命令：mvn -version
   ~~~


3. ant解压、配置  ANT _HOME和PATH

   ~~~
   [root@hadoop101 software]# tar -zxvf apache-ant-1.9.9-bin.tar.gz -C /opt/module/
   [root@hadoop101 apache-ant-1.9.9]# vi /etc/profile
   #ANT_HOME
   export ANT_HOME=/opt/module/apache-ant-1.9.9
   export PATH=$PATH:$ANT_HOME/bin
   
   [root@hadoop101 software]#source /etc/profile
   验证命令：ant -version
   ~~~

4. 安装  glibc-headers 和  g++  命令如下

   ~~~
   [root@hadoop101 apache-ant-1.9.9]# yum install glibc-headers
   [root@hadoop101 apache-ant-1.9.9]# yum install gcc-c++
   ~~~

5.安装make和cmake

~~~
[root@hadoop101 apache-ant-1.9.9]# yum install make
[root@hadoop101 apache-ant-1.9.9]# yum install cmake
~~~

6.解压protobuf ，进入到解压后protobuf主目录，/opt/module/protobuf-2.5.0，然后相继执行命令

~~~
[root@hadoop101 software]# tar -zxvf protobuf-2.5.0.tar.gz -C /opt/module/
[root@hadoop101 opt]# cd /opt/module/protobuf-2.5.0/

[root@hadoop101 protobuf-2.5.0]#./configure 
[root@hadoop101 protobuf-2.5.0]# make 
[root@hadoop101 protobuf-2.5.0]# make check 
[root@hadoop101 protobuf-2.5.0]# make install 
[root@hadoop101 protobuf-2.5.0]# ldconfig

[root@hadoop101 hadoop-dist]# vi /etc/profile
#LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/opt/module/protobuf-2.5.0
export PATH=$PATH:$LD_LIBRARY_PATH

[root@hadoop101 software]#source /etc/profile
验证命令：protoc --version**
~~~

7. 安装openssl库

   ~~~
   [root@hadoop101 software]#yum install openssl-devel
   ~~~

8. 安装 ncurses-devel库

~~~
[root@hadoop101 software]#yum install ncurses-devel
~~~

到此，编译工具安装基本完成。

## 3.开始编译

1.	解压源码到/opt/目录

~~~1.	解压源码到/opt/目录
[root@hadoop101 software]# tar -zxvf hadoop-2.7.2-src.tar.gz -C /opt/
~~~

2. 进入到hadoop源码主目录

   ~~~
   [root@hadoop101 hadoop-2.7.2-src]# pwd
   /opt/hadoop-2.7.2-src
   ~~~

3. 通过maven执行编译命令

   ~~~
   [root@hadoop101 hadoop-2.7.2-src]#mvn package -Pdist,native -DskipTests -Dtar
   ~~~

等待时间30分钟左右，最终成功是全部SUCCESS，如图2-42所示。

4. 成功的64位hadoop包在/opt/hadoop-2.7.2-src/hadoop-dist/target下

   ~~~
   [root@hadoop101 target]# pwd
   /opt/hadoop-2.7.2-src/hadoop-dist/target
   ~~~


