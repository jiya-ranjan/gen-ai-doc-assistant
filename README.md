# 📄 Gen-AI Document Assistant

A **Retrieval-Augmented Generation (RAG)** web application that lets you upload documents and ask natural language questions — powered by **Groq LLaMA 3**, **Prompt Engineering**, and **FastAPI**.

> Built as part of exploring Generative AI application development with real-world data pipelines.

---

## 🚀 Live Demo

Upload any PDF or TXT → Ask a question → Get AI-powered answers with source references.

![Demo](https://img.shields.io/badge/Status-Live-brightgreen)
![Python](https://img.shields.io/badge/Python-3.10+-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-0.111-green)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA3-orange)

---

## 🧠 What is RAG?

**Retrieval-Augmented Generation (RAG)** is a technique where:
1. Documents are split into chunks and indexed
2. When a question is asked, the most relevant chunks are **retrieved**
3. Those chunks are passed as context to an **LLM** to generate an accurate answer

This reduces hallucination and keeps answers grounded in your actual documents.

---

## ⚙️ Tech Stack

| Layer | Technology |
|-------|-----------|
| LLM | Groq API (LLaMA 3.3 70B) |
| Backend | FastAPI + Uvicorn |
| Document Processing | PyPDF, custom chunking pipeline |
| Similarity Search | TF-based cosine similarity |
| Frontend | Vanilla HTML/CSS/JS |
| Containerization | Docker |

---

## 🔬 Prompt Engineering Techniques

Two techniques implemented and switchable from the UI:

### 1. Chain-of-Thought (CoT) Prompting
Instructs the LLM to reason step-by-step before answering. Better for complex, multi-part questions.

```
Steps:
1. Find relevant info in context.
2. Reason through the question.
3. Give a clear answer based only on context.
Final Answer: ...
```

### 2. Few-Shot Prompting
Provides the LLM with 2 worked examples showing expected input-output format. Better for factual, direct lookups.

```
Example 1:
Context: "The company was founded in 2010."
Question: When was it founded?
Answer: The company was founded in 2010.
```

---

## 📁 Project Structure

```
gen-ai-doc-assistant/
├── backend/
│   ├── main.py          # FastAPI routes (upload, query, list, delete)
│   └── rag_pipeline.py  # RAG logic + Prompt Engineering
├── frontend/
│   └── index.html       # Web UI
├── data/                # Uploaded documents stored here
├── requirements.txt
├── Dockerfile
└── README.md
```

---

## 🛠️ Setup & Run

### Prerequisites
- Python 3.10+
- Groq API key (free at [console.groq.com](https://console.groq.com))

### Installation

```bash
# 1. Clone the repo
git clone https://github.com/jiya-ranjan/gen-ai-doc-assistant.git
cd gen-ai-doc-assistant

# 2. Create virtual environment
python -m venv venv

# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set API key
# Windows CMD:
set GROQ_API_KEY=your_groq_api_key_here
# Mac/Linux:
export GROQ_API_KEY=your_groq_api_key_here

# 5. Run server
cd backend
uvicorn main:app --reload --port 8000
```

### Open in browser
**http://localhost:8000**

---

## 🐳 Run with Docker

```bash
docker build -t gen-ai-doc-assistant .
docker run -p 8000:8000 -e GROQ_API_KEY=your_key_here gen-ai-doc-assistant
```

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/upload` | Upload PDF or TXT document |
| POST | `/query` | Ask a question about uploaded docs |
| GET | `/documents` | List uploaded documents |
| DELETE | `/documents` | Clear all documents |

---

## 💡 How It Works

```
User uploads PDF/TXT
        ↓
Document split into 400-word chunks
        ↓
Each chunk stored with TF-based embedding
        ↓
User asks question
        ↓
Top 4 most similar chunks retrieved (cosine similarity)
        ↓
Context + Prompt (CoT or Few-Shot) sent to Groq LLaMA 3
        ↓
Answer returned with source references
```

---

## 🎯 Key Learnings

- Implemented **RAG pipeline** from scratch — chunking, indexing, retrieval, generation
- Applied **two prompt engineering techniques** with measurable impact on answer quality
- Built and deployed a **REST API** with FastAPI handling file uploads and async requests
- Handled **real-world data preprocessing** — PDF parsing, text cleaning, chunk overlap strategy
- Integrated **LLM API** (Groq) for production-grade text generation

---

## 📬 Contact

**Jiya Ranjan** — [LinkedIn](https://www.linkedin.com/in/jiya-ranjan55) | [GitHub](https://github.com/jiya-ranjan)
