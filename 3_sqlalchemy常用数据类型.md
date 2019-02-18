### sqlalchemy常用数据类型
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



