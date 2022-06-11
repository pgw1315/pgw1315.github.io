
---
title: Selenium超级详细的教程
date: 2022-06-09 11:18:11
author: Will
img: /images/banner/web-scraping-with-selenium-aind-python.png
categories: Python
tags:
  - Python
  - 爬虫
  - 浏览器
---
        
## 前言

相信搞过Python的人绝大部分都会一点点爬虫技能，但是很多时候爬虫也不是万能的，这个时候就需要我们的自动化测试框架了，于是Selenium就应运而生了，它可以算的上是自动化测试框架中的佼佼者，因为它解决了大多数用来爬取页面的模块的一个永远的痛，那就是Ajax异步加载，今天小编就带大家来好好了解下这个Selenium 。
## 一、安装与导入

这里我们需要安装三个东西，一个是Selenium框架，还有一个浏览器，最后就是驱动。这里小编选择了谷歌浏览器。然后Selenium框架嘛，大家都会下的啦，PIP就搞定了，最后就是要下载个Chrome浏览器的驱动程序，为了让Selenium和浏览器之间产生关联的一个东西，下载地址：[https://registry.npmmirror.com/binary.html?path=chromedriver/](https://registry.npmmirror.com/binary.html?path=chromedriver/)。安装好浏览器后，将浏览器驱动放在浏览器同级目录下，这样前期工作就算都预备好了。
注：不要随便乱下浏览器和驱动，每个浏览器和驱动器的版本都必须是一一对应的，不是通用的。
## 二、与浏览器建立连接

做好前面的准备工作，我们只需要写入几行Python代码即可与浏览器进行交互，如下：
```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的驱动，并启动Chrome浏览器
c.get('https://www.baidu.com')#打开百度
```

## 三、查找元素

对于操作浏览器中的页面的自动化测试框架来说，肯定少不了 去发现网页中的元素，你只有发现那些元素实时存在了才能做出下一步的操作。Selenium中提供了众多的方法供我们去找到网页中的元素，这给我们带来了很大的便利，那么都有哪些方法了，我们可以通过Python快速获取到这些方法：
```python
find_element                         #通过指定方法查找指定的一个元素(需指定两个参数)
find_element_by_class_name           #通过Class name查找指定的一个元素
find_element_by_css_selector         #通过CSS选择器查找指定的一个元素
find_element_by_id                   #通过ID查找指定的一个元素
find_element_by_link_text            #通过链接文本获取指定的一个超链接(精确匹配)
find_element_by_name                 #通过Name查找指定的一个元素
find_element_by_partial_link_text    #通过链接文本获取指定的一个超链接(模糊匹配)
find_element_by_tag_name             #通过标签名查找指定的一个元素
find_element_by_xpath                #通过Xpath语法来指定的一个元素
find_elements                        #通过指定方法查找所有元素(需指定两个参数)
find_elements_by_class_name          #通过Class name查找所有元素
find_elements_by_css_selector        #通过CSS选择器查找所有元素
find_elements_by_id                  #通过ID查找所有元素
find_elements_by_link_text           #通过链接文本获取所有超链接(精确匹配)
find_elements_by_name                #通过Name查找所有元素
find_elements_by_partial_link_text   #通过链接文本获取所有超链接(模糊匹配)
find_elements_by_tag_name            #通过标签名查找所有元素
find_elements_by_xpath               #通过Xpath语法来查找所有元素
```

可以看到输入框的ID为KW，Name为WD，这里我们选择ID，选择ID有三种方法，如下：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
c=webdriver.Chrome(executable_path=r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')
c.get('https://www.baidu.com')
kw1=c.find_element(By.ID,'kw')
kw2=c.find_element_by_id('kw')
kw3=c.find_elements_by_id('kw')[0]
print(kw1)
print(kw2)
print(kw3)
```

可以看到我们成功使用三种方法获取到了这个元素，其它方法差不多，都是一通百通，喜欢哪种方法就使用哪种方法。

## 四、浏览器操作

### 1.获取本页面URL

```python
c.current_url
```

### 2.获取日志

```python
c.log_types  #获取当前日志类型
c.get_log('browser')#浏览器操作日志
c.get_log('driver') #设备日志
c.get_log('client') #客户端日志
c.get_log('server') #服务端日志
```

### 3.窗口操作
```python
c.maximize_window()#最大化
c.fullscreen_window() #全屏
c.minimize_window() #最小化
c.get_window_position() #获取窗口的坐标
c.get_window_rect()#获取窗口的大小和坐标
c.get_window_size()#获取窗口的大小
c.set_window_position(100,200)#设置窗口的坐标
c.set_window_rect(100,200,32,50)    #设置窗口的大小和坐标
c.set_window_size(400,600)#设置窗口的大小
c.current_window_handle   #返回当前窗口的句柄
c.window_handles         #返回当前会话中的所有窗口的句柄
```

### 4.设置延时
```python
c.set_script_timeout(5) #设置脚本延时五秒后执行
c.set_page_load_timeout(5)#设置页面读取时间延时五秒
```

### 5.关闭
```python
c.close() #关闭当前标签页
c.quit() #关闭浏览器并关闭驱动
```

### 6.打印网页源代码
```python
c.page_source
```

### 7.屏幕截图操作
```python
c.save_screenshot('1.png')#截图，只支持PNG格式
c.get_screenshot_as_png() #获取当前窗口的截图作为二进制数据
c.get_screenshot_as_base64() #获取当前窗口的截图作为base64编码的字符串
```

### 8.前进后退刷新
```python
c.forward() #前进
c.back()  #后退
c.refresh()#刷新
```

### 9.执行JS代码

在Selenium中也可以自定义JS代码并带到当前页面中去执行，如下：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.get('https://www.baidu.com')
kw1=c.find_element(By.ID,'kw')
c.execute_script("alert('hello')")
time.sleep(3)
c.quit()

```

这里我使用一个JS中的函数来执行屏幕提示的功能，成功被执行。

### 10.Cookies操作

```python
c.get_cookie('BAIDUID') #获取指定键的Cookies
c.get_cookies()         #获取所有的Cookies
for y in c.get_cookies():
   x=y
   if x.get('expiry'):
       x.pop('expiry')     
   c.add_cookie(x) #添加Cookies  
c.delete_cookie('BAIDUID') #删除指定键的Cookies内容
c.delete_all_cookies() #删除所有cookies
```

### 11.获取标题内容

```python
c.title
```

### 12.获取当前浏览器名

```python
c.name
```

### 13.全局超时时间

```python
c.implicitly_wait(5)
```

## 五、元素操作

对我们找到的元素进行二次操作，不仅可以再次选择子元素还可以进行其它操作。如下：
```python
kw1.clear()        #清除元素的值
kw1.click()        #点击元素
kw1.id             #Selenium所使用的内部ID
kw1.get_property('background') #获取元素的属性的值
kw1.get_attribute('id') #获取元素的属性的值
kw1.location       #不滚动获取元素的坐标
kw1.location_once_scrolled_into_view  #不滚动且底部对齐并获取元素的坐标
kw1.parent         #父元素
kw1.send_keys('')  #向元素内输入值
kw1.size           #大小
kw1.submit         #提交
kw1.screenshot('2.png') #截取元素形状并保存为图片
kw1.tag_name       #标签名
kw1.text           #内容，如果是表单元素则无法获取
kw1.is_selected()  #判断元素是否被选中
kw1.is_enabled()   #判断元素是否可编辑
kw1.is_displayed   #判断元素是否显示
kw1.value_of_css_property('color') #获取元素属性的值
kw1._upload('2.png') #上传文件

```

## 六、键盘鼠标操作

### 1.模拟键盘输入和按键

```python
from selenium.webdriver.common.keys import Keys
c.find_element(By.ID,'kw').send_keys('python')#输出Python
c.find_element(By.ID,'kw').send_keys(Keys.ENTER)#回车键
c.find_element(By.ID,'kw').click()#点击
```

这里列举出了一个最简单的键盘输入和鼠标点击的例子，不过我们在Selenium中可以使用更为高逼格的操作，那么是什么了？当然是咱们的鼠标键盘监听事件来触发事件啦，而且它里面的方法的确也很多样化，满足小编日常的骚操作不在话下，如下所示：
```python
click(on_element=None)                 #鼠标左键单击
click_and_hold(on_element=None)        #单击鼠标左键，不松开
context_click(on_element=None)         #单击鼠标右键
double_click(on_element=None)          #双击鼠标左键
drag_and_drop(source,target)           #拖拽到某个元素然后松开
drag_and_drop_by_offset(source,xoffset,yoffset) #拖拽到某个坐标然后松开
key_down(value,element=None)     #按下键盘上的某个键
key_up(value, element=None)      #松开键盘上的某个键
move_by_offset(xoffset, yoffset)  #鼠标从当前位置移动到某个坐标
move_to_element(to_element)        #鼠标移动到某个元素
move_to_element_with_offset(to_element, xoffset, yoffset) #移动到距某个元素（左上角坐标）多少距离的位置
pause(seconds)                  #暂停所有输入(指定持续时间以秒为单位)
perform()                       #执行所有操作
reset_actions()                 #结束已经存在的操作并重置
release(on_element=None)       #在某个元素位置松开鼠标左键
send_keys(*keys_to_send)        #发送某个键或者输入文本到当前焦点的元素
send_keys_to_element(element, *keys_to_send) #发送某个键到指定元素
```

以上就是咱们鼠标和键盘的全部操作了，小编将用一个例子带大家零基础入门。如下：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
c=webdriver.Chrome(executable_path=r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')
c.get('https://www.baidu.com')
a=ActionChains(c)
kw1=c.find_element(By.ID,'kw')
tj=c.find_element(By.ID,'su')
tj.send_keys(Keys.CONTROL,'c') #复制
a.drag_and_drop(kw1,tj).perform()#从输入框拖动到搜索按钮
kw1.send_keys(Keys.CONTROL,'v')#粘贴
tj.send_keys(Keys.ENTER)
time.sleep(3)
c.close()
c.quit()
```

这里我们通过对事件的监控，进行复制和粘贴，这里涉及到一个组合键的知识，大家注意。
## 七、选项操作

我们可以通过给当前操作的对象一些选项来增强交互能力，如下：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from selenium.webdriver.chrome.options import Options
o=Options()
o.add_argument('--headless')#无界面浏览
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe'),options=o) #获取chrome浏览器的

c.get('https://www.baidu.com')
kw1=c.find_element(By.ID,'kw')
print(c.title)
time.sleep(3)
c.close()
c.quit()
```

这个时候就实现了咱们的无界面浏览了，也就是不用打开浏览器即可自动返回执行的结果。不过你可别以为选项就那么一两个，那可是多到你怀疑人生的，例如：
```python
o.add_argument('--window-size=600,600') #设置窗口大小
o.add_argument('--incognito') #无痕模式
o.add_argument('--disable-infobars') #去掉chrome正受到自动测试软件的控制的提示
o.add_argument('user-agent="XXXX"') #添加请求头
o.add_argument("--proxy-server=http://200.130.123.43:3456")#代理服务器访问
o.add_experimental_option('excludeSwitches', ['enable-automation'])#开发者模式
o.add_experimental_option("prefs",{"profile.managed_default_content_settings.images": 2})#禁止加载图片
o.add_experimental_option('prefs',
{'profile.default_content_setting_values':{'notifications':2}}) #禁用浏览器弹窗
o.add_argument('blink-settings=imagesEnabled=false')  #禁止加载图片
o.add_argument('lang=zh_CN.UTF-8') #设置默认编码为utf-8
o.add_extension(create_proxyauth_extension(
           proxy_host='host',
           proxy_port='port',
           proxy_username="username",
           proxy_password="password"
       ))# 设置有账号密码的代理
o.add_argument('--disable-gpu')  # 这个属性可以规避谷歌的部分bug
o.add_argument('--disable-javascript')  # 禁用javascript
o.add_argument('--hide-scrollbars')  # 隐藏滚动条
o.binary_location=r"C:\Users\Administrator\AppData\Local\Google\Chrome\Application" #指定浏览器位置
o.add_argument('--no-sandbox')  #解决DevToolsActivePort文件不存在的报错
```

其实选项的添加无非就是分为以下这几种，如下：

```python
o.set_headless()          #设置启动无界面化
o.binary_location(value)  #设置chrome二进制文件位置
o.add_argument(arg)               #添加启动参数
o.add_extension(path)                #添加指定路径下的扩展应用
o.add_encoded_extension(base64)      #添加经过Base64编码的扩展应用
o.add_experimental_option(name,value)         #添加实验性质的选项
o.debugger_address(value)                #设置调试器地址 
o.to_capabilities()                    #获取当前浏览器的所有信息
```

虽然选项很多，但是我们真正能用到的不多，一般就是无痕模式或者禁用JavaScript和图片来快速获取到相关信息。虽然我们上面使用的是Options方法，但是在实际应用中建议大家使用的ChromeOptions方法。

## 八、框架操作(Frame/IFrame)

我们还可以操作框架里的东西，比如IFrame，Frame等等，虽然都是框架，但是这两者操作起来还是有很大差别的。下面我们就来看看吧，如下：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.implicitly_wait(10)
c.get('https://hao.360.com/?a1004')
#ss=c.find_element(By.CLASS_NAME,'NEWS_FEED_VIDEO_1595850774217HPA70')#不容易找到标签
c.switch_to.frame(0)#索引
c.switch_to.frame('NEWS_FEED_VIDEO_1595850774217HPA70-VideoIframe') #ID
c.switch_to.frame('NEWS_FEED_VIDEO_1595850774217HPA70')#Class
c.switch_to.frame(c.find_element_by_tag_name("iframe"))#标签
time.sleep(3)
c.close()
c.quit()
```

这里小编是以360浏览器的主页为例子，对它里面的IFrame进行访问，最有效的方法一般就是我上面提到的四种了。这里我们有时候因为这个框架需要加载才可以出来，所以很多时候是无法获取到的，因此我们只有使用滑动加载到出现这个标签或者ID，Class为可以获取到，这在刚才小编是说了的，大家可以往前看看，不过这个方法不推荐使用，为啥？因为开发者文档上是这样写的。我们的Frame由于是IFrame里的子集，所以上面的方法便是可以层层遍历的好方法，但是如果我们遍历到最后了如何返回主框架了，可以这样做，如下所示：
```python
c.switch_to.default_content()
```

这样就可以回到主框架继续进行操作了。如果我们由里往外遍历的话，那么可以这样来做，如下：
```python
c.switch_to.parent_frame()
```

## 九、Alert

在弹窗处理中，我们会遇到三种情况，如下：
浏览器弹出框
+ 新窗口弹出框
+ 人为弹出框
那么我们该怎么分辨了？下面跟我一起看看吧。

### 1.浏览器弹出框

首先说说浏览器弹出框，想必大家对JavaScript中的Alert，Confirm，Prompt应该不是很陌生，就是弹出框，确认框，输入框；基本方法我们来看下，如下：
```python
from selenium.webdriver.common.alert import Alert
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.implicitly_wait(10)
c.get('https://www.baidu.com')
a1=Alert(c)
a1.accept() #确定
a1.dismiss() #取消
a1.authenticate(username,password) #用户身份验证
a1.send_keys('') #输入文本或按键
a1.text  #获取弹窗内容
```

这里我们应对每种情况它上面的方法的对应位置都是会有所变化的，所以我们需要根据具体情况来进行操作，而且还可以使用另一种方法，如下：
```python
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.implicitly_wait(10)
c.get('https://www.baidu.com')
a1=c.switch_to_alert()
a1.accept() #确定
a1.dismiss() #取消
a1.authenticate(username,password) #用户身份验证
a1.send_keys('') #输入文本或按键
a1.text  #获取弹窗内容
```

注：该类方法必须在有弹框的情况下才有作用，如没有会报错。
### 2.新窗口弹出框

上面就是浏览器弹出框的处理方法了，如果是新窗口弹出的话那么就不一样了，我们需要通过句柄来定位，前面我们提到过这两个方法。下面我们来看看它们的具体用法，如下：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.implicitly_wait(10)
c.get('https://www.baidu.com')
kw1=c.find_element(By.ID,'kw')
tj=c.find_element(By.ID,'su')
hwnd=c.window_handles #所有窗口句柄
for h in hwnd:
   if h !=c.current_window_handle:  #如果句柄不是当前窗口句柄则切换                          c.switch_to_window(h)  #切换窗口
   else:
       print('无需切换窗口') 
time.sleep(3)
c.close()
c.quit()
```

注：如果有多个窗口，当你关闭了当前窗口想切换到另一个窗口，你需要把没关闭的窗口切换成当前活动窗口，因为Selenium是不会为你做这件事的。
### 3.人为弹出框

这类弹出框是我们自己开发的，一般都是使用Div包裹一些其它的元素标签然后形成一个整体，当我们触发某个事件的时候就会出现，否则消失。这种弹出框使用我们的众多Find前缀的方法就能遍历到，很方便，这里不一一细说。
## 十、判断

在Selenium中我们在做自动化测试时常无法判断一个元素是否真的显示出来了，因此会各种报错，接下来我们对这些操作进行判断，如果显示出了我们预期的值，那么就进行下一步操作，否则就关闭或者暂停几秒然后再判断，这里我要跟大家说Selenium中的一个模块-----Expected_Conditions，简称为EC，如下所示：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
import time
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.implicitly_wait(10)
c.get('https://baidu.com')
t=EC.title_is('百度一下，你就知道')
print(t(c))
time.sleep(3)
c.close()
c.quit()
```

这里其实就是判断当前页面的标题是否是我们给定的文本，可以看出这里为True，说明是。它不光就一个方法哦，还有其它的，小编在这里大致说下，如下所示：
```python
EC.title_contains('')(c)#判断页面标题是否包含给定的字符串
EC.presence_of_element_located('')(c) #判断某个元素是否加载到dom树里，该元素不一定可见
EC.url_contains('')(c) #判断当前url是否包含给定的字符串
EC.url_matches('')(c) #匹配URL
EC.url_to_be('')(c)  #精确匹配
EC.url_changes('')(c) #不完全匹配
EC.visibility_of_element_located('')(c) #判断某个元素是否可见,可见代表元素非隐藏元素
EC.visibility_of('')(c)   #跟上面一样，不过是直接传定位到的element
EC.presence_of_all_elements_located('')(c) #判断是否至少有1个元素存在于dom树中
EC.visibility_of_any_elements_located('')(c) #判断是否至少一个元素可见，返回列表
EC.visibility_of_all_elements_located('')(c) #判断是否所有元素可见，返回列表
EC.text_to_be_present_in_element('')(c) #判断元素中的text是否包含了预期的字符串
EC.text_to_be_present_in_element_value('')(c)#判断元素中value属性是否包含预期的字符串
EC.frame_to_be_available_and_switch_to_it('')(c) # 判断该frame是否可以switch进去
EC.invisibility_of_element_located('')(c) #判断某个元素是否不存在于dom树或不可见
EC.element_to_be_clickable('')(c) #判断某个元素中是否可见并且可点击
EC.staleness_of('')(c)  #等某个元素从dom树中移除
EC.element_to_be_selected('')(c)  #判断某个元素是否被选中了,一般用在下拉列表
EC.element_located_to_be_selected('')(c) #判断元组中的元素是否被选中
EC.element_selection_state_to_be('')(c) #判断某个元素的选中状态是否符合预期
EC.element_located_selection_state_to_be('')(c) #跟上面一样，只不过是传入located
EC.number_of_windows_to_be('')(c)  #判断窗口中的数字是否符合预期
EC.new_window_is_opened('')(c)  #判断新的窗口是否打开
EC.alert_is_present('')(c)  #判断页面上是否存在alert
```

这就是它全部的方法了，简直不要多简单。
## 十一、选择

刚刚讲过判断，现在我们来说说选择，选择无非就是挑好的扔烂的，顺着思路来不会错，总体来讲还是挺简单的，如下：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.select import Select
import time
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.implicitly_wait(10)
c.get('http://www.juliwz.cn/forum.php')
s=Select(c.find_element_by_id('ls_fastloginfield'))#实例化
res=s.all_selected_options#全部选中子项
res1=s.options#全部子项
print(res)
print(res1)
time.sleep(3)
c.close()
c.quit()
```

发觉主流网站都没有Select这个标签，于是找了个很冷门的网站，就一个Select。Select里面的方法也是相当多的，如下：
```python
s.first_selected_option  #第一个选中的子项
s.select_by_index(index) #根据索引选择
s.select_by_value(value)   #根据值来选择
s.select_by_visible_text(text)  #根据选项可见文本
s.deselect_by_index(index)   #根据索引来取消选择
s.deselect_by_value(value)   #根据值来取消选择
s.deselect_by_visible_text(text)  #根据可见文本来取消选择
s.deselect_all()                #取消所有选择
```

## 十二、显示等待和隐式等待

想必大家应该听过这个概念，显示等待就是浏览器在我们设置的时间内不断寻找，等到元素后才继续执行，如果没在规定时间内找到，也会抛异常；而隐式等待则是我们设置时间，然后程序去找元素，期间会不断刷新页面，到了时间仍然没找到就抛异常。这里有个常用的模块专门用来实现显示等待和隐式等待的，它就是”wait“,我们来看看吧。如下：
```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from selenium.webdriver.chrome.service import Service
c=webdriver.Chrome(service=Service(r'C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chromedriver.exe')) #获取chrome浏览器的
c.get('https://www.baidu.com/')
su=WebDriverWait(c,10).until(lambda x:x.find_element_by_id('su')) 
su.location_once_scrolled_into_view
print(su.get_attribute('value'))
time.sleep(3)
c.close()
c.quit()
```

隐式等待很简单，就一行代码，如下：
```python
c.implicitly_wait(10)
```

它的等待时间适用于全局的环境，也就是任何地方找不到某个元素，它都将发挥作用，如果找得到，则不会产生作用。
## 十三、总结

Selenium的内容其实还是挺丰富的，它还有手机端的自动化测试框架，不过小编先把WEB端讲完就可以了，毕竟也写了这么多了，日后有时间再慢慢了解。大家如果是对触摸活动事件感兴趣的也可以看看“touch_actions"这个模块。这个模块里集成的都是关于触摸屏的操作，里面也有很多的方法，小编之所以能把Selenium一天学完，还是Selenium模块中的文档比较给力，主要是介绍的比较好，让人能轻松联想到方法的使用，感激开源作者的无私奉献。

## 参考文章
[Selenium超级详细的教程](https://zhuanlan.zhihu.com/p/343948620)