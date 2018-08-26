title: 'Selenium Headless Automated Testing'
date: 2016-08-14 10:13:30
tags: Python/Ruby
---

关于ubuntu的介绍如下：

http://www.installationpage.com/selenium/how-to-run-selenium-headless-firefox-in-ubuntu/


1. 如果使用的是yum源系统，安装如下的包：

```
yum install xorg-x11-server-Xvfb.x86_64
```

2. 然后执行如下，启动xvfb服务：

```
Xvfb :10 -ac
```

3. 在系统中以headless方式启动centos

```
export DISPLAY=:10
firefox
```

4.  执行selenium测试：
python code 如下：

```
from selenium import webdriver
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.support.ui import WebDriverWait # available since 2.4.0
from selenium.webdriver.support import expected_conditions as EC # available since 2.26.0

# Create a new instance of the Firefox driver
driver = webdriver.Firefox()

# go to the baidu home page
driver.get("http://www.baidu.com")

# the page is ajaxy so the title is originally this:
print driver.title

# find the element that's name attribute is q (the baidu search box)
inputElement = driver.find_element_by_name("wd")

# type in the search
inputElement.send_keys("cheese!")

# submit the form (although baidu automatically searches now without submitting)
inputElement.submit()

try:
    # we have to wait for the page to refresh, the last thing that seems to be updated is the title
    WebDriverWait(driver, 10).until(EC.title_contains("cheese!"))

    # You should see "cheese! - baidu Search"
    print driver.title

finally:
        driver.quit()

```

运行如下：
百度一下，你就知道
cheese!_百度搜索
