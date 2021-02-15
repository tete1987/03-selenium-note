# 二、seleniumIDE用例录制
## （一）下载、安装
1.官网：https://www.selenium.dev/

2.Chrome插件：
https://chrome.google.com/webstore/detail/selenium-ide/mooikfakhdbckjjdioackbalphokd/

3.Firefox插件：
https://addons.mozilla.org/en-US/firefox/addon/selenium-ide/

4.github release：
https://github.com/SeleniumHQ/selenium-ide/release

5.其他版本：
https://addons.mazilla.org/en-GB/firefox/addon/selenium-ide/version/

注意：Chrome插件在国内无法下载，Firefox可以直接下载。

## （二）启动IDE
1.安装完成后，通过在浏览器的菜单栏中点击它的图标来启动它。

2.如果没有看到图标，首先确保是否安装了seleniumIDE扩展插件，其次，可以在下面的地址访问所有插件。
- Chrome：chrome://extensions
- Firefox:about:addons

## （三）IDE的使用
1.创建新项目后，系统将提示为其命名

2.然后要求提供URL：要录制脚本的网站URL。设置一次就可以在整个项目的所有测试中使用。

3.在页面的操作都将记录在IDE中。操作完成后，切换到IDE窗口，并单击停止录制图标；

4.停止后，我们为刚录制的Test取名。

![seleniumIDE1](https://github.com/tete1987/picture_resource/blob/master/selenium/seleniumIDE1.png)

![seleniumIDE2](https://github.com/tete1987/picture_resource/blob/master/selenium/seleniumIDE2.png)

1.新建、保存、打开
2.开始和停止录制
3.运行8中的所有的示例
4.运行单个实例
5.调试模式
6.调整案例的运行速度
7.要录制的网址
8.实例列表
9.动作、目标、值
10.对单条命令的解释
11.运行日志

## （四）管理用例
1.Suite:
- 当Test越来越多时，可以将多个Test归类到Suite中，Suite就像小柜子。
- 创建项目时，IDE会创建一个默认Suite，并将第一个Test添加到其中，你可以点击Test，在下拉菜单中选择Test Suite 进入Suite管理界面；
- 首先进入Suite管理界面，点击“+”，提供名称，然后单击add；
- 将鼠标悬停在suite1上，点击三个点弹出Suite管理菜单；
- 可以对suite1进行管理，包括添加test，重命名，删除，设置，导出。
