## jpype

使用python调用java

```
jvm_path = jpype.getDefaultJVMPath()
                jvm_arg = u'-Djava.class.path='
                for (root, dirs, files) in os.walk(u"./extract/lib"):
                    for filename in files:
                        jvm_arg = jvm_arg + u'./extract/lib/' + filename + u";"
                if not jpype.isJVMStarted():
                    jpype.startJVM(jvm_path, jvm_arg)
                Foo = jpype.JPackage('com.AlphaX').Foo
                foo = Foo()
                paragraph[u"formatted_html"] = foo.start(original_html)
                jpype.shutdownJVM()
```

