---
layout: libdoc/page
title: ActionOperations
description: The ActionOperations class provides functionality to retrieve available action types and fetch configuration details for specific action types within the Weav.ai platform. This class ensures proper handling of authentication, validation, and error responses, making it straightforward to integrate and interact with action configurations.
---

**Prerequisite** - To get started, ensure your python environment has the weav.ai developer library correctly installed [Installation Guide](setup).

---

### Initialization

```python
from weavaidev import Config
from weavaidev.actions import ActionOperations

config = Config(auth_token="eyJhbGci....",env="https://subdomain.weav.ai/")
action_ops = ActionOperations(config=config)
```

- **Parameters**:
    - `config`: A `Config` object containing the base configuration and authentication token.

---

### Method Summary

1. **get_action_types**: Uploads a document and creates a new record.
2. **get_action_type_configuration**: Fetches details of a specific page in a document.

### Method Details

### 1. `get_action_types`

Retrieves all available action types from the configured API endpoint.

- **Parameters**:
    - None.
- **Returns**:
    - `List[Dict]`: A JSON response containing the available action types.
- **Raises**:
    - `ActionOperationsException` for authentication issues (status code 401), when action types are not found (status code 404), or other general API errors.
- **Usage**:
    
    ```python
    action_types = action_ops.get_action_types()
    ```
    

---

### 2. `get_action_type_configuration`

Fetches the configuration details for a specified action type based on its unique identifier.

- **Parameters**:
    - `action_type` (str): The unique identifier of the action type whose configuration details are to be retrieved.
- **Returns**:
    - `Action`: An instance of `Action` populated with configuration data from the API response.
- **Raises**:
    - `ActionOperationsException` for authentication failure (status code 401), if the action type is not found (status code 404), or other errors.
- **Usage**:
    
    ```python
    action_configuration = action_ops.get_action_type_configuration(action_type="sample_action")
    ```
    

---

### Exception Handling

Both methods in the `ActionOperations` class may raise an `ActionOperationsException` in cases such as:

- Unauthorized access (status code 401).
- Action type or configuration not found (status code 404).
- Any other unexpected errors, providing debugging details.