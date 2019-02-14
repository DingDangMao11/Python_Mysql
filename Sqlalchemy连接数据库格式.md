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
```
```
#encoding: utf-8
from sqlalchemy import create_engine

HOSTNAME = '127.0.0.1'
PORT     = '3306'
# DATABASE:表示要连接的数据库名称，首先要创建一个数据库
DATABASE = 'first_sqlalchemy'
USERNAME = 'root'
PASSWORD = '0000'
# mysqldb：用在python2中
# pymysql；用在python3中
# 信息拼接模板：
# DB_URI = 'mysql+mysqldb://{}:{}@{}:{}/{}'.format(USERNAME,PASSWORD,HOSTNAME,PORT,DATABASE)
# 1.什么数据库 2.
DB_URI = "mysql+pymysql://{username}:{password}@{host}:{port}/{db}?charset=utf8".format\
    (username=USERNAME,password=PASSWORD,host=HOSTNAME,port=PORT,db=DATABASE)
# 创建数据库引擎
engine = create_engine(DB_URI)
# 判断是否连接成功
conn = engine.connect()
result = conn.execute('select 1')
# fetchone()函数:可以将第一条数据拿出来
print(result.fetchone())
# 执行结果：
# (1,) 连接成功
```
