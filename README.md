#  学生健康打卡

<img src='https://img.shields.io/badge/Version-1.0.2-green' style='float:left; width:100px'/>

`Jnu-StuHealth  `模拟滑块实现打卡项目

本项目实现自动打卡建议自备一台**连续不断运行**的服务器，该项目是在**ubuntu**上面实现的。

本项目的设想是必须**开通邮件通**知，因为上去检查下今天打卡没与设计概念**背道而驰**

因为验证码具有短暂的时效性，后改用了**生产者与消费者**模式，**即产即消**！

##  责任说明 😊

> *最终解释权由发布者持有

若您**切换了定位**，请及时手动打卡一次即可。

本项目初衷是避免**重复性**，但请自己确保**真实性**。

本项目已于Github进行**开源/共享**,秉承技术无界限的原则

一切使用该项目造成的后果应由**使用者负责**

##  Docker部署(推荐) 🏖️

> 仅限于x86架构，同我arm架构的话就采用下面的快速部署

请跳转到对应的分支下👉 https://github.com/HengY1Sky/Jnu-Stuhealth/tree/docker

##  快速部署 🚀

> 特别注意⚠️：
>
> selenium中的谷歌版本存在BUG即chromedriver的无头版本会报错❌
>
> `window.initNECaptcha`会说找不到的问题，但是**火狐**是没问题的。

授权码的获取简单给个链接🔗： https://www.cnblogs.com/kimsbo/p/10671851.html

```bash
# git下载
$ sudo git clone https://github.com/HengY1Sky/Jnu-Stuhealth clock
$ cd clock

# 安装依赖
$ pip install -r requirements.txt

# 安装了firefox
$ apt update && apt upgrade # 更新包 
$ apt install firefox

# 安装geckodriver https://github.com/mozilla/geckodriver/releases
$ wget https://github.com/mozilla/geckodriver/releases/download/v0.30.0/geckodriver-v0.30.0-linux64.tar.gz
$ tar -zxvf ./geckodriverxxx  # 解压下来
$ cp ./geckodriver /usr/bin/geckodriver  # 丢到环境中去必要赋予权限
$ chmod 755 /usr/bin/geckodriver

# 写入 账号 密码 邮箱 备注
# 写入邮箱与授权码
$ vim dayClock.txt
$ vim utils.py # SEND_EMAIL AUTH_REGISTERED 设置通知邮箱📮以及授权码

# 为了corntab找到路径
$ cp -r ./hideHeader /home/ubuntu
$ chmod -R 777 /home/ubuntu/hideHeader

# 运行
$ python app.py
```

## 文件结构 📁

```
├── app.py  # 入口运行文件
├── bgImg # 背景图片
│
├── dayClock.txt  # 保存打卡账号密码文件
├── handlePackage.py # 处理发包
├── handleValidate.py # 处理验证码
│
├── log  # 输出日志
│   
├── requirements.txt # 依赖文件
└── utils.py  # 仓库
```

##  定时开启任务 ⏰

```bash
# 开启定时
# 参考链接 https://blog.csdn.net/longgeaisisi/article/details/90477975
$ sudo apt-get install cron
$ crontab -l # 是否安装以及已有任务
$ service cron start # 开启cron
$ crontab -e # 选择3
# 将  1 0 * * * /usr/bin/python /home/ubuntu/clock/app.py  写入注意修改路径
$ service cron restart
# 建议将app文件中的记录日志的路径写为绝对的
```

##  注意事项 ⚠️

1. 当遇到说`webp`·文件不识别的时候： `pip install --upgrade pillow`升级下就好了
2. 当出现`state code 1`时候，在当前目录下打开`geckodriver.log`查看情况进行修复
3. 当`root`用户无法使用，`sudo crontab -u ubuntu -e`为ubuntu用户开启定时任务

> 其他问题请[谷歌](https://www.google.com.hk/)解决～ 代码问题请提交PR或者开Issue

##  更新日志

最新描述：**添加Docker容器**

<details>
<summary>20220326</summary>
<h3>添加Docker容器</h3>

为了快速上手使用，造福大家免除配置环境的烦恼。
~~被迫~~花了点时间搭建了个容器，记得点个星星。
</details>

<details>
<summary>20220325</summary>
<h3>添加表格参数</h3>

新的表格出现了早/中/晚的昨日测量数据
现在已经加上且附带35.5-36.5的随机体温
</details>


<details>
<summary>20220227</summary>
<h3>紧急修复Crontab问题</h3>

加入拓展之后并在Crontab下执行会路径发生问题，经过我的排查在当前文件夹下使用 `cp -r ./hideHeader /home/ubuntu`并赋予执行权限 `chmod -R 777 /home/ubuntu/hideHeader`即可。至于路径不统一，查看[我的博客](https://hengy1.top/article/2c7b2295.html)简单配置即可找到问题所在。
</details>

<details>
<summary>20220226</summary>
<h3>重构发包与修改日志记录</h3>

当初年少不懂事，写的代码自尝苦果，写的不好自己现在重新写下。
新版发出，敬请谅解，多多指教～

- 重构发包
- 修改日志记录
</details>

<details>
<summary>20220225</summary>
<h3>修改方式以及优化部分代码</h3>

受到小透明的启发，发现利用油猴方式是可以在不降低版本的情况下进行浏览器头部的绕过，以及发现部分代码存在可以优化空间。
正如小透明所说，降低版本是一个不明智的决定，抱歉～

- 添加上了绕过方式
- 添加了Logging日志记录格式
- 暂时修复Connection aborted
</details>

<details><summary>20220215</summary>
<h3>动模块检测window.navigator修复(废弃)</h3>

收到邮箱错误，上去Debug发现只要是自动浏览器总是错的，不可能是对的。由于issue#1680  链接：https://github.com/mozilla/geckodriver/issues/1680

**作者在上面说**：And that is because the WebDriver spec defines that property on the Navigator object, which has to be set to true 
when tests are running with webdriver enabled. **即在88.0版本以上之后geckodriver不提供window.navigator.webdriver设置为None**

如果我发现有像谷歌存在`execute_cdp_cmd`的方法**我会第一时间更新**，所以总的思路已经有了。

``` bash
$ apt remove firefox # 卸载最新版本的firefox
$ wget https://ftp.mozilla.org/pub/firefox/releases/87.0b1/linux-x86_64/zh-CN/firefox-87.0b1.tar.bz2
$ tar -jxvf firefox-87.0b1.tar.bz2 # 会有个firefox文件
$ cd firefox
$ ./firefox --version # 查看版本
$ pwd # 查看firefox在哪里
$ sudo echo "export PATH=/home/ubuntu/firefox:$PATH" >  /etc/profile # 写入
$ source /etc/profile
$ firefox --version # 查看版本
$ python handleValidate.py # 如果为None就是成功了的(执行时注意用户)
# 关闭自动更新 https://blog.csdn.net/yu1014745867/article/details/79639440
$ crontab -e # 重新编辑
# 1 0 * * * . /etc/profile;/usr/bin/python /home/ubuntu/clock/app.py 将这句话改成你的对应路径写入
```
