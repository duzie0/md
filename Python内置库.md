# Python内置库和模块

## logging

## os

```python
os.name
os.uname()
os.environ
os.environ.get('key')
os.getcwd()
os.mkdir('path')
os.rmdir('path')
os.rename('path','path') #可以是目录也可以是文件
os.remove('filename')
os.listdir('path')
os.path.isdir('path')
os.path.isfile('path')
os.path.abspath('path')
os.path.join('path','path')
os.path.split('path')
os.path.splittext('path')

```

`shutil`模块提供了`copyfile()`的函数，可以看做是`os`模块的补充。

## re

match