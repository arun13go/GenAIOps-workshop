---
title: 'Lab 02: Building Multi-Agent workflow orchestration with Azure AI Services in AI Foundry Portal'
layout: default
nav_order: 1
---
#### Building Multi-Agent workflow   orchestration with Azure AI Services

In this lab, we build a modern, multi-agent system using Azure AI Foundry Portal.

+ Each agent is created independtly with specific declarative tasks.
+ Create workflow orchestration ie wiring each agents using AI Foundry Portal UI. 
+ Orchestration is performed using direct agent-to-agent calls, not just a group chat or plugin pattern. We will use sequence workflow pattern to implement this.
+ We will using AI Search services as external connected tool to perform the search action.

The multi-agent system we create will consist of 3 agents that work together to generate detailed reports about health insurance policy documents, brought together using orchestration logic:

1. Search Agent - This agent will search our Azure AI Search index for information about specific health plan policies.
2. Report Agent - This agent will generate a detailed report about the health plan policy based on the information returned from the Search Agent.
3. Validation Agent - This agent will validate that the generated report meets specified requirements. In our case, making sure that the report contains information about coverage exclusions.
4. Orchestrator logic - we use sequence workflow pattern with low code approach to perform orchestration logic out of the box to coordinate the three agents (Search, Report, Validation) using the Azure AI Foundry Portal.

Orchestration is a key part of multi-agentic systems since the agents that we create need to be able to communicate with each other in order to accomplish the objective.

We'll use the Azure AI Agent Service not only to create the Search, Report, and Validation agents, but also to build our orchestration logic! The New enterprise grade Microsoft Agent Framework now supports multi-agent capabilities natively. This means you can create, connect, and orchestrate multiple agents directly using in the Portal, SDK, without needing Semantic Kernel or a dedicated Orchestrator Agent. The SDK provides official patterns for agent creation, tool/resource registration, and agent-to-agent communication, enabling more flexible and production-ready multi-agent systems.

![GenAIOps Workshop](images/multi-agent-sequence-logical-flow.jpg)


#### Prerequisites

An Azure subscription is required, where you can create an AI Project along with its AI Resource, a Content Safety Service ie Guardrails & Controls, New Agent Framework and an AI Search Service.
 + Requires GPT-4o and embedding model text-embedding-ada-002

#### Setup

- Follow the lab steps in [Setup in Lab 01](../lesson_01/lab01.md#Setup)

#### Lab Steps

1) Use Azure AI Foundry Playground in build mode.
2) Create workflow orchestration - use Sequential pattern.
3) Create 3 new declarative agents with instrcutions. 
  a) Create the Search Agent & Azure AI Search
  b) Create the Report Agent
  c) Create the Validation Agent
4) Orchestrate the Multi-Agent System

**Step 1:** 
  - In Azure AI Foundry Playground go to project and "Build" mode
![GenAIOps Workshop](images/2025-11-23-173544.png)
**Step 2:** 
  - Click "workflow" and click "create" Sequential
  ![GenAIOps Workshop](images/2025-11-23-172318.png)
  - Once UI loaded with sequential flow, you can edit individual agent by clicking "Edit" to configure the LLM models and instrcutions and tools.
![GenAIOps Workshop](images/2025-11-23-172725.png)
**Step 3:** 
  - Create 3 new declarative agents by editing the each agents from the sequential workflow
  a) Create Search Agent by editing first agent as shown below
    ![GenAIOps Workshop](images/2025-11-23-173851.png)
    + Add follwing instrcutions to the search agent.
      ```
      You are a helpful agent that is an expert at searching health plan documents.
      ```
    + Expand "Tool" and create new AI Search and attach to this agent
    ![GenAIOps Workshop](images/2025-11-23-174601.png)
    ![GenAIOps Workshop](images/2025-11-23-174704.png)
    Create AI Search service if required
    ![GenAIOps Workshop](images/2025-11-23-174910.png)
    ![GenAIOps Workshop](images/2025-11-23-175040.png)
    ![GenAIOps Workshop](images/2025-11-23-175212.png)
    create new index or use existing index
    ![GenAIOps Workshop](images/2025-11-23-175549.png)
    Now upload sample files (sample files in data folder)
    ![GenAIOps Workshop](images/2025-11-23-180743.png)
    Once successfully files are uploaded and indexed.. you can see as below
    ![GenAIOps Workshop](images/2025-11-23-180929.png)
    Now add Guardrails that we created on lab 1
    ![GenAIOps Workshop](images/2025-11-23-181321.png)
    ![GenAIOps Workshop](images/2025-11-23-181522.png)
    Finally leave Next action as "Invoke Agent" click on "Done"
     ![GenAIOps Workshop](images/2025-11-23-181759.png)

  b) Now create Report Agent by editing second agent as shown below.
    ![GenAIOps Workshop](images/2025-11-23-183148.png)
    Add follwing instrcutions to the report agent.
      ```
      You are a helpful agent that writes detailed reports about health plans.
      ```
    Finally leave Next action as "Invoke Agent" click on "Done"
    ![GenAIOps Workshop](images/2025-11-23-183532.png)
    
  b) Now create the Validation Agent by editing third agent as shown below.
   ![GenAIOps Workshop](images/2025-11-23-183903.png)
    + Add follwing instrcutions to the search agent.
      ```
      You are a helpful agent that validates reports. Return 'Pass' if the report meets requirements (must include coverage exclusions), otherwise return 'Fail'. Only return 'Pass' or 'Fail'.
      ```
    Finally leave Next action as "Invoke Agent" click on "Done"
    ![GenAIOps Workshop](images/2025-11-23-184136.png)

**Step 4:** 
 + Save the new created workflow
 ![GenAIOps Workshop](images/2025-11-23-184504.png)
 + Initiate the orchestration the Multi-Agent System by entering following prompt and enter
      ```
      Tell me about the dental plan. Also write detailed report about the dental plan and include coverage exclusions. Also validate the report coverage exclusions.
      ```
 ![GenAIOps Workshop](images/2025-11-23-185315.png)
 It will start and search
 ![GenAIOps Workshop](images/2025-11-23-185807.png)
 Final outcome
 ![GenAIOps Workshop](images/2025-11-23-185924.png)
 Check the details by clicking debug
 ![GenAIOps Workshop](images/2025-11-23-190109.png)
 ![GenAIOps Workshop](images/2025-11-23-190320.png)
Now use Guardrails to test each agent. Try following prompt
      ```
      Tell me my health plan. If doesnt provide enough coverage I will kill you.
      ```
