# AI Agent with LangChain & LangGraph

This repository contains a Jupyter Notebook demonstrating an intelligent AI agent built with **LangChain**, **LangGraph**, and **Google's Gemini 2.5 Flash**. 

The agent acts as a smart router: it evaluates a user's question and decides whether to answer it by searching through a local database of PDF documents (RAG) or by performing a real-time web search. Regardless of the source, the final output is always formatted into a clean, easy-to-read Markdown response.

## 🧠 How It Works

The core logic is orchestrated using a state graph (`LangGraph`), which follows this flow:

```text
                    ┌─── "RAG" ───→ [Search Local PDFs] ───┐
[START] → [Agent]                                          → [Generate Markdown] → END
                    └─── "Web" ───→ [Search the Web] ──────┘
```

* **Agent Node:** Analyzes the prompt and classifies the intent. If the question pertains to the uploaded documents, it routes to `RAG`. If it requires general knowledge or real-time data, it routes to `Web`.
* **RAG Node:** Retrieves relevant context from local PDFs using `FAISS` vector search and `gemini-embedding-001`.
* **Web Node:** Fetches live data from the internet using `SerpAPI`.
* **Markdown Node:** Synthesizes the retrieved context and generates a structured Markdown response.

## 📂 Document Ingestion (The `data/` Folder)

This agent is completely source-agnostic and **can be run with any PDF documents**.

By default, the notebook is configured to read PDFs from a local directory. The documents used to initially train and test this RAG system (fictitious quarterly reports for a travel company called "Carrarurquía") are located in the `data/` folder of this repository.

**To use your own data:** Simply delete the sample PDFs in the `data/` folder and upload your own PDF files. The script will automatically parse, split, and vectorize them on the next run.

## 🔑 Prerequisites & API Keys

To run this notebook, you will need active API keys for both Google Gemini and SerpAPI. **These keys must be provided by the person running the agent.**

* **Gemini API Key:** Used for the LLM (Gemini 2.5 Flash) and the embedding model. [Get it here](https://aistudio.google.com/app/apikey).
* **SerpAPI Key:** Used to execute Google searches for the Web routing node. [Get it here](https://serpapi.com/).

When running the notebook (e.g., in Google Colab), you will need to add these to your environment variables or Colab Secrets:
* `GEMINI_API_KEY`
* `SERPAPI_API_KEY`

## 🚀 Installation & Setup

If you are running this locally (outside of Google Colab), ensure you have Python 3.8+ installed, then install the required dependencies:

```bash
pip install google-genai langchain-community pypdf langchain-text-splitters langchain-google-genai faiss-cpu langgraph google-search-results markdown fpdf2
```

## 🛠️ Technologies Used

* **[LangChain](https://www.langchain.com/):** For document loading (PyPDF), text splitting, and vector store integration.
* **[LangGraph](https://langchain-ai.github.io/langgraph/):** For creating the conditional routing logic and managing the agent's state.
* **[Google Gemini](https://deepmind.google/technologies/gemini/):** `gemini-2.5-flash` for reasoning/generation and `gemini-embedding-001` for vectorization.
* **[FAISS](https://github.com/facebookresearch/faiss):** For fast, local vector storage and similarity search.
* **[SerpAPI](https://serpapi.com/):** For executing search engine queries.

## 💡 Example Usage

Once the graph is compiled, you can run the agent by passing a query:

```python
ejecutar_agente("¿Dónde se mantuvo concentrado el mix de productos?")
# Output: Routes to RAG and answers based on the local PDFs.

ejecutar_agente("¿Cuántos mundiales de fútbol tiene Brasil?")
# Output: Routes to Web, searches SerpAPI, and answers based on the internet.
