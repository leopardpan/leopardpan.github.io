---
layout: post
title: "Python自动化测试iOS项目"
date: 2016-08-04 
tags: python  
---

作为一个开发人员，为了保证自己的代码的健壮，写单元测试是必不可少的环节，然而最痛快的是每天去手动跑一遍所有的case。那么什么能帮我们解决这些繁琐的操作呢，大家应该会想到自动化测试脚本了，是的，我们可以借助脚本来完成全自动化测试，下面是我列的每天脚本自动执行流程：       

 >* 1、`pull` git仓库里面的最新代码到本地。    
 >* 2、然后打包成`App`。   
 >* 3、安装到模拟器上。    
 >* 4、运行App，执行单元测试，生成测试数据并保存到本地。    
 >* 5、脚本读取测试数据，邮件发送给相关人员。    


当这些全自动化后，可以大大减少开发人员的维护成本，即使每次项目里面有新增模块后，增加测试case就行了，下面会介绍自动测试这5步具体怎么去执行，整个脚本是使用Python写的，代码很少功能也很简单，但这已经可以帮我们完成基本的自动化测试了，这就是脚本的强大之处，选择Pyhton纯属个人喜好，最近也在学习Python，当然了最终使用什么语言看你自己。   

### python执行shell命令完成测试       

首先确认本机上安装了`git` 和 `python` 。    
脚本判断本地是否存在项目，不存在则使用命令 `git clone ...` ，存在则使用命令 `git pull ...` 。       
这些在Linux的命令都可以使用脚本来完成的，python的 `os.popen()` 方法 就是可以在Linux上执行shell命令。     
**例如：**  把下面这段代码添加到一个 test.py 的文件里，然后在终端上执行 `python test.py` 命令你就会看到，你的当前目录下正在下载我的博客了。

```     
import os

os.popen('git clone https://github.com/leopardpan/leopardpan.github.io.git')   

```       
git pull 。。。 更新代码也是一样的。

接下来的打包、安装、运行都是使用python执行shell命令      

**把iOS项目打包成App，下面的 `Demo` 是项目的名字**              

>* os.popen('xcodebuild -project Demo.xcodeproj -target Demo -configuration Debug -sdk iphonesimulator')	 

这行脚本运行完成后，你就会发现同会生成一个 `build` 的文件夹。  
Debug参数表示现在是Debug模式，如果Xcode里面改成Release了，这里需要改成Release。  
xcodebuild 命令是 Xcode Command Line Tools 的一部分。通过调用这个命令，可以完成 iOS 工程的编译，打包和签名过程。可以使用 xcodebuild --help 来看看具体有哪些功能。 

**打开iOS模拟器，这里运行的是`iPhone 6 Plus` 你也可以换成其它型号的模拟器**      

>*  os.popen('xcrun instruments -w "iPhone 6 Plus"')	

**把刚才打包生成的App安装到模拟器上**      
在安装之前要先卸载App,不然你运行的永远是最初安装的那个，后来安装的不会覆盖之前的，卸载App

>* os.popen('xcrun simctl uninstall booted com.test.Demo')

booted 后面接的是 `Bundle Identifier`，我的是 com.test.Demo，然后再安装App     

>* os.popen('xcrun simctl install booted build/Debug-iphonesimulator/Demo.app ')	

booted 后面接的是.app的路径，我打包的时候的是Debug，所以这个的文件夹名称是Debug-iphonesimulator。

**在模拟器里运行App**      

>* os.popen('xcrun simctl launch booted com.test.Demo')

booted 后面接的是 `Bundle Identifier`，我的是 com.test.Demo。

到目前为止，你就会发现你的项目已经运行起来了，你可以在项目是Debug模式下一启动就执行单元测试，然后把对应的测试数据保存到本地为data.json。然后在使用python脚本读取测试文件的数据，最终使用邮件发送给相关人员，pyhton读取数据很简单，一行代码就行

>* data = open('data.json').read() 

data里面就是json字符串，为了脚本操作简单，我在存储的时候直接把json格式的转成了字符串类型。

### python发送邮件     

我使用的是SMTP进行邮件发送的，SMTP是发送邮件的协议，Python内置对SMTP的支持，可以发送纯文本邮件、HTML邮件以及带附件的邮件。     

Python对SMTP支持有smtplib和email两个模块，email负责构造邮件，smtplib负责发送邮件，具体代码如下： 


	from email import encoders
	from email.header import Header
	from email.mime.text import MIMEText
	from email.utils import parseaddr, formataddr
	import smtplib

	def format_addr(self,s):
	    name, addr = parseaddr(s)
	    return formataddr(( \
	        Header(name, 'utf-8').encode(), \
	        addr.encode('utf-8') if isinstance(addr, unicode) else addr))

	def send_mail(self, mail, message, title):
		from_addr = 'leopardpan@163.com'
		password = ''
		to_addr = mail
		smtp_server = 'smtp.163.com'

		msg = MIMEText(message, 'plain', 'utf-8')
		msg['From'] = self.format_addr(u'自动化测试邮件 <%s>' % from_addr)
		msg['To'] = self.format_addr(u'管理员 <%s>' % to_addr)
		msg['Subject'] = Header(title, 'utf-8').encode()

		server = smtplib.SMTP(smtp_server, 25)
		server.set_debuglevel(1)
		server.login(from_addr, password)
		server.sendmail(from_addr, [to_addr], msg.as_string())
		server.quit()

	send_mail('leopardpan@icloud.com','正文','标题')


from_addr是发送方的邮箱地址，password是开通SMTP时输入的密码     
smtp_server是smtp的服务，如果你的from_addr是gamil.com，那么就要写成smtp_server = 'smtp.gmail.com' 了。

方法 send_mail(self, mail, message, title): 有四个参数，第一个不用传，第二个参数是收信人的邮箱，第三个是邮件的正文，第四个是邮件的标题，方法调用格式： `send_mail('leopardpan@icloud.com','正文','标题')`

注意：发送方的邮箱必须要开通SMTP功能才行，否则会报错

>* SMTPSenderRefused: (550, 'User has no permission', 'leopardpan@163.com')

163的SMTP开通，需要你登录网易邮箱，然后点击顶部的设置就会出现`POP3/SMTP/IMAP`，点击之后，勾选选择开启，这个时候需要你输入密码，记住这个密码就是上面代码中的`password`，如果你都完成的话，你把上面的代码拷贝出现，把邮箱修改成你自己的，使用 pyhton 运行一下吧。


上面的几个流程结合起来就可以实现一个简单的自动化测试了，如果你有什么建议和意见欢迎讨论。


参考链接：
[SMTP发送邮件](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386832745198026a685614e7462fb57dbf733cc9f3ad000)     

<br>

转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2016/08/PythonTestAutomationiOS/) 

 



