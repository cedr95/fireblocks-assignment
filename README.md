# Technical Writer Tasks API

## Introduction

The Technical Writer Tasks API provides endpoints to manage tasks for technical writers. The API includes:

- **GET `/tasks`**: Retrieves a list of all tasks that match the specified search criteria.
- **POST `/task`**: Adds a new task to the system.

All endpoint URIs are relative to the base URL: `https://api.techwriter.xyz`.

## Authentication

This API uses API key-based authentication. Include your API key in the `X-API-KEY` header of all requests.

To obtain an API key, please email `support@techwriter.xyz`.

## Endpoints

### GET /tasks

- **Description**: Retrieves a list of tasks based on the provided parameters and its connected tasks.
- **Method**: GET
- **URI**: `/tasks`
- **Required Headers**:
  - `X-API-KEY`: Your API key for authentication.
- **Parameters**:
  - `status` (optional): Filter tasks by status (OPEN, IN_PROGRESS, COMPLETED).
  - `component` (optional): Filter tasks by component (API_DOCS, HELP_CENTER, SDK_DOCS, OAS_FILE).
  - `updatedAfter` (optional): Filter tasks updated after a specific date (YYYY-MM-DD).
- **Response Examples**:

  - **200 OK**:

    ```json
    [
      {
        "task_id": "12345",
        "title": "Sample Task",
        "description": "Description of the sample task.",
        "status": "OPEN",
        "component": "API_DOCS",
        "connected_tasks": [
          {
            "task_id": "67890",
            "title": "Connected Task",
            "description": "Description of the connected task.",
            "status": "IN_PROGRESS"
          }
        ]
      }
    ]
    ```

    - **Explanation**: Successfully retrieved a list of tasks. The response includes task details and any connected tasks. If there are no other tasks connected then `connected_tasks` will be empty.

  - **400 Bad Request**:

    ```json
    {
      "error": "Bad Request",
      "message": "Invalid request parameters."
    }
    ```

    - **Explanation**: The request parameters were invalid. Check the parameters and try again.

  - **401 Unauthorized**:

    ```json
    {
      "error": "Unauthorized",
      "message": "API key missing or invalid."
    }
    ```

    - **Explanation**: Authentication failed. Ensure the API key is included in the request headers and is valid.

  - **500 Internal Server Error**:
    ```json
    {
      "error": "Internal Server Error",
      "message": "An unexpected error occurred."
    }
    ```
    - **Explanation**: An internal server error occurred. This is usually not related to your request. Retry later or contact support if the issue persists.

### POST /task

- **Description**: Adds a new technical writer task.
- **Method**: POST
- **URI**: `/task`
- **Required Headers**:

  - `X-API-KEY`: Your API key for authentication.

- **Request Body Parameters**:

  - `title` (string) required: The title of the task.
  - `description` (string): A description of the task.
  - `status` (string) required: The status of the task (OPEN, IN_PROGRESS, COMPLETED).
  - `component` (string) required: The component the task relates to (API_DOCS, HELP_CENTER, SDK_DOCS, OAS_FILE).
  - `connected_tasks` (array) : A list of connected tasks, each with `task_id`, `title`, `description`, and `status`.

  ```json
  {
    "title": "New Task with Connection",
    "description": "Description of the new task with a connected task.",
    "status": "OPEN",
    "component": "API_DOCS",
    "connected_tasks": [
      {
        "task_id": "67890",
        "title": "Connected Task",
        "description": "Description of the connected task.",
        "status": "IN_PROGRESS"
      }
    ]
  }
  ```

- **Response Examples**:

  - **201 Created**:

  ```json
  {
    "task_id": "54321",
    "title": "New Task",
    "description": "Description of the new task.",
    "status": "OPEN",
    "component": "API_DOCS",
    "connected_tasks": []
  }
  ```

  - **Explanation**: The task was successfully created.

  - **400 Bad Request**:

  ```json
  {
    "error": "Bad Request",
    "message": "Invalid request parameters."
  }
  ```

  - **Explanation**: The request parameters were invalid. Check the parameters and try again.

  - **401 Unauthorized**:

  ```json
  {
    "error": "Unauthorized",
    "message": "API key missing or invalid."
  }
  ```

  - **Explanation**: Authentication failed. Ensure the API key is included in the request headers and is valid.

  - **500 Internal Server Error**:

  ```json
  {
    "error": "Internal Server Error",
    "message": "An unexpected error occurred."
  }
  ```

  - **Explanation**: An internal server error occurred. This is usually not related to your request. Retry later or contact support if the issue persists.

## Example Python Code Post Request

### Create a New Task

Here is a Python example to make a POST request to the API to create a new task:

```python
import requests

api_key = 'YOUR_API_KEY'
headers = {'X-API-KEY': api_key}
response = requests.post(
    'https://api.techwriter.xyz/task',
    json ={
        'title': 'New Task with Connection',
        'description': 'Description of the new task with a connected task.',
        'status': 'OPEN',
        'component': 'API_DOCS',
        'connected_tasks': [
            {
                'task_id': '67890',
                'title': 'Connected Task',
                'description': 'Description of the connected task.',
                'status': 'IN_PROGRESS'
            }
        ]
    },
    headers=headers
)

if response.status_code == 201:
    print("Task created successfully!")
    print(response.json())
else:
    print(f"Failed to create task: {response.status_code}")
    print(response.json())

```

## Best Practices

### Handling Rate Limits

- **Respect Rate Limits**: Be aware of any rate limits imposed by the API. Rate limits are set to ensure fair usage and to prevent abuse. If you need to increase your rate limit please email `support@techwriter.xyz`.

- **Implement Retry Logic**: If you receive a rate limit error (e.g., HTTP status code `429`), implement retry logic in your application.

### Handling Error Codes

- **Check for Errors**
  - **400 Bad Request**: Indicates invalid request parameters or payload. Check and correct your request format or parameters.
  - **401 Unauthorized**: Indicates missing or invalid API key. Ensure your API key is included in the request headers and is valid.
  - **500 Internal Server Error**: Indicates server-side issues. This is usually not related to your request. Implement retry logic or notify support if the issue persists.

### Keeping the API Key Secure

- **Never Hardcode API Keys**: Avoid hardcoding your API key directly in your source code. Use environment variables or configuration files that are not included in version control.

- **Restrict API Key Usage**: If possible, restrict the usage of your API key to specific IP addresses or endpoints to reduce the risk of misuse.

- **Use HTTPS**: Always use HTTPS for API requests to encrypt the data transmitted, including your API key. This prevents the key from being exposed in transit.

### Support

- For any questions or issues, please contact our support team at `support@techwriter.xyz`.
