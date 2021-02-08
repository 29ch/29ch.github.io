# Selenium using Python

```
pip install selenium
pip install webdriver_manager

brew install chromedriver
#brew install geckodriver
```

```
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager

browser = webdriver.Chrome()
```

## ChromeOptions
```
options = webdriver.ChromeOptions()
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

driver = new ChromeDriver(options)
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

```
from selenium.webdriver.firefox import service as fs
firefox_servie = fs.Service(executable_path="/usr/local/bin/geckodriver", log_path='/my/log/path')
driver = webdriver.Firefox(service=firefox_servie)

element = driver.find_element(by=By.XPATH, value="//input[@type='text']")
element.send_keys('Qiita')
element.submit()
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
