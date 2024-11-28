---
layout: libdoc/page
title: Forms
description: Learn how to create, manage, and analyze form definitions on the Weav.ai platform. This guide covers creating forms, updating definitions, filtering instances, and running analytics for efficient document processing.
---

## **Overview**
The Forms section enables users to define, manage, and analyze form structures on the Weav.ai platform. By setting up form definitions, users can extract structured data, run analytics, and streamline workflows with customizable fields and parameters. This guide provides detailed instructions for creating forms, filtering instances, and executing analytics on extracted data.

**Prerequisite** - To get started, ensure your environment is properly configured by following the [Setup Guide](setup).

-----
### Create form

Creating a form definition. A form definition is required before running `process_form` workflow.

```bash
python3 documents/forms/create_form.py --name "new form" --category "new" --description "test" --is_shared true --is_searchable true --fields "[{\n  \"name\": \"MICROSOFT FORM\",\n  \"description\": \"A form for microsoft\",\n  \"category\": \"ANNUAL REPORT\",\n  \"fields\": [\n    {\n      \"name\": \"Cost of revenue\",\n      \"field_type\": \"Number\",\n      \"is_array\": false,\n      \"fill_by_search\": false,\n      \"description\": \"Extract cost of revenue\"\n    }\n  ],\n  \"is_searchable\": false,\n  \"is_shared\": false\n}]"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| name | The form name | Required |  |
| category | A category name for the form | Required |  |
| description | The description of the form | Required |  |
| is_shared | A flag to decide sharing permissions | Optional (default : False) | false, f, False, true, t, True |
| is_searchable | A flag to decide visibility | Optional (default : False) | false, f, False, true, t, True |
| fields | Form fields | Optional | Stringified JSON |

Format for `fields`

```bash
[{
  "name": "str",
  "description": "str",
  "category": "str",
  "fields": [
    {
      "name": "str",
      "field_type": "Number",
      "is_array": boolean,
      "fill_by_search": boolean,
      "description": "str"
    }
  ],
  "is_searchable": boolean,
  "is_shared": boolean
}]

## Example stringified version for CLI

"[{\n  \"name\": \"MICROSOFT FORM\",\n  \"description\": \"A form for microsoft\",\n  \"category\": \"ANNUAL REPORT\",\n  \"fields\": [\n    {\n      \"name\": \"Cost of revenue\",\n      \"field_type\": \"Number\",\n      \"is_array\": false,\n      \"fill_by_search\": false,\n      \"description\": \"Extract cost of revenue\"\n    }\n  ],\n  \"is_searchable\": false,\n  \"is_shared\": false\n}]"
```

| **Key** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| name | Name of the Entity | Required |  |
| field_type | The data type of the entity | Required | "Number", "Date", "Text", "Table” |
| description | A short instruction to the prompt about the field | Optional |  |
| is_array | If it’s an entity with multiple values | Optional (default: False) | True, False |
| fill_by_search | Use internet search to fill this information | Optional (default: False | True, False |

**Response:**

```bash
{
   "category":"new",
   "created_at":datetime.datetime(2024, 9, 25, 20, 12, 11, tzinfo=datetime.timezone.utc),
   "description":"True",
   "fields":[
      {
         "description":"Net Sales for the quarter",
         "field_type":"Number",
         "fill_by_search":false,
         "is_array":true,
         "name":"Net Sales"
      }
   ],
   "id":"66f46e9b70dd6d497d9b8a37",
   "is_searchable":true,
   "is_shared":true,
   "name":"new form",
   "user_id":"google-oauth2|117349365869611297391"
}
```

---

### Delete Form definition

Deleting a form definition. The user is prompted to reconfirm the deletion. 

```bash
python3 documents/forms/delete_form_definition.py --form_id 66ea66d547fff0950cba17e
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| form_id | The unique identifier of the form | Required |

**Response:**

```bash
{
   "category":"new",
   "created_at":datetime.datetime(2024, 9, 25, 20, 12, 11, tzinfo=datetime.timezone.utc),
   "description":"True",
   "fields":[
      {
         "description":"Net Sales for the quarter",
         "field_type":"Number",
         "fill_by_search":false,
         "is_array":true,
         "name":"Net Sales"
      }
   ],
   "id":"66f46e9b70dd6d497d9b8a37",
   "is_searchable":true,
   "is_shared":true,
   "name":"new form",
   "user_id":"google-oauth2|117349365869611297391"
}
```

---

### Filter form

Search capabilities to retrieve form definitions

```bash
python3 documents/forms/filter_form.py --query "SECURITIES AND EXCHANGE COMMISSION" --scope "all_forms"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| scope | The scope of search | Required | all_forms, my_forms |
| is_searchable | Filter for visibility | Optional (default : False) | false, f, False, true, t, True |
| query | When applied, string matches category | Optional (default : “”) |  |

**Response:**

```bash
{
   "forms":[
      {
         "category":"SECURITIES AND EXCHANGE COMMISSION",
         "created_at":"2024-09-25T09:13:50Z",
         "description":"",
         "fields":[
            {
               "description":"",
               "field_type":"Date",
               "fill_by_search":false,
               "is_array":false,
               "name":"testEnitity1"
            }
         ],
         "id":"66f3d44eeb87303bc52bb9b4",
         "is_searchable":false,
         "is_shared":false,
         "name":"test",
         "user_id":"google-oauth2|117349365869611297391"
      }
   ]
}
```

---

### Get form definitions

Fetching the form definition of a particular form.

```bash
python3 documents/forms/get_form_definition.py --form_id "66f46e9b70dd6d497d9b8a37
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| form_id | Form ID | Required |

**Response:**

```bash
{
   "category":"SECURITIES AND EXCHANGE COMMISSION",
   "created_at":"2024-09-25T09:13:50Z",
   "description":"",
   "fields":[
      {
         "description":"",
         "field_type":"Date",
         "fill_by_search":false,
         "is_array":false,
         "name":"testEnitity1"
      }
   ],
   "id":"66f3d44eeb87303bc52bb9b4",
   "is_searchable":false,
   "is_shared":false,
   "name":"test",
   "user_id":"google-oauth2|117349365869611297391"
}
```

---

### Update form definition

Updating the definition of a form such as name, description, entities and their data etc.

```bash
python3 documents/forms/update_form_definition.py --form_id 66f46e9b70dd6d497d9b8a37 --name "update" --category "new" --description "Test desc" --is_shared false --is_searchable false
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| form_id | Form identifier | Required |  |
| name | Form name | Required |  |
| category | Form category | Required |  |
| description  | Form description | Optional (default : “”) |  |
| is_shared | Filter for sharing permissions | Optional (default : False) | false, f, False, true, t, True |
| is_searchable | Filter for visibility | Optional (default : False) | false, f, False, true, t, True |
| fields | Form fields | Optional | Stringified JSON |

Format for `fields`

```bash
[{
  "name": "str",
  "description": "str",
  "category": "str",
  "fields": [
    {
      "name": "str",
      "field_type": "Number",
      "is_array": boolean,
      "fill_by_search": boolean,
      "description": "str"
    }
  ],
  "is_searchable": boolean,
  "is_shared": boolean
}]

## Example stringified version for CLI

"[{\n  \"name\": \"MICROSOFT FORM\",\n  \"description\": \"A form for microsoft\",\n  \"category\": \"ANNUAL REPORT\",\n  \"fields\": [\n    {\n      \"name\": \"Cost of revenue\",\n      \"field_type\": \"Number\",\n      \"is_array\": false,\n      \"fill_by_search\": false,\n      \"description\": \"Extract cost of revenue\"\n    }\n  ],\n  \"is_searchable\": false,\n  \"is_shared\": false\n}]"
```

| **Key** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| name | Name of the Entity | Required |  |
| field_type | The data type of the entity | Required | "Number", "Date", "Text", "Table” |
| description | A short instruction to the prompt about the field | Optional |  |
| is_array | If it’s an entity with multiple values | Optional (default: False) | True, False |
| fill_by_search | Use internet search to fill this information | Optional (default: False | True, False |

**Response:**

```bash
{
   "category":"SECURITIES AND EXCHANGE COMMISSION",
   "created_at":"2024-09-25T09:13:50Z",
   "description":"",
   "fields":[
      {
         "description":"",
         "field_type":"Date",
         "fill_by_search":false,
         "is_array":false,
         "name":"testEnitity1"
      }
   ],
   "id":"66f3d44eeb87303bc52bb9b4",
   "is_searchable":false,
   "is_shared":false,
   "name":"test",
   "user_id":"google-oauth2|117349365869611297391"
}
```

---

### Filter form instances

A search query for retrieving form instances.

```bash
python3 documents/forms/filter_form_instances.py --scope all_documents --status "DONE" --category "SECURITIES AND EXCHANGE COMMISSION"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| scope | The scope of search | Required |  "all_documents",
"current_document",
"my_documents",
"shared_documents" |
| status | Status of workflow | Optional | "NOT_STARTED", "IN_PROGRESS", "DONE", "FAILED” |
| category | Category of  | Optional |  |
| query | When applied, string matches category | Optional |  |
| form_id | Form identifier | Optional |  |
| doc_id | Document identifier | Optional |  |
| only_latest | Fetches only latest | Optional (default True) |  |
| skip | Number of documents to skip | Optional (default 0) |  |
| limit | Max number of documents | Optional (default 25) |  |
| all | If set to true, all instances are fetched | Optional (default : False) |  |

**Response:**

```bash
{
   "total":4,
   "form_instances":[
      {
         "form_instance":{
            "data":[
               {
                  "name":"testEnitity1",
                  "value":"2023-08-03T00:00:00",
                  "identifier":"1f929c08-655e-4cf2-845c-18e3eb428ce7",
                  "weav_page_number":24
               }
            ],
            "metadata":{
               "modified_at":"2024-09-25T22:09:52.016000",
               "status":"DONE"
            }
         },
         "doc_id":"66e0fba3089fbd21c4dd80c3",
         "form_id":"66f3d44eeb87303bc52bb9b4",
         "file_name":"AAPL_10Q.pdf",
         "status":"AI_READY",
         "category":"SECURITIES AND EXCHANGE COMMISSION",
         "in_folders":[
            "66e0f93093798ee1c937e39a"
         ],
         "owner_id":"google-oauth2|117349365869611297391"
      },
      .
      .
      .
      .
   ]
}
```

---

### Execute form analytics

Running analytics on a form.

```bash
python3 documents/forms/execute_form_analytics.py --form_id 66f3d44eeb87303bc52bb9b4 --skip 0 --limit 25
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| skip | Number of documents to skip | Optional (default 0) |
| limit | Total number of documents to consider | Optional (default 25) |
| form_id | Form Identifier | Required |
| query | This is a mongo pipeline query that is fetched from the Search service. (default:  "{\n    'reason_for_no_pymongo_pipeline': 'No user request provided'\n}”) | Optional |

**Response:**

```bash
{
   "columns":[
      "Net Sales",
      "Total sales"
   ],
   "results":[
      {
         "Net Sales":[
            81797.0,
            82959.0,
            .
            .
            .
            .
         ],
         "metadata":{
            "_id":"66fe1b65b1d0dfb13c9975f0",
            "file_name":"AAPL_10Q.pdf",
            "in_folders":[
               
            ]
         }
      },
      {
         "Net Sales":[
            81797.0,
            82959.0,
            .
            .
            .
            .
         ],
         "Total sales":94569.0,
         "metadata":{
            "_id":"66fe29e1eb87303bc52bba93",
            "file_name":"AAPL_10Q.pdf",
            "in_folders":[
               
            ]
         }
      }
   ],
   "summary":"",
   "total_count":2
}
```

---

### Download query result

```bash
python3 documents/forms/download_query_result.py --form_id 66f3d44eeb87303bc52bb9b4 --download_format "JSON" 
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| form_id | Form Identifier | Required |  |
| query | This is a mongo pipeline query that is fetched from the Search service. (default:  "{\n    'reason_for_no_pymongo_pipeline': 'No user request provided'\n}”) | Optional |  |
| download_format | The format in which the results need to be viewed | Optional (default : “JSON”) | JSON, CSV |

**Response:**

`--download_format = "JSON"`

```bash
{'docs': [{'testEnitity1': '2023-08-03T00:00:00'}]}
```

`--download_format = "CSV"`

```bash
          testEnitity1
0  2023-08-03T00:00:00
```