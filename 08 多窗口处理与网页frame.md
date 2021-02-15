# 八、多窗口处理与网页frame
## （一）selenium里面如何处理多窗口处理
1、多个窗口识别

1）点击某些链接，会重新打开一个窗口，对于这种情况，想在新页面上操作，就得先切换窗口了。

2）获取窗口的唯一标识用句柄表示，所以只需要切换句柄，就可以多个页面灵活操作了。

3）多窗口处理流程：
- 先获取到当前的窗口句柄（driver.current_window_handle）
- 再获取到所有的窗口句柄（driver.window_handles）
- 判断是否想要操作的窗口，如果是，就可以对窗口进行操作，如果不是，跳转到另外一个窗口，对另一个窗口进行操作（drvier.swich_to_window）

2、多个窗口之间切换-案例

1）打开百度页面

2）点击登录

3）弹框中点击“立即注册”，输入用户名和账号

4）返回刚才的登录页，点击登录

5）输入用户名和密码，点击登录

示例：
```python
from time import sleep

from selenium.webdriver.common.by import By

from selenium_code.base import Base


class TestWindow(Base):
    def test_window(self):
        self.driver.get("https://www.baidu.com/")
        self.driver.find_element_by_link_text("登录").click()
        print("登录页面",self.driver.current_window_handle)
        print("所有页面：",self.driver.window_handles)
        sleep(3)
        self.driver.find_element_by_link_text("立即注册").click()
        print("立即注册页面：",self.driver.current_window_handle)
        print("所有页面：",self.driver.window_handles)
        windows = self.driver.window_handles
        self.driver.switch_to_window(windows[-1])
        sleep(2)
        self.driver.find_element(By.ID,'TANGRAM__PSP_4__userName').send_keys("aaaaa")
        self.driver.find_element(By.ID,'TANGRAM__PSP_4__phone').send_keys("18900389202")
        sleep(2)
        self.driver.switch_to_window(windows[0])
        self.driver.find_element(By.ID,'TANGRAM__PSP_11__footerULoginBtn').click()
        self.driver.find_element(By.ID,'TANGRAM__PSP_11__userName').send_keys("dddd")
        self.driver.find_element(By.ID,'TANGRAM__PSP_11__password').send_keys("234345")
        self.driver.find_element(By.ID,'TANGRAM__PSP_11__submit').click()
        sleep(3)
```

## （二）selenium里面如何处理frame
1.frame介绍：
在web自动化中，如果一个元素定位不到，那么很大可能是在iframe中。

1）什么是iframe？
- frame是html中的框架，在html中，所谓的框架就是可以在同一浏览器中显示不止一个页面。
- 基于html的框架，又分为垂直框架和水平框架（cols，rows）

2）Frame分类
- frame标签包含frameset，frame，iframe三种。
- frameset和普通的标签一样，不会影响整体的定位，可以使用index、id、name、webelement任意一种方式定位frame。
- 而frame与iframe 对selenium定位而言是一样的。selenium有一组方法对frame进行操作。

2.多个frame识别

1）frame存在两种：
一种是嵌套，一种是未嵌套的。

2）切换frame
- driver.switch_to.frame() ——根据元素id或者index切换frame
- driver.switch_to.default_content()——切换到默认frame
- driver.switch_to.parent_frame()——切换到父级

3）处理未嵌套的iframe：
- driver.switch_to_frame("frame 的 id")
- driver.switch_to_frame("frame-index") frame 无id的时候依据索引来处理，索引从0开始 driver.switch_to_frame(0)

4）处理嵌套的iframe：
- 对于嵌套的先进入到iframe的父节点，再进到子节点，然后可以对子节点里面的对象进行处理和操作。
- driver.switch_to.frame("父节点")
- driver.switch_to.frame("子节点")

3.多个frame之间切换-案例

1）打开包含frame的web页面https://www.runoob.com/try/try.php?filename=jqueryui-api-droppable

2）打印“请拖拽我”元素的文本

3）打印“点击运行”元素的文本

示例：
```python
from selenium_code.base import Base

class TestFrame(Base):
    def test_frame(self):
        self.driver.get("https://www.runoob.com/try/try.php?filename=jqueryui-api-droppable")
        #先进入到iframe中
        self.driver.switch_to_frame("iframeResult")
        #打印“请拖拽我”元素的文本
        print(self.driver.find_element_by_id("draggable").text)
        #再回到原来默认的iframe中
        self.driver.switch_to_default_content()
        #打印“点击运行”元素的文本
        print(self.driver.find_element_by_id("submitBTN").text)

```
