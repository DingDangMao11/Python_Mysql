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
