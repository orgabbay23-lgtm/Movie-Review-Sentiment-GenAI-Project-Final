# Synthesizing the Voice of the Crowd: Faithful Rating-Specific Opinion Summarization

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c?logo=pytorch&logoColor=white)
![Transformers](https://img.shields.io/badge/HuggingFace-Transformers-yellow?logo=huggingface&logoColor=white)
![Gemini](https://img.shields.io/badge/Google-Gemini%20API-4285F4?logo=google&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

> **Authors:** Or Gabbay & Daniel Yerichman | **Date:** January 2026

A GenAI research project investigating whether AI-generated movie review summaries can faithfully reflect the sentiment intensity of specific rating groups. We introduce a dual-phase framework that synthesizes "Representative Reviews" using Google Gemini and validates them with a fine-tuned DistilBERT "Judge" model.

---

## Table of Contents

- [Overview](#overview)
- [Key Findings](#key-findings)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Technologies](#technologies)
- [Authors](#authors)
- [License](#license)

---

## Overview

This research addresses a core challenge in opinion summarization: ensuring that AI-generated summaries maintain the authentic voice and sentiment intensity of a target rating cohort (e.g., "What does a 3-star review sound like?").

The framework consists of two phases:

1. **The Judge** -- A DistilBERT model fine-tuned on ~50K IMDb reviews to predict sentiment ratings on a continuous scale. Two architectures were explored: multi-class classification (baseline) and regression (final).

2. **The Generator** -- Google Gemini (2.5 Flash & 3.0 Flash) generates synthetic representative reviews using two prompting strategies: a direct "Basic" strategy and a "Persona-Based" strategy.

Generated reviews are evaluated both by the Judge (sentiment accuracy) and by standard NLP metrics (ROUGE and BERTScore) to measure textual and semantic fidelity.

---

## Key Findings

| Finding | Detail |
|---|---|
| **"U-Shaped" Performance** | GenAI achieves near-perfect accuracy for extreme ratings (1 and 10 stars) but struggles with mid-range nuances (e.g., distinguishing 7 from 9). |
| **Prompt Strategy** | Counter-intuitively, the Persona-based prompting **degraded** performance, increasing error rates compared to the simpler Basic strategy. |
| **Model Comparison** | Gemini 3.0 Flash improved nuance for negative ratings (1--4), while Gemini 2.5 Flash remained superior for positive-range nuance. |
| **Metrics Divergence** | High ROUGE/BERTScore does not guarantee sentiment accuracy -- a review can be semantically similar yet miss the target rating. |

---

## Project Structure

```
Movie-Review-Sentiment-GenAI-Project-Final/
├── notebooks/
│   ├── GenAi_project_regression_model_training.ipynb   # Judge model (regression) -- main
│   ├── GenAi_classification_model_training.ipynb       # Judge model (classification) -- baseline
│   ├── GenAi_Gemini_api_integration_automation.ipynb   # Gemini API generation & evaluation
│   └── Rouge_BertScore.ipynb                           # ROUGE & BERTScore evaluation
├── data/
│   └── imdb_sup.csv                                    # IMDb reviews dataset (~50K reviews)
├── reports/
│   └── GenAi_Project_Or_and_Daniel.pdf                 # Full academic paper
├── requirements.txt
├── LICENSE
└── ReadMe.md
```

### Notebooks

| Notebook | Role | Description |
|---|---|---|
| `GenAi_project_regression_model_training.ipynb` | **Judge (Main)** | Fine-tunes DistilBERT as a regression model to predict sentiment ratings as continuous values. Includes training, evaluation, and confusion matrix visualization. |
| `GenAi_classification_model_training.ipynb` | **Judge (Baseline)** | Trains DistilBERT as a multi-class classifier over 8 rating categories. Serves as a baseline to demonstrate why regression is superior for this task. |
| `GenAi_Gemini_api_integration_automation.ipynb` | **Generator** | Integrates with Google Gemini API to generate synthetic reviews using Basic and Persona-based prompting strategies. Automates the full generation-evaluation pipeline. |
| `Rouge_BertScore.ipynb` | **Metrics** | Computes ROUGE (text overlap) and BERTScore (semantic similarity) to evaluate generated text quality. |

---

## Installation

### Prerequisites

- Python 3.10+
- A Google Cloud API key with access to the Gemini API
- GPU recommended for model training (notebooks were developed on Google Colab)

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/orgabbay23-lgtm/Movie-Review-Sentiment-GenAI-Project-Final.git
   cd Movie-Review-Sentiment-GenAI-Project-Final
   ```

2. **Create a virtual environment (recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate        # Linux/macOS
   venv\Scripts\activate           # Windows
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure API key:**
   Open `notebooks/GenAi_Gemini_api_integration_automation.ipynb` and replace `YOUR_API_KEY_HERE` with your Google Gemini API key.

---

## Usage

The notebooks are designed to run on **Google Colab** with GPU acceleration. They can also be run locally with a CUDA-capable GPU.

### Recommended execution order:

1. **Train the Judge model** -- Run either the regression notebook (recommended) or the classification notebook:
   ```
   notebooks/GenAi_project_regression_model_training.ipynb
   ```

2. **Generate reviews with Gemini** -- Run the API integration notebook (requires a trained Judge model and a Gemini API key):
   ```
   notebooks/GenAi_Gemini_api_integration_automation.ipynb
   ```

3. **Evaluate with ROUGE & BERTScore** -- Run the metrics notebook to compare generated vs. original reviews:
   ```
   notebooks/Rouge_BertScore.ipynb
   ```

---

## Technologies

| Category | Tools |
|---|---|
| **Language** | Python 3.12 |
| **Deep Learning** | PyTorch, Hugging Face Transformers |
| **LLM** | Google Gemini 2.5 Flash, Gemini 3.0 Flash |
| **NLP Evaluation** | ROUGE, BERTScore, Hugging Face Evaluate |
| **Data & Visualization** | Pandas, NumPy, Matplotlib, Seaborn |
| **Platform** | Google Colab (GPU) |

---

## Authors

- **Or Gabbay** -- [GitHub](https://github.com/orgabbay23-lgtm)
- **Daniel Yerichman**

Project submitted as part of the degree requirements, January 2026.

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
