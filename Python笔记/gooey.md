- Gooey例子
	
	```python
	# coding:utf-8
	import sys
	reload(sys)
	sys.setdefaultencoding('utf-8')
	
	import json
	from gooey import Gooey
	from gooey import GooeyParser
	import requests
	
	# config
	## 正式环境
	###kafka
	kafka_host = "10.0.4.150"
	kafka_port = "8080"
	## 测试环境a
	
	@Gooey
	class Message():
	    def __init__(self):
	        parser = GooeyParser(description="Shutadata")
	        parser.add_argument("-data_id", action="store", help="Data id")
	        parser.add_argument("-message_type", action="store", help="like:auto_gs_quanguo")
	        args = parser.parse_args()
	        self.data_id = args.data_id
	        self.message_type = args.message_type
	        
	    def send2kafka(self):
	        url = "http://{host}:{port}/{path}".format(host=kafka_host, port=kafka_port, path="kafka/offer?_=0")
	        data = {
	          "_traceid": "256879",
	          "_sent_time": "2017-07-01 15:20:17.315",
	          "_dataid": self.data_id,
	          "_type": self.message_type,
	          "keyword": ""
	        }
	        payload = {
	            "group_id": "",
	            "data_str": json.dumps(data),
	            "topic_names": "tp_diff_comp"
	        }
	        res = requests.post(url=url, data=payload)
	        print(res.content)
	
	
	if __name__ == "__main__":
	    #data_id = raw_input("请输入要处理的dataid:")
	    #message_type = raw_input("请输入处理type(比如auto_gs_quanguo):")
	    message = Message()
	    print("向kafka发送消息")
	    message.send2kafka()
	    
- windows 生成gui，会生成example.spec文件
	
	```shell
	pyinstaller -F example.py
	
- 修改example.spec，主要添加
	
	```
	gooey_languages = Tree('C:/Python27/Lib/site-packages/gooey/languages', prefix = 'gooey/languages')
	gooey_images = Tree('C:/Python27/Lib/site-packages/gooey/images', prefix = 'gooey/images')
	```
	
	在a.datas后边添加
	
	```
   gooey_languages, # Add them in to collected files
   gooey_images, # Same here
	
	```
	
	修改后的example.spec（注:下面的代码仅仅是例子，要根据自己生成的example.spec，自己添加gooey_languages 以及 gooey_images）
	
	```
	# -*- mode: python -*-

	block_cipher = None
	gooey_languages = Tree('C:/Python27/Lib/site-packages/gooey/languages', prefix = 'gooey/languages')
	gooey_images = Tree('C:/Python27/Lib/site-packages/gooey/images', prefix = 'gooey/images')
	
	
	a = Analysis(['nnnn.py'],
	             pathex=['C:\\Users\\user\\Desktop\\tmp\\tmp1'],
	             binaries=[],a
	             datas=[],
	             hiddenimports=[],
	             hookspath=[],
	             runtime_hooks=[],
	             excludes=[],
	             win_no_prefer_redirects=False,
	             win_private_assemblies=False,
	             cipher=block_cipher)
	pyz = PYZ(a.pure, a.zipped_data,
	             cipher=block_cipher)
	exe = EXE(pyz,
	          a.scripts,
	          a.binaries,
	          a.zipfiles,
	          a.datas,
	          gooey_languages, # Add them in to collected files
	          gooey_images, # Same here
	          name='nnnn',
	          debug=False,
	          strip=False,
	          upx=True,
	          runtime_tmpdir=None,
	          console=True )
	          
- 运行pyinstaller example.spec生成 example.exe