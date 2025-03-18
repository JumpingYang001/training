# Web Testing Patterns

## Page Object Pattern
```javascript
class BasePage {
    constructor(page) {
        this.page = page;
    }
    
    async waitForElement(selector) {
        return await this.page.waitForSelector(selector, {
            visible: true,
            timeout: 5000
        });
    }
}

class LoginPage extends BasePage {
    constructor(page) {
        super(page);
        this.usernameInput = '#username';
        this.passwordInput = '#password';
        this.loginButton = '#login';
    }
    
    async login(username, password) {
        await this.page.type(this.usernameInput, username);
        await this.page.type(this.passwordInput, password);
        await this.page.click(this.loginButton);
    }
}
```

## Component Testing
```javascript
class NavigationComponent {
    constructor(page) {
        this.page = page;
    }
    
    async navigateToSection(section) {
        await this.page.click(`nav >> text="${section}"`);
        await this.page.waitForLoadState('networkidle');
    }
    
    async verifyActiveSection(section) {
        const activeElement = await this.page.$('nav .active');
        return await activeElement.innerText() === section;
    }
}
```

## Visual Testing
```javascript
const { eyes } = require('@applitools/eyes-playwright');

class VisualTest {
    async checkElement(page, elementSelector, testName) {
        await eyes.open(page, 'Web App', testName);
        await eyes.check(elementSelector);
        await eyes.close();
    }
    
    async compareScreenshots(page, testName) {
        const screenshot1 = await page.screenshot();
        // Perform action
        const screenshot2 = await page.screenshot();
        await eyes.check(testName, screenshot1, screenshot2);
    }
}
```
