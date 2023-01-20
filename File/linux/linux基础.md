---
title : Linux笔记
category : 学习
date : 20221222_周四
update : 
tags : 
- linux
- shell
- vim
---

## bash 基础

1. `bash`简介

   - `bash`是一个命令处理器,运行在文本窗口,能执行用户输入的命令

   - `bash`还能从文件中读取 linux 命令,称之为脚本

   - `bash` 支持通配符、管道、命令替换、条件判断、循环等逻辑控制语句

2. `bash`特性

   - _**{}**_ 解构

     - 离散不连续

     ```sh
     $ echo test{1,3}

     test1 test3
     ```

     - 闭区间

     ```sh
     $ echo test{01..03}

     test01 test02 test03
     ```

     - 闭区间带步长

     ```sh
     $ echo test{01..05..2}

     test01 test03 test05
     ```

   - _**alias**_

     - 不带参数,查看已创建的别名

     - _order = “some order -option”_ 创建别名

     ```sh
     $ alias rm='echo 你没有rm权限'
     $ rm a.txt

     你没有rm权限 a.txt
     ```

     - _**unalias** order_ 取消别名

   - 光标移动快捷键

     - _ctrl + a_ 移动到行首

     - _ctrl + e_ 移动到行尾

     - _ctrl + u_ 删除光标之前的字符

     - _ctrl + k_ 删除光标之后的字符

     - _ctrl + l_ 清空屏幕终端内容,同 _**clear**_

   - _**tab**_ 补全

     - `$PATH` 环境变量中存在的命令补全

     - 文件路径补全

   - 替换

     - _**${}**_ 变量替换

     ```sh
     # 初始化数组,空格分隔,可检索单双大中小括号探索
     $  a=(1 2 3)
     $  echo ${a[1]}

     2
     ```

     - _**$()**_ 命令替换

     ```sh
     $ echo $(date +%Y-%m-%d)

     2022-12-24
     ```

     - `linux`中三种引号的区别

       - 反引号:同 _**$()**_ 命令替换,先执行反引号内的命令,使用结果替换掉反引号处的内容

       - 双引号:保护特殊元字符和通配符不被 shell 解析,但允许变量和命令以及转义符的解析

       - 单引号:不允许任何变量、元字符、通配符、转义符被 shell 解析，均被原样输出

     ```sh
     $ echo `date +%D`
     12/24/22

     $ echo "$(date +%D)"
     12/24/22

     $ echo '$(date+%D)'
     $(date+%D)
     ```

## linux 常用命令

1. _**cd**_

   - _dir_ 切换目录

   - _~_ 切换至根目录

   - _.._ 切换至上级目录

   - _-_ 切换到之前的目录,因为我们可能是从上级的上级切换过来的

   - _._ 当前目录,比如 cp dir/file . 将 dir 目录下 file 文件拷贝到当前目录

2. _**pwd**_

   - _-L_ 可省略,默认打印逻辑上的工作目录

   - _-P_ 打印物理上的工作目录

3. _**ls**_

   - 查看当前路径下目录与文件列表

   - _-a_ 包含隐藏目录及文件

   - _-l_ 查看目录与文件详情,该命令也常可简写成`ll`

   - _-d_ 只查看当前路径下的目录列表

4. _**mkdir**_

   - 新建目录

   - _-p_ 创建多级目录,确保上级目录存在,若不存在则先新建上级目录再创建下级目录

5. _**touch**_

   - 对已存在的文件,修改其时间属性

   - 若文件不存在则新建文件

   - 搭配 _**{}**_ 解构批量新建文件

     ```sh
     $ touch test{1..5..2}.txt
     $ ls | grep '^test'

     test1.txt
     test3.txt
     test5.txt
     ```

6. _**echo**_

   - 在终端打印输出

   - 输出重定向

     - _**>**_ 覆写输出重定向到文件

     - _**>>**_ 追加重输出定向到文件

     - 输出重定向到文件时,若文件不存在,则新建文件

   - _-e_ 启用转义字符的解析

     ```sh
     $ echo -e "hello echo.\n\t-mt"
     hello echo.
           -mt
     # 搭配模式匹配字符使用
     $ echo *.sh
     mt.sh  obsidian.sh
     ```

7. 文件浏览

   - _**cat**_ 一次性在终端显示文件的所有内容

     - _-n_ 带行号显示

     - _file1 file2_ 连接显示多个文件

   - _**less**_ 分页显示文件内容

   - _**tac**_ 从后往前 _**cat**_

   - _**head** -n num_ 显示前 num 行

   - _**tail** -n num_ 显示后 num 行

   - 强大的第三方工具`vim`等

8. _**rm**_

   - _-f_ 强制删除文件,危险命令

   - _-i_ 删除文件前进行确认

   - _-r_ 删除目录,将目录及以下内容递归逐一删除,删库跑路

9. _**mv**_

	- 重命名文件:_**mv**_ 源文件 目标文件
	
	- 移动文件:_**mv**_ 源文件 目标目录
	
		- _-i_ 若存在同名文件,向用户确认是否覆盖
	
		- _-f_ 直接覆盖不询问
	
		- _-b_ 目标目录中同名文件创建副本,再行覆盖
	
		- _-u_ 源文件最后更改时间新于目标目录中同名文件,或目标文件不存在时,才移动文件
	
		- _-t_ 移动多个文件到目标目录时,目录在前: _**mv** -t_ 目标目录 源文件1 源文件2
	
	- 移动目录(与 _**rm**_, _**cp**_ 目录操作不同,无须 _-r_ ):_**mv**_ 源目录 目标目录

10. _**cp**_

	- 复制文件到新目录下: _**cp**_ 源文件 目标目录
	
		- _-i_ 若存在同名文件,向用户确认是否覆盖
	
		- _-f_ 直接覆盖不询问
	
		- _-I_ 不做复制,只是链接文件
	
		- _-d_ 复制时保留链接
	
		- _-p_ 除复制源文件的内容外,还复制其修改时间及访问权限
	
	- 复制文件到新目录下并重命名为目标文件: _**cp**_ 源文件 目标目录/目标文件
	
	- 复制目录到新目录下: _**cp** -r_  源目录 目标目录
	
	- _-a_ 在复制目录时保留链接、文件属性,并递归的复制目录,等同于 _-dpr_ 选项


## 进阶命令

1. _**grep**_

   - 文本搜索工具,根据指定模式对目标文件匹配检查, 输出匹配到的行

   - _-i_ 忽略字符大小写

   - _-E_ 使用扩展正则模式

   - _-n_ 带行号显示

   - _-o_ 仅显示匹配到的字符串, 而非输出匹配到字符串的行

   - _-v_ 反选,显示未匹配到的行

   - _-c_ 统计匹配到的行数(count)

2. _**sed**_

   - 流编辑器,逐行读取内容到模式空间中进行操作、过滤、转换文本的强大工具

   - 语法: _sed \[选项\] \[sed 匹配范围\] \[sed 命令符\] \[数据或文件\]_

     - _**选项**_

       - _-n_ 取消 sed 的默认全部输出,常与 _p_ 命令符一起使用,只输出那些匹配到的行

       - _-i_ 直接修改原文件,不使用 _-i_ 参数时,默认修改的是模式空间的内存数据

       - _-e_ 多次编辑,类似管道操作符功能

       - _-r_ 支持正则扩展

     - _**sed 匹配范围**_

       - 默认空地址,全文匹配

       - 单地址,指定一行进行匹配

       - _/pattern/_ 指定模式匹配到的行

       - 区间匹配

         - 10,20 表示第十至 20 行

         - 10,+5 表示从第十行开始往下 5 行

         - _/pattern1/_,_/pattern2/_ 模式匹配限定区间

       - 步长

         - 3~4 表示从 1 开始 2 个步长即 3,7,11...

     - _**sed 命令符**_

       - _p_ 打印匹配行的内容,常与 _-n_ 参数一起使用

       - _i_ 在匹配行前插入一行或多行内容

       - _a_ 在匹配行后追加一行或多行内容

       - _d_ 删除匹配行

       - _s/正则/替换内容/g_ 正则替换,其中 g 表示全局匹配

   ```sh
   # 打印mt.sh中能匹配到linux的行
   $  sed -n "/linux/p" mt.sh
   # 删除空行
   $  sed -i "/^$/d" mt.sh
   ```

## shell 脚本

1. 批量修改目录

```sh
#!/bin/bash

cd desktop && mkdir demo && cd demo
mkdir work01day
mkdir work02day
mkdir work03day
mkdir work04day

# shell中for循环,以及使用$引用变量
## 正则中?表示1个字符,*表示任意多个字符，这里表示work后面跟任意多个字符
for i in work*
do
## mv重命名,#掐头%去尾
j=${i#work}
mv ${i} week${j%day}
done



#!/bin/bash

# 克隆地址
addr='https://github.com/WangyiG/git-demo-repo20230108.git'

# 在桌面创建demo目录
cd desktop && mkdir demo && cd demo

# 在demo目录下clone2个路径来模拟不同本地仓库用户
git clone ${addr} mydir1
git clone ${addr} mydir2


# 反复横跳循环提交
for i in mydir{1,2,1}
do
    cd ${i}
    echo "\n  ${i} commit  " >> README.md
    git commit -am "${i} commit"

    # 检查是否有拒绝推送,一个不严谨的判断
    if [[ "$(git push origin main 2>&1 | sed -n "/rejected/p")" != "" ]]
    then
	
	# 意识到忘记pull了    
        git pull --rebase origin main
        
	# 解决conflict,不严谨的处理成均保留
        if [[ "$(sed -nr "/>{5}/p" README.md)" != "" ]]
        then
            sed -ir "/[<=>]\{5,\}/d" README.md
        fi

	# 继续推送
	git commit -am "pull behind push ${i} commit"
	git rebase --continue
	git push origin main
    fi
	

    # 远程仓库,网络波动啥的
    sleep 5

    # 切换回demo文件夹
    cd ..

done 




```

2. _**sleep**_

	- _n_ 默认以秒为单位睡眠

	```sh
	#! /bin/bash
	date 
	echo '等待3秒'
	sleep 5
	date
	```

3. _**read**_

	- _-p_ 带注释的`input`

	```sh
	#! /bin/bash
	read -p '请输入x的值:' x
	echo ${x}
	```

4. 运算符

	- 算术运算符
	
		- _+-*/_ 加减乘除
	
		- _%_ 取余
	
		- _$[]_ 包裹运算

		```sh
		#!/bin/bash
		x=2
		y=3
		sum=$[$x+$y]
		echo $sum
		```
 
	- 逻辑运算符
	
		- _&&_ 并
	
		- _||_ 或
	
		- _!_ 非
	
	- 比较运算符(有坑,待挖)
	
		- _== , eq_  相等
	
		- _!= , ne_ 不等
	
		- _<= , -le_ 小于等于
	
		- _>= , -ge_ 大于等于
	
		- _< , -lt_ 小于
	
		- _> , -gt_ 大于

#### 判断

1. _**if**_

```sh
#!/bin/bash
read -p "Please input a num:" num

if (($num>5))
then
    echo "$num is a large number"
elif (($num<5))
then
    echo "$num is a small number"
else
    echo "$num just so so"
fi
```

2. _**case**_

```sh
#!/bin/bash
read -p "Please input your choose Y/N:" y

case $y in
    "Y")
        echo "your choose is yes"
		;;
    "N")
        echo "youe choose is no"
		;;
    *)
        echo "Please choose with Y or N"
		;;
esac
```

#### 循环

1. _**for**_

```sh
#!/bin/bash
for i in `seq 1 3`
do
    echo $[$i*2]
done
```

2. _**while**_

```sh
#!/bin/bash
n=3
# 比较运算使用空格,赋值与算术运算不能有空格
while [ $n -ge 1 ]
do
    echo $[$n*2]
    n=$[$n-1]
done
```

3. 几种不同的循环跳出方式
	
	- _continue_ 跳出本值循环,继续后值循环
	
	- _break_ 跳出循环体
	
	- _exit_ 退出脚本

#### 函数

1. 函数定义与调用

```sh
#!/bin/bash

# function关键字可省略
# 直接使用函数名调用,不必带小括号
function myfunc()
{
    echo `date +"%H:%M:%S"`
}

for i in `seq 1 3`
do
    myfunc
    sleep 2
done
```

2. 系统变量

	- _$0_ `shell`脚本的绝对路径
	
	- _$#_ 代表脚本后跟的参数个数
	
	- _$?_ 显示最后命令的退出状态 ,0表示正确运行,其他值表明有误
	
	-  _$*_ 以一个单字符串显示所有向脚本传递的参数,如\" $* \"双引号包裹则以"$1 $2 … $n"的形式输出所有参数
	
	-  _$@_ 以一个单字符串显示所有向脚本传递的参数,如\" $@ \"双引号包裹则以"$1"  "$2 \" …  \" $n\" 的形式输出所有参数
