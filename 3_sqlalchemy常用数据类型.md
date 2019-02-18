### sqlalchemy常用数据类型
```
from sqlalchemy import create_engine,Column,Integer,String,Float,Boolean,DECIMAL,Enum,Date,DateTime,Time
```
```
#encoding: utf-8

from sqlalchemy import create_engine,Column,Integer,String,Float
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
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
     price = Column(Float)


Base.metadata.create_all()


article = Article(price=10.294334)
session.add(article)
session.commit()
```

#### 1.Float
```
#article = Article(price=10.2334)
+----+--------+
| id | price  |
+----+--------+
|  1 | 10.239 |
+----+--------+
# article = Article(price=10.294334)
# 丢失精度
+----+---------+
| id | price   |
+----+---------+
|  3 | 10.2943 |
+----+---------+
```
#### 2.Boolean
```
is_delete = Column(Boolean)
article = Article(is_delete=True)
+----+-----------+
| id | is_delete |
+----+-----------+
|  1 |         1 |
+----+-----------+
```

#### 3.DECIMAL：定点类型
```
解决精度丢失的的问题
DECIMAL(x,y):使用时需传递两个参数，第一个参数表示总共存储多少位数字，第二个参数表示小数点后有多少位数字
```
```
# 100000.0001
# DECIMAL(10,4) :存储10位数字，小数点后为4位
price = Column(DECIMAL(10,4))
article = Article(price=999999.9999)
+----+-------------+
| id | price       |
+----+-------------+
|  1 | 999999.9999 |
+----+-------------+
```
### 3.Enum：枚举类型
#### (1) from sqlalchemy import create_engine,Column,Integer,String,Float,Boolean,DECIMAL,Enum
```
tag = Column(Enum("python","flask","django"))
article = Article(tag="python")
```
#### (2) import enum
```
#encoding: utf-8

from sqlalchemy import create_engine,Column,Integer,String,Float,Boolean,DECIMAL
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
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
class TagEnum(enum.Enum):
    python='python'
    flask = 'flask'
    django = 'django'
```
```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     tag = Column(Enum(TagEnum))

# drop_all()将article表中的属性全部删掉
Base.metadata.drop_all()
Base.metadata.create_all()
```
```
#article = Article(tag="python")
article = Article(tag=TagEnum.flask)
```
```
session.add(article)
session.commit()
```
### 4.datetime类型
```
Date:存储时间，只能存储年月日。映射到数据库中是data类型。在+Python中，可以使用datatime.data来指定。
```
```
time = Column(Date)
from datetime import date
article = Article(time=date(2017,10,10))
+----+------------+
| id | time       |
+----+------------+
|  1 | 2017-10-10 |
+----+------------+
```
### # 5.Datetime
```
存储时间，可以存储到年月日时分秒毫秒等，映射到数据库中也是datetime类型。
在Python代码中，可以使用‘datetime.datetime’
time = Column(DateTime)
from datetime import datetime
article = Article(time=datetime(2011,10,10,11,11,11))
+----+---------------------+
| id | time                |
+----+---------------------+
|  1 | 2011-10-10 11:11:11 |
+----+---------------------+
```
### 6.Time
```
Time: 存储时间，可以存储时分秒，映射到数据库中也是time类型。
在Python代码中，可以使用‘datetime.time’
```
```
class Article(Base):
     __tablename__ = 'article'
     id = Column(Integer,primary_key=True,autoincrement=True)
     create_time = Column(Time)
from datetime import time
article = Article(create_time=time(hour=11,minute=1,second=11))
+----+-------------+
| id | create_time |
+----+-------------+
|  1 | 11:01:11    |
+----+-------------+
```
### 7.String
```
可变字符类型，映射到数据库是varchar类型
```
```
title = Column(String(50))
article = Article(title='adb')
```
