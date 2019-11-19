# 01 Docker序幕揭开篇

# 1.1 使用Vagrant+VirtualBox安装Cnetos7

# Vagrant安装：
    https://www.vagrantup.com/
# Virtual Box安装
    https://www.virtualbox.org/
# 在线玩Docker
	http://labs.play-with-docker.com

# 检查端口号是否被占用

netstat -ano|findstr 8009
taskkill /f /pid 14496

# 1.1.1 创建D:\VM\docker-centos7文件夹，并进入此目录

	mkdir D:\VM\docker-centos7 [创建文件夹]
	cd D:\VM\docker-centos7 [进入目录]


# 1.1.2 在目录中打开cmd命令

    vagrant init centos/7 [此时会在当前目录下生成Vagrantfile文件]

# 1.1.3 添加virtualbox

    vagrant box add centos/7 D:\VM\virtualbox.box [添加virtualbox]
    vagrant box list [查看是否添加成功]

# 1.1.4 修改Vagrantfile文件

01 修改config.vm.box 镜像名，centos/7是在第三步添加的
    config.vm.box = "centos/7"
02 指定使用桥接模式连接网络
    config.vm.network "public_network"
03 判断如果是virtualbox的话，就设置指定内存、主机名、cpu数
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"
        vb.name= "hadoop-centos7"
        vb.cpus= 2
    end

# 1.1.5 开启虚拟机
    vagrant up

# 1.1.6 Xshell工具连接虚拟机

	vagrant ssh-config
IP
    127.0.0.1
Port
    2222
用户名
    vagrant
密码
    vagrant
Key文件
    Identityfile路径
使用Xshell连接centos7
    用127.0.0.1连2222端口 或者 用centos7的ip连22端口

# 1.1.7 Root账户登陆

01 切换root用户
    sudo -i
02 修改sshd配置文件
    vi /etc/ssh/sshd_config
03 修改PasswordAuthentication 以及修改permitRootLogin
    PasswordAuthentication yes / permitRootLogin yes 把前面的#号去掉
04 passwd修改密码为root
    passwd
05 重启sshd
    systemctl restart sshd
06 使用root登陆
    ssh root@10.13.11.84

# 1.1.8 box打包分发

01 退出虚拟机
    vagrant halt
02 打包
    vagrant package --output first-docker-centos7.box
03 添加到box list
    vagrant box add first-docker-centos7 first-docker-centos7.box
04 生成Vagrantfile文件
    vagrant init first-docker-centos7
05 启动虚拟机
    vagrant up

# 1.3 常用命令

# 1.3.1 vagrant常用命令

# 进入虚拟机
    vagrant ssh
# 查看虚拟机状态
    vagrant status
# 停止虚拟机
    vagrant halt
# 删除虚拟机
    vagrant destory
# 修改Vagrantfile文件之后重新加载生效
    vagrant reload
# 查看虚拟机信息 Hostname Port IdentityFile
    vagrant ssh-config
# 打包
    vagrant package --output first-docker-centos7.box
# 添加到box list
    vagrant box add first-docker-centos7 first-docker-centos7.box
# 生成Vagrantfile文件
    vagrant init first-docker-centos7
