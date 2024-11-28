---
layout: libdoc/page
title: DocumentOperations
description: The DocumentOperations class provides various operations for uploading, retrieving, analyzing, and managing documents within the Weav.ai platform. This class handles authentication, validation, and error responses for ease of use.
---
**Prerequisite** - To get started, ensure your python environment has the weav.ai developer library correctly installed [Installation Guide](setup).

---

### Initialization

To initialize the `DocumentOperations` class, you need a `Config` object containing the authentication details.

```python
from weavaidev import Config
from weavaidev.documents import DocumentOperations

config = Config(auth_token="eyJhbGci....",env="")
doc_ops = DocumentOperations(config=config)
```

- **Parameters**:
    - `config`: A `Config` object containing the base configuration and authentication token.

---

### Method Summary

1. **create_document**: Uploads a document and creates a new record.
2. **get_page**: Fetches details of a specific page in a document.
3. **get_page_text_and_words**: Retrieves text and word-level data for a page.
4. **get_page_level_status**: Retrieves the page-level status of a document.
5. **get_document_summary_status**: Retrieves summary and redacted summary status of a document.
6. **get_document**: Fetches full details of a document.
7. **get_document_hierarchy**: Retrieves hierarchical structure of a document.
8. **download_form_instance**: Downloads a form instance in JSON or CSV format.
9. **get_document_categories**: Fetches available document categories.
10. **get_document_tags**: Retrieves all available document tags.
11. **trigger_document_summary**: Initiates generation of a document summary.

---

### Method Details

### 1. `create_document`

Uploads a document to create a new record.

- **Parameters**:
    - `file_path` (str): Path to the document file.
    - `folder_id` (Optional[str]): Folder ID for the document. Defaults to an empty string.
- **Returns**:
    - `CreateDocumentResponse`: Contains details about the created document.
- **Raises**:
    - `FileNotFoundError` if the file does not exist.
    - `DocumentProcessingException` for any error during document creation.
- **Usage**:
    
    ```python
    response = doc_ops.create_document(file_path="/path/to/document.pdf", folder_id="folder123")
    ```
    

---

### 2. `get_page`

Fetches the status and details of a specific page within a document.

- **Parameters**:
    - `document_id` (str): ID of the document.
    - `page_number` (int): Page number.
    - `bounding_boxes` (Optional[bool]): Flag to include bounding boxes for text. Default is `False`.
- **Returns**:
    - `GetPageStatusResponse`: Details of the specified page.
- **Raises**:
    - `DocumentProcessingException` for authentication, validation, or page retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_page(document_id="doc123", page_number=1, bounding_boxes=True)
    ```
    

---

### 3. `get_page_text_and_words`

Retrieves text and word-level details for a page.

- **Parameters**:
    - `document_id` (str): ID of the document.
    - `page_number` (int): Page number.
- **Returns**:
    - `GetPageTextResponse`: Contains text, words, and extracted entities for the page.
- **Raises**:
    - `DocumentProcessingException` for authentication, validation, or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_page_text_and_words(document_id="doc123", page_number=1)
    ```
    

---

### 4. `get_page_level_status`

Retrieves page-level processing status for a document.

- **Parameters**:
    - `document_id` (str): ID of the document.
- **Returns**:
    - `PageLevelStatusResponse`: Contains OCR, classification, and entity extraction statuses.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_page_level_status(document_id="doc123")
    ```
    

---

### 5. `get_document_summary_status`

Retrieves summary and redacted summary status for a document.

- **Parameters**:
    - `document_id` (str): ID of the document.
- **Returns**:
    - `DocumentSummaryResponse`: Summary and redacted summary status.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_document_summary_status(document_id="doc123")
    ```
    

---

### 6. `get_document`

Fetches full details of a document.

- **Parameters**:
    - `document_id` (str): ID of the document.
    - `fill_pages` (Optional[bool]): Flag to include detailed page data. Default is `False`.
- **Returns**:
    - `CreateDocumentResponse`: Document details, including pages and metadata.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_document(document_id="doc123", fill_pages=True)
    ```
    

---

### 7. `get_document_hierarchy`

Fetches the hierarchical structure of a document.

- **Parameters**:
    - `document_id` (str): ID of the document.
- **Returns**:
    - `DocumentHierarchyResponse`: Contains the structure of the document.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_document_hierarchy(document_id="doc123")
    ```
    

---

### 8. `download_form_instance`

Downloads a form instance from a document in the specified format.

- **Parameters**:
    - `document_id` (str): ID of the document.
    - `download_format` (Literal["JSON", "CSV"]): Format for download. Default is `JSON`.
- **Returns**:
    - `Dict[str, Any]` if JSON, or `pd.DataFrame` if CSV.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.download_form_instance(document_id="doc123", download_format="CSV")
    ```
    

---

### 9. `get_document_categories`

Fetches all available document categories.

- **Parameters**:
    - None.
- **Returns**:
    - `DocumentCategoriesResponse`: List of document categories.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_document_categories()
    ```
    

---

### 10. `get_document_tags`

Retrieves all available tags for documents.

- **Parameters**:
    - None.
- **Returns**:
    - `DocumentTagResponse`: List of tags for documents.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.get_document_tags()
    ```
    

---

### 11. `trigger_document_summary`

Triggers the generation of a summary for a document.

- **Parameters**:
    - `document_id` (str): ID of the document.
- **Returns**:
    - `DocumentSummaryResponse`: Summary and redacted summary of the document.
- **Raises**:
    - `DocumentProcessingException` for authentication or retrieval errors.
- **Usage**:
    
    ```python
    response = doc_ops.trigger_document_summary(document_id="doc123")
    ```
    

---

### Exception Handling

All methods in `DocumentOperations` may raise a `DocumentProcessingException` if:

- Authentication fails (status code 401).
- Validation issues occur (status code 422).
- The requested document or resource is not found (status code 404).
- Any general API error arises during the request.