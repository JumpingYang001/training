# Performance Testing Guide

## Types of Performance Tests
1. Load Testing
2. Stress Testing
3. Endurance Testing
4. Spike Testing

## Load Testing Example with k6
```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
    vus: 10,
    duration: '30s',
};

export default function () {
    const res = http.get('http://test.k6.io');
    check(res, {
        'is status 200': (r) => r.status === 200,
        'response time < 500ms': (r) => r.timings.duration < 500
    });
    sleep(1);
}
```

## Browser Performance Metrics
```javascript
const metrics = await page.metrics();
console.log('Performance metrics:', {
    JSHeapUsedSize: metrics.JSHeapUsedSize,
    JSHeapTotalSize: metrics.JSHeapTotalSize,
    FirstContentfulPaint: metrics.FirstContentfulPaint
});
```
