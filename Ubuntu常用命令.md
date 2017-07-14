#### 清空 redis 数据

	redis-cli
	flushall

#### MySQL 5.2 以上版本开启 query log

> 注意： 这里记录的 MySQL SQL 语句是MySQL服务器接收的语句并没有经过任何解析。

    sudo vim /etc/mysql/my.cnf

[mysqld] 后 添加 下面两行：

	// 这一步 日志文件最好放在 /var/log 目录下（需要 mysql 用户可写 权限）
    general_log_file = /var/log/mysql/query.log
    general_log      = on

进入 MySQL 命令行 启用log：

    mysql -uroot -p

    set global general_log = on;

重启 MySQL 使得 配置生效：

    sudo service mysql restart

#### 创建软连接

	ln -sf `pwd`/bin/casperjs /usr/local/bin/casperjs

#### TCPDump 抓 HTTP 包

	// -s 0 表示抓全包  -A 表示所有
	// -A 表示以 ASCII 字母方式打印数据包
	GET： sudo tcpdump -s 0 -A 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x47455420'
	POST： sudo tcpdump -s 0 -A 'tcp dst port 80 and (tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354)'


#### fuck 黑魔法

	alias fuck='sudo $(history -p \!\!)'

#### 显示所有系统变量

	env

#### apt 查看软件版本，并安装特定版本的软件

	sudo apt-cache showpkg package  // 查看可以安装的版本
	sudo apt-get install xxx=x.y.z  // 指定版本安装

#### 在新的 Tab 页打开 CMD

	Ctrl + Shift + T

#### 设置环境变量

添加全局变量在`/etc/profile`文件中

	vi /etc/profile
	添加 export PATH="$PATH:/etc/apache/bin"

注意：＝ 即等号两边不能有任何空格
修改后运行 `./profile` 重新加载环境变量使修改生效。

#### 命令行方式给浏览器设置代理

	Google-Chrome-stable --proxy-server="127.0.0.1:8080"

默认使用 HTTP 代理，支持 HTTPS, SOCKS.

#### 递归更改文件夹权限

	sudo chmod -R 777 <dirname>

#### 修改本机 HOSTS 文件

	sudo vi /etc/hosts

#### 从命令行打开可视化当前目录

	nautilus

#### 运行 jar 包

	java -jar <jarname>

#### 查找命令在系统中的位置

	which ls # 查找 ls 命令的默认位置
	whereis ls # 查找 系统中所有叫做 ls 的文件和文件夹

#### tar 压缩

	tar -xzvf file.tar.gz	# 解压缩到当前目录

#### 取消MySQL开机自启动

	vi /etc/inid/mysql.cnf
	// 对应的条目修改成以下
	#start on runlevel[2345]
	stop on starting rc RUNLEVEL=[0123456]

#### 关闭 mysql 服务

	sudo service mysql stop

#### 取消 Apache 开机自启动

	update-rc.d -f apache2 remove

#### 截图

	gnome-screenshot -a // 手动截取选定区域

#### 运行 Burp Suite

得到`BurpLoader.jar burpsuite_pro_v1.5.20.jar`文件，移动到`/opt/burp`文件夹内

	sudo chmod +x BurpLoader.jar burpsuite_pro_v1.5.20.jar 
	sudo touch /usr/bin/burp && sudo chmod +x /usr/bin/burp
	// burp 的文件内容为：
	#! /bin/bash
	java -jar /opt/burp/BurpLoader.jar

#### 配置右键打开终端

	sudo apt-get install nautilus-open-terminal	// 重启生效

#### 配置 FTP 服务器

	apt-get install vsftpd  // 需要先安装 FTP 服务
	service vsftpd start	// 启动

打开 `/etc/vsftpd` 修改下面几项：

	listen=YES				// 开启服务
	anonmyous_enable=YES // 允许匿名登录
	local_enable=YES		// 允许本地用户登录
	write_enable=YES		// 允许全局写权限
	local_umask=022		// 设置访问权限 上一条写权限同样适用
	// 022表示7（rwx）5（rx）5（rx）
	// u 所有者 g 同组人员 o 其他组人员

#### 进程相关

	pkill -9 进程名  // 通过进程名杀死(-9 表示强制)
	kill -9 进程ID	// 通过进程ID杀死
	netstat [-lnp|-anp|-tnp] | grep 8000  // 查找使用8000端口号的进程
	ps -aux	// 显示所有用户的进程并打印相应的CMD命令

#### Ubuntu 设置静态 IP

	sudo vim /etc/network/interfaces
	// 修改成如下
	auto eth0
	iface eth0 inet static
	address 192.168.0.117
	gateway 192.168.0.1 
	netmask 255.255.255.0
	network 192.168.0.0
	broadcast 192.168.0.255

重启服务 `service networking restart`

#### Ubuntu 配置 DHCP 连接方式

	sudo vim /etc/network/interfaces
	// 修改成如下
	auto eth0
	iface eth0 inet dhcp

#### Ubuntu 设置临时 IP

	ifconfig eth0 192.168.1.130 netmask 255.255.255.0 up  // IP 子网掩码
	route add default gw 192.168.1.1	// 网管
	vim /etc.resolv.conf
	添加 nameserver 8.8.8.8  // DNS

#### 开启 关闭 SSH

	service ssh start/stop  // 需要先安装 SSH

#### 禁止 Ping 主机

	echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all

#### 查看系统磁盘使用率

	df -h

#### 查看内核版本

	uname -a

#### 查看 CPU ,内存

	top	 // 查看 CPU ， 内存情况
	free -h // 查看内存使用率

#### 安装/卸载 .deb 包

	sudo dpkg -i package.deb // 安装
	sudo dpkg -r package   // 卸载

#### 将普通用户（anka9080）添加到 sudoers

	adduser anka9080  //填写好密码之后
	sudo vi /etc/sudoers
	// 在 root ALL=(ALL) ALL 下一行添加
	anka9080 ALL=(ALL) ALL

#### zip 压缩解压缩

	zip -r new.zip dirname
	unzip new.zip

#### Linux 目录介绍

- /	根目录，必须要挂载的目录 1-2G（主分区）
- /boot	引导目录，建议单独挂载 大小100M（主分区）
- /swap	交换分区，设置为物理内存的2倍，若物理内存>2G，设为2G，类似Win下虚拟内存，休眠时会用到
- /home	用户home目录所在地，容量依普通用户数量定，一般每个用户100M（主分区）
- /opt	第三方应用程序默认安装目录
- /usr	系统软件所在目录
- /var	动态的应用程序数据，余下空间（10-20G）

##### 设置Apache 支持 GD库 扩展

	apt-get install php5-gd  // 重启服务器

#### 设置 MySQL 外网可访问：

	use mysql;
	update user set host = '%' where user = '需要授权的用户名';
	修改 /etc/mysql/my.cnf 文件 bind ip 127.0.0.1 改成需要授权的ip
	重新启动 MySQL

#### 安装 Flask

	pip install flask

#### Ubuntu14.04 安装 LAMP

	apt-get install apache2 php5-mysql libapache2-mod-php5 mysql-server

#### Python 安装 MySQL库

	sudo pip install mysql-python
	// 若报错 pip install mysql-python fails with EnvironmentError: mysql_config not found
	sudo apt-get install libmysqlclient-dev
	// 若报错 fatal error: Python.h: No such file or directory
	sudo apt-get install python-dev

#### Mysql5.7 忘记root密码

	service mysql stop
	mysqld_safe --skip-grant-tables --skip-networking &
	mysql -p
	update mysql.user set authentication_string=password('123qwe') where user='root' and Host = 'localhost';
	flush privileges;
	service mysql restart
	set password for 'root'@'localhost'=password('123');  // ？

#### MySQL 创建新用户

	// 新建用户
	CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';

	// 赋权
	GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';


	GRANT SELECT,DELETE,UPDATE,INSERT,TRIGGER ON hivescan.* TO 'hivescan'@'%';

	// 刷新生效
	FLUSH PRIVILEGES;

#### MySQL 导入导出表结构

	// 导出数据库
	mysqldump -uroot -p database_name > filename
	// 到出表
	mysqldump -uroot -p db_name table_name > filename
	// 导入表
	source filename.sql
	// 查看表结构
	desc table_name

#### Ubuntu 查看当前远程链接用户

	w

#### ssh 日志存放位置

	/var/log/auth.log

#### 查看SSH最近几次 成功登录的日志

	last
	若因为wtmp文件不存在导致last命令不可用，可以手动创建 wtmp 文件
	touch /var/log/wtmp
	chmod 0664 /var/log/wtmp
	chown root:utmp /var/log/wtmp

#### Ubuntu连接 PPTP VPN服务器

新建一个 VPN 连接，点击 Edit，按如下图配置

<img src="http://7xku36.com1.z0.glb.clouddn.com/connect_pptp_conf1.png" width="60%">
<img src="http://7xku36.com1.z0.glb.clouddn.com/connect_pptp_conf2.png" width="60%">

#### 查看文件夹大小

	du -sh [filename|dirname]

#### split 分割文本文件

	split -b 50m es.zip es_part_  // 分割后每个文件大小50m
	// 组装分割后的文件
	cat es_part_* > es.zip
	// 计算md5sum
	md5sum es.zip

#### TCPDUMP抓包

	tcpdump -i eth0 -w output.cap

#### 开启 Apache URL 重写

	// 加载重写模块
	cd /etc/apache2/mods-enable
	ln -sv ../mods-available/rewrite.load rewrite.load
	// 更改重写配置
	vi /etc/apache2/apache2.conf
	AllowOverride None 改成 AllowOverride All

#### 一条命令杀死所有celery的相关进程

	ps aux | grep celery | awk '{print "sudo kill -9 " $2}' | sh

#### 设置命令别名alias和取消命令别名unalias

	alias kc='echo HAHA'
	unalias kc
	// 若命令太长建议写成bash脚本的形式放在/bin目录下就可以直接执行