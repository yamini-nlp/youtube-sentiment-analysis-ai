# 🎥 YouTube Sentiment Analysis Using AI

An end-to-end machine learning system for analyzing sentiment in YouTube video comments and transcripts using deep learning and NLP techniques.

Built using Flask, MySQL, TensorFlow/Keras, and NLTK.

---

## 1️⃣ Problem Statement

Online video platforms like YouTube generate massive volumes of user comments that reflect public opinion, audience engagement, and emotional reactions.

Manually analyzing this content is inefficient and unscalable.

This project aims to build a system that:

* Automatically classifies YouTube comments into sentiment categories (Positive, Negative, Neutral)
* Analyzes sentiment of video transcripts
* Provides visual sentiment distribution insights
* Integrates ML models into an interactive web application

---

## 2️⃣ Why It Matters

Sentiment analysis of user-generated content is valuable for:

* Content creators measuring audience feedback
* Brand monitoring and reputation analysis
* Public opinion research
* Social media analytics

This project demonstrates how deep learning models can be deployed in real-world content analysis systems.

---

## 3️⃣ Dataset

Primary dataset used for training:

* `GBcomments.csv`

The dataset contains labeled YouTube comments used for supervised sentiment classification.

Preprocessing included:

* Text cleaning (regex-based filtering)
* Stopword removal
* Lemmatization
* Tokenization
* Sequence padding using Keras Tokenizer

Transcript data is fetched dynamically using:

* `youtube_transcript_api`

Comment data is fetched using:

* YouTube Data API v3

---

## 4️⃣ Methodology

The system consists of two sentiment analysis pipelines:

### A) Comment Sentiment Classification

1. Fetch comments via YouTube Data API.
2. Apply text preprocessing.
3. Convert text to padded sequences.
4. Pass sequences into trained LSTM model.
5. Generate multi-class sentiment prediction.
6. Aggregate results for visualization.

### B) Transcript Sentiment Analysis

1. Extract transcript using `youtube_transcript_api`.
2. Apply VADER sentiment analysis.
3. Compute overall sentiment distribution.
4. Visualize results using Matplotlib.

The application integrates:

Frontend → Flask Backend → ML/NLP Processing → Visualization Output

---

## 5️⃣ Model Architecture

### LSTM Model (Primary Model)

* Embedding layer
* LSTM layer
* Dense layer
* Softmax output layer (multi-class classification)

Additional experimental notebooks include:

* GRU-based model
* BERT-based experimentation

Model trained using:

* TensorFlow 2.15.0
* Keras 2.15.0
* Cross-entropy loss
* Multi-class classification

Saved model file:

```
Models/lstm_sentiment_model.h5
```

---

## 6️⃣ Results

The trained LSTM model successfully classifies comments into:

* Positive
* Negative
* Neutral

The system provides:

* Sentiment distribution pie charts
* Individual comment sentiment predictions
* Transcript-level sentiment analysis

Experimental comparisons were conducted in Jupyter notebooks for LSTM, GRU, and BERT-based approaches.

(Quantitative metrics can be referenced in the associated experimental notebooks.)

---

## 7️⃣ Limitations

* Model trained on a specific dataset (GBcomments), limiting generalization.
* No large-scale hyperparameter optimization performed.
* BERT experimentation not fully integrated into production system.
* Transcript analysis relies on lexicon-based VADER (rule-based).
* Local deployment only (no cloud hosting).

---

## 8️⃣ Future Work

* Integrate fine-tuned transformer-based models (e.g., BERT)
* Deploy as a cloud-based scalable service
* Add model evaluation dashboard (accuracy, F1-score visualization)
* Improve database security (password hashing)
* Containerize using Docker
* Extend to multilingual sentiment analysis

---

## 9️⃣ How to Run

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/Youtube_Sentiment_Analysis_AI.git
cd Youtube_Sentiment_Analysis_AI
```

---

### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # Mac/Linux
venv\Scripts\activate     # Windows
```

---

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

### 4. Setup MySQL Database

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

Or run:

```bash
source db.sql;
```

---

### 5. Add Trained Model

Ensure:

```
Models/lstm_sentiment_model.h5
```

If not available, train using `notebooks/LSTM.ipynb` and save:

```python
model.save("lstm_sentiment_model.h5")
```

---

### 6. Add YouTube API Key

In `app.py`:

```python
api_key = "YOUR_YOUTUBE_API_KEY"
```

---

### 7. Run Application

```bash
python app.py
```

Visit:

```
http://127.0.0.1:5000/
```
