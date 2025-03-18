# Web Automation Best Practices

## Element Selection Strategies
```javascript
class ElementHelper {
    static async getByTestId(page, testId) {
        return await page.$(`[data-testid="${testId}"]`);
    }
    
    static async getByAriaLabel(page, label) {
        return await page.$(`[aria-label="${label}"]`);
    }
    
    static async getByRole(page, role, name) {
        return await page.$(`role=${role}[name="${name}"]`);
    }
}
```

## Error Handling
```javascript
class ErrorHandler {
    static async retryOnError(fn, retries = 3) {
        for (let i = 0; i < retries; i++) {
            try {
                return await fn();
            } catch (error) {
                if (i === retries - 1) throw error;
                await new Promise(r => setTimeout(r, 1000 * (i + 1)));
            }
        }
    }
    
    static async handleAlert(page) {
        page.on('dialog', async dialog => {
            console.log(`Dialog message: ${dialog.message()}`);
            await dialog.accept();
        });
    }
}
```

## Performance Monitoring
```javascript
class PerformanceMonitor {
    static async measurePageLoad(page) {
        const metrics = await page.evaluate(() => ({
            loadTime: performance.timing.loadEventEnd - performance.timing.navigationStart,
            domContentLoaded: performance.timing.domContentLoadedEventEnd - performance.timing.navigationStart,
            firstPaint: performance.getEntriesByType('paint')[0].startTime
        }));
        return metrics;
    }
    
    static async measureNetworkRequests(page) {
        const requests = [];
        page.on('requestfinished', request => {
            requests.push({
                url: request.url(),
                duration: request.timing().responseEnd - request.timing().requestStart
            });
        });
        return requests;
    }
}
```
