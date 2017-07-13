# 调用第三方邮箱发邮件的py脚本

网上找的，原作者不详。这脚本用起来比linux系统自带的sendmail或者postfix强太多，无论是在shell告警脚本中引用，还是用于zabbix都非常好用。

```py
#!/usr/bin/python
#coding:utf-8 

import smtplib
from email.mime.text import MIMEText
import sys 

#邮箱服务器地址
mail_host = 'smtp.163.com'
#邮箱用户名
mail_user = 'testname@163.com'
#邮箱密码
mail_pass = 'passwd'
mail_postfix = '163.com'

def send_mail(to_list,subject,content):
    me = mail_user+"<"+mail_user+"@"+mail_postfix+">"
    msg = MIMEText(content)
    msg['Subject'] = subject
    msg['From'] = me
    msg['to'] = to_list 

    try:
        s = smtplib.SMTP()
        s.connect(mail_host)
        s.login(mail_user,mail_pass)
        s.sendmail(me,to_list,msg.as_string())
        s.close()
        return True
    except Exception,e:
        print str(e)
        return False

if __name__ == "__main__":
    send_mail(sys.argv[1], sys.argv[2], sys.argv[3])
```

---

```py
#!/usr/bin/env python
#-*- coding: UTF-8 -*-
import os,sys
import getopt
import smtplib
from email.MIMEText import MIMEText
from email.MIMEMultipart import MIMEMultipart
from  subprocess import *
def sendqqmail(username,password,mailfrom,mailto,subject,content):

#定义邮箱服务器地址和端口
gserver = 'smtp.qq.com'
gport = 25
try:
msg = MIMEText(unicode(content).encode('utf-8'))
msg['from'] = mailfrom
msg['to'] = mailto
msg['Reply-To'] = mailfrom
msg['Subject'] = subject
smtp = smtplib.SMTP(gserver, gport)
smtp.set_debuglevel(0)
smtp.ehlo()
smtp.login(username,password)
smtp.sendmail(mailfrom, mailto, msg.as_string())
smtp.close()
except Exception,err:
print "Send mail failed. Error: %s" % err
def main():
to=sys.argv[1]
subject=sys.argv[2]
content=sys.argv[3]

##定义QQ邮箱的账号和密码，你需要修改成你自己的账号和密码（请不要把真实的用户名和密码放到网上公开，否则你会死的很惨）
sendqqmail('1234567@qq.com','aaaaaaaaaa','1234567@qq.com',to,subject,content)
if __name__ == "__main__":
main()
```

脚本使用说明：

1. 首先定义好脚本中的邮箱账号和密码
2. python sendmail.py 'test@qq.com' 'Test Mail' 'This is Test Mail' \#三个参数分别是收件人、主题、邮件内容



