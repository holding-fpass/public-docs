## Introduction

The Fpass platform is an education system. The main goal of Single Sign-On (SSO) is to allow users to log into the Fpass platform from another application, providing a smoother and more secure user experience.

## SSO Overview

SSO is based on JSON Web Tokens (JWT). Below are listed and detailed the required properties in the JWT payload:

- `userId`: (string) Unique identifier of the user.
- `whitelabel`: (string) Name of the partner or application.
- `ssoVersion`: (string) Must be "v2.0.0".
- `iss`: (string) Issuer of the token.

## JWT

JWT (JSON Web Token) is an open standard (RFC 7519) that defines a compact and secure way to transmit information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. For more information, visit [jwt.io](https://jwt.io).

## Technical Requirements

To implement SSO, the following technical requirements are necessary:

1. Knowledge of the shared secret key between the partner and Fpass.
2. Ability to generate a JWT using this secret key.

## Implementation Steps

### Generate the JWT

To generate a JWT, use the secret key and whitelabel key provided by Fpass. Below are code examples in Python and Node.js to generate a JWT with the following payload:

```json
{
    "userId": "example-user-id",
    "whitelabel": "your-whitelabel-key",
    "ssoVersion": "v2.0.0",
    "iss": "your-whitelabel-key"
}
```

#### Python Example

```python
import jwt
import datetime

# Define the secret key
SECRET_KEY = 'your_secret_key'

# Create the payload
payload = {
    'userId': 'example-user-id',
    'whitelabel': 'your-whitelabel-key',
    'ssoVersion': 'v2.0.0',
    'iss': 'your-whitelabel-key',
    'exp': datetime.datetime.utcnow() + datetime.timedelta(hours=1)  # Token expires in 1 hour
}

# Generate the JWT
token = jwt.encode(payload, SECRET_KEY, algorithm='HS256')

print(token)
```

#### Node.js Example

```javascript
const jwt = require('jsonwebtoken');

// Define the secret key
const SECRET_KEY = 'your_secret_key';

// Create the payload
const payload = {
    userId: 'example-user-id',
    whitelabel: 'your-whitelabel-key',
    ssoVersion: 'v2.0.0',
    iss: 'your-whitelabel-key',
    exp: Math.floor(Date.now() / 1000) + (60 * 60) // Token expires in 1 hour
};

// Generate the JWT
const token = jwt.sign(payload, SECRET_KEY);

console.log(token);
```

### Redirect the User

After generating the JWT, redirect the user to the Fpass URL with the generated JWT. Below are code examples for this redirection in Python and Node.js.

#### Python Example

```python
from flask import Flask, redirect

app = Flask(__name__)

@app.route('/login')
def login():
    # Generate the JWT
    token = jwt.encode(payload, SECRET_KEY, algorithm='HS256')
    # Redirect the user
    return redirect(f'https://your-whitelabel-url.com?jwt={token}')

if __name__ == '__main__':
    app.run()
```

#### Node.js Example

```javascript
const express = require('express');
const app = express();

app.get('/login', (req, res) => {
    // Generate the JWT
    const token = jwt.sign(payload, SECRET_KEY);
    // Redirect the user
    res.redirect(`https://your-whitelabel-url.com?jwt=${token}`);
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

## Testing and Troubleshooting

### Testing

To ensure the SSO integration is working correctly, follow these steps:

1. Generate the JWT with the correct data.
2. Redirect the user to the Fpass URL with the generated JWT.
3. Verify if the user is authenticated correctly on the platform.

### Troubleshooting

Common issues that may occur during integration:

- **Expired Token**: Ensure that the `exp` field in the payload is set correctly.
- **Invalid Signature**: Verify that the secret key used to generate the JWT is correct.
- **Incorrect Payload**: Ensure that all required fields are present in the payload.

## Conclusion

By following the steps detailed in this documentation, you can implement SSO on the Fpass platform efficiently and securely. Be sure to follow all steps correctly to ensure a successful integration.
