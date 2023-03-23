# Background_Management_Systeme
 基于Django前后端不分离开发的后端管理系统学习

# 初始化设置

## 安装依赖

```python
pip3 install -r requirements.txt
```

## 数据库创建

![image-20230323232659140](https://gitee.com/HMTeenage/image/raw/master/Summary/image-20230323232659140.png)

```python
DATABASES = {
    'default': {
        # 连接本地mysql数据库【需要下载一个第三方库：mysqlclient】
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'Background_Management_Systeme',		# 数据库名字，可以更改为自己定义的
        'USER': 'root',
        'PASSWORD': '123456',							# 数据库密码
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}

# 其余选项，若是在本地创建数据库，则不需要改动；若是链接服务器，需要变更
```

## 数据库迁移

```python
# 命令行依次输入

python manage.py makemigrations

python manage.py migrate
```

## 生成初始密码

### 密码表存储逻辑

数据库中存储关于用户名和密码的数据表设计如下：

- 用户名：真实数据
- 密码：MD5算法加密后的结果

因此初始化数据库密码表的时候，不能把密码存储为真实的密码，而应该存储原始密码经过MD5算法加密后的结果

### 初始化密码

1. 找到密码表

![image-20230323233929893](https://gitee.com/HMTeenage/image/raw/master/Summary/image-20230323233929893.png)

2. 新增密码

```
用户：GSF
密码：7d716550d434a76e707377db056a9bcd

# 上述密码的真实值为：123，在用户登录页面输入该真实值，即可通过验证，完成登录
```

![image-20230323234123928](https://gitee.com/HMTeenage/image/raw/master/Summary/image-20230323234123928.png)

3. 自定义新增密码

- MD5加密算法如下：

```python
def md5(data_string):
    obj = hashlib.md5(settings.SECRET_KEY.encode('utf-8'))
    obj.update(data_string.encode('utf-8'))
    return obj.hexdigest()

# settings.SECRET_KEY是Django自己生成的字符密钥，可在settings.py文件内查看。即加密算法等价于：

def md5(data_string):
    obj = hashlib.md5('django-insecure-hgel4k_@i84z%dwm8!70#%!)=p#*9p#7t8h2my@*s=npzo5ku6'.encode('utf-8'))
    obj.update(data_string.encode('utf-8'))
    return obj.hexdigest()

# 新建文件，运行该函数，即可得到你创建的密码在数据库对应的存储值

# 注意：函数参数为字符类型
md5('你的密码')  
```

# 登录

## 运行项目

```
python manage.py runserver
```

## 登陆网址

http://127.0.0.1:8000/login/

![image-20230323235028066](https://gitee.com/HMTeenage/image/raw/master/Summary/image-20230323235028066.png)

![image-20230323235059126](https://gitee.com/HMTeenage/image/raw/master/Summary/image-20230323235059126.png)

