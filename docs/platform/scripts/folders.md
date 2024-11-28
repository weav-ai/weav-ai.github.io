---
layout: libdoc/page
title: Folders
description: Learn how to manage and organize documents effectively on the Weav.ai platform using folders. This guide covers creating folders, retrieving metadata, and managing folder-level operations for better document organization.
---

## **Overview**
The Folders section allows users to group and manage documents efficiently on the Weav.ai platform. By organizing documents into categorized folders, users can streamline their workflows, enhance accessibility, and maintain better control over their data. Key functionalities include creating folders, retrieving writable folder lists, and accessing detailed folder metadata.

**Prerequisite** - To get started, ensure your environment is properly configured by following the [Setup Guide](setup).

-----
### Create folder

Allows the user to create a folder that they can use to group documents together.

```bash
python3 documents/folders/create_folder.py --name "Bank docs" --description "BANKING DOCUMENT" --category "BANKING"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| name | Name of the folder to be created | Required |
| category | The category of the entire folder | Optional |
| description | A description for the folder | Optional |

**Response:**

```bash
{
   "id":"66ff3b79eb87303bc52bbae4",
   "documents":"None",
   "document_ids":[
      
   ],
   "shared_with_users":{
      
   },
   "shared_with_groups":{
      
   },
   "name":"Bank docs",
   "category":"BANKING",
   "description":"BANKING DOCUMENT",
   "created_at":"2024-10-04T00:48:57Z",
   "modified_at":"2024-10-04T00:48:57.573312Z",
   "user_id":"google-oauth2|117349365869611297391",
   "tenant_id":"",
   "workflow":{
      "workflow_id":"process_document_sensors",
      "form_id":"",
      "workflow_params":{
         
      }
   }
}
```

---

### Get writable folders

Gets a list of all writable folders giving their `name` and `folder_id`

```bash
python3 documents/folders/get_writable_folders.py
```

**Response:**

```bash
{
   "folders":[
      {
         "id":"66ff3b79eb87303bc52bbae4",
         "name":"Bank docs"
      },
	    .
	    .
	    .
   ]
}
```

### Get folder definition

Gets all metadata about a folder. Also displays the IDs of documents present inside the folder.

```bash
python3 documents/folders/get_folder_definition.py --folder_id 66e0f93093798ee1c937e39aent
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| folder_id | The unique identifier of the folder | Required |

**Response:**

```bash
{
   "category":"",
   "created_at":"2024-09-11T01:58:08Z",
   "description":"",
   "document_ids":[
      "66f9ccbb927ce8c0ebda4261",
      "66e0fba3089fbd21c4dd80c3"
   ],
   "documents":"None",
   "id":"66e0f93093798ee1c937e39a",
   "modified_at":"2024-10-04T01:01:05.011000",
   "name":"BANKING",
   "shared_with_groups":{
      
   },
   "shared_with_users":{
      
   },
   "tenant_id":"",
   "user_id":"google-oauth2|117349365869611297391",
   "workflow":{
      "form_id":"",
      "workflow_id":"process_document_sensors",
      "workflow_params":{
         
      }
   }
}
```