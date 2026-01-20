# Deep Research (Agents + Web Search + Report + Email)

A multi-agent “deep research” app built with the OpenAI Agents SDK.
You type a topic → the system plans multiple searches → runs web searches → synthesizes a long markdown report → optionally emails the report → streams progress in a Gradio UI.

This project is designed for **fast, reproducible Python environments** using `uv` instead of pip/venv.

---

## What’s inside

* **Gradio UI**: enter a query, see streaming status + final report.
* **Planner agent**: proposes a small set of targeted searches.
* **Search agent**: runs web searches (via hosted `WebSearchTool`) and returns short summaries.
* **Writer agent**: turns summaries into a detailed markdown report.
* **Email agent**: sends the report as a clean HTML email via SendGrid.

---

## Project structure

```
Deep-Research/
├── deep_research.py        # Gradio app entrypoint
├── research_manager.py    # orchestration (plan → search → write → email)
├── planner_agent.py       # creates the search plan
├── search_agent.py        # runs web searches + summarizes results
├── writer_agent.py        # produces the final report (markdown)
├── email_agent.py         # sends the report via SendGrid
├── .env                  # API keys (not committed)
└── pyproject.toml        # uv dependency manifest
```

---

## Requirements

* Python **3.10+**
* **uv** installed

Restart your terminal after installation.

---

## Setup (with uv)

### 1) Create project environment

From inside the `Deep-Research` repo:

```bash
uv venv
```

Activate it:

**macOS / Linux**

```bash
source .venv/bin/activate
```

**Windows (PowerShell)**

```powershell
.\.venv\Scripts\Activate.ps1
```

---

### 2) Create `pyproject.toml`

If not already present:

```bash
cat > pyproject.toml << 'EOF'
[project]
name = "deep-research"
version = "0.1.0"
description = "Multi-agent deep research app with web search and report generation"
requires-python = ">=3.10"

dependencies = [
    "gradio",
    "python-dotenv",
    "pydantic",
    "sendgrid",
    "openai-agents"
]
EOF
```

Then install dependencies:

```bash
uv sync
```

This creates a **locked, reproducible environment**.

---

### 3) Set environment variables

Create a `.env` file:

```bash
cat > .env << 'EOF'
OPENAI_API_KEY=your_openai_api_key_here
SENDGRID_API_KEY=your_sendgrid_api_key_here
EOF
```

---

### 4) Configure email sender (optional)

Open `email_agent.py` and update:

* the verified **from** sender
* the **to** recipient email

If you do not want email sending, comment out the email step in `research_manager.py`.

---

## Run the app

```bash
uv run python deep_research.py
```

Gradio will launch in your browser.

---

## OpenAI Hosted Tools

OpenAI Agents SDK includes the following hosted tools:

* The `WebSearchTool` lets an agent search the web.
* The `FileSearchTool` allows retrieving information from your OpenAI Vector Stores.
* The `ComputerTool` allows automating computer use tasks like taking screenshots and clicking.

---

## Important note — WebSearchTool pricing

This project uses OpenAI’s hosted **WebSearchTool**.

> ⚠️ **Cost warning**
> WebSearchTool costs around **$0.025 per call**.
> A few runs can easily reach **$2–$3** depending on how many searches are triggered.

Student **Christian W.** also noted that OpenAI may bill multiple searches for a single logical call, so costs can exceed $0.025 per run.

Always check the latest pricing here:

```
https://platform.openai.com/docs/pricing#web-search
```

If cost is a concern, you can disable web search and plug in free or low-cost search APIs.

---

## Troubleshooting

### uv not found

Make sure `~/.cargo/bin` or `~/.local/bin` is in your PATH (depends on install method).

### Web search errors

Ensure:

* `OPENAI_API_KEY` is valid
* Your account has access to WebSearchTool
* Billing is enabled

### Email not sent

Ensure:

* `SENDGRID_API_KEY` is valid
* Sender email is verified in SendGrid


---

## Author

**Creator:** Baddy Reda  
**Status:** UM6P Student | AI & Agents Enthusiast 
**Contact:** redabaddy@emines.um6p.ma

