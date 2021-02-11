# Selenium using Python

## Selenium Installation
### Installing Selenium libraries
```
pip install selenium
```
### Installing WebDriver binaries
```
# pip install webdriver_manager
brew install chromedriver
#brew install geckodriver
```

### Driver requirements
Adding Executables to your PATH

#### Chromium/Chrome
```
#Simple assignment
from selenium.webdriver import Chrome

driver = Chrome()
# Chrome(executable_path='/path/to/chromedriver')

#Or use the context manager
from selenium.webdriver import Chrome

with Chrome() as driver:
    #your code inside this indent
```

#### Firefox
```
#Simple assignment
from selenium.webdriver import Firefox

driver = Firefox()
# Firefox(executable_path='/path/to/geckodriver')

#firefox_servie = Firefox.service.Service(executable_path="/usr/local/bin/geckodriver", log_path='/my/log/path')
#driver = Firefox(service=firefox_servie)

#Or use the context manager
from selenium.webdriver import Firefox

with Firefox() as driver:
   #your code inside this indent
```

#### Edge
```
#Simple assignment
from selenium.webdriver import Edge

driver = Edge()
# Edge(executable_path='/path/to/MicrosoftWebDriver.exe')

#Or use the context manager
from selenium.webdriver import Edge

with Edge() as driver:
   #your code inside this indent
```

### Browser manipulation
#### Browser navigation
```
# Navigate to
driver.get("https://selenium.dev")

# Get current URL
driver.current_url

# Back
driver.back()

# Forward
driver.forward()

# Refresh
driver.refresh()

# Get title
driver.title
```

#### Windows and tabs
```
# Get window handle
driver.current_window_handle

# Switching windows or tabs
# Create new window (or) new tab and switch
## Opens a new tab and switches to new tab
driver.switch_to.new_window('tab')

## Opens a new window and switches to new window
driver.switch_to.new_window('window')

# Closing a window or tab
##Close the tab or window
driver.close()

##Switch back to the old tab or window
driver.switch_to.window(original_window)
```

#### Quitting the browser at the end of a session
```
driver.quit()

##
try:
    #WebDriver code here...
finally:
    driver.quit()
```

#### Frames and Iframes
```
# This Wont work
driver.find_element(By.TAG_NAME, 'button').click()
```

#### Using a WebElement
```
# Store iframe web element
iframe = driver.find_element(By.CSS_SELECTOR, "#modal > iframe")

# switch to selected iframe
driver.switch_to.frame(iframe)

# Now click on button
driver.find_element(By.TAG_NAME, 'button').click()
```

#### Using a name or ID
```
# Switch frame by id
driver.switch_to.frame('buttonframe')

# Now, Click on the button
driver.find_element(By.TAG_NAME, 'button').click()
```

#### Using an index
```
# Switch to the second frame
driver.switch_to.frame(1)
```

#### Leaving a frame
```
# switch back to default content
driver.switch_to.default_content()
```

#### Window management
```
# Maximize window
driver.maximize_window()

# Minimize window
driver.minimize_window()

# Fullscreen window
driver.fullscreen_window()

# TakeScreenshot
## Returns and base64 encoded string into image
driver.save_screenshot('./image.png')

# TakeElementScreenshot
ele = driver.find_element(By.CSS_SELECTOR, 'h1')
## Returns and base64 encoded string into image
ele.screenshot('./image.png')

# Print Page
    from selenium.webdriver.common.print_page_options import PrintOptions

    print_options = PrintOptions()
    print_options.page_ranges = ['1-2']

    driver.get("printPage.html")

    base64code = driver.print_page(print_options)
```

#### Wait
```
WebDriverWait(driver, timeout=3).until(some_condition)
```

### JavaScript alerts, prompts and confirmations
#### Alerts
```
# Click the link to activate the alert
driver.find_element(By.LINK_TEXT, "See an example alert").click()

# Wait for the alert to be displayed and store it in a variable
alert = wait.until(expected_conditions.alert_is_present())

# Store the alert text in a variable
text = alert.text

# Press the OK button
alert.accept()
```
#### Confirm
```
# Click the link to activate the alert
driver.find_element(By.LINK_TEXT, "See a sample confirm").click()

# Wait for the alert to be displayed
wait.until(expected_conditions.alert_is_present())

# Store the alert in a variable for reuse
alert = driver.switch_to.alert

# Store the alert text in a variable
text = alert.text

# Press the Cancel button
alert.dismiss()
```

#### Prompt
```
# Click the link to activate the alert
driver.find_element(By.LINK_TEXT, "See a sample prompt").click()

# Wait for the alert to be displayed
wait.until(expected_conditions.alert_is_present())

# Store the alert in a variable for reuse
alert = Alert(driver)

# Type your message
alert.send_keys("Selenium")

# Press the OK button
alert.accept()
```

### Page loading strategy
#### normal
```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
options = Options()
options.page_load_strategy = 'normal'
driver = webdriver.Chrome(options=options)
# Navigate to url
driver.get("http://www.google.com")
driver.quit()
```
#### eager
```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
options = Options()
options.page_load_strategy = 'eager'
driver = webdriver.Chrome(options=options)
# Navigate to url
driver.get("http://www.google.com")
driver.quit()
```
#### none
```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
options = Options()
options.page_load_strategy = 'none'
driver = webdriver.Chrome(options=options)
# Navigate to url
driver.get("http://www.google.com")
driver.quit()
```

### この接続ではプライバシーが保護されません
```
import pytest
import time
import json
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support import expected_conditions
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
from selenium.webdriver.chrome.options import Options

class TestWebdriver():
  def setup_method(self, method):
    Driver_path=r'C:\selenium\chromedriver.exe' #ダウンロードしたchromeドライバーを置いた場所を指定
    options = Options()
    options.add_argument('--headless') #ヘッドレスモードを有効
    capabilities = DesiredCapabilities.CHROME.copy()
    capabilities['acceptInsecureCerts'] = True
    self.driver = webdriver.Chrome(desired_capabilities=capabilities, executable_path=Driver_path, options=options)
    # self.driver = webdriver.Firefox()
    self.vars = {}

  def teardown_method(self, method):
    self.driver.quit()
    # self.driver.close()

  def test_webdriver(self):
    url='http://www.python.org' #ここに開きたいURL
    # self.driver.get("https://127.0.0.1/")
    self.driver.get(url)
    self.assertIn('Python',self.driver.title)
    self.driver.find_element_by_link_text('Downloads').click()

    element = WebDriverWait(self.driver, 10).until(
        EC.presence_of_element_located(
            (By.CLASS_NAME, 'widget-title')))
    self.assertEqual('Looking for a specific release?', element.text)

    self.driver.find_element_by_link_text('Documentation').click()

    element = WebDriverWait(self.driver, 10).until(
        EC.presence_of_element_located(
            (By.CLASS_NAME, 'call-to-action')))
    self.assertIn('Browse the docs', element.text)

    element = self.driver.find_element_by_name('q')
    element.clear()
    element.send_keys('pycon')
    element.send_keys(Keys.RETURN)
    assert 'No results found' not in self.driver.page_source
```

```
html = driver.page_source.encode('utf-8')
soup = BeautifulSoup(html, 'lxml')
#この後は普通にBeautiful Soupの文法通り使える。
```

```
# ブラウザを開く。
driver = webdriver.Chrome()
# Googleの検索TOP画面を開く。
driver.get("https://www.google.co.jp/")

# 検索語として「selenium」と入力し、Enterキーを押す。
search = driver.find_element_by_name('q') 
search.send_keys("selenium automation")
search.send_keys(Keys.ENTER)
# タイトルに「Selenium - Web Browser Automation」と一致するリンクをクリックする。
#element = driver.find_element_by_partial_link_text("SeleniumHQ Browser Automation")
#element = driver.find_element_by_link_text("WebDriver")
element = driver.find_element_by_partial_link_text("Selenium")
element.click()

# 5秒間待機してみる。
sleep(5)
# ブラウザを終了する。
driver.close()
```

### Web element
#### Find Element
```
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Firefox()
driver.get("http://www.google.com")

# Get search box element from webElement 'q' using Find Element
search_box = driver.find_element(By.NAME, "q")

search_box.send_keys("webdriver")
```

#### Find Elements
```
# Get all the elements available with tag name 'p'
elements = driver.find_elements(By.TAG_NAME, 'p')

for e in elements:
    print(e.text)
```

#### Find Element From Element
```
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Firefox()
driver.get("http://www.google.com")

search_form = driver.find_element(By.TAG_NAME, "form")
search_box = search_form.find_element(By.NAME, "q")
search_box.send_keys("webdriver")
```

#### Find Elements From Element
```
# Get element with tag name 'div'
element = driver.find_element(By.TAG_NAME, 'div')

# Get all the elements available with tag name 'p'
elements = element.find_elements(By.TAG_NAME, 'p')
for e in elements:
    print(e.text)
  
```

#### Get Active Element
```
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get("https://www.google.com")
driver.find_element(By.CSS_SELECTOR, '[name="q"]').send_keys("webElement")

# Get attribute of current active element
attr = driver.switch_to.active_element.get_attribute("title")
print(attr)
```

#### Is Element Enabled
```
# Navigate to url
driver.get("http://www.google.com")
   
# Returns true if element is enabled else returns false
value = driver.find_element(By.NAME, 'btnK').is_enabled()
```

#### Is Element Selected
```
# Navigate to url
driver.get("https://the-internet.herokuapp.com/checkboxes")

# Returns true if element is checked else returns false
value = driver.find_element(By.CSS_SELECTOR, "input[type='checkbox']:first-of-type").is_selected()
```

#### Get Element Text
```
# Navigate to url
driver.get("https://www.example.com")

# Retrieves the text of the element
text = driver.find_element(By.CSS_SELECTOR, "h1").text
```

### Keyboard
#### sendKeys
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
driver = webdriver.Firefox()

# Navigate to url
driver.get("http://www.google.com")

# Enter "webdriver" text and perform "ENTER" keyboard action
driver.find_element(By.NAME, "q").send_keys("webdriver" + Keys.ENTER)
```

#### keyDown
```
# Perform action ctrl + A (modifier CONTROL + Alphabet A) to select the page
webdriver.ActionChains(driver).key_down(Keys.CONTROL).send_keys("a").perform()
```

#### keyUp
```
action = webdriver.ActionChains(driver)

# Enters text "qwerty" with keyDown SHIFT key and after keyUp SHIFT key (QWERTYqwerty)
action.key_down(Keys.SHIFT).send_keys_to_element(search, "qwerty").key_up(Keys.SHIFT).send_keys("qwerty").perform()
```

#### clear
```
# Store 'SearchInput' element
SearchInput = driver.find_element(By.NAME, "q")
SearchInput.send_keys("selenium")

# Clears the entered text
SearchInput.clear()
```

### 要素探索
#### 親要素
```
driver.find_element_by_xpath('//div[@id="cnfm_btn"]').find_element_by_xpath('..')
```
#### 子要素
```
driver.find_element_by_xpath('//div[@id="cnfm_btn"]/div')
```
#### 隣の要素（前）
```
driver.find_element_by_xpath("//p[@id='one']/preceding-sibling::p")
```
#### 隣の要素（後）
```
driver.find_element_by_xpath("//p[@id='one']/following-sibling::p")
```
#### クラス名で探す(単数)
```
driver.find_element_by_class_name('content')
```
#### クラス名で探す(複数)
```
driver.find_elements_by_class_name('content')
```

## Driver specific capabilities
### ChromeOptions
https://sites.google.com/a/chromium.org/chromedriver/capabilities
```
options = webdriver.Chrome.Options()
options.add_argument("start-maximized")
options.add_argument("enable-automation")
options.add_argument("--headless")
options.add_argument("--no-sandbox")
options.add_argument("--disable-infobars")
options.add_argument('--disable-extensions')
options.add_argument("--disable-dev-shm-usage")
options.add_argument("--disable-browser-side-navigation")
options.add_argument("--disable-gpu")
options.add_argument('--ignore-certificate-errors')
options.add_argument('--ignore-ssl-errors')
prefs = {"profile.default_content_setting_values.notifications" : 2}
options.add_experimental_option("prefs",prefs)

driver = new ChromeDriver(chrome_options=options)
```
### FirefoxOptions
```
from selenium.webdriver.firefox.options import Options
options = Options()
options.headless = True
driver = webdriver.Firefox(options=options)
```
```
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.firefox.firefox_profile import FirefoxProfile
options=Options()
firefox_profile = FirefoxProfile()
firefox_profile.set_preference("javascript.enabled", False)
options.profile = firefox_profile
```

## Working with select elements
```
from selenium.webdriver.support.select import Select
```

```
select_element = driver.find_element(By.ID,'selectElementID')
select_object = Select(select_element)
```

```
# Select an <option> based upon the <select> element's internal index
select_object.select_by_index(1)

# Select an <option> based upon its value attribute
select_object.select_by_value('value1')

# Select an <option> based upon its text
select_object.select_by_visible_text('Bread')
```

```
# Return a list[WebElement] of options that have been selected
all_selected_options = select_object.all_selected_options

# Return a WebElement referencing the first selection option found by walking down the DOM
first_selected_option = select_object.first_selected_option
```

```
# Return a list[WebElement] of options that the &lt;select&gt; element contains
all_available_options = select_object.options
```

```
# Deselect an <option> based upon the <select> element's internal index
select_object.deselect_by_index(1)

# Deselect an <option> based upon its value attribute
select_object.deselect_by_value('value1')

# Deselect an <option> based upon its text
select_object.deselect_by_visible_text('Bread')

# Deselect all selected <option> elements
select_object.deselect_all()
```

```
does_this_allow_multiple_selections = select_object.is_multiple
```

## Mouse actions in detail
### clickAndHold
```
from selenium import webdriver
from selenium.webdriver.common.by import By
driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.google.com")

# Store 'google search' button web element
searchBtn = driver.find_element(By.LINK_TEXT, "Sign in")

# Perform click-and-hold action on the element
webdriver.ActionChains(driver).click_and_hold(searchBtn).perform()
```

### contextClick
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.google.com")

# Store 'google search' button web element
searchBtn = driver.find_element(By.LINK_TEXT, "Sign in")

# Perform context-click action on the element
webdriver.ActionChains(driver).context_click(searchBtn).perform()
```

### doubleClick
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.google.com")

# Store 'google search' button web element
searchBtn = driver.find_element(By.LINK_TEXT, "Sign in")

# Perform double-click action on the element
webdriver.ActionChains(driver).double_click(searchBtn).perform()
```

### moveToElement
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.google.com")

# Store 'google search' button web element
gmailLink = driver.find_element(By.LINK_TEXT, "Gmail")

# Performs mouse move action onto the element
webdriver.ActionChains(driver).move_to_element(gmailLink).perform()
```

### moveByOffset
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.google.com")

# Store 'google search' button web element
gmailLink = driver.find_element(By.LINK_TEXT, "Gmail")
#Set x and y offset positions of element
xOffset = 100
yOffset = 100
# Performs mouse move action onto the element
webdriver.ActionChains(driver).move_by_offset(xOffset,yOffset).perform()
```

### dragAndDrop
```
from selenium import webdriver
from selenium.webdriver.common.by import By
driver = webdriver.Chrome()

# Navigate to url
driver.get("https://crossbrowsertesting.github.io/drag-and-drop")

# Store 'box A' as source element
sourceEle = driver.find_element(By.ID, "draggable")
# Store 'box B' as source element
targetEle  = driver.find_element(By.ID, "droppable")
# Performs drag and drop action of sourceEle onto the targetEle
webdriver.ActionChains(driver).drag_and_drop(sourceEle,targetEle).perform()
```

### dragAndDropBy
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("https://crossbrowsertesting.github.io/drag-and-drop")

# Store 'box A' as source element
sourceEle = driver.find_element(By.ID, "draggable")
# Store 'box B' as source element
targetEle  = driver.find_element(By.ID, "droppable")
targetEleXOffset = targetEle.location.get("x")
targetEleYOffset = targetEle.location.get("y")

# Performs dragAndDropBy onto the target element offset position
webdriver.ActionChains(driver).drag_and_drop_by_offset(sourceEle, targetEleXOffset, targetEleYOffset).perform()
```

### release
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("https://crossbrowsertesting.github.io/drag-and-drop")

# Store 'box A' as source element
sourceEle = driver.find_element(By.ID, "draggable")
# Store 'box B' as source element
targetEle  = driver.find_element(By.ID, "droppable")

# Performs dragAndDropBy onto the target element offset position
webdriver.ActionChains(driver).click_and_hold(sourceEle).move_to_element(targetEle).perform()
#Performs release event
webdriver.ActionChains(driver).release().perform()
```

## Working with cookies
### Add Cookie
```
from selenium import webdriver

driver = webdriver.Chrome()

driver.get("http://www.example.com")

# Adds the cookie into current browser context
driver.add_cookie({"name": "key", "value": "value"})
```

### Get Named Cookie
```
from selenium import webdriver

driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.example.com")

# Adds the cookie into current browser context
driver.add_cookie({"name": "foo", "value": "bar"})

# Get cookie details with named cookie 'foo'
print(driver.get_cookie("foo"))
```

### Get All Cookies
```
from selenium import webdriver

driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.example.com")

driver.add_cookie({"name": "test1", "value": "cookie1"})
driver.add_cookie({"name": "test2", "value": "cookie2"})

# Get all available cookies
print(driver.get_cookies())
```

### Delete Cookie
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.example.com")
driver.add_cookie({"name": "test1", "value": "cookie1"})
driver.add_cookie({"name": "test2", "value": "cookie2"})

# Delete a cookie with name 'test1'
driver.delete_cookie("test1")
```

### Delete All Cookies
```
from selenium import webdriver
driver = webdriver.Chrome()

# Navigate to url
driver.get("http://www.example.com")
driver.add_cookie({"name": "test1", "value": "cookie1"})
driver.add_cookie({"name": "test2", "value": "cookie2"})

#  Deletes all cookies
driver.delete_all_cookies()
```

## 
```
Selector = "h1.nl-Hero_title"  # <= 対象のCSSセレクタ

from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

driver.get(URL)
WebDriverWait(driver, 30).until(
    EC.presence_of_element_located((By.CSS_SELECTOR, Selector))
)
```
