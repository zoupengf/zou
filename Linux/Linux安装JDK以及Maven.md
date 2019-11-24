#Linux 安装JDK以及Maven配置环境变量

#JDK安装

1.上官网下载jdk压缩包

 http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html 

 本人orcale账号 ：348735445@qq.com 密码：Zz3223368

2.把jdk压缩文件上传至服务器

```shell
yum install -y lsrsz [安装直接上传的命令]
cd /usr/local
mkdir java
tar -zxvf jdk-8u181-linux-x64.tar.gz
```

3.配置环境变量

```
vim /etc/profile

export JAVA_HOME=/usr/local/java/jdk1.8.0_171 [自己安装是什么版本的就修改]
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export  PATH=${JAVA_HOME}/bin:$PATH

```

4.刷新 配置文件

```
source /etc/profile
```

5.检查是否安装成功

```
java -version
```



#Maven安装

1.安装wget 命令以及下载安装包

```
yum -y install wget

wget http://mirrors.cnnic.cn/apache/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz
```

2.解压maven安装包

```
cd /usr/local
mkdir maven
tar -zxvf apache-maven-3.6.2-bin.tar.gz
```

3.配置环境变量

```
vim /etc/profile
export MAVEN_HOME=/usr/local/maven/apache-maven-3.6.2
export PATH=$MAVEN_HOME/bin:$PATH
```

4.刷新配置文件

```
source /etc/profile
```

5.检查下maven是否安装成功

```
mvn -version
```

