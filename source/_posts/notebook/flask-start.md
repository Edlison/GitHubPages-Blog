---
title: Flask入门
categories:
- Notebook
tags:
- python
- flask
---

## Flask模块化管理
Bluepoint可以实现Flask的模块化管理，在每个包的`__init__`文件中可以定义对应的蓝图。蓝图仅是静态的，需要在应用运行时，对其进行装载。

**注意**
1. `__init__`文件中定义好蓝图后一定要将包含路由的文件import进来，否则不能识别相应的路由。
2. import文件时不能使用通配符*进行导入。

## Flask中的request与respond

### request
`from flask import request`

1. param传参
使用request.args.get('argname')获取键值对。

2. body传参
使用request.form.get('argname')获取form格式传参键值对。
使用request.get_data()获取全部raw格式内容。
使用request.get_json()获取全部raw格式内容，且必须为json格式。

**注意**
form-data: 可以传键值对和文件。
x-www-form-urlencodes: 只能传键值对。

### respond
`from flask import jsoniify`
`import json`

1. 直接返回json
- jsonify()可以直接制作json返回 (返回的格式也定义为application/json)
- json.dumps()可以将dict生成json (返回的格式默认为text)

2. 简易定义make_respond

| args | 含义 |  
| - | - |  
| `respond` | 返回的内容data |  
| `status_code` | 返回状态码 |  
| `headers` | 返回头部信息为键值对 |  

3. 详细定义Respond

| args | 含义 |  
| - | - |  
| `respond` | 返回的内容data |  
| `status` | 返回状态 格式为'200 OK' |  
| `status_code` | 返回状态码 |  
| `headers` | 返回头部信息为键值对 |  
| `set_cookie` | 设置cookie |  
| `del_cookie` | 删除cookie |  

## Flask中的session
`from flask import session`

1. 使用session需要设置config  
app.config['SECRET_KEY']=os.urandom(24)

2. Flask提供的session是储存在cookie中的  
session['username'] = data['username']  
此时的cookie中存储了加密过的username。

3. 获取session  
session.get('key')或session['key']  
但是后者如果没有这个key会报错。  
获取到的session自动经过了解密。

4. 删除session  
session.pop('key')或session['key']=False  
session.clear清除session中所有数据。

## Flask_SQLAlechemy
**注意**
1. 如果蓝图中有用到db，则一定要在db创建后import蓝图！！！
```py
db = SQLAlchemy()

def create_app():
    app = Flask(__name__)
    app.config['SECRET_KEY'] = os.urandom(24)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://library:library@119.23.107.61:3306/library'
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
    db.init_app(app)
    from .user import user_blu
    app.register_blueprint(user_blu)
    print(app.url_map)
    return app
```
因为代码是按照顺序执行的，当你从其他模块导入蓝图的时候就需要 db 了，但是这个时候db 还没有生成，所以会报出错误 提示不可以从当中导入相应的模块。

2. 如果db=SQLAlechemy()中没有传app，则后面需要先`db.app=app`然后再`db.init_app(app)`???

## Flask g

1. g对象
- 专门用来存储用户信息
- g只在一次请求中的所有代码有用

2. g与session的区别
- session只要未失效，不同的request请求都可以获得session，由cookie决定
- 每一次请求都会改变g对象