# 创建项目

项目结构：

- static
- templates
- app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(debug=true)

```



## 配置文件

在config.py中配置（flask的配置项名字都是大写字母）

```pytho
JSON_AS_ASCII = True
```

并在app.py中加上：

```python
app.config.from_object(config)
```



## 返回JSON格式

用flask里面自带的jsonify函数转换成json



## 带参数的url

```python
@app.route('/book/<int:book_id>')
def book_detail(book_id):
    print(book_id)
    return "success"

```



## 构造URL（url_for）

通过函数反转生成url，因为url并不是固定的，但函数名是固定的

```python
@app.route("/book/list")
def book_list():
    for book in books:
        book['url'] = url_for("book_detail",book_id=book['id'])
    return jsonify(books)
```



## 指定HTTP方法

```python
@app.route("\",methods=['GET','POST'])
```

### GET参数

```python
from flask import Flask,jsonify,request
@app.route('/api/v1/user/info')
def LoginInfo():
    token = request.args.get('token')
```

### POST参数

```python
from flask import Flask,jsonify,request
@app.route('/api/v1/user/login',methods=["POST"])
def Login():
    # 1. 前端返回的是JSON类型
    req = dict(request.json)
    print(req['username'])
    
    # 2. 前端返回的是form类型
    print(request.form.get('username'))
```





