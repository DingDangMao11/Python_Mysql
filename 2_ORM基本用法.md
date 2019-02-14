### ORM介绍：
```
# 1.Object Relationship Mapping
# 2.对象模型与数据库表的映射
# 3.要使用ORM来操作数据库，首先需要创建一个类来与对应的表进行映射。
```
```
将数据对象模板映射到数据库中案例：
#encoding: utf-8

from sqlalchemy import create_engine,Column,Integer,String
from sqlalchemy.ext.declarative import declarative_base

HOSTNAME = '127.0.0.1'
PORT     = '3306'
# DATABASE:表示要连接的数据库名称，首先要创建一个数据库
DATABASE = 'first_sqlalchemy'
USERNAME = 'root'
PASSWORD = '0000'

DB_URI = "mysql+pymysql://{username}:{password}@{host}:{port}/{db}?charset=utf8".format\
    (username=USERNAME,password=PASSWORD,host=HOSTNAME,port=PORT,db=DATABASE)
# 创建数据库引擎
engine = create_engine(DB_URI)
# declarative_base方法产生一个类，用来存放数据类对象。Base 有创建ORM模型的能力
Base = declarative_base(engine)

# 1. 创建一个ORM模型，这个ORM模型必须继承自sqlalchemy给我们提供的好的基类
class Person(Base):
     __tablename__ = 'person'
    # 2. 在这个ORM模型中创建一些属性，来和表中的字段进行一一映射。这些属性必须是sqlalchemy给我们提供好的数据类型
     # 将id设置为主键，并且默认是自增长的
     # Column :表示列，primary_key：主键，autoincrement：自增长
     id = Column(Integer, primary_key=True, autoincrement=True)
     name = Column(String(50))
     age = Column(Integer)
     country = Column(String(50))
# 3. 将创建好的ORM模型映射到数据库中
Base.metadata.create_all()
# 运行结果:
  在mysql数据库中查询映射到的数据：
mysql> show tables;
+----------------------------+
| Tables_in_first_sqlalchemy |
+----------------------------+
| person                     |
+----------------------------+
1 row in set (0.00 sec)

mysql> desc person;
+---------+-------------+------+-----+---------+----------------+
| Field   | Type        | Null | Key | Default | Extra          |
+---------+-------------+------+-----+---------+----------------+
| id      | int(11)     | NO   | PRI | NULL    | auto_increment |
| name    | varchar(50) | YES  |     | NULL    |                |
| age     | int(11)     | YES  |     | NULL    |                |
| country | varchar(50) | YES  |     | NULL    |                |
+---------+-------------+------+-----+---------+----------------+
```
