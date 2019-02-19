### query可用参数:
####  (1)模型对象。
```
指定查找这个模型中的所有对象
articles = session.query(Article).all()
```
```
#encoding: utf-8
import random

from sqlalchemy import create_engine,Column,Integer,String,Float
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
     title = Column(String(50),nullable=False)
     price = Column(Float,nullable=False)
     def __repr__(self):
         return "<Article(title:%s)>" % self.title

# for x in range(6):
#     article = Article(title='title%s'%x,price=random.randint(50,100))
#     session.add(article)
# session.commit()
articles = session.query(Article).all()
```
##### (1)
```
for article in articles:
    print(article)
```
```
<Article(title:title1)>
<Article(title:title2)>
<Article(title:title3)>
<Article(title:title4)>
<Article(title:title5)>
``` 
##### (2)
```
print(articles)
```
```
[<Article(title:title0)>, <Article(title:title1)>, <Article(title:title2)>, <Article(title:title3)>, <Article(title:title4)>, <Article(title:title5)>]
```
#### (2).模型中的属性。
```
可以指定只查找某个模型的其中几个属性。
```
```
# 模型中的属性
articles = session.query(Article.title,Article.price).all()
print(articles)
```
```
[('title0', 65.0), ('title1', 85.0), ('title2', 55.0), ('title3', 96.0), ('title4', 72.0), ('title5', 92.0)]
```
#### (4)聚合函数
```
func.count：统计行的数量。
func.avg：求平均值。
func.max：求最大值。
func.min：求最小值。
func.sum：求和。
```
```
result = session.query(func.count(Article.id)).first()
print(result)
(6,)
```
```
result = session.query(func.max(Article.price)).first()
print(result)
(96.0,)
```
```
result = session.query(func.min(Article.price)).first()
print(result)
```
```
result = session.query(func.sum(Article.price)).first()
print(result)
```





