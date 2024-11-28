---
layout: libdoc/page
title: Documents
description: Learn how to interact with and manage documents using the Weav.ai platform. This guide covers uploading, retrieving, analyzing and more.
---

## **Overview**
The Documents section provides functionalities to upload, manage, and extract insights from documents on the Weav.ai platform. This guide walks you through key operations such as creating documents, retrieving metadata, downloading forms, generating summaries, and analyzing page-level data. These tools are designed to streamline document workflows and improve efficiency.

**Prerequisite** - To get started, ensure your environment is properly configured by following the [Setup Guide](setup).

-----
### Create document

Upload a document from your local system to the copilot. Has the ability to upload documents into a folder.

```bash
python3 documents/documents/create_document.py --file_path "AAPL_10Q.pdf"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| file_path | The file path to your document on your local system | Required |
| folder_id | The ID of the folder that should hold the document. If it is not provided, the file will be uploaded but will not be inside any folder. | Optional |

**Response:**

Upon uploading, the document response should be as follows.

```bash
{
   "ai_tags":[
      
   ],
   "category":"",
   "created_at":datetime.datetime(2024, 10, 3, 22, 14, 10, tzinfo=TzInfo(UTC)),
   "download_url":"/doc-proc-service/local_store/google-oauth2|117349365869611297391/66ff1732927ce8c0ebda42bd/66ff1732927ce8c0ebda42bd",
   "file_name":"AAPL_10Q.pdf",
   "form_instances":"None",
   "id":"66ff1732927ce8c0ebda42bd",
   "in_folders":[
      
   ],
   "media_type":"application/pdf",
   "pages":[
      
   ],
   "redacted_summary":"",
   "size":654929,
   "source":"application",
   "status":"NEW",
   "step_status":{
      "FORM_EXTRACTION":{
         "error":"",
         "modified_at":datetime.datetime(2024, 10, 3, 22, 14, 10, tzinfo=TzInfo(UTC)),
         "response":{
            
         },
         "status":"NOT_STARTED"
      }
   },
   "summary":"",
   "summary_status":"",
   "tags":[
      
   ],
   "tenant_id":"",
   "user_id":"google-oauth2|117349365869611297391"
}
```

After the document has been uploaded, it undergoes `process_document_sensors`  workflow which may take a while. 

Once the processing is completed, you will notice that some fields from before are updated using information extracted from the document using the `Get Document` API

---

### Get Document

Retrieve information about the uploaded document. Fields in this response might be empty initially but are completely filled once basic processing is completed

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| document_id | The unique identifier of the document | Required |
| fill_pages | If false pages will be empty | Optional (default: False) |

**Response:**

```bash
{
   "id":"66ff1732927ce8c0ebda42bd",
   "media_type":"application/pdf",
   "download_url":"/doc-proc-service/local_store/google-oauth2|117349365869611297391/66ff1732927ce8c0ebda42bd/66ff1732927ce8c0ebda42bd",
   "pages":[
      
   ],
   "status":"AI_READY",
   "file_name":"AAPL_10Q.pdf",
   "created_at":"",
   "size":654929,
   "source":"application",
   "category":"UNITED STATES SECURITIES AND EXCHANGE COMMISSION",
   "summary":"",
   "redacted_summary":"",
   "summary_status":"",
   "step_status":{
      "FORM_EXTRACTION":{
         "status":"DONE",
         "modified_at":"",
         "error":"",
         "response":{
            "name":"New",
            "category":"UNITED STATES SECURITIES AND EXCHANGE COMMISSION",
            "description":"A Test form",
            "fields":[
               {
                  "identifier":"4b68933c-2432-4784-8b01-37a1803b72a0",
                  "name":"new",
                  "field_type":"Number",
                  "description":"",
                  "is_array":true,
                  "fill_by_search":false,
                  "value":[
                     "0.00001",
                     "1.375",
                     .
                     .
                  ],
                  "weav_page_number":[
                     0,
                     0,
                     0,
                     0,
                     0,
                     .
                     .
                  ]
               }
            ],
            "is_shared":false,
            "is_searchable":false,
            "_id":"66ff076db1d0dfb13c99760f",
            "user_id":"google-oauth2|117349365869611297391",
            "created_at":"2024-10-03T21:06:53Z",
            "form_id":"66ff076db1d0dfb13c99760f"
         }
      }
   },
   "in_folders":[
      
   ],
   "tags":[
      
   ],
   "ai_tags":[
      
   ],
   "user_id":"google-oauth2|117349365869611297391",
   "tenant_id":"",
   "form_instances":"None"
}
```

<aside>
💡

You can observe that fields like `category` are updated once processing has finished.

</aside>

---

### Download form instance

Allows the user to download the extracted form

```bash
python3 documents/documents/download_form_instance.py --download_format "JSON" --document_id "66ff1732927ce8c0ebda42bd"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| document_id | The unique identifier of the document | Required |  |
| download_format | The format in which the results need to be viewed | Optional (default : “JSON”) | JSON, CSV |

**Response:**

`--download_format = "JSON"`

```bash
{
   "doc_id":"66ff1732927ce8c0ebda42bd",
   "form_id":"66ff076db1d0dfb13c99760f",
   "new":[
      "0.00001",
      "1.375",
      "0.000",
      .
      .
      .
    ]
}
```

`--download_format = "CSV"`

```bash
                     doc_id                   form_id                                                new
0  66ff1732927ce8c0ebda42bd  66ff076db1d0dfb13c99760f  ['0.00001', '1.375', '0.000', '0.875', '1.625'...
```

<aside>
💡

Downloading form instance can only be done once form processing has been completed and form definition has been created.

</aside>

<aside>
💡

If form processing has not been run, you can run it by first [creating a form definition](https://www.notion.so/How-to-use-the-Developer-Scripts-10c5084286c5801bb388fbcc98dc5626?pvs=21) and using the [Run workflow](https://www.notion.so/How-to-use-the-Developer-Scripts-10c5084286c5801bb388fbcc98dc5626?pvs=21) script by passing `workflow_name` as `process_form_workflow` 

</aside>

---

### Get document categories

Retrieves a list of all categories present on the copilot, considering categories from all documents.

```bash
python3 documents/documents/get_document_categories.py
```

**Response:**

```bash
{
   "categories":[
      "ANNUAL REPORT",
      "SECURITIES AND EXCHANGE COMMISSION",
      "UNITED STATES SECURITIES AND EXCHANGE COMMISSION"
   ]
}
```

---

### Get document tags

Retrieves a list of all tags present on the copilot, considering tags from all documents.

```bash
python3 documents/documents/get_document_tags.py
```

**Response:**

```bash
{'tags': [['apple']]}
```

---

### Get document page level status

Retrieves count of pages on which workflow has succeeded or failed.

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| document_id | The unique identifier of the document | Required |

**Response:**

```bash
{
   "classification":{
      "pages_done":25,
      "pages_failed":0
   },
   "entity_extraction":{
      "pages_done":25,
      "pages_failed":0
   },
   "ocr":{
      "pages_done":25,
      "pages_failed":0
   },
   "vectorization":{
      "pages_done":25,
      "pages_failed":0
   }
}
```

---

### Get page text and words

Retrieves information about words and text in a single page of the document.

```bash
python3 documents/documents/get_page_text_and_words.py --document_id 66f9ccbb927ce8c0ebda4261 --page_number 1
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| document_id | The unique identifier of the document | Required |
| page_number | The page number for which the information is required | Required |

**Response:**

```bash
{
   "page_number":1,
   "media_type":"NONE",
   "page_text":"9/6/23, 9:57 AM\naapl-20230701\nIndicate by check mark whether the Registrant has submitted electronically every Interactive Data File required to be submitted pursuant to Rule 405 of Regulation S-T (§232.405 of this chapter) during the preceding 12 months (or for such shorter period that the Registrant was required to submit such files).\nYes :selected: :unselected: No\nIndicate by check mark whether the Registrant is a large accelerated filer, an accelerated filer, a non-accelerated filer, a smaller reporting company, or an emerging growth company. See the definitions of \"large accelerated filer,\" \"accelerated filer,\" \"smaller reporting company,\" and \"emerging growth company\" in Rule 12b-2 of the Exchange Act.\nLarge accelerated filer :selected:\nAccelerated filer :unselected:\nNon-accelerated filer :unselected:\nSmaller reporting company :unselected:\nEmerging growth company :unselected:\nIf an emerging growth company, indicate by check mark if the Registrant has elected not to use the extended transition period for complying with any new or revised financial accounting standards provided pursuant to Section 13(a) of the Exchange Act. :unselected:\nIndicate by check mark whether the Registrant is a shell company (as defined in Rule 12b-2 of the Exchange Act). :unselected: Yes :selected: No\n15,634,232,000 shares of common stock were issued and outstanding as of July 21, 2023.\nhttps://www.sec.gov/Archives/edgar/data/320193/000032019323000077/aapl-20230701.htm\n2/31",
   "status":"NONE",
   "classification":{
      "page_class":"Company Regulatory Compliance",
      "page_sections":[
         "Interactive Data File Submission",
         "Company Classification",
         "Emerging Growth Company Status",
         "Shell Company Status",
         "Common Stock Issuance"
      ],
      "page_no":2
   },
   "extracted_entities":[
      {
         "entity_group":"default",
         "entities":[
            {
               "polygon":[
                  
               ],
               "key":"Date",
               "value":"9/6/23, 9:57 AM",
               "label":"Document Date",
               "is_sensitive":false
            },
            {
               "polygon":[
                  
               ],
               "key":"Document ID",
               "value":"aapl-20230701",
               "label":"Document Identifier",
               "is_sensitive":false
            },
            .
            .
            .
         ]
      }
   ],
   "redacted_summary":"",
   "words":[
      {
         "content":"9/6/23,",
         "polygon":[
            {
               "x":71.0,
               "y":42.0
            },
            .
            .
         ],
         "span":{
            "offset":0,
            "length":7
         },
         "confidence":0.994
      },
      .
      .
      .
   ]
}
```

---

### Get Page

Retrieves all the information from a single page of a document

```bash
python3 documents/documents/get_page.py --document_id 66f9ccbb927ce8c0ebda4261 --page_number 1
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| document_id | The unique identifier of the document | Required |  |
| page_number | The page number for which the information is required | Required |  |
| bounding_boxes | Get information about bounding boxes polygons | Optional | false, f, False, true, t, True |

**Response:**

```bash
{
   "classification":{
      "page_class":"Company Regulatory Compliance",
      "page_no":2,
      "page_sections":[
         "Interactive Data File Submission",
         "Company Classification",
         "Emerging Growth Company Status",
         "Shell Company Status",
         "Common Stock Issuance"
      ]
   },
   "download_url":"/doc-proc-service/local_store/google-oauth2|117349365869611297391/66ff1732927ce8c0ebda42bd/1.jpg",
   "extracted_entities":[
      {
         "entities":[
            {
               "is_sensitive":false,
               "key":"Date",
               "label":"Document Date",
               "polygon":[
                  [
                     [
                        {
                           "x":71.0,
                           "y":42.0
                        },
                        {
                           "x":135.0,
                           "y":43.0
                        },
                        {
                           "x":136.0,
                           "y":66.0
                        },
                        {
                           "x":71.0,
                           "y":66.0
                        }
                     ]
                  ],
                  [
                     [
                        {
                           "x":140.0,
                           "y":43.0
                        },
                        {
                           "x":180.0,
                           "y":43.0
                        },
                        {
                           "x":180.0,
                           "y":66.0
                        },
                        {
                           "x":140.0,
                           "y":66.0
                        }
                     ]
                  ],
                  [
                     [
                        {
                           "x":185.0,
                           "y":43.0
                        },
                        {
                           "x":212.0,
                           "y":42.0
                        },
                        {
                           "x":212.0,
                           "y":66.0
                        },
                        {
                           "x":185.0,
                           "y":66.0
                        }
                     ]
                  ]
               ],
               "value":"9/6/23, 9:57 AM"
            },
            .
            .
            .
         ],
         "entity_group":"default"
      }
   ],
   "media_type":"image/jpeg",
   "page_hierarchy":"None",
   "page_number":1,
   "page_text":"Indicate by check mark whether the Registrant has submitted electronically every Interactive Data File required to be ....",
   "redacted_summary":"The document, identified as 'aapl-20230701', was ... ",
   "sensitive_words":[
      
   ],
   "status":"VECTORIZATION_DONE",
   "step_status":{
      "OCR":{
         "error":"",
         "modified_at":datetime.datetime(2024, 10, 3, 22, 14, 48, tzinfo=TzInfo(UTC)),
         "response":{
            
         },
         "status":"DONE"
      },
      "classification":{
         "error":"",
         "modified_at":datetime.datetime(2024, 10, 3, 22, 15, 33, tzinfo=TzInfo(UTC)),
         "response":{
            
         },
         "status":"DONE"
      },
      "entity_extraction":{
         "error":"",
         "modified_at":datetime.datetime(2024, 10, 3, 22, 16, 47, tzinfo=TzInfo(UTC)),
         "response":{
            
         },
         "status":"DONE"
      },
      "vectorization":{
         "error":"",
         "modified_at":datetime.datetime(2024, 10, 3, 22, 17, 42, tzinfo=TzInfo(UTC)),
         "response":{
            
         },
         "status":"DONE"
      }
   },
   "summary":"The document, identified as 'aapl-20230701', was submitted on..."
}
```

---

### Trigger document summary

Requests the copilot to generate a summary for a document. The response changes based on the state of the summarization workflow. Once the summarization is completed, the script returns the summary.

```bash
python3 documents/documents/trigger_document_summary.py --document_id 66ff1732927ce8c0ebda42bd
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| document_id | The unique identifier of the document | Required |

**Response:**

```bash
{
   "summary_status":"PROCESSING",
   "summary":"",
   "redacted_summary":""
}
```

<aside>
💡

If this script is run again after summarization is completed, you should receive a different response as shown below

</aside>

```bash
{
   "summary_status":"READY",
   "summary":"Apple Inc.'s Q3 2023 ...",
   "redacted_summary":"Apple Inc.'s Q3 2023 ..."
}
```

---

### Get document summary status

Once the [document summary](https://www.notion.so/How-to-use-the-Developer-Scripts-10c5084286c5801bb388fbcc98dc5626?pvs=21) has been triggered, this script helps check the status of summarization for the document.

```bash
python3 documents/documents/get_document_summary_status.py --document_id 66fe1b65b1d0dfb13c9975f042.0
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| document_id | The unique identifier of the document | Required |

**Response:**

If the script returns the following:

```bash
{'message': 'Summarization not triggered'}
```

Follow [document summary](https://www.notion.so/How-to-use-the-Developer-Scripts-10c5084286c5801bb388fbcc98dc5626?pvs=21) script to trigger summarization.

Once the summarization is triggered, 

The same script should return:

```bash
{'redacted_summary': '', 'summary': '', 'summary_status': 'PROCESSING'}
```

Once processing is completed, the script should return

```bash
{
   "summary_status":"READY",
   "summary":"Apple Inc.'s Q3 2023 ...",
   "redacted_summary":"Apple Inc.'s Q3 2023 ..."
}
```