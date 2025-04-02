[TOC]

### stat

```bash
# 类似window右键属性
$ stat xxx                  
  文件：xxx
  大小：817             块：8          IO 块大小：4096   普通文件
设备：8,1       Inode: 3932441     硬链接：1
权限：(0744/-rwxr--r--)  Uid: ( 1000/    kali)   Gid: ( 1000/    kali)
访问时间：2024-04-28 15:37:25.838624039 -0400 cp cat
修改时间：2024-04-28 15:37:20.354631554 -0400 vi >>
变更时间：2024-04-28 15:37:20.358631549 -0400 chmod mv
### 创建时间：2024-04-28 15:37:20.354631554 -0400 touch
```

### chattr+lsattr

```bash
不更新atime(A)
同步更新(S)
只能添加(a)
压缩(c)
不可变(i)
不可转移(d)
删除保护(s)
不可删除(u)
-R 递归目录

chattr +i file  文件加锁
chattr +d file  绕过dump

lsattr file   查看文件属性锁
```

### atime,mtime,ctime

```sh
atime	access time	访问时间	文件中的数据库最后被访问的时间
mtime	modify time	修改时间	文件内容被修改的最后时间
ctime	change time	变化时间	文件的元数据发生变化。比如权限，所有者等
```

### cp

```bash
cp -a 源 目
-a：它保留链接、文件属性，并复制目录下的所有内容。
-p：保留原始属性
-r：递归复制目录
```

### curl 

```sh
（HTTP、HTTPS、FTP、FTPS、TFTP、DICT、TELNET、LDAP 或 FILE）
-v 输出头信息
-u user:pass  "ftp://192.168.121.166:21/"
-X PUT 或 HEAD
--proxy [protocol://]host[:port]
-A "Mozilla/5.0"
-H 'header: 空格' -H ''
-c cookie.txt  接收
-b cookie.txt  发送
--data-urlencode "post1=1&post2=2" -d "@本地文件"
-e  "Referer"
-F "file=@shell.php;filename=me.jpg;type=image/jpg"


curl -v "dict://127.0.0.1:8080"
curl -v "gopher://127.0.0.1:8080/_Hello%0aHello"
```

### ln

```bash
ln -s ./源 ./快捷方式  #创建快捷方式, 可以链接目录
unlink ./快捷名        #删除此快捷方式
ln -d src dst         #创建硬链接接, 表示所有文件操作会同步,底层都是指向同一处硬盘地址
```

### vi-vim

```sh
vi
	/xx 	查找xx,  n下一个 N上一个
	:u 		撤销
	:set nu 显示行号
	:6  	跳转第6行
	:12,24d 删除12-24行
	dd/D 		删一行/删到尾行
	yy  	拷贝光标行
	p  		粘贴
	gg/G   	跳至首行/最后一行
	:sort	排序行
```

### sed

```sh
	sed -i(直接修改源文件) -e '' -e '' ./文件名	
	's/xx/yy/g' 全局替换xx为yy
	'/root/d' 删除含root的行
	'2,5d' 删2-5行
	'2,$d' 删2-最后行
	'2,5cxxx' 2-5行重写成xxx
	'2ixxx' 2行前插入xxx
	'2axx' 2行后追加xxx
```

### grep 

```sh
# 使用管道符时确保前面的使用了-e解读binary
-i   忽略字符大小写的差别
-n   标示行号
-v   反转查找
-r   递归目录下查找
-C  10 同时回显匹配行的前后10行
-o  只输出匹配红字内容
-E  '正则'

# 匹配url
grep -Eo "((http|https)://[a-Z0-9./?_\-]*)|(.+)\?.+="
```

### awk   -e 

```sh
awk -e -F '分隔符' '{print $1}' file
	-F '[: /]' '{print $1}' 多个分隔符同时使用,:和空格和/
	'{print "http://"$1}'  字符串拼接
	'{if($1=="root"){print $1;}}'
	'{if($1~/ro{2}t/){print $1;}}'  ~/正则匹配/  匹配root 
awk '/allen|alan/' file  匹配allen或 alan
```

### sort | uniq  -c 去重输出

```sh
sort passwd -t ':' -k 2 # 以:分隔,按第2列排序
-h   人类易读
-n   依照字符串数值的大小排序。
-r   以相反的顺序来排序。
-t <分隔字符>,默认空格分隔
-k n 在位置n列排序 
```

### 正则

```shell
.		 除\n任何一个
\b		 \w单词边界（space , . \r  \t \n 等 ）
\s 		space \t \r \n \v \f
\w		字母数字下划线 aZ9_



a|b		 a 或 b
[] 		只能代表单个字符,其中的特殊字符需要转义, "-"横杠在其中有特殊意义标识[a-z]范围, 需要转义[a\-z]
()		可以代表一个词组
(?<!text)	左侧不能是text
(?!text)	右侧不能是text
[abc]	 a 或 b 或 c
[^abc]	 不包括 a, b, c
[a-z]		 匹配从 a 到 z 之间的任一字符
[a-zA-Z]		 匹配从 a 到 z, 及从 A 到 Z 之间的任一字符
^头部
$尾部
( )		 匹配标记的子表达式
\1-9		 标记的子表达式 代表 1 到 9


*?		 匹配前一项内容 0 或多次 (jin)
+?		 匹配前一项内容 1 或多次 (尽可少)
{x}		 匹配前一项内容 x 次
{x,}		 匹配前一项内容 x 或多次
{x,y}		 匹配前一项内容次数介于 x 和 y 之间
```

### 网络

```sh
ip address add/del 192.168.0.20/24 dev ens33   #临时改IP
route add/del default gw 192.168.0.1 #改网关
ip route flush cache  #刷新路由
traceroute #跟踪路由
ifconfig ens33 up/down  -- systemctl restart network

vim /etc/network/interfaces  #编辑 IP等配置 Debian
vi /etc/sysconfig/network-scripts/ifcfg-eth0 #编辑 IP等配置 Redhat
```

### 用户权限操作

```sh
    groupadd/mod 组名
    groupdel
groups username  #查看所在组

    useradd/mod [-g 组名]  [-G 附加组] [-d 用户主目录路径] [--shell --login]用户名
    usrmod  -L [ 锁定一个用户的帐号] -U [解锁一个用户的帐号]
    userdel [-f 强制] [-r 同时删除主目录] 用户名 
    
chmod [a/u/g/o +- rwx(-R 421)] 文件名
chgrp [-R 递归] 组名 文件名
chown user file
passwd kali # 改密码

_____________________________uname -a; id ;whoami;w;who;____________________________
uptime #运行时间统计
	19:50:10 up 8 days,  9:51,  1 
	user,  load average: 0.00, 0.01, 0.05
```

### sudo

```bash
su - username  #切换用户登录
sudo /bin/bash
sudo -l #查看自己可以免密执行的,即/etc/sudoers
find / -perm -u=s 2>/dev/null #查看可以无需root授权即可执行的
/etc/sudoers
	username 主机=以哪用户权限 NOPASSWD:路径
```

### find

```sh
find ./ -iname '*pattern*' #不区分大小写 回显所有文件带路径
find ./ -name '[^a-z][0-9]*.ex?t' 
find ./ -perm -u=s #查找可以使用root权限的
find ./ -perm 4000 #同上
find ./ -type f/d
find /var -regex '.*/tmp/.*[0-9]*.file'

-or  或者
-a   且
-not  取反
-path "*lib*" 匹配路径
-mmin -5 在5分钟内被修改过  -amin -cmin
### -mtime +5 在5天前被修改过   -atime -ctime

```

### docker

```sh
docker ps -a	List all containers
docker ps -s	List running containers
docker images	List all images
docker exec -it <container> bash	Connecting to container
docker stop <container>	Stop a container
docker rm <container>
docker rmi <images>
# 在配置文件目录下执行docker-compose
docker-compose up -d
docker-compose stop
```

### 系统内核版本

```bash
/etc/issue 操作系统版本
uname -a 内核
hostnamectl set-hostname xxx #改主机名
sysctl -a    #返回net.ipv4.tcp_syn_retries = 2等各种信息
sysctl -w net.ipv4.tcp_syn_retries=2 #修改配置
echo 1 > /proc/sys/net/ipv4/ip_forward #开启转发请求,中间人监听
sysctl -w net.ipv4.ip_forwards=1  #等效同上
```

### tarunzip

```sh
#解压
tar -zxvf ./xx.tar.gz -C ./     
unzip -v hello.zip
# 归档压缩
tar -cvf  ./opt.tar  /opt
### gzip ./opt.tar ./opt.tar.gz

```

### 进程与服务

```sh
ps -ef/-aux  #任务管理器
top -n 1 # 运行一次后,标准输出
kill -9 pid
pkill -ef 进程名
pkill -kill -t pts/1  #杀掉某用户的bash连接, pts/1来源于who/w查询

# 查看所有服务 
service --status-all | grep +	
systemctl list-unit-files

systemctl status 服务名
service sshd start/stop
### systemctl start sshd

```

### 标准输入输出

```sh
1>标准正确
2>标准错误

2>&1|tee -a ./xx.txt #标准输出同时保存; 
2>/dev/null    #不输出报错
1>xx 2>&1       #报错指向标准输出


<<EOF 输入文本内容可,  #EOF是标识,其他字母也行
代替文件
EOF
cat >1.txt <<EOF
...
EOF
```

### 后台挂起

```sh
Ctrl+Z -- 转入后台
	jobs -- 挂起程序的编号
	fg %编号 -- 调回前台继续执行
	bg %编号 -- 后台运行
	&  -- 直接后台执行
	kill -9 %编号 结束挂起的程序
```

### 定时任务Crontab

```sh
cat etc/crontab
SHELL=/bin/bash                            -- 使用那个shell
PATH=/sbin:/bin:/usr/sbin:/usr/bin         -- 的环境变量路径
MAILTO=root                                -- 邮件接收用户
*  *  *  *  * 脚本
分 时 天 月 周
*/5 每5
{minute} {hour} {day-of-month} {month} {day-of-week} {full-path-to-shell-script} 

修改/etc/crontab这种方法只有root用户能用，这种方法更加方便与直接，直接给其他用户设置计划任务，而且还可以指定执行shell等等

crontab 
-e 这种所有用户都可以使用，普通用户也只能为自己设置计划任务。然后自动写入/var/spool/cron/  以文件名区分用户
crond会每分钟读取一次/etc/crontab与/var/spool/cron中的数据内容
-l 查看定时任务
-r 删除定时任务
```

### 安装软件

```bash
apt list   dpkg -qa
yum list   rpm -qa

fakeroot alien xxx.rpm  可以转为deb包来安装
dpkg -i xx.deb 即可安装
dpkg -r 包名 卸载

apt update  是从etc/apt/source.list获取最新的软件包列表
apt upgrade  是真正的下载更新并安装软件,如果需要更新依赖,默认不更新

apt autoremove xxx 移除软件 yum erase
vim /etc/apt/sources.list 编辑 apt源
vim /etc/resolce.conf DNS的配置信息
```

### tcpdump

```sh

### tcpdump tcp and dst port 80 -i ens33 -c 100 -w ./xx.cap

```

### ssh-keygen

```sh
ssh-keygen -t rsa  #生成RSA公钥和私钥
	Enter file in which to save the key (/root/.ssh/id_rsa):./id_rsa
	Your identification has been saved in ./id_rsa
	Your public key has been saved in ./id_rsa.pub
	#将公钥id_rsa.pub内容追加到服务器 ~/.ssh/authorized_keys
	#然后客户端,ssh -i ./id_rsa user@serve_ip  
	
```

### 防火墙

```sh
/etc/sysconfig/iptables-config
/etc/firewalld/firewalld.conf
firewall-cmd --reload
firewall-cmd --list-all
firewall-cmd --add-port=3306/tcp  (--permanent)(--timeout=60s/1m/24h)
firewall-cmd --add-service=http
firewall-cmd --add-rich-rule='rule family = ipv4 source address = 192.168.0.5(或192.168.0.0/24) port protocol = tcp port = 80 drop/accept'
             或) service name = http           drop/accept
             
iptables -nL
-I 设置规则
### http://linux.51yip.com/search/iptables

```

### 当前SHELL环境变量

```bash
# 终端编码
locale
echo $LANG
echo $SHELL
ps
set |grep -i 
```

### linux下的敏感文件路径

```php
/root/.ssh/authorized._keys //ssh登录认证文件
/root/.ssh/id_rsa.keystore.//密钥存放文件
/root/.ssh/known_hosts  //已访问过的主机公钥记录文件
/root/.bash_history    //记录系统历史文件
/root/.mysql_history  //记录数据库历史文件
```