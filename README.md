# Synthesizing the Voice of the Crowd: Faithful Rating-Specific Opinion Summarization

**Authors:** Daniel Yerichman & Or Gabbay
**Date:** January 2026

## 📌 Overview
This research addresses the challenge of ensuring AI-generated movie review summaries faithfully reflect the specific sentiment intensity of a target rating group. We introduced a dual-phase framework designed to synthesize and validate "Representative Reviews" that maintain the authentic voice of specific rating cohorts.

Our framework compares **Gemini 2.5 Flash** and **Gemini 3.0 Flash** using a custom "Judge" model to measure sentiment fidelity.

## 📂 Repository Structure

### 1. The "Judge" (Evaluation Models)
* **`GenAi_project_regression_model_training.ipynb`** (Main):
    * Trains the final "Judge" model using a **Regression** architecture based on DistilBERT.
    * This model measures sentiment intensity as a continuous value to quantify the distance between the AI output and human ground truth.
* **`GenAi_classification_model_training.ipynb`** (Baseline):
    * Contains the initial experiment treating the problem as a multi-class classification task.
    * Used as a baseline comparison to demonstrate why regression is superior for this task.

### 2. The Generator
* **`GenAi_Gemini_api_integration_automation.ipynb`**:
    * Handles the integration with Google's **Gemini API** (Flash 2.5 & 3.0).
    * Implements the two prompting strategies: **"Basic Strategy"** vs. **"Persona-Based Strategy"**.
    * Automates the generation of synthetic reviews based on cluster sampling from the dataset.

### 3. Analysis & Metrics
* **`Rouge_BertScore.ipynb`**:
    * Evaluates the generated text using standard linguistic metrics: **ROUGE** (text overlap) and **BERTScore** (semantic similarity).
    * Demonstrates the divergence between semantic fidelity and sentiment accuracy.

### 4. Data
* **`imdb_sup.csv`**:
    * A subset of the IMDb Movie Reviews dataset used for training and testing.
    * *Note: If the file is too large for GitHub, please download the full dataset from Kaggle.*

## 🚀 Key Findings
1.  **"U-Shaped" Performance:** GenAI achieves near-perfect accuracy for extreme ratings (1 and 10 stars) but struggles significantly with mid-range nuances (e.g., distinguishing a 7 from a 9).
2.  **Prompt Strategy:** Counter-intuitively, the sophisticated **Persona-based prompting degraded performance**, increasing error rates compared to direct "Basic" constraints.
3.  **Model Comparison:** Gemini 3.0 Flash showed improved nuance for negative ratings (1-4) but Gemini 2.5 remained superior for positive nuance.

## 🛠️ Installation
1.  Clone the repository:
    ```bash
    git clone [https://github.com/YOUR_USERNAME/Movie-Review-Sentiment-GenAI.git](https://github.com/YOUR_USERNAME/Movie-Review-Sentiment-GenAI.git)
    ```
2.  Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3.  **Important:** You must add your own Google API Key in `GenAi_Gemini_api_integration_automation.ipynb` before running the generation script.

---
*Project submitted as part of the degree requirements, January 2026.*
