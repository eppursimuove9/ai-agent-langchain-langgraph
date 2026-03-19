# AI Agent with LangChain & LangGraph

This repository contains a Jupyter Notebook demonstrating an intelligent AI agent built with **LangChain**, **LangGraph**, and **Google's Gemini 2.5 Flash**. 

The agent acts as a smart router: it evaluates a user's question and decides whether to answer it by searching through a local database of PDF documents (RAG) or by performing a real-time web search. Regardless of the source, the final output is always formatted into a clean, easy-to-read Markdown response.

## 🧠 How It Works

The core logic is orchestrated using a state graph (`LangGraph`), which follows this flow:

```text
                    ┌─── "RAG" ───→ [Search Local PDFs] ───┐
[START] → [Agent]                                          → [Generate Markdown] → END
                    └─── "Web" ───→ [Search the Web] ──────┘
