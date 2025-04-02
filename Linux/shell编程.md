[TOC]



启动项脚本/环境变量

```ini
/etc/profile : 系统开机运行
~/.bashrc	 : 解锁屏幕自动运行
~/.bash_profile : 打开终端自动运行
```

/etc/shells

```bash
echo "环境变量的路径为：$PATH"
echo '环境变量的路径为：$PATH'
echo 'date "+%Y-%m-%d %H:%M:%S"' 
echo `date "+%Y-%m-%d %H:%M:%S"`

单双引号用于字符串拼接,echo 'user '"is "$USER
$()命令加参数
$(()) 表达式
${} 变量名引用, {}一般省略
""只能解析变量
```

环境变量

```sh
/etc/profile 	 #全局变量
~/.bash_profile  #用户变量

set #当前shell变量
env #当前用户变量
export unset

$SHELL $USER $PWD $HOME TERM=xterm-256color
```

+=	

```sh
a=1
let a+=1  等价 ((a++)) 
let b=$((a++))
echo $a $b
```

$$

```bash
#!/usr/bin/bash
$_ 上个命令最后一个参数
$? 程序退出码 0代表无异常
$$ 当前bash的pid
$0 当前bash的名字,或者脚本名字
$* $@ 命令行参数字符串合集包含空格 $* "11 22 33" $@ "11" "22" "33"
$# 变量长度 len()
$1-9 shell默认接受的前9个位置参数
$! 后台运行最后一个进程pid
$$ 当前进程pid

```

布尔判断

```sh
#  test 等价 [  ]
[ 1 == 2 ] 注意空格

-e 是否存在  
-rwx 测试权限

#大小比较
-eq 	 ==
-ne 	 !=
-lt/e    </=
-gt/e	 >/=

#与或非
-a &&
-o ||
!
```

分支

```bash
#!/usr/bin/bash
if [ $? -eq 0 ]; then    
elif test -e ${1}; then
else
fi


if [ $# -eq 1 -a -e $1 ]; then
    grep -n root $1
else
    echo "参数错误或文件不存在"
fi

while [ $i -le 10 ]; do
    let sum+=$i
    let i++
done
```

循环

```bash
for i in 1 2 3 4 5 6 7 8 9 10; do
done

sum=0
for ((i=1; i<=10; i++)); do
    let sum+=i
done

case $1 in
    1)
        echo "Monday"
        ;;
    7)
        echo "Sunday"
        ;;
    *)
        echo "Error"
esac
```

函数

```bash
function add() {
    let sum=$1+$2   # 这个是函数参数不要和脚本传参混肴
    echo $sum   # 相当 return
}

result=`add 100 200`
result=$(add 100 200)
echo "我才是脚本参数 $1 $2"
```

数组

```bash
# 定义数组的方式有两种   
declare -a weekday
weekday[1]=Monday
# 另外一种定义数组的方式：定义的同时进行赋值
weekday=(Monday Tuesday Wednesday Thursday Friday)

# 数组的取值
$arr 	是可迭代
$#arr	数组长度

unset arr[1] 删除元素
```









