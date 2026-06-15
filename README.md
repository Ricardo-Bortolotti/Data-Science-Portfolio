# Data Science, Machine Learning & AI Portfolio

Hi, I'm Ricardo Bortolotti — a Data Scientist with a Ph.D. in Mathematics and professional experience developing **Machine Learning** and **AI** solutions for real-world applications.

My background combines:

- Machine Learning & Predictive Modeling
- Deep Learning & Computer Vision
- **Retrieval-Augmented Generation (RAG) & LLM Applications**
- **Multi-Agent AI Workflows**
- Statistics & Applied Mathematics
- Data Engineering & Automation
- MLOps & Production AI/ML Systems
- API Development & Data Products

I currently work on large-scale data problems involving fraud detection, healthcare analytics, graph-based analysis, embeddings, semantic search, and operational machine learning systems.

This portfolio contains projects ranging from exploratory machine learning notebooks to **end-to-end production-oriented AI applications** deployed on the cloud.

---

# 🚀 Featured Projects — Production AI/ML Systems

## 📚 [Book Research Agent — RAG + Multi-Agent Study Materials](https://github.com/Ricardo-Bortolotti/research-workflow-agent)

End-to-end **Generative AI** platform that transforms PDFs into structured study materials using RAG, vector search, and a **LangGraph** multi-agent workflow — with a separated Streamlit frontend and FastAPI backend deployed in production.

### Overview

This project implements a complete document intelligence pipeline — from PDF ingestion to grounded LLM outputs:

- PDF loading, chunking, and embedding via Hugging Face Inference API
- ChromaDB vector store for semantic retrieval
- Five specialized agents orchestrated as a linear LangGraph DAG
- REST API for upload, analysis, and structured results
- Slim Docker API image (no local PyTorch) + lightweight Streamlit client
- Automated CI/CD and split cloud deployment

### Live demo

| Service | URL |
|---------|-----|
| Frontend | https://research-workflow-agent-giekafxwvrbkmzllhjlnt2.streamlit.app/ |
| API | https://research-workflow-agent-production.up.railway.app/ |
| API docs | https://research-workflow-agent-production.up.railway.app/docs |

### Architecture

```text
Streamlit Cloud              Railway (Docker API)
     │                              │
     │  HTTPS                       │
     └──────────►  FastAPI  ────────┘
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
      load PDF    chunk + embed   ChromaDB
          │             │             │
          └─────────────┴─────────────┘
                        │
                        ▼
              retrieve top-k chunks
                        │
                        ▼
              LangGraph agent DAG
         Summary → Concepts → Quiz
              → Flashcards → Mind Map
                        │
                        ▼
              Hugging Face Inference API
```

### Agent workflow (DAG)

```text
START → SummaryAgent → ConceptAgent → QuizAgent
      → FlashcardAgent → MindMapAgent → END
```

Each agent receives the same retrieved context and produces structured JSON (or text) grounded in the document.

### CI/CD pipeline

```text
git push main → GitHub Actions (Ruff + Pytest) → Docker build (API + UI images)
```

Railway deploys the API from the main `Dockerfile`. Streamlit Community Cloud deploys the UI using `pyproject.toml` + `uv.lock`.

### Main Features

- **RAG pipeline** — PDF ingestion, recursive text splitting, HF API embeddings (`BAAI/bge-small-en-v1.5`)
- **Vector search** — ChromaDB persistent store with configurable `top_k` retrieval
- **Multi-agent workflow** — LangGraph linear DAG with five specialized agents
- **Structured outputs** — executive summary, concepts, quiz, flashcards, hierarchical mind map
- FastAPI REST API (`/upload`, `/analyze`, `/results/{id}`, `/health`)
- Streamlit UI as a thin HTTP client (HF token only on the API)
- Separate Docker images for API (full RAG stack) and UI (minimal deps)
- **108+ unit tests** with mocked LLM and vector store (no API key in CI)
- GitHub Actions — lint, tests, and Docker build on every push/PR
- Cloud deploy — API on Railway, frontend on Streamlit Community Cloud

### Tech Stack

- Python 3.11
- FastAPI & Pydantic
- LangChain & LangGraph
- ChromaDB
- Hugging Face Inference API (LLM + embeddings)
- Streamlit & httpx
- Docker & docker-compose
- **uv** (`pyproject.toml` / `uv.lock`)
- GitHub Actions
- Railway & Streamlit Cloud
- Ruff & Pytest

### Why This Project Matters

This project demonstrates how to build a **production GenAI application** with RAG and agent orchestration — including document indexing, grounded retrieval, multi-step LLM workflows, API design, split frontend/backend deployment, container optimization for cloud RAM limits, and engineering practices (tests, lint, CI).

---

## 🐾 [PetVision AI — Cat vs Dog Image Classification](https://github.com/Ricardo-Bortolotti/pet-classifier)

End-to-end **Deep Learning** platform for image classification, with a separated frontend and REST API deployed in production on the cloud.

### Overview

This project implements a complete MLOps workflow for computer vision — from baseline CNN experiments to a champion **EfficientNet-B0** model served in production:

- Transfer learning and fine-tuning experiments
- Hyperparameter optimization with Optuna
- Experiment tracking and model registry with MLflow
- REST API for inference and Grad-CAM explainability
- Automated CI/CD and cloud deployment

### Live demo

| Service | URL |
|---------|-----|
| Frontend | https://pet-classifier-up.streamlit.app/ |
| API | https://pet-classifier-production.up.railway.app/ |

### Architecture

```text
Streamlit Cloud              Railway (Docker)
     │                              │
     │  HTTPS                       │
     └──────────►  FastAPI  ────────┘
                        │
                        ▼
              EfficientNet-B0 (champion)
                        │
                        ▼
              Cat / Dog prediction
              + Grad-CAM explainability
```

### CI/CD pipeline

```text
git push main → GitHub Actions (Ruff + Pytest) → Docker build → Deploy Railway
```

### Main Features

- **Deep Learning** inference with PyTorch (EfficientNet-B0 champion model)
- FastAPI REST API (`/predict`, `/explain`, health, monitoring)
- Grad-CAM explainability overlay
- Streamlit frontend integrated with the live API
- Lean Docker image (CPU-only PyTorch, no CUDA)
- GitHub Actions — lint and tests on every push
- Fully automatic deploy to Railway on `main`
- Model baked into the container at build time (GitHub Release artifact)
- Modular architecture — inference decoupled from training code

### Tech Stack

- Python 3.11
- PyTorch & TorchVision
- FastAPI & Pydantic
- Streamlit
- MLflow & Optuna
- Docker
- GitHub Actions
- Railway & Streamlit Cloud
- Ruff & Pytest

### Why This Project Matters

This project demonstrates the full journey from **Deep Learning experimentation** to a **production AI system** — including HPO, champion selection, containerized inference, cloud deploy, frontend/backend integration, and automated CI/CD.

---

## 💳 [Credit Card Fraud Detection](https://github.com/Ricardo-Bortolotti/Fraud-detection-project)

End-to-end Machine Learning system for credit card fraud detection using FastAPI, PostgreSQL, MLflow, Docker, and Streamlit.

### Overview

This project implements a complete production-oriented ML workflow, including:

- Model experimentation and tracking
- API deployment
- Prediction monitoring
- Containerized services
- Database persistence
- Model lifecycle management

### Architecture

```text
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Streamlit     │────▶│   FastAPI       │────▶│   PostgreSQL    │
│   Dashboard     │     │   Prediction API│     │   Persistence   │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

### Main Features

- FastAPI REST API for fraud prediction
- MLflow experiment tracking and model versioning
- PostgreSQL persistence layer
- Streamlit monitoring dashboard
- Docker Compose orchestration
- Champion/challenger model workflow
- Automated model selection
- Modular project architecture
- Unit testing structure
- Production-oriented design decisions

### Tech Stack

- Python 3.11
- FastAPI
- PostgreSQL
- MLflow
- Docker & Docker Compose
- Streamlit
- SQLAlchemy
- Scikit-learn
- XGBoost
- Plotly

### Why This Project Matters

This project demonstrates the transition from notebook-based Machine Learning experimentation to a production-ready ML architecture with deployment, monitoring, experiment tracking, reproducibility, and scalable organization.

---

# 📘 Classical Machine Learning Projects

## 🧩 Customer Segmentation — Clustering & PCA

### [Customer Segmentation with K-Means and PCA](https://github.com/Ricardo-Bortolotti/Customer-segmentation)

Machine Learning project using clustering techniques to segment retail customers according to purchasing behavior and demographic information.

### Highlights

- K-Means clustering
- PCA dimensionality reduction
- Exploratory Data Analysis
- Customer behavior analysis

### Results

Customers were segmented into behavioral groups, generating insights for targeted marketing and sales strategies.

---

## 🏠 House Price Prediction — Regression

### [Prediction of House Prices](https://github.com/Ricardo-Bortolotti/Regression-house-prices)

Regression project using supervised Machine Learning algorithms to predict house prices based on property characteristics.

### Highlights

- Regression modeling
- Feature analysis
- Hyperparameter tuning with GridSearchCV
- Model comparison

### Results

The best-performing model was Random Forest Regression after hyperparameter optimization.

---

## 🩺 Breast Cancer Prediction — Classification

### [Prediction of Breast Cancer](https://github.com/Ricardo-Bortolotti/Breast-cancer-prediction)

Classification project for predicting whether a breast mass is malignant or benign using supervised learning techniques.

### Highlights

- Classification models
- Medical dataset analysis
- Model evaluation metrics
- LightGBM implementation

### Results

Developed a classification model capable of supporting diagnostic analysis with strong predictive performance.

---

## 📈 Stock Price Time Series Analysis

### [Stock Price Time Series Analysis](https://github.com/Ricardo-Bortolotti/Stock-price-time-series)

Time series analysis project focused on stock price behavior, volatility, trends, and technical indicators.

### Highlights

- Financial time series analysis
- Trend analysis
- Volatility exploration
- Technical indicators

### Results

Exploratory analysis identified relevant temporal patterns and market behavior indicators.

---

# 🛠️ Skills & Technologies

## Machine Learning, AI & Deep Learning

- Scikit-learn
- XGBoost
- LightGBM
- **PyTorch**
- **Deep Learning** & Transfer Learning
- Computer Vision
- **Retrieval-Augmented Generation (RAG)**
- **LangGraph** & multi-agent workflows
- **Embeddings & semantic search**
- **Hugging Face Inference API**
- Statistical Modeling
- Feature Engineering
- Predictive Analytics

## Data Engineering & MLOps

- FastAPI
- MLflow
- Optuna
- Docker
- PostgreSQL
- SQLAlchemy
- **LangChain**
- **ChromaDB**
- **uv** (Python package manager)
- **GitHub Actions**
- **Railway**
- CI/CD
- Experiment Tracking
- Model Registry

## Data Analysis & Visualization

- Python
- SQL
- Pandas
- Polars
- Plotly
- Streamlit
- Power BI

## Software & Infrastructure

- Git & GitHub
- REST APIs
- Containerization
- Modular Architecture
- Testing & Automation
- **pytest** & **Ruff**
- Cloud Deploy

---

# 👨‍💻 Professional Background

Before transitioning full-time into Data Science, I worked for over 10 years as a university professor and researcher in Mathematics, publishing academic papers in international journals and developing strong analytical and problem-solving skills.

Today, I apply this analytical background to **Machine Learning**, **Data Science**, **AI**, and production-oriented systems.

---

# 📫 Contact

- GitHub: https://github.com/Ricardo-Bortolotti
- LinkedIn: https://www.linkedin.com/in/ricardo-bortolotti

