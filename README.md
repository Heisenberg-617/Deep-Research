# Deep Research (Agents + Web Search + Report + Email)

A multi-agent “deep research” app built with the OpenAI Agents SDK.
You type a topic → the system plans multiple searches → runs web searches → synthesizes a long markdown report → optionally emails the report → streams progress in a Gradio UI.

## What’s inside

* **Gradio UI**: enter a query, see streaming status + final report.
* **Planner agent**: proposes a small set of targeted searches.
* **Search agent**: runs web searches (via hosted `WebSearchTool`) and returns short summaries.
* **Writer agent**: turns summaries into a detailed markdown report.
* **Email agent**: sends the report as a clean HTML email via SendGrid.

---

## Project structure

* `deep_research.py` — Gradio app entrypoint
* `research_manager.py` — orchestration (plan → search → write → email)
* `planner_agent.py` — creates the search plan
* `search_agent.py` — runs web searches + summarizes results
* `writer_agent.py` — produces the final report (markdown)
* `email_agent.py` — sends the report via SendGrid

---

## Setup

### 1) Create and activate a virtual environment

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows (PowerShell)**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 2) Install dependencies

If you don’t already have a `requirements.txt`, create one:

```bash
cat > requirements.txt << 'EOF'
gradio
python-dotenv
pydantic
sendgrid
openai-agents
EOF
```

Then install:

```bash
pip install -r requirements.txt
```

> Note: if your Agents SDK package name differs in your environment, replace `openai-agents` with the package name you’re using for `from agents import ...`.

### 3) Set environment variables

Create a `.env` file in the project root:

```bash
cat > .env << 'EOF'
OPENAI_API_KEY=your_openai_api_key_here
SENDGRID_API_KEY=your_sendgrid_api_key_here
EOF
```

### 4) Configure email sender/recipient (optional)

If you want the app to email the report, open `email_agent.py` and update:

* the verified **from** sender
* the **to** recipient email

If you **don’t** want email sending, you can comment out the email step in `research_manager.py`.

---

## Run the app

```bash
python deep_research.py
```

It launches a Gradio page in your browser. Type a topic and click **Run**.

---

## OpenAI Hosted Tools

OpenAI Agents SDK includes the following hosted tools:

* The `WebSearchTool` lets an agent search the web.
* The `FileSearchTool` allows retrieving information from your OpenAI Vector Stores.
* The `ComputerTool` allows automating computer use tasks like taking screenshots and clicking.

### Important note - API charge of WebSearchTool

This is costing me **2.5 cents per call** for OpenAI `WebSearchTool`. That can add up to **$2–$3 for the next 2 labs**. We'll use free and low cost search tools with other platforms, so feel free to skip running this if the cost is a concern. Also student **Christian W.** pointed out that OpenAI can sometimes charge for multiple searches for a single call, so it could sometimes cost more than **2.5 cents per call**.

Current pricing is here:

```text
https://platform.openai.com/docs/pricing#web-search
```

---

## Troubleshooting

* **No email sent / SendGrid error**
  Confirm `SENDGRID_API_KEY` is set and your sender address is verified in SendGrid.

* **Web search errors / tool required**
  Ensure `OPENAI_API_KEY` is valid and your account has access to web search. Costs apply.

* **Nothing streams in Gradio**
  Make sure you’re running Python 3.10+ and installed dependencies in the active venv.

---

## License

Add a license if you plan to share publicly (MIT is a common default).
