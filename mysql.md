# mysql简单使用

## 安装mysql

先配置环境变量  
然后以管理员权限进入mysql目录执行命令

-----------

1. 初始化mysql user指定的是创建的文件所属的用户
> mysqld --initialize-insecure --user=mysql

2. 安装mysql
> mysqld -install mysql

3. 登录mysql;第一次登录不需要密码
> mysql -u root -p

4. 修改root密码
> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

5. 刷新权限
> FLUSH PRIVILEGES;

6. 重启mysql服务
> net stop mysql
>
>net start mysql

7. 登录mysql
> mysql -uroot -p123456

## 卸载

- 停止mysql服务
> net stop mysql

- 卸载mysql
> mysqld -remove mysql



## 常用操作

- 显示所有数据库
> show databases;

- 切换操作的数据库
> use sys

- 显示当前正在操作的数据库
> select database();

- 创建数据库
> create database  if not exists name

- 删除数据库
> drop database if exists name;

- 查询表结构
> desc table;

- 创建表

**例如:**
```

create table movie (
    id int not null  PRIMARY KEY  AUTO_INCREMENT, 
    name varchar(100) not null,
    imgsrc varchar(1000) ,
    type  varchar(100) default '其他',
    area  char(10) default '其他',
    year  char(10) default '未知',
    mdesc  varchar(1000) default '暂无描述',
    sell  double(10, 0) default 0,
    love  double(10, 0) default 0,
    share double(10, 0) default 0,
    up    double(10, 0) default 0,
    down  double(10, 0) default 0,
    issale char(1) default 0,
    cuser varchar(100) null,
    date  timestamp null default CURRENT_TIMESTAMP
) AUTO_INCREMENT 10000

```

- 显示表或数据库的创建语句
> show create table name;



# 错误记录

## 1，mysql驱动使用问题

由于帮人写大学生毕业设计的时候遇到数据库无法链接的问题，经过半天的摸索发现,新版本的数据库更新了链接字符串

记录下修改的步骤

- 下面是代码

```java

	    	try {
            //这个地方的类名变了,其实这句可加可不叫
	    		 Class.forName("com.mysql.cj.jdbc.Driver");
	        //获取数据库连接，数据库的链接字符串变成了这样，有时区，ssl安全性,和编码方式
	        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:65000/dinglijuan?useUnicode=true&characterEncoding=utf8&useSSL=false","root","123456789");
	        //定义SQL语句
	        String sql = "select * from user";

	        //获取执行sql的对象
	        Statement statement = connection.createStatement();

	        //执行sql
	        int i = statement.executeQuery(sql).getRow();

	        //处理结果
	        System.out.println(i);

	        //释放资源
	        statement.close();
	        connection.close();}
	    	catch(Exception e) {
	    		System.out.println("失败了"+e.toString());
	    		
	    	}


```

- 使用上面代码编译时，需要使用命令,显示带上jar库
> javac -classpath .;../你的驱动jar;其他的jar -d e:/myjavaclass-file

- 运行时使用下面语句,可以包含多个路径
> java -cp ../lib/你的jar路径;你的jar路径; myjavaclass



## mysql复制文件夹备份
> 进入到mysql的data文件夹下：复制数据库的除了带`#`的文件文件夹和ibdata1和mysql.ibd三种文件，到数据库相应的路径
