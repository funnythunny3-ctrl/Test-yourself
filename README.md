# Test-yourself
Application 
app.py
database.py
pdf_extractor.py
question_parser.py
ai_explainer.py
requirements.txt
data/
import pdfplumber

def extract_text_from_pdf(pdf_path):
    text = ""
    with pdfplumber.open(pdf_path) as pdf:
        for page in pdf.pages:
            page_text = page.extract_text()
            if page_text:
                text += page_text + "\n"
    return text
import sqlite3
import os

DB_PATH = os.path.join(os.getcwd(), "app.db")

def get_connection():
    return sqlite3.connect(DB_PATH, check_same_thread=False)

def create_tables():
    conn = get_connection()
    cur = conn.cursor()

    cur.execute("""
    CREATE TABLE IF NOT EXISTS questions (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        subject TEXT,
        question TEXT,
        option_a TEXT,
        option_b TEXT,
        option_c TEXT,
        option_d TEXT,
        correct_option TEXT,
        explanation TEXT
    )
    """)

    cur.execute("""
    CREATE TABLE IF NOT EXISTS test_results (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        test_date TEXT,
        total_questions INTEGER,
        attempted INTEGER,
        correct INTEGER,
        accuracy REAL
    )
    """)

    conn.commit()
    conn.close()
