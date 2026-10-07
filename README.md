<div align="center">

# ðŸ§  CogniCode

**Automated code intelligence and quality audits for Python projects.**

CogniCode maps your codebase as a dependency graph, writes pytest tests with AI, runs security scans, and finds duplicate logic. You see all of it in one dashboard in your browser.

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-backend-009688?logo=fastapi&logoColor=white)
![Gemini](https://img.shields.io/badge/Google-Gemini-4285F4?logo=google&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-LLM-F55036)
![pytest](https://img.shields.io/badge/tests-pytest-0A9EDC?logo=pytest&logoColor=white)

</div>

---

## âœ¨ Features

| Module | What it does |
|---|---|
| ðŸ•¸ï¸ **Graph Explorer** | Parses your code into a dependency graph (NetworkX) of files, classes and functions. Includes search, filters and a collapsible sidebar. |
| ðŸ’¥ **Impact Analysis** | Shows which parts of the codebase a file change will affect. |
| ðŸ§ª **AI Test Generation** | Uses Groq to write pytest cases (normal, edge and error cases) for any function. The tests run live in the browser over a WebSocket. |
| ðŸ“Š **QA Ledger** | Keeps a history of every test run, stored in SQLite. |
| ðŸ›¡ï¸ **Security Command Center** | Runs a static security scan (based on Bandit) to catch hard-coded secrets, injection risks, unsafe deserialization and more. |
| ðŸ“ **"What-If" Architect** | Lets you simulate architectural changes before you make them. |
| ðŸ§  **Semantic Knowledge Base** | Lets you search your code in plain English, using Gemini embeddings and FAISS. |
| ðŸ§¬ **Semantic Clone Detector** | Finds functions that do the same thing even when the code looks different. |
| ðŸ“ **AI Synopsis** | Writes a short plain-English summary of any node in the graph. |
| ðŸ“„ **Report Generation** | Exports a full quality-audit report covering graph, tests and security. |
| ðŸ§© **VS Code Extension** | A lightweight client (`vscode-extension/`) that connects your editor to the CogniCode server. |

---

## ðŸ—ï¸ Architecture

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”      HTTP / WebSocket      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  Dashboard (browser) â”‚ â—„â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â–º â”‚  CogniServer (FastAPI)       â”‚
â”‚  dashboard/index.htmlâ”‚                            â”‚  â”œâ”€ graph_engine.py  (AST â†’   â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜                            â”‚  â”‚   NetworkX graph)          â”‚
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”                            â”‚  â”œâ”€ scanner.py  (security)    â”‚
â”‚  VS Code extension   â”‚ â—„â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â–º â”‚  â”œâ”€ clone_detector.py (Gemini â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜                            â”‚  â”‚   + FAISS)                 â”‚
                                                    â”‚  â”œâ”€ database.py  (SQLite)     â”‚
                                                    â”‚  â””â”€ test generator  (Groq)    â”‚
                                                    â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## ðŸš€ Getting Started

### Prerequisites

- Python 3.9 or newer
- A [Google Gemini API key](https://aistudio.google.com/app/apikey), used for AI summaries and semantic search
- A [Groq API key](https://console.groq.com/keys), used for AI test generation

### 1. Clone the repo

```bash
git clone https://github.com/larissamartis/cognicode-ai-code-intelligence.git
cd cognicode-ai-code-intelligence
```

### 2. Create a virtual environment

```bash
python -m venv myenv

# Windows
myenv\Scripts\activate

# macOS / Linux
source myenv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure your API keys

```bash
copy .env.example .env      # Windows
cp .env.example .env        # macOS / Linux
```

Open `.env` and fill in your keys:

```env
GEMINI_API_KEY=your_actual_gemini_key
GROQ_API_KEY=your_actual_groq_key
```

> ðŸ”’ `.env` is listed in `.gitignore`. **Never commit your real API keys.**

### 5. Start the server

```bash
python -m uvicorn cogniserver.main:app --port 8000
```

On Windows you can also double-click **`run_dashboard.bat`**.

### 6. Open the dashboard

Go to **http://localhost:8000** in your browser. ðŸŽ‰

---

## ðŸ”Œ API Overview

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Serves the dashboard |
| `GET` | `/graph` | Returns the full dependency graph |
| `POST` | `/refresh` | Rebuilds the graph from source |
| `POST` | `/impact/file` | Returns the impact of changing a file |
| `GET` | `/summary/{node_id}` | Returns an AI summary of a function or class |
| `POST` | `/run_test` | Generates and runs tests for a node |
| `WS` | `/ws/run/{node_id}` | Streams test generation and run output live |
| `GET` | `/tests/history` | Returns past test results |
| `GET` | `/clone/scan` | Finds semantic code clones |
| `GET` | `/knowledge/search?q=` | Runs a natural-language code search |
| `GET` | `/report/data` | Returns the full audit report data |

FastAPI also gives you interactive API docs at **http://localhost:8000/docs**.

---

## ðŸ“ Project Structure

```
cognicode/
â”œâ”€â”€ cogniserver/          # FastAPI backend: graph engine, scanner, clone detector, DB
â”œâ”€â”€ dashboard/            # Single-page web dashboard
â”œâ”€â”€ cogni/                # Static security analysis engine (based on Bandit)
â”œâ”€â”€ vscode-extension/     # VS Code client
â”œâ”€â”€ test_repo/            # Sample e-commerce codebase used as the analysis target
â”œâ”€â”€ generated_tests/      # AI-generated pytest files
â”œâ”€â”€ examples/             # Small example modules
â”œâ”€â”€ tests/                # Unit and functional tests for the security engine
â”œâ”€â”€ test_generator_groq.py
â”œâ”€â”€ gemini_client.py
â”œâ”€â”€ groq_client.py
â”œâ”€â”€ requirements.txt
â””â”€â”€ .env.example
```

> ðŸ’¡ By default CogniCode analyses the sample project in `test_repo/`. To analyse your own code, change `ROOT_DIR` in `cogniserver/main.py`.

---

## ðŸ› ï¸ Tech Stack

**Backend:** Python Â· FastAPI Â· Uvicorn Â· NetworkX Â· SQLite
**AI:** Google Gemini Â· Groq Â· FAISS
**Quality and security:** pytest Â· coverage Â· Bandit
**Frontend:** HTML Â· CSS Â· JavaScript
**Editor:** VS Code extension (TypeScript)

---

## ðŸ¤ Contributing

Pull requests are welcome. For bigger changes, please open an issue first so we can discuss them.

1. Fork the repo
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add my feature"`
4. Push the branch and open a pull request

---

## ðŸ™ Acknowledgements

- The security scanning engine in `cogni/` is adapted from **[Bandit](https://github.com/PyCQA/bandit)** by PyCQA, licensed under Apache 2.0.
- AI features are powered by [Google Gemini](https://ai.google.dev/) and [Groq](https://groq.com/).
