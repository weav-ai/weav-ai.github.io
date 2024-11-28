---
layout: libdoc/page
title: Workflows
description:  Learn how to manage, run, and monitor workflows on the Weav.ai platform. This guide provides insights into creating, executing, and troubleshooting workflows for document processing.
---
## **Overview**
The Workflows section allows users to automate document processing tasks on the Weav.ai platform. Workflows define a sequence of steps to extract, process, and analyze document data. This guide details operations such as retrieving workflow lists, running workflows, monitoring their statuses, and re-running or skipping steps for enhanced flexibility.

**Prerequisite** - To get started, ensure your environment is properly configured by following the [Setup Guide](setup).

-----
### Get all workflows

Get a list of all workflows that are present on the [Weav.ai](http://Weav.ai) platform.

```bash
 python3 workflows/get_all_workflows.py --show_internal_steps false
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| show_internal_steps | Set to true to show detailed internal steps of the workflowx | Optional (default : False) | false, f, False, true, t, True |

**Response:**

```bash
{
   "workflows":[
      {
         "name":"dagtasktest",
         "params":[
            
         ],
         "tasks":[
            {
               "downstream_tasks":[
                  
               ],
               "is_active":true,
               "name":"only_if_failed"
            },
            {
               "downstream_tasks":[
                  
               ],
               "is_active":true,
               "name":"only_if_success"
            }
         ]
      }
   ]
}
```

---

### Get Single workflow

Get all the steps that are present inside a particular workflow. 

```bash
python3 workflows/get_single_workflow.py --workflow_name dagtasktest --show_internal_steps false
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| show_internal_steps | Set to true to show detailed internal steps | Optional (default : False) | false, f, False, true, t, True |
| workflow_name | The name of the workflow to be fetched | Required | String |

**Response:**

```bash
{
   "name":"dagtasktest",
   "params":[
      
   ],
   "tasks":[
      {
         "downstream_tasks":[
            
         ],
         "is_active":true,
         "name":"only_if_failed"
      },
      {
         "downstream_tasks":[
            
         ],
         "is_active":true,
         "name":"only_if_success"
      },
      {
         "downstream_tasks":[
            "second"
         ],
         "is_active":true,
         "name":"first"
      },
      {
         "downstream_tasks":[
            "third"
         ],
         "is_active":true,
         "name":"second"
      },
      {
         "downstream_tasks":[
            "only_if_success",
            "only_if_failed"
         ],
         "is_active":true,
         "name":"third"
      },
      {
         "downstream_tasks":[
            "second",
            "third",
            "first"
         ],
         "is_active":true,
         "name":"split"
      }
   ]
}
```

---

### Run Workflow

Run a particular workflow for a document. The name of the workflow can be obtained from `name` field in the `Get all workflows` API response demonstrated above.

```bash
python3 workflows/run_workflow.py --doc_id 66e0fba3089fbd21c4dd80c3 --workflow_name dagtest --data "{\"form_id\":\"66fe5c58b1d0dfb13c9975f3\"}"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| workflow_name | Name of the workflow to be run | Required |  |
| doc_id | Document for which the workflow has to be run | Required |  |
| data | Any extra parameters that are required by the worflow | Optional | Stringified JSON |

**Response:**

```bash
{
   "created_at":"2024-09-25T19:33:32.000000+00:00",
   "document_id":"66e0fba3089fbd21c4dd80c3",
   "document_name":"AAPL_10Q.pdf",
   "end_date":"None",
   "in_folders":[
      "66e0f93093798ee1c937e39a"
   ],
   "run_id":"66e0fba3089fbd21c4dd80c3_3df1b127-9ea5-4714-9bf5-b1a5653859f6",
   "start_date":"None",
   "state":"None",
   "workflow_id":"dagtest"
}
```

---

### Get Workflow status

Upon running a workflow, check the status of the run.

```bash
python3 workflows/get_workflow_status.py --workflow_id "dagtasktest" --workflow_run_id "66e0fba3089fbd21c4dd80c3_3df1b127-9ea5-4714-9bf5-b1a5653859f6"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| show_internal_steps | Set to true to show detailed internal steps | Optional (default : False) | false, f, False, true, t, True |
| workflow_id | Document identifier for which the workflow has to be run | Required |  |
| workflow_run_id | The run ID of the workflow | Required |  |

<aside>
💡

The `workflow_run_id` and `workflow_id` can be obtained from the `run_id` and `workflow_id` fields in the `Run Workflow` response above.

</aside>

**Response:**

```bash
{
    "document_id": "66fe1752927ce8c0ebda42b9",
    "end_date": datetime.datetime(2024, 10, 3, 4, 23, 42, 58430, tzinfo=TzInfo(UTC)),
    "start_date": datetime.datetime(2024, 10, 3, 4, 14, 45, 30553, tzinfo=TzInfo(UTC)),
    "status": "failed",
    "tasks": [
        {
            "end_date": datetime.datetime(
                2024, 10, 3, 4, 20, 4, 293969, tzinfo=TzInfo(UTC)
            ),
            "failed_task_ids": [-1],
            "name": "set_processing_to_in_state__1",
            "start_date": datetime.datetime(
                2024, 10, 3, 4, 20, 4, 293969, tzinfo=TzInfo(UTC)
            ),
            "status": "failed",
            "task_status_summary": {
                "failed": 1,
                "queued": 0,
                "running": 0,
                "skipped": 0,
                "success": 0,
            },
        },
       .
       .
       .
    ],
}
```

---

### Rerun workflow

If a workflow has already run before, re-run the workflow as per requirements

```bash
python3 workflows/rerun_workflow.py --doc_id 66df87ec2b1edfc0dc3b556f --workflow_name "dagtest"
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| workflow_name | Name of the workflow to be run | Required |  |
| doc_id | Document for which the workflow has to be run | Required |  |
| data | Any extra parameters that are required by the worflow | Optional | Stringified JSON |

**Response:**

```bash
{
   "created_at":"2024-09-25T19:33:32.000000+00:00",
   "document_id":"66df87ec2b1edfc0dc3b556f",
   "document_name":"AAPL_10Q.pdf",
   "end_date":"None",
   "in_folders":[
      "66e0f93093798ee1c937e39a"
   ],
   "run_id":"66e0fba3089fbd21c4dd80c3_3df1b127-9ea5-4714-9bf5-b1a5653859f6",
   "start_date":"None",
   "state":"None",
   "workflow_id":"dagtest"
}
```

---

### Skip steps in a workflow

Allow the workflow to skip any steps that are present in the workflow.

```bash
python3 workflows/skip_tasks.py --workflow_name "dagtest" --tasks task_1 task_2
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** | **Allowed values** |
| --- | --- | --- | --- |
| workflow_name | Name of the workflow to be run | Required |  |
| tasks | A list of tasks to be skipped | Required | Strings seperated by spaces (Example —tasks task_1 task_2 |

---

### Get Workflow Runs For Document

Fetches all the workflows runs for a particular document.

```bash
python3 workflows/get_workflows_for_document.py --doc_id 66df87ec2b1edfc0dc3b556f --state "success" --query "ANNUAL REPORT" --skip 0 --limit 1
```

**Parameters:**

| **Parameter** | **Description** | **Required/Optional** |
| --- | --- | --- |
| doc_id | Document for which the workflow has to be fetched | Required |
| state | State of workflow | Optional |
| query | This string is matched with workflow and document name | Optional |
| skip | Number of workflows to skip | Optional |
| limit | Max fetch size | Optional |

**Response:**

```bash
{
   "docs":[
      {
         "created_at":"None",
         "document_id":"66fe1752927ce8c0ebda42b9",
         "document_name":"66fe1752927ce8c0ebda42b9",
         "end_date":"2024-10-03T20:37:32.405064+00:00",
         "in_folders":[
            
         ],
         "run_id":"66fe1752927ce8c0ebda42b9_078c1e26-a82b-4ada-ac5e-357eac3eb2b3",
         "start_date":"2024-10-03T20:37:26.234912+00:00",
         "state":"failed",
         "workflow_id":"process_form_workflow"
      },
      .
      .
      .
      .
      {
         "created_at":"None",
         "document_id":"66fe1752927ce8c0ebda42b9",
         "document_name":"MCS-CS-Handbook-2022-2023Publish.pdf",
         "end_date":"2024-10-03T04:26:21.715502+00:00",
         "in_folders":[
            
         ],
         "run_id":"66fe1752927ce8c0ebda42b9_2451533e-bbec-4e1a-acd5-0ab686f6d430",
         "start_date":"2024-10-03T04:14:47.246008+00:00",
         "state":"failed",
         "workflow_id":"process_form_workflow"
      }
   ],
   "total":7
}
```