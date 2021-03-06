# 第零章  计算机概论

### CPU的架构

精简指令集（Reduced Instruction Set Computer,RISC）：指令执行时间短，完成动作单一，执行性能强。

复杂指令集（Complex Instruction Set Computer,CISC）：指令数目多且复杂，指令长度不同。

### 芯片结构

北桥：负责链接速度较快的CPU、内存与显卡接口等。（现多将北桥内存控制器整合到CPU封装中）

南桥：负责链接速度较慢的硬盘、USB、网卡等。

### 运算与判断的CPU

##### CPU的工作频率（=外频*倍频）

外频：CPU与外部元件进行数据传输时的速度。

倍频：CPU内部用来加速工作性能的倍数。

##### 前端总线速度（Front Side Bus，FSB）

即内存能提供的数据量。

内存的工作频率也是由CPU内的内存控制器决定。

##### 超线程（Hyper-Threading，HT）

### 主板

##### 设备位置

I/O位置：类似于门牌号码。

IRQ：各门牌链接到CPU的专门路径。

##### CMOS与BIOS

CMOS：记录主板重要参数，如系统时间、CPU电压与频率、I/O位置与IRQ等。

BIOS：写入到主板上的flash或EEPROM程序，开机时执行，以载入CMOS参数，并尝试调用存储设备中的开机程序进入操作系统。

# 主机规划与磁盘分区
* 在Linux系统中，每个设备都被当成一个文件来对待，都有文件名。
* 磁盘文件名通常分为：实际SATA/USB设备文件名为/dev/sd[a-p]；虚拟机的设备可能为/dev/vd[a-p]。
*  磁盘的第一个扇区记录了两个重要的信息：（1）主要开机记录区 446Bytes （2）分表区：记录整颗硬盘分区的状态 64Bytes。
*  磁盘的MBR分区方式中，主要与延伸分区最多可以有四个，逻辑分区的设备文件名号码，一定从5开始。
*  某些操作系统要使用GPT分区时，必须要搭配UEFI的新型BIOS格式才可安装使用。
*  开机流程：BIOS->MBR->->boot loader->核心文件

# 第四章 首次登陆与线上求助
## 文字模式下指令的下达
### 初识指令
[dmtsai@study ~]$ command [-options] parameter1 parameter2 ...
* command为指令的名称，例如变换工作目录的指令cd等。
* 中括号[]并不存在于实际的指令中，而加入选项设置时，通常前加 - 号，如 -h；用选项完成全名，则前加 --，如 --help。
* [Enter]键代表一行指令的开始启动。
* 反斜线(\)+特殊字符可跳脱[Enter]使指令连续到下一行。
* Linux对大小写敏感。

### 基础指令的操作
* 显示日期与时间：date
* 显示日历：cal
* 计算器：bc

### 重要热键
#### [Tab]键
*  [Tab]接在一串指令的第一个词后，则为“命令补全”
*   [Tab]接在一串指令的第二个词后，则为“文件补齐”
*   若安装bash-completion软件，则在某些指令后使用 [Tab]，可进行“选项/参数的补齐”

#### [ctrl]-c
中断目前程序/指令

#### [ctrl]-d
键盘输入结束，可直接离开命令行（有时可替代exit）

#### [shift]+{[PageUP]|[PageDown]}
翻上/下页

## Linux系统的线上求助
### man page
DATE(代号)
* 1 使用者在shell环境中可以操作的指令或可执行文件
* 2 系统核心可调用的函数与工具等
* 3 一些常用函数和函数库
* 4 设备文件的说明
* 5 配置文件或者是某些文件的格式
* 6 游戏
* 7 惯例与协定等
* 8 系统管理员可用的管理指令

常用按键
* /string - 向下搜索string这个字串
* n,N - 在搜寻时，n继续下一个搜寻，N回到上一个
* q 结束此次 man page

# 第五章 Linux的文件权限与目录配置
## Linux文件权限
### Linux文件属性
  ```
  [dmtsai@study ~]$ su -  # 切换成root身份(可用exit返回)
  PassWord:
  Last login:xxx
  [root@study ~]# ls -al	#ls是list的意思，重在显示文件的文件名与相关属性。-al表示列出所有的文件详细的权限与属性
  drwxr-xr-x. 3    root    root      17  May   6   00:14  .config
  ```
  * 第一栏：文件的类型与权限（共10字符）
  	* 第一个字符代表文件属性
  		* [d]目录
  		* [-]文件
  		* [l]链接文件
  		* [b]可随机存储设备
  		* [c]一次性读取设备
  	* 剩下的字符三个为一组-[r]代表可读，[w]代表科协，[x]代表可执行，[-]代表无权限
  		* 第一组为“文件拥有权限”
  		* 第二组为“加入此群组之账号的权限”
  		* 第三组为“非本人且为其他人的权限”
  * 第二栏：有多少文件名链接到此节点
  * 第三栏：文件/目录的“拥有者账号”
  * 第四栏：文件的所属群组
  * 第五栏：文件的容量大小，默认单位是Bytes
  * 第六栏：文件的创建日期或最近修改日期
  * 第七栏：文件名
> 注：如果文件名之前多一个“."则该文件为隐藏文件。
> 注：root身份不受上述条件控制。
 

 ### 改变文件属性与权限
 #### 改变所属群组，chgrp
   ```
   [root@study ~]# chgrp [-R] dirname/filename ...
   -R:连同次目录下的所有文件、目录都进行更改
   [root@study ~]# chgrp users file.cfg
     ```
#### 改变文件拥有者，chown
   ```
   [root@study ~]# chown [-R]  账号名称  文件或目录
   [root@study ~]# chown [-R]  账号名称：群组名称   文件或目录
   ```

#### 改变权限，chmod
r:4 > w:2 > x:1
   ```
   [root@study ~]# chmod [-R] xyz 文件或目录	#xyz：为rwx属性数值的相加
      ```
### 权限意义
文件：r-读取文件内容，w-修改文件内容（不能删除），x-执行文件内容
目录：r-读到文件名，w-修改文件名，x-进入该目录的权限（key）

## Linux目录配置
### 配置的依据--FHS
* 不变的： /opt（第三方协力软件）
* 可变动的： /var/spool/news（新闻群组）
* 可分享的： /usr（软件放置处） /boot（开机与核心档） /var/mail（使用者邮件信箱） /var/lock（程序相关）
* 不可分享的： /etc（配置文件） /var/run（程序相关）

# 第六章 Linux文件与目录管理
## 目录与路径
### 相对路径与绝对路径
* 绝对路径：一定由根目录/写起，如：/usr/share/doc
* 相对路径：不是由/写起，如：../man

### 目录的相关操作
  ```
  .			代表此层目录
  ..		   代表上一层目录
  -			代表前一个工作目录
  ~			代表“目前使用者身份”所在的主文件夹
  ~account	 代表account这个使用者的主文件夹
    ```
   * cd:变换目录
   * pwd：显示当前目录	[-p]：显示实际工作路径
   * mkdir：创建一个新目录	[-p]：建立多层目录
   * rmdir：删除一个空目录 [-p]：连同下层的空目录一起删除

## 文件与目录管理
### 文件与目录的检视：ls
  ```
  [root@study ~]# ls [-aAdfFhilnrRSt]  文件名或目录名称..
  选项与参数：
  -a:全部的文件，连同隐藏文件一起列出来
  -d:仅列出目录本身，而不是列出目录内的文件数据
  -l:长数据串行出，包含文件的属性与权限等
  
    ```
### 复制、删除与移动文件
   ```
   [root@study ~]# cp  [options]  来源文件  目的文件
   选项与参数：
   -a:相当于 -dr  --preserve=all
   -i:若目标文件已存在，在覆盖前会先询问动作的进行
   -p:连同文件的属性（权限、用户、时间）一起复制过去，而非使用默认属性
   -r:递回持续复制，用于目录的复制行为
      ```
   ```
   [root@study ~]# rm [-fir] 文件或目录		#移除文件或目录
   选项与参数：
   -f:忽略不存在的文件，不会出现警告
   -i:在删除前询问
   -r:递回删除（危险！）
      ```
   ```
   [root@study ~]# mv [-fiu] source destination
   选项与参数：
   -f:若目标文件已存在，不询问直接覆盖
   -i:若存在则询问
   -u:若存在，且sourse比较新，才会更新
      ```

## 文件内容查阅
### 直接检视文件内容
#### cat
由第一行开始显示文件内容（以行为单位显示）
#### tac
由最后一行开始显示文字内容（[-b]:列出除空行外行号，[-v]列出一些看不出的特殊字符）
#### nl
添加行号打印
### 可翻页检视
#### more
* 最后一行显示目前百分比，并可输入指令
* 空格：向下翻一页
* Enter：向下翻一行
* /字串：向下搜索关键字
* :f：立即显示文件名以及目前显示的行数
* q：立即离开more
* b：往回翻页，只对文件有用
* /：到最后一行

#### less
* 空格：向下翻一页
* [pagedown]：向下翻页
* [pageup]：向上翻页
* /字串：向下搜索关键字
* ?字串：向上搜索关键字

### 数据截取
#### head
取出前面几行（数字代表行数）
[root@study ~]# head [-n number] 文件	
####tail
取出后面几行
-n:后面接数字，代表显示几行
-f:持续侦测后面所接的文件名，知道按下[ctrl]-c
### 非纯文本文件：od

   ```
[root@study ~]# od [-t TYPE] 文件
选项或参数：
a:利用默认字符输出
c:使用ASCII字符输出
d[size]:利用十进制来输出数据，每个整数占用 size Bytes	f-浮点型，o-八进制，x-十六进制
      ```

### 修改文件时间或创建新文件：touch
* modification time(mtime):内容数据变更该时间
* status time(ctime):状态改变更新该时间
* access time(atime):当文件的内容被取用时更新该时间

## 指令与文件的搜寻
### 指令文件名的搜寻
#### which （寻找“可执行文件”）
   ```
[root@study ~]# which [-a] command	#-a:将所有由PATH目录中可以找到的指令均列出

   ```
#### whereis（由一些特定的目录中寻找文件名）
   ```
[root@study ~]# whereis [-bmsu] 文件或目录名	#-b:只找binary格式的文件

   ```
#### locate/updatedb
   ```
[root@study ~]# locate [-ir] keyword
   ```