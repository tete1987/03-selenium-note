# 四、selenium三种等待方式
## （一）直接等待
1.强制等待，线程休眠一定时间
```
time.sleep(3)
```
## （二）隐式等待
1.设置一个等待时间，轮询查找（默认0.5秒）元素是否出现，如果没出现就抛出异常
- self.driver.implicity_wait(3)
- 这个等待时间是全局的。

## （三）显式等待
1.在代码中定义等待条件，当条件发生时才继续执行代码
- ‘WebDriverWait’配合until() 和 until_not() 方法，根据判断条件进行等待
- 程序每隔一段时间（默认为0.5秒）进行条件判断，如果条件成立，则执行下一步，否则继续等待，知道超过设置的最长的时间。

示例：
```python
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions


class TestWait():
    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.get("https://ceshiren.com/")
        self.driver.implicitly_wait(3)

    def teardown(self):
        self.driver.quit()

    def test_wait(self):
        self.driver.find_element(By.XPATH, '//*[@id="ember31"]/a').click()
        WebDriverWait(self.driver, 10).until(
            expected_conditions.visibility_of_element_located((By.XPATH, '//*[@class="link-top-line"]/a')))
        self.driver.find_element(By.XPATH, '//*[@class="link-top-line"]/a').click()

```
