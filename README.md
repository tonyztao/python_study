本文章内容收录自一位同事的博客，记录的都是一些最常用的Linux操作和命令，为了方便自己学习和查阅，同时也为了加深记忆，故收录在此，今后也会随着对Linux的学习对下面的内容进行增补！也欢迎大家对我同事的github进行关注[@imaoda！](https://github.com/imaoda)

## 基础操作
命令 | 操作 | 备注
----|------|----
查看 | ls -al path | ll是个别名
新建目录 | mkdir -p path | -p可递归创建,即便存在也不报错
当前目录 | pwd  | 
新建文件 | vi path/file | :wq退出
删除 | rm -rf path | 递归强制删除文件/目录
剪切/更名	| mv pathA pathB | pathB不存在则改名，否则拷进去
拷贝 |	cp -r pathA pathB	| pathB存在会提醒覆盖
文件大小 |	du -sm *	| 路径下所有目录/文件大小(单位MB)
压缩	| tar -zcf file path |	(张c锋)把path压成file，后缀上tar.gz以增加可读性
解压	| tar -zxf file [-C path]	| (张x锋)以压缩前文件名解压到当前路径
查进程	| ps -ef	| 执行者(root/mysql/apache) pid 执行的命令
杀进程	| kill [-9] pid	| 带-9强制关闭，不带请求关闭
查杀一键 |	killall nginx	| 自动grep nginx对应的pid，杀之
查端口	| netstat -npl	(拿破仑) | 本地ip:端口 对端ip pid uid
环境变量 |	env	| 环境变量多渠道可设，env查看当前用户最终结果
修改环境 |	export可定义 |	不过重启失效
开机启动 |chkconfig	| chkconfig on xxx 开机自启动
关机 | /sbin/shutdown -h now |
重启 | reboot |
打开服务 |	service XXX start	| restart重启 stop停止
下载 | wget	| 需要先安装wget
别名 | alias ll="ls -al" | 定义别名
创建软链 | ln -s file link	| 快捷方式，可放在path定义的目录下
后台运行 | nohup command &	| 退出终端需要用exit，否则程序中断
远程拷贝 |	scp file root@ip:/root	| 最好先ssh免密
流量监控 | ifstat	| 查看上传下载流量  

> 如果某个应用只有一个执行文件，如node、npm，可将其软链放置到/usr/local/bin中，并添加到path中，避免path中过多路径

Linux默认目录的作用：  

目录 | 作用
----|----
/usr/bin | 非必须用户命令：gcc make
/usr/sbin	| 非必须系统命令：httpd sendmail
/bin	| 必须用户命令：mkdir cp
/sbin	| 必须系统命令(需root)：reboot fdisk
/usr/local	| 用户安装软件的位置 【常用】
/usr/local/bin	| 放置一些/usr/local里安装程序的link
/home	| 其他用户的~，比如/home/maoda
/root	| 管理员的home
/etc	| 配置：账密、开机启动、hosts修改
~/.bashrc | 修改当前用户的环境变量，source ~/.bashrc生效

## 权限
ls 同时列出用户对文件的权限：  
- 文件权限:d/-[owner权限][group权限][other权限]
- owner：如maoda  
- group：如dimpt  

root可更改文件的归属，以及设定权限：  

目的 | 命令  
---- | ----
更换所有权	| chown [-R] group:owner path/file
更改权限	| chmod [-R] 777 path/file  

权限rwx对应的意义如下：  

权限 | 对于文件 |	对于目录
---- | ----- |----
r	| 可读取 |	可查看(ls)
w	| 可编辑	| 可在该目录下增改删
x |	可执行	| 可进入目录(cd)

上面所说的用户和组，都保存在/etc配置下：

文件名 | 描述
---- | ----
/etc/passwd	| 用户信息：用户名UIDGID
/etc/group	| 组信息：组名GIDuser1,user2
/etc/shadow	| 存加密密码：第八位改0可使账号临时失效

> 在/etc/passwd中可以看到诸如mysql的账号，是系统和服务给自己设定了一个较小权限的账号来运行服务

root 有权限操作用户： 

操作 | 命令
---- | ----
创建用户	| useradd maoda ; passwd maoda
删除用户	| userdel -r maoda
切换用户	| su maoda
创建组	| group add family
加入组	| usermod -a -G family maoda

## 开机启动
### 启动流程
linux有7种运行等级，一般运行在等级3，开机后：  
1. 执行脚本/sbin/init（读取/etc/inittab文件确定运行等级）  
2. 执行脚本/etc/rc.d/rc.sysinit，设定path等  
3. 根据等级3，执行/etc/rc3.d/目录下软链的服务（软链指向/etc/init.d/目录）  
4. 执行/etc/rc.d/rc.local脚本，可DIY shell命令  
5. 执行/bin/login触发登录

### 手动开关服务(了解即可)
所有的服务类应用放init.d里，自启动的在rc3.d里建个软链   

''' /etc/init.d/
mysqld php-fpm pureftpd nginx/httpd redis-server
'''

## 环境变量

环境变量是程序之间调用的纽带，承载的是路径；  

已经安装的程序是可以更改目录的，但需确认：
1. 依赖你的程序知道你换位置了  
2 .重启后如需自启动，init.d下的脚本知道你换位置了  
3. 之前创建的一些软链知道你换位置了  
 
如果你的位置信息通过一个环境变量来记录，更换后，只需修改环境变量即可，否则要去各个地方一一修改。



