# 基于flask的个人博客系统搭建

## TODO
- [ ] 添加搜索功能

## 部署要点：

首先为了验证程序功能，需要在本机上部署，之后使用nginx反向代理工具，可以将端口号暴露出去，这样其他人就可以在公网访问你的博客了，之后在服务器进行同样的步骤。具体过程如下。

1. 安装virtualenv python虚拟环境，` pip install virtualenv ` 或者 `pip3 install virtualenv` 。然后使用virtualenv在一个合适的目录，例如在该项目下面建立venv环境` virtualenv venv `，
2. 激活virtualenv环境，运行venv目录下的activate，`. venv/bin/activate`， 注意点的位置，之后你会看到命令行前面多了(venv)这个标记。
3. 在虚拟环境下安装所有requirements.txt中的模块, `pip3 install -r requirements.txt`。如果安装太慢，你可能需要配置pip的国内源。更改pip源的方法参见pip官方页面。
4. 导入坏境变量，在项目目录下创建文件 `env` ，输入如下字段：

  * `MAIL_USERNAME=email@example.com` (服务器发验证码用的邮箱，开启了smtp服务的邮箱账号，程序里默认使用qq邮箱，修改config文件可成其它类型邮箱)
  * `MAIL_PASSWORD=password` (上面邮箱的密码，注意qq邮箱使用特殊16位专用密码)
  * `FLASK_ADMIN=admin@example.com` (服务器运行后，使用该邮箱创建账号就是管理者)
  * `MAIL_SERVER=smtp.qq.com`(邮箱服务器地址)
  * `FLASKY_MAIL_SENDER=example@foxmail.com`(发送人)

5. 安装数据库迁移。输入以下命令

  * `python manager.py db init` (使用init命令创建迁移仓库，此时会多出 migrations 文件夹)
  * `python manager.py db migrate -m "initial migration"`(migrate命令用来自动创建迁移脚本)
  * `python manager.py db upgrade`(更新数据库，第一次使用该命令会新建一个数据库data-dev.sqlite)
  
6. 部署程序， `python manager.py deploy`
7. 在本地运行程序, `python manager.py runserver`打开 http://127.0.0.1:5000 端口查看，按Ctrl+C退出程序。
8. 如果在服务器运行，要保留数据，则可以复制数据库 migrations 文件夹以及数据库 data-dev.sqlite 到服务器，然后使用 `./venv/bin/gunicorn -w 4 -b 127.0.0.1:8080 manager:app` 此时应该可以查看8080端口显示了网页，且该端口是暴露外网的

