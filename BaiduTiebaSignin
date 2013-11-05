# -*- coding: utf-8 -*-  
# ===author: sylsaint===

import urllib
import urllib2
import cookielib 
import time
import random
import logging
import threading
import re

#检查cookies
def checkAllCookiesExist(cookieNameList, cookieJar) :
    cookiesDict = {}
    for eachCookieName in cookieNameList :
        cookiesDict[eachCookieName] = False
     
    allCookieFound = True
    for cookie in cookieJar :
        if(cookie.name in cookiesDict) :
            cookiesDict[cookie.name] = True
     
    for eachCookie in cookiesDict.keys() :
        if(not cookiesDict[eachCookie]) :
            allCookieFound = False
            break
 
    return allCookieFound
#正则匹配模块
def r1(pattern, text):
    m = re.search(pattern, text)
    if m:
        return m.group(1)
#登陆贴吧
def tieba_login():  
    cookiejar = cookielib.CookieJar()  
    opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookiejar))
    opener.addheaders = [('Host', 'passport.baidu.com'),  
                         ('User-Agent', 'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:19.0) Gecko/20100101 Firefox/19.0'),  
                         ('Accept', 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8'),  
                         ('Accept-Language', 'zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3'),
                         ('Connection', 'keep-alive'),  
                         ('Referer', 'http://www.baidu.com/'),
                         ('Content-Type','application/x-www-form-urlencoded')]
    login_url = r'https://passport.baidu.com/v2/api/?login'
    print "[step1] to get cookie BAIDUID"
    baiduMainUrl = "http://www.baidu.com/"
    resp = opener.open(baiduMainUrl)
    print "[step2] to get token value"
    getapiUrl = "https://passport.baidu.com/v2/api/?getapi&class=login&tpl=mn&tangram=true"
    getapiResp = opener.open(getapiUrl)
    getapiRespHtml = getapiResp.read()
    foundTokenVal = re.search("bdPass\.api\.params\.login_token='(?P<tokenVal>\w+)'", getapiRespHtml)
    if(foundTokenVal):
        tokenVal = foundTokenVal.group("tokenVal")
        print "tokenVal=",tokenVal
        print "[step3] emulate login baidu"
        staticpage = "http://tieba.baidu.com/tb/static-common/html/pass/v3Jump.html"
        baiduMainLoginUrl = "https://passport.baidu.com/v2/api/?login"
        postDict = {
            #'ppui_logintime': "",
            'charset'       : "utf-8",
            #'codestring'    : "",
            'token'         : tokenVal, #de3dbf1e8596642fa2ddf2921cd6257f
            'isPhone'       : "false",
            'index'         : "0",
            #'u'             : "",
            #'safeflg'       : "0",
            'staticpage'    : staticpage, #http%3A%2F%2Fwww.baidu.com%2Fcache%2Fuser%2Fhtml%2Fjump.html
            'loginType'     : "1",
            'tpl'           : "mn",
            'callback'      : "parent.bdPass.api.login._postCallback",
            'username'      : 'limerary@126.com',
            'password'      : '11131214s',
            #'verifycode'    : "",
            'mem_pass'      : "on",
        }
        post_data = urllib.urlencode(postDict)
        opener.open(login_url, post_data)
        cookiesToCheck = ['BDUSS', 'PTOKEN', 'STOKEN', 'SAVEUSERID']
        loginBaiduOK = checkAllCookiesExist(cookiesToCheck, cookiejar)
        if(loginBaiduOK):
            print "+++ Emulate login baidu is OK, ^_^"
        else:
            print "--- Failed to emulate login baidu !"
    return opener  
opener = tieba_login()
class BaiduTieba(threading.Thread):
    def __init__(self,tiebaName):
        self.tiebaName = tiebaName 
        threading.Thread.__init__(self)
    def run(self):
        self.SignIn(self.tiebaName)
    def SignIn(self,tiebaName):
        signurl = 'http://tieba.baidu.com/sign/add'
        url = 'http://tieba.baidu.com/f?ie=utf-8&kw=' + urllib.quote(tiebaName)
        content = opener.open(url).read()
        tbs = r1(r'PageData.tbs[\s]*=[\s]*"(\w+)"', content)
        postData = urllib.urlencode({'tbs':str(tbs),'ie':'utf-8','kw':tiebaName})
        timeline = time.time()
        while (time.time() - timeline) < 120:
            opener.open(signurl, postData)
            print self.name + " Begin to sign in " + urllib.quote(tiebaName) + " tieba."

def run():
    while 1:
        t1 = time.ctime().split(' ')[3]
        timedata = int(t1.split(':')[0])*3600 + int(t1.split(':')[1])*60 + int(t1.split(':')[2])
        if timedata > 12*3600+39*60+30:
            print 'now time is on !'
            for i in range(5):
                hefei = BaiduTieba('合肥')
                hefei.start()
            for i in range(5):
                liyi = BaiduTieba('李毅')
                liyi.start()
        else:
            print 'not at the right time !'
            time.sleep(5)
if __name__ == '__main__':
    print time.ctime()
    time.sleep(360)
    for i in range(5):
        hefei = BaiduTieba('合肥')
        hefei.start()
    for i in range(5):
        liyi = BaiduTieba('李毅')
        liyi.start()
