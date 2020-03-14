# 打包 python 项目
Learning how to package the module and send it to PYPI.  

对于如何编写和打包模块并上传到 PYPI 的方式详见 PYPI 的官方文档：  
[Packaging Python Projects](https://packaging.python.org/tutorials/packaging-projects/)  

在这里我主要说一下我遇到的问题和相关的解决方式：  

* 问题：我在 setup.py 中使用了 
  ```
  with open('README.md')as fl:
      long_description = fl.read()
  ```
  但是在使用 `python setup.py sdist` 时却产生了以下错误：
  ```
      Traceback (most recent call last):
        File "<string>", line 1, in <module>
        File "C:\Users\ADMINI~1\AppData\Local\Temp\pip-req-build-2xmdmk1p\setup.py
  ", line 3, in <module>
          with open('README.md')as fl:
      FileNotFoundError: [Errno 2] No such file or directory: 'README.md'
      ----------------------------------------
  ERROR: Command errored out with exit status 1: python setup.py egg_info Check th
  e logs for full command output.
  ```
  提示说找不到 README.md 文件。但是在 setup.py 路径下明明存在着该文件！  
  解决方式：  
  在 setup.py 路径下新建一个 MANIFEST.in 文件。在这个文件中添加
  ```
  include README.md
  ```
  保存后重新运行 `python setup.py sdist` 就发现不会提示这些错误了。  
  (具体见 [PyPI & pip - installed package receive error reading README.md](https://stackoverflow.com/questions/34415182/pypi-pip-installed-package-receive-error-reading-readme-md))  

* 问题：PYPI 上面的 long_description 支持 markdown 语法，但我一开始在 README.md 中使用 `[Google](https://www.google.com)` 上传到 PYPI 后发现没有生效。  
原因：**要求 setuptools 版本 >= 38.6.0 markdown 语法才能生效。**(具体见文章 [PyPI 终于支持 Markdown 了](https://zhuanlan.zhihu.com/p/34853707))  
解决方式：在 console 窗口运行 `pip install setuptools --upgrade` 将 setuptools 升级到最近版本。接着重新打包上传就生效了。  

* PYPI 支持的 markdown 语法没有 github 那么智能，在使用显示代码时要加上代码所属语言，不然显示可能会出问题！比如在写以下语句时，记得加上 python。
  <pre>
  ```python
  print('Hello World!')
  ```
  </pre>
 
* 关于 PYPI 上我们的模块/包版本更新的问题。  
  一般情况下，我们成功上传一个版本到 PYPI 以后，如果我们对代码有调整，想重新上传覆盖掉原始版本。这种方式是行不通的。即便在 PYPI 上删除掉该版本，还是**有可能上传失败**。正常使用方式是在我们修改更新完代码后，将 setup.py 里面的 version 更新，接着重新上传就行。从另一方面上说就是：**已上传的版本是无法被覆盖/删除掉的。**

参考文章：  
[Packaging Python Projects](https://packaging.python.org/tutorials/packaging-projects/)  
[How to upload your python package to PyPi](https://medium.com/@joel.barmettler/how-to-upload-your-python-package-to-pypi-65edc5fe9c56)  
相关链接：  
[Choose an open source license](https://choosealicense.com/)  
[Open Source Initiative](https://opensource.org/licenses/)  
[Classifiers](https://pypi.org/classifiers/)  
