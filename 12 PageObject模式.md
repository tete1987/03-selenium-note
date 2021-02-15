# 十二、PageObject模式
## （一）pageObject原则
1.定义：
PageObject设计模式：是将某个页面的所有"元素（包含控件）属性"及"元素操作"封装在1个类(Class)里面，以page为单位进行管理。

2.原则：

1）用公共方法代表UI所提供的功能

2）方法应该返回其他的PageObject或者返回用于断言的数据

3）同样的行为不同的结果可以建模为不同的方法

4）不要在方法内加断言

5）不要暴露页面内部的元素给外部

6）不需要建模UI内的所有元素，只为页面中重要的元素创建page类

## （二）selenium企业微信实战
1.复用已有浏览器功能：

1）浏览器：（在cmd中启动）
```
chrome -remote-debugging-port=9222
```
2）python：
```
chrome_arg = webdriver.ChromeOptions()
chrome_arg.debugger_address='127.0.0.1:9222'
self.driver=webdriver.Chrome(option=chrome_arg)
```

示例：
```python
from time import sleep

from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By


class TestChrome:
    def setup_method(self,method):
        option = Options()
        option.debugger_address = '127.0.0.1:9222'
        self.driver = webdriver.Chrome(options=option)

    def teardown(self):
        self.driver.quit()

    def test_chrome(self):
        self.driver.get("https://work.weixin.qq.com/wework_admin/frame")
        self.driver.find_element(By.XPATH,'//*[@id="menu_contacts"]/span').click()
        sleep(3)
```

2.使用cookie登录浏览器

1）先使用复用浏览器的方式登录，并获取cookie，打印出来（print(self.driver.get_cookies())）

2）再将cookie存到变量中，其中 expiry字段是有效期，可能会影响登录，所以需要先遍历一下，然后删除，代码如下：
```python
class TestChrome:
    def setup_method(self, method):
        option = Options()
        option.debugger_address = '127.0.0.1:9222'
        self.driver = webdriver.Chrome()
        self.driver.maximize_window()
        self.driver.implicitly_wait(5)

    def teardown(self):
        self.driver.quit()

    def test_cookie(self):
        # print(self.driver.get_cookies())
        cookies = {'domain': '.work.weixin.qq.com', 'expiry': 1638411313, 'httpOnly': False,
                                 'name': 'Hm_lvt_9364e629af24cb52acc78b43e8c9f77d', 'path': '/', 'secure': False,
                                 'value': '1606811058,1606874353,1606875304'}, {'domain': '.qq.com',
                                                                                'expiry': 1606962817, 'httpOnly': False,
                                                                                'name': '_gid', 'path': '/',
                                                                                'secure': False,
                                                                                'value': 'GA1.2.887702860.1606811058'}, {
                                    'domain': '.qq.com', 'expiry': 1606876466, 'httpOnly': False, 'name': '_gat',
                                    'path': '/', 'secure': False, 'value': '1'}, {'domain': '.qq.com',
                                                                                  'expiry': 1669948417,
                                                                                  'httpOnly': False, 'name': '_ga',
                                                                                  'path': '/', 'secure': False,
                                                                                  'value': 'GA1.2.1123723369.1606811058'}, {
                                    'domain': '.work.weixin.qq.com', 'expiry': 1609468420.311611, 'httpOnly': False,
                                    'name': 'wwrtx.i18n_lan', 'path': '/', 'secure': False, 'value': 'zh'}, {
                                    'domain': '.work.weixin.qq.com', 'expiry': 1638347048.359358, 'httpOnly': False,
                                    'name': 'wwrtx.c_gdpr', 'path': '/', 'secure': False, 'value': '0'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': True, 'name': 'wwrtx.refid',
                                    'path': '/', 'secure': False, 'value': '27184059492011728'}, {'domain': '.qq.com',
                                                                                                  'expiry': 2147385600,
                                                                                                  'httpOnly': False,
                                                                                                  'name': 'pgv_pvi',
                                                                                                  'path': '/',
                                                                                                  'secure': False,
                                                                                                  'value': '2033898496'}, {
                                    'domain': '.qq.com', 'expiry': 2147483640.977564, 'httpOnly': False, 'name': 'RK',
                                    'path': '/', 'secure': False, 'value': 'uqj4btYTEa'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': False, 'name': 'wxpay.vid',
                                    'path': '/', 'secure': False, 'value': '1688850785700311'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': True, 'name': 'wwrtx.ltype',
                                    'path': '/', 'secure': False, 'value': '1'}, {'domain': 'work.weixin.qq.com',
                                                                                  'expiry': 1606905863.695738,
                                                                                  'httpOnly': True, 'name': 'ww_rtkey',
                                                                                  'path': '/', 'secure': False,
                                                                                  'value': '15k11f8'}, {
                                    'domain': '.qq.com', 'expiry': 2147483640.977684, 'httpOnly': False, 'name': 'ptcz',
                                    'path': '/', 'secure': False,
                                    'value': 'f416945781245808a65612452c26fe42b33c64ac1771380fa02f46b3600919e3'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': True, 'name': 'wwrtx.ref', 'path': '/',
                                    'secure': False, 'value': 'direct'}, {'domain': '.work.weixin.qq.com',
                                                                          'httpOnly': False, 'name': 'wwrtx.d2st',
                                                                          'path': '/', 'secure': False,
                                                                          'value': 'a2382445'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': True, 'name': 'wwrtx.sid', 'path': '/',
                                    'secure': False,
                                    'value': 'SsjRpQMl9IX4iWi5eOG0Su9rPRfcUxY5glEos9Iom6akPCPfy5vDKYWV1JBStQsw'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': False, 'name': 'wxpay.corpid',
                                    'path': '/', 'secure': False, 'value': '1970325111201446'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': True, 'name': 'wwrtx.vst', 'path': '/',
                                    'secure': False,
                                    'value': 'kh329F94GdkgRDzYlbPI2qFgR75YgKOHma3967osbvpNweaVi1mDZs-bWGZ_8cQEB0Vk_ojUCFq3PJBH99zSNUuesJdDrp5a0wqcbCwlUBBUKo8bj0HggYAygn5mMd_RT8rZMtnGB-FCkLK5dnrxrE26c-BHpX7llaReX2Eu0fmRYRt8WNlDGjJ48esikg5rg6KN3iXQjx6C1xKHqQOv_AwBRnFRcMeISrQQrGFFK_paZVRO7Casj6NfyIzUwwhnaxPlDRo_bJxy0YuT01AHow'}, {
                                    'domain': '.work.weixin.qq.com', 'httpOnly': False, 'name': 'wwrtx.vid',
                                    'path': '/', 'secure': False, 'value': '1688850785700311'}
        print(self.driver.get_cookies())
        self.driver.get("https://work.weixin.qq.com/wework_admin/frame")
        for cookie in cookies:
            if "expiry" in cookie.keys():
                cookie.pop("expiry")
            self.driver.add_cookie(cookie)
        self.driver.get("https://work.weixin.qq.com/wework_admin/frame")
        self.driver.find_element(By.XPATH, '//*[@id="menu_contacts"]/span').click()
        sleep(3)

```

3）将cookie值保存到shelve中，相当于一个小型数据存储
```python
import shelve
from time import sleep

from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By


class TestChrome:
    def setup_method(self, method):
        option = Options()
        option.debugger_address = '127.0.0.1:9222'
        self.driver = webdriver.Chrome()

        self.driver.implicitly_wait(5)

    def teardown(self):
        self.driver.quit()

    def test_cookie(self):
        self.driver.get("https://work.weixin.qq.com/wework_admin/frame")
        db= shelve.open("cookies")
        #第一次获取cookie值时使用以下语句，复用时需注释
        # db['cookie']=self.driver.get_cookies()
        cookies =db['cookie']

        for cookie in cookies:
            if "expiry" in cookie.keys():
                cookie.pop("expiry")
            self.driver.add_cookie(cookie)
        self.driver.get("https://work.weixin.qq.com/wework_admin/frame")
        db.close()
        self.driver.find_element(By.XPATH, '//*[@id="menu_contacts"]/span').click()
        sleep(3)
```

## （三）使用PageObject模式进行测试用例设计
1.企业微信首页pageObject

1）立即注册
- 点击立即注册
- return 立即注册page

2）企业登录
- 点击企业登录
- return 企业登录pageObject

2.登录PageObject

1）扫码
- 用手机扫码

2）立即注册
- 点击立即注册
- return 立即注册page

3.立即注册pageObject

1）填表
- 输入文本
- 下拉框
- 点击确定

示例：
```python
1）新建index.py：
from selenium import webdriver
from selenium.webdriver.common.by import By

from selenium_code.wx_code01.login import Login
from selenium_code.wx_code01.register import Register

class Index:
    def __init__(self):
        self._driver = webdriver.Chrome()
        self._driver.get("https://work.weixin.qq.com/")
        self._driver.implicitly_wait(5)
        self._driver.maximize_window()

    def goto_login(self):
        #点击登录
        self._driver.find_element(By.CSS_SELECTOR,'.index_top_operation_loginBtn').click()
        return Login(self._driver)

    def goto_register(self):
        #点击立即注册
        self._driver.find_element(By.CSS_SELECTOR,'.index_head_info_pCDownloadBtn').click()
        return Register(self._driver)
        
```

2）新建login.py：
```python
from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver

from selenium_code.wx_code01.register import Register


class Login:
    def __init__(self,driver:WebDriver):
        #python3 特性，类似标签
        self._driver = driver

    def scanf(self):
        #扫码登录，暂不解决
        pass

    def goto_register(self):
        #点击立即注册
        self._driver.find_element(By.CSS_SELECTOR,'.login_registerBar_link').click()
        return Register()
```

3）新建register.py：
```python
from time import sleep

from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver


class Register:
    def __init__(self,driver:WebDriver):
        self._driver = driver

    def register(self):
        self._driver.find_element(By.CSS_SELECTOR,'#corp_name').send_keys("aaaaa")
        self._driver.find_element(By.XPATH,'//*[@id="corp_industry"]/a/span[1]').click()
        self._driver.find_element_by_link_text("IT服务").click()
        self._driver.find_element_by_link_text("计算机软件/硬件/信息服务").click()
        sleep(2)
        self._driver.find_element(By.ID,'manager_name').send_keys("管理员A")
        sleep(3)
        self._driver.quit()
        return True
```
4）新建执行的测试用例：test_register.py
```python
from selenium_code.wx_code01.index import Index


class TestRegister:
    def setup(self):
        self.index = Index()

    def test_register(self):
        self.index.goto_register().register()
```


### （四）使用PageObject模式进行测试用例设计2（企业微信增加成员）
1.初始版本：

1).新建首页文件（main_pege.py）：
```python
from selenium.webdriver.common.by import By

from selenium_code.wx_PageObject02.Page.addMember import AddMember


class Index:
    def __init__(self):
        option = Options()
        option.debugger_address = '127.0.0.1:9222'
        self._driver = webdriver.Chrome(options=option)
        self._driver.get("https://work.weixin.qq.com/wework_admin/frame#index")
        self._driver.implicitly_wait(5)

    def teardown(self):
        self._driver.quit()

    def goto_add_member(self):
        self._driver.find_element(By.CSS_SELECTOR,'.index_service_cnt_item:nth-child(1)').click()
        return AddMember(self._driver)
```

2)新建“添加成员”的文件(addMember.py)：
```python
from time import sleep

from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver


class AddMember:
    def __init__(self,driver:WebDriver):
        self._driver= driver

    def add_member(self):
        sleep(2)
        self._driver.find_element(By.ID,'username').send_keys("AAAA")
        self._driver.find_element(By.ID,'memberAdd_acctid').send_keys("AAAAA")
        self._driver.find_element(By.ID,'memberAdd_phone').send_keys("18700090008")
        self._driver.find_element(By.CSS_SELECTOR,'.js_btn_save').click()
        sleep(2)
        return True
```

3)新建添加成员的测试用例(test_addMember.py)
```python
from selenium_code.wx_PageObject02.Page.main_page import Index


class TestAddMember:
    def setup(self):
        self.main_page = Index()

    def test_addMember(self):
        assert self.main_page.goto_add_member().add_member()
```

2.优化断言版本：

1)修改addMember.py 文件
```python
from time import sleep

from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver


class AddMember:
    def __init__(self,driver:WebDriver):
        self._driver= driver

    def add_member(self):
        sleep(2)
        self._driver.find_element(By.ID,'username').send_keys("AAAA")
        self._driver.find_element(By.ID,'memberAdd_acctid').send_keys("AAAAA")
        self._driver.find_element(By.ID,'memberAdd_phone').send_keys("18700090008")
        self._driver.find_element(By.CSS_SELECTOR,'.js_btn_save').click()
        sleep(2)
        return True


    def get_member(self):
        elements = self._driver.find_elements(By.CSS_SELECTOR,'.member_colRight_memberTable_td:nth-child(2)')
        list =[]
        for ele in elements:
            list.append(ele.get_attribute("title"))

        return list
```
或
```python
from time import sleep

from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver


class AddMember:
    def __init__(self,driver:WebDriver):
        self._driver= driver

    def add_member(self):
        sleep(2)
        self._driver.find_element(By.ID,'username').send_keys("AAAA")
        self._driver.find_element(By.ID,'memberAdd_acctid').send_keys("AAAAA")
        self._driver.find_element(By.ID,'memberAdd_phone').send_keys("18700090008")
        self._driver.find_element(By.CSS_SELECTOR,'.js_btn_save').click()
        sleep(2)
        return True


    def get_member(self):
        elements = self._driver.find_elements(By.CSS_SELECTOR,'.member_colRight_memberTable_td:nth-child(2)')
        return [ele.get_attribute("title") for ele in elements]

```
2)修改test_addMember.py文件
```python
from time import sleep

from selenium_code.wx_PageObject02.Page.main_page import Index


class TestAddMember:
    def setup(self):
        self.main_page = Index()

    def test_addMember(self):
        add_member = self.main_page.goto_add_member()
        add_member.add_member()
        sleep(2)
        assert "AAAA" in add_member.get_member()
```

3.优化公共类版本（将公共的类或方法写一个base_page.py文件中）：

1）base_page文件的内容：
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.remote.webdriver import WebDriver

class BasePage:
    _driver = None
    _base_url = ""

    def __init__(self,driver:WebDriver=None):
        if driver is None:
            option = Options()
            option.debugger_address = '127.0.0.1:9222'
            self._driver = webdriver.Chrome(options=option)
            self._driver.implicitly_wait(5)

        else:
            self._driver=driver

        if self._base_url != "" :
            self._driver.get(self._base_url)
```

2）修改main_page.py文件：
```python
from time import sleep

from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By

from selenium_code.wx_PageObject02.Page.addMember import AddMember
from selenium_code.wx_PageObject02.Page.base_page import BasePage


class Index(BasePage):
    _base_url = "https://work.weixin.qq.com/wework_admin/frame#index"


    def teardown(self):
        self._driver.quit()

    def goto_add_member(self):
        self._driver.find_element(By.CSS_SELECTOR,'.index_service_cnt_item:nth-child(1)').click()
        return AddMember(self._driver)
```

3）修改add_memeber.py文件：
```python
from time import sleep

from selenium.webdriver.common.by import By
from selenium.webdriver.remote.webdriver import WebDriver

from selenium_code.wx_PageObject02.Page.base_page import BasePage


class AddMember(BasePage):

    def add_member(self):
        sleep(2)
        self._driver.find_element(By.ID,'username').send_keys("AAAA")
        self._driver.find_element(By.ID,'memberAdd_acctid').send_keys("AAAAA")
        self._driver.find_element(By.ID,'memberAdd_phone').send_keys("18700090008")
        self._driver.find_element(By.CSS_SELECTOR,'.js_btn_save').click()
        sleep(2)
        return True

    def get_member(self):
        elements = self._driver.find_elements(By.CSS_SELECTOR,'.member_colRight_memberTable_td:nth-child(2)')
        list =[]
        for ele in elements:
            list.append(ele.get_attribute("title"))

        return list
```

4.继续优化：将查找元素进行封装：

1）修改base_page.py 文件：
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.remote.webdriver import WebDriver


class BasePage:
    _driver = None
    _base_url = ""

    def __init__(self,driver:WebDriver=None):
        if driver is None:
            option = Options()
            option.debugger_address = '127.0.0.1:9222'
            self._driver = webdriver.Chrome(options=option)
            self._driver.implicitly_wait(5)

        else:
            self._driver=driver

        if self._base_url != "" :
            self._driver.get(self._base_url)

    def find(self,by ,locator):
        return self._driver.find_element(by ,locator)

    def finds(self,by ,locator):
        return self._driver.find_elements(by ,locator)
```

2）修改addMember.py 文件
```python
from time import sleep
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions
from selenium_code.wx_PageObject02.Page.base_page import BasePage


class AddMember(BasePage):

    def add_member(self):
        sleep(2)
        self.find(By.ID,'username').send_keys("AAAA")
        self.find(By.ID,'memberAdd_acctid').send_keys("AAAAA")
        self.find(By.ID,'memberAdd_phone').send_keys("18700090008")
        self.find(By.CSS_SELECTOR,'.js_btn_save').click()
        sleep(2)
        return True

    def get_member(self):
        elements = self.finds(By.CSS_SELECTOR,'.member_colRight_memberTable_td:nth-child(2)')
        list =[]
        for ele in elements:
            list.append(ele.get_attribute("title"))

        return list
```

3）修改main_page.py 文件
```python
from time import sleep
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium_code.wx_PageObject02.Page.addMember import AddMember
from selenium_code.wx_PageObject02.Page.base_page import BasePage


class Index(BasePage):
    _base_url = "https://work.weixin.qq.com/wework_admin/frame#index"

    def teardown(self):
        self._driver.quit()

    def goto_add_member(self):
        self.find(By.CSS_SELECTOR,'.index_service_cnt_item:nth-child(1)').click()
        return AddMember(self._driver)
```

5.加入显式等待，并进行封装：

1）修改base_page.py 文件：
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.remote.webdriver import WebDriver
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions

class BasePage:
    _driver = None
    _base_url = ""

    def __init__(self,driver:WebDriver=None):
        if driver is None:
            option = Options()
            option.debugger_address = '127.0.0.1:9222'
            self._driver = webdriver.Chrome(options=option)
            self._driver.implicitly_wait(5)

        else:
            self._driver=driver

        if self._base_url != "" :
            self._driver.get(self._base_url)

    def find(self,by ,locator):
        return self._driver.find_element(by ,locator)

    def finds(self,by ,locator):
        return self._driver.find_elements(by ,locator)

    def wati_for_click(self,locator,time=10):
        WebDriverWait(self._driver,time).until(expected_conditions.element_to_be_clickable(locator))
```

2）修改addMember.py 文件
```python
from time import sleep
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions
from selenium_code.wx_PageObject02.Page.base_page import BasePage


class AddMember(BasePage):

    def add_member(self):
        
        self.find(By.ID,'username').send_keys("AAAA")
        self.find(By.ID,'memberAdd_acctid').send_keys("AAAAA")
        self.find(By.ID,'memberAdd_phone').send_keys("18700090008")
        self.find(By.CSS_SELECTOR,'.js_btn_save').click()
        return True

    def update_page(self):
        content: str = self.find(By.CSS_SELECTOR, '.ww_pageNav_info_text').text
        return [int(x) for x in content.split('/',1)]

    def get_member(self,value):
        self.wati_for_click((By.CSS_SELECTOR,'.ww_checkbox'))
        cur_page,total_page = self.update_page()
        while True:
            elements = self.finds(By.CSS_SELECTOR, '.member_colRight_memberTable_td:nth-child(2)')
            for ele in elements:
                if value == ele.get_attribute("title"):
                    return True

            cur_page=self.update_page()
            if cur_page == total_page:
                return False
            self.find(By.CSS_SELECTOR,'.js_next_page').click()
```

3）修改main_page.py 文件
```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions
from selenium_code.wx_PageObject02.Page.addMember import AddMember
from selenium_code.wx_PageObject02.Page.base_page import BasePage


class Index(BasePage):
    _base_url = "https://work.weixin.qq.com/wework_admin/frame#index"

    def teardown(self):
        self._driver.quit()

    def goto_add_member(self):
        self.find(By.CSS_SELECTOR,'#menu_contacts').click()
        self.wati_for_click((By.LINK_TEXT,'添加成员'))
        self.find(By.LINK_TEXT,'添加成员').click()
        return AddMember(self._driver)
```

4）修改test_addMember.py
```python
from selenium_code.wx_PageObject02.Page.main_page import Index


class TestAddMember():
    def setup(self):
        self.main_page = Index()

    def test_addMember(self):
        add_member = self.main_page.goto_add_member()
        add_member.add_member()
        assert add_member.get_member("李四")

```
