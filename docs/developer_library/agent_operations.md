---
layout: libdoc/page
title: AgentOperations
description: The AgentOperations class provides various operations for interacting with agents, managing chat history, and retrieving agent responses within the Weav.ai platform. It handles authentication, validation, and error responses for simplified integration.
---
**Prerequisite** - To get started, ensure your python environment has the weav.ai developer library correctly installed [Installation Guide](setup).

---
### Initialization

To initialize the `AgentOperations` class, you need a `Config` object containing the authentication details.

```python
from weavaidev import Config
from weavaidev.agents import AgentOperations

config = Config(auth_token="eyJhbGci....",env="https://subdomain.weav.ai/")
agent_operations = AgentOperations(config=config)
```

- **Parameters**:
    - `config`: A `Config` object containing the base configuration and authentication token.

---

### Method Summary

1. **get_all_agents**: Retrieves all available agent types.
2. **get_agent_response**: Fetches an agent's response based on user input.
3. **get_agent**: Fetches configuration about a particular agent

---

### Method Details

### 1. `get_all_agents`

Fetches all available agent configurations from the system.

- **Parameters**:
    - None.
- **Returns**:
    - `AgentConfigurations`: A response object containing a list of available agent configurations.
- **Raises**:
    - `AgentServiceException` for authentication issues (status code 401) or other errors during retrieval.
- **Usage**:
    
    ```python
    agent_configurations = agent_operations.get_all_agents()
    ```
    

---

### 2. `get_agent_response`

Retrieves a response from a specific agent based on user input, specified agent ID, and other parameters.

- **Parameters**:
    - `user_input` (str): The user input for which the agent provides a response.
    - `chat_id` (str): Unique identifier for the chat session.
    - `agent_id` (str): The unique identifier of the agent for generating the response.
    - `stream` (bool): Indicates if the response should be streamed. Default is `False`.
- **Returns**:
    - `List[GetAgentResponse]`: A list of responses from the agent parsed from server-sent events (SSE).
- **Raises**:
    - `AgentServiceException` for authentication issues (status code 401), validation errors (status code 422), or other general API errors.
- **Usage**:
    
    ```python
    response = agent_operations.get_agent_response(user_input="Hello", chat_id="chat123", agent_id="<agent_id>")
    ```
    

---

### 3. `get_agent`

Fetches the configuration details for a specified agent based on its unique identifier.

- **Parameters**:
    - `agent_id` (str): The unique identifier of the agent whose configuration details are to be retrieved.
- **Returns**:
    - `AgentConfiguration`: An instance containing the agent’s configuration details.
- **Raises**:
    - `AgentServiceException` for authentication failure (status code 401), if the agent ID is not found (status code 404), or other errors.
- **Usage**:
    
    ```python
    agent_details = agent_operations.get_agent(agent_id="agent123")
    ```
    

---

### Exception Handling

All methods in the `AgentOperations` class may raise an `AgentServiceException` if:

- Authentication fails (status code 401).
- Validation issues arise (status code 422).
- The requested agent or agent configuration is not found (status code 404).
- Any other unexpected error occurs.