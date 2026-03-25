# Potential Talents: AI-Powered Candidate Ranking for Talent Sourcing

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-blue?style=flat-square&logo=pandas)
![scikit--learn](https://img.shields.io/badge/scikit--learn-1.3%2B-orange?style=flat-square&logo=scikit-learn)
![Sentence-Transformers](https://img.shields.io/badge/Sentence%20Transformers-3.0%2B-green?style=flat-square)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?style=flat-square&logo=jupyter)

**Apziva AI Residency Project** | *End-to-End NLP Pipeline for Semantic Talent Matching*

## Table of Contents
- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Step-by-Step Project Flow](#step-by-step-project-flow)
- [Key Technologies](#key-technologies)
- [Results & Visualizations](#results--visualizations)
- [How to Reproduce](#how-to-reproduce)
- [Conclusion for Recruiters & Hiring Managers](#conclusion-for-recruiters--hiring-managers)

## Overview
This project, completed during the Apziva AI Residency, builds an intelligent talent-sourcing system for a talent management and recruitment company. Instead of relying on keyword matching or manual screening, the solution uses **semantic embeddings** and **hybrid feature scoring** to rank candidates by their fit to a specific Human Resources job posting.

By transforming raw candidate profiles into ranked recommendations, the system dramatically reduces time-to-hire while surfacing high-potential talent that traditional Boolean searches would miss.

**Key Outcome**: A production-ready pipeline that delivers a clean, interpretable `fit` score (0–1) for every candidate, combining textual semantics with structured attributes (location & network strength).

## Problem Statement
Talent sourcing teams review hundreds of profiles daily. Manual review is slow, subjective, and inefficient. The business need is clear:

> *“Given a pool of candidate profiles and a target HR job posting, rank candidates by relevance so recruiters can focus on the top 10–20 matches.”*

The dataset contains noisy, real-world LinkedIn-style profiles. The challenge: clean the data, extract deep semantic meaning from job titles, incorporate location and network signals, and produce a robust ranking.

## Dataset
- **File**: `potential-talents.xlsx`
- **Size**: 104 records (duplicates removed during preprocessing)
- **Features**:
  | Column      | Type       | Description                          | Notes |
  |-------------|------------|--------------------------------------|-------|
  | `id`        | Integer    | Unique identifier                    | Dropped (irrelevant) |
  | `job_title` | Text       | Candidate’s current / most recent title | Heavily cleaned |
  | `location`  | Text       | Geographic location                  | One-hot encoded |
  | `connection`| Text       | LinkedIn-style connections (e.g., “500+”) | Parsed & scaled |
  | `fit`       | Float      | Target fit score (initially NaN)     | Computed by model |

## Step-by-Step Project Flow
The notebook `potential-talents.ipynb` follows a clean, reproducible pipeline:

1. **Data Ingestion & EDA**  
   Load Excel file → inspect shape, duplicates, missing values → visualize initial job-title distribution.

2. **Data Cleaning & Preprocessing**  
   - Drop irrelevant `id` column.  
   - Remove exact duplicates.  
   - **Custom noise removal** (`remove_noise()` function): regex-based stripping of numbers, special characters (`| / \ - _ ,`), parentheses content, common company names, and location artifacts.  
   - Parse `connection` into numeric/categorical format.  
   - Prepare `location` for encoding.

3. **Feature Engineering**  
   - **Semantic Embeddings**: Use `sentence-transformers` (pre-trained model) to convert cleaned job titles into 384-dimensional dense vectors. This captures contextual meaning far beyond TF-IDF or keywords.  
   - **Structured Features**:  
     - One-hot encode `location` with `OneHotEncoder`.  
     - Scale connection counts with `MinMaxScaler`.  
   - Generate target embedding from the HR job description.

4. **Similarity & Ranking**  
   - Compute **cosine similarity** between each candidate’s job-title embedding and the target job embedding.  
   - Combine textual similarity with normalized location + connection features into a weighted composite `fit` score.  
   - Sort candidates descending by `fit` score → output final ranked list.

5. **Evaluation & Insights**  
   - Review top-ranked profiles with their original titles and scores.  
   - Compare pre- vs. post-cleaning embedding quality.  
   - (Optional) WordCloud visualization of frequent terms in job titles.

6. **Output**  
   Clean DataFrame with added `fit` column + ranked list ready for recruiter review.

## Key Technologies
- **Core Stack**: Python 3, Jupyter Notebook, Pandas, NumPy  
- **NLP / Embeddings**: `sentence-transformers` (semantic vectors)  
- **ML Preprocessing**: scikit-learn (`OneHotEncoder`, `MinMaxScaler`, cosine similarity)  
- **Visualization**: Matplotlib, WordCloud  
- **Data I/O**: openpyxl (Excel handling)

## Results & Visualizations
- **Top candidates** achieve `fit` scores > 0.90 after cleaning and embedding.  
- Semantic approach clearly outperforms keyword search (e.g., “HR” or “recruiter” alone).  
- Noise removal improved embedding coherence and ranking relevance.  
- Hybrid scoring balances textual fit with practical factors (location proximity & network strength).

(Example top-ranked profiles from the pipeline are displayed in the notebook with original job titles and final `fit` scores.)

## How to Reproduce
```bash
# 1. Clone the repo
git clone https://github.com/Gabriel2002Can/potential-talents.git
cd potential-talents

# 2. Install dependencies
pip install pandas numpy scikit-learn sentence-transformers matplotlib wordcloud openpyxl jupyter

# 3. Launch notebook
jupyter notebook potential-talents.ipynb
```
Run cells sequentially. All outputs (ranked DataFrame, visualizations) are generated on-the-fly.

**Note:** The checkpoints/ folder caches Sentence Transformer models for faster re-runs.

## Conclusion for Recruiters & Hiring Managers
This project demonstrates my ability to take a messy, real-world dataset and deliver an end-to-end, production-oriented AI solution that directly solves a high-impact business problem in talent acquisition.

**What I delivered:**
- Robust data-cleaning pipeline that handles noisy LinkedIn-style text.
- Modern NLP techniques (sentence embeddings + cosine similarity) for semantic understanding.
- Hybrid modeling that integrates unstructured text with structured features.
- Clean, reproducible code with clear documentation and visualizations.
- Actionable ranking system that recruiters can immediately use.

**Skills highlighted:**
- Advanced NLP & semantic search
- Feature engineering & hybrid ML pipelines
- Data cleaning at scale
- End-to-end project ownership from raw data to business-ready output
- Clear technical communication (notebook + this README)

I am proud of how this system moves beyond simple keyword matching to truly understand candidate potential. I am excited to bring the same rigor, creativity, and business focus to your team—whether in HR tech, AI-driven recruitment, or any data-intensive domain.

**Ready to discuss how I can contribute to your next AI project? Feel free to connect or open an issue!**

---

**License:** MIT

**Author:** Luis Gabriel Stedile Portella

**Project completed:** Potential Talents

**Last updated:** March 2026
