---
title: docker MySQL主从复制
date: 2018-05-19 22:53:33
tags: [docker, mysql]
---


master01.cnf配置

[mysqld]
log-bin=mysql-master01-bin # 使用binary logging，mysql-master01-bin是log文件名的前缀
server-id=1 # 唯一服务器ID，非0整数，不能和其他服务器的server-id重复

创建master01容器

docker run -d --name master01 -v /mysql/data/master01:/var/lib/mysql -v /mysql/master01:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=xzz520 -p 3306:3306 mysql:5.7


show master status; # 查看master的状态

SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
GRANT REPLICATION SLAVE ON *.* to 'backup'@'%' identified by '123456';
 

SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
GRANT REPLICATION SLAVE ON *.* to 'backup'@'%' identified by '123456';


创建一个用户
CREATE USER 'backup'@'X.X.X.X' IDENTIFIED BY '123456';

赋予“REPLICATION SLAVE”的权限
GRANT REPLICATION SLAVE ON *.* TO 'user'@'X.X.X.X' IDENTIFIED BY '123456';
grant replication slave on *.* to slave@112.74.34.233 identified by 'slave';

slave01.cnf配置

[mysqld]
log-bin=mysql-slave01-bin 
server-id=2

创建slave01容器

docker run -d --name slave01 -v /mysql/data//slave01:/var/lib/mysql -v /mysql/slave01:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=xzz520 -p 3306:3306 mysql:5.7

在slave01中配置与master01相关的内容

SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));

change master to master_host='144.202.114.103',master_port=3306,master_user='slave',master_password='slave',master_log_file='mysql-master01-bin.000003', master_log_pos=2720;

change master to master_host='192.168.10.223',master_port=3306,master_user='backup',master_password='123456',master_log_file='mysql-master01-bin.000003', master_log_pos=154;
 
master_host是宿主机的IP（ifconfig查到的操作系统IP，不是容器的IP，一定不能搞错） 
master_port是主服务器的映射到3306的端口（默认3306） 
master_user为创建的备份用户 
master_password备份用户的密码 
master_log_file和master_log_pos则是show master status列表里面的两个值，分别是mysql日志名和起始备份位置 

start slave;

show slave status;

如果是Waiting for master to send event则说明主从复制成功了，若是Connecting to master肯定是配置错误，楼主就错了几次，重删除配置一下就行，这就是容器的好处，删除的代价很小，短时间就可以恢复。如果是Waiting for master to send event，对主库的增删改查从库都会同步修改。

删除用户
drop user test;


查看权限

show grants ;  #查看自己的权限

show grants for backup@'xxx.xxx.xx.xx'; #查看其他用户的权限。

select * from mysql.user where user='backup';


　二、常用命令

　　1、显示当前数据库服务器中的数据库列表：

　　mysql> SHOW DATABASES;

　　2、建立数据库：

　　mysql> CREATE DATABASE 库名;

　　3、建立数据表：

　　mysql> USE 库名;

　　mysql> CREATE TABLE 表名 (字段名 VARCHAR(20), 字段名 CHAR(1));

　　4、删除数据库：

　　mysql> DROP DATABASE 库名;

　　5、删除数据表：

　　mysql> DROP TABLE 表名;

　　6、将表中记录清空：

　　mysql> DELETE FROM 表名;

　　7、往表中插入记录：

　　mysql> INSERT INTO 表名 VALUES ("hyq","M");

　　8、更新表中数据：

　　mysql-> UPDATE 表名 SET 字段名1='a',字段名2='b' WHERE 字段名3='c';

　　9、用文本方式将数据装入数据表中：

　　mysql> LOAD DATA LOCAL INFILE "D:/mysql.txt" INTO TABLE 表名;

　　10、导入.sql文件命令：

　　mysql> USE 数据库名;

　　mysql> SOURCE d:/mysql.sql;

　　11、命令行修改root密码：

　　mysql> UPDATE mysql.user SET password=PASSWORD('新密码') WHERE User='root';

　　mysql> FLUSH PRIVILEGES;