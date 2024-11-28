---
layout: libdoc/page
title: Agents
description: Learn how to interact with and manage AI agents on the Weav.ai platform.
---
## **Overview**
The Agents section provides tools to interact with AI agents on the Weav.ai platform. You can fetch agent types, query agents for responses, manage chat histories, and delete conversations. These functionalities enable seamless integration of AI-powered agents into your workflows.

**Prerequisite** - To get started, ensure your environment is properly configured by following the [Setup Guide](setup).

-----

### Get Agent Types

Fetches all the agents present on the platform.

```bash
python3 agents/get_agent_types.py
```

**Response:**

```python
[
    {
        "id": "6718b5d4843ed7e25cd695c7",
        "name": "Default weav agent ",
        "verbose": False,
        "tone": "professional",
        "background": "",
        "reply_format": "    * If your response contains data across categories or time periods or organizing multiple statistics, format them as a table in markdown format.\n    * If your response has a list of items, please format your response as bulleted list in markdown format.\n    * If your response contains instructions or steps, please format your response as numbered list in markdown format.",
        "max_iterations": 5,
        "llm_model_name": "azure-gpt4-turbo",
        "intents": {
            "event_message": "Detecting intents...",
            "intents": [
                {
                    "index": 0,
                    "name": "knowledge_or_information_search",
                    "description": "Search for documents",
                    "event_message": "Search documents...",
                    "allowed_actions": [
                        "google",
                        "semantic",
                        "form",
                        "web_browse",
                        "plotly_chart",
                    ],
                }
            ],
        },
        "actions": [
            {
                "name": "Google Search",
                "description": "Initiates a Google search on the domain to obtain pertinent information or web links within the domain defined in the action config.",
                "event_message": "Performing Google Search operation",
                "input_schema": '{"properties": {"query": {"description": "Google search query.", "title": "Query", "type": "string"}}, "required": ["query"], "title": "GoogleSearchInput", "type": "object"}',
                "identifier": "google",
                "llm_model_name": None,
                "type": "google_search",
                "k": 5,
                "domain": "",
                "exclude_sites": [],
                "enable_context_relevancy_filtering": True,
            },
            
        ],
        "publish_results_configuration": {
            "publish_only_if_asked": False,
            "publish_action": {
                "name": "Publish result document",
                "description": "Creates a pdf from markdown text and pushes it to weav copilot",
                "event_message": "Performing publish to Weav operation",
                "input_schema": '{"properties": {"doc_contents": {"description": "Dictionary with keys: page_title, page_text (in markdown) and (optional) image_file_path", "title": "Doc Contents", "type": "object"}}, "required": ["doc_contents"], "title": "CreateDocInput", "type": "object"}',
                "identifier": "publish",
                "llm_model_name": None,
                "type": "create_weav_document",
                "folder_name_keywords": [],
                "folder_id": "",
            },
        },
    },
   
]

```

### Get Agent Response

Fetches the response of a selected agent from the platform based on user input.

```bash
python3 agents/get_agent_response.py --user_input "Summarize the document" --chat_id "google-oauth2|117349365869611297391_Insurance Underwriting AI Agent" --agent_id "Insurance Underwriting AI Agent"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| user_input | The question provided to the agent | Required |
| agent_id | The ID of the agent required | Required |
| chat_id | Chat ID in which the conversation takes place | Required |

**Response:**

```bash
[GetAgentResponse(id=None, event=None, data='{"type": "assistant", "chat_id": "google-oauth2|117349365869611297391_Insurance Underwriting AI Agent", "vote": "no vote", "message_id": "d79c34f5-f4f3-4170-8baa-8baf2efea2a4", "search_results": [], "generate_button": null, "tags": [], "text": "**Analyzing request...<br><br>** ", "timestamp": "2024-09-25 19:12:20.178930+00:00"}', retry=None),...]
```

### Get Agent

Fetches information about a particular agent

```python
python3 agents/get_agent.py --agent_id "6718b5d4843ed7e25cd695c7"
```

---

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| agent_id | The ID of the agent required | Required |

**Response:**

```python
{
    "id": "6718b5d4843ed7e25cd695c7",
    "name": "Default weav agent ",
    "verbose": False,
    "tone": "professional",
    "background": "",
    "custom_instructions": "",
    "reply_format": "    * If your response contains data across categories or time periods or organizing multiple statistics, format them as a table in markdown format.\n    * If your response has a list of items, please format your response as bulleted list in markdown format.\n    * If your response contains instructions or steps, please format your response as numbered list in markdown format.",
    "max_iterations": 5,
    "llm_model_name": "azure-gpt4-turbo",
    "intents": {
        "event_message": "Detecting intents...",
        "intents": [
            {
                "index": 0,
                "name": "knowledge_or_information_search",
                "description": "Search for documents",
                "event_message": "Search documents...",
                "allowed_actions": [
                    "google",
                    "semantic",
                    "form",
                    "web_browse",
                    "plotly_chart",
                ],
            }
        ],
    },
    "actions": [
        {
            "name": "Google Search",
            "description": "Initiates a Google search on the domain to obtain pertinent information or web links within the domain defined in the action config.",
            "event_message": "Performing Google Search operation",
            "input_schema": '{"properties": {"query": {"description": "Google search query.", "title": "Query", "type": "string"}}, "required": ["query"], "title": "GoogleSearchInput", "type": "object"}',
            "identifier": "google",
            "llm_model_name": None,
            "type": "google_search",
            "folder_name_keywords": [],
            "folder_ids": [],
            "enable_context_relevancy_filtering": True,
            "k": 5,
            "domain": "",
            "exclude_sites": [],
            "documents": {},
            "tmp_image_path": None,
            "url": None,
            "rest_verb": None,
            "max_char": None,
            "folder_id": None,
        },
    ],
    "publish_results_configuration": {
        "publish_only_if_asked": False,
        "publish_action": {
            "name": "Publish result document",
            "description": "Creates a pdf from markdown text and pushes it to weav copilot",
            "event_message": "Performing publish to Weav operation",
            "input_schema": '{"properties": {"doc_contents": {"description": "Dictionary with keys: page_title, page_text (in markdown) and (optional) image_file_path", "title": "Doc Contents", "type": "object"}}, "required": ["doc_contents"], "title": "CreateDocInput", "type": "object"}',
            "identifier": "publish",
            "llm_model_name": None,
            "type": "create_weav_document",
            "folder_name_keywords": [],
            "folder_id": "",
        },
    },
}

```