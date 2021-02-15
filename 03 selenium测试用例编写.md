# 三、selenium测试用例编写
## （一）用例的关键要素
1.小白入门：https://selenium-python.readthedocs.io/
2.导入依赖
3.创建driver
4.执行自动化步骤
5.断言

## （二）编写测试用例
1.打开页面： https://testerhome.com/
2.点击-社团  标签
3.访问顶部的第一个帖子
```python
import pytest
from selenium import webdriver
from time import sleep

class TestSelenium():
    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.maximize_window()
        #隐式等待
        self.driver.implicitly_wait(3)

    def teardown(self):
        self.driver.quit()

    def test_s01(self):
        self.driver.get("https://testerhome.com/")
        self.driver.find_element_by_link_text("社区").click()
        self.driver.find_element_by_xpath('//a[@title="MTSC2020 深圳大会集中问答帖"]').click()
```

注：隐式等待只能判断元素是否存在，无法判断元素是否可点击。
