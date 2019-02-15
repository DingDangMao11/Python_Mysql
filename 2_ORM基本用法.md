### ORM介绍：
```
# 1.Object Relationship Mapping
# 2.对象模型与数据库表的映射
# 3.要使用ORM来操作数据库，首先需要创建一个类来与对应的表进行映射。
```
### 1.创建一个person表：
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
### 2.对person表进行增删改查：
```
#encoding: utf-8

from sqlalchemy import create_engine,Column,Integer,String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
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
# 所有的类都要继承自`declarative_base`这个函数生成的基类
Base = declarative_base(engine)
# 1.点击sessionmaker():ctrl+b可看到点击sessionmaker函数的源代码
# 2.session是一个类，sessionmaker是session是一个类类中的方法
# 3.sessionmaker(engine)表示创建了一个对象
# Session = sessionmaker(engine)
# 拿到Session这个对象在调用一下Session()
# session = Session()
# 简化为：
session = sessionmaker(engine)()

class Person(Base):
     # 定义表名为person
     __tablename__ = 'person'
     # 将id设置为主键，并且默认是自增长的
     id = Column(Integer,primary_key=True,autoincrement=True)
     # name字段，字符类型，最大的长度是50个字符
     name = Column(String(50))
     age = Column(Integer)
     country = Column(String(50))
     def __str__(self):
         return "<Person(name:%s,age:%s,country:%s)>" % (self.name,self.age,self.country)
# session 会话
# 增
def add_data():
    p = Person(name='zhiliao',age=18,country='china')
    # 1.首先将数据添加到session中
    # 2.再将数据提交给数据库
    session.add(p)
    session.commit()
# 查
def search_data():
    pass
# 改
def updata_data():
    pass
# 删
def delete_data():
    pass
if __name__ == '__main__':
    add_data()
# add_data()方法的运行结果：
mysql> select * from person;
+----+---------+------+---------+
| id | name    | age  | country |
+----+---------+------+---------+
|  1 | zhiliao |   18 | china   |
+----+---------+------+---------+
```
### 增的方法
#### 1.添加单条数据
```
p = Person(name='zhiliao',age=18,country='china')
session.add(p)
session.commit()
```
#### 2.添加多条数据
```
p1 = Person(name='zhiliao1',age=19,country='china')
p2 = Person(name='zhiliao2', age=20, country='china')
session.add_all([p1,p2])
session.commit()
```
### 查的方法
####  1.查找表中的所有数据
```
def search_data():
    all_person  = session.query(Person).all()
    for p in all_person:
        print(p)
```
#### 2.根据条件查找数据
##### (1)
```
    all_person = session.query(Person).filter_by(name='zhiliao1').all()
    for x in all_person:
        print(x)
        
```
##### (2)
```
all_person = session.query(Person).filter(Person.name=='zhiliao').all()
    for x in all_person:
        print(x)
```
#### 3.根据id查好数据
```
使用get方法查找数据，get方法根据id查找数据，只会返回一条数据或者NONE
```
```
person = session.query(Person).get(1)
print(person)
```
#### 4.使用first方法获取结果集中的第一条数据
```
person = session.query(Person).first()
print(person)
```
### 改
```
def updata_data():
    person = session.query(Person).first()
    person.name = 'ketang'
    session.commit()
```
### 删
```
def delete_data():
    person = session.query(Person).first()
    session.delete(person)
    session.commit()
```
