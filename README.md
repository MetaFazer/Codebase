# CodexAI 🚀

> An AI-powered codebase exploration and chat engine. 

CodexAI (formerly Codebase OS) is a full-stack application that allows you to easily explore, understand, and chat with any GitHub repository using AI. It clones repositories, indexes their content using vector embeddings, and allows you to ask natural language questions about the codebase.

## ✨ Features

- 📂 **Instant Cloning:** Enter a GitHub URL to instantly clone and explore the repository locally.
- 🌳 **Interactive File Explorer:** Browse through the repository structure with a responsive, tree-based UI.
- 📝 **Code Viewer:** View the content of any supported file with full syntax highlighting.
- 🧠 **AI Embeddings:** Automatically processes and embeds the source code into a persistent vector database (ChromaDB) using the Cohere API.
- 💬 **Codebase Chat:** Ask questions about the codebase architecture, logic, or specific features, and get accurate answers grounded in the repository's source code.

## 🛠️ Tech Stack

**Frontend:**
- React 19
- Material UI (MUI)
- Framer Motion (Animations)
- React Syntax Highlighter

**Backend:**
- Python & FastAPI
- ChromaDB (Persistent Vector Database)
- Cohere API (Embeddings & Text Generation)

## 🚀 Getting Started

### Prerequisites
- Python 3.8+
- Node.js & npm
- A [Cohere API Key](https://dashboard.cohere.com/api-keys)

### 1. Clone the repository
```bash
git clone https://github.com/MetaFazer/CodexAI.git
cd CodexAI
```

### 2. Backend Setup
Navigate to the backend directory, install dependencies, and set up your environment variables.
```bash
cd backend

# Install dependencies
pip install -r requirements.txt

# Create a .env file and add your Cohere API key
echo "COHERE_API_KEY=your_api_key_here" > .env

# Run the backend server
python -m uvicorn main:app --reload --port 8000
```
*The backend will be running at `http://localhost:8000`*

### 3. Frontend Setup
Open a new terminal, navigate to the frontend directory, install dependencies, and start the development server.
```bash
cd frontend

# Install dependencies
npm install

# Start the frontend
npm start
```
*The frontend will be running at `http://localhost:3000`*

## 📖 How It Works

1. **Clone:** The frontend sends a GitHub URL to the `/clone` endpoint. The backend uses `git clone` to pull the code into a local `repos/` folder.
2. **Explore:** The `/files` and `/file_content` endpoints allow the React frontend to visualize the directory structure and read files.
3. **Embed:** The `/embed` endpoint scans the repository for supported file extensions (e.g., `.py`, `.js`, `.md`), chunks the content, generates embeddings using Cohere, and stores them in ChromaDB.
4. **Chat:** The `/chat` endpoint takes a natural language query, embeds it, queries ChromaDB for the top 5 most relevant code snippets, and passes those snippets as context to Cohere's Chat model to generate an answer.