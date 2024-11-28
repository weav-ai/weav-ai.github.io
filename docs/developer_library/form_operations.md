---
layout: libdoc/page
title: FormOperations
description: The FormOperations class provides a set of operations to create, retrieve, update, and delete forms, as well as execute analytics and download query results. This class interacts with the Weav.ai API, enabling simplified interactions with forms and their data.
---




**Prerequisite** - To get started, ensure your python environment has the weav.ai developer library correctly installed [Installation Guide](setup).

---

### Initialization

```python
from weavaidev import Config
from weavaidev.folders import FolderOperations

config = Config(auth_token="eyJhbGci....",env="https://subdomain.weav.ai/")
form_ops = FormOperations(config)
```

- **Parameters**:
    - `config`: A `Config` object containing the base configuration, including the authentication token.

---

### Method Summary

1. **create_form**: Create a new form with specified fields and metadata.
2. **filter_form**: Retrieve forms based on a query and scope.
3. **execute_form_analytics**: Run analytics on a form using specified criteria.
4. **filter_form_instances**: Filter form instances based on status, scope, category, and other parameters.
5. **get_form_definition**: Retrieve the definition of a specified form.
6. **update_form_definition**: Update the structure or metadata of a form.
7. **delete_form_definition**: Delete a specific form by ID.
8. **download_query_result**: Download query results from a form in JSON or CSV format.

---

### Method Details

### `create_form`

Creates a new form with specified fields and metadata.

- **Parameters**:
    - `form_data` (`CreateFormRequest`): Object containing the form details including `name`, `category`, `description`, `is_shared`, `is_searchable`, and `fields`.
        
        ### `CreateFormRequest` Parameters
        
        - **name** (`str`):
            
            The name of the form. This field is required.
            
        - **category** (`str`):
            
            The category to which the form belongs, useful for organizing similar forms. This field is required.
            
        - **description** (`Optional[str]`, default=""):
            
            A brief description of the form. This field is optional and defaults to an empty string if not provided.
            
        - **is_shared** (`Optional[bool]`, default=`False`):
            
            Indicates whether the form is shared across users. Defaults to `False` (not shared).
            
        - **is_searchable** (`Optional[bool]`, default=`False`):
            
            Determines if the form should appear in search results. Defaults to `False` (not searchable).
            
        - **fields** (`Optional[List[FormField]]`, default=`[]`):
            
            A list of fields that define the structure of the form. Each field in the list is of type `FormField`. This field is optional and defaults to an empty list if no fields are specified.
            
            ### **`FormField`** Parameters
            
            - **identifier** (`Optional[str]`, default=`str(uuid4())`):
                
                A unique identifier for the form field. By default, it is automatically generated using `uuid4()`.
                
            - **name** (`str`):
                
                The display name of the form field. This field is required.
                
            - **field_type** (`Literal["Number", "Date", "Text", "Table"]`):
                
                Specifies the data type of the form field. Options include:
                
                - `"Number"`: For numerical data.
                - `"Date"`: For date values.
                - `"Text"`: For textual data.
                - `"Table"`: For tabular data.
                
                This field is required.
                
            - **description** (`Optional[str]`, default=`""`):
                
                A brief description of the form field. Defaults to an empty string if not provided.
                
            - **is_array** (`Optional[bool]`, default=`False`):
                
                Specifies if the field can hold multiple values. Defaults to `False` (single value).
                
            - **fill_by_search** (`Optional[bool]`, default=`False`):
                
                Indicates whether this field can be populated through a search operation. Defaults to `False`.
                
- **Returns**:
    - `CreateFormResponse`: An object with details about the created form.
- **Raises**:
    - `FormProcessingException` if authentication fails or if there are issues during form creation.
- **Usage**:
    
    ```python
    
    form_data = CreateFormRequest(name="Survey Form", category="Survey", escription="Customer Survey")
    response = form_ops.create_form(form_data=form_data)
    ```
    

---

### `filter_form`

Filters forms based on a search query and scope.

- **Parameters**:
    - `query` (str): Search term for filtering.
    - `scope` (Literal["all_forms", "my_forms"]): Scope of the search; either all forms or the user’s forms.
    - `is_searchable` (Optional[bool]): Filter to include only searchable forms.
- **Returns**:
    - `FilterFormResponse`: Contains a list of forms that match the query.
- **Raises**:
    - `FormProcessingException` for authentication failures or if filtering fails.
- **Usage**:
    
    ```python
    response = form_ops.filter_form(query="Survey", scope="my_forms")
    ```
    

---

### `execute_form_analytics`

Executes analytics on a specified form.

- **Parameters**:
    - `form_id` (str): ID of the form.
    - `form_data` (`ExecuteFormAnalyticsRequest`): Request body containing query parameters, pagination, etc.
        
        ### `ExecuteFormAnalyticsRequest` Parameters
        
        - **query** (`str`, default=`"{'reason_for_no_pymongo_pipeline': 'No user request provided'}"`):
            
            A MongoDB-style query string used to specify the analytics criteria. By default, it includes a placeholder indicating that no specific query was provided.
            
        - **skip** (`int`, default=`0`):
            
            The number of records to skip in the result set, used for pagination. Defaults to `0` (no skipping).
            
        - **limit** (`int`, default=`25`):
            
            The maximum number of records to return in the result set, also used for pagination. Defaults to `25`.
            
- **Returns**:
    - `ExecuteFormAnalyticsResponse`: Object containing results, summary, and counts from the analytics query.
- **Raises**:
    - `FormProcessingException` if analytics execution fails.
- **Usage**:
    
    ```python
    form_data = ExecuteFormAnalyticsRequest(query="{{mongo_query}}", skip=0, limit=10)
    response = form_ops.execute_form_analytics(form_id="12345", form_data=form_data)
    ```
    

---

### `filter_form_instances`

Filters instances of a form based on various criteria.

- **Parameters**:
    - `form_data` (`FilterFormInstanceRequest`): Contains scope, status, category, and other filters.
        
        ### Parameters
        
        - **scope** (`Literal["all_documents", "current_document", "my_documents", "shared_documents"]`):
            
            Specifies the scope of the documents to be filtered. Options include:
            
            - `"all_documents"`: All available documents.
            - `"current_document"`: Only the current document.
            - `"my_documents"`: Documents owned by the current user.
            - `"shared_documents"`: Documents shared with the current user.
        - **status** (`Optional[Literal["NOT_STARTED", "IN_PROGRESS", "DONE", "FAILED"]]`, default=`None`):
            
            Filters based on the status of the form instance. Options include:
            
            - `"NOT_STARTED"`: Instances that haven't started.
            - `"IN_PROGRESS"`: Instances currently in progress.
            - `"DONE"`: Completed instances.
            - `"FAILED"`: Instances where processing failed.
        - **category** (`Optional[str]`, default=`""`):
            
            Specifies the category of the form instance. Defaults to an empty string (no category).
            
        - **query** (`Optional[str]`, default=`""`):
            
            A search query to further filter the form instances. Defaults to an empty string (no query).
            
        - **form_id** (`Optional[str]`, default=`""`):
            
            The ID of a specific form to filter instances by. Defaults to an empty string (no form ID).
            
        - **doc_id** (`Optional[str]`, default=`""`):
            
            The document ID associated with the form instance. Defaults to an empty string (no document ID).
            
        - **only_latest** (`Optional[bool]`, default=`False`):
            
            If set to `True`, only the latest form instances are returned. Defaults to `False`.
            
        - **skip** (`Optional[int]`, default=`0`):
            
            The number of records to skip in the result set, used for pagination. Defaults to `0` (no skipping).
            
        - **limit** (`Optional[int]`, default=`25`):
            
            The maximum number of records to return in the result set, also used for pagination. Defaults to `25`.
            
        - **all** (`Optional[bool]`, default=`True`):
            
            If set to `True`, returns all form instances, ignoring pagination limits. Defaults to `True`.
            
- **Returns**:
    - `FilterFormInstanceResponse`: Response with the list of filtered form instances.
- **Raises**:
    - `FormProcessingException` for authentication or filtering errors.
- **Usage**:
    
    ```python
    form_data = FilterFormInstanceRequest(scope="my_documents", status="IN_PROGRESS")
    response = form_ops.filter_form_instances(form_data=form_data)
    ```
    

---

### `get_form_definition`

Retrieves the definition of a specific form.

- **Parameters**:
    - `form_id` (str): ID of the form.
- **Returns**:
    - `GetFormDefinitonResponse`: Contains metadata and structure of the form.
- **Raises**:
    - `FormProcessingException` if the form definition cannot be retrieved.
- **Usage**:
    
    ```python
    response = form_ops.get_form_definition(form_id="12345")
    ```
    

---

### `update_form_definition`

Updates the metadata or structure of a form.

- **Parameters**:
    - `form_id` (str): ID of the form.
    - `form_data` (`UpdateFormDefinitonRequest`): Object containing the updated form details.
- **Returns**:
    - `GetFormDefinitonResponse`: Updated form details.
- **Raises**:
    - `FormProcessingException` if updating the form fails.
- **Usage**:
    
    ```python
    form_data = UpdateFormDefinitonRequest(name="Updated Survey Form")
    response = form_ops.update_form_definition(form_id="12345", form_data=form_data)
    ```
    

---

### `delete_form_definition`

Deletes a form by ID.

- **Parameters**:
    - `form_id` (str): ID of the form to delete.
- **Returns**:
    - `GetFormDefinitonResponse`: Confirmation of the form deletion.
- **Raises**:
    - `FormProcessingException` for authentication or deletion errors.
- **Usage**:
    
    ```python
    
    response = form_ops.delete_form_definition(form_id="12345")
    ```
    

---

### `download_query_result`

Downloads query results from a form, in JSON or CSV format.

- **Parameters**:
    - `form_id` (str): ID of the form.
    - `form_data` (`DownloadQueryResultRequest`): Object containing the query.
    - `download_format` (Literal["JSON", "CSV"]): Format for download; defaults to JSON.
        
        ### Parameters
        
        - **query** (`str`, default=`"{'reason_for_no_pymongo_pipeline': 'No user request provided'}"`):A MongoDB-style query string used to specify the data retrieval criteria. This field includes a default placeholder indicating that no specific query was provided.
- **Returns**:
    - `DownloadQueryResultResponse` for JSON format or `pd.DataFrame` for CSV format.
- **Raises**:
    - `FormProcessingException` if the download fails.
- **Usage**:
    
    ```python
    
    form_data = DownloadQueryResultRequest()
    response = form_ops.download_query_result(form_id="{{mongo_query}}", form_data=form_data, download_format="CSV")
    ```
    

---

### Exception Handling

All methods in `FormOperations` may raise a `FormProcessingException` if:

- Authentication fails.
- Validation issues occur.
- Any general API error arises during a request.