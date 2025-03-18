# REST API Testing Guide

## Fundamentals
### HTTP Methods and Status Codes
- GET (200 - OK)
- POST (201 - Created)
- PUT (200 - OK)
- DELETE (204 - No Content)
- PATCH (200 - OK)

### Request Components
- Headers
- Query Parameters
- Path Parameters
- Request Body
- Authentication Tokens

## Python Examples using Requests

### Basic Requests
```python
import requests
import json

# GET Request
def test_get_user():
    response = requests.get('https://api.example.com/users/1')
    assert response.status_code == 200
    assert 'name' in response.json()

# POST Request
def test_create_user():
    payload = {
        'name': 'John Doe',
        'email': 'john@example.com'
    }
    response = requests.post(
        'https://api.example.com/users',
        json=payload,
        headers={'Content-Type': 'application/json'}
    )
    assert response.status_code == 201
```

### Authentication Examples
```python
# Basic Auth
response = requests.get(
    'https://api.example.com/secure',
    auth=('username', 'password')
)

# Bearer Token
headers = {'Authorization': f'Bearer {token}'}
response = requests.get(
    'https://api.example.com/secure',
    headers=headers
)

# OAuth2
from requests_oauthlib import OAuth2Session
oauth = OAuth2Session(client_id)
token = oauth.fetch_token(
    token_url='https://api.example.com/oauth/token',
    client_secret=client_secret
)
```

## JavaScript Examples using Axios

### Basic Requests
```javascript
const axios = require('axios');

// GET Request
async function getUser(id) {
    try {
        const response = await axios.get(`/api/users/${id}`);
        return response.data;
    } catch (error) {
        handleError(error);
    }
}

// POST Request
async function createUser(userData) {
    try {
        const response = await axios.post('/api/users', userData);
        return response.data;
    } catch (error) {
        handleError(error);
    }
}
```

## Test Organization with pytest
```python
import pytest
import requests

class TestUserAPI:
    @pytest.fixture
    def base_url(self):
        return 'https://api.example.com'
    
    @pytest.fixture
    def auth_token(self):
        # Get authentication token
        return 'your-auth-token'
    
    def test_get_user(self, base_url, auth_token):
        headers = {'Authorization': f'Bearer {auth_token}'}
        response = requests.get(f'{base_url}/users/1', headers=headers)
        assert response.status_code == 200
        
    def test_create_user(self, base_url, auth_token):
        headers = {
            'Authorization': f'Bearer {auth_token}',
            'Content-Type': 'application/json'
        }
        payload = {'name': 'John Doe', 'email': 'john@example.com'}
        response = requests.post(
            f'{base_url}/users',
            json=payload,
            headers=headers
        )
        assert response.status_code == 201
```

## Request/Response Validation
```python
from jsonschema import validate

# JSON Schema for User
user_schema = {
    "type": "object",
    "properties": {
        "id": {"type": "integer"},
        "name": {"type": "string"},
        "email": {"type": "string", "format": "email"}
    },
    "required": ["id", "name", "email"]
}

def test_user_schema():
    response = requests.get('https://api.example.com/users/1')
    user_data = response.json()
    
    # Validate response against schema
    validate(instance=user_data, schema=user_schema)
```

## Error Handling Patterns
```python
class APIError(Exception):
    def __init__(self, status_code, message):
        self.status_code = status_code
        self.message = message
        super().__init__(self.message)

def api_request(method, url, **kwargs):
    try:
        response = requests.request(method, url, **kwargs)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.HTTPError as http_err:
        raise APIError(
            response.status_code,
            f"HTTP error occurred: {http_err}"
        )
    except requests.exceptions.RequestException as err:
        raise APIError(500, f"Error occurred: {err}")
```

## Performance Testing
```python
import time
import statistics

def measure_response_time(func, iterations=100):
    times = []
    for _ in range(iterations):
        start_time = time.time()
        func()
        end_time = time.time()
        times.append(end_time - start_time)
    
    return {
        'min': min(times),
        'max': max(times),
        'avg': statistics.mean(times),
        'median': statistics.median(times)
    }

# Usage
def test_api_performance():
    results = measure_response_time(
        lambda: requests.get('https://api.example.com/users')
    )
    assert results['avg'] < 0.5  # Response should be under 500ms
```

## Best Practices
1. Always validate response status codes
2. Implement proper error handling
3. Use authentication tokens securely
4. Validate response schemas
5. Monitor API performance
6. Implement retry mechanisms for flaky tests
7. Use appropriate assertions for data validation
8. Maintain test data independence
9. Follow API documentation guidelines
10. Implement proper logging for debugging
