# 🎥 YouTube Sentiment Analysis Using AI

This project analyzes the sentiment of YouTube video comments and video transcripts using artificial intelligence and natural language processing techniques.

It combines:

* A Flask-based backend application
* A MySQL database for user management
* An LSTM deep learning model for comment sentiment classification
* VADER (NLTK) for transcript sentiment analysis
* Matplotlib visualizations for sentiment distribution

The system runs locally and provides an interactive web interface for analyzing YouTube content.

---

## 📁 Project Structure

```
Youtube_Sentiment_Analysis_AI/
│
├── app.py
├── requirements.txt
├── db.sql
│
├── Models/
│   └── lstm_sentiment_model.h5
│
├── Dataset/
│   └── GBcomments.csv
│
├── templates/
│   ├── index.html
│   ├── login.html
│   ├── register.html
│   ├── home.html
│   ├── comment.html
│   ├── video.html
│   └── about.html
│
├── static/
│
├── notebooks/
│   ├── LSTM.ipynb
│   ├── GRU.ipynb
│   └── BERT.ipynb
│
├── README.md
└── .gitignore
```

---

## ✨ Features

* User registration and login system using MySQL
* Fetches YouTube comments using YouTube Data API v3
* Performs sentiment classification using a trained LSTM model
* Extracts video transcripts using `youtube_transcript_api`
* Performs transcript sentiment analysis using NLTK VADER
* Displays sentiment distribution using dynamically generated pie charts
* Local execution without external deployment

---

## 🛠️ Technologies Used

### ⚙️Backend

* Python 3.10.8
* Flask
* MySQL
* mysql-connector-python
* YouTube Data API v3
* youtube_transcript_api
* TensorFlow 2.15.0
* Keras 2.15.0
* NLTK 3.9.1
* NumPy 1.26.4
* Pandas 2.2.2
* Scikit-learn 1.2.2
* Matplotlib

### 🧠 Machine Learning

* LSTM (Long Short-Term Memory) Neural Network
* Keras Tokenizer
* Sequence Padding
* Multi-class Softmax Classification

### 🧾NLP Processing

* Tokenization
* Stopword Removal
* Lemmatization
* Regex-based text cleaning
* VADER Sentiment Analysis

---

## 🚀Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/Youtube_Sentiment_Analysis_AI.git
cd Youtube_Sentiment_Analysis_AI
```

---

### 2. Create a Virtual Environment (Recommended)

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

Login to MySQL:

```bash
mysql -u root -p
```

Create the database:

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

Or run the provided:

```bash
source db.sql;
```

---

### 5. Add the Trained LSTM Model

Ensure the following file exists:

```
Models/lstm_sentiment_model.h5
```

If not available, open `notebooks/LSTM.ipynb`, train the model, and run:

```python
model.save("lstm_sentiment_model.h5")
```

Move the saved file into the `Models/` directory.

---

### 6. Add Your YouTube API Key

In `app.py`, replace the API key:

```python
api_key = "YOUR_YOUTUBE_API_KEY"
```

with your own YouTube Data API v3 key.

---

### 7. Run the Application

```bash
python app.py
```

Open your browser:

```
http://127.0.0.1:5000/
```

---

## Sample YouTube Link Used

Example:

```
https://www.youtube.com/watch?v=9yJWo7jafv4
```

---

## 📊 Example Output

* Pie chart showing Positive, Negative, and Neutral sentiment distribution
* Sentiment analysis results for YouTube comments
* Sentiment distribution for video transcripts

---

## 🔮 Future Improvements

* Password hashing for enhanced security
* Transformer-based sentiment models (e.g., BERT)
* Docker containerization
* REST API modularization
* Enhanced frontend UI design
* Export/download sentiment reports

