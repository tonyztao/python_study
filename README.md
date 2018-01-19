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

```
/etc/init.d/
mysqld php-fpm pureftpd nginx/httpd redis-server
```

>yum安装的软件，才会自动放置该脚本  

service 命令可用来开关服务，实际是语法糖
```
以下两者等价
service mysqld start/stop/restart/status 
/etc/init.d/mysqld start/stop/restart/status
```

## 环境变量

环境变量是程序之间调用的纽带，承载的是路径；  

已经安装的程序是可以更改目录的，但需确认：
1. 依赖你的程序知道你换位置了  
2 .重启后如需自启动，init.d下的脚本知道你换位置了  
3. 之前创建的一些软链知道你换位置了  
 
如果你的位置信息通过一个环境变量来记录，更换后，只需修改环境变量即可，否则要去各个地方一一修改。

#### 自启脚本

文件 | 描述
---- | ----
~/.bashrc	| 随sh执行，当前用户，别名和函数。
~/.bash_profile	| 登录执行，当前用户，系统环境
/etc/bashrc	| 随sh执行，全用户，别名和函数。加载/etc/profile.d/中sh
/etc/profile	| 登录执行，全用户，系统环境。加载/etc/profile.d/中sh

>理论上讲，配置/etc/profile 就可以全用户配置，配置完成后需重启，把别名、环境变量放在自启动脚本里，可实现开机初始化

**别名**可以把长串命令用一个字符串代替  

别名 | 配置命令
---- | ----
进入特定目录	| alias www1 = cd /data/wwwroot/
修改特定配置	| alias conf1 = vi /usr/local/nginx/conf/nginx.conf

#### 环境变量

变量名 | 释义
---- | ----
JAVA_HOME	| jdk的安装路径(不含bin)，如：JAVA_HOME=/usr/lib/jvm/java-1.8
CLASSPATH	| java类的查找目录，.:/usr/lib/jvm/java-1.8/lib/dt.jar:/usr/lib/jvm/java-1.8/lib/tools.jar
PATH	| /usr/local/bin:/usr/sbin:/usr/bin

为了便于管理，PATH默认这三个足够。后续安装软件如需任意路径执行，可向/usr/local/bin中添加软链
```
ln -s /usr/local/spark/bin/spark /usr/local/bin/spark
软链名字最好与程序名一致，使用统一
如果该路径下有多个执行程序，一一添加麻烦，可加入path
```
设置别名/环境变量案例
```
vi ~/.bashrc
# vi ~/.bash_profile
export JAVA_HOME=/usr/lib/jvm/java-1.7
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
alias rm='rm -i'
alias wb="cd /data/wwwroot/temp/ant && npm run build"
```

## 防火墙
默认屏蔽一切外部的访问，随着应用的安装逐渐开放端口（诸如80）。如果不打开对应端口，应用就无法通信。  

>当然关闭防火墙很简单粗暴，但是不安全
```
启动/自启动防火墙
service  iptables start/stop/restart
chkconfig iptables on/off

打开某端口
iptables -I INPUT 4 -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
service iptables save

查询已打开端口
iptables -L -n  查看开了哪些端口号
iptables -L     查看那个应用开的
```
>centos6使用的iptable，centos7有较大变化

## 定时任务
每个用户可设定自己的定时任务crontab，注意定时任务是对“账户”设置的  
```javascript
编辑
crontab -e 可以编辑/删除

格式：分钟(0-59) 小时 日期 月份 周 命令   *表示都ok ,表示枚举都ok -表示时间段都ok */n表示每隔时间段
20 8 */2 3-6 * /home/test.sh  表示春天每隔一天每天8:20发(注意如果php需要写绝对路径)  
27 * * * * /usr/local/php/bin/php /data/wwwroot/default/edu/spiderSina.php

在crontab中执行的脚本，一定要注意程序内的文件路径，相对路径是不可以的，程序中建议用 __dirname, __filename
可用path模块来获取
```

## vim编辑器
虽然我们一般在 vs-code 里写代码，但是直接在linux上改个配置啥的还是要用 vim ，常用操作：  

命令 | 操作 
---- | ----
命令模式	| esc ： 例如输入:wq表示保存退出
编辑模式	| aeiou 都可以触发编辑
查询关键字	| ?query 向前查找； /query 向后查找； n继续找
快速翻页	| 30↓可快速移动光标，←↑↓→都可以，也可pageUp/pageDown
删除	| 10dd 删除光标所在向下20行
复制	| 10yy 下乡复制10行
撤销	| u

## 其他配置 
ip配置  
``` javascript
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```
dns配置
``` 
vi /etc/resolv.conf
加入：
nameserver 10.100.29.35
nameserver 202.106.46.151
nameserver 10.202.72.116
```
>网络相关配置完毕后 service network restart

## 软件安装
#### 基础工具
yum 是linux用来安装软件的工具，其特点：

自动安装依赖
* 默认目录，并自动创建软链和服务
* 可通过rpm -qa |grep xxx 看某程序是否是npm安装的
* 再通过rpm -ql | grep 完整名 看安装位置

>有些软件系统自带，并非yum安装的，可用 which 查看一下是否可用，如 which wget

yum 需先配置镜像源，镜像源配置如下：

``` 
描述文件
/etc/yum.repos.d/xxx.repo
名字后缀为.repo即可
```
>建议使用阿里云镜像加速，修改完需执行 yum makecache把镜像库缓存到本地。公司linux版本号有问题，应为6,却显示为6Server，因此连不上镜像，需手动修改repo里$releasever为6，偶尔gpgcheck报错，可在.repo配置里置0关闭验证

## FTP
安装vsftpd，便于大量文件上传  
```python
yum install vsftpd -y
vi /etc/vsftpd/vsftpd.conf (其实默认即可)
//启动ftp
service vsftpd start
//设置开机启动
chkconfig vsftpd on
//配置ftp账号
useradd www
passwd www
```
>使用ftp请建立一个ftp的账户，不要使用root

## git
创建私有 git，便于代码的一键同步（替代原来的 svn）

```
####### 创建git账号 ###
# 添加账号
useradd git | passwd git Asdqwe123

####### 安装git软件 ###
yum install git

（以下所有操作在git账号下进行）

######## 配置免密登录 #####
# 在需要免密的账号@设备下创建 rsa
ssh-keygen -t rsa  (后将生成的文件rz到服务器中)
# 把id_rsa.pub内容追加到 /home/git/.ssh/authorized_keys
mkdir -p /home/git/.ssh 
cat id_rsa.pub >> /home/git/.ssh/authorized_keys && rm -rf id_rsa.pub
# 如果不行，请以root身份vi /etc/ssh/sshd_config修改StrictModes no 后service sshd restart

注意！ 每次生成新的rsa_key老的就失效，git钩子需要自己给自己免密

######## 初始化服务器仓库 ######
# --bare参数一定要有，注意顺序
git init --bare xx.git 
# 第一次使用空仓库要通过push来定义master
git add .
git commit -am "ss"
git push -u origin master

######## windows检出仓库 ######
# (origin名称可自定)
git init
git remote add origin git@ip:/somepath/git 
git remote add origin git@github.com:imaoda/projectName.git 
# 第一通过这两个命令打通，后续只用pull push 无需参数
git pull origin master
git push -u origin master

######## linux钩子检出仓库 ######
# 首先确保自己ssh自己免密，在制定路径（/data/www/imaoda.com/g）初始化（有权限）
cd /data/www/imaoda.com/g
git init 
git remote add origin git@localhost:/somepath/git
# 第一通过这两个命令打通，后续只用pull push 无需参数
git pull origin master
git push -u origin master
# 修改远程库中的钩子,并确认git账户keep执行
vi /somepath/git/post-update
#!/bin/sh
cd /somepath/imaoda.com/g && unset GIT_DIR && git pull
# 这里unset GIT_DIR 一定要加
```











