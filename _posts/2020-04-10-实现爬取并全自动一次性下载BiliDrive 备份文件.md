---
layout:     post
title:      实现爬取并全自动一次性下载BiliDrive文件
subtitle:   
date:       2020-04-10 16:40:56
author:     terese
header-img: img/post-6.jpg
catalog:   true
tags:
    - 高效工具
---

## 实现爬取并全自动一次性下载BiliDrive文件

工具就是为懒人发明的...今天想在BiliDrive上下载知识星球精华，一个个粘贴链接太累了，于是用python实现爬取并全自动一次性下载BiliDrive 备份文件...这里以简书上的一篇文章为例，用正则表达式匹配出link后利用python中调用os 的命令行，实现自动下载link文件。

**一般BiliDrive 备份文件下载步骤**

终端命令行输入：

```pip install BiliDriveEx```

```bdex download <link>```

注：\<link>以bdex://开头

**python实现爬取并全自动一次性下载BiliDrive 文件步骤**

终端命令行输入：

```pip install BiliDriveEx```

运行python脚本

```python
from selenium import webdriver
from selenium.common.exceptions import TimeoutException,NoSuchElementException
import os
import re
import sys


def save_the_article(title,novel):
    with open('./抓取页面内容.txt','w') as f:
        f.write(title)
        f.write('\n')
        f.write(novel)
        f.write("\n"*2+"=="*50+"=="*50+"\n"*1)
    print('保存至文本成功！')

def get_article(article_url):
    try:
        browser.get(article_url)
    except TimeoutException:
        print('Time out!')
    try:
        novel = browser.find_element_by_class_name('ouvJEz')
        title = browser.find_element_by_class_name('_1RuRku')
        title = title.text
        novel = novel.text
        print(title)
        save_the_article(title,novel)
    except NoSuchElementException:
        print('No such element!')

if __name__ == '__main__':
    #在简书抓取bdex链接，这里只以爬取一个页面为例子
    browser = webdriver.Chrome()
    article_url = 'https://www.jianshu.com/p/f0c3812b32f8'
    get_article(article_url)
    browser.close()

    #实现全自动一次性下载BiliDrive 备份文件
    capture_file  = open('./抓取页面内容.txt')
    str1 = capture_file.read()
    info = re.findall(r'(bdex://\S+.*?)',str1)
    print(info)
    for i in info:
        # 命令行下载
        os.system('bdex download '+i) #执行成功 返回 0
        # 输出重定向：在控制台的输出重定向到 txt文本文件中
        output = sys.stdout
        outputfile = open('./命令行执行返回结果.txt', 'w')
        sys.stdout = outputfile
```

output：

![image-20200410164204116](https://tva1.sinaimg.cn/large/00831rSTgy1gdori488r6j30he01c74a.jpg)

![image-20200410165930608](https://tva1.sinaimg.cn/large/00831rSTgy1gdos09suxyj30yx0bq0y5.jpg)

