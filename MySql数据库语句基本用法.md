```
# 显示所有数据库：show databases;
# 删除旧的数据库：drop database first_sqlalchemy;
# 创建新的数据库：create database first_sqlalchemy charset utf8;
# 数据库的配置变量
# 空数据库查询： select 1;
# +---+
# | 1 |
# +---+
# | 1 |
# +---+
# use first_sqlalchemy;
# show tables;
# drop tables person;
# mysql> show tables;
# +----------------------------+
# | Tables_in_first_sqlalchemy |
# +----------------------------+
# | person                     |
# +----------------------------+
#
# mysql> desc person;
# +---------+-------------+------+-----+---------+----------------+
# | Field   | Type        | Null | Key | Default | Extra          |
# +---------+-------------+------+-----+---------+----------------+
# | id      | int(11)     | NO   | PRI | NULL    | auto_increment |
# | name    | varchar(50) | YES  |     | NULL    |                |
# | age     | int(11)     | YES  |     | NULL    |                |
# | country | varchar(50) | YES  |     | NULL    |                |
# +---------+-------------+------+-----+---------+----------------+
```
```
mysql> show create table article\G
*************************** 1. row ***************************
       Table: article
Create Table: CREATE TABLE `article` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `telephione` varchar(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `telephione` (`telephione`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8
1 row in set (0.01 sec)
```
