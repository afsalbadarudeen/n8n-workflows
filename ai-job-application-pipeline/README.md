# AI-Powered Job Application & Outreach Pipeline (n8n)

An automated, end-to-end workflow built in n8n that scrapes job listings, evaluates fit using LLMs, and generates tailored application materials and outreach messages.

## 🏗️ Architecture & Visual Flow

![Workflow Diagram](./image.png)

## 🎯 Objective
To streamline the job search process by eliminating manual screening and generic applications. This pipeline autonomously identifies relevant opportunities, scores them against my specific background, and drafts highly personalized cover letters and recruiter messages.

## ⚙️ Core Logic & Features

1. **Automated Ingestion:** A Cron trigger fires every 12 hours, fetching the latest job postings via a target job board API.
2. **Data Transformation & Deduplication:** Custom JavaScript parses the raw JSON payload. A Google Sheets lookup prevents redundant processing by checking a database of previously applied roles.
3. **LLM Fit Scoring:** Integrates with OpenAI (GPT-4) to analyze the job description against my resume. A custom parsing node extracts a quantitative "Fit Score."
4. **Conditional Routing:** An `IF` node acts as a gateway; only roles scoring above an 80/100 threshold proceed to the generation phase.
5. **Prompt Chaining for Content Generation:** * **Step A:** Generates a targeted cover letter and adapts resume bullet points to the specific job requirements.
    * **Step B:** Drafts a concise, engaging LinkedIn cold outreach message for the hiring manager.
6. **Execution & Logging:** Drafts the email via Gmail integration and logs the targeted company, role, and fit score into a tracking spreadsheet.

## 🛠️ Tech Stack & Nodes Utilized
* **n8n:** Workflow orchestration and visual programming.
* **OpenAI API (GPT-4):** Semantic analysis and natural language generation.
* **JavaScript:** Data mapping, regex operations, and payload restructuring.
* **Google Workspace API:** Interacting with Sheets (Database) and Gmail (Communication).
* **REST APIs:** HTTP nodes for fetching external job data.

## 🚀 How to Import and Run

1. Copy the contents of `workflow.json`.
2. Open your n8n instance (Desktop or Cloud).
3. Create a new workflow, click the top-right menu, and select **Import from File** (or simply paste the JSON directly onto the canvas).
4. **Configure Credentials:** You will need to authenticate the following nodes with your own accounts:
   * OpenAI API Key
   * Google Sheets OAuth
   * Gmail OAuth
5. Update the HTTP Request node with your preferred job board API endpoint.
