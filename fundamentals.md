# Test Automation Fundamentals

## Key Concepts

### 1. Test Automation Pyramid
- Unit Testing (Base Layer)
  - Function/method level testing
  - Mocking external dependencies
  - Fast execution & high coverage
  
- Integration Testing (Middle Layer)
  - Component interaction testing
  - Database integration
  - API communication
  - Service integration
  
- UI/E2E Testing (Top Layer)
  - Full user journey testing
  - Cross-browser validation
  - Performance monitoring

### 2. Test Design Patterns
#### 2.1 Page Object Model (POM)
```typescript
class LoginPage {
    constructor(private driver: WebDriver) {}
    
    // Locators
    private usernameInput = By.id('username');
    private passwordInput = By.css('.login-btn');
    
    async login(username: string, password: string) {
        await this.driver.findElement(this.usernameInput).sendKeys(username);
        await this.driver.findElement(this.passwordInput).sendKeys(password);
        await this.driver.findElement(this.loginButton).click();
    }
}
```

#### 2.2 Data-Driven Testing
```python
class EmailValidator:
    def __init__(self):
        self.email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    
    def is_valid(self, email: str) -> bool:
        """
        Validates email format using regex pattern
        Args:
            email: String to validate
        Returns:
            bool: True if email is valid, False otherwise
        """
        if not email:
            return False
        import re
        return bool(re.match(self.email_pattern, email))

@pytest.mark.parametrize("input,expected", [
    ("test@email.com", True),
    ("invalid-email", False),
    ("", False)
])
def test_email_validation(input, expected):
    validator = EmailValidator()
    assert validator.is_valid(input) == expected
```

#### 2.3 Factory Pattern

// TestDataFactory creates different types of test users with predefined configurations
interface UserData {
    username: string;
    password: string;
    role: string;
}

class TestDataFactory {
    // Creates test users with different roles and permissions
    static createTestUser(type: 'admin' | 'regular'): UserData {
        // Base user configuration
        const baseUser = {
            username: 'test@example.com',
            password: 'Test123!'
        };
        
        // Return different user types based on the requested type
        switch(type) {
            case 'admin':
                return { ...baseUser, role: 'ADMIN' };
            case 'regular':
                return { ...baseUser, role: 'USER' };
        }
    }

    // Example usage:
    // const adminUser = TestDataFactory.createTestUser('admin');
    // const regularUser = TestDataFactory.createTestUser('regular');
}

// Another example: Test Data Factory for different test environments
class TestEnvironmentFactory {
    static createConfig(env: 'dev' | 'staging' | 'prod') {
        const baseConfig = {
            timeout: 5000,
            retries: 3
        };

        switch(env) {
            case 'dev':
                return {
                    ...baseConfig,
                    baseUrl: 'http://dev.example.com',
                    logLevel: 'debug'
                };
            case 'staging':
                return {
                    ...baseConfig,
                    baseUrl: 'http://staging.example.com',
                    logLevel: 'info'
                };
            case 'prod':
                return {
                    ...baseConfig,
                    baseUrl: 'http://example.com',
                    logLevel: 'error'
                };
        }
    }

    // Example usage:
    // const devConfig = TestEnvironmentFactory.createConfig('dev');
    // const prodConfig = TestEnvironmentFactory.createConfig('prod');
}

// Example of using Factory Pattern in tests:
describe('User Authentication', () => {
    it('should allow admin access', async () => {
        const adminUser = TestDataFactory.createTestUser('admin');
        const result = await loginAs(adminUser);
        expect(result.hasAdminAccess).toBe(true);
    });

    it('should restrict regular user access', async () => {
        const regularUser = TestDataFactory.createTestUser('regular');
        const result = await loginAs(regularUser);
        expect(result.hasAdminAccess).toBe(false);
    });
});

### 3. Testing Types
- Functional Testing
- Regression Testing
- Performance Testing
- Security Testing
- Compatibility Testing
- Accessibility Testing
- Localization Testing

## Prerequisites

### 1. Programming Fundamentals
#### 1.1 JavaScript/TypeScript
[View Examples](../examples/javascript/)
```typescript
// Basic Concepts
interface TestCase {
    id: string;
    description: string;
    steps: string[];
    expectedResult: string;
}

class TestRunner {
    async executeTest(testCase: TestCase): Promise<boolean> {
        console.log(`Running test: ${testCase.description}`);
        // Test implementation
        return true;
    }
}
```

#### 1.2 Python
[View Examples](../examples/python/)
```python
# Basic Concepts
class TestCase:
    def __init__(self, description):
        self.description = description
        self.steps = []
    
    def add_step(self, step):
        self.steps.append(step)
    
    def execute(self):
        print(f"Executing test: {self.description}")
        for step in self.steps:
            step.run()
```

### 2. Web Technologies
#### 2.1 HTML
```html
<!-- Common Elements -->
<div class="login-form">
    <input type="text" id="username" />
    <input type="password" id="password" />
    <button type="submit">Login</button>
</div>
```

#### 2.2 CSS Selectors
```javascript
// Common Selector Patterns
const selectors = {
    id: '#loginForm',
    class: '.submit-button',
    attribute: '[data-testid="login-btn"]',
    child: '.form > input',
    combined: 'button.primary[type="submit"]'
};
```

### 3. Version Control (Git)
```bash
# Essential Git Commands
git clone <repository>
git checkout -b feature/new-tests
git add .
git commit -m "Add new test cases"
git push origin feature/new-tests
```

### 4. API Knowledge
[View Examples](../examples/api-testing/)
```javascript
// REST API Testing
async function apiTest() {
    // GET Request
    const response = await fetch('https://api.example.com/users');
    const data = await response.json();
    
    // POST Request
    const createResponse = await fetch('https://api.example.com/users', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            name: 'Test User',
            email: 'test@example.com'
        })
    });
}
```

### 5. DevOps Basics
[View Examples](../examples/devops/)
```yaml
# Basic CI/CD Pipeline
name: Test Automation
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Tests
        run: |
          npm install
          npm test
```

### 6. Testing Tools
[View Examples](../examples/testing-tools/)
// Test Runners (Jest, Pytest, Mocha)
// Assertion Libraries (Chai, Assert)
// Mocking Frameworks (Sinon, Jest Mock)
// Browser Automation (Selenium, Puppeteer)
// API Testing (Postman, REST Assured)

### 7. Design Patterns
[View Examples](../examples/design-patterns/)
```typescript
// Singleton Pattern Example
class TestConfig {
    private static instance: TestConfig;
    private constructor() {}
    
    static getInstance(): TestConfig {
        if (!TestConfig.instance) {
            TestConfig.instance = new TestConfig();
        }
        return TestConfig.instance;
    }
}

// Builder Pattern Example
class TestCaseBuilder {
    private testCase: TestCase;
    
    constructor() {
        this.testCase = new TestCase();
    }
    
    withDescription(description: string): TestCaseBuilder {
        this.testCase.description = description;
        return this;
    }
    
    withStep(step: string): TestCaseBuilder {
        this.testCase.steps.push(step);
        return this;
    }
    
    build(): TestCase {
        return this.testCase;
    }
}
```

### 8. Debugging Tips
1. Logging
```python
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)
logger.debug("Executing login test")
```

2. Screenshots
```javascript
await page.screenshot({
    path: `error-${Date.now()}.png`,
    fullPage: true
});
```

3. Video Recording
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

options = Options()
options.set_capability('browserVersion', '93')
options.set_capability('platformName', 'Windows')
options.set_capability('se:recordVideo', True)
