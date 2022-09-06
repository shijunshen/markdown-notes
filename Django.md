# 安装

```shell
pip install django==1.11.7
```





# 创建

```shell
#进入项目保存目录
django-admin startproject 项目名字
```



```shell
#创建app
django-admin startapp app01
```



# 启动

- 命令行：

  ```shell
  python manage.py runserver #这个命令启动起来只能在本地访问
  python manage.py runserver 0.0.0.0:8000 #表示所有人都能通过8000端口访问
  ```
  



- 修改Host访问权限，使所有主机都可以远程访问	

在setting.py里面将

```pyt
ALLOWED_HOSTS = ['*']
```

