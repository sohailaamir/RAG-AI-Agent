# RAG-AI-AGENT

A **Retrieval-Augmented Generation (RAG)** web app that answers questions **strictly from your PDFs** using **FAISS** vector search, **Hugging Face embeddings**, and an **LLM hosted on Hugging Face Inference**. It includes a clean UI, friendly “not found” messages with emojis, and clickable links in answers.

***

## ✨ Features

*   🔎 **Multi‑PDF indexing**: Seamlessly combine multiple PDFs into one FAISS index
*   🧠 **Semantic search** with `sentence-transformers/all‑MiniLM‑L6‑v2`
*   🤖 **Grounded answers**: LLM uses only retrieved context from your PDFs
*   ✅ **Clear UX**:
    *   “**Answer Found**” card only when a real answer exists
    *   Friendly **“Sorry 😔… not in the PDF”** panel when content isn’t found
*   🔗 **Clickable links** in responses
*   🔐 **`.env` support** for secure tokens
*   🐍 **Virtual environment** friendly (Windows/macOS/Linux)

***

## 📁 Project Structure

    rag-ai-agent/
    │
    ├── app.py                 # Streamlit app (RAG UI + pipeline)
    ├── requirements.txt       # Python dependencies
    ├── .env                   # Holds HF_TOKEN (not committed)
    ├── qa_pdf/
    │   ├── rag_sample_qa.pdf  # 20 Q&A sample PDF
    │   └── rag_50_questions.pdf  # 50 Q&A support-style PDF
    └── README.md

> You can add more PDFs inside `qa_pdf/` and update the list in the app to include them.

***

## 🔐 Environment Variables

Create a `.env` file at the project root:

```env
HF_TOKEN=hf_your_real_token_here
```

*   Get your token: <https://huggingface.co/settings/tokens>
*   **No quotes**, **no spaces**, just `KEY=VALUE`.

***

## 🐍 Set Up a Virtual Environment

> **Python 3.10+** recommended (3.11 preferred)

### Windows (PowerShell)

```powershell
cd rag-ai-agent
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

> If `py` is not available, use:
>
> ```powershell
> python -m venv .venv
> .\.venv\Scripts\Activate.ps1
> ```

### macOS / Linux

```bash
cd rag-ai-agent
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

***

## 📦 Install Dependencies

```bash
pip install -r requirements.txt
```

**`requirements.txt`**

    streamlit
    langchain
    langchain-community
    langchain-huggingface
    langchain-core
    sentence-transformers
    faiss-cpu
    pypdf
    python-dotenv
    transformers

***

## 📚 Add/Verify PDFs

This app ships with two sample PDFs for testing:

*   `qa_pdf/rag_sample_qa.pdf` (20 Q\&A)
*   `qa_pdf/rag_50_questions.pdf` (50 Q\&A)

You can add more PDFs to `qa_pdf/`, and then include their paths in the list used by the app.

***

## ▶️ Run the App

```bash
streamlit run app.py
```

Open the local URL shown in the terminal (typically <http://localhost:8501>).  
Type your question in the text area and click **Submit**.

*   If a relevant answer is found in your PDFs → ✅ **Answer Found** (green card)
*   If not → 😔 **Sorry** message with suggestions to **try another question**

***

## 🧠 How It Works

1.  **Load PDFs** using `PyPDFLoader`
2.  **Split** into chunks with `RecursiveCharacterTextSplitter`
3.  **Embed** chunks using `HuggingFaceEmbeddings` (`all‑MiniLM‑L6‑v2`)
4.  **Index** with FAISS (single combined index across all PDFs)
5.  **Retrieve** top‑K chunks for a query
6.  **Generate** a grounded answer via a **Hugging Face Inference** instruct model
7.  **Polish** the UI: success/error panels, emojis, and **clickable links**

***

## 🤖 Model Notes

The app uses a Hugging Face Inference LLM. If your `HF_TOKEN` doesn’t have access to the default model, switch to a model you can use by changing the `repo_id` in `app.py`, e.g.:

*   `mistralai/Mistral-7B-Instruct-v0.2`
*   `meta-llama/Meta-Llama-3-8B-Instruct`

> You can browse models at <https://huggingface.co/models> (filter for **instruct** or **chat** variants).

***

## 🧪 Troubleshooting

*   **`HF_TOKEN missing`**  
    Ensure `.env` exists and contains:  
    `HF_TOKEN=hf_XXXXXXXXXXXXXXXXXXXXXXXX`  
    Then restart the app.

*   **Model access denied (403/404)**  
    Your token may not have access to the default model. Choose a public instruct model and update `repo_id`.

*   **No answers for known questions**
    *   Check the PDF actually contains the info
    *   Increase `TOP_K` (e.g., 6)
    *   Tune chunking: `chunk_size=600`, `chunk_overlap=80`

*   **ModuleNotFoundError**  
    Make sure your virtual environment is **active** and you ran `pip install -r requirements.txt`.

*   **PowerShell script execution policy (Windows)**  
    If activation fails:  
    `Set-ExecutionPolicy Unrestricted -Scope CurrentUser`  
    Then re-run: `.\.venv\Scripts\Activate.ps1`

***

## ➕ Extending (Ideas)

*   📎 **PDF Uploader**: Ingest any PDF at runtime
*   💾 **Persistent FAISS**: Save/load index to speed up startup
*   💬 **Chat history + streaming responses**
*   🎨 **Branding / Theme toggle / Sidebar settings**
*   🐳 **Docker**: Containerize for deployment

***

## 🤝 Contributing

1.  Fork the repository
2.  Create a feature branch: `git checkout -b feat/your-feature`
3.  Commit your changes: `git commit -m "Add your feature"`
4.  Push to your branch: `git push origin feat/your-feature`
5.  Open a Pull Request

***

## 📝 License

This project is released under the **MIT License**.  
Feel free to use, modify, and distribute.

***

## 🙏 Acknowledgments

*   Streamlit
*   LangChain
*   Hugging Face
*   Sentence Transformers
*   FAISS

***
