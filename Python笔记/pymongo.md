# pymongo
##### 连接
```python
class CONFIG(object):
    mongo_iGS_url =
    mongo_iGS_port =
    mongo_iGS_db =
    mongo_iGS_user =
    mongo_iGS_password =

# 初始化mongo_iGS
mongo_iGS_client = pymongo.MongoClient(host=CONFIG.mongo_iGS_url)
mongo_iGS_db = mongo_iGS_client[CONFIG.mongo_iGS_db]
mongo_iGS_result1 = mongo_iGS_db.authenticate(name=CONFIG.mongo_iGS_user, password=CONFIG.mongo_iGS_password)

entity_db = mongo_iGS_db[u""]
gs_basic_db = mongo_iGS_db[u""]
```
##### 分批取
```
```
