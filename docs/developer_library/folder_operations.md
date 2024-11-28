---
layout: libdoc/page
title: FolderOperations
description: The FolderOperations class provides methods for creating, retrieving, and managing folders within the Weav.ai platform. It offers simplified interactions with the Weav.ai API to manage folder resources, handling authentication, validation, and error responses internally.
---

**Prerequisite** - To get started, ensure your python environment has the weav.ai developer library correctly installed [Installation Guide](setup).

---
### Initialization

To initialize the `FolderOperations` class, you need a `Config` object containing the necessary configuration details, including the authentication token.

```python
from weavaidev import Config
from weavaidev.folders import FolderOperations

config = Config(auth_token="eyJhbGci....",env="https://subdomain.weav.ai/")
folder_ops = FolderOperations(config=config)
```

- **Parameters**:
    - `config`: A `Config` object containing the base configuration, including the authentication token.

---

### Method Summary

1. **create_folder**: Creates a new folder with specified details.
2. **get_writable_folders**: Retrieves folders to which the user has write access.
3. **get_folder_definition**: Fetches detailed information about a specific folder.

---

### Method Details

### 1. `create_folder`

Creates a new folder with the specified name, category, and description.

- **Parameters**:
    - `name` (str): The name of the folder.
    - `category` (Optional[str]): The category of the folder. Default is an empty string.
    - `description` (Optional[str]): The description of the folder. Default is an empty string.
- **Returns**:
    - `CreateFolderResponse`: Contains details about the created folder, including its ID, documents, and workflow.
- **Raises**:
    - `FolderProcessingException`: Raised if the creation request fails due to authentication, validation, or other issues.
- **Usage**:
    
    ```python
    response = folder_ops.create_folder(name="New Project Folder", category="Project", description="Folder for project documents")
    ```
    

---

### 2. `get_writable_folders`

Fetches a list of folders that the user has write access to.

- **Parameters**:
    - None.
- **Returns**:
    - `WritableFoldersResponse`: Contains a list of folders that the user can write to, including their names and IDs.
- **Raises**:
    - `FolderProcessingException`: Raised if authentication fails or if the request fails.
- **Usage**:
    
    ```python
    writable_folders = folder_ops.get_writable_folders()
    ```
    

---

### 3. `get_folder_definition`

Fetches detailed information about a specific folder, including its documents, workflow, and metadata.

- **Parameters**:
    - `folder_id` (str): The ID of the folder whose details are being fetched.
- **Returns**:
    - `CreateFolderResponse`: Contains the full details of the folder, including its ID, documents, and workflow.
- **Raises**:
    - `FolderProcessingException`: Raised if authentication fails, folder validation fails, or if the folder is not found.
- **Usage**:
    
    ```python
    folder_details = folder_ops.get_folder_definition(folder_id="folder123")
    ```
    

---

### Exception Handling

All methods in the `FolderOperations` class may raise a `FolderProcessingException` if:

- Authentication fails (status code 401).
- Validation issues occur (status code 422).
- The requested resource is not found (status code 404).
- Any general API error arises during the request.

### Example Usage

```python
# Initialize the config and folder operations
config = Config(auth_token="eyJhbGci....",env="https://subdomain.weav.ai/")
folder_ops = FolderOperations(config=config)

# Example: Create a folder
create_response = folder_ops.create_folder(name="Research Documents", category="Research", description="Folder for research-related files")

# Example: Retrieve writable folders
writable_folders = folder_ops.get_writable_folders()

# Example: Get folder definition
folder_details = folder_ops.get_folder_definition(folder_id="folder123")
```

This documentation provides a comprehensive overview of the `FolderOperations` class, detailing each method, its parameters, return types, exceptions, and usage examples.