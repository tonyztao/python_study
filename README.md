本文章内容收录自一位同事的博客，记录的都是一些最常用的Linux操作和命令，为了方便自己学习和查阅，同时也为了加深记忆，故收录在此！也欢迎大家对我同事的github进行关注[@imaoda！](https://github.com/imaoda)

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






