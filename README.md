# Gen-AI Document Assistant

A Retrieval-Augmented Generation (RAG) web application powered by **LangChain**, **OpenAI GPT-3.5**, and **FastAPI**.

Upload PDF/TXT documents and ask natural language questions — the app retrieves relevant chunks and generates accurate answers using two prompt engineering techniques: **Chain-of-Thought** and **Few-Shot Prompting**.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| LLM | OpenAI GPT-3.5-turbo |
| RAG Framework | LangChain |
| Vector Store | FAISS (in-memory) |
| Embeddings | OpenAI text-embedding-ada-002 |
| Backend | FastAPI + Uvicorn |
| Frontend | Vanilla HTML/CSS/JS |
| Containerization | Docker |

---

## Project Structure

```
gen-ai-doc-assistant/
├── backend/
│   ├── main.py          # FastAPI routes
│   └── rag_pipeline.py  # LangChain RAG logic + Prompt Engineering
├── frontend/
│   └── index.html       # Web UI
├── data/                # Uploaded documents stored here
├── requirements.txt
├── Dockerfile
└── README.md
```

---

## Setup & Run (Without Docker)

### Step 1 — Prerequisites

- Python 3.10 or above installed
- OpenAI API key (get from https://platform.openai.com/api-keys)

### Step 2 — Clone / Download the project

```bash
git clone https://github.com/jiya-ranjan/gen-ai-doc-assistant.git
cd gen-ai-doc-assistant
```

### Step 3 — Create virtual environment

```bash
python -m venv venv

# Windows:
venv\Scripts\activate

# Mac/Linux:
source venv/bin/activate
```

### Step 4 — Install dependencies

```bash
pip install -r requirements.txt
```

### Step 5 — Set your OpenAI API Key

**Windows (Command Prompt):**
```cmd
set OPENAI_API_KEY=sk-your-key-here
```

**Windows (PowerShell):**
```powershell
$env:OPENAI_API_KEY="sk-your-key-here"
```

**Mac/Linux:**
```bash
export OPENAI_API_KEY="sk-your-key-here"
```

### Step 6 — Run the server

```bash
cd backend
uvicorn main:app --reload --port 8000
```

### Step 7 — Open in browser

Go to: **http://localhost:8000**

---

## Setup & Run (With Docker)

### Step 1 — Build the image

```bash
docker build -t gen-ai-doc-assistant .
```

### Step 2 — Run the container

```bash
docker run -p 8000:8000 -e OPENAI_API_KEY=sk-your-key-here gen-ai-doc-assistant
```

### Step 3 — Open in browser

Go to: **http://localhost:8000**

---

## How to Use

1. **Upload a document** — Click the upload zone, select a PDF or TXT file, click "Upload"
2. **Ask a question** — Type your question in the text box
3. **Choose prompt technique:**
   - ✅ Checked = **Chain-of-Thought** (thinks step-by-step, better for complex questions)
   - ☐ Unchecked = **Few-Shot** (uses examples, better for factual lookups)
4. Click **Ask** and get your answer with source references

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/upload` | Upload a PDF or TXT document |
| POST | `/query` | Ask a question about uploaded docs |
| GET | `/documents` | List uploaded documents |
| DELETE | `/documents` | Clear all documents |

### Example API call (curl):

```bash
# Upload a file
curl -X POST http://localhost:8000/upload \
  -F "file=@my_document.pdf"

# Ask a question
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"question": "What is the main topic?", "use_chain_of_thought": true}'
```

---

## Key Concepts Implemented

### 1. RAG (Retrieval-Augmented Generation)
Documents are split into 500-token chunks → embedded using OpenAI embeddings → stored in FAISS vector store. On each query, top-4 most similar chunks are retrieved and passed to the LLM as context.

### 2. Chain-of-Thought Prompting
The prompt instructs the LLM to reason step-by-step before answering, reducing hallucinations on complex queries.

### 3. Few-Shot Prompting
The prompt includes 2 worked examples showing the LLM the expected input-output format, improving consistency on factual lookups.

### 4. Data Preprocessing Pipeline
Uploaded files go through: loading → chunking (RecursiveCharacterTextSplitter) → embedding → FAISS indexing — before any query is answered.
