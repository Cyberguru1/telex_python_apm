# Telex Python APM Middleware

The **Telex Python APM Middleware** is a FastAPI middleware that monitors incoming HTTP requests. It sends request data (if the response status code is 400 or higher) to a specified Telex webhook for monitoring and analytics.

## Installation

You can easily install the Telex Python APM Middleware via `pip`:

```bash
pip install telex-python-apm
```

## Setup and Usage

1. **Import the Middleware**
   In your FastAPI app, import the `MonitorMiddleware` class from the `telex_python_apm.fastapi.middleware` package.
2. **Add the Middleware to FastAPI**
   Use `app.add_middleware()` to add the `MonitorMiddleware` and pass the `webhook_url` parameter.
3. **Set the Webhook URL**
   Set your Telex channel webhook URL where the request data will be sent.

### Example FastAPI Application

```python
from fastapi import FastAPI
from telex_python_apm.fastapi.middleware import MonitorMiddleware

app = FastAPI()

# Set your Telex channel webhook URL
WEBHOOK_URL = "https://ping.telex.im/v1/webhooks/25..."

# Add the APM middleware to the FastAPI app
app.add_middleware(MonitorMiddleware, webhook_url=WEBHOOK_URL)

@app.get("/test")
async def test_route():
    return {"message": "This is a test route"}

@app.get("/client_error", status_code=400)
async def error_route():
    return {"error": "This is an error"}, 400

@app.get("/server_error", status_code=500)
async def error_route():
    return {"error": "This is an error"}, 500
```

### Explanation

- **`MonitorMiddleware`**: Monitors incoming requests and sends request details to the specified Telex webhook if the response status code is 400 or higher.
- **`webhook_url`**: A URL pointing to your Telex channel webhook, which receives the request data for monitoring purposes.

## Run Your Application

Once the middleware is set up, you can start your FastAPI application using `uvicorn`:

```bash
uvicorn main:app --reload
```

Replace `main` with the name of your Python file if necessary. The app will start on `http://127.0.0.1:8000`.

## License

This package is licensed under the MIT License.
