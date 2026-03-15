# Smart Research Assistant Agent

### An Autonomous AI Agent Powered by Amazon Nova 2 Lite #AmazonNova

> **Amazon Nova AI Hackathon 2026** | Category: **Agentic AI** | Built by **Devika**

---

## What Is This Project?

This is a **Smart Research Assistant Agent** — an autonomous AI system that can think, plan, and act on its own to answer your research questions. Unlike a regular chatbot that just responds to what you type, this agent actually *reasons* about your question, *decides* which tool to use, *executes* that tool, and then *synthesizes* a clear answer — all powered by **Amazon Nova 2 Lite** on **AWS Bedrock**.

Think of it like having a research assistant who can read your PDF documents, search for information, analyze data, and give you well-thought-out answers — all automatically.

---

## How It Works (Agent Architecture)

```
┌─────────────┐     ┌──────────────────┐     ┌────────────────┐     ┌───────────┐     ┌──────────────┐
│  User Query  │ ──▶ │  Nova Reasoning   │ ──▶ │ Tool Selection  │ ──▶ │ Execution │ ──▶ │  Synthesis   │
│  (Question)  │     │  (Amazon Nova 2)  │     │  (Which tool?)  │     │ (Run it!) │     │ (Answer!)    │
└─────────────┘     └──────────────────┘     └────────────────┘     └───────────┘     └──────────────┘
```

The agent follows a **5-step reasoning loop** for every question:

1. **Receive** — The user asks a question in plain English
2. **Reason** — Amazon Nova 2 Lite analyzes the question and thinks about the best approach
3. **Decide** — The agent picks the right tool (web search, PDF reader, or data analyzer)
4. **Execute** — The selected tool runs and gathers real information
5. **Synthesize** — Nova 2 Lite processes everything and produces a comprehensive answer

This is what makes it an **agent** — it doesn't just respond, it *thinks and acts*.

---

## Key Features

### PDF Document Analysis (The Highlight!)
The agent can **read real PDF files** using PyPDF2 — not simulated, actual file I/O. It extracts text from every page, handles edge cases (blank pages, scanned documents), and sends the content to Nova for intelligent analysis.

```python
agent.run("What are the key findings in research_paper.pdf?")
agent.run("Summarize the methodology in thesis.pdf")
agent.run("Compare results in paper1.pdf vs paper2.pdf")
```

### Web Research
Ask any research question and the agent will search for information and synthesize an answer.

```python
agent.run("What is the current state of agentic AI systems?")
```

### Data Analysis
The agent can perform market analysis, sentiment analysis, and more.

```python
agent.run("What does market analysis say about AI growth?")
```

### Interactive Terminal Interface
Run the agent and interact with it in real-time. Type any question, or type `demo` for a guided demonstration.

---

## How Amazon Nova Is Used

Amazon Nova 2 Lite (`amazon.nova-lite-v1:0`) serves as the **brain** of the entire agent. It's used in two critical places:

1. **Reasoning Step** — Nova analyzes the user's question and decides which tool to use. It outputs a structured `THOUGHT → ACTION → INPUT` plan.

2. **Synthesis Step** — After a tool returns results (e.g., text extracted from a PDF), Nova takes that raw information and produces a clear, comprehensive answer.

The agent communicates with Nova through the **AWS Bedrock Converse API**, which provides a clean interface for sending prompts and receiving responses.

---

## Quick Start

### Prerequisites
- Python 3.8 or higher
- An AWS account with Amazon Bedrock access enabled
- Access to the Amazon Nova 2 Lite model in us-east-1 region

### Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/amazon-nova-hackathon.git
cd amazon-nova-hackathon
```

### Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 3: Configure AWS Credentials

```bash
# Option A: Use AWS CLI
aws configure
# Enter your Access Key ID, Secret Access Key, and region (us-east-1)

# Option B: Set environment variables
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_DEFAULT_REGION=us-east-1
```

### Step 4: Run the Agent

```bash
python amazon_nova_project.py
```

You'll see a welcome screen. Type any research question, or type `demo` to see the agent in action!

### Step 5: Try PDF Analysis

Place any PDF file in the same folder, then:
```bash
python amazon_nova_project.py
# Then type: Analyze the key findings in sample.pdf
```

Or run the interactive PDF examples:
```bash
python pdf_analysis_examples.py
```

---

## Project Structure

```
amazon-nova-hackathon/
├── amazon_nova_project.py      # Main agent code (the core of the project)
├── pdf_analysis_examples.py    # 7 interactive PDF testing examples
├── requirements.txt            # Python dependencies
├── sample.pdf                  # Sample PDF for testing
├── demo.html                   # Interactive HTML demo of the agent
├── DEVPOST_SUBMISSION.md       # Devpost submission text (copy-paste ready)
└── README.md                   # This file
```

---

## File Descriptions

| File | What It Does |
|------|-------------|
| `amazon_nova_project.py` | The main agent — contains `ResearchTools` (4 tools including PDF analysis), `ResearchAgent` (reasoning loop, tool selection, Nova API calls), and interactive CLI |
| `pdf_analysis_examples.py` | 7 runnable examples showing different ways to use PDF analysis: basic analysis, information extraction, PDF comparison, Q&A, batch processing, research workflows, and custom queries |
| `requirements.txt` | Lists all Python packages needed: `boto3`, `botocore`, `python-dotenv`, `PyPDF2` |
| `sample.pdf` | A sample research paper PDF for testing the PDF analysis feature |
| `demo.html` | An animated HTML demo that shows how the agent works — open in any browser |

---

## Technical Details

### Tools Available to the Agent

| Tool | Description | Real/Simulated |
|------|-------------|---------------|
| `analyze_pdf` | Reads PDF files, extracts text from all pages, handles edge cases | **Real** (PyPDF2) |
| `search_web` | Searches for information on a topic | Simulated (placeholder for real API) |
| `fetch_document` | Retrieves document content | Simulated |
| `analyze_data` | Performs data analysis (market, sentiment) | Simulated |

### Amazon Nova API Integration

The agent uses the **Bedrock Converse API** to communicate with Nova:

```python
client = boto3.client('bedrock-runtime', region_name='us-east-1')
response = client.converse(
    modelId="amazon.nova-lite-v1:0",
    messages=[{
        "role": "user",
        "content": [{"text": prompt}]  # Content must be a list of blocks
    }],
    inferenceConfig={
        "maxTokens": 1024,
        "temperature": 0.7
    }
)
```

### Key Design Decisions

- **Lazy client initialization** — The AWS Bedrock client is only created when the first API call is made, not at import time. This means you can import the module and test PDF extraction even without AWS credentials configured.
- **Robust action parsing** — The LLM might return `ACTION: analyze_pdf("file.pdf")` or `ACTION: SEARCH_WEB`. The parser handles all variations using regex cleanup and fuzzy matching.
- **Safe PDF handling** — Handles `None` from `extract_text()` (blank/scanned pages), truncates very long documents to stay within token limits, and provides clear error messages.

---

## Demo

Open `demo.html` in your browser to see an interactive animation of the agent processing three different types of queries:

1. **Web Research** — Asking about agentic AI trends
2. **PDF Analysis** — Reading and analyzing a research paper
3. **Data Analysis** — Market analysis of AI growth

The demo includes an architecture diagram showing the agent's 5-step reasoning loop.

---

## Tech Stack

- **AI Model**: Amazon Nova 2 Lite (`amazon.nova-lite-v1:0`)
- **Cloud Platform**: AWS Bedrock (Converse API)
- **Language**: Python 3
- **PDF Processing**: PyPDF2
- **AWS SDK**: boto3

---

## What Makes This Project Special

| Aspect | Why It Matters |
|--------|---------------|
| **True Agentic Behavior** | The agent reasons, plans, selects tools, and synthesizes — not just prompt-in, text-out |
| **Real PDF Analysis** | Actual file I/O with PyPDF2, not simulated responses |
| **Nova as the Brain** | Amazon Nova 2 Lite drives both the reasoning and synthesis steps |
| **Production-Ready Patterns** | Lazy initialization, error handling, input sanitization |
| **Extensible Design** | Easy to add new tools — just add a method to `ResearchTools` and register it |

---

## Future Improvements

- Connect `search_web` to a real search API (Google, DuckDuckGo, or Bing)
- Add image analysis using Amazon Nova's multimodal capabilities
- Add conversation memory so the agent remembers previous questions
- Support for more document formats (Word, Excel, HTML)
- Deploy as a web application with a proper UI

---

## Built With

Amazon Nova 2 Lite on AWS Bedrock | Python | PyPDF2 | boto3

**#AmazonNova** | Amazon Nova AI Hackathon 2026 | Agentic AI Category

---

*Built by Devika*
