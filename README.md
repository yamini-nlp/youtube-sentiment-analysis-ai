# 🎥 YouTube Sentiment Analysis Using AI

![Stack](https://img.shields.io/badge/Stack-Python%20%7C%20Flask%20%7C%20TensorFlow%20%7C%20Keras%20%7C%20NLTK-1b2e2b?style=flat-square)
![Model](https://img.shields.io/badge/Model-LSTM%20%7C%20GRU%20%7C%20BERT-d9c5b2?style=flat-square)
![Database](https://img.shields.io/badge/Database-MySQL-4479a1?style=flat-square)
![APIs](https://img.shields.io/badge/APIs-YouTube%20Data%20API%20v3-ff0000?style=flat-square)
![Status](https://img.shields.io/badge/Status-Active-7ecb84?style=flat-square)

An end-to-end machine learning system for analyzing sentiment in YouTube video comments and transcripts using deep learning and NLP techniques.

Built with **Flask** (Python) for the backend, **TensorFlow / Keras** for model training, and a **MySQL** database for user management.

---

## 1️⃣ Problem Statement

Online video platforms like YouTube generate massive volumes of user comments that reflect public opinion, audience engagement, and emotional reactions. Manually analyzing this content is inefficient and unscalable.

This project builds a system that:

- Automatically classifies YouTube comments into sentiment categories (Positive, Negative, Neutral)
- Analyzes sentiment across full video transcripts
- Provides visual sentiment distribution insights
- Integrates trained ML models into an interactive web application

---

## 2️⃣ Why It Matters

Sentiment analysis of user-generated content has measurable value across multiple domains:

- **Content creators** measuring authentic audience feedback and engagement signals
- **Brand monitoring** and reputation analysis at scale
- **Public opinion research** tracking sentiment shifts over time
- **Social media analytics** platforms requiring automated classification pipelines

This project demonstrates how deep learning models trained on real-world comment data can be deployed into production-grade content analysis systems, replacing manual review workflows with scalable automation.

---

## 3️⃣ Dataset

**Primary Training Dataset**

| Dataset | Format | Task |
|---|---|---|
| GBcomments.csv | Labeled YouTube comments | Supervised multi-class sentiment classification |

**Dynamic Runtime Data**

- Video transcripts fetched dynamically via `youtube_transcript_api`
- Comment data retrieved in real time using **YouTube Data API v3**

**Preprocessing Pipeline**

- Text cleaning via regex-based filtering
- Stopword removal
- Lemmatization
- Tokenization using Keras Tokenizer
- Sequence padding to fixed input length

---

## 4️⃣ Methodology

The system consists of two independent sentiment analysis pipelines.

**Pipeline A: Comment Sentiment Classification**

1. Fetch comments via YouTube Data API.
2. Apply text preprocessing (cleaning, stopword removal, lemmatization).
3. Convert cleaned text to padded sequences using the trained tokenizer.
4. Pass sequences into the trained LSTM model.
5. Generate multi-class sentiment predictions per comment.
6. Aggregate results and render visualization.

**Pipeline B: Transcript Sentiment Analysis**

1. Extract transcript using `youtube_transcript_api`.
2. Apply VADER sentiment analysis on the full transcript text.
3. Compute overall positive / negative / neutral distribution.
4. Visualize results using Matplotlib.

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
          Flask Backend → Visualization Output → Frontend Rendering
```

---

## 5️⃣ Model Architecture

**Primary Model: LSTM**

| Layer | Type |
|---|---|
| Input | Padded token sequences |
| Embedding | Trainable embedding layer |
| Recurrent | LSTM layer |
| Output | Dense + Softmax (multi-class) |

**Training Configuration**

| Parameter | Value |
|---|---|
| Framework | TensorFlow 2.15.0 / Keras 2.15.0 |
| Loss Function | Categorical cross-entropy |
| Task | Multi-class classification (Positive / Negative / Neutral) |
| Saved Model | `Models/lstm_sentiment_model.h5` |

**Additional Experimental Models**

- GRU-based model (Jupyter notebook)
- BERT-based experimentation (Jupyter notebook)

Both were evaluated experimentally and are available in the associated notebooks for comparison.

---

## 6️⃣ Results

The trained LSTM model successfully classifies YouTube comments into three sentiment categories: Positive, Negative, and Neutral.

**System Outputs**

- Sentiment distribution pie charts per video
- Individual comment-level sentiment predictions
- Transcript-level overall sentiment analysis

**Experimental Comparisons**

LSTM, GRU, and BERT-based approaches were compared in Jupyter notebooks. LSTM demonstrated the best balance of training speed and classification performance on the GBcomments dataset.

> Quantitative accuracy and F1-score metrics are available in the associated experimental notebooks.

---

## 7️⃣ Limitations

- **Dataset Generalization:** Model trained on GBcomments dataset; performance may vary on comments from different regions or content niches.
- **No Hyperparameter Optimization:** Large-scale tuning was not performed in this version.
- **BERT Not Production-Integrated:** BERT experimentation was conducted in notebooks only and is not deployed in the live application.
- **Rule-Based Transcript Analysis:** Transcript sentiment relies on lexicon-based VADER, which cannot capture contextual nuance.
- **Local Deployment Only:** No cloud hosting; the application runs locally.
- **Database Security:** Password hashing is not implemented in the current user management system.

---

## 8️⃣ Future Work

- Integrate fine-tuned transformer models (BERT, RoBERTa) for improved comment classification
- Deploy as a cloud-based scalable service (AWS / GCP / Azure)
- Add a model evaluation dashboard with accuracy, F1-score, and confusion matrix visualization
- Implement password hashing and improved database security
- Containerize using Docker for consistent deployment
- Extend to multilingual comment sentiment analysis
- Add time-series sentiment tracking for channels and creators over time

---

## 9️⃣ How to Run

**1. Clone the Repository**

```bash
git clone https://github.com/your-username/Youtube_Sentiment_Analysis_AI.git
cd Youtube_Sentiment_Analysis_AI
```

**2. Create Virtual Environment**

```bash
python -m venv venv
source venv/bin/activate   # Mac / Linux
venv\Scripts\activate      # Windows
```

**3. Install Dependencies**

```bash
pip install -r requirements.txt
```

**4. Setup MySQL Database**

```sql
CREATE DATABASE youtube;
USE youtube;

CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(1000),
    email VARCHAR(1000),
    password VARCHAR(225)
);
```

Or run: `source db.sql`

**5. Add Trained Model**

Ensure `Models/lstm_sentiment_model.h5` is present. If not available, train using `notebooks/LSTM.ipynb` and save:

```python
model.save("lstm_sentiment_model.h5")
```

**6. Add YouTube API Key**

In `app.py`:

```python
api_key = "YOUR_YOUTUBE_API_KEY"
```

**7. Run Application**

```bash
python app.py
```

Visit: `http://127.0.0.1:5000/`

---

## 🔟 Conclusion

This project demonstrates how deep learning models can be integrated into a real-world content analysis pipeline to automate sentiment classification at scale. By combining an LSTM-based comment classifier with VADER-based transcript analysis, the system provides dual-signal sentiment insights for any YouTube video. The architecture cleanly separates data ingestion, model inference, and frontend rendering — establishing a modular foundation for future improvements including transformer-based classification, cloud deployment, and multilingual support.

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Backend | Flask (Python) |
| Deep Learning | TensorFlow 2.15.0, Keras |
| NLP | NLTK, VADER |
| Database | MySQL |
| YouTube Data | YouTube Data API v3, youtube_transcript_api |
| Visualization | Matplotlib |
| Experimental Models | LSTM, GRU, BERT (Jupyter notebooks) |
