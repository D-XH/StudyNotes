# [Shell脚本](https://www.runoob.com/linux/linux-shell-func.html)

## 变量

```bash
var_name="value"			# 定义变量时不加$
LD_LIBRARY_PATH="/bin/"		# 用大写代表常量

PORT=${PORT:-29500}			# 如果PORT变量已经被定义并且非空
								#那么它的值保持不变。
							# 如果PORT变量没有被定义或者为空
								#那么PORT的值将被设置为29500

# 使用语句赋值
for file in `ls /etc`
for file in $(ls /etc)

# 变量的使用
echo $var_name
echo ${var_name}

# 重新定义
var1='ha'
var2='xi'

# 删除变量
unset var_name			# 不能删只读变量

# 只读变量
readonly var_name		# 不能重新定义了

# 定义类型
declare -i var_name=42	# 定义为整型，非整型会转换

# 特殊变量
$PATH		# 环境变量
$0			# 脚本名
$1 $2 $n	# 脚本接收的第n个参数
$#			# 参数数量
$*			# 用一个单字符串表示所有参数
$@			# 同$*，但是每个参数用""包裹
$!			# 当前进程的ID
$-			# 显示Shell使用的当前选项，与set命令功能相同。
$?			# 上一个命令的退出状态。0表示正常退出

# 字符串
str='sss'		# 单引号中的内容会原样输出。变量不能放在里面。
				# 相当于字符常量。
str="sss"		# 双引号中可以有变量，也可以有转义字符

str2=$str"bbb"			# 拼接。不需要+号
num=${#str}				# 字符串长度，用#获取。
substr=${str:1:2}		# 提取子字符串。var_str:start:num
`expr index "$str" sc`	# 查找s或c的第一个位置

# 数组
arr=(value1 value2 value3 value4)
arr[1]=value1

${arr[2]}	# 读取数组
${arr[@]}	# 读取所有元素

declare -A associative_arr
associative_arr['cnt']=1
```

## 注释

```bash
# 单行注释

:<<'COMMENT'
多行注释
:是空命令
<<'COMMENt'是开启Here文档
COMMENt

:<<'
多行注释
精简版
'

:'
多行注释
最精简版
'
```

## printf

```bash
printf format-string [args]

printf "%d %s" 1 "ha"	# format-string使用单引号、双引号或不使用都可以

printf "%s" ss ff gg	# 重复使用格式输出

printf %d %s			# 参数为空时，%s默认为NULL，%d默认为0

'%-4.2f'	# - 表示左对齐
			# 4 表示4位长度
			# 2 表示保留2位

'\a'	# 警告字符
'\b'	# 光标退格
'\f'	# 换页
'\t'	# 制表符
'\v'	# 垂直制表符

# 进度条
for i in $(ls)
do
	echo -n "[$j / $res] $video_name" $'\r'	# -n 表示不加换行符， $'\r'表示回车，光标回到行首
	((j++))
done

```

## 运算符

```bash
# bash不支持数学运算
# expr 一款简单的运算工具。
var=`expr 2 + 2`	# 之间的空格不可少
var=`expr 2 \* 2`	# *前必须有\

[ $a -lt 20 -o $b -gt 100 ]	# 或预算
[ $a -lt 20 -a $b -gt 100 ]	# 与运算

[ -z $var ]		# 字符串长度是否为0，0则True
[ -n "$a" ]		# 字符串长度是否不为 0

[ -b $file ]	# 是否是块设备文件
[ -c $file ]	# 是否是字符设备文件
[ -d $file ]	# 是否是目录
[ -f $file ]	# 是否是普通文件
[ -g $file ]	# 是否设置了 SGID 位
[ -k $file ]	# 是否设置了粘着位(Sticky Bit)
[ -p $file ]	# 是否是有名管道
[ -u $file ]	# 是否设置了 SUID 位
[ -r $file ]	# 
```

