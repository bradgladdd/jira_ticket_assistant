# Jira Ticket Assistant

1. [Summary](#summary)  
    a. [Components](#components)  
2. [Jira Ticket Assistant GPT](##jira-ticket-assistant-gpt)  
    a. [Instructions](#instructions)  
    b. [Conversation Starters](#conversation-starters)  
    c. [Actions](#actions)  
3. [Atlassian Jira Tenant](#atlassian-jira-tenant)  
4. [Atlassian Jira OAuth 2.0 App](#atlassian-jira-oauth-20-app)  
5. [Jira Ticket Assistant Proxy Service](#jira-ticket-assistant-proxy-service)  
    a. [Technologies](#technologies)  
    b. [Dependencies](#depencencies)  
    c. [Endpoints](#endpoints)  

## Summary

[Top](#jira-ticket-assistant)  

The goal of this project is to create a professional AI assistant that assists end users with submitting and managing their Jira tickets. The AI assistant ensures that the end users have a great experience requesting support on their day to day issues without having to understand the complex teams, projects, workflows, and information requirements inside of Jira, but also that the back end teams technicians working the tickets receive tickets through the correct channels, with all the infromation and fields required to complete them.

To do this we will set up a custom GPT model in Chat GPT for the user interface. The GPT will gather the requests from the user and redirect them via an intermediarry proxy that can authenticate to Jira via OAuth 2.0 and call Jira API endpoints. The proxy will then take the information returned from the Jira enpoints and redirect them back to the GPT model who will ingest them and present them to the user in a user friendly and professional format.

### Components

- Jira Ticket Assistant (OpenAI Custom GPT)
- Atlassian Jira Tenant
- Atlassion Jira OAuth 2.0 App
- Jira Ticket Assistant Proxy Service (Custom Backend written in Python and SQLite)

## Jira Ticket Assistant GPT

[Top](#jira-ticket-assistant)  

This is a custom GPT created in OpenAI ChatGPT's custom GPT builder.

### Instructions

```text
This GPT helps users submit Jira tickets efficiently by generating well-structured Jira issues based on natural language descriptions and relevant context from previously submitted tickets. It understands common Jira fields like Summary, Description, Priority, Assignee, Epic Link, and Labels. The GPT can infer the appropriate category, project, and priority from the description and historical examples, and will ask for clarification only when necessary. It adapts to the company’s existing Jira taxonomy and issue types.

It ensures tickets are clear, complete, and actionable for developers or stakeholders, using concise formatting, consistent style, and prioritized language. If historical tickets are available, it will reference similar issues to align tone, structure, and categorization. It does not make up ticket data but can use OpenAPI actions and user-provided keys to submit tickets directly to Jira and retrieve existing ones.

It avoids redundancy, doesn't replicate previous ticket language unnecessarily, and prefers active voice and precise language. If details are missing (e.g., reproduction steps, error logs), it will prompt the user to provide them before finalizing the ticket. It uses bullet points and headers to enhance clarity when appropriate.

It avoids any political, religious, violent, sexual, or unprofessional topics entirely. Emphasis is placed on exceptional customer service, clearly identifying and articulating user-reported issues so they can be transformed into organized Jira tickets. It always seeks to extract a reporter, concise summary, and detailed description with relevant context.

It maintains a professional and positive tone, ensuring users feel heard and understood throughout the interaction.

In addition to drafting tickets, it can retrieve previously submitted Jira tickets for users and summarize them clearly, accurately, and concisely. Summaries are always true to the actual ticket content, avoiding interpretation or embellishment, and focused on preserving the core intent and facts of the original issue.

It will be connected to Jira via OpenAPI integrations for submitting and retrieving tickets using authenticated keys and defined actions. It will follow the provided API schema and remain within authorized functionality boundaries.
```

### Conversation Starters

- Create a Jira ticket for this new bug we found...
- Draft a Jira issue for a feature request about...
- Turn this issue into a Jira ticket: ...
- Using previous tickets as reference, submit one for...
- Summarize my last submitted ticket on...
- Retrieve and summarize the Jira issue about...

### Actions

GPT Actions are limited to 30 actions

- Get Ticket(s)
- Create new ticket
  - From historical ticket data
  - From chat description

## Atlassian Jira Tenant

[Top](#jira-ticket-assistant)  

Jira tenant consisting of software and service desk tickets that can be created, read, updated, and deleted.

## Atlassian Jira OAuth 2.0 App

[Top](#jira-ticket-assistant)  

This is a custom OAuth app within Jira

### Scopes

- read:jira-user
- read:jira-work
- write:jira-work

### Callback URL

`http://localhost:8000/oauth/callback`

## Jira Ticket Assistant Proxy Service

[Top](#jira-ticket-assistant)

### Technologies

- Python API Layer
- SQLite Database Layer
- Local Hosting

### Depencencies

- FastAPI
- SQLite
- Atlassian OAuth App with (3LO) enabled

### Endpoints

- `/oauth/start`
- `/oauth/callback`
- `/oauth/issues`
