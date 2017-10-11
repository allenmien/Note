# Json
##### unicode 转dict
```Python
initial_data = json.loads(text.encode("utf-8"))
```
##### 转json
```Python
record = json.loads(json.dumps(record, cls=CJsonEncoder))
```
```Python
class CJsonEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime.datetime):
            return obj.strftime('%Y-%m-%d %H:%M:%S')
        elif isinstance(obj, ObjectId):
            return str(obj)
        elif isinstance(obj, int):
            return str(obj)
        elif isinstance(obj, float):
            return str(obj)
        else:
            return json.JSONEncoder.default(self, obj)
```
