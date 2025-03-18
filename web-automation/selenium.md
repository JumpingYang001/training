# Selenium Testing Guide

## Introduction
Selenium is a popular open-source tool for web automation testing.

## Setup
```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
```

## Advanced Setup
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

def setup_driver():
    chrome_options = Options()
    chrome_options.add_argument('--headless')
    chrome_options.add_argument('--no-sandbox')
    chrome_options.add_argument('--disable-dev-shm-usage')
    
    driver = webdriver.Chrome(options=chrome_options)
    driver.implicitly_wait(10)
    return driver
```

## Key Features
1. Cross-browser testing
2. Multiple language support
3. Large community

## Common Operations
```python
# Finding elements
element = driver.find_element(By.ID, "search")
element = driver.find_element(By.CSS_SELECTOR, ".class-name")

# Actions
element.click()
element.send_keys("text")
element.submit()
```

## Wait Strategies
1. Explicit Wait
```python
wait = WebDriverWait(driver, 10)
element = wait.until(EC.presence_of_element_located((By.ID, "myDynamicElement")))
```

2. Custom Wait Conditions
```python
class element_has_css_class(object):
    def __init__(self, locator, css_class):
        self.locator = locator
        self.css_class = css_class

    def __call__(self, driver):
        element = driver.find_element(*self.locator)
        if self.css_class in element.get_attribute("class"):
            return element
        return False

# Usage
wait.until(element_has_css_class((By.ID, "myElement"), "active"))
```

## Advanced Interactions
```python
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.keys import Keys

# Drag and Drop
source = driver.find_element(By.ID, "source")
target = driver.find_element(By.ID, "target")
ActionChains(driver).drag_and_drop(source, target).perform()

# File Upload
file_input = driver.find_element(By.CSS_SELECTOR, "input[type='file']")
file_input.send_keys("/path/to/file")

# Handle Multiple Windows
main_window = driver.current_window_handle
driver.switch_to.window(driver.window_handles[-1])
```

## Error Handling
```python
def safe_click(driver, by, value, max_attempts=3):
    for attempt in range(max_attempts):
        try:
            element = driver.find_element(by, value)
            element.click()
            return True
        except Exception as e:
            if attempt == max_attempts - 1:
                logging.error(f"Failed to click element: {e}")
                raise
            time.sleep(1)
```

## Performance Testing
```python
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities

caps = DesiredCapabilities.CHROME
caps['goog:loggingPrefs'] = {'performance': 'ALL'}

performance_logs = driver.get_log('performance')
```

## Test Organization
```python
class TestBase:
    def setup_method(self):
        self.driver = setup_driver()
        
    def teardown_method(self):
        self.driver.quit()

class TestLogin(TestBase):
    def test_valid_login(self):
        login_page = LoginPage(self.driver)
        dashboard = login_page.login("user", "pass")
        assert dashboard.is_loaded()
```

## Best Practices
1. Use explicit waits
2. Implement Page Object Model
3. Handle dynamic elements properly
