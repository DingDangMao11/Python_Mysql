```
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
