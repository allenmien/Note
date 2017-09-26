# exception
##### 自定义exception及调用
```Python
class DataException(Exception):
    def __init__(self, message, exception):
        Exception.__init__(self, message)
        self.exception = exception
try:
  try:
    try:
      raise Exception(u"exception test")
    except Exception as e:
      raise DataException(traceback.format_exc(), e)
  except DataException as de:
    raise de
except DataException as de:
  message = de[0]
  print message
```
