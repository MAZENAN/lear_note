- [1. Linux常用命令](#ml)
- [2. Linux系统定时任务](#dsrw)
- [3. vi/vim编辑器](#bjq)
- [4. shell基础](#shell)
- [5. 正则表达式考察知识点](#zz)


# <a id="ml">Linux常用命令</a>  
## (1) 系统安全  
    sudo  
    su  
    chmod  
    setfacl  
## (2) 进程管理  
    w、top、ps、kill、pkill、pstree、killall  
## (3) 用户命令  
    id、usermod、useradd、groupadd、userdel  
## (4) 文件系统  
    mount、umount、fsck、df、du  
## (5) 系统关机和重启  
    shutdown、reboot  
## (6) 网络应用  
    curl、telnet、mail、elinks  
## (7) 网络测试  
    ping、netstat、host  
## (8) 网络配置  
    hostname、ifconfig  
## (9) 常用工具  
    ssh、screen、clear、who、date  
## (10) 软件包管理  
    yum、rpm、apt-get  
## (11) 文件查找和比较  
    locate、find  
## (12) 文件内容查看  
    head、tail、less、more  
## (13)  文件处理  
    touch、unlink、rename、ln、cat  
## (14) 目录操作  
    cd、mv、rm、pwd、tree、cp、ls  
## (15) 文件权限属性  
    setfacl、chmod、chown、chgrp  
## (16) 压缩/解压  
    bzip2/bunzip2、gzip/gunzip、zip/unzip、tar  
## (17) 文件传输  
    ftp、scp  

# <a id="dsrw">Linux系统定时任务</a>  
## crontab命令  
crontab -e  
*****命令(分时日月周)  
## at命令  
at 2:00 tomorrow  

# <a id="bjq">vi/vim编辑器</a>   
__模式__：  
一般模式、编辑模式、m命令行模式。  
一般模式：删除、复制和粘贴。  
切换编辑模式：`i、I、o、O、a、A、r、R `   
切换命令行模式：`: / ?`  

__移动光标：__  
ctrl+f、ctrl+b、0或者功能键Home、$或者功能键End、G、gg、N+enter  

__查找和替换：__  

__删除、复制和粘贴：__  

x、X、dd、ndd、yy、nyy、p、P、ctrl+r  

__保存和退出：__  
w、q、wq  

__视图模式(vim)：__  
v、V、ctrl+v、y、d  

__配置:__  
:setnu、:setnonu  


# <a id="shell">shell基础</a>  
## (1) 脚本执行方式  
赋予权限，直接执行，例如：chmod + x test.sh;./test.sh  
调用解释器使得脚本执行，例如：bash、csh、ash、bsh、ksh等等。  
使用source命令，例如：source test.sh  
## (2) 编写基础  
开头用#!指定脚本解释器，例如: #!/bin/sh  
编写具体功能。  
