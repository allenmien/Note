# request模块
1. GET 请求
```python
import requests
import json
MAX_TIMES = 3

def redis_service_get(service_url, schema, db, key, feilds):
    """
    redis GET 服务
    :param service_url:
    :param table_name:
    :param feilds:
    :param query_str:
    :return:
    """
    times = 1
    data = None
    if service_url:
        service_url += u"/hget?schema=" + schema + u"&db=" + db + u"&key=" + key + u"&fields=[\"" + feilds + u"\"]"
        logger.debug(u"Getting doc from URL: " + service_url)
        while times <= MAX_TIMES:
            try:
                times += 1
                response = requests.get(service_url, timeout=10)
                response_text = response.text
                if response_text:
                    data = json.loads(response_text)
                break
            except Exception as e:
                logger.exception(u"Error when getting message from queue " + str(e))
    else:
        logger.warning(u"The Queue service url is invalid.")
    return data
```
2. POST 请求
```
import requests
import json
MAX_TIMES = 3
def queue_service_offer(service_url, topics, data_json_str):
    """
    发送消息
    :param service_url:
    :param topics:
    :param data_json_str:
    :return:
    """
    times = 1
    result = False
    if service_url:
        service_url += u"/offer?_=0"
        req_data = {
            u"group_id": u"",
            u"data_str": data_json_str,
            u"topic_names": topics
        }
        while times <= MAX_TIMES:
            try:
                times += 1
                response = requests.post(service_url, data=req_data, timeout=10)
                response_text = response.text
                if response_text == u"true":
                    result = True
                else:
                    logger.error(u"Putting queue data error!")
                break
            except Exception as e:
                logger.exception(u"Error when putting message to queue " + str(e))
    else:
        logger.warning(u"The Queue service url is invalid.")
    return result
```
