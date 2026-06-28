# 🎥 YouTube Sentiment Analysis

**Dual-model sentiment pipeline** — LSTM-based comment classification and VADER-based transcript analysis — built to document divergence patterns between creator content and audience response across five content domains, with findings linked to an SSRN preprint.

![Stack](https://img.shields.io/badge/Stack-Flask%20%7C%20TensorFlow%20%7C%20Keras-blue?style=flat-square)
![NLP](https://img.shields.io/badge/NLP-NLTK%20%7C%20VADER-orange?style=flat-square)
![DB](https://img.shields.io/badge/Database-MySQL-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Local%20Only-lightgrey?style=flat-square)

---

## 💡 Motivation

YouTube generates large volumes of comments and transcripts that carry very different kinds of sentiment signal — one from the audience, one from the creator. Most sentiment tools treat these as interchangeable. This project treats them as distinct: it runs two separate pipelines on the same video, compares their outputs, and documents where and why they diverge. The divergence patterns across five content domains are the subject of the associated SSRN preprint.

---

## 🧭 Overview

A user submits a YouTube URL and receives two independent sentiment analyses:

- **Comment pipeline:** fetches up to 100 comments via the YouTube Data API v3, preprocesses them, runs them through a trained LSTM classifier, and returns a Positive / Negative / Neutral distribution rendered as a pie chart.
- **Transcript pipeline:** extracts the video transcript via `youtube_transcript_api`, preprocesses it, and runs VADER token-level sentiment analysis, returning the same three-class distribution.

The application is a Flask web app with MySQL-backed user authentication, running locally only.

---

## 🎯 Problem Statement

YouTube comments and creator transcripts reflect fundamentally different perspectives on the same content — one from the audience, one from the creator. Analysing them together, to find where they agree or diverge, is rarely attempted in practice. This project automates both pipelines simultaneously and measures the gap, providing a foundation for studying creator-audience sentiment divergence across content categories.

---

## 🏗️ Two-Pipeline Architecture

### Pipeline A — Comment Sentiment (LSTM)

```
YouTube URL
    │
    ├─► Extract video ID (regex)
    ├─► YouTube Data API v3 → fetch up to 100 top-level comments
    ├─► Preprocessing: lowercase → URL/emoji removal → regex cleaning → stopword removal
    ├─► Tokeniser (Keras, fitted on GBcomments.csv, vocab size 5000) → pad to length 100
    ├─► LSTM model inference → argmax → per-comment label
    └─► Aggregate counts → Positive / Negative / Neutral % → Matplotlib pie chart
```

### Pipeline B — Transcript Sentiment (VADER)

```
YouTube URL
    │
    ├─► Extract video ID (regex)
    ├─► youtube_transcript_api → raw transcript text
    ├─► Preprocessing: sentence tokenisation → word tokenisation → lowercasing
    │   → stopword removal → lemmatisation → numeral replacement → punctuation removal
    ├─► VADER SentimentIntensityAnalyzer → per-token compound score
    │   (positive: compound > 0.05 · negative: compound < -0.05 · neutral: otherwise)
    └─► Aggregate token counts → Positive / Negative / Neutral % → Matplotlib pie chart
```

---

## ⚙️ Model Architecture

### Primary model: LSTM (deployed in application)

| Layer | Detail |
|---|---|
| Input | Padded token sequences (max length 100) |
| Embedding | Trainable, vocabulary size 5000 |
| Recurrent | LSTM layer |
| Output | Dense + Softmax — 3 classes (Positive / Negative / Neutral) |

| Parameter | Value |
|---|---|
| Framework | TensorFlow 2.15.0 / Keras 2.15.0 |
| Loss | Categorical cross-entropy |
| Saved model | `Models/lstm_sentiment_model.h5` |

### Additional experimental models (notebooks only; not deployed)

| Model | Notebook |
|---|---|
| GRU | `notebooks/GRU.ipynb` |
| BERT | `notebooks/BERT.ipynb` |

---

## 📋 Dataset

| Dataset | Format | Task |
|---|---|---|
| `GBcomments.csv` | Labelled YouTube comments | Supervised multi-class sentiment classification |

**Preprocessing pipeline (comments):** regex cleaning → URL/emoji removal → stopword removal → Keras tokenisation (vocab 5000) → padding (length 100)

**Preprocessing pipeline (transcripts):** sentence tokenisation → lowercasing → stopword removal → lemmatisation → numeral replacement → punctuation removal

---

## 📊 Findings

The two pipelines consistently produce divergent sentiment distributions on the same video — the LSTM comment classifier and VADER transcript analyser disagree in both direction and magnitude across content categories. Divergence patterns across five content domains are documented in the associated SSRN preprint.

**Scope:** distribution-level sentiment only. Semantic quality, causal explanations for divergence, and cross-platform generalisation are addressed in the preprint, not the pipeline.

---

## ⚙️ Key Design Decisions

| Component | Choice | Rationale |
|---|---|---|
| Comment classifier | LSTM (TensorFlow/Keras) | Sequence model suited to variable-length comment text; trained on domain-relevant labelled data |
| Transcript analyser | VADER | Lexicon-based, no inference cost, operates on preprocessed tokens without retraining |
| Visualisation | Matplotlib → base64 PNG | Chart rendered server-side and embedded inline in Flask template; no static file serving required |
| Database | MySQL | Authentication only; lightweight relational store sufficient for session management |
| Experimental models | GRU, BERT (notebooks only) | Comparison experiments conducted during development; not integrated due to deployment scope |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Flask (Python) |
| Deep Learning | TensorFlow 2.15.0, Keras |
| NLP | NLTK, VADER SentimentIntensityAnalyzer |
| YouTube Data | YouTube Data API v3, youtube_transcript_api |
| Visualisation | Matplotlib (base64-encoded pie charts) |
| Database | MySQL (user authentication) |
| Experimental | LSTM, GRU, BERT (Jupyter notebooks) |

---

## 🔒 Security

- API key is currently hardcoded in `app.py` — must be moved to an environment variable before any sharing or deployment.
- Passwords are stored in plaintext in MySQL — bcrypt hashing is identified for remediation before deployment.
- No production hardening applied; intended for local use only.

---

## ⚠️ Limitations

- **VADER is lexicon-based** — does not capture sarcasm, irony, or contextual nuance.
- **Dataset scope** — LSTM trained on `GBcomments.csv`; performance may vary across regions and content niches.
- **BERT not integrated** — experimentation conducted in notebooks only.
- **Tokeniser re-fitted at startup** — risks subtle inconsistency with the saved model if the dataset changes.
- **No quantitative metrics reported** — accuracy and F1 figures are available in the notebooks but not reproduced here.
- **Local deployment only** — no cloud hosting.

---

## 🔭 Future Work

- Save and load tokeniser from a pickle file rather than re-fitting at startup
- Integrate fine-tuned BERT or RoBERTa for comment classification
- Move credentials to environment variables; implement password hashing
- Deploy as a cloud-based service with Docker
- Add a model evaluation dashboard (accuracy, F1, confusion matrix)
- Extend to multilingual comments and time-series sentiment tracking

---

## 🚀 Local Setup

**Prerequisites:** Python 3.8+ · MySQL · YouTube Data API v3 key · trained LSTM model file

**1. Clone**
```bash
git clone https://github.com/yamini-nlp/youtube-sentiment-analysis-ai.git
cd youtube-sentiment-analysis-ai
```

**2. Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate    # Windows: venv\Scripts\activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Set up MySQL database**
```sql
CREATE DATABASE youtube;
USE youtube;

CREATE TABLE users (
    id       INT PRIMARY KEY AUTO_INCREMENT,
    name     VARCHAR(1000),
    email    VARCHAR(1000),
    password VARCHAR(225)
);
```
Or run: `source db.sql`

**5. Add trained model**

Ensure `Models/lstm_sentiment_model.h5` is present. If not, train using `notebooks/LSTM.ipynb` and save:
```python
model.save("lstm_sentiment_model.h5")
```

**6. Add your YouTube API key**

In `app.py`, replace the hardcoded key:
```python
api_key = "YOUR_YOUTUBE_DATA_API_V3_KEY"
```

**7. Add dataset**

Place `GBcomments.csv` in a `Dataset/` directory at the project root.

**8. Run**
```bash
python app.py
```
Visit `http://127.0.0.1:5000/`

---

## 📁 Repository Structure

```
youtube-sentiment-analysis-ai/
├── app.py                          # Flask application — routes, pipelines, auth
├── Models/
│   └── lstm_sentiment_model.h5     # Trained LSTM model
├── notebooks/
│   ├── LSTM.ipynb                  # LSTM training and evaluation
│   ├── GRU.ipynb                   # GRU comparison experiment
│   └── BERT.ipynb                  # BERT experimentation
├── templates/                      # Flask HTML templates
├── static/                         # CSS and JS assets
├── db.sql                          # Database setup script
├── requirements.txt
└── README.md
```

---

## 📄 Publication

Findings from this pipeline are documented in a preprint available on SSRN:

> **YouTube Transcript vs Comment Sentiment** — dual-model pipeline documenting divergence patterns across five content domains; failure modes in public discourse analysis.
> SSRN · [DOI](http://dx.doi.org/10.2139/ssrn.6344859) · [PDF](paper/youtube-sentiment-interpretability.pdf)

---

<div align="center">

Built by Yamini G

</div>
