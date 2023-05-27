```mysql
DDL数据库操作
SHOW DATABASES;
CREATE DATABASE 数据库名;
USE 数据库名;
SELECT DATABASE();                     //查看当前处于哪个数据库中
DROP DATABASE 数据库名;

```

```mysql
DDL表操作
SHOW TABLES;
CREATE TABLE 表名 (字段 字段类型 ,  字段 字段类型);
DESC 表名;
SHOW CREATE TABLE 表名;               //查询建表语句
ALTER TABLE 表名 ADD/ MODIFY/ CHANGE/ DROP/ RENAME TO...;
			(添加)/(修改字段类型)/(修改名称及类型)/(删除字段)/(修改表名)
DROP TABLE 表名;
```



```mysql
DML操作
-- 1. 添加数据
INSERT INTO 表名 (字段一, 字段二, ...) VALUE (值1,值2, ...),(值1,值2, ...);
-- 2.修改数据
UPDATE 表名 SET 字段1=值1,字段2=值2 WHERE 条件; 
-- 3.删除数据
DELETE FROM  表名  WHERE 条件; 



```

```mysql
DQL操作

select
 	字段列表   ------------------------>   字段名[AS] 别名
 from
 	表名列表
 where                                       > >= < <= like between and 
 	条件列表       ------------------------>  in and or
 group by
 	分组字段
 having
 	分组之后的条件   ------------------------> 分组后过滤
 order by
 	排序       ------------------------> 升序默认 asc, 降序 desc
 limit
 	分页限定     ------------------------> 起始索引从0开始,每页展示记录数
							起始索引：(n-1)*10
```

```mysql
DCL操作
1.创建用户，只有指定IP才能登陆数据库服务器
CREATE USER 用户名@IP IDENTIFY BY 登录密码;
# 示例
CREATE USER 'lmc'@'223.88.93.55' IDENTIFY BY '123456';
2.创建用户，在所有IP都能登录数据库服务器
CREATE USER 用户名@'%' IDENTIFY BY 登录密码
# 示例
CREATE USER 'lmc'@'%' IDENTIFY BY '123456';

2.用户授权
1.给指定用户授予指定数据库的指定权限
GRANT 权限1,权限2,...,权限n ON 指定数据库名.* TO 用户名@IP;
2.给指定用户授予所有数据库的指定权限
GRANT 权限1,权限2,...,权限n ON *.* TO 用户名@IP;
3.给指定用户授予所有数据库的所有权限
GRANT ALL ON *.* TO 用户名@IP;
3.用户权限查询
SHOW GRANTS FOR 用户名@IP;
4.撤销用户权限
REVOKE 权限1,权限2,...,权限n ON 数据库名.* FROM 用户名@IP;
5.删除用户
DROP USER 用户名@IP;




```

