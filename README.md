 Multi-Agent Research & Evidence-Based Report Generator

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-brightgreen)]()

A modular, production-ready research assistant that orchestrates planning, evidence gathering, verification, and report writing using **LangGraph**, **FastAPI**, and **Streamlit**.

It autonomously generates structured, citation-backed, verifiable research reports on any user-defined topic.

---

# ✨ Features

- 🤖 Multi-Agent Orchestration (Planner, Researcher, Verifier, Writer)
- 📚 Evidence-Based Research with RAG (ChromaDB)
- ✅ Automatic Verification & Coverage Scoring
- 📄 Structured Reports (Markdown + PDF)
- 🔧 Modular & Extensible Architecture
- 💾 Persistent Embeddings & State Snapshots
- 🎯 Multi-LLM Support (OpenAI, Groq, Google Gemini)
- 🌐 REST API + Web UI

---

# 🏗 Architecture

## Workflow Pipeline

```
User Topic
    ↓
Planner Agent
    ↓
Researcher Agent
    ↓
Verifier Agent
    ↓
Writer Agent
    ↓
Structured Report (MD/PDF)
```

## Technology Stack

| Component     | Technology               | Purpose                |
| ------------- | ------------------------ | ---------------------- |
| Orchestration | LangGraph                | State machine workflow |
| API           | FastAPI                  | REST API               |
| Frontend      | Streamlit                | Interactive UI         |
| LLM           | OpenAI / Groq / Gemini   | Language models        |
| Embeddings    | sentence-transformers    | Semantic encoding      |
| Vector DB     | ChromaDB                 | Persistent retrieval   |
| Search        | DuckDuckGo               | Web search             |
| Scraping      | BeautifulSoup + requests | Content extraction     |
| Export        | ReportLab                | PDF generation         |

---

# 📦 Project Structure

```
.
├── backend/
│   ├── app/
│   │   ├── agents/               # Agent logic
│   │   ├── api/                  # FastAPI routes
│   │   ├── config/               # Settings & env handling
│   │   ├── core/                 # Shared utilities
│   │   ├── data/
│   │   │   ├── chroma/           # ChromaDB persistence
│   │   │   └── state/            # JSON state snapshots per report
│   │   ├── graph/                # LangGraph workflow definition
│   │   ├── schemas/              # Pydantic models
│   │   ├── tools/                # Search, loader, embedding tools
│   │   ├── outputs/              # ← Generated Markdown + PDF reports go here
│   │   └── main.py               # FastAPI entry point
│   └── tests/                    # Tests
├── frontend/
│   └── app.py                    # Streamlit UI
├── sample-scr/                   # Sample screenshots / example outputs
├── .env_example
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

# 🚀 Quick Start

## 1️⃣ Clone Repository

```bash
git clone https://github.com/Raghul-S-Coder/Multi-Agent-Research-Evidence-Based-Report-Generator.git
cd Multi-Agent-Research-Evidence-Based-Report-Generator
```

## 2️⃣ Create Virtual Environment

```bash
python -m venv .venv
source .venv/bin/activate     # macOS/Linux
.venv\Scripts\activate        # Windows
```

## 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

## 4️⃣ Configure Environment

```bash
cp .env_example .env
```

Add at least one API key:

```bash
OPENAI_API_KEY=
# or
GROQ_API_KEY=
# or
GOOGLE_API_KEY=
```

## 5️⃣ Start Backend

```bash
cd backend
python -m uvicorn app.main:app --reload
```

API:
`http://localhost:8000`
Docs:
`http://localhost:8000/docs`

## 6️⃣ Start Frontend

```bash
python -m streamlit run frontend/app.py
```

UI:
`http://localhost:8501`

---

# 🔧 Configuration

## LLM Priority Order

1. OpenAI
2. Groq
3. Google Gemini

Override model:

```bash
OPENAI_MODEL=gpt-4o-mini
GROQ_MODEL=llama-3.1-8b-instant
GOOGLE_MODEL=gemini-2.0-flash
```

---

## Search & Retrieval

| Variable           | Default |
| ------------------ | ------- |
| MAX_SEARCH_RESULTS | 4       |
| MIN_TEXT_LENGTH    | 800     |
| CHUNK_SIZE         | 800     |
| CHUNK_OVERLAP      | 120     |
| TOP_K_EVIDENCE     | 5       |

---

## Verification

| Variable             | Default |
| -------------------- | ------- |
| COVERAGE_THRESHOLD   | 0.7     |
| MIN_SOURCE_DIVERSITY | 0.3     |
| MAX_RETRIES          | 1       |

---

## Storage

| Variable           | Default                 |
| ------------------ | ----------------------- |
| CHROMA_PERSIST_DIR | backend/app/data/chroma |
| STATE_DIR          | backend/app/data/state  |
| OUTPUT_DIR         | backend/app/outputs     |
| EXPORT_PDF         | 1                       |

---

# 📡 API Reference

Base URL:

```
http://localhost:8000
```

---

## 1️⃣ Generate Research Report

**POST** `/generate-report`

Request:

```json
{
  "topic": "Applications of artificial intelligence in healthcare"
}
```

Response:

```json
{
  "topic": "...",
  "report": "...",
  "citations": [],
  "coverage_score": 0.85,
  "verified": true,
  "report_id": "uuid",
  "outputs": {
    "markdown": "...",
    "pdf": "..."
  },
  "research_status": {
    "planning": "completed",
    "research": "completed",
    "verification": "completed",
    "writing": "completed"
  }
}
```

---

## 2️⃣ Report History

**GET** `/history`

Returns list of all reports.

---

## 3️⃣ Retrieve Specific Report

**GET** `/history/{name}`

Returns markdown content and PDF path.

---

## 4️⃣ Health Check

**GET** `/health`

```json
{
  "status": "ok"
}
```

---

# 🧠 Agents

### Planner

- Decomposes topic into structured outline.

### Researcher

- Performs web search.
- Scrapes content.
- Generates embeddings.
- Stores in ChromaDB.

### Verifier

- Computes coverage score.
- Evaluates source diversity.
- Applies trust scoring.

### Writer

- Synthesizes final markdown report.
- Generates PDF.

---

# 🗂 State Management

State snapshots saved at:

```
backend/app/data/state/
```

Files:

```
{report_id}_planner.json
{report_id}_researcher.json
{report_id}_verifier.json
{report_id}_writer.json
```

---

# 📄 Report Output

Generated reports stored in:

```
backend/app/outputs/
```

Formats:

- Markdown (.md)
- PDF (.pdf)

---

# 🧪 Testing

```bash
pytest backend/tests/
pytest backend/tests/ --cov=backend/app
```

---

# ⚡ Performance Tuning

## Optimize for Speed

```bash
MAX_SEARCH_RESULTS=2
CHUNK_SIZE=500
TOP_K_EVIDENCE=3
```

## Optimize for Quality

```bash
MAX_SEARCH_RESULTS=6
CHUNK_SIZE=1000
TOP_K_EVIDENCE=10
COVERAGE_THRESHOLD=0.8
```

## Optimize for Cost

```bash
GROQ_MODEL=llama-3.1-8b-instant
GOOGLE_MODEL=gemini-2.0-flash
```

---

# 🛡 Security Considerations

- Never commit `.env`
- Use HTTPS in production
- Implement authentication
- Enable rate limiting
- Rotate API keys
- Log API access
- Use secrets manager
- Backup state & outputs

---

# 🐳 Docker Deployment

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY backend ./backend
COPY frontend ./frontend
EXPOSE 8000 8501
CMD ["sh", "-c", "python -m uvicorn backend.app.main:app --host 0.0.0.0 --port 8000 & python -m streamlit run frontend/app.py --server.port 8501 --server.address 0.0.0.0"]
```

Build:

```bash
docker build -t research-generator .
docker run -p 8000:8000 -p 8501:8501 -e OPENAI_API_KEY=your_key research-generator
```

---

# 🔍 Troubleshooting

### No API Key

Ensure at least one provider key is set.

### Port in Use

Use different port with `--port`.

### ChromaDB Issues

Clear:

```bash
rm -rf backend/app/data/chroma
```

### Frontend Cannot Connect

Check:

```bash
curl http://localhost:8000/health
```

---

# 🗺 Roadmap

- Streaming responses
- Batch reports
- Academic DB integration
- Agent pipeline builder
- Version comparison
- Multi-language support

---

# 🤝 Contributing

1. Fork
2. Create feature branch
3. Commit changes
4. Push
5. Create Pull Request

Follow:

- PEP 8
- Write tests
- Update documentation

---




---

**Version:** 0.1.0

**Last Updated:** February 2026


Happy coding :)
