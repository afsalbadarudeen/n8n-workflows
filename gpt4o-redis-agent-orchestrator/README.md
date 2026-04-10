# Stateful AI Agent with Custom Tool Calling (n8n)

An advanced, from-scratch implementation of an LLM agent orchestrator in n8n. This workflow manually handles OpenAI's tool-calling loop and maintains persistent multi-turn conversation memory using Redis.

## 🏗️ Architecture & Visual Flow

![Workflow Diagram](./Screenshot.jpg)

This workflow operates as an API endpoint, receiving user messages and responding dynamically based on whether the LLM needs to use external tools to formulate its answer.

### Core Features
* **Persistent Memory:** Utilizes Redis to store and retrieve conversation history using a unique `session_id`. Context windows are strictly managed to the last 20 messages.
* **Dynamic Tool Dispatching:** Parses OpenAI's `tool_calls` finish reason and routes execution to custom code blocks.
* **Multi-Step Reasoning:** If a tool is called, the results are injected back into the conversation history, and a subsequent request is made to the LLM to synthesize the final answer.

## 🛠️ Tech Stack & Nodes
* **Entry/Exit:** Webhook Node (POST) & Respond to Webhook Node.
* **State Management:** Redis Nodes (Get/Set with 24h TTL).
* **LLM Engine:** HTTP Request Nodes calling the OpenAI API (`gpt-4o`) directly.
* **Logic & Routing:** Custom JavaScript nodes for payload formatting, math evaluation, and conditional routing.

## 🔌 Available Tools (Extensible)
The agent is currently equipped with the following functional and stubbed tools:
1. `calculate`: Safely evaluates mathematical expressions.
2. `web_search`: Pre-configured to accept live integration with SerpAPI or Brave Search.
3. `get_weather`: Pre-configured for OpenWeatherMap integration.
4. `query_database`: Designed to route SQL queries to internal Postgres/MySQL nodes.

## 🚀 How to Run

1. Import the `workflow.json` into your n8n instance.
2. Ensure you have a running Redis instance connected to n8n.
3. Add your OpenAI API credentials to the HTTP Request nodes.
4. Send a POST request to the webhook URL with the following JSON body:
   ```json
   {
     "session_id": "user_123_abc",
     "message": "What is 15% of 1200?",
     "user_id": "user_123"
   }
