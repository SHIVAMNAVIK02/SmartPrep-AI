# SmartPrep AI

**SmartPrep AI** is an AI-powered study and career assistant built with **LangGraph** and **Google Gemini**. It uses a router-based workflow to direct a user's request to a specialized assistant node.

The project is currently being developed and tested in a Jupyter Notebook.

## Features

SmartPrep AI is designed to support these workflows:

- **General AI Chat** — answers questions about programming, AI, machine learning, and career preparation.
- **Resume Analysis** — reviews resume text and identifies skills, strengths, gaps, and possible improvements.
- **Job Description Matching** — compares resume content with a job description and highlights matching skills and skill gaps.
- **PDF Question Answering (RAG)** — retrieves relevant passages from an uploaded PDF and uses them to answer questions.
- **Interview Preparation** — generates interview questions and evaluates user-provided answers.
- **Study Plan Generation** — creates a structured learning plan based on a study goal.
- **Web Search** — retrieves web results and uses the LLM to summarize relevant information.

> **Development status:** These are the project's implemented or developing workflows. PDF upload handling, Streamlit integration, and other enhancements may still require further development and testing.

## Workflow Architecture

The application follows a router-based LangGraph architecture:

```text
                         START
                           |
                       Input Node
                           |
                       Router Node
                           |
          +----------------+----------------+
          |                |                |
     Resume Analysis   Job Matching       PDF Q&A
          |                |                |
     Interview         Study Planner     Web Search
          |                |                |
          +----------------+----------------+
                           |
                        General Chat
                           |
                      Response Node
                           |
                          END
```

The router classifies the user's query and sends it to the relevant workflow. Each workflow returns a response that is passed through the Response Node.

## Tech Stack

- **Python** — core programming language
- **LangGraph** — workflow orchestration and conditional routing
- **LangChain** — LLM and document-processing integrations
- **Google Gemini** — language model for routing and response generation
- **ChromaDB** — vector database for document retrieval
- **Hugging Face Sentence Transformers** — local text embeddings (`all-MiniLM-L6-v2`)
- **PyPDF** — PDF text extraction
- **DDGS** — web search
- **Jupyter Notebook** — development and testing environment

## Project Structure

```text
SmartPrep_AI/
│
├── SmartPrep.ipynb       # Main notebook containing the workflow and nodes
├── requirements.txt      # Python dependencies
├── README.md             # Project documentation
├── .gitignore            # Files excluded from Git
├── .env                  # API key configuration (not committed)
└── genai/                # Local virtual environment (not committed)
```

Your local project may contain additional files. Do not commit your `.env` file or virtual environment.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/SHIVAMNAVIK02/SmartPrep-AI.git
cd SmartPrep-AI
```

### 2. Create and activate a virtual environment

**Windows PowerShell:**

```powershell
python -m venv genai
.\genai\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

If a dependency is missing while running the notebook, install it in the active environment. For example:

```bash
pip install langgraph langchain langchain-google-genai langchain-community langchain-chroma langchain-text-splitters langchain-huggingface sentence-transformers pypdf ddgs python-dotenv
```

### 4. Configure the Gemini API key

Create a `.env` file in the project root:

```env
GOOGLE_API_KEY=your_gemini_api_key
```

Keep this file private. Never commit API keys to GitHub.

### 5. Run the notebook

Start Jupyter:

```bash
pip install notebook
jupyter notebook
```

Open `SmartPrep.ipynb` and run the cells in order, beginning with the imports, environment configuration, model initialization, state definition, and node definitions. Then build and compile the graph.

## Example Usage

The graph accepts a state dictionary. For example:

```python
test_state = {
    "user_query": "Explain the difference between machine learning and deep learning."
}

result = graph.invoke(test_state)

print("Intent:", result.get("intent"))
print("Response:", result.get("final_response"))
```

### Example: Resume analysis

Provide the resume text and a request:

```python
test_state = {
    "user_query": "Analyze my resume and identify my strengths and skill gaps.",
    "resume_text": "Paste your resume text here."
}

result = graph.invoke(test_state)

print("Intent:", result.get("intent"))
print(result.get("final_response"))
```

### Example: Job description matching

```python
test_state = {
    "user_query": "Compare my resume with this job description.",
    "resume_text": "Paste your resume text here.",
    "job_description": "Paste the job description here."
}

result = graph.invoke(test_state)

print("Intent:", result.get("intent"))
print(result.get("final_response"))
```

### Example: PDF question answering

First, process a PDF with the notebook's PDF-processing function to create a Chroma vector store. Then pass that vector store into the graph:

```python
vector_store = process_pdf("path/to/your-document.pdf")

test_state = {
    "user_query": "Summarize the key points in this document.",
    "vector_store": vector_store
}

result = graph.invoke(test_state)

print("Intent:", result.get("intent"))
print(result.get("final_response"))
```

The PDF Q&A workflow requires a processed vector store. Local Hugging Face embeddings download the embedding model the first time they are used; a Hugging Face API token is not required for this public model.

## Environment Variables

| Variable | Purpose |
|---|---|
| `GOOGLE_API_KEY` | Authenticates requests to Google Gemini |

Do not hard-code secrets in notebook cells or source files.

## Current Development Notes

- The main implementation is in `SmartPrep.ipynb`.
- The graph uses conditional routing to select a specialized workflow.
- PDF retrieval uses ChromaDB and local Sentence Transformers embeddings.
- The project should be tested workflow by workflow before adding a user interface.
- A Streamlit interface and direct user PDF-upload flow are possible future enhancements; do not assume they are already available unless implemented in the current code.

## Future Improvements

- Add a user-friendly Streamlit interface.
- Support direct PDF uploads and automatic routing between resume analysis and document Q&A.
- Improve router accuracy and add fallback handling for ambiguous requests.
- Add conversation memory and multi-turn interview sessions.
- Add automated tests and more robust error handling.
- Improve source display and citations in RAG and web-search responses.

## Contributing

Suggestions, bug reports, and improvements are welcome. For major changes, open an issue first to discuss what you would like to change.

## Author

**Shivam Navik**

- GitHub: [SHIVAMNAVIK02](https://github.com/SHIVAMNAVIK02)

---

If you find this project useful, consider giving the repository a star.
