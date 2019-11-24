#Linux 安装JDK/Maven/Git配置环境变量

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



#GIT 安装

1.下载安装包

```
sudo wget https://github.com/git/git/archive/v2.8.0.tar.gz
```

2.解压GIT安装包

```
cd /usr/local
mkdir git
tar -zxvf v2.8.0.tar.gz
```

3.解压Git安装包 -C后为解压的目标路径 

```
sudo yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker
```

4.安装GIT所需要的依赖

```
cd /usr/local/git/git-2.8.0

make prefix=/usr/local/git all
make prefix=/usr/local/git install
```

5.配置环境变量

```
vim /etc/profile

GIT_HOME=/usr/local/git/git-2.8.0
path=$GIT_HOME/bin:$path
```

6.刷新配置文件

```
source /etc/profile
```

7.配置GIT

```
git config --global user.name "Ronin"
git config --global user.email "ITMater233@gmail.com"
git config --global core.autocrlf false
git config --global gui.encoding utf-8
....[一直回车执行]

ssh-keygen -t rsa -C "ITMaster233@gmail.com"

```

8.生成 ssh key 

```
cat ~/.ssh/id_rsa.pub
.....[复制 sha 密钥]
```

9.登录GitHub配置 密钥

```
GitHub->Setting->SSH and GPG keys->NEW SSH KEY->密钥复制进去
```

10.测试是否配置成功

```
ssh -T git@github.com
```

