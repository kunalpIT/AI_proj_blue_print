# Advanced RAG Hackathon Projects (Production-Style Templates)

# Project 1 — InsureAI

## AI-Powered Insurance Claim & Policy Assistant

---

# Problem Statement

Insurance documents are lengthy and difficult for customers to understand.
Users struggle with:

- claim eligibility
- exclusions
- waiting periods
- premium details
- hospitalization coverage
- document comparison

The solution is an AI-powered RAG assistant that can answer questions from uploaded insurance policies instantly.

---

# Features

## Core Features

- Multi-PDF Upload
- Conversational Chatbot
- RAG Pipeline
- Source Citations
- Chat History
- Semantic Search
- PDF Summarization
- Policy Comparison
- Insurance Recommendation Engine

---

# Advanced Features

## 1. Policy Comparison

Upload 2 insurance PDFs.

Ask:

"Which policy has better hospitalization coverage?"

---

## 2. Claim Eligibility Checker

User inputs:

- disease
- hospitalization amount
- waiting period

System checks policy rules.

---

## 3. Smart Recommendations

Example:

"Recommend best policy for diabetes patient under 10 lakhs coverage"

---

## 4. Explain Complex Terms

Example:

"Explain co-payment in simple language"

---

# Tech Stack

## Frontend

- Streamlit

## Backend

- Python

## RAG Framework

- LangChain

## Vector Database

- ChromaDB

## Embeddings

- OpenAI Embeddings

## LLM

- GPT-4 / GPT-3.5

---

# Final Architecture

```text
User Uploads PDFs
        ↓
PDF Loader
        ↓
Chunking
        ↓
Embeddings
        ↓
Chroma Vector DB
        ↓
Retriever
        ↓
Custom Prompt
        ↓
LLM
        ↓
Chatbot Response
```

---

# Recommended Folder Structure

```text
InsureAI/
│
├── app.py
├── requirements.txt
├── .env
├── README.md
│
├── data/
│   └── uploaded_pdfs/
│
├── utils/
│   ├── pdf_loader.py
│   ├── chunking.py
│   ├── embeddings.py
│   ├── retriever.py
│   ├── prompts.py
│   └── comparison.py
│
├── vectorstore/
│
└── assets/
```

---

# Core Modules

## 1. pdf_loader.py

```python
from langchain_community.document_loaders import PyPDFLoader


def load_pdf(pdf_path):
    loader = PyPDFLoader(pdf_path)
    return loader.load()
```

---

## 2. chunking.py

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter


def split_documents(documents):

    splitter = RecursiveCharacterTextSplitter(
        chunk_size=800,
        chunk_overlap=100
    )

    return splitter.split_documents(documents)
```

---

## 3. embeddings.py

```python
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import Chroma


def create_vectorstore(chunks):

    embeddings = OpenAIEmbeddings()

    vectorstore = Chroma.from_documents(
        documents=chunks,
        embedding=embeddings,
        persist_directory="vectorstore"
    )

    return vectorstore
```

---

## 4. prompts.py

```python
SYSTEM_PROMPT = """
You are an insurance policy assistant.

Rules:
1. Answer only from provided context.
2. If answer not found, say:
   'Information not available in policy document.'
3. Keep answers concise.
4. Mention exclusions if applicable.
"""
```

---

# Main app.py

```python
import streamlit as st
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain.chains import RetrievalQA

from utils.pdf_loader import load_pdf
from utils.chunking import split_documents
from utils.embeddings import create_vectorstore

load_dotenv()

st.set_page_config(page_title="InsureAI")

st.title("🛡️ InsureAI")

uploaded_files = st.file_uploader(
    "Upload Insurance PDFs",
    type="pdf",
    accept_multiple_files=True
)

if uploaded_files:

    all_docs = []

    for file in uploaded_files:

        with open(file.name, "wb") as f:
            f.write(file.read())

        docs = load_pdf(file.name)
        all_docs.extend(docs)

    chunks = split_documents(all_docs)

    vectorstore = create_vectorstore(chunks)

    retriever = vectorstore.as_retriever(
        search_kwargs={"k": 4}
    )

    llm = ChatOpenAI(
        model="gpt-4",
        temperature=0
    )

    qa_chain = RetrievalQA.from_chain_type(
        llm=llm,
        retriever=retriever,
        return_source_documents=True
    )

    query = st.chat_input("Ask insurance-related question")

    if query:

        result = qa_chain(query)

        st.write(result["result"])

        with st.expander("Sources"):
            for doc in result["source_documents"]:
                st.write(doc.page_content)
```

---

# Sample Hackathon Questions

- What is the waiting period?
- Is diabetes covered?
- What are exclusions?
- Which policy has lower co-payment?
- Compare hospitalization coverage.

---

# Winning Point

Explain:

"We built semantic insurance document intelligence using Retrieval-Augmented Generation."

============================================================

# Project 2 — MediMind AI

## AI-Powered Healthcare Knowledge Assistant

---

# Problem Statement

Healthcare documents are difficult for patients and staff to understand.

Hospitals and clinics deal with:

- medical reports
- discharge summaries
- treatment guidelines
- insurance approvals
- medicine instructions

The project creates an AI healthcare assistant using RAG.

---

# Features

## Core Features

- Medical PDF Upload
- AI Chatbot
- RAG Retrieval
- Medical Report Summarization
- Treatment Guideline Search
- Medicine Information Retrieval
- Doctor Notes Assistant
- Clinical Knowledge Base

---

# Advanced Features

## 1. Medical Report Summarizer

Upload blood report PDF.

Get:

- concise summary
- abnormal values
- observations

---

## 2. Symptom-to-Guideline Search

Example:

"What is treatment for hypertension?"

Retrieves guideline chunks.

---

## 3. Hospital SOP Assistant

Hospitals can upload SOP PDFs.

Staff can ask:

- ICU procedure
- emergency workflow
- patient admission steps

---

## 4. Multi-Document Retrieval

Search across:

- reports
- prescriptions
- SOPs
- insurance docs

---

# Tech Stack

## Frontend

- Streamlit

## Backend

- Python

## RAG Framework

- LangChain

## Embeddings

- OpenAI Embeddings

## Vector Database

- ChromaDB

## Optional Additions

- Whisper for speech input
- OCR for scanned PDFs
- FAISS for offline vector search

---

# Folder Structure

```text
MediMind-AI/
│
├── app.py
├── requirements.txt
├── .env
├── README.md
│
├── data/
│
├── modules/
│   ├── loader.py
│   ├── splitter.py
│   ├── vectordb.py
│   ├── rag_chain.py
│   ├── prompts.py
│   └── summarizer.py
│
└── vectorstore/
```

---

# prompts.py

```python
HEALTHCARE_PROMPT = """
You are a healthcare AI assistant.

Rules:
1. Answer only from retrieved medical context.
2. Never generate fake diagnosis.
3. If answer unavailable, say:
   'Medical information not found in uploaded documents.'
4. Add disclaimer:
   'This is not medical advice.'
"""
```

---

# rag_chain.py

```python
from langchain.chains import RetrievalQA
from langchain_openai import ChatOpenAI


def build_rag_chain(retriever):

    llm = ChatOpenAI(
        model="gpt-4",
        temperature=0
    )

    qa_chain = RetrievalQA.from_chain_type(
        llm=llm,
        retriever=retriever,
        return_source_documents=True
    )

    return qa_chain
```

---

# Main app.py

```python
import streamlit as st
from dotenv import load_dotenv

from modules.loader import load_pdf
from modules.splitter import split_documents
from modules.vectordb import create_vectorstore
from modules.rag_chain import build_rag_chain

load_dotenv()

st.set_page_config(page_title="MediMind AI")

st.title("🩺 MediMind AI")

st.write("Healthcare Knowledge Assistant")

uploaded_file = st.file_uploader(
    "Upload Medical PDF",
    type="pdf"
)

if uploaded_file:

    with open(uploaded_file.name, "wb") as f:
        f.write(uploaded_file.read())

    documents = load_pdf(uploaded_file.name)

    chunks = split_documents(documents)

    vectorstore = create_vectorstore(chunks)

    retriever = vectorstore.as_retriever(
        search_kwargs={"k": 5}
    )

    qa_chain = build_rag_chain(retriever)

    user_query = st.chat_input(
        "Ask healthcare-related question"
    )

    if user_query:

        result = qa_chain(user_query)

        st.chat_message("user").write(user_query)

        st.chat_message("assistant").write(
            result["result"]
        )

        with st.expander("Retrieved Sources"):

            for doc in result["source_documents"]:
                st.write(doc.page_content)
```

---

# Sample Demo Questions

- What medicines are prescribed?
- Summarize this report.
- What are abnormal observations?
- What is treatment guideline for asthma?
- Explain hypertension in simple language.

---

# High-Level Enhancements

## Add OCR

For scanned hospital reports.

Use:

- pytesseract
- easyocr

---

## Add Voice Assistant

Doctor speaks question.

Speech → Text → RAG → Voice Response

---

## Add Patient Dashboard

Show:

- uploaded reports
- medical summaries
- history

---

# Recommended README Structure

## Sections

- Problem Statement
- Solution Architecture
- Features
- Tech Stack
- Installation
- Screenshots
- Demo Flow
- Future Scope

---

# Strong Resume Points

- Built enterprise-grade RAG architecture using LangChain and ChromaDB.
- Developed AI-powered document intelligence chatbot using semantic retrieval.
- Implemented vector search, embeddings, and contextual LLM pipelines.
- Designed scalable healthcare/insurance knowledge assistant.

---

# Final Hackathon Advice

Focus on:

1. Smooth Demo
2. Clear Business Problem
3. Explain Retrieval Clearly
4. Add Source Citations
5. Keep UI Professional
6. Show Real-Time PDF Upload

That combination performs extremely well in hackathons.

