# Adaptive RAG

An implementation of Adaptive Retrieval-Augmented Generation (RAG) using LangChain and LangGraph. This system intelligently routes queries between a vector store and web search based on the question type, and includes quality checks to ensure accurate and relevant responses.

## Features

- **Intelligent Routing**: Automatically routes questions to either a vector store (for domain-specific knowledge) or web search (for general queries)
- **Document Grading**: Filters retrieved documents for relevance to the question
- **Hallucination Detection**: Verifies that generated answers are grounded in retrieved documents
- **Answer Quality Assessment**: Ensures generated answers address the user's question
- **Query Transformation**: Automatically rewrites queries for better retrieval when needed
- **State Graph Workflow**: Uses LangGraph to orchestrate the adaptive RAG pipeline

## Architecture

The system implements a state graph with the following components:

1. **Router**: Determines whether to use vector store or web search
2. **Retriever**: Fetches relevant documents from the vector store
3. **Document Grader**: Filters documents for relevance
4. **Query Transformer**: Rewrites queries for better retrieval
5. **Generator**: Creates answers using retrieved context
6. **Hallucination Grader**: Checks if answers are grounded in documents
7. **Answer Grader**: Verifies answers address the question

## Setup

### Prerequisites

- Python 3.8 or higher
- OpenAI API key
- Tavily API key (for web search)

### Installation

1. Clone this repository:
```bash
git clone
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Create a `.env` file in the root directory with your API keys:
```env
OPENAI_API_KEY=your_openai_api_key_here
TAVILY_API_KEY=your_tavily_api_key_here
```

## Usage

1. Open the Jupyter notebook:
```bash
jupyter notebook AdaptiveRAG.ipynb
```

2. Run the cells sequentially to:
   - Build the vector store index from web documents
   - Set up the routing, grading, and generation components
   - Create and compile the state graph
   - Query the system

### Example Queries

- **Domain-specific query** (uses vector store): "What are the types of agent memory?"
- **General query** (uses web search): "Who won the Cricket world cup 2023?"

## How It Works

1. **Question Routing**: The system first determines if the question should be answered using the vector store (for topics like agents, prompt engineering, adversarial attacks) or web search (for general knowledge).

2. **Retrieval & Grading**: If using the vector store:
   - Documents are retrieved
   - Each document is graded for relevance
   - If no relevant documents are found, the query is transformed and retrieval is retried

3. **Generation**: An answer is generated using the retrieved context.

4. **Quality Checks**: The generated answer is checked for:
   - **Hallucination**: Is it grounded in the retrieved documents?
   - **Relevance**: Does it address the user's question?

5. **Adaptive Behavior**: If quality checks fail, the system can:
   - Regenerate the answer
   - Transform the query and retry retrieval
   - Use web search as a fallback

## Dependencies

- `langchain`: Core LangChain functionality
- `langchain-community`: Community integrations (FAISS, web loaders, Tavily)
- `langchain-openai`: OpenAI embeddings and chat models
- `langgraph`: State graph orchestration
- `faiss-cpu`: Vector store for embeddings
- `python-dotenv`: Environment variable management
- `pydantic`: Data validation and models

## Notes

- The vector store is built from web documents about agents, prompt engineering, and adversarial attacks
- The system uses GPT-4o-mini for routing and grading, and GPT-3.5-turbo for generation
- FAISS is used as the vector store backend
- Tavily is used for web search functionality

## License

MIT license
