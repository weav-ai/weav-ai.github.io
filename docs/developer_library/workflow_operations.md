---
layout: libdoc/page
title: WorkflowOperations
description: The WorkflowOperations class provides methods for managing workflows in the Weav.ai platform. It simplifies interactions with the Weav.ai API to execute, retrieve, and manage workflows, handling authentication, validation, and error responses internally.

---
**Prerequisite** - To get started, ensure your python environment has the weav.ai developer library correctly installed [Installation Guide](setup).

---
### Initialization

To initialize the `WorkflowOperations` class, you need a `Config` object containing the necessary configuration details, including the authentication token.

```python

from weavaidev import Config
from weavaidev.workflows import WorkflowOperations

config = Config(auth_token="eyJhbGci....", env="https://subdomain.weav.ai/")
workflow_ops = WorkflowOperations(config=config)

```

- **Parameters**:
    - `config`: A `Config` object containing the base configuration, including the authentication token.

---

### Method Summary

1. **get_all_workflows**: Retrieves all workflows, with an option to include internal steps.
2. **get_single_workflow**: Fetches a specific workflow by name.
3. **skip_steps_in_workflow**: Skips specified tasks in a workflow.
4. **rerun_workflow**: Re-runs a specific workflow with updated data.
5. **run_workflow**: Executes a specific workflow.
6. **get_workflow_status**: Fetches the status of a workflow run.
7. **get_workflow_runs_for_document**: Retrieves workflow runs for a specific document.

---

### Method Details

### 1. `get_all_workflows`

Fetches all workflows from the system.

- **Parameters**:
    - `show_internal_steps` (bool, optional): Whether to include internal steps in the workflows. Defaults to `False`.
- **Returns**:
    - `GetAllWorkflowsResponse`: A list of workflows in the system.
- **Raises**:
    - `WorkflowException`: Raised if authentication fails or if an error occurs while fetching workflows.
- **Usage**:
    
    ```python
    all_workflows = workflow_ops.get_all_workflows(show_internal_steps=True)
    ```
    

---

### 2. `get_single_workflow`

Fetches a specific workflow by its name.

- **Parameters**:
    - `workflow_name` (str): The name of the workflow to fetch.
    - `show_internal_steps` (bool, optional): Whether to include internal steps in the workflow. Defaults to `False`.
- **Returns**:
    - `Workflow`: Details of the specified workflow.
- **Raises**:
    - `WorkflowException`: Raised if authentication fails, the workflow is not found, or if any other error occurs.
- **Usage**:
    
    ```python
    
    single_workflow = workflow_ops.get_single_workflow(workflow_name="example_workflow")
    ```
    

---

### 3. `skip_steps_in_workflow`

Skips specified tasks in a workflow.

- **Parameters**:
    - `workflow_name` (str): The name of the workflow.
    - `tasks` (str): Names of tasks to skip.
- **Returns**:
    - `Workflow`: The updated workflow after skipping tasks.
- **Raises**:
    - `WorkflowException`: Raised if authentication fails or an error occurs while skipping tasks.
- **Usage**:
    
    ```python
    
    updated_workflow = workflow_ops.skip_steps_in_workflow("example_workflow", "task1", "task2")
    ```
    

---

### 4. `rerun_workflow`

Re-runs a specific workflow with updated data.

- **Parameters**:
    - `workflow_name` (str): The name of the workflow to re-run.
    - `doc_id` (str): The document ID associated with the workflow.
    - `data` (Dict[str, Any]): Additional data for the workflow execution.
- **Returns**:
    - `RunWorkflowResponse`: Details of the re-run workflow.
- **Raises**:
    - `WorkflowException`: Raised if authentication fails or an error occurs during re-run.
- **Usage**:
    
    ```python
    
    rerun_response = workflow_ops.rerun_workflow("example_workflow", doc_id="doc123", data={"key": "value"})
    ```
    

---

### 5. `run_workflow`

Executes a specific workflow.

- **Parameters**:
    - `workflow_name` (str): The name of the workflow.
    - `doc_id` (str): The document ID associated with the workflow.
    - `data` (Dict[str, Any]): Data required for workflow execution.
- **Returns**:
    - `RunWorkflowResponse`: Details of the workflow execution.
- **Raises**:
    - `WorkflowException`: Raised if authentication fails or an error occurs during execution.
- **Usage**:
    
    ```python
    
    run_response = workflow_ops.run_workflow("example_workflow", doc_id="doc123", data={"key": "value"})
    ```
    

---

### 6. `get_workflow_status`

Fetches the status of a specific workflow run.

- **Parameters**:
    - `workflow_id` (str): The ID of the workflow.
    - `workflow_run_id` (str): The ID of the workflow run.
    - `show_internal_steps` (Optional[bool], optional): Whether to include internal steps. Defaults to `False`.
- **Returns**:
    - `WorkflowStatusResponse`: The current status of the workflow run.
- **Raises**:
    - `WorkflowException`: Raised if authentication fails or an error occurs while fetching status.
- **Usage**:
    
    ```python
    
    status = workflow_ops.get_workflow_status("workflow123", "run123", show_internal_steps=True)
    ```
    

---

### 7. `get_workflow_runs_for_document`

Retrieves workflow runs for a specific document.

- **Parameters**:
    - `doc_id` (str): The document ID.
    - `state` (str, optional): Filter by workflow state (e.g., "success"). Defaults to `"success"`.
    - `query` (str, optional): A search query. Defaults to `""`.
    - `skip` (int, optional): Records to skip for pagination. Defaults to `0`.
    - `limit` (int, optional): Maximum records to return. Defaults to `25`.
- **Returns**:
    - `DocumentWorkflowRunsResponse`: Workflow runs for the document.
- **Raises**:
    - `WorkflowException`: Raised if authentication fails or an error occurs while fetching workflow runs.
- **Usage**:
    
    ```python
    document_runs = workflow_ops.get_workflow_runs_for_document(doc_id="doc123", state="running", limit=10)
    ```
    

---

### Exception Handling

All methods in the `WorkflowOperations` class may raise a `WorkflowException` if:

- Authentication fails (status code 401).
- Validation issues occur (status code 422).
- The requested resource is not found (status code 404).
- Any general API error arises during the request.