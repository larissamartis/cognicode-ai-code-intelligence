<div align="center">

# 🧠 CogniCode: AI Code Intelligence

**Automated code intelligence and quality audits for Python projects.**

CogniCode maps your codebase as a dependency graph, writes pytest tests with AI, runs security scans, and finds duplicate logic. You see all of it in one dashboard in your browser.

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-backend-009688?logo=fastapi&logoColor=white)
![Gemini](https://img.shields.io/badge/Google-Gemini-4285F4?logo=google&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-LLM-F55036)
![pytest](https://img.shields.io/badge/tests-pytest-0A9EDC?logo=pytest&logoColor=white)

</div>

---

## ✨ Features

| Module | What it does |
|---|---|
| 🕸️ **Graph Explorer** | Parses your code into a dependency graph (NetworkX) of files, classes and functions. Includes search, filters and a collapsible sidebar. |
| 💥 **Impact Analysis** | Shows which parts of the codebase a file change will affect. |
| 🧪 **AI Test Generation** | Uses Groq to write pytest cases (normal, edge and error cases) for any function. The tests run live in the browser over a WebSocket. |
| 📊 **QA Ledger** | Keeps a history of every test run, stored in SQLite. |
| 🛡️ **Security Command Center** | Runs a static security scan (based on Bandit) to catch hard-coded secrets, injection risks, unsafe deserialization and more. |
| 📐 **"What-If" Architect** | Lets you simulate architectural changes before you make them. |
| 🧠 **Semantic Knowledge Base** | Lets you search your code in plain English, using Gemini embeddings and FAISS. |
| 🧬 **Semantic Clone Detector** | Finds functions that do the same thing even when the code looks different. |
| 📝 **AI Synopsis** | Writes a short plain-English summary of any node in the graph. |
| 📄 **Report Generation** | Exports a full quality-audit report covering graph, tests and security. |
| 🧩 **VS Code Extension** | A lightweight client (`vscode-extension/`) that connects your editor to the CogniCode server. |

---

## 🏗️ Architecture

```
┌──────────────────────┐      HTTP / WebSocket      ┌──────────────────────────────┐
│  Dashboard (browser) │ ◄────────────────────────► │  CogniServer (FastAPI)       │
│  dashboard/index.html│                            │  ├─ graph_engine.py  (AST →   │
└──────────────────────┘                            │  │   NetworkX graph)          │
┌──────────────────────┐                            │  ├─ scanner.py  (security)    │
│  VS Code extension   │ ◄────────────────────────► │  ├─ clone_detector.py (Gemini │
└──────────────────────┘                            │  │   + FAISS)                 │
                                                    │  ├─ database.py  (SQLite)     │
                                                    │  └─ test generator  (Groq)    │
                                                    └──────────────────────────────┘
```

---

## 🚀 Getting Started

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

> 🔒 `.env` is listed in `.gitignore`. **Never commit your real API keys.**

### 5. Start the server

```bash
python -m uvicorn cogniserver.main:app --port 8000
```

On Windows you can also double-click **`run_dashboard.bat`**.

### 6. Open the dashboard

Go to **http://localhost:8000** in your browser. 🎉

---

## 🔌 API Overview

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

## 📁 Project Structure

```
cognicode-ai-code-intelligence/
├── cogniserver/          # FastAPI backend: graph engine, scanner, clone detector, DB
├── dashboard/            # Single-page web dashboard
├── cogni/                # Static security analysis engine (based on Bandit)
├── vscode-extension/     # VS Code client
├── test_repo/            # Sample e-commerce codebase used as the analysis target
├── generated_tests/      # AI-generated pytest files
├── examples/             # Small example modules
├── tests/                # Unit and functional tests for the security engine
├── test_generator_groq.py
├── gemini_client.py
├── groq_client.py
├── requirements.txt
└── .env.example
```

> 💡 By default CogniCode analyses the sample project in `test_repo/`. To analyse your own code, change `ROOT_DIR` in `cogniserver/main.py`.

---

## 🛠️ Tech Stack

**Backend:** Python · FastAPI · Uvicorn · NetworkX · SQLite
**AI:** Google Gemini · Groq · FAISS
**Quality and security:** pytest · coverage · Bandit
**Frontend:** HTML · CSS · JavaScript
**Editor:** VS Code extension (TypeScript)

---

## 🤝 Contributing

Pull requests are welcome. For bigger changes, please open an issue first so we can discuss them.

1. Fork the repo
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add my feature"`
4. Push the branch and open a pull request

---

## 🙏 Acknowledgements

- The security scanning engine in `cogni/` is adapted from **[Bandit](https://github.com/PyCQA/bandit)** by PyCQA, licensed under Apache 2.0.
- AI features are powered by [Google Gemini](https://ai.google.dev/) and [Groq](https://groq.com/).
