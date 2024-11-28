---
layout: libdoc/page
title: ChatOperations
description: The ChatOperations class provides methods to manage and interact with chat logs, chat history, and active chat sessions within the Weav.ai platform. This class handles requests, authentication, and error handling.
---
**Prerequisite** - To get started, ensure your python environment has the weav.ai developer library correctly installed [Installation Guide](setup).

---

### Initialization

To initialize the `ChatOperations` class, a `Config` object containing the authentication details is required.

```python
from weavaidev import Config
from weavaidev.chats import ChatOperations

config = Config(auth_token="eyJhbGci....",env="https://subdomain.weav.ai/")
chat_service = ChatOperations(config=config)
```

- **Parameters**:
    - `config`: A `Config` object containing the base configuration and authentication token.

---

### Method Summary

1. **get_chat_logs**: Retrieves chat logs with filtering options.
2. **get_chat_history**: Retrieves chat history for a specific chat session.
3. **chat**: Sends a chat message and retrieves a response.

---

### Method Details

### 1. `get_chat_logs`

Retrieves chat logs based on date range, pagination, and SOP chat filtering.

- **Parameters**:
    - `skip` (int): Number of records to skip for pagination. Default is `0`.
    - `limit` (int): Maximum number of records to return. Default is `25`.
    - `start_datetime` (str): Start date and time for filtering chat logs.
    - `end_datetime` (str): End date and time for filtering chat logs.
    - `is_sop_chat` (bool): Flag to filter SOP chats.
- **Returns**:
    - `ChatLogsResponse`: Contains the list of messages and total record count.
- **Raises**:
    - `ChatServiceException` for authentication failures or other errors during chat log retrieval.
- **Usage**:
    
    ```python
    chat_logs = chat_service.get_chat_logs(skip=0, limit=10, start_datetime="2023-01-01T00:00:00Z", end_datetime="2023-12-31T23:59:59Z")
    ```
    

---

### 2. `get_chat_history`

Retrieves the full chat history for a specific chat session.

- **Parameters**:
    - `chat_id` (str): Unique identifier of the chat session.
- **Returns**:
    - `ChatHistoryResponse`: Contains the list of messages in the chat session.
- **Raises**:
    - `ChatServiceException` for authentication failures or other errors during chat history retrieval.
- **Usage**:
    
    ```python
    chat_history = chat_service.get_chat_history(chat_id="chat123")
    ```
    

---

### 3. `chat`

Sends a chat message to the service and retrieves the response.

- **Parameters**:
    - `user_input` (str): The text input from the user.
    - `chat_id` (str): Unique identifier of the chat session.
    - `file_id` (str): Identifier of the file associated with the chat.
    - `stream` (bool): Indicates whether the response should be streamed. Default is `False`.
- **Returns**:
    - `ChatResponse`: Contains the details of the chat message, search results, and tags.
- **Raises**:
    - `ChatServiceException` for authentication failures or errors in sending the chat message.
- **Usage**:
    
    ```python
    chat_response = chat_service.chat(user_input="Hello, how can I assist you?", chat_id="chat123", file_id="file456", stream=False)
    ```
    

---

### Exception Handling

All methods in the `ChatOperations` class may raise a `ChatServiceException` if:

- Authentication fails (status code 401).
- The requested chat or chat logs are not found (status code 404).
- Any general API error arises during the request.

### Example Usage

```python
# Initialize the config and chat service
config = Config(auth_token="eyJhbGci....",env="https://subdomain.weav.ai/")
chat_service = ChatOperations(config=config)

# Example: Get chat logs
chat_logs = chat_service.get_chat_logs(skip=0, limit=5)

# Example: Get chat history for a specific chat
chat_history = chat_service.get_chat_history(chat_id="chat123")

# Example: Send a chat message
chat_response = chat_service.chat(user_input="Hello, can you help?", chat_id="chat123", file_id="file456")
```