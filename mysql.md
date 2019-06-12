1. mysql 创建用户
	
	A:  create user 用户名 identified by '密码';
	
		1.允许本地访问的用户（127.0.0.1）
			create user zhrt@localhost identified by '123456';  

		2.允许外网IP访问的用户
			create user 'zhrt'@'%' identified by '123456'; 
		
		新创建的用户，默认情况下是没有任何权限的。
	
2. 给用户分配权限
	
	A: 	grant 权限 on 数据库.数据表 to '用户' @ '主机名';
	
		例1：给 heqq 分配所有的权限
	
			grant all on *.* to 'heqq'@'%';
		
			这个时候 heqq 就拥有了 所有权限了
			
		例2：
			2.1: 授予用户在本地服务器对该数据库的全部权限
			
			grant all privileges on dbname.* to heqq@localhost identified by '123456';

			2.2: 授予用户通过外网IP对于该数据库的全部权限
			
			grant all privileges on dbname.* to 'heqq'@'%' identified by '123456';  
			
			2.3: 授予用户通过外网IP对于该数据库的部分权限
			
			grant select,insert,update,delete,create,drop  privileges on dbname.* to 'heqq'@'%' identified by '123456';  

	B:  刷新权限
	
		flush privileges;  
		
		
3:  收回 权限,一般指有root用户才具有该权限

	A: revoke 权限 on  数据库.数据表 from '用户'@'主机名';
	
		例1：收回 heqq 的所有权限
		
			revoke all on *.* from 'heqq' @'%';
			
		例2：
			2.1: 收回用户在本地服务器对该数据库的全部权限
			
			revoke all privileges on dbname.* to heqq@localhost identified by '123456';

			2.2: 收回用户通过外网IP对于该数据库的全部权限
			
			revoke all privileges on dbname.* to 'heqq'@'%' identified by '123456';  
			
			2.3: 收回用户通过外网IP对于该数据库的部分权限
			
			revoke select,insert,update,delete,create,drop  privileges on dbname.* to 'heqq'@'%' identified by '123456';  
			
			
			
4. 修改指定用户密码

  　　mysql>update mysql.user set password=password('新密码') where User="heqq" and Host="localhost";
  
  　　mysql>flush privileges;	
  
  
5、删除用户

 　　mysql>Delete FROM user Where User='heqq' and Host='localhost';
 
 　　mysql>flush privileges;
			
			
			
6、创建数据库

	create database 数据库名 DEFAULT CHARSET utf8 COLLATE utf8_general_ci;  
	
	
7、数据库表操作

	7.0 创建数据库
		create table table_name(
		   id INT NOT NULL AUTO_INCREMENT,
		   title VARCHAR(100) NOT NULL default '' comment '标题',
		   author VARCHAR(40) NOT NULL default '' comment '作者',
		   submission_date DATE,
		   PRIMARY KEY ( tutorial_id )
		)ENGINE=INNODB  DEFAULT CHARSET=utf8;  #指定引擎和存储字符类型
	
	
	7.1 修改列类型   #注意：不是任何情况下都可以去修改的，只有当字段只包含空值时才可以修改。
	
		alter table 表名 modify 列名  varchar(4);
		
	7.2 增加列
		alter table 表名 add 列名 varchar(12) not null default '' comment '新添加列' after `列名`;
		
	7.3 删除列
		alter table 表名 drop 列名;
		
		alter table 表名 drop column 列名;
		
	7.4 列改名
		alter table 表名 change 列名 新列名  varchar(18);
	 
	7.5 更改表名
		alter table 表名 rename 新表名;
		
		rename table 新表名 to 表名;
		
	7.6 添加外键约束
		alter table 表名 add constraint foreign key 外键表名(外键表id) references 主键表名(主键表id);
		
	7.7 删除外键约束
		alter table 主键表名 drop  foreign key 外键约束名;

	7.8 删除主键约束
		alter table 表名 modify id int;
		alter table 表名 drop  primary key;
	
	7.9 删除唯一约束
		alter table 表名 drop index 列名;
		
	7.10 添加唯一键
		ALTER TABLE `表名称` ADD UNIQUE (`列名`)
		
	7.11 增加索引
		ALTER TABLE `表名称` ADD INDEX ( `列名` )
		
	7.12 查看表的字段信息
		desc 表名;
		show columns from `表名`；
		
		SHOW FULL COLUMNS  FROM `表名称`; # 查看字段的详细信息

	7.13 查看表的所有信息
		show create table `表名`;
			
			
	7.14 修改数据库的编码格式
		mysql>alter database <数据库名> character set utf8;
		
	7.15 修改数据表格编码格式
		mysql>alter table <表名> character set utf8;
		
	7.16 修改字段编码格式
		mysql>alter table <表名> change <字段名> <字段名> <类型> character set utf8;

		mysql>alter table user change username username varchar(20) character set utf8 not null;
			
			
	7.17 查看数据库编码格式
		mysql> show variables like 'character_set_database';
	
	7.18 查看数据表的编码格式	
		mysql> show create table <表名>;
			
	7.19 看mysql支持哪些存储引擎:
		mysql> show engines;
	
	7.20 看mysql当前默认的存储引擎:
		mysql> show variables like '%storage_engine%';

		
		
		
		
		
		
		## 命令行启动关闭重启MySQL服务

在命令行终端启动 MySQL 非常方便，下面大概介绍几个平台通过命令启动服务的方法。

### 查看服务是否启动

```
# 还可以这么查看，MySQL服务器是否启动
ps -ef | grep mysqld

# 查看服务运行的状态
service mysqld status
```

### Mac OS X 下命令操作

在 Mac 系统下操作起来就非常方便了。安装完之后就可以在终端上运行全局命令 mysql.server 命令，假设这个命令没有，你在系统的MySQL安装目录中找到 mysql.server 命令，运行它是一样的效果。

```
mysqld start
mysql.server start    # 1. 启动
mysql.server stop     # 2. 停止
mysql.server restart  # 3. 重启
```

当你安装过 MySQL 并没有找到 mysql.server 命令，那这时你需要找到安装目录中的 mysql.server 命令工具了，如 `sudo /usr/local/mysql/support-files/mysql.server start`

### Linux 下命令操作

Linux生态系统中对服务的操作有点区别。其实在 Mac 系统下也可以直接 `mysqld start` 来启动服务。

1. 启动：`service mysqld start`
2. 停止：`service mysqld stop`
3. 重启：`service mysqld restart`
4. 查看状态：`service mysqld status`
5. 查看状态：`systemctl status mysqld.service`

systemctl是一个systemd工具，主要负责控制systemd系统和服务管理器，在 Linux 系统中可以通过它来启动 mysql 服务。
		
		
