# 🚀 RAG RepoPilot AI

### Repository-Grounded AI Assistant for Understanding and Querying Codebases

**RAG RepoPilot AI** is an AI-powered repository assistant that uses **Retrieval-Augmented Generation (RAG)** to understand a software codebase and answer developer questions using relevant source-code context instead of relying only on the model's general knowledge.

It allows developers to connect a repository, index its source code, and ask questions about the project such as:

* Where is a particular feature implemented?
* How does a specific function work?
* Where is authentication handled?
* Which files are responsible for a particular operation?
* How are different components connected?
* What changes are required to implement a new feature?

The goal is to make AI responses **repository-aware, context-grounded, and less prone to hallucination**.

---

## 🌐 Live Demo

**Live Application:**
https://rag-repo-pilot-ai.vercel.app/

---

## ✨ Features

### 📂 Repository Ingestion

* Clone and process a Git repository.
* Extract relevant source files from the repository.
* Ignore unnecessary files such as `.git` content.
* Support common project files including:

  * Python
  * Markdown
  * YAML
  * TOML
  * JSON

### 🔎 Intelligent Code Search

The system converts repository content into vector embeddings and stores them in a vector database.

This allows semantic queries such as:

> "Where is the authentication logic?"

instead of requiring an exact filename or keyword.

### 🧠 Retrieval-Augmented Generation

The application follows a RAG pipeline:

```text
Repository
     ↓
Repository Ingestion
     ↓
File Extraction
     ↓
Text Chunking
     ↓
Embedding Generation
     ↓
Vector Database
     ↓
Semantic Retrieval
     ↓
Relevant Repository Context
     ↓
LLM
     ↓
Grounded Answer
```

### 📚 Repository-Grounded Answers

Instead of asking the LLM to answer purely from its pretrained knowledge, RepoPilot retrieves relevant repository content and provides it as context.

This helps the assistant answer questions based on the actual codebase.

### 💻 Developer-Friendly Interface

The frontend provides an interactive interface for interacting with the repository assistant without requiring developers to manually search through hundreds or thousands of files.

---

# 🏗️ Architecture

```text
                    ┌──────────────────────┐
                    │      Developer       │
                    │  Repository Question │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      Frontend        │
                    │   Web Application    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       Backend        │
                    │      API Server      │
                    └──────────┬───────────┘
                               │
                ┌──────────────┴──────────────┐
                │                             │
                ▼                             ▼
      ┌──────────────────┐          ┌──────────────────┐
      │ Repository       │          │ Query Processing │
      │ Ingestion        │          │                  │
      └────────┬─────────┘          └────────┬─────────┘
               │                             │
               ▼                             ▼
      ┌──────────────────┐          ┌──────────────────┐
      │ Text Chunking    │          │ Query Embedding  │
      └────────┬─────────┘          └────────┬─────────┘
               │                             │
               ▼                             ▼
      ┌──────────────────────────────────────────────┐
      │                 Pinecone                     │
      │             Vector Database                  │
      └──────────────────────┬───────────────────────┘
                             │
                             ▼
                    ┌──────────────────────┐
                    │ Relevant Code / Docs │
                    │       Context        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     LLM / RAG        │
                    │    Answer Engine     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     Final Answer     │
                    └──────────────────────┘
```

---

# 🧩 Project Structure

```text
RAG_RepoPilot-AI/
│
├── backend/
│   └── Backend API and RAG logic
│
├── frontend/
│   └── Frontend web application
│
├── Ingestion_and_Indexing.py
│   └── Repository cloning, chunking,
│       embedding generation and indexing
│
├── .gitignore
│
└── README.md
```

---

# 🛠️ Tech Stack

### Frontend

* React
* JavaScript
* Vite
* CSS

### Backend

* Python
* FastAPI

### AI / RAG

* Retrieval-Augmented Generation (RAG)
* Sentence Transformers
* `all-MiniLM-L6-v2`
* LangChain text splitters

### Vector Database

* Pinecone

### Repository Processing

* GitPython

### Environment Management

* Python `venv`
* `python-dotenv`

---

# 🔄 RAG Pipeline

RepoPilot processes a repository through several stages.

## 1. Repository Ingestion

The repository is cloned locally using Git.

```python
Repo.clone_from(REPO_URL, LOCAL_PATH)
```

The system then walks through the repository and identifies supported files.

---

## 2. File Filtering

Only relevant project files are processed.

Current supported extensions include:

```text
.py
.md
.yaml
.toml
.json
```

Git metadata and irrelevant files are skipped.

---

## 3. Text Chunking

Large files are divided into smaller chunks using LangChain's `RecursiveCharacterTextSplitter`.

Example configuration:

```text
Chunk size: 1000
Chunk overlap: 100
```

Chunking makes it possible to retrieve only the relevant portions of large files.

---

## 4. Embedding Generation

Each chunk is converted into a vector representation using:

```text
all-MiniLM-L6-v2
```

The generated embeddings have **384 dimensions**.

---

## 5. Vector Storage

The embeddings are uploaded to **Pinecone**.

Each vector contains metadata such as:

```text
filename
text
chunk_index
page_content
```

This allows retrieved results to be traced back to the original repository content.

---

## 6. Retrieval

When a developer asks a question, the query can be converted into an embedding and compared against the repository vectors.

The most relevant chunks are retrieved.

---

## 7. Grounded Generation

The retrieved repository context is passed to the AI generation layer.

The model can therefore produce an answer based on the actual repository rather than guessing from generic programming knowledge.

---

# ⚙️ Running Locally

Follow these steps to run RepoPilot AI on your own machine.

## Prerequisites

Install the following:

* Python 3.10+
* Node.js 18+
* npm
* Git
* Pinecone account/API key
* Required LLM API key used by the backend

Check your installations:

```bash
python --version
node --version
npm --version
git --version
```

---

# 1️⃣ Clone the Repository

```bash
git clone https://github.com/anandd25/RAG_RepoPilot-AI.git
```

Move into the project:

```bash
cd RAG_RepoPilot-AI
```

---

# 2️⃣ Create Python Virtual Environment

### Windows

```bash
python -m venv .venv
```

Activate it:

### Windows CMD

```bash
.venv\Scripts\activate
```

### Windows PowerShell

```powershell
.venv\Scripts\Activate.ps1
```

### macOS / Linux

```bash
source .venv/bin/activate
```

---

# 3️⃣ Install Backend Dependencies

Navigate to the backend:

```bash
cd backend
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

If your backend uses a `pyproject.toml`, install it with:

```bash
pip install -e .
```

Return to the project root:

```bash
cd ..
```

---

# 4️⃣ Configure Environment Variables

Create a `.env` file in the appropriate backend/project directory.

Example:

```env
PINECONE_API_KEY=your_pinecone_api_key
```

Add the LLM/API credentials required by your backend:

```env
LLM_API_KEY=your_llm_api_key
```

> **Important:** Never commit your real API keys to GitHub.

Your `.gitignore` should include:

```text
.env
.venv/
__pycache__/
```

---

# 5️⃣ Configure Pinecone

Create a Pinecone index corresponding to the configuration used by the ingestion script.

The repository's ingestion script uses:

```text
all-MiniLM-L6-v2
```

which produces:

```text
384-dimensional embeddings
```

Therefore, the Pinecone index should be configured with the appropriate vector dimension.

The ingestion script currently references:

```python
index_name = "fastapi-repo-index"
```

Make sure this index exists in your Pinecone project before running the indexing process.

---

# 6️⃣ Run Repository Ingestion and Indexing

From the project root:

```bash
python Ingestion_and_Indexing.py
```

The script performs the following:

```text
Clone Repository
       ↓
Scan Files
       ↓
Filter Supported Extensions
       ↓
Read File Contents
       ↓
Split Into Chunks
       ↓
Generate Embeddings
       ↓
Upload Vectors to Pinecone
```

You should see output similar to:

```text
Cloning repository...
Processing files...
Uploaded batch including: ...
Uploaded batch including: ...
Indexing complete!
```

The indexing script currently uses the FastAPI repository as its example source repository.

You can modify:

```python
REPO_URL
```

and:

```python
LOCAL_PATH
```

to index another repository.

---

# 7️⃣ Start the Backend

Open a new terminal.

Activate the virtual environment again if required:

```bash
.venv\Scripts\activate
```

Navigate to the backend:

```bash
cd backend
```

Start the FastAPI server using the entry point defined by your backend.

For example:

```bash
uvicorn main:app --reload
```

If your backend entry point is located elsewhere, use the corresponding module path.

The backend will normally be available at:

```text
http://localhost:8000
```

FastAPI documentation:

```text
http://localhost:8000/docs
```

---

# 8️⃣ Start the Frontend

Open another terminal.

From the project root:

```bash
cd frontend
```

Install frontend dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Vite will provide a local URL similar to:

```text
http://localhost:5173
```

Open that URL in your browser.

---

# 🖥️ Running the Complete Application

You will typically need **three processes** during local development:

### Terminal 1 — Backend

```bash
cd RAG_RepoPilot-AI/backend
uvicorn main:app --reload
```

### Terminal 2 — Frontend

```bash
cd RAG_RepoPilot-AI/frontend
npm install
npm run dev
```

### Terminal 3 — Optional Indexing

Run this when you need to create/update the repository vector index:

```bash
cd RAG_RepoPilot-AI
python Ingestion_and_Indexing.py
```

Then open:

```text
http://localhost:5173
```

---

# 🔐 Environment Variables

Do not commit secrets.

Example `.env`:

```env
PINECONE_API_KEY=your_pinecone_api_key
LLM_API_KEY=your_llm_api_key
```

For deployment, configure these values through the hosting provider's environment-variable settings.

---

# 🧪 Example Questions

Once the repository has been indexed, you can ask questions such as:

```text
Where is the authentication logic implemented?
```

```text
How does the API handle errors?
```

```text
Where is the database connection initialized?
```

```text
Which file contains the main application entry point?
```

```text
How does the frontend communicate with the backend?
```

```text
Where should I add a new API endpoint?
```

The objective is to answer these questions using the repository's own source code as the primary context.

---

# 🎯 Why RepoPilot?

Traditional LLM-based coding assistants can generate technically valid code that does not match the architecture of an existing project.

RepoPilot focuses on **repository grounding**.

Instead of:

```text
Question
   ↓
LLM
   ↓
Generic Answer
```

RepoPilot follows:

```text
Question
   ↓
Semantic Retrieval
   ↓
Relevant Repository Code
   ↓
Context-Aware Generation
   ↓
Repository-Grounded Answer
```

This makes the system particularly useful for:

* Understanding unfamiliar codebases
* Developer onboarding
* Debugging
* Code navigation
* Architecture exploration
* Maintaining consistency with existing project patterns
* Working with large repositories

---

# 🚀 Future Improvements

Potential future improvements include:

* 🔗 GitHub OAuth integration
* 📊 Repository architecture visualization
* 🕸️ Dependency graph generation
* 🔍 Symbol-level code retrieval
* 🧠 Multi-agent repository investigation
* 💬 Conversational repository memory
* 🐛 AI-assisted debugging
* 🔄 Automatic repository re-indexing
* 🔀 Pull-request analysis
* 📝 AI-generated documentation
* 🧪 Automated test generation
* 🔐 Repository-level access control
* ⚡ Incremental indexing instead of full re-indexing

---

# 📌 Limitations

* Retrieval quality depends on the quality of chunking and embeddings.
* Large repositories may require more indexing time and vector storage.
* API usage may incur costs depending on the configured AI/vector services.
* The current ingestion implementation focuses on a defined set of file extensions.
* Generated responses should still be reviewed by developers before applying changes.

---


