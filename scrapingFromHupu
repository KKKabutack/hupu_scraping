from bs4 import BeautifulSoup
import requests
import smtplib
from email.mime.text import MIMEText
from email.utils import formataddr
from email.header import Header


def getPages(key, sender, receivers, pw):
    head = {
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) "
                      "Chrome/109.0.0.0 Safari/537.36"
    }
    baseurl = "https://bbs.hupu.com"
    mylist = []
    for i in range(1, 5):
        url = baseurl + "/502-" + str(i)
        res = requests.get(url=url, headers=head)
        res = BeautifulSoup(res.text, 'html.parser')
        titles = res.select('.bbs-sl-web-post li')
        for i in titles:
            if key in i.text:
                newurl = baseurl + i.a['href']
                print(i.text, newurl)
                mylist.append((i.text, newurl))
    resmsg = ''
    for line in mylist:
        resmsg += line[0]
        resmsg += "(" + line[1] + ")\n"
    sendMail(sender, receivers, pw, resmsg)


def sendMail(sender, receivers, mypassword, subject):
    message = MIMEText(subject, 'plain', 'utf-8')
    message['From'] = formataddr(('虎扑', sender))
    message['Subject'] = Header("虎扑有新内容了", 'utf-8')
    try:
        smtpObj = smtplib.SMTP_SSL('smtp.qq.com', 465)
        smtpObj.login(sender, mypassword)
        smtpObj.sendmail(sender, receivers, message.as_string())
        smtpObj.quit()
    except smtplib.SMTPException:
        pass


def main():
    sender = input("Please provide a valid email address as the sender: ")
    r = input("Please provide a valid email address as the receiver: ")
    receivers = [r]
    pw = input("Please provide the code authorization code of the SMTP service in QQ mail (sender email): ")  # qq邮箱的授权码
    key = input("Which player's news you are looking for?: ")
    getPages(key, sender, receivers, pw)


main()
