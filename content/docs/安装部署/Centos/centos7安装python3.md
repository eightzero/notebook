# 1. 安装编译相关

```sh
yum -y groupinstall "Development tools"
```

```sh
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```

```sh
yum install libffi-devel -y
```

# 2. 下载安装包解压

```sh
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
```

```sh
tar -xvJf  Python-3.7.0.tar.xz
```

# 3.编译安装python

```sh
mkdir /usr/python3 #创建编译安装目录
cd Python-3.7.0
./configure --prefix=/usr/python3
make && make install
```

# 4.创建软连接

```sh
ln -s /usr/python3/bin/python3 /usr/bin/python3
ln -s /usr/python3/bin/pip3 /usr/bin/pip3
```

# 5. 验证是否成功

```sh
python3 -V
pip3 -V
```

