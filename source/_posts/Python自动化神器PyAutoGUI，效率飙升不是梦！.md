title: Python自动化神器PyAutoGUI，效率飙升不是梦！
author: l1n6yun
tags: []
categories: []
date: 2024-12-14 13:00:00
---
## 一、PyAutoGUI 是什么

在 Python 的自动化领域中，PyAutoGUI 是一个非常实用的库，它允许我们通过编写代码来模拟鼠标和键盘的操作，从而实现自动化任务 。无论是重复性的日常工作，还是复杂的软件测试流程，PyAutoGUI 都能发挥重要作用，帮助我们节省时间和精力。

想象一下，你需要在某个软件中重复进行一系列的点击、输入操作，要是手动完成，不仅耗时，还容易出错。但有了 PyAutoGUI，你只需编写一个简单的 Python 脚本，就能让计算机自动执行这些任务。比如自动填写表格、批量处理文件、自动化测试软件功能等，这些操作都能轻松实现。

## 二、PyAutoGUI 的安装

在开始使用 PyAutoGUI 之前，我们需要先将其安装到我们的 Python 环境中。安装 PyAutoGUI 非常简单，使用 pip 命令即可完成 。打开你的命令行工具（如 Windows 下的命令提示符、Linux 或 macOS 下的终端），输入以下命令：

```python
pip install pyautogui
```

如果你使用的是 Python 3.9 或更高版本，也可以使用pip3命令进行安装：

```python
pip3 install pyautogui
```

安装过程中，pip 会自动下载 PyAutoGUI 及其依赖项（如 Pillow 库，用于图像处理） 。等待安装完成后，你就可以在 Python 脚本中导入并使用 PyAutoGUI 了。

需要注意的是，在安装之前，请确保你的 Python 环境已经正确配置，并且 pip 已经安装在系统上。如果你在 Windows 操作系统上使用 Python，还要确保已将 Python 添加到系统的环境变量中，以便能够在命令提示符中运行 pip。 如果你在安装过程中遇到问题，可以参考 PyAutoGUI 的官方文档，或者在相关技术论坛上寻求帮助。

## 三、PyAutoGUI 的强大功能展示

### （一）鼠标操作

#### 1. 移动鼠标

在 PyAutoGUI 中，控制鼠标移动主要通过moveTo()和moveRel()函数 。moveTo()函数用于将鼠标移动到屏幕上的指定坐标位置，它的语法如下：

```python
pyautogui.moveTo(x, y, duration=0)
```

其中，x和y是目标坐标的横坐标和纵坐标，duration是可选参数，表示鼠标移动到目标位置所需的时间，单位为秒。如果不设置duration，鼠标会瞬间移动到指定位置。例如，要将鼠标移动到屏幕坐标为 (500, 300) 的位置，可以使用以下代码：

```python
import pyautogui

# 将鼠标移动到坐标(500, 300)，耗时2秒
pyautogui.moveTo(500, 300, duration=2) 
```

moveRel()函数则是相对于当前鼠标位置进行移动，语法为：

```python
pyautogui.moveRel(xOffset, yOffset, duration=0)
```

xOffset和yOffset分别是水平和垂直方向上的偏移量，正数表示向右和向下移动，负数表示向左和向上移动。同样，duration是移动所需的时间。比如，要让鼠标在当前位置的基础上向右移动 100 个像素，向下移动 50 个像素，可以这样写：

```python
import pyautogui

# 鼠标在当前位置基础上，向右移动100像素，向下移动50像素，耗时1秒
pyautogui.moveRel(100, 50, duration=1) 
```

#### 2. 点击操作

PyAutoGUI 提供了多个函数来实现鼠标的点击操作，包括click()、doubleClick()、rightClick()和middleClick()等 。click()函数是最常用的点击函数，它可以模拟鼠标的左键点击、右键点击以及中键点击，还可以设置点击的次数和间隔时间。语法如下：

```python
pyautogui.click(x=None, y=None, clicks=1, interval=0.0, button='left')
```

x和y是点击的坐标位置，如果不指定则在当前鼠标位置点击；clicks表示点击的次数，默认为 1 次；interval是每次点击之间的间隔时间，单位为秒；button指定点击的鼠标按钮，可选值为 'left'（左键，默认值）、'right'（右键）和 'middle'（中键）。例如，要在坐标 (400, 200) 处进行两次左键点击，每次点击间隔 0.5 秒，可以使用以下代码：

```python
import pyautogui

# 在坐标(400, 200)处进行两次左键点击，每次间隔0.5秒
pyautogui.click(400, 200, clicks=2, interval=0.5) 
```

doubleClick()函数专门用于模拟鼠标左键的双击操作，语法为：

```python
pyautogui.doubleClick(x=None, y=None, interval=0.0)
```

x和y是双击的坐标位置，interval是两次点击之间的间隔时间。例如：

```python
import pyautogui

# 在当前鼠标位置进行双击
pyautogui.doubleClick() 
```

rightClick()和middleClick()函数分别用于模拟鼠标右键点击和中键点击，语法类似，只需在调用时传入相应的坐标位置即可。例如：

```python
import pyautogui

# 在坐标(300, 100)处进行右键点击
pyautogui.rightClick(300, 100) 

# 在坐标(200, 150)处进行中键点击
pyautogui.middleClick(200, 150) 
```

#### 3. 鼠标拖拽

实现鼠标拖拽操作的函数是dragTo()和dragRel() 。dragTo()函数将鼠标从当前位置拖动到指定的坐标位置，语法如下：

```python
pyautogui.dragTo(x, y, duration=0, button='left')
```

x和y是目标坐标，duration是拖动所需的时间，button指定拖动时使用的鼠标按钮，默认为左键。比如，要将鼠标从当前位置拖动到坐标 (600, 400) 处，耗时 3 秒，可以使用以下代码：

```python
import pyautogui

# 从当前位置将鼠标拖动到坐标(600, 400)，耗时3秒，使用左键
pyautogui.dragTo(600, 400, duration=3) 
```

dragRel()函数则是相对于当前鼠标位置进行拖动，语法为：

```python
pyautogui.dragRel(xOffset, yOffset, duration=0, button='left')
```

xOffset和yOffset是水平和垂直方向上的偏移量，duration是拖动时间，button是鼠标按钮。例如，要让鼠标在当前位置的基础上，向右拖动 80 个像素，向上拖动 30 个像素，耗时 2 秒，可以这样写：

```python
import pyautogui

# 鼠标在当前位置基础上，向右拖动80像素，向上拖动30像素，耗时2秒，使用左键
pyautogui.dragRel(80, -30, duration=2) 
```

#### 4. 鼠标滚动

控制鼠标滚轮滚动的函数是scroll()，语法如下：

```python
pyautogui.scroll(clicks)
```

clicks是一个整数参数，表示滚动的距离，正数表示向上滚动，负数表示向下滚动。例如，要让鼠标滚轮向上滚动 5 个单位，可以使用以下代码：

```python
import pyautogui

# 鼠标滚轮向上滚动5个单位
pyautogui.scroll(5) 
```

如果要向下滚动 10 个单位，则可以这样写：

```python
import pyautogui

# 鼠标滚轮向下滚动10个单位
pyautogui.scroll(-10) 
```

### （二）键盘操作

#### 1. 按键模拟

在 PyAutoGUI 中，模拟按键按下和释放主要使用press()、keyDown()和keyUp()函数 。press()函数用于模拟按下并释放一个按键，语法如下：

```python
pyautogui.press(key)
```

key是要按下的按键名称，可以是单个字符，如 'a'、'b'，也可以是特殊按键，如 'enter'（回车键）、'esc'（退出键）等。例如，要模拟按下回车键，可以使用以下代码：

```python
import pyautogui

# 模拟按下回车键
pyautogui.press('enter') 
```

keyDown()函数用于模拟按下一个按键，而不释放，语法为：

```python
pyautogui.keyDown(key)
```

keyUp()函数则用于模拟释放一个按键，语法为：

```python
pyautogui.keyUp(key)
```

这两个函数通常一起使用，以实现对按键的精确控制。例如，要模拟按住 Shift 键的同时按下 'a' 键，然后释放 Shift 键，可以这样写：

```python
import pyautogui

# 按下Shift键
pyautogui.keyDown('shift') 

# 按下'a'键
pyautogui.press('a') 

# 释放Shift键
pyautogui.keyUp('shift') 
```

#### 2. 文本输入

实现自动化文本输入的函数是typewrite()，语法如下：

```python
pyautogui.typewrite(message, interval=0.0)
```

message是要输入的文本内容，可以是字符串；interval是可选参数，表示输入每个字符之间的时间间隔，单位为秒。例如，要在当前光标位置输入 "Hello, World!"，每个字符之间间隔 0.2 秒，可以使用以下代码：

```python
import pyautogui

# 输入"Hello, World!"，每个字符间隔0.2秒
pyautogui.typewrite('Hello, World!', interval=0.2) 
```

如果要输入包含特殊按键的组合，比如先按 'enter' 键，再输入 "Python"，可以将按键和文本内容放在一个列表中传递给typewrite()函数，例如：

```python
import pyautogui

# 先按'enter'键，再输入"Python"
pyautogui.typewrite(['enter', 'Python']) 
```

#### 3. 组合键操作

模拟组合键操作可以使用hotkey()函数，语法如下：

```python
pyautogui.hotkey(*keys)
```

keys是要组合的按键名称，可以传递多个参数。例如，要模拟按下 Ctrl+C 组合键（复制操作），可以使用以下代码：

```python
import pyautogui

# 模拟按下Ctrl+C组合键
pyautogui.hotkey('ctrl', 'c') 
```

同样，要模拟按下 Alt+Tab 组合键（切换应用程序），可以这样写：

```python
import pyautogui

# 模拟按下Alt+Tab组合键
pyautogui.hotkey('alt', 'tab') 
```

### （三）屏幕操作

#### 1. 屏幕截图

获取屏幕截图的函数是screenshot()，它可以返回一个表示屏幕截图的 Pillow 图像对象 。语法如下：

```python
im = pyautogui.screenshot()
```

im就是返回的图像对象，你可以对其进行保存、分析等操作。例如，要将屏幕截图保存为名为 "screenshot.png" 的文件，可以使用以下代码：

```python
import pyautogui

# 获取屏幕截图并保存为"screenshot.png"
im = pyautogui.screenshot()im.save('screenshot.png') 
```

如果你只想截取屏幕的某个区域，可以使用region参数指定截取区域的左上角坐标和宽度、高度，语法如下：

```python
im = pyautogui.screenshot(region=(left, top, width, height))
```

left和top是截取区域左上角的横坐标和纵坐标，width和height是截取区域的宽度和高度。例如，要截取屏幕左上角坐标为 (100, 100)，宽度为 200，高度为 150 的区域，可以这样写：

```python
import pyautogui

# 截取指定区域的屏幕截图
im = pyautogui.screenshot(region=(100, 100, 200, 150))im.save('partial_screenshot.png') 
```

#### 2. 图像识别定位

在屏幕上查找指定图像位置的函数主要有locateOnScreen()和locateCenterOnScreen() 。locateOnScreen()函数用于在屏幕上查找指定图像的位置，并返回其边界框（bounding box）的坐标，语法如下：

```python
location = pyautogui.locateOnScreen(image, grayscale=False, confidence=None)
```

image是要查找的图像文件名或 Pillow 图像对象；grayscale是可选参数，设置为True时会以灰度模式查找图像，这样可以提高查找速度，但可能会降低准确性；confidence是可选参数，表示匹配的置信度，取值范围为 0 到 1，值越高表示匹配要求越严格，默认值为None，即不进行置信度匹配。location返回一个包含边界框坐标的四元组(left, top, width, height)，如果未找到图像，则返回None。例如，要在屏幕上查找名为 "button.png" 的图像位置，可以使用以下代码：

```python
import pyautogui

# 在屏幕上查找"button.png"的位置
location = pyautogui.locateOnScreen('button.png')
	if location:    left, top, width, height = location
    	print(f'找到图像，位置为：({left}, {top})，宽度为：{width}，高度为：{height}')
    else:
    	print('未找到图像')
```

locateCenterOnScreen()函数则是在屏幕上查找指定图像的位置，并返回其中心点的坐标，语法如下：

```python
center = pyautogui.locateCenterOnScreen(image, grayscale=False, confidence=None)
```

center返回一个包含中心点坐标的二元组(x, y)，如果未找到图像，则返回None。例如：

```python
import pyautogui
# 在屏幕上查找"icon.png"的中心点位置
center = pyautogui.locateCenterOnScreen('icon.png')
if center:
	x, y = center
    print(f'找到图像，中心点位置为：({x}, {y})')
else:
    print('未找到图像')
```

这些屏幕操作函数结合鼠标和键盘操作函数，可以实现更加复杂的自动化任务，比如根据屏幕上的图像位置进行点击、输入等操作。

## 四、实战应用案例

（一）自动化测试

假设我们正在开发一款简单的图形界面应用程序，其中有一个登录窗口，包含用户名输入框、密码输入框和登录按钮 。我们可以使用 PyAutoGUI 编写自动化测试脚本来模拟用户登录操作，检查应用程序的登录功能是否正常。以下是一个简单的示例代码：

```python
import pyautogui
import time

# 模拟打开应用程序（这里假设应用程序图标在屏幕上的位置已知）
app_icon_location = pyautogui.locateCenterOnScreen('app_icon.png')
if app_icon_location:
    pyautogui.doubleClick(app_icon_location.x, app_icon_location.y)
    time.sleep(3)  # 等待应用程序打开

# 模拟输入用户名和密码
username_input_location = pyautogui.locateCenterOnScreen('username_input.png')
password_input_location = pyautogui.locateCenterOnScreen('password_input.png')
login_button_location = pyautogui.locateCenterOnScreen('login_button.png')

if username_input_location and password_input_location and login_button_location:
    pyautogui.click(username_input_location.x, username_input_location.y)
    pyautogui.typewrite('test_user')
    pyautogui.click(password_input_location.x, password_input_location.y)
    pyautogui.typewrite('test_password')
    pyautogui.click(login_button_location.x, login_button_location.y)

    time.sleep(2)  # 等待登录结果显示

    # 检查登录是否成功（这里假设登录成功后会出现一个特定的提示框）
    success_dialog_location = pyautogui.locateOnScreen('success_dialog.png')
    if success_dialog_location:
        print('登录测试成功')
    else:
        print('登录测试失败')
else:
    print('无法找到界面元素，测试终止')
```

在这个示例中，我们首先通过locateCenterOnScreen函数查找应用程序图标、用户名输入框、密码输入框和登录按钮的位置，然后使用click和typewrite函数模拟用户的点击和输入操作 。最后，通过查找登录成功后的提示框来判断登录是否成功。这样，我们就可以自动化地对应用程序的登录功能进行多次测试，大大提高了测试效率和准确性。

### （二）数据采集与处理

比如，我们需要从一个电商网站上采集商品信息，包括商品名称、价格、销量等 。我们可以使用 PyAutoGUI 结合一些图像识别和文本处理技术来实现自动化采集。以下是一个简单的思路和示例代码：

```python
import pyautogui
import time
import pytesseract
from PIL import Image

# 打开浏览器并访问电商网站
pyautogui.hotkey('win', 'r')  # 打开运行对话框
pyautogui.typewrite('chrome')
pyautogui.press('enter')
time.sleep(2)
pyautogui.typewrite('https://example_ecommerce.com')
pyautogui.press('enter')
time.sleep(5)  # 等待页面加载

# 模拟搜索商品
search_box_location = pyautogui.locateCenterOnScreen('search_box.png')
if search_box_location:
    pyautogui.click(search_box_location.x, search_box_location.y)
    pyautogui.typewrite('手机')
    pyautogui.press('enter')
    time.sleep(5)  # 等待搜索结果页面加载

# 采集商品信息
for _ in range(3):  # 假设采集3页商品信息
    # 截取当前页面商品信息区域的屏幕截图
    screenshot = pyautogui.screenshot(region=(100, 200, 800, 600))
    screenshot.save('product_info.png')

    # 使用OCR识别截图中的文本信息
    text = pytesseract.image_to_string(Image.open('product_info.png'))

    # 处理识别出的文本，提取商品名称、价格、销量等信息
    # 这里只是简单示例，实际需要更复杂的文本处理逻辑
    lines = text.split('\n')
    for line in lines:
        if '价格' in line:
            price = line.split('：')[-1]
            print(f'商品价格：{price}')
        elif '销量' in line:
            sales = line.split('：')[-1]
            print(f'商品销量：{sales}')

    # 模拟点击下一页按钮
    next_page_button_location = pyautogui.locateCenterOnScreen('next_page_button.png')
    if next_page_button_location:
        pyautogui.click(next_page_button_location.x, next_page_button_location.y)
        time.sleep(5)  # 等待下一页加载
    else:
        break
```

在这个示例中，我们首先打开浏览器并访问电商网站，然后模拟搜索商品 。接着，通过截取屏幕上商品信息区域的截图，并使用 OCR 技术（这里使用pytesseract库）识别截图中的文本，从而提取出商品的相关信息。最后，通过模拟点击下一页按钮，实现多页商品信息的采集。

### （三）软件演示与教程录制

假设我们要制作一个关于某个绘图软件使用教程的视频，我们可以使用 PyAutoGUI 自动化演示软件的各种功能，并配合录屏软件进行录制 。以下是一个简单的示例代码，展示如何使用 PyAutoGUI 打开绘图软件并进行一些基本操作：

```python
import pyautogui
import time

# 模拟打开绘图软件（假设软件图标在屏幕上的位置已知）
paint_icon_location = pyautogui.locateCenterOnScreen('paint_icon.png')
if paint_icon_location:
    pyautogui.doubleClick(paint_icon_location.x, paint_icon_location.y)
    time.sleep(5)  # 等待软件打开

# 演示绘制一个矩形
rectangle_tool_location = pyautogui.locateCenterOnScreen('rectangle_tool.png')
if rectangle_tool_location:
    pyautogui.click(rectangle_tool_location.x, rectangle_tool_location.y)
    time.sleep(1)

    start_x, start_y = 100, 100
    end_x, end_y = 300, 300
    pyautogui.moveTo(start_x, start_y)
    pyautogui.mouseDown()
    pyautogui.dragTo(end_x, end_y)
    pyautogui.mouseUp()

# 演示保存绘制的图形
pyautogui.hotkey('ctrl','s')
time.sleep(2)
file_name_input_location = pyautogui.locateCenterOnScreen('file_name_input.png')
if file_name_input_location:
    pyautogui.click(file_name_input_location.x, file_name_input_location.y)
    pyautogui.typewrite('drawing.png')
    pyautogui.press('enter')
```

在这个示例中，我们首先通过图像识别找到绘图软件的图标并打开软件 。然后，找到矩形绘制工具并使用鼠标操作绘制一个矩形。最后，演示保存绘制图形的操作。在运行这段代码时，同时开启录屏软件，就可以录制出一个完整的软件使用教程视频，大大提高了制作教程的效率和准确性。

### （四）游戏辅助工具

以简单的扫雷游戏为例，我们可以使用 PyAutoGUI 制作一个辅助工具，帮助玩家自动识别雷区和点击安全区域 。以下是一个简单的实现思路和示例代码：

```python
import pyautogui
import time
import cv2

# 定义雷区格子的大小和初始位置
cell_size = 18
left, top = 0, 0  # 假设雷区左上角坐标，实际需要根据屏幕截图识别

# 加载数字图片模板，用于识别雷区数字
number_templates = []
for i in range(1, 9):
    template = cv2.imread(f'{i}.png', cv2.IMREAD_GRAYSCALE)
    number_templates.append(template)


def recognize_number(image):
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    best_match = None
    best_score = 0
    for i, template in enumerate(number_templates):
        result = cv2.matchTemplate(gray, template, cv2.TM_CCOEFF_NORMED)
        min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(result)
        if max_val > best_score:
            best_score = max_val
            best_match = i + 1
    return best_match


def analyze_minefield():
    screenshot = pyautogui.screenshot(region=(left, top, cell_size * 30, cell_size * 16))  # 假设雷区大小为30x16
    screenshot = cv2.cvtColor(numpy.array(screenshot), cv2.COLOR_RGB2BGR)
    for y in range(0, 16):
        for x in range(0, 30):
            cell = screenshot[y * cell_size:(y + 1) * cell_size, x * cell_size:(x + 1) * cell_size]
            number = recognize_number(cell)
            if number:
                print(f'坐标({x}, {y})处的数字为：{number}')


# 开始分析雷区
time.sleep(3)  # 等待游戏界面加载完成
analyze_minefield()
```

在这个示例中，我们首先定义了雷区格子的大小和初始位置，然后加载数字图片模板用于识别雷区中的数字 。recognize_number函数通过模板匹配的方式识别每个格子中的数字，analyze_minefield函数则对整个雷区进行截图并分析每个格子的数字。通过这种方式，我们可以根据识别出的数字来判断哪些区域是安全的，哪些区域可能有雷，从而实现简单的扫雷游戏辅助功能。 请注意，在实际游戏中使用辅助工具可能涉及违反游戏规则的问题，仅用于技术学习和研究目的。

## 五、使用注意事项与技巧

（一）防故障机制

PyAutoGUI 提供了自动防故障功能，默认情况下是开启的 。当鼠标移动到屏幕的左上角（坐标为 (0, 0)）时，会触发FailSafeException异常，程序会停止执行，这可以防止程序出现异常情况时无法停止，导致不可预期的后果 。如果你确定自己的程序不会出现问题，或者在调试过程中不想被这个机制中断，可以通过以下方式禁用它：

```python
import pyautogui

# 禁用自动防故障功能
pyautogui.FAILSAFE = False 
```

不过，禁用故障保护可能会带来风险，因此请谨慎操作。 另外，为了避免操作速度过快导致程序出错或错过某些界面元素的响应，你可以设置停顿功能 。通过设置pyautogui.PAUSE变量，可以让每个 PyAutoGUI 函数调用在执行动作后暂停指定的秒数。例如，设置暂停时间为 1 秒：

```python
import pyautogui

# 设置每次操作后暂停1秒
pyautogui.PAUSE = 1 
```

这样，在执行诸如鼠标移动、点击、键盘输入等操作后，程序都会暂停 1 秒，给系统和其他应用程序足够的时间来响应。

### （二）坐标定位技巧

在使用 PyAutoGUI 进行鼠标操作时，准确获取屏幕坐标非常关键 。你可以使用pyautogui.position()函数来获取当前鼠标的坐标位置，返回一个包含横坐标和纵坐标的元组 。例如：

```python
import pyautogui

# 获取当前鼠标坐标
x, y = pyautogui.position()
print(f'当前鼠标坐标: ({x}, {y})') 
```

另外，在进行图像识别定位时，为了提高定位的准确性和稳定性，可以设置locateOnScreen()函数的confidence参数 。该参数表示匹配的置信度，取值范围为 0 到 1，值越高表示匹配要求越严格 。例如，将置信度设置为 0.8：

```python
import pyautogui

# 在屏幕上查找图像，置信度为0.8
location = pyautogui.locateOnScreen('image.png', confidence=0.8)
if location:
	left, top, width, height = location
    print(f'找到图像，位置为：({left}, {top})，宽度为：{width}，高度为：{height}')
else:
	print('未找到图像')
```

当在不同分辨率的屏幕上运行自动化脚本时，由于相同的像素坐标在不同分辨率下代表的实际位置可能不同，会导致坐标不准确 。为了解决这个问题，可以使用pyautogui.size()函数获取当前屏幕的分辨率，并根据分辨率调整坐标 。例如，假设你希望在屏幕中心进行点击操作，无论屏幕分辨率如何变化，都可以这样实现：

```python
import pyautogui

# 获取屏幕分辨率
screen_width, screen_height = pyautogui.size()
# 计算屏幕中心坐标
center_x = screen_width // 2
center_y = screen_height // 2
# 移动鼠标到屏幕中心并点击
pyautogui.click(center_x, center_y) 
```

### （三）异常处理

在使用 PyAutoGUI 时，可能会遇到各种异常情况 。除了前面提到的FailSafeException异常外，还可能遇到ImageNotFoundException异常，当使用locateOnScreen()等图像识别函数找不到指定图像时会抛出该异常 。你可以使用try - except语句来捕获并处理这些异常，使程序更加健壮 。例如：

```python
import pyautogui

try:
    # 在屏幕上查找图像
    location = pyautogui.locateOnScreen('icon.png')
    if location:
        x, y = pyautogui.center(location)
        pyautogui.click(x, y)
    else:
        print('未找到图像')
except pyautogui.ImageNotFoundException:
    print('在屏幕上未找到指定图像')
except pyautogui.FailSafeException:
    print('触发自动防故障机制，程序停止') 
```

另外，在进行键盘输入操作时，如果目标窗口没有获得焦点，可能会导致输入内容没有出现在预期的位置 。为了避免这种情况，可以在执行键盘输入操作前，使用pyautogui.click()函数先将鼠标点击到目标窗口，使其获得焦点 。例如：

```python
import pyautogui

# 假设目标窗口的位置已知
window_x, window_y = 100, 100
# 点击目标窗口，使其获得焦点
pyautogui.click(window_x, window_y)
# 进行键盘输入操作
pyautogui.typewrite('Hello, World!') 
```

通过合理运用这些使用注意事项与技巧，可以让你在使用 PyAutoGUI 进行自动化任务时更加得心应手，提高脚本的稳定性和可靠性 。