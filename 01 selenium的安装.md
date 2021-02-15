# 一、selenium的安装
## 1.selenium的介绍：
1)简介：selenium支持web浏览器的自动化。它主要由三个工具构成：webdriver、IDE、Grid

2)官方网站：https://www.selenium.dev/

![selenium](https://github.com/tete1987/picture_resource/blob/master/selenium/selenium1.png)

2.selenium环境配置步骤

&emsp;1）准备好python环境

&emsp;2）准备好selenium环境

&emsp;3）下载浏览器对于的driver版本

&emsp;4）driver配置环境变量

&emsp;5）在python中import对应的依赖

3.selenium的安装

&emsp;1）前提：
- 配置好python环境
- 配置好pip工具

&emsp;2）安装：
```
pip install selenium
或者在pycharm直接安装
```
4.Driver的配置

1）Driver的介绍：https://www.selenium.dev/documentation/en/webdriver/driver_requirements/

2）Driver的下载：
- 淘宝镜像：https://npm.taobao.org/mirrors/chromedriver/
- 官方网站：https://chromedriver.storage.googleapis.com/index.html

3）Driver的安装：
- 找到和自己浏览器版本适配的driver版本
- 导入到环境变量中

5.python中如何使用
```
import selenium
from selenium import webdriver
def test_selenium()：
    driver = webdriver.Chrome()
    driver.get("https://www.baidu.com")

```
