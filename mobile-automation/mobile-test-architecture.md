# Mobile Test Architecture

## Setup and Configuration
```python
from airtest.core.api import *
from airtest.core.android.android import Android
from poco.drivers.android.uiautomation import AndroidUiautomationPoco

class MobileTestSetup:
    def __init__(self):
        # Connect to device
        self.device = Android()
        self.poco = AndroidUiautomationPoco(self.device)
        
        # Set global settings
        ST.FIND_TIMEOUT = 20
        ST.SNAPSHOT_QUALITY = 70

    def launch_app(self, package):
        self.device.start_app(package)
        sleep(2)
```

## Page Object Pattern
```python
class LoginPage:
    def __init__(self, poco):
        self.poco = poco
        
    def enter_username(self, username):
        self.poco("username_field").set_text(username)
        
    def enter_password(self, password):
        self.poco("password_field").set_text(password)
        
    def click_login(self):
        self.poco("login_button").click()
        
    def verify_error_message(self):
        return self.poco("error_text").get_text()
```

## Common Mobile Actions
```python
class MobileActions:
    def __init__(self, device, poco):
        self.device = device
        self.poco = poco
    
    def swipe_screen(self, direction):
        screen_size = self.device.get_current_resolution()
        center_x = screen_size[0] / 2
        center_y = screen_size[1] / 2
        
        if direction == "up":
            swipe((center_x, center_y), (center_x, center_y - 300))
        elif direction == "down":
            swipe((center_x, center_y), (center_x, center_y + 300))
            
    def take_screenshot(self, name):
        snapshot(filename=f"{name}.png")
        
    def check_element_exists(self, element):
        return self.poco(element).exists()
```
