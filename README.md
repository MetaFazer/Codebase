# CodexAI

An AI-powered codebase exploration and chat engine.

CodexAI (formerly Codebase OS) is a full-stack application designed to explore, analyze, and interact with GitHub repositories using natural language. It clones codebases locally, processes and indexes their content using vector embeddings, and enables users to ask specific questions about the architecture, logic, or implementation of the project.

## Features

- **Repository Cloning:** Clones any public GitHub repository locally via URL.
- **Interactive File Explorer:** Navigates the directory structure of the cloned repository with a responsive, tree-based file explorer interface.
- **File Viewer:** Displays the contents of source files with integrated syntax highlighting.
- **Semantic Code Embeddings:** Automatically processes, chunks, and indexes source code files into a local, persistent vector database (ChromaDB) using the Cohere API.
- **Codebase Chat Context:** Queries the vector database for relevant code blocks matching user questions, then uses Cohere's language model to answer questions with grounded codebase context.

## Tech Stack

### Frontend
- React 19
- Material UI (MUI)
- Framer Motion (Animations)
- React Syntax Highlighter

### Backend
- Python & FastAPI
- ChromaDB (Persistent Vector Database)
- Cohere API (Embeddings and Chat Generation)

## Getting Started

### Prerequisites
- Python 3.8+
- Node.js and npm
- A Cohere API key

### 1. Clone the Repository
```bash
git clone https://github.com/MetaFazer/Codebase.git
cd Codebase
```

### 2. Backend Configuration
Navigate to the backend directory, install the required packages, and configure the environment variables:
```bash
cd backend

# Install dependencies
pip install -r requirements.txt

# Create environment file and add your Cohere API key
echo "COHERE_API_KEY=your_api_key_here" > .env

# Run the backend server
python -m uvicorn main:app --reload --port 8000
```
The backend server runs locally at `http://localhost:8000`.

### 3. Frontend Configuration
In a separate terminal, navigate to the frontend directory, install the node modules, and start the React dev server:
```bash
cd frontend

# Install dependencies
npm install

# Start the application
npm start
```
The frontend application will be accessible at `http://localhost:3000`.

## Architecture and Workflow

1. **Repository Ingestion:** The frontend posts a target GitHub URL to the `/clone` endpoint. The backend clones the repository into the local `repos/` workspace.
2. **Directory Visualization:** The backend exposes `/files` and `/file_content` endpoints to allow the frontend's file explorer to render the directory structure and read file content dynamically.
3. **Chunking & Indexing:** The `/embed` endpoint walks the project tree to gather supported source files (such as `.py`, `.js`, `.md`), segments the code into manageable chunks, generates vector representations using Cohere embeddings, and commits them to ChromaDB.
4. **Retrieval-Augmented Generation (RAG):** When a user asks a question via the `/chat` endpoint, the system embeds the query, retrieves the most similar code chunks from ChromaDB, compiles those chunks into a developer context window, and requests a response from Cohere's model.