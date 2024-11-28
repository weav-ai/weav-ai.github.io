---
layout: libdoc/page
title: weavaidev library documentation
description: This documentation provides a comprehensive guide to the Weavaidev Python library, tailored to enhance interactions with the Weav.ai platform. This library delivers powerful APIs and streamlined workflows, enabling seamless integration, automation, and customization for your applications.
---
The best part? It's really simple!

```python
from weavaidev import Config
from weavaidev.documents import DocumentOperations

config = Config(auth_token="my_auth_token",env="my_domain")
doc_ops = DocumentOperations(config=config)
response = doc_ops.get_document(document_id="my_document_id", fill_pages=True)
```


## **Getting started** 
Kickstart your integration with the Weavaidev Python library by setting up your environment and installing the library. Follow the steps in our [Installation guide](setup).

## **Key Features**
- [**Form Operations**](form_operations): Create, retrieve, and manage forms with advanced analytics and data export options.
- [**Folder Operations**](folder_operations): Organize resources efficiently with folder creation, retrieval, and metadata management.
- [**Document Operations**](document_operations): Upload, analyze, and retrieve document data with hierarchical and summary-based insights.
- [**Chat Operations**](chat_operations): Manage and interact with chat logs, histories, and sessions in real-time.
- [**Agent Operations**](agent_operations): Engage with configurable agents for task automation and response generation.
- [**Action Operations**](action_operations): Retrieve and utilize action types for workflow automation.


