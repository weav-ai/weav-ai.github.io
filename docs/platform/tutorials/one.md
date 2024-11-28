---
layout: libdoc/page
title: Document Management
description: This tutorial walks through key features of the Weav.ai platform, focusing on how to upload, manage, and process documents efficiently. By following this step-by-step guide, users will gain a deeper understanding of performing actions such as uploading files, retrieving their statuses, summarizing content, creating folders, and executing semantic searches. 
---

### **Key Steps Covered**
- **Token Generation**: Obtain a temporary token for secure API access.
- **Uploading Documents**: Learn how to upload a document and observe its status updates.
- **Processing Documents**: Retrieve detailed information about uploaded documents, including summaries and page-specific data.
- **Folder Management**: Organize files into folders for better workflow efficiency.
- **Semantic Search**: Explore how to search across documents and folders using semantic capabilities.
- **Summary Creation**: Generate concise summaries of search results for quick insights.

To obtain a temporary token from your web session, please see below:

![Untitled](/assets/libdoc/img/sampleImages/tutorial1/1.png)

To work with this tutorial, we will take a step by step approach by uploading, deleting and searching information from the uploaded document. We will also take a quick glance at creating folders and uploading documents into folders.

Note:
In the tutorial, the APIs on [`dev2.copilot.weav.ai`](http://dev2.copilot.weav.ai) are being used, however, this can be replaced by the environment you are working with.

On the home screen, you can initially observe there are no documents present. 

![Screenshot 2024-09-09 at 4.25.03 PM.png](/assets/libdoc/img/sampleImages/tutorial1/2.png)

## UPLOAD DOCUMENT

For the purpose of this tutorial, we will be uploading a file named `AAPL_10Q.pdf`

**cURL**

```jsx
curl -X 'POST' 'https://dev2.copilot.weav.ai/file-service/documents/' \
--header 'Content-Type: multipart/form-data' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' \
--form 'file_uploaded=@"AAPL_10Q.pdf"'
```

**Response**

```json
{
  "_id": "66e0f8993c95029325001a78",
  "media_type": "application/pdf",
  "download_url": "/doc-proc-service/local_store/google-oauth2|117349365869611297391/66e0f8993c95029325001a78/66e0f8993c95029325001a78",
  "pages": [],
  "status": "NEW",
  "file_name": "AAPL_10Q.pdf",
  "created_at": "2024-09-11T01:55:37Z",
  "size": 654929,
  "source": "application",
  "step_status": {
    "FORM_EXTRACTION": {
      "status": "NOT_STARTED",
      "modified_at": "2024-09-11T01:55:37Z"
    }
  },
  "in_folders": [],
  "tags": [],
  "ai_tags": [],
  "user_id": "google-oauth2|117349365869611297391",
  "tenant_id": ""
}
```

![New Document Uploaded.jpg](/assets/libdoc/img/sampleImages/tutorial1/3.jpg)

Every time a new document is uploaded, a new ID for that document is generated.

### GET DOCUMENT STATUS

To find out at what stage the documents processing is, we can hit the cURL command below.

**cURL**

```bash
curl -X 'GET' 'https://dev2.copilot.weav.ai/file-service/documents/66e0f8993c95029325001a78/step' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'
```

**Response**

```json
{
   "step_status":{
      "FORM_EXTRACTION":{
         "status":"NOT_STARTED",
         "modified_at":"2024-09-09T21:26:29Z"
      }
   }
}
```

### GET DOCUMENT

To retrieve the uploaded document, we can use the document ID in the cURL command. We can retrieve the document ID from the `UPLOAD DOCUMENT` API response using the `_id` field or simply go on the UI, click the document and retrieve it from the URL.

![Screenshot 2024-09-10 at 6.56.18 PM.png](/assets/libdoc/img/sampleImages/tutorial1/4.png)

**cURL**

```json
curl -X 'GET' 'https://dev2.copilot.weav.ai/file-service/documents/66e0f8993c95029325001a78' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' 

```

**Response**

```json
{
   "_id":"66e0f8993c95029325001a78",
   "media_type":"application/pdf",
   "download_url":"/doc-proc-service/local_store/google-oauth2|117349365869611297391/66e0f8993c95029325001a78/66df67a3a2adcf1fe2e3b900",
   "pages":[
      {
         "page_number":0,
         "media_type":"image/jpeg",
         "download_url":"/doc-proc-service/local_store/google-oauth2|117349365869611297391/66e0f8993c95029325001a78/0.jpg",
         "page_text":"aapl-20230701\n9/6/23, 9:57 AM\nUNITED STATES SECURITIES AND EXCHANGE COMMISSION Washington, D.C. 20549\nFORM 10-Q\n(Mark One) :selected: X QUARTERLY REPORT PURSUANT TO SECTION 13 OR 15(d) OF THE SECURITIES EXCHANGE ACT OF 1934 For the quarterly period ended July 1, 2023 or :unselected:\nTRANSITION REPORT PURSUANT TO SECTION 13 OR 15(d) OF THE SECURITIES EXCHANGE ACT OF 1934 For the transition period from to\nCommission File Number: 001-36743\nApple Inc.\n(Exact name of Registrant as specified in its charter)\nCalifornia (State or other jurisdiction of incorporation or organization)\n94-2404110 (I.R.S. Employer Identification No.)\nOne Apple Park Way\nCupertino, California (Address of principal executive offices) (408) 996-1010 (Registrant's telephone number, including area code)\n95014 (Zip Code)\nSecurities registered pursuant to Section 12(b) of the Act:\nTitle of each class\nTrading symbol(s)\nCommon Stock, $0.00001 par value per share\nAAPL\n1.375% Notes due 2024\n0.000% Notes due 2025\n0.875% Notes due 2025\n1.625% Notes due 2026\n2.000% Notes due 2027\n1.375% Notes due 2029\n3.050% Notes due 2029\n0.500% Notes due 2031\n3.600% Notes due 2042\nName of each exchange on which registered\nThe Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC The Nasdaq Stock Market LLC\nIndicate by check mark whether the Registrant (1) has filed all reports required to be filed by Section 13 or 15(d) of the Securities Exchange Act of 1934 during the preceding 12 months (or for such shorter period that the Registrant was required to file such reports), and (2) has been subject to such filing requirements for the past 90 days.\nYes :unselected: :selected: No\nhttps://www.sec.gov/Archives/edgar/data/320193/000032019323000077/aapl-20230701.htm\n1/31 :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected: :unselected:",
         "status":"VECTORIZATION_DONE",
         "step_status":{
            "classification":{
               "status":"DONE",
               "modified_at":"2024-09-09T21:25:12Z"
            },
            "entity_extraction":{
               "status":"DONE",
               "modified_at":"2024-09-09T21:25:49Z"
            },
            "vectorization":{
               "status":"DONE",
               "modified_at":"2024-09-09T21:26:12Z"
            },
            "OCR":{
               "status":"DONE",
               "modified_at":"2024-09-09T21:25:07Z"
            }
         },
         "classification":{
            "page_class":"UNITED STATES SECURITIES AND EXCHANGE COMMISSION",
            "page_sections":[
               "FORM 10-Q",
               "Apple Inc.",
               "Securities registered pursuant to Section 12(b) of the Act:",
               "Indicate by check mark whether the Registrant"
            ],
            "page_no":1
         }
      },
      {...},
      {...},
      .
      .
      .
      .
      {...}
   ],
   "status":"AI_READY",
   "file_name":"AAPL_10Q.pdf",
   "created_at":"2024-09-09T21:24:51Z",
   "size":654929,
   "source":"application",
   "category":"UNITED STATES SECURITIES AND EXCHANGE COMMISSION",
   "step_status":{
      "FORM_EXTRACTION":{
         "status":"NOT_STARTED",
         "modified_at":"2024-09-09T21:26:29Z"
      }
   },
   "in_folders":[
      
   ],
   "tags":[
      
   ],
   "ai_tags":[
      
   ],
   "user_id":"google-oauth2|117349365869611297391",
   "tenant_id":""
}
```

### SUMMARIZE DOCUMENT

To create a summary once the document is uploaded, we can use the cURL below

**cURL**

```json
curl -X 'POST' 'https://dev2.copilot.weav.ai/file-service/documents/66e0f8993c95029325001a78/summary' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' 
```

Note: The summary is only available once the processing of the document is completed. If the document is still processing, you will receive the response shown below:

**Response**

```json
{
   "summary_status":"PROCESSING",
   "summary":"",
   "redacted_summary":""
}
```

Once, the document processing is completed, hitting the same document would response with the summary, shown below:

**Response**

```json
{
   "summary_status":"READY",
   "summary":"Apple Inc.'s Q3 2023 report shows a slight decrease in net sales primarily due to lower sales of iPad and iPhone, offset by higher sales of Services. The company announced new products including the 15-inch MacBook Air, Mac Studio, Mac Pro, and Apple Vision Pro. Apple repurchased 365 million shares for $56.1 billion and paid dividends of $3.8 billion. The company is involved in legal proceedings with Epic Games, with the court ruling in favor of Apple on nine out of ten counts. However, certain provisions of Apple's App Store Review Guidelines were found to violate California's unfair competition law. The company also conducted share repurchase activity from April 2, 2023 to July 1, 2023, purchasing a total of 102,673 shares at an average price of $94,569 per share.",
   "redacted_summary":"Apple Inc.'s Q3 2023 report shows a slight decrease in net sales primarily due to lower sales of iPad and iPhone, offset by higher sales of Services. The company announced new products including the 15-inch MacBook Air, Mac Studio, Mac Pro, and Apple Vision Pro. Apple repurchased 365 million shares for $56.1 billion and paid dividends of $3.8 billion. The company is involved in legal proceedings with Epic Games, with the court ruling in favor of Apple on nine out of ten counts. However, certain provisions of Apple's App Store Review Guidelines were found to violate California's unfair competition law. The company also conducted share repurchase activity from April 2, 2023 to July 1, 2023, purchasing a total of 102,673 shares at an average price of $94,569 per share."
}
```

### GET SUMMARY

**cURL**

```json
curl -X 'GET' 'https://dev2.copilot.weav.ai/file-service/documents/66e0f8993c95029325001a78/summary' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'
```

**Response**

```json
{
   "summary_status":"READY",
   "summary":"Apple Inc.'s Q3 2023 report shows a slight decrease in net sales primarily due to lower sales of iPad and iPhone, offset by higher sales of Services. The company announced new products including the 15-inch MacBook Air, Mac Studio, Mac Pro, and Apple Vision Pro. Apple repurchased 365 million shares for $56.1 billion and paid dividends of $3.8 billion. The company is involved in legal proceedings with Epic Games, with the court ruling in favor of Apple on nine out of ten counts. However, certain provisions of Apple's App Store Review Guidelines were found to violate California's unfair competition law. The company also conducted share repurchase activity from April 2, 2023 to July 1, 2023, purchasing a total of 102,673 shares at an average price of $94,569 per share.",
   "redacted_summary":"Apple Inc.'s Q3 2023 report shows a slight decrease in net sales primarily due to lower sales of iPad and iPhone, offset by higher sales of Services. The company announced new products including the 15-inch MacBook Air, Mac Studio, Mac Pro, and Apple Vision Pro. Apple repurchased 365 million shares for $56.1 billion and paid dividends of $3.8 billion. The company is involved in legal proceedings with Epic Games, with the court ruling in favor of Apple on nine out of ten counts. However, certain provisions of Apple's App Store Review Guidelines were found to violate California's unfair competition law. The company also conducted share repurchase activity from April 2, 2023 to July 1, 2023, purchasing a total of 102,673 shares at an average price of $94,569 per share."
}
```

### GET PAGE

You can also fetch the information of a document by it’s page number. Shown below is an example of retrieving information from Page 1

**cURL**

```json
curl -X 'GET' 'https://dev2.copilot.weav.ai/file-service/documents/66e0f8993c95029325001a78/pages/1?bounding_boxes=false' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'
```

**Response**

```json
{
   "page_number":1,
   "media_type":"image/jpeg",
   "download_url":"/doc-proc-service/local_store/google-oauth2|117349365869611297391/66e0f8993c95029325001a78/1.jpg",
   "page_text":"9/6/23, 9:57 AM\naapl-20230701\nIndicate by check mark whether the Registrant has submitted electronically every Interactive Data File required to be submitted pursuant to Rule 405 of Regulation S-T (§232.405 of this chapter) during the preceding 12 months (or for such shorter period that the Registrant was required to submit such files).\nYes :selected: :unselected: No\nIndicate by check mark whether the Registrant is a large accelerated filer, an accelerated filer, a non-accelerated filer, a smaller reporting company, or an emerging growth company. See the definitions of \"large accelerated filer,\" \"accelerated filer,\" \"smaller reporting company,\" and \"emerging growth company\" in Rule 12b-2 of the Exchange Act.\nLarge accelerated filer :selected:\nAccelerated filer :unselected:\nNon-accelerated filer :unselected:\nSmaller reporting company :unselected:\nEmerging growth company :unselected:\nIf an emerging growth company, indicate by check mark if the Registrant has elected not to use the extended transition period for complying with any new or revised financial accounting standards provided pursuant to Section 13(a) of the Exchange Act. :unselected:\nIndicate by check mark whether the Registrant is a shell company (as defined in Rule 12b-2 of the Exchange Act). :unselected: Yes :selected: No\n15,634,232,000 shares of common stock were issued and outstanding as of July 21, 2023.\nhttps://www.sec.gov/Archives/edgar/data/320193/000032019323000077/aapl-20230701.htm\n2/31",
   "status":"VECTORIZATION_DONE",
   "step_status":{
      "classification":{
         "status":"DONE",
         "modified_at":"2024-09-09T21:25:14Z"
      },
      "entity_extraction":{
         "status":"DONE",
         "modified_at":"2024-09-09T21:25:34Z"
      },
      "vectorization":{
         "status":"DONE",
         "modified_at":"2024-09-09T21:25:40Z"
      },
      "OCR":{
         "status":"DONE",
         "modified_at":"2024-09-09T21:25:08Z"
      }
   },
   "classification":{
      "page_class":"Company Filing Information",
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
               "label":"Document Timestamp",
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
            {
               "polygon":[
                  
               ],
               "key":"Interactive Data File Submission",
               "value":"Yes",
               "label":"Interactive Data File Submission Status",
               "is_sensitive":false
            },
            {
               "polygon":[
                  
               ],
               "key":"Registrant Type",
               "value":"Large accelerated filer",
               "label":"Type of Registrant",
               "is_sensitive":false
            },
            {
               "polygon":[
                  
               ],
               "key":"Shell Company Status",
               "value":"No",
               "label":"Shell Company Status",
               "is_sensitive":false
            },
            {
               "polygon":[
                  
               ],
               "key":"Shares Issued",
               "value":"15,634,232,000",
               "label":"Number of Shares Issued",
               "is_sensitive":false
            },
            {
               "polygon":[
                  
               ],
               "key":"Date of Shares Issued",
               "value":"July 21, 2023",
               "label":"Date of Shares Issued",
               "is_sensitive":false
            },
            {
               "polygon":[
                  
               ],
               "key":"Source",
               "value":"https://www.sec.gov/Archives/edgar/data/320193/000032019323000077/aapl-20230701.htm",
               "label":"Source URL",
               "is_sensitive":false
            }
         ]
      }
   ],
   "sensitive_words":[
      
   ],
   "summary":"The document with ID 'aapl-20230701' was submitted on 9/6/23 at 9:57 AM. The registrant has submitted all required Interactive Data Files electronically and is identified as a large accelerated filer. The registrant is not a shell company. As of July 21, 2023, there were 15,634,232,000 shares of common stock issued and outstanding. The document can be accessed at the provided SEC Archives URL.",
   "redacted_summary":"The document with ID 'aapl-20230701' was submitted on 9/6/23 at 9:57 AM. The registrant has submitted all required Interactive Data Files electronically and is identified as a large accelerated filer. The registrant is not a shell company. As of July 21, 2023, there were 15,634,232,000 shares of common stock issued and outstanding. The document can be accessed at the provided SEC Archives URL."
}
```

### GET WORDS FROM PARTICULAR PAGE IN DOCUMENT

**cURL**

```bash
curl --location 'https://dev2.copilot.weav.ai/file-service/documents/66e0f8993c95029325001a78/pages/1/words' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'

```

**Response**

```bash
{
  "page_number": 1,
  "media_type": "NONE",
  "page_text": "9/6/23, 9:57 AM\naapl-20230701\nIndicate by check mark whether the Registrant has submitted electronically every Interactive Data File required to be submitted pursuant to Rule 405 of Regulation S-T (§232.405 of this chapter) during the preceding 12 months (or for such shorter period that the Registrant was required to submit such files).\nYes :selected: :unselected: No\nIndicate by check mark whether the Registrant is a large accelerated filer, an accelerated filer, a non-accelerated filer, a smaller reporting company, or an emerging growth company. See the definitions of \"large accelerated filer,\" \"accelerated filer,\" \"smaller reporting company,\" and \"emerging growth company\" in Rule 12b-2 of the Exchange Act.\nLarge accelerated filer :selected:\nAccelerated filer :unselected:\nNon-accelerated filer :unselected:\nSmaller reporting company :unselected:\nEmerging growth company :unselected:\nIf an emerging growth company, indicate by check mark if the Registrant has elected not to use the extended transition period for complying with any new or revised financial accounting standards provided pursuant to Section 13(a) of the Exchange Act. :unselected:\nIndicate by check mark whether the Registrant is a shell company (as defined in Rule 12b-2 of the Exchange Act). :unselected: Yes :selected: No\n15,634,232,000 shares of common stock were issued and outstanding as of July 21, 2023.\nhttps://www.sec.gov/Archives/edgar/data/320193/000032019323000077/aapl-20230701.htm\n2/31",
  "status": "NONE",
  "classification": {
    "page_class": "Company Filing Information",
    "page_sections": [
      "Interactive Data File Submission",
      "Company Classification",
      "Emerging Growth Company Status",
      "Shell Company Status",
      "Common Stock Information"
    ],
    "page_no": 2
  },
  "extracted_entities": [
    {
      "entity_group": "default",
      "entities": [
        {
          "polygon": [],
          "key": "Date",
          "value": "9/6/23, 9:57 AM",
          "label": "Document Date",
          "is_sensitive": false
        },
        {
          "polygon": [],
          "key": "Document ID",
          "value": "aapl-20230701",
          "label": "Document Identifier",
          "is_sensitive": false
        },
		    {...},
		    {...},
		    .
		    .
			  {...}
      ]
    }
  ],
  "redacted_summary": "",
  "words": [
    {
      "content": "9/6/23,",
      "polygon": [
        {
          "x": 71,
          "y": 42
        },
        {
          "x": 135,
          "y": 43
        },
        {
          "x": 136,
          "y": 66
        },
        {
          "x": 71,
          "y": 66
        }
      ],
      "span": {
        "offset": 0,
        "length": 7
      },
      "confidence": 0.994
    },
    {...},
    {...},
    .
    .
	  {...}
      ],
      "span": {
        "offset": 1474,
        "length": 4
      },
      "confidence": 0.993
    }
  ]
}
```

### DELETE DOCUMENT

**cURL**

```json
curl -X 'DELETE' 'https://dev2.copilot.weav.ai/file-service/documents/66e0f8993c95029325001a78' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'
```

**Response**

```json
{
   "_id":"66e0f8993c95029325001a78",
   "media_type":"application/pdf",
   "download_url":"/doc-proc-service/local_store/google-oauth2|117349365869611297391/66e0f8993c95029325001a78/66e0f8993c95029325001a78",
   "pages":[
      
   ],
   "status":"PROCESSING",
   "file_name":"AAPL_10Q.pdf",
   "created_at":"2024-09-09T21:39:04Z",
   "size":654929,
   "source":"application",
   "step_status":{
      "FORM_EXTRACTION":{
         "status":"NOT_STARTED",
         "modified_at":"2024-09-09T21:39:04Z"
      }
   },
   "in_folders":[
      
   ],
   "tags":[
      
   ],
   "ai_tags":[
      
   ],
   "user_id":"google-oauth2|117349365869611297391",
   "tenant_id":""
}
```

![Screenshot 2024-09-09 at 4.25.03 PM.png](/assets/libdoc/img/sampleImages/tutorial1/5.png)

Once, the document is deleted, if the API is called again, you would receive the following response

```json
{"message":"Could not find the identifier"}
```

### CREATE FOLDER

Let’s create a folder named ‘Test Folder’

**cURL**

```json
curl --location 'https://dev2.copilot.weav.ai/file-service/folders/' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' \
--data '{
  "name": "Test Folder"
}'
```

**Response**

```json
{
   "_id":"66e0f93093798ee1c937e39a",
   "documents":null,
   "document_ids":[
      
   ],
   "shared_with_users":{
      
   },
   "shared_with_groups":{
      
   },
   "name":"Test Folder",
   "category":"",
   "description":"",
   "created_at":"2024-09-09T21:47:33Z",
   "modified_at":"2024-09-09T21:47:33.442789Z",
   "workflow":{
      "workflow_id":"process_document",
      "form_id":"",
      "workflow_params":{
         
      }
   },
   "user_id":"google-oauth2|117349365869611297391",
   "tenant_id":""
}
```

You should see that a folder named `Test Folder` has been created

![New folder created.jpg](/assets/libdoc/img/sampleImages/tutorial1/6.jpg)

However, once we click on the folder, we observe that the folder does not contain any document.

![No file.jpg](/assets/libdoc/img/sampleImages/tutorial1/7.png)

![Screenshot 2024-09-10 at 7.09.59 PM.png](/assets/libdoc/img/sampleImages/tutorial1/8.png)

Similar to before, the `folder_id` can be obtained from the URL when we click on the folder as seen above.

Next, let’s upload a document in the required folder

### UPLOAD DOCUMENT IN FOLDER

This is similar to uploading a new document, however, observe that a new parameter `folder_id` is added to the request

**cURL**

```json
curl -X 'POST' '[https://dev2.copilot.weav.ai/file-service/documents/](http://localhost:7015/documents/)' \
--header 'Content-Type: multipart/form-data' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' \
--form 'file_uploaded=@"AAPL_10Q.pdf"' \
--form 'folder_id="66e0f93093798ee1c937e39a"'
```

**Response**

```json
{
  "_id": "66e0fba3089fbd21c4dd80c3",
  "media_type": "application/pdf",
  "download_url": "/doc-proc-service/local_store/google-oauth2|117349365869611297391/66e0fba3089fbd21c4dd80c3/66e0fba3089fbd21c4dd80c3",
  "pages": [],
  "status": "NEW",
  "file_name": "AAPL_10Q.pdf",
  "created_at": "2024-09-11T02:08:35Z",
  "size": 654929,
  "source": "application",
  "step_status": {
    "FORM_EXTRACTION": {
      "status": "NOT_STARTED",
      "modified_at": "2024-09-11T02:08:35Z"
    }
  },
  "in_folders": [],
  "tags": [],
  "ai_tags": [],
  "user_id": "google-oauth2|117349365869611297391",
  "tenant_id": ""
}
```

You should see folder now contains an icon on the top right which indicates a document is present inside it.

![Screenshot 2024-09-09 at 3.04.42 PM.png](/assets/libdoc/img/sampleImages/tutorial1/9.png)

Upon clicking the folder we observe that the document is now inside the folder

![Screenshot 2024-09-09 at 4.42.47 PM.png](/assets/libdoc/img/sampleImages/tutorial1/10.png)

We can retrieve the folder ID and file ID from the URL as before or from the `_id` fields of the respective responses

![Screenshot 2024-09-10 at 7.11.00 PM.png](/assets/libdoc/img/sampleImages/tutorial1/11.png)

### Get Documents in a Folder

**cURL**

```bash
curl --location 'https://dev2.copilot.weav.ai/file-service/documents/?query=&folder_id=66e0f93093798ee1c937e39a&only_non_nested=true&skip=0&limit=100&all=false' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'
```

**Response**

```bash
{
  "docs": [
    {
      "_id": "66e0fba3089fbd21c4dd80c3",
      "media_type": "application/pdf",
      "download_url": "/doc-proc-service/local_store/google-oauth2|117349365869611297391/66e0fba3089fbd21c4dd80c3/66e0fba3089fbd21c4dd80c3",
      "status": "AI_READY",
      "file_name": "AAPL_10Q.pdf",
      "created_at": "2024-09-09T23:42:36Z",
      "size": 654929,
      "source": "application",
      "category": "SECURITIES AND EXCHANGE COMMISSION",
      "in_folders": [
        "66e0fba3089fbd21c4dd80c3"
      ],
      "tags": [],
      "ai_tags": [],
      "user_id": "google-oauth2|117349365869611297391",
      "tenant_id": ""
    }
  ],
  "total": 1
}
```

### GET DOCUMENT CATEGORIES

**cURL**

```bash
curl --location 'https://dev2.copilot.weav.ai/file-service/documents/categories/' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'

```

**Response**

```bash
{"categories":["SECURITIES AND EXCHANGE COMMISSION"]}
```

### GET ALL DOCUMENT IDs FROM A FOLDER

**cURL**

```bash
curl --location 'https://dev2.copilot.weav.ai/file-service/folders/files/?query=&folder_id=66e0f93093798ee1c937e39a&only_non_nested=true' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>'

```

**Response**

```bash
{"document_ids":["66e0fba3089fbd21c4dd80c3"]}
```

### SEMANTIC SEARCH

Searching a document inside a folder

**cURL**

```json
curl -X 'POST' 'https://dev2.copilot.weav.ai/search-service/search' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' \
--data '{
  "search_query": "What does the Apple contract state?",
  "search_config": {
    "scope": "all_documents",
    "types": [
      "semantic",
      "internet"
    ]
  },
  "file_ids": [
    "66e0fba3089fbd21c4dd80c3"
  ],
  "folder_ids": [
    "66e0f93093798ee1c937e39a"
  ]
}'
```

**Response**

```json
{
   "type":"data",
   "results":[
      {
         "text":"Company(Company Name): Apple Inc.",
         "file_id":"66e0fba3089fbd21c4dd80c3",
         "folder_id":"66e0f93093798ee1c937e39a",
         "page_nr":2,
         "score":0.85080373,
         "rank":1,
         "file_name":"AAPL_10Q.pdf",
         "classification":{
            "page_class":"Apple Inc. Form 10-Q For the Fiscal Quarter Ended July 1, 2023",
            "page_sections":[
               "Financial Statements",
               "Management's Discussion and Analysis of Financial Condition and Results of Operations",
               "Quantitative and Qualitative Disclosures About Market Risk",
               "Controls and Procedures",
               "Legal Proceedings",
               "Risk Factors",
               "Unregistered Sales of Equity Securities, Use of Proceeds, and Issuer Purchases of Equity Securities",
               "Defaults Upon Senior Securities",
               "Mine Safety Disclosures",
               "Other Information",
               "Exhibits"
            ],
            "page_no":31
         },
         "result_type":"semantic",
         "section_headers":[
            
         ],
         "tags":null
      },
	    {...},
	    .
	    .
	    .
	    {...}
   ],
   "text":{
      "message_code":"success"
   }
}
```

**Searching only by document**

**cURL**

```json
curl -X 'POST' 'https://dev2.copilot.weav.ai/search-service/search' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' \
--data '{
  "search_query": "What does the Apple contract state?",
  "search_config": {
    "scope": "all_documents",
    "types": [
      "semantic",
      "internet"
    ]
  },
  "file_ids": [
    "66e0fba3089fbd21c4dd80c3"
  ]
}'
```

**Searching in all folders**

**cURL**

```json
curl -X 'POST' 'https://dev2.copilot.weav.ai/search-service/search' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' \
--data '{
  "search_query": "What does the Apple contract state?",
  "search_config": {
    "scope": "all_documents",
    "types": [
      "semantic",
      "internet"
    ]
  },
"folder_ids": [
    "66e0f93093798ee1c937e39a"
  ]
}'
```

### GET SEARCH SUMMARY

To get the summary of the search results, the data from `results` field and the original `search_query` is passed as data to the search summary API

**cURL**

```bash
curl -X 'POST' 'https://dev2.copilot.weav.ai/search-service/summarize_search' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <your token>' \
--data '{
   "search_query":"Summarize this data?",
   "results":[
      {
         "text":"Company(Company Name): Apple Inc.",
         "file_id":"66e0fba3089fbd21c4dd80c3",
         "folder_id":"66e0fba3089fbd21c4dd80c3",
         "page_nr":2,
         "score":0.85080373,
         "rank":1,
         "file_name":"AAPL_10Q.pdf",
         "classification":{
            "page_class":"Apple Inc. Form 10-Q For the Fiscal Quarter Ended July 1, 2023",
            "page_sections":[
               "Financial Statements",
               "Managements Discussion and Analysis of Financial Condition and Results of Operations",
               "Quantitative and Qualitative Disclosures About Market Risk",
               "Controls and Procedures",
               "Legal Proceedings",
               "Risk Factors",
               "Unregistered Sales of Equity Securities, Use of Proceeds, and Issuer Purchases of Equity Securities",
               "Defaults Upon Senior Securities",
               "Mine Safety Disclosures",
               "Other Information",
               "Exhibits"
            ],
            "page_no":31
         },
         "result_type":"semantic",
         "section_headers":[
            
         ],
         "tags":null
      }
   ],
   "response_format":"concise",
   "stream":false
}'
```

**Response**

```bash
{
  "summary": "The document \"AAPL_10Q.pdf\" is related to the company Apple Inc.[1]"
}
```