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
