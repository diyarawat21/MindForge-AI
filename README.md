# 🧠 MindForge AI

**MindForge AI** is an AI-powered personalized learning platform that helps students learn topics in a structured and personalized way.

Users can generate learning roadmaps, get AI-generated quizzes, find learning resources, create study material, and identify topics where they need more practice.

The project combines **Generative AI, semantic search, vector databases, REST APIs, and a React frontend** to create an interactive learning experience.

---

## ✨ Key Features

### 🗺️ AI Learning Roadmaps

Generate a structured learning roadmap based on:

* Learning topic
* Difficulty level
* User requirements

Roadmaps are generated using **Groq LLMs** and stored in PostgreSQL. Semantic search with Qdrant helps find similar previously generated roadmaps.

### 📚 Learn & Quiz

Users can enter a topic and get learning resources through:

* AI-generated PDF study material
* YouTube learning videos
* AI-generated quizzes

### 📝 AI-Generated Quizzes

MindForge generates questions based on the selected learning topic using an LLM.

Users can attempt quizzes and receive their scores to understand their performance.

### 🔎 Semantic Search

The project uses **Sentence Transformers and Qdrant** to convert roadmap information into embeddings and perform similarity-based search.

This helps reuse relevant roadmaps instead of generating a new one every time a similar topic is requested.

### 🤔 Confusion Detector

The platform can identify weak areas based on quiz performance and help users focus on topics that need more practice.

### 🧠 Recall Cards

Recall cards are designed to help users revise important concepts and remember key information.

### 📊 Progress Tracking

The project includes progress-related features for tracking learning sessions and quiz performance.

### 🎯 Personalized Learning

Additional backend modules support:

* Skill scanning
* Learning style analysis
* Knowledge decay tracking
* Learning sessions
* Personalized roadmaps

---

## 🏗️ How It Works

A simplified flow of the application:

```text
                 User
                   │
                   ▼
            React Frontend
                   │
             HTTP / REST API
                   │
                   ▼
            FastAPI Backend
                   │
       ┌───────────┼───────────┐
       │           │           │
       ▼           ▼           ▼
 PostgreSQL     Qdrant      Groq LLM
       │           │           │
       │           │           │
       └───────────┼───────────┘
                   │
                   ▼
          Personalized Learning
```

### AI Roadmap Flow

```text
User enters topic
       ↓
Create embedding
       ↓
Search Qdrant for similar roadmap
       ↓
Similar roadmap found?
    ↙           ↘
  Yes            No
   ↓              ↓
Return         Groq LLM
existing       generates
roadmap        roadmap
                  ↓
             Save in PostgreSQL
                  ↓
             Store embedding
                  ↓
               Qdrant
```

---

## 🤖 AI & Machine Learning

MindForge AI uses several AI technologies:

### Generative AI

**Groq LLM APIs** are used to generate:

* Learning roadmaps
* Quiz questions
* Learning content

### Embeddings

The project uses:

**`all-MiniLM-L6-v2`**

from Sentence Transformers to convert text into numerical vectors.

These embeddings allow the application to compare the similarity between learning topics and previously generated roadmaps.

### Vector Search

**Qdrant** is used as the vector database.

It stores roadmap embeddings and performs similarity searches using cosine similarity.

This provides a retrieval-based approach for reusing relevant roadmap information.

> Note: The current implementation uses semantic retrieval for roadmaps rather than a traditional document-based RAG pipeline.

---

## 🛠️ Tech Stack

| Category        | Technologies                            |
| --------------- | --------------------------------------- |
| Frontend        | React, Vite, React Router, Tailwind CSS |
| Backend         | Python, FastAPI, Uvicorn                |
| Database        | PostgreSQL                              |
| ORM             | SQLAlchemy                              |
| Migrations      | Alembic                                 |
| Vector Database | Qdrant                                  |
| AI / LLM        | Groq API                                |
| Embeddings      | Sentence Transformers                   |
| Authentication  | JWT, bcrypt                             |
| Cloud Storage   | AWS S3                                  |
| Video Resources | YouTube Data API                        |
| Visualization   | React Flow, Chart.js                    |
| Development     | Git, GitHub, Docker                     |

---

## 📂 Project Structure

```text
MindForge-AI/
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── database.py
│   │   ├── auth/
│   │   ├── routers/
│   │   ├── models/
│   │   ├── schemas/
│   │   └── crud/
│   │
│   ├── services/
│   │   ├── roadmap_service.py
│   │   ├── quiz_service.py
│   │   ├── qdrant_service.py
│   │   ├── youtube_service.py
│   │   └── content_service.py
│   │
│   ├── utils/
│   │   └── s3/
│   │
│   ├── alembic/
│   ├── requirements.txt
│   └── .env.example
│
└── roadmap-frontend/
    └── mindforge-full/
        ├── src/
        │   ├── pages/
        │   ├── components/
        │   └── services/
        │
        ├── package.json
        └── vite.config.js
```

---

## 🔑 Environment Variables

Create a `.env` file inside the backend directory.

Example:

```env
SECRET_KEY=your_secret_key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

DATABASE_URL=your_postgresql_database_url

GROQ_KEY=your_groq_api_key

YOUTUBE_API_KEY=your_youtube_api_key

AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_S3_REGION_NAME=your_aws_region
AWS_STORAGE_BUCKET_NAME=your_bucket_name
```

**Never commit API keys or `.env` files to GitHub.**

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/diyarawat21/MindForge-AI.git

cd MindForge-AI
```

---

### 2. Set up PostgreSQL

Create a PostgreSQL database:

```sql
CREATE DATABASE mindforge;
```

Configure the database connection in your backend environment variables.

---

### 3. Start Qdrant

The project uses Qdrant for vector search.

If Docker is installed:

```bash
docker run -p 6333:6333 -p 6334:6334 qdrant/qdrant
```

Qdrant will run locally on:

```text
http://localhost:6333
```

---

### 4. Set up the backend

Move into the backend directory:

```bash
cd backend
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

If additional packages are required for your local setup:

```bash
pip install fastapi uvicorn sqlalchemy psycopg2-binary python-dotenv passlib bcrypt alembic python-jose qdrant-client sentence-transformers requests openai boto3 pydantic python-multipart markdown2 xhtml2pdf
```

Run database migrations:

```bash
alembic upgrade head
```

Start the FastAPI server:

```bash
uvicorn app.main:app --reload
```

Backend:

```text
http://127.0.0.1:8000
```

API documentation:

```text
http://127.0.0.1:8000/docs
```

---

### 5. Start the frontend

Open another terminal:

```bash
cd roadmap-frontend/mindforge-full
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Open:

```text
http://localhost:5173
```

---

## 🔌 Main Backend APIs

The FastAPI backend provides APIs for different learning features.

| API                     | Purpose                         |
| ----------------------- | ------------------------------- |
| `/auth`                 | User authentication             |
| `/api/generate-roadmap` | Generate learning roadmap       |
| `/api/search-roadmap`   | Search similar roadmaps         |
| `/api/get-roadmap`      | Retrieve roadmap                |
| `/api/quiz`             | Generate and manage quizzes     |
| `/confusion-signals`    | Confusion and weak-topic data   |
| `/recall-cards`         | Recall card operations          |
| `/progress-card`        | Learning progress data          |
| `/skill-scan`           | Skill assessment                |
| `/learning-style`       | Learning style information      |
| `/knowledge-decay`      | Knowledge retention tracking    |
| `/api/youtube-links`    | Find YouTube learning resources |
| `/api/generate-pdf`     | Generate learning PDFs          |

---

## 👩‍💻 My Contribution

I worked mainly on the **AI and backend side** of MindForge AI.

My contributions include:

* Developed REST APIs using **FastAPI**
* Worked with **PostgreSQL and SQLAlchemy**
* Integrated **Groq LLM APIs** for AI-generated content
* Implemented **text embeddings using Sentence Transformers**
* Worked with **Qdrant for vector storage and similarity search**
* Built roadmap generation and retrieval functionality
* Worked on AI-generated quiz functionality
* Integrated learning resources such as YouTube and PDF generation
* Worked on authentication and backend services
* Integrated the React frontend with backend APIs

---

## 🎯 What I Learned

Working on MindForge AI helped me gain practical experience in:

* Generative AI application development
* LLM API integration
* Embeddings and vector search
* Retrieval-based AI systems
* REST API development
* FastAPI
* PostgreSQL
* Qdrant
* React
* Authentication
* AWS S3 integration
* Connecting AI systems with real-world applications

---

## 🔮 Future Improvements

Some areas that can be improved in future versions:

* Connect all personalization features directly to the frontend
* Improve authentication and user-specific data handling
* Add a complete document-based RAG pipeline
* Improve quiz and progress persistence
* Add more personalized recommendations
* Deploy the complete application
* Improve automated testing and API coverage

---

## 📌 Project Status

MindForge AI is a **full-stack AI learning application developed as a project**.

The core roadmap generation, semantic retrieval, AI quiz generation, learning resources, and FastAPI backend functionality are implemented. Some additional personalization and history features are still areas for further development.

---

## 📄 License

This project is licensed under the **MIT License**.

---

<p align="center">
  Built with Python, FastAPI, React, PostgreSQL, Qdrant and Generative AI.
</p>
