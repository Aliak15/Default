[TOC]

### 包管理`pip`

```python
"""
阿里云：http://mirrors.aliyun.com/pypi/simple/
华中理工大学：http://pypi.hustunique.com/
山东理工大学：http://pypi.sdutlinux.org/
豆瓣：http://pypi.douban.com/simple/
"""

https://pypi.tuna.tsinghua.edu.cn/simple
https://pypi.mirrors.ustc.edu.cn/simple/


python -m pip install 包名 [== 2.5.8指定版本] -i https://pypi.tuna.tsinghua.edu.cn/simple  #在线安装

pip list
pip show scapy
pip install -r requirements.txt

源配置: python安装路径/pip/pip.ini

```

### 杂项

```py
python3.exe -m pydoc -p 80 #文档

python -c "import sys; print(sys.executable)" #解释器依赖库路径

```

### venv

```python
python -m venv c:\path\to\myenv

c:\path\to\myenv\Scripts\activate.bat
```

### HTTP服务

```python
# 搭建simplehttpweb服务,目录显示为当前的pwd目录下的所有文件
python -m http.server 8000  --bind  127.0.0.1
可以访问
http://[::1]:8000
http://127.0.0.1:8000


# Flask框架
from flask import Flask,Response,request  
from flask_cors import CORS

app = Flask(__name__)  
CORS(app)  # This will enable CORS for all routes

@app.route('/api',methods=["GET"])  
def api():
    return Response('HEllO')

#app.debug = True # 设置调试模式，生产模式的时候要关掉debug  
app.run(host="0.0.0.0",port="8888")
```

### 模拟按键

```py
import pyautogui
import time

# 延时，给你准备时间
time.sleep(3)
pyautogui.press('a')
pyautogui.hotkey('ctrl', 's')
time.sleep(0.2) 
pyautogui.press('tab')
time.sleep(0.2) 
pyautogui.press('enter')
time.sleep(0.2) 
pyautogui.hotkey('ctrl', 'w')
time.sleep(0.2) 

```

### Md图片转本地

```py
import re
import os
with open('./1.md','r',encoding='utf-8') as f:
    a=f.read()
    b = re.findall('https?://.+?\.png|https?://.+?\.jpg|https?://.+?\.jpeg',a)
    for i in b:
        image_name=i.split('/')[-1]
        os.system(f'curl {i} -o ./image/{image_name} -k')
        a = a.replace(i,f'./image/{image_name}')

with open('./2.md','w',encoding='utf-8') as f:
    f.write(a)
```

