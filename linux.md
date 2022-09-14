# Python相关环境

## Python3.6.5安装

```shell
# 下载安装包并解压
wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
tar zvxf Python-3.6.5.tgz

# 编译安装
cd Python-3.6.5
mkdir /usr/local/python3.6.5 # 创建安装目录
./configure --prefix=/usr/local/python3.6.5 # 编译
make && make install # 安装

# 创建软链接
mv /usr/bin/python /usr/bin/python_bak # 备份原链接
ln -s /usr/local/python3.6.5/bin/python3 /usr/bin/python # 创建新的软连接并且运行时候不用输入版本号（python指向python3）

# 测试运行
python

```





## pip

安装pip前需要前置安装setuptool

```shell
wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz#md5=c607dd118eae682c44ed146367a17e26

tar -zxvf setuptools-19.6.tar.gz

cd setuptools-19.6

python setup.py build

python setup.py install
```



```shell
wget "https://pypi.python.org/packages/source/p/pip/pip-1.5.4.tar.gz#md5=834b2904f92d46aaa333267fb1c922bb" --no-check-certificate

tar -xzvf pip-1.5.4.tar.gz
cd pip-1.5.4
python setup.py install
```



# 运行

```shell
nohup python -u Main.py > monitor.log 2>&1 &
nohup python -u Main.py >/dev/null  2>&1 &  #不打印log
nohup python -u Main.py >/dev/null 2>error.log  2>&1 &  #只记录异常日志
```

[(1条消息) linux后台运行python程序_遂顺自然的博客-CSDN博客_linux 后台运行python](https://blog.csdn.net/weixin_44366822/article/details/121207690)



# 常用命令

## 查看端口占用

```shell
netstat -tunlp | grep 端口号
```

