### Windows平台下安装MongoDB

1，在官方网站下载32-bit二进制的MongoDB版本。这里：[MongoDB下载页](http://www.mongodb.org/downloads "The download page of mongodb")。

2，解压下载的安装包到硬盘某一位置，如：D:\mongodb，在目录下新建一个data目录和log.txt文件，分别用来储存数据库文件和记录MongoDB日志。

3，开启命令行窗口，切换当前目录到“D:\mongodb\bin”，执行如下命令安装MongoDB。

    mongod.exe --install --logpath=D:\mongodb\log.txt --dbpath=D:\mongodb\data

4，启动MongoDB服务

    net start mongodb

5，进入MongoDB的shell环境

    mongo

6，为了更方便的使用和管理MongoDB，将MongoDB的二进制目录路径添加到系统变量Path值中。