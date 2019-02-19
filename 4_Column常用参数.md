###  Column常用参数：
```
(1)default：默认值
(2)nullable：是否可空。默认是True，也就是可以为空
(3)primary_key:是否为主键
(4)autoincrement:是否自动增长
(5)onupdate:更新的时候执行的函数
(6)name:该属性在数据库中的字段映射
(7)unique:指定某个值是否唯一，默认为False
```
```
from sqlalchemy import create_engine,Column,Integer,String,Float,Boolean,DECIMAL,Enum,Date,DateTime,Time,Text
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from datetime import datetime
# python3中才有
import enum
HOSTNAME = '127.0.0.1'
PORT     = '3306'
DATABASE = 'first_sqlalchemy'
USERNAME = 'root'
PASSWORD = '0000'

DB_URI = "mysql+pymysql://{username}:{password}@{host}:{port}/{db}?charset=utf8".format\
    (username=USERNAME,password=PASSWORD,host=HOSTNAME,port=PORT,db=DATABASE)
# 创建数据库引擎
engine = create_engine(DB_URI)
Base = declarative_base(engine)

session = sessionmaker(engine)()

class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     create_time = Column(DateTime)
     rea_count = Column(Integer,default=11)
Base.metadata.drop_all()
Base.metadata.create_all()

article = Article()
article.create_time = datetime.now()
session.add(article)
session.commit()
```
### 1.default
#### (1) article.create_time = datetime.now()
```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     create_time = Column(DateTime)
     rea_count = Column(Integer,default=11)
```
```
article.create_time = datetime.now()
```
```
+----+---------------------+-----------+
| id | create_time         | rea_count |
+----+---------------------+-----------+
|  1 | 2019-02-18 19:36:36 |        11 |
+----+---------------------+-----------+
```
#### (2) create_time = Column(DateTime,default=datetime.now)
```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     create_time = Column(DateTime,default=datetime.now)
     rea_count = Column(Integer,default=11)
Base.metadata.drop_all()
Base.metadata.create_all()
```
```
article = Article()
session.add(article)
session.commit()
```
```
+----+---------------------+-----------+
| id | create_time         | rea_count |
+----+---------------------+-----------+
|  1 | 2019-02-18 19:41:59 |        11 |
+----+---------------------+-----------+
```
### 2.nullable
```
(1)  nullable=False,表示title不可以为空
(2)nullable=True,表示title可以为空
```
```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     # (1)title = Column(String(50)) 默认nullable值为True。表示title可以为空
     # (2)title = Column(String(50),nullable=False),表示title不可以为空
     title = Column(String(50),nullable=False)
```
```
article = Article()
article.title='abc'
session.add(article)
session.commit()
```
### 3.unique
```
unique=True ：数据唯一
```
```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     telephione = Column(String(11),unique=True)
Base.metadata.drop_all()
Base.metadata.create_all()

article = Article(telephione='18888888888')
session.add(article)
session.commit()
```
### 3.onupdate
```
在数据更新的时候会调用这个参数指定的值或者函数。在第一次插入这条数据的时候，不会用onupdate的值，只会用default的值。
常用的就是‘update_time’(每次更新数据的时候都要更新的值)
```

```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     title = Column(String(50),nullable=False)
     update_time = Column(DateTime,onupdate=datetime.now,default=datetime.now)
# Base.metadata.drop_all()
# Base.metadata.create_all()

article = session.query(Article).first()
article.title = '123'
session.commit()
```
### 4.name 
```
指定ORM模型中某个属性映射到表中的字段名。如果不指定，那么会使用这个属性的名字来作为字段名。如果指定了，就会使用指定的这个值作为参数。
title = Column(String(50),name='my_title',nullable=False)
```
```
#encoding: utf-8
from sqlalchemy import create_engine,Column,Integer,String,Float,Boolean,DECIMAL,Enum,Date,DateTime,Time,Text
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from datetime import datetime
# python3中才有
import enum
HOSTNAME = '127.0.0.1'
PORT     = '3306'
DATABASE = 'first_sqlalchemy'
USERNAME = 'root'
PASSWORD = '0000'

DB_URI = "mysql+pymysql://{username}:{password}@{host}:{port}/{db}?charset=utf8".format\
    (username=USERNAME,password=PASSWORD,host=HOSTNAME,port=PORT,db=DATABASE)
# 创建数据库引擎
engine = create_engine(DB_URI)
Base = declarative_base(engine)

session = sessionmaker(engine)()
```
```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     title = Column(String(50),name='my_title',nullable=False)
     update_time = Column(DateTime,onupdate=datetime.now,default=datetime.now)
Base.metadata.drop_all()
Base.metadata.create_all()
article =Article(title='abc')
session.add(article)
session.commit()
session.commit()
```
```
+----+----------+---------------------+
| id | my_title | update_time         |
+----+----------+---------------------+
|  1 | abc      | 2019-02-19 10:25:23 |
+----+----------+---------------------+
```








