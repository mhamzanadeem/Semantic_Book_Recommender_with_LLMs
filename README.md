# Semantic Book Recommender with LLMs

A complete end-to-end semantic book recommendation system built with LLMs, vector search, text classification, and sentiment analysis. Based on the freeCodeCamp course "Build a Semantic Book Recommender with LLMs – Full Course".

## Project Overview

This project builds a semantic book recommender that allows users to find books using natural language queries (e.g., "a book about a person seeking revenge"). The system includes:

1. **Data Exploration & Cleaning** (`data-exploration.ipynb`) - Download and clean the 7K books dataset from Kaggle
2. **Vector Search** (`vector-search.ipynb`) - Build a Chroma vector database with OpenAI embeddings for semantic search
3. **Text Classification** (`text-classification.ipynb`) - Classify books as Fiction/Non-fiction using zero-shot classification
4. **Sentiment Analysis** (`sentiment-analysis.ipynb`) - Extract emotions (joy, sadness, anger, fear, surprise, disgust) from book descriptions
5. **Gradio Dashboard** (`gradio-dashboard.py`) - Web UI for book recommendations with category and emotional tone filters

## File Structure

```
Semantic_Book_Recommender_with_LLMs/
├── data-exploration.ipynb       # Data download, cleaning, EDA
├── vector-search.ipynb          # Vector DB creation with Chroma + OpenAI embeddings
├── text-classification.ipynb    # Zero-shot classification (Fiction/Non-fiction)
├── sentiment-analysis.ipynb     # Emotion extraction from descriptions
├── gradio-dashboard.py          # Web application for recommendations
├── requirements.txt             # Python dependencies
├── .env                         # OpenAI API key (create this)
├── books.csv                    # Raw Kaggle dataset (downloaded)
├── books_cleaned.csv            # Cleaned dataset (output of data-exploration)
├── books_with_categories.csv    # Dataset with simple_categories (output of text-classification)
├── books_with_emotions.csv      # Final dataset with emotion scores (output of sentiment-analysis)
├── tagged_description.txt       # ISBN + description for vector DB (output of vector-search)
└── chroma_db/                   # Chroma vector database (auto-created)
```

## Prerequisites

- Python 3.11+
- OpenAI API key (for embeddings)
- Kaggle account (for dataset download)

## Setup Instructions

### 1. Clone Repository

```bash
# Bash
git clone <repository-url>
cd Semantic_Book_Recommender_with_LLMs
```

```powershell
# PowerShell
git clone <repository-url>
cd Semantic_Book_Recommender_with_LLMs
```

### 2. Create Virtual Environment

```bash
# Bash
python -m venv venv
source venv/bin/activate
```

```powershell
# PowerShell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
# Bash
pip install -r requirements.txt
```

```powershell
# PowerShell
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root with your OpenAI API key:

```bash
# Bash
echo "OPENAI_API_KEY=your-openai-api-key-here" > .env
```

```powershell
# PowerShell
"OPENAI_API_KEY=your-openai-api-key-here" | Out-File -FilePath .env -Encoding utf8
```

**Get your OpenAI API key from:** https://platform.openai.com/api-keys

### 5. Kaggle Setup (for data download)

The `data-exploration.ipynb` uses `kagglehub` to download the dataset automatically. No manual Kaggle setup required - `kagglehub` handles authentication.

If you prefer manual download:
1. Go to https://www.kaggle.com/datasets/dylanjcastillo/7k-books-with-metadata
2. Download `books.csv`
3. Place in project root

## Running the Pipeline

### Step 1: Data Exploration & Cleaning

**File:** `data-exploration.ipynb`

```bash
# Bash
jupyter notebook data-exploration.ipynb
```

```powershell
# PowerShell
jupyter notebook data-exploration.ipynb
```

**What it does:**
- Downloads the 7K books dataset from Kaggle using `kagglehub`
- Explores data: missing values, distributions, correlations
- Cleans data: removes rows with missing descriptions, pages, ratings, or publication year
- Adds derived columns: `age_of_book`, `words_in_description`
- Outputs: `books_cleaned.csv`

**Expected output:** `books_cleaned.csv` (~6,500 rows)

---

### Step 2: Vector Search & Database Creation

**File:** `vector-search.ipynb`

```bash
# Bash
jupyter notebook vector-search.ipynb
```

```powershell
# PowerShell
jupyter notebook vector-search.ipynb
```

**What it does:**
- Loads `books_cleaned.csv`
- Creates `tagged_description` column (ISBN + description)
- Exports to `tagged_description.txt`
- Creates Chroma vector database with OpenAI embeddings
- Demonstrates semantic similarity search

**Prerequisites:** 
- `.env` with `OPENAI_API_KEY`
- `books_cleaned.csv` from Step 1

**Expected outputs:**
- `tagged_description.txt`
- `chroma_db/` directory (vector database)

---

### Step 3: Text Classification (Fiction/Non-fiction)

**File:** `text-classification.ipynb`

```bash
# Bash
jupyter notebook text-classification.ipynb
```

```powershell
# PowerShell
jupyter notebook text-classification.ipynb
```

**What it does:**
- Loads `books_cleaned.csv`
- Uses zero-shot classification (via transformers) to categorize books as "Fiction" or "Non-fiction"
- Adds `simple_categories` column
- Outputs: `books_with_categories.csv`

**Expected output:** `books_with_categories.csv` with `simple_categories` column

---

### Step 4: Sentiment Analysis (Emotion Extraction)

**File:** `sentiment-analysis.ipynb`

```bash
# Bash
jupyter notebook sentiment-analysis.ipynb
```

```powershell
# PowerShell
jupyter notebook sentiment-analysis.ipynb
```

**What it does:**
- Loads `books_with_categories.csv`
- Uses `j-hartmann/emotion-english-distilroberta-base` model to extract emotions from descriptions
- Splits descriptions into sentences, classifies each, takes max score per emotion
- Adds 7 emotion columns: `anger`, `disgust`, `fear`, `joy`, `sadness`, `surprise`, `neutral`
- Outputs: `books_with_emotions.csv`

**Note:** This step takes ~10 minutes (processes 5,197 books). GPU recommended (uses MPS on Mac, CUDA on Linux/Windows).

**Expected output:** `books_with_emotions.csv` with emotion scores

---

### Step 5: Launch Gradio Dashboard

**File:** `gradio-dashboard.py`

```bash
# Bash
python gradio-dashboard.py
```

```powershell
# PowerShell
python gradio-dashboard.py
```

**What it does:**
- Loads `books_with_emotions.csv` and `tagged_description.txt`
- Initializes Chroma vector store with OpenAI embeddings
- Launches web UI at http://localhost:7860

**Features:**
- Natural language query input (e.g., "a story about forgiveness")
- Category filter: All, Fiction, Nonfiction
- Emotional tone filter: All, Happy, Surprising, Angry, Suspenseful, Sad
- Displays book covers with titles, authors, and truncated descriptions

**Prerequisites:**
- All previous steps completed
- `.env` with `OPENAI_API_KEY`
- `books_with_emotions.csv` and `tagged_description.txt` exist

---

## Complete Pipeline Commands

### Bash (Linux/macOS/WSL)

```bash
# Full setup
git clone <repo-url>
cd Semantic_Book_Recommender_with_LLMs
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
echo "OPENAI_API_KEY=your-key" > .env

# Run notebooks in order (or use jupyter lab)
jupyter notebook data-exploration.ipynb
jupyter notebook vector-search.ipynb
jupyter notebook text-classification.ipynb
jupyter notebook sentiment-analysis.ipynb

# Launch app
python gradio-dashboard.py
```

### PowerShell (Windows)

```powershell
# Full setup
git clone <repo-url>
cd Semantic_Book_Recommender_with_LLMs
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
"OPENAI_API_KEY=your-key" | Out-File -FilePath .env -Encoding utf8

# Run notebooks in order
jupyter notebook data-exploration.ipynb
jupyter notebook vector-search.ipynb
jupyter notebook text-classification.ipynb
jupyter notebook sentiment-analysis.ipynb

# Launch app
python gradio-dashboard.py
```

---

## Data Files Reference

| File | Description | Generated By |
|------|-------------|--------------|
| `books.csv` | Raw Kaggle dataset (6,810 books) | Kaggle download |
| `books_cleaned.csv` | Cleaned data (~6,500 books) | `data-exploration.ipynb` |
| `books_with_categories.csv` | + `simple_categories` (Fiction/Non-fiction) | `text-classification.ipynb` |
| `books_with_emotions.csv` | + 7 emotion scores | `sentiment-analysis.ipynb` |
| `tagged_description.txt` | ISBN + description for vector DB | `vector-search.ipynb` |
| `chroma_db/` | Vector database | `vector-search.ipynb` / `gradio-dashboard.py` |

---

## Key Dependencies

| Package | Purpose |
|---------|---------|
| `kagglehub` | Dataset download |
| `pandas`, `numpy` | Data manipulation |
| `matplotlib`, `seaborn` | Visualization |
| `langchain-chroma`, `langchain-openai` | Vector DB + embeddings |
| `transformers` | Zero-shot classification, emotion model |
| `gradio` | Web UI |
| `python-dotenv` | Environment variables |

---

## Troubleshooting

### OpenAI API Errors
- Verify `.env` file exists and contains valid `OPENAI_API_KEY`
- Check API key has credits at https://platform.openai.com/usage

### Chroma DB Issues
- Delete `chroma_db/` folder and re-run `vector-search.ipynb`
- Ensure `tagged_description.txt` exists

### GPU/MPS for Sentiment Analysis
- Mac: Uses MPS automatically (set `device="mps"` in notebook)
- Linux/Windows: Change `device="cuda"` if GPU available, or `device="cpu"`

### Port 7860 in Use
- Gradio will auto-select next available port (7861, 7862, etc.)
- Or set custom port: `dashboard.launch(server_port=7865)`

