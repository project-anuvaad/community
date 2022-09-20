# Anuvaad Workflow Manager

## ANUVAAD DATAFLOW PIPELINE WORKFLOW MANAGER

Workflow Manager is the orchestrator for the entire dataflow pipeline.

### Overview

This document provides details about the Workflow Manager. WFM is the orchestrating module for the Anuvaad pipeline.

### Getting Started

WFM is the backbone service of the Anuvaad system, it is a centralized orchestrator which directs the user input through the dataflow pipeline to achieve the desired output. It maintains a record of all the jobs and all the tasks involved in each job. WFM is the SPOC for the clients to retrieve details, status, error reports etc about the jobs executed (sync/async) in the system. Using WFM, we’ve been able to use Anuvaad not just as a Translation platform but also as an OCR platform, Tokenization platform, Sentence Alignment platform for dataset curation. Every use-case in Anuvaad is defined as a ‘Workflow’ in the WFM, These workflow definitions are in the form of a YAML file, which is read by WFM as an external configuration file.

WFM Config: This is a YAML file which has a well defined structure to create workflows in the Anuvaad system. Every use-case in Anuvaad is called ‘Workflow’.

Workflow - Set of steps to be executed on a given input to obtain the desired output. Anuvaad has 2 types of workflows: Async WF and Sync WF.

Async WF - These are asynchronous workflows, wherein the modules involved in this flow communicate with each other and the WFM via the kafka queue asynchronously.

Sync WF - These are synchronous workflows wherein the modules involved communicate with each other and the WFM via REST APIs. The client receives responses in real time.

Structure of the config is as follows:

* workflowCode: An alphanumeric code that UNIQUELY identifies a workflow. Format: WF\_\<A/S>\_\<codes\_of\_modules\_in\_sequence>
* type: Type of the workflow - ASYNC or SYNC
* description: Description of the workflow to explain what the workflow does
* useCase: An alphanumeric prefix to the job ID signifying a reference to the workflowCode.
* sequence: The set of steps to be defined under the workflow. This is a list of ‘steps’ where each ‘step’ contains keys order, tool & endState.
* The ‘tool’ key is the definition of the tool used in the corresponding ‘step’ in the ‘sequence’. Each tool contains keys name, description, kafka-input, topic, partitions, kafka-output. In case of Sync WFs, the tool contains keys name, description, api-details, uri.
* Order: Number that defines the order of this step in the sequence. 0 is the value for the first step, 1 being next and so on.
* name: Name of the tool
* description: Description of the tool
* kafka-input: Details of the kafka input for that particular tool. The tool must accept input on this topic from the WFM.
* kafka-output: Details of the kafka output for that particular tool. The tool must produce output on this topic from the WFM.
* api-details: Details of the API exposed by the tool for WFM to access.

An example workflowCode: WF\_A\_FCBMTKTR WF = Workflow A = Async FC = File Converter BM = Block Merger TK = Tokeniser TR = Translator Configs can be found here: [wfm\_configs](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-workflow-mgr/config)

WFM has 2 types of IDs involved in the jobs that hep uniquely identify a job and its intermediate tasks: jobID & taskID. jobID: This is a alphanumeric ID that uniquely identifies a job in the system. jobIDs are generated for both Sync and Async Jobs. Format: \<use\_case>-\<random\_string>-<13-digit epoch time> taskID: A job contains multiple intermediate tasks, taskID is a unique ID used to idenity each of those tasks. A combination of these taskIDs mapped to a given jobID can help trace an entire job through the system. Format: \<module\_code>-<13-digit epoch time>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Modules

API Details WFM exposes multiple APIs for the client to execute and fetch jobs in the Anuvaad system. The APIs are as follows: /async/initiate: API to execute Async workflows. /sync/initiate: API to execute Sync workflows. /configs/search: API to search WFM configs /jobs/search: API to search initiated jobs.

### Postman Collection:

[https://www.getpostman.com/collections/11b7d2bc4e5aa37d04c8](https://www.getpostman.com/collections/11b7d2bc4e5aa37d04c8)

### Code:

[https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-workflow-mgr](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-workflow-mgr)

### Prerequisites

* python 3.7
* ubuntu 16.04

Dependencies:

```bash
pip install -r requirements.txt
```

Run:

```bash
python app.py
```

### APIs and Documentation

Details of the APIs can be found here: [https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-workflow-mgr/docs/etl-wf-manager-api-contract.yml](https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-workflow-mgr/docs/etl-wf-manager-api-contract.yml)

Details of the requests flowing in and out through kafka can be found here: [https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-workflow-mgr/docs/etl-wf-manager-kafka-contract.yml](https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-workflow-mgr/docs/etl-wf-manager-kafka-contract.yml)

### Configs

Wokflows have to be configured in a .yaml file as shown in the following document: [https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-workflow-mgr/config/etl-wf-manager-config.yml](https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-workflow-mgr/config/etl-wf-manager-config.yml)

### License

[MIT](https://choosealicense.com/licenses/mit/)
