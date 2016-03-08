UEFI折腾
===============

UEFI安装OS过程
---------------

1、下载镜像
2、初始化硬盘为GPT
3、解压镜像到Ｕ盘，或者压缩出一个分区，解压到该分区。该分区必须为fat32
4、开始安装。

注意：
1、UEFI只能在fat32分区启动；所以所有的EFI（ESP）分区均为fat32
2、ESP分区为100M左右，如果双系统，要在200M以上。
3、


转换分区为GPT——windows命令行
------------------------------

1、在安装界面shift+F10调出命令行
2、
# 进入diskpart命令
diskpart
list disk
select disk 0
# 注意这一步是把所有分区内容都删掉，抹去所有数据
clean   
# 转换分区类型
covert gpt  /  mbr
# efi即esp分区，如果是双系统，建议200m以上
create partition efi size=100  
create partition msr size=128
create partition primary size=50000 


重建UEFI引导
------------------------------

### 使用场景 

UEFI分区损坏 ghost安装的系统

### efi启动过程

uefi bios启动时，自动查找硬盘下esp分区的bootx64.efi，然后由bootx64.efi引导efi下的bcd文件，由bcd引导指定系统文件（一般为c:\windows\system32\winload.efi）

### 修复命令

环境： 64位 win7 或 win8 PE
bcdboot c:\windows /s o: /f uefi /l zh-cn

c:\windows  硬盘系统目录，根据实际情况修改
/s o:       指定esp分区所在磁盘，根据实际情况修改
/f uefi     指定启动方式为uefi
/l zh-cn    指定uefi启动界面语言为简体中文