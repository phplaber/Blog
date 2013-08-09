### 「20 Useful Commands for Linux Newbies」笔记 ###

作为一个Linux初学者，掌握一些常用的Linux命令很有帮助。这些命令能帮助你完成绝大多数基础的工作，同时，又能培养你在Linux上工作的感觉，这种感觉对日后的进阶很有帮助。下面是我在看「20 Useful Commands for Linux Newbies」一文所做的摘要，方便日后查阅。

原文链接：[20 Useful Commands for Linux Newbies](http://www.tecmint.com/useful-linux-commands-for-newbies/)

![Linux mascot](http://www.lingcc.com/wp-content/uploads/2013/03/Linux.jpg)

******

#### 1，``ls`` ####

「ls」代表 **List Directory Contents** ，意思是：罗列当前目录内容。

「ls -l」会以更加详细的信息罗列目录内容，包括文件的权限，硬连接数，拥有者等信息。

	root@tecmint:~# ls -l

	total 40588
	drwxrwxr-x 2 ravisaive ravisaive     4096 May  8 01:06 Android Games
	drwxr-xr-x 2 ravisaive ravisaive     4096 May 15 10:50 Desktop
	drwxr-xr-x 2 ravisaive ravisaive     4096 May 16 16:45 Documents
	drwxr-xr-x 6 ravisaive ravisaive     4096 May 16 14:34 Downloads
	drwxr-xr-x 2 ravisaive ravisaive     4096 Apr 30 20:50 Music
	drwxr-xr-x 2 ravisaive ravisaive     4096 May  9 17:54 Pictures
	drwxrwxr-x 5 ravisaive ravisaive     4096 May  3 18:44 Tecmint.com
	drwxr-xr-x 2 ravisaive ravisaive     4096 Apr 30 20:50 Templates

「ls -a」罗列包括以点号开头的隐藏文件的所有文件。

#### 2，``lsblk`` ####

「lsblk」代表 **List Block Devices** ，意思是：罗列块设备。默认会以树状（tree-like）形式输出，加上-l参数后，以列表形式输出。

#### 3，``md5sum`` ####

「md5sum」代表 **Compute and Check MD5 Message Digest** ，意思是：计算和校验MD5消息摘要，即：hash之意。用来校验文件是否已更改。

#### 4，``dd`` ####

「dd」代表 **Convert and Copy a file** ，意思是：复制文件。执行该命令花费的时间由文件大小，类型以及USB闪存读写速度决定。

	root@tecmint:~# dd if=/home/user/Downloads/debian.iso of=/dev/sdb1 bs=512M; sync

#### 5，``uname`` ####

「uname」代表 **Unix Name** 。执行命令会输出kernel类型。想要了解平台更多的信息，加上-a参数，即：「uname -a」。

	root@tecmint:~# uname -a

	Linux tecmint 3.8.0-19-generic #30-Ubuntu SMP Wed May 1 16:36:13 UTC 2013 i686 i686 i686 GNU/Linux

tecmint为机器的节点名称，i686为处理器体系架构。

#### 6，``history`` ####

「history」代表 **History (Event) Record** 。执行该命令将打印之前所有命令执行的记录。配合「Ctrl + R」可以迅速找到指定的执行命令。

#### 7，``sudo`` ####

「sudo」代表 **super user do** ，意思是：做超级用户做的事。该命令允许普通用户使用超级用户的特权。记住一条忠告：

>  To err is human, but to really foul up everything, you need root password.

#### 8，``mkdir`` ####

「mkdir」代表 **Make directory** ，意思是：创建目录。目录存在时会报错，类似"mkdir: cannot create directory `syn': File exists"。注意：在Linux的世界，一切皆文件。

#### 9，``touch`` ####

「touch」代表 **Update the access and modification times of each FILE to the current time** ，意思是：访问文件并修改时间为当前时间。执行该命令会创建新文件，如果文件存在，则清空文件内容。

#### 10，``chmod`` ####

「chmod」代表 **change file mode bits** ，意思是：更改文件权限。一个文件有读（Read(r)=4），写（Write(w)=2）和执行（Execute(x)=1）三种类型的权限。通过设置这三种权限，给owner，usergroup和world分配不同的文件访问权限。如：「rwxr-x--x   abc.sh」表示：owner对abc.sh具有read,write,execute的权限，usergroup对abc.sh具有read,execute的权限，world对abc.sh具有execute的权限。

为了给owner，usergroup和world分别赋予对abc.sh有读写执行(4+2+1)，仅执行(1)，仅执行(1)的权限，可以执行如下命令：

	root@tecmint:~# chmod 711 abc.sh

#### 11，``chown`` ####

「chown」代表 **change file owner and group** ，意思是：更改文件的所有者和组。语法：chown owner:group file。

#### 12，``apt`` ####

「apt」代表 **Advanced Package Tool** ，意思是：高级包工具。apt比yum更高级和智能。

#### 13，``tar`` ####

「tar」代表 **Tape Archive** ，意思是：打包文件。

#### 14，``cal`` ####

「cal」代表 **Calendar** ，用来展现当月日历，或任何过去和将来某月的日历。如：想知道1988年3月27日是星期几，可以通过执行命令：「cal 03 1988」来查看。

#### 15，``date`` ####

「date」代表 **Date** ，用来展现当前日期和时间。通过--set参数可以修改日期和时间。

#### 16，``cat`` ####

  ...（待补全）

#### 17，``cp`` ####

「cp」代表 **Copy** ，复制文件从此处到他处。支持通配符。

#### 18，``mv`` ####

「mv」代表 **Move** ，移动文件从此处到他处。支持通配符。

#### 19，``pwd`` ####

「pwd」代表 **print working directory** ，意思是：打印当前工作目录。当“迷路”时，执行这个命令。

#### 20，``cd`` ####

「cd」代表 **change directory** ，意思是：切换当前目录。

（完）