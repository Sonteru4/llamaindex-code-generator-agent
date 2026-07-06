# AI Agent Code Generator

Interactive CLI agent that reads API documentation and generates Python code using LlamaIndex, Ollama, and local embeddings.

## Overview

This project uses a LlamaIndex ReAct agent backed by Ollama LLMs to analyze documentation and generate code from natural-language prompts. Documents in a `./data` directory are indexed with local HuggingFace embeddings (`BAAI/bge-m3`), and PDF files are parsed via LlamaParse. The agent has two tools: a vector query engine over the documentation and a file reader for inspecting source code. Generated code is parsed into structured JSON (code, description, filename) and written to an `output/` directory.

## Tech Stack

- Python
- LlamaIndex (`ReActAgent`, `VectorStoreIndex`, `QueryEngineTool`)
- Ollama (`mistral` for queries/parsing, `codellama` for code generation)
- LlamaParse (PDF parsing)
- HuggingFace embeddings (`local:BAAI/bge-m3` via `llama-index-embeddings-huggingface`)
- Pydantic (structured output parsing)
- python-dotenv

## Features

- Loads documents from `./data` (PDF files parsed with LlamaParse, other files via `SimpleDirectoryReader`)
- Vector index over documentation for semantic search (`api_documentation` tool)
- `code_reader` tool to read individual files from `./data`
- ReAct agent (`codellama`) that analyzes code and generates new code from prompts
- Structured output pipeline that extracts code, description, and filename into JSON
- Saves generated code to `output/<filename>`
- Interactive REPL loop (`Enter a prompt (q to quit)`)
- Sample Flask CRUD API in `test.py` for reference/documentation
- `create_item_script.py` — helper script to POST `test.py` contents to an external API

## Getting Started

1. Install dependencies:

```bash
pip install -r "requirements (1).txt"
```

2. Install and run [Ollama](https://ollama.com/) with the required models:

```bash
ollama pull mistral
ollama pull codellama
```

3. Create a `.env` file if using LlamaParse (requires a LlamaCloud API key):

```
LLAMA_CLOUD_API_KEY=your_llama_cloud_api_key
```

4. Add API documentation and source files to a `data/` directory (expected by the app but not included in the repo). Include at least the reference API code (e.g., `test.py`).

5. Create an output directory:

```bash
mkdir output
```

6. Run the agent:

```bash
python "main (1).py"
```

Enter prompts at the interactive prompt. Type `q` to quit. Generated files are saved to `output/`.

## Project Structure

```
AI-Agent-code-generator/
├── main (1).py              # Entry point: agent setup and REPL loop
├── code_reader.py           # Tool to read files from data/
├── prompts.py               # Agent context and output parser template
├── test.py                  # Sample Flask CRUD API (reference code)
├── create_item_script.py    # Script to upload test.py to an external API
├── requirements (1).txt     # Python dependencies
├── data/                    # Documentation and source files (create locally)
└── output/                  # Generated code output (create locally)
```
