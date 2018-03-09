# Django

## html模板

- 一般的变量之类的用 {{ }}（变量），功能类的，比如循环，条件判断是用 {%  %}（标签）

  ```python
  教程列表：
  {% for i in TutorialList %}
  {{ i }}
  {% endfor %}

  def home(request):
      TutorialList = ["HTML", "CSS", "jQuery", "Python", "Django"]
      return render(request, 'home.html', {'TutorialList': TutorialList})
  ```


- 字典：

  ```python
  站点：{{ info_dict.site }} 内容：{{ info_dict.content }}

  def home(request):
      info_dict = {'site': u'自强学堂', 'content': u'各种IT技术教程'}
      return render(request, 'home.html', {'info_dict': info_dict})
  ```

  遍历字典：

  ```python
  {% for key, value in info_dict.items %}
      {{ key }}: {{ value }}
  {% endfor %}
  ```

  ​