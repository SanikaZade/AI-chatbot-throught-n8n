# 🤖 AI Chatbot using n8n

An AI-powered chatbot workflow built with **n8n**, **Google Gemini**, and **Google Sheets**. The chatbot receives messages through the n8n chat interface, processes them using an AI Agent powered by Google Gemini, maintains short-term conversation context using memory, and provides the AI Agent with a Google Sheets tool for spreadsheet-related actions.

---

## 📌 Project Overview

This project demonstrates how to build an AI chatbot automation using n8n's AI capabilities.

The workflow consists of four main components:

1. **Chat Trigger** – Receives messages from the user.
2. **AI Agent** – Processes the user's request and decides how to respond or which tool to use.
3. **Google Gemini Chat Model** – Provides the AI/LLM capabilities to the agent.
4. **Simple Memory** – Maintains recent conversation context.
5. **Google Sheets Tool** – Allows the AI Agent to interact with Google Sheets.

The workflow is designed as an automation rather than a traditional application with custom backend code.

---

## ✨ Features

* 💬 Interactive AI chatbot
* 🧠 AI-powered responses using Google Gemini
* 🔄 Conversation memory
* 📊 Google Sheets integration
* 🤖 AI Agent capable of using connected tools
* ⚡ Built completely inside n8n
* 🔐 OAuth/API-based authentication for Google services
* 🧩 Easily extendable with additional AI tools

---

## 🏗️ Workflow Architecture

```text
                    ┌──────────────────────────┐
                    │  User sends chat message │
                    └────────────┬─────────────┘
                                 │
                                 ▼
                    ┌──────────────────────────┐
                    │      Chat Trigger        │
                    │ "When chat message       │
                    │       received"          │
                    └────────────┬─────────────┘
                                 │
                                 ▼
                    ┌──────────────────────────┐
                    │        AI Agent          │
                    │                          │
                    │ Processes user request   │
                    └───────┬─────────┬────────┘
                            │         │
              ┌─────────────┘         └──────────────┐
              ▼                                      ▼
 ┌────────────────────────┐              ┌──────────────────────┐
 │ Google Gemini Chat     │              │    Simple Memory     │
 │ Model                  │              │                      │
 │                        │              │ Context Window: 20   │
 └────────────────────────┘              └──────────────────────┘
                                                   
                            │
                            ▼
                  ┌─────────────────────────┐
                  │ Google Sheets Tool      │
                  │                         │
                  │ Spreadsheet operations  │
                  └─────────────────────────┘
```

---

## Workflow 

 <img width="1206" height="533" alt="Screenshot 2026-09-09 111556" src="https://github.com/user-attachments/assets/d0f82cd2-9377-44ae-a56d-f27a1f4fbb09" />


## 🔄 How the Workflow Works

### Step 1 – User Sends a Message

The chatbot starts when a user sends a message through the n8n chat interface.

The workflow uses the **"When chat message received"** node as its trigger.

```text
User → Chat Trigger
```

The Chat Trigger is configured as an n8n LangChain chat trigger.

---

### Step 2 – Message Goes to the AI Agent

The message received from the Chat Trigger is passed to the **AI Agent**.

The AI Agent acts as the central component of the workflow.

```text
Chat Trigger
      ↓
AI Agent

```

<img width="502" height="641" alt="Screenshot 2026-09-09 111830" src="https://github.com/user-attachments/assets/0bd88c21-8e5d-48e3-8cb4-fc5553078a15" />


<img width="501" height="647" alt="Screenshot 2026-09-09 111847" src="https://github.com/user-attachments/assets/5753ee9f-c829-43c6-87d9-37a87ddcd79f" />


The agent is responsible for understanding the user's request and generating an appropriate response.

---

### Step 3 – Google Gemini Processes the Request

The AI Agent uses the **Google Gemini Chat Model** as its language model.

```text
AI Agent
   ↓
Google Gemini
   ↓
AI-generated response 
```

Google Gemini provides the natural-language understanding and response-generation capabilities required by the chatbot.

The uploaded workflow identifies the model node as:

```text
Google Gemini Chat Model
```

and connects it to the AI Agent as its language model.

---

### Step 4 – Conversation Memory

The chatbot includes a **Simple Memory** node.

The configured context window is:

```text
20 messages
```


This allows the AI Agent to remember recent parts of the conversation rather than treating every message as completely independent.

For example:

<img width="943" height="485" alt="Screenshot 2026-09-09 112127" src="https://github.com/user-attachments/assets/fc7cb0dd-d255-42ca-8442-4549128aefb5" />

---

The memory is connected directly to the AI Agent as its AI memory component.
---

### Step 5 – Google Sheets Integration

The workflow also provides the AI Agent with a **Google Sheets Tool**.

The tool is named:

```text
Create sheet in Google Sheets
```

The configured operation is:

```text
Create
```

This allows the AI Agent to use Google Sheets as an external tool when appropriate.

The Google Sheets node is connected to the AI Agent as an **AI Tool**, rather than as a normal sequential workflow node.

This distinction is important:

```text
Chat Trigger → AI Agent
                    │
                    ├── Gemini → Language Model
                    │
                    ├── Simple Memory → Conversation Memory
                    │
                    └── Google Sheets → AI Tool
```

---

# 🧩 Nodes Used

## 1. When Chat Message Received

**Node Type:**


```text
@n8n/n8n-nodes-langchain.chatTrigger
```

**Purpose:**

Starts the workflow whenever a user sends a chat message.

### Responsibilities

* Accepts incoming chat messages
* Starts an n8n workflow execution
* Passes the user's message to the AI Agent

---

## 2. AI Agent

**Node Type:**

```text
@n8n/n8n-nodes-langchain.agent
```

**Purpose:**

Acts as the central intelligence layer of the chatbot.

The AI Agent receives:

* User input
* Google Gemini language model
* Conversation memory
* Google Sheets tool

The agent determines how the user's request should be handled.

---

## 3. Google Gemini Chat Model

**Node Type:**

```text
@n8n/n8n-nodes-langchain.lmChatGoogleGemini
```

**Purpose:**

Provides the language model used by the AI Agent.

It is connected to the AI Agent through the:

```text
ai_languageModel
```

connection.

This means the Gemini node supplies the AI reasoning and natural-language generation capability to the agent.

---

## 4. Simple Memory

**Node Type:**

```text
@n8n/n8n-nodes-langchain.memoryBufferWindow
```

**Context Window:**

```text
20
```

**Purpose:**

Maintains a window of recent conversation messages.

The memory is connected to the AI Agent using:

```text
ai_memory
```

This enables the chatbot to maintain conversational context.

---

## 5. Create Sheet in Google Sheets

**Node Type:**

```text
n8n-nodes-base.googleSheetsTool
```

**Operation:**

```text
Create
```

**Purpose:**

Provides the AI Agent with access to Google Sheets functionality.

The node is connected using the:

```text
ai_tool
```

connection.

The configured Google Sheets document is supplied to the node through its document configuration.

---

<img width="1177" height="325" alt="Screenshot 2026-09-09 111908" src="https://github.com/user-attachments/assets/0d660c3a-ac3c-4fd3-a2df-a3ee90c323a1" />



# 🔗 Node Connections

The workflow uses the following connections:

### Main Workflow

```text
When chat message received
            │
            ▼
         AI Agent
```

### AI Language Model

```text
Google Gemini Chat Model
            │
            ▼
         AI Agent
```

### AI Memory

```text
Simple Memory
      │
      ▼
   AI Agent
```

### AI Tool

```text
Create sheet in Google Sheets
             │
             ▼
          AI Agent
```

Therefore, the AI Agent is the central component connecting all the AI functionality.

---


# 🛠️ Technology Stack

| Technology           | Purpose                      |
| -------------------- | ---------------------------- |
| **n8n**              | Workflow automation platform |
| **n8n AI Agent**     | AI orchestration             |
| **Google Gemini**    | Large language model         |
| **LangChain nodes**  | AI workflow components       |
| **Google Sheets**    | Spreadsheet integration      |
| **n8n Chat Trigger** | User interaction             |
| **Simple Memory**    | Conversation context         |

---

# 📋 Prerequisites

Before setting up the project, make sure you have:

* An n8n installation or n8n Cloud account
* A Google account
* Google Gemini API access
* Google Sheets access
* Required Google credentials configured in n8n
* Permission to access the Google Spreadsheet used by the workflow

---

# 🚀 Installation & Setup

## Step 1 – Install or Open n8n

You can use either:

* n8n Cloud
* Self-hosted n8n
* Local n8n installation

Open your n8n editor.

---

## Step 2 – Import the Workflow

The project includes the exported workflow JSON file:

```text
AI chatbot.json
```

To import it into n8n:

1. Open n8n.
2. Go to the **Workflows** section.
3. Select **Import from File**.
4. Select:

```text
AI chatbot.json
```

5. Import the workflow.
6. Open the imported workflow.

---

# 🔐 Google Gemini Configuration

The workflow requires a Google Gemini credential.

In n8n:

1. Open the **Google Gemini Chat Model** node.
2. Open the credentials section.
3. Create or select a Google Gemini credential.
4. Enter the required API credentials.
5. Save the credential.
6. Return to the workflow.

The Gemini node is connected to the AI Agent as its language model.

---

# 🔑 Google Sheets Configuration

The project also requires Google Sheets authentication.

In n8n:

1. Open the **Create sheet in Google Sheets** node.
2. Open its credentials.
3. Create/select a Google Sheets OAuth2 credential.
4. Complete the Google authorization process.
5. Give n8n the required Google Sheets permissions.
6. Select the spreadsheet/document required by the workflow.
7. Save the node.

The workflow contains a Google Sheets document reference, so ensure that the authenticated Google account has access to the relevant spreadsheet.

---

# 💬 Testing the Chatbot

After configuring the credentials:

1. Open the workflow.
2. Make sure all required credentials are valid.
3. Save the workflow.
4. Start/test the workflow.
5. Open the chat interface provided by the Chat Trigger.
6. Send a message.

Example:

```text
Hello
```

The message should follow this flow:

```text
User
 ↓
Chat Trigger
 ↓
AI Agent
 ↓
Google Gemini
 ↓
Response
 ↓
User
```

---

# 🧪 Example Conversations

### Basic Question

```text
User:
Hello

AI:
Hello! How can I help you today?
```

### Context Example

```text
User:
My name is Sanika.

AI:
Nice to meet you, Sanika!

User:
What is my name?

AI:
Your name is Sanika.
```

The second response can use the conversation context maintained by the Simple Memory node.

---

# 📊 Google Sheets Tool

The Google Sheets integration makes the chatbot more than a simple question-answer system.

The AI Agent has access to a Google Sheets tool.

The tool can be used when the user's request requires a spreadsheet-related action supported by the configured node.

For example, a future extension could allow requests such as:

```text
Create a new spreadsheet for my project.
```

The AI Agent can determine whether the connected Google Sheets tool is relevant to the request.

---

# 🧠 Memory Configuration

The workflow uses:

```text
Simple Memory
```

with:

```text
Context Window Length = 20
```

This means the memory node maintains a window of recent conversation context.

### Why Memory Is Useful

Without memory:

```text
User → Message 1
Bot → Response

User → Message 2
Bot → Treats it independently
```

With memory:

```text
Message 1
   ↓
Memory
   ↓
Message 2
   ↓
AI Agent
   ↓
Context-aware response
```

### Changing the Memory Size

To change the amount of conversation context:

1. Open **Simple Memory**.
2. Find **Context Window Length**.
3. Change `20` to the desired value.
4. Save the workflow.

A larger context window can provide more conversation history but may increase model usage.

---

# 🔄 Complete Execution Flow

The complete workflow can be summarized as:

```text
┌───────────────────────────┐
│           User            │
│      Sends a message      │
└─────────────┬─────────────┘
              │
              ▼
┌───────────────────────────┐
│       Chat Trigger        │
│                           │
│ When chat message         │
│ is received               │
└─────────────┬─────────────┘
              │
              ▼
┌───────────────────────────┐
│         AI Agent          │
│                           │
│ Understands the request   │
│ and selects response/tool │
└───────┬────────┬──────────┘
        │        │
        │        │
        ▼        ▼
┌────────────┐ ┌───────────────┐
│  Gemini    │ │ Simple Memory │
│ Chat Model │ │   20 messages │
└────────────┘ └───────────────┘
        │
        │
        ▼
┌───────────────────────────┐
│     Google Sheets Tool    │
│                           │
│ Spreadsheet functionality │
└───────────────────────────┘
```

---

# ⚙️ Configuration Summary

| Component          | Configuration                           |
| ------------------ | --------------------------------------- |
| Chat Trigger       | Receives chat messages                  |
| AI Agent           | Main processing/orchestration node      |
| Gemini             | AI language model                       |
| Memory             | Window-based memory                     |
| Memory Window      | 20 messages                             |
| Google Sheets      | Create spreadsheet operation            |
| Workflow Execution | Version 1 execution order               |
| Workflow Status    | Imported workflow is initially inactive |

---

# 🔒 Security Best Practices

When publishing this project to GitHub, follow these practices.

### Never upload:

* API keys
* OAuth client secrets
* Access tokens
* Refresh tokens
* Passwords
* Private Google credentials
* Private spreadsheet data

### Use environment/credential management

Keep authentication details inside n8n's credential system instead of hardcoding them into workflow files.

### Check exported JSON

Before uploading the workflow to GitHub, inspect the JSON and remove or replace any sensitive information that should not be publicly shared.

---

# 🐛 Troubleshooting

## 1. Google Gemini Authentication Error

### Possible causes

* Invalid API key
* Expired credential
* Incorrect Google Gemini credential
* API access not enabled

### Solution

Open:

```text
Google Gemini Chat Model
```

and verify that the correct credential is selected.

---

## 2. Google Sheets OAuth Error

### Possible causes

* OAuth client is not configured correctly
* Google account is not authorized
* Required Google APIs are not enabled
* Redirect URI is incorrect
* Credential was created for a different n8n instance

### Solution

Check the Google Sheets OAuth2 credential inside n8n and complete the authorization process again if required.

---

## 3. Chatbot Does Not Respond

Check:

1. Is the workflow active/testable?
2. Is the Chat Trigger configured correctly?
3. Does the AI Agent have a valid language model?
4. Is the Gemini credential valid?
5. Are there execution errors in n8n?

Check the **Executions** section in n8n for detailed error information.

---

## 4. Memory Does Not Work

Verify that:

```text
Simple Memory
```

is connected to:

```text
AI Agent
```

through the AI memory connection.

Also verify the session/conversation configuration of the chat trigger.

---

## 5. Google Sheets Tool Does Not Work

Check:

* Google Sheets OAuth credentials
* Spreadsheet access permissions
* Selected document
* Google Sheets node configuration
* AI Agent tool connection

The Google Sheets node must be connected to the AI Agent as an AI Tool.

---

# 📁 Project Structure

A simple repository structure can be:

```text
AI-Chatbot/
│
├── AI chatbot.json
└── README.md
```

### `AI chatbot.json`

Contains the exported n8n workflow.

### `README.md`

Contains project documentation, installation instructions, architecture, configuration, and troubleshooting information.



# 🌱 Possible Future Improvements

The current workflow provides a strong foundation for a more advanced AI automation system.

Potential improvements include:

### 1. More AI Tools

Additional tools could be connected to the AI Agent, such as:

* Gmail
* Google Calendar
* Notion
* Slack
* Databases
* REST APIs
* Web search

---

### 2. Better Long-Term Memory

The current workflow uses window-based memory.

A future version could use a database or vector store for persistent long-term memory.

Possible options include:

```text
PostgreSQL
Supabase
Pinecone
Qdrant
```

---

### 3. Advanced Google Sheets Automation

The Google Sheets integration could be expanded to support additional spreadsheet operations, such as:

```text
Create spreadsheet
Read data
Append rows
Update rows
Search records
Generate reports
```

---

### 4. Custom System Prompt

A custom system prompt can be added to define:

* Chatbot personality
* Response format
* Business rules
* Allowed actions
* Tool usage rules
* Error-handling behavior

---

### 5. User Authentication

For production applications, user authentication can be added so that users can securely access their own conversations and data.

---

### 6. Production Chat Interface

The n8n chat interface can be integrated into a custom frontend using technologies such as:

```text
React
Next.js
HTML/CSS/JavaScript
Streamlit
```

---

# 📈 Production Considerations

Before deploying this workflow for real users, consider:

* Credential security
* API usage limits
* Gemini API costs
* Google Sheets permissions
* Error handling
* Logging
* User authentication
* Rate limiting
* Data privacy
* Conversation storage
* Monitoring

The workflow is currently a foundation that can be extended into a production-grade AI assistant.

---

# 🎯 Learning Objectives

This project demonstrates the following concepts:

* n8n workflow automation
* AI Agent implementation
* Google Gemini integration
* LangChain-based AI nodes
* Chat-based workflow triggers
* AI memory
* Tool calling
* Google Sheets integration
* OAuth authentication
* AI-powered workflow automation

---

# 📌 Key Takeaway

This project demonstrates how **n8n can be used to create an AI chatbot that combines a large language model, conversational memory, and external tools in a single automation workflow**.

The architecture is centered around the AI Agent:

```text
                    AI Agent
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
     Gemini         Memory       Google Sheets
   AI Model       20 messages        Tool
        │              │              │
        └──────────────┼──────────────┘
                       │
                       ▼
                  Chat Trigger
                       │
                       ▼
                      User
```

This modular architecture makes the chatbot easy to understand and provides a foundation for adding additional AI capabilities and automation tools in the future.

---

---

## ⭐ Acknowledgements

Built using:

* n8n
* Google Gemini
* Google Sheets
* LangChain-compatible n8n AI nodes

---

##  Requirements 

If the workflow does not work after import, check the following in order:

```text
1. n8n installation
        ↓
2. Workflow import
        ↓
3. Gemini credentials
        ↓
4. Google Sheets credentials
        ↓
5. Spreadsheet permissions
        ↓
6. Chat Trigger
        ↓
7. AI Agent connections
        ↓
8. Workflow execution logs
```

This sequence helps identify most configuration-related problems quickly.
