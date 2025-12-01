# 🎓 Smart Campus Assistant 

> **Spring Boot RAG backend that turns campus notes into an AI-powered academic assistant for grounded Q&A, summaries, and quizzes.**

---

## 📚 Problem Statement

College students and faculty often rely on scattered PDFs, university notes, and personal materials spread across drives, mail, and LMS platforms.  

When exams approach, students waste time:

- Skimming long PDFs to find specific topics  
- Re-reading notes instead of actively recalling concepts  
- Searching for doubts across multiple documents  

There is no unified system that can:

- Understand campus-provided notes and student-uploaded materials  
- Answer questions *from those materials*  
- Generate summaries for revision  
- Create quizzes based on what the student is studying  

**Smart Campus Assistant** addresses this by providing a backend AI assistant that uses **Retrieval-Augmented Generation (RAG)** to ground all responses in uploaded academic materials, with controlled access and role-based usage.

---

## 🎯 Project Overview

Smart Campus Assistant is a **backend / mid-tier Spring Boot service** that powers an AI-driven academic assistant for a university environment.

The system is designed to:

- Let an **Admin team** upload official campus materials (university notes, PDFs, reference docs).
- Allow **authenticated users** (students, faculty, trainers) to:
  - Ask academic questions in natural language.
  - Get answers grounded in campus materials (via RAG).
  - Generate summaries of selected documents or topics.
  - Generate basic quizzes for practice from chosen materials.
- Optionally let users upload **temporary personal notes** that:
  - Are used only within the current session.
  - Do not pollute the global campus knowledge base.
- Support **optional citations** when the user explicitly asks to “show sources”.
- Maintain **limited chat history per session** so users can revisit previous prompts, answers, and quizzes.

> This project focuses on backend architecture, data processing, and RAG integration. Any frontend (web/mobile) can consume these APIs later.

---

## 👥 Roles & Access Model

### Current Scope (MVP)

- **Admin**
  - Uploads and manages *official campus materials* (long-term knowledge base).
  - Controls what becomes part of the permanent RAG index.
- **User**
  - Logs in with valid credentials (e.g., campus email).
  - Asks questions, generates summaries, and quizzes.
  - Can upload **session-only materials** (used for that session’s context only).

### Future Roles (Planned)

- Faculty-specific material owners  
- Trainers / external mentors  
- Role-based access to specific course repositories

---

## 🧩 Core Features (MVP)

- 🔐 **JWT-Based Authentication**
  - Secure login for Admin and Users.
- 📄 **Campus Materials Management (Admin)**
  - Upload, store, and manage official PDFs / notes.
- 🗂 **Session-Only User Materials (User)**
  - Temporary material upload used only within that session’s assistant context.
- ✂️ **PDF Text Extraction & Chunking**
  - Extracts text from uploaded PDFs and breaks them into semantic chunks.
- 🔎 **RAG-based Question Answering**
  - Answers grounded in campus materials (+ optional user session materials).
- 🧠 **Summarization**
  - Generates summaries from selected materials or topics.
- 📝 **Quiz Generation**
  - Creates quizzes (e.g., MCQs/short questions) from chosen content to support revision.
- 📚 **Optional Citations**
  - Includes references to specific documents/sections when the user asks for citations.
- 💬 **Limited Chat History**
  - Stores a bounded list of previous prompts and responses per session.
- 🧾 **Interaction Logging**
  - Logs Q&A (and optionally quiz generations) for analytics and improvement.

---

## 🛠️ Tech Stack

| Layer           | Technology                          |
|----------------|--------------------------------------|
| Language        | Java                                |
| Backend         | Spring Boot                         |
| Security        | Spring Security + JWT               |
| Database        | PostgreSQL                          |
| Vector Storage  | pgvector (vector extension in Postgres) |
| AI Integration  | Spring AI / LangChain4j + LLM APIs  |
| PDF Processing  | Apache PDFBox / iText               |
| API Docs & Testing | Swagger UI / Postman            |

---

## 🗄️ System Architecture 

```text
[ Client / UI ]
        |
        v
[ Spring Boot REST API ]
  ├── Auth Controller
  ├── Material Controller
  ├── Assistant (RAG) Controller
        |
        v
[ Services Layer ]
  ├── Auth Service         (User/Auth management, JWT)
  ├── Material Service     (Upload, PDF extraction, chunking, indexing)
  ├── Rag Service          (Embeddings, vector search, prompt building)
  ├── Chat/Quiz Service    (Answer generation, quiz creation, history)
        |
        v
[ Data Layer ]
  ├── PostgreSQL (Users, Materials, Chunks, Sessions, Logs)
  └── pgvector   (Embeddings + similarity search)
        |
        v
[ LLM Providers ]
  ├── Embedding API        (text → vector)
  └── Chat Completion API  (context + question → answer/summary/quiz)
```


