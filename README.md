# 🎥 YouTube Sentiment Analysis

> An end-to-end machine learning system for classifying sentiment in YouTube video comments and transcripts using deep learning and NLP, with a Flask web interface for interactive analysis.

![Stack](https://img.shields.io/badge/Stack-Flask%20%7C%20TensorFlow%20%7C%20Keras-blue?style=flat-square)
![NLP](https://img.shields.io/badge/NLP-NLTK%20%7C%20VADER-orange?style=flat-square)
![DB](https://img.shields.io/badge/Database-MySQL-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Local%20Only-lightgrey?style=flat-square)

---

## Overview

This project builds a sentiment analysis pipeline for YouTube content, combining an LSTM-based classifier for comment-level sentiment prediction with VADER-based transcript analysis. Users submit a YouTube URL and receive a sentiment distribution across Positive, Negative, and Neutral categories, rendered as visualisations in a Flask web application.

> ⚠️ **Deployment note:** This application runs locally only. Cloud hosting and production hardening are identified as future work.

---

## 🎯 Problem Statement

YouTube generates large volumes of user comments that reflect public opinion, audience engagement, and emotional reactions. Manually analysing this content is inefficient and unscalable. This project explores automated multi-class sentiment classification on real YouTube comment data, with comparison to lexicon-based transcript analysis.

---

## 🏗️ Two-Pipeline Architecture

**Pipeline A — Comment Sentiment Classification**

1. Fetch comments via YouTube Data API v3
2. Apply text preprocessing (regex cleaning, stopword removal, lemmatisation)
3. Convert cleaned text to padded sequences using the trained tokeniser
4. Pass sequences to the trained LSTM model
5. Generate per-comment sentiment predictions
6. Aggregate results and render visualisation

**Pipeline B — Transcript Sentiment Analysis**

1. Extract transcript using `youtube_transcript_api`
2. Apply VADER sentiment analysis on the full transcript text
3. Compute overall positive / negative / neutral distribution
4. Visualise results using Matplotlib

```
User Input (YouTube URL)
        │
        ├─► [Comment Pipeline]
        │   YouTube Data API → Preprocessing → LSTM → Sentiment Labels
        │
        └─► [Transcript Pipeline]
            youtube_transcript_api → VADER → Sentiment Distribution
                    │
                    ▼
          Flask Backend → Matplotlib Visualisation → Frontend Rendering
```

---

## ⚙️ Model Architecture

**Primary model: LSTM**

| Layer | Type |
|---|---|
| Input | Padded token sequences |
| Embedding | Trainable embedding layer |
| Recurrent | LSTM layer |
| Output | Dense + Softmax (Positive / Negative / Neutral) |

| Parameter | Value |
|---|---|
| Framework | TensorFlow 2.15.0 / Keras 2.15.0 |
| Loss function | Categorical cross-entropy |
| Saved model | `Models/lstm_sentiment_model.h5` |

**Additional experimental models** (Jupyter notebooks only; not integrated into the application):
- GRU-based model (`notebooks/GRU.ipynb`)
- BERT-based experimentation (`notebooks/BERT.ipynb`)

---

## 📋 Dataset

| Dataset | Format | Task |
|---|---|---|
| `GBcomments.csv` | Labelled YouTube comments | Supervised multi-class sentiment classification |

**Preprocessing pipeline:**
1. Regex-based text cleaning
2. Stopword removal
3. Lemmatisation
4. Tokenisation using Keras `Tokenizer`
5. Sequence padding to fixed input length

---

## ⚠️ Limitations

- **Dataset generalisation:** model trained on `GBcomments.csv`; performance may vary on comments from different regions or content niches
- **No hyperparameter optimisation:** large-scale tuning was not performed
- **BERT not integrated:** BERT experimentation was conducted in notebooks only and is not deployed in the application
- **Lexicon-based transcript analysis:** VADER does not capture contextual nuance; qualitative accuracy on complex or ironic text is limited
- **Local deployment only:** no cloud hosting; the application runs locally
- **No quantitative test-set metrics reported:** accuracy and F1-score figures from notebook experiments are available in the associated notebooks but not reproduced here
- **Password storage:** user passwords are stored without hashing in the current version — a known security deficiency, identified for remediation before any deployment

---

## 🔭 Future Work

- Integrate fine-tuned transformer models (BERT, RoBERTa) for improved comment classification
- Deploy as a cloud-based scalable service
- Add a model evaluation dashboard with accuracy, F1-score, and confusion matrix visualisation
- Implement password hashing and improved database security before any deployment
- Containerise using Docker for consistent deployment
- Extend to multilingual comment sentiment analysis
- Add time-series sentiment tracking for channels and creators over time

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Flask (Python) |
| Deep Learning | TensorFlow 2.15.0, Keras |
| NLP | NLTK, VADER |
| Database | MySQL |
| YouTube Data | YouTube Data API v3, youtube_transcript_api |
| Visualisation | Matplotlib |
| Experimental Models | LSTM, GRU, BERT (Jupyter notebooks) |

---

## 🚀 Local Setup

**Prerequisites:** Python 3.8+ · MySQL · YouTube Data API v3 key · LSTM model file

**1. Clone**
```bash
git clone https://github.com/yamini-nlp/Youtube_Sentiment_Analysis_AI.git
cd Youtube_Sentiment_Analysis_AI
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

Ensure `Models/lstm_sentiment_model.h5` is present. If not available, train using `notebooks/LSTM.ipynb` and save:
```python
model.save("lstm_sentiment_model.h5")
```

**6. Add YouTube API key**

In `app.py`:
```python
api_key = "YOUR_YOUTUBE_API_KEY"
```

**7. Run application**
```bash
python app.py
```
Visit `http://127.0.0.1:5000/`

---

## 📁 Repository Structure

```
Youtube_Sentiment_Analysis_AI/
├── app.py                          # Flask application entry point
├── Models/
│   └── lstm_sentiment_model.h5     # Trained LSTM model
├── notebooks/
│   ├── LSTM.ipynb                  # LSTM training + evaluation
│   ├── GRU.ipynb                   # GRU comparison experiment
│   └── BERT.ipynb                  # BERT experimentation
├── templates/                      # Flask HTML templates
├── static/                         # CSS and JS assets
├── db.sql                          # Database setup script
├── requirements.txt
└── README.md
```

---

*Built by Yamini G &nbsp;·&nbsp; [GitHub](https://github.com/yamireddy04/Youtube_Sentiment_Analysis_AI)*
