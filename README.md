# Faithful Rating-Specific Opinion Summarization for Movie Reviews

This repository contains a notebook-based GenAI/NLP project on sentiment-faithful opinion summarization for movie reviews. The project investigates whether a language model can generate a short "representative review" from multiple IMDb reviews while still preserving the sentiment intensity of a specific rating group.

The workflow combines a fine-tuned DistilBERT judge model, a classification baseline, a Gemini-based generation pipeline, and a small manual evaluation notebook for ROUGE and BERTScore checks. The main written deliverable is the final report: [reports/GenAi_Project_Or_and_Daniel.pdf](reports/GenAi_Project_Or_and_Daniel.pdf).

## Objective

The core question is whether generated review summaries can stay faithful to the "voice" of a target rating cohort instead of drifting toward a generic positive or negative summary.

To study that, the repository trains an automated evaluator on IMDb reviews and uses it to score generated reviews across eight rating groups:

`1, 2, 3, 4, 7, 8, 9, 10`

## Repository Structure

```text
.
|-- data/
|   `-- imdb_sup.csv
|-- notebooks/
|   |-- 01_judge_regression_training.ipynb
|   |-- 02_judge_baseline_classification.ipynb
|   |-- 03_gemini_generation_pipeline.ipynb
|   `-- 04_rouge_bertscore_evaluation.ipynb
|-- reports/
|   `-- GenAi_Project_Or_and_Daniel.pdf
|-- requirements.txt
`-- README.md
```

Important generated directories are not committed and are ignored by Git:

- `artifacts/` for trained judge checkpoints and saved models
- `outputs/` for Excel experiment results and summary statistics

Main files:

- [data/imdb_sup.csv](data/imdb_sup.csv): Working dataset used by the notebooks. It contains 50,000 rows with `Review`, `Rating`, and `Sentiment` columns.
- [notebooks/01_judge_regression_training.ipynb](notebooks/01_judge_regression_training.ipynb): Main notebook. Trains the DistilBERT regression-based judge and saves the model to `artifacts/judge_regression_model/`.
- [notebooks/02_judge_baseline_classification.ipynb](notebooks/02_judge_baseline_classification.ipynb): Trains a classification baseline that predicts one of the eight rating groups directly.
- [notebooks/03_gemini_generation_pipeline.ipynb](notebooks/03_gemini_generation_pipeline.ipynb): Generates short representative reviews with Gemini, scores them with the saved judge model, and writes Excel outputs to `outputs/`.
- [notebooks/04_rouge_bertscore_evaluation.ipynb](notebooks/04_rouge_bertscore_evaluation.ipynb): Interactive notebook for manual ROUGE and BERTScore spot checks between generated text and reference text.
- [reports/GenAi_Project_Or_and_Daniel.pdf](reports/GenAi_Project_Or_and_Daniel.pdf): Final report with the project framing, analysis, and conclusions.
- [requirements.txt](requirements.txt): Python dependency list used by the notebook workflow.

## Method / Workflow

1. **Load and filter the dataset**
   The notebooks load `data/imdb_sup.csv` and work with the eight target rating groups used throughout the project.

2. **Train the main judge model**
   [01_judge_regression_training.ipynb](notebooks/01_judge_regression_training.ipynb) fine-tunes `distilbert-base-uncased` as a regression model. Instead of predicting a class directly, it predicts a continuous rating value that is later rounded to the nearest valid target rating.

3. **Train a classification baseline**
   [02_judge_baseline_classification.ipynb](notebooks/02_judge_baseline_classification.ipynb) trains a direct eight-class DistilBERT classifier for comparison against the regression-based judge.

4. **Generate representative reviews**
   [03_gemini_generation_pipeline.ipynb](notebooks/03_gemini_generation_pipeline.ipynb) samples 10 reviews from a single target rating group, prompts Gemini to write one short representative review, and scores the output with the saved judge model. By default, the notebook runs two prompt styles (`Basic` and `Persona`) for 10 iterations per rating group and saves the results to Excel.

5. **Run manual text-similarity checks**
   [04_rouge_bertscore_evaluation.ipynb](notebooks/04_rouge_bertscore_evaluation.ipynb) provides manual ROUGE and BERTScore checks as a complement to the judge model.

## Key Artifacts

If you only want the most important materials, start here:

- [reports/GenAi_Project_Or_and_Daniel.pdf](reports/GenAi_Project_Or_and_Daniel.pdf): Best entry point for the project narrative and findings.
- [notebooks/01_judge_regression_training.ipynb](notebooks/01_judge_regression_training.ipynb): Core technical notebook and the main custom modeling component.
- [notebooks/03_gemini_generation_pipeline.ipynb](notebooks/03_gemini_generation_pipeline.ipynb): The end-to-end generation and scoring workflow.
- [notebooks/02_judge_baseline_classification.ipynb](notebooks/02_judge_baseline_classification.ipynb): Supporting baseline for comparison.
- [notebooks/04_rouge_bertscore_evaluation.ipynb](notebooks/04_rouge_bertscore_evaluation.ipynb): Supporting evaluation utility.

## Setup and Usage

### 1. Create an environment and install dependencies

```bash
python -m venv .venv
```

PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

macOS/Linux:

```bash
source .venv/bin/activate
pip install -r requirements.txt
```

Notes:

- The repository includes `requirements.txt`, but dependency versions are not pinned.
- The project is notebook-first. If you want to execute the notebooks locally, use Jupyter or VS Code's notebook support. Notebook tooling itself is not listed in `requirements.txt`.
- The dataset is already included in the repository, so there is no separate dataset download step documented here.

### 2. Run the notebooks in practical order

1. Run [notebooks/01_judge_regression_training.ipynb](notebooks/01_judge_regression_training.ipynb) to train and save the main judge model.
2. Optionally run [notebooks/02_judge_baseline_classification.ipynb](notebooks/02_judge_baseline_classification.ipynb) for the comparison baseline.
3. Set `GEMINI_API_KEY` before running [notebooks/03_gemini_generation_pipeline.ipynb](notebooks/03_gemini_generation_pipeline.ipynb).
4. Optionally use [notebooks/04_rouge_bertscore_evaluation.ipynb](notebooks/04_rouge_bertscore_evaluation.ipynb) for manual spot checks.

PowerShell example:

```powershell
$env:GEMINI_API_KEY = "your-api-key"
```

Optional environment variables supported by the generation notebook:

- `JUDGE_MODEL_PATH`: Override the default path to the saved judge model
- `GEMINI_MODEL_NAME`: Override the Gemini model name used for generation
- `GEMINI_MODEL_LABEL`: Override the model label written into the Excel outputs

### 3. Generated files

When the notebooks are executed, the main generated artifacts are:

- `artifacts/judge_regression_checkpoints/`
- `artifacts/judge_regression_model/`
- `artifacts/judge_classification_checkpoints/`
- `artifacts/judge_classification_model/`
- `outputs/results_gemini_3_0_flash.xlsx`
- `outputs/summary_statistics.xlsx`

These directories are not committed to the repository.

## Results Summary

Detailed findings are documented in [reports/GenAi_Project_Or_and_Daniel.pdf](reports/GenAi_Project_Or_and_Daniel.pdf).

At a high level, the report states that:

- rating-faithful generation is stronger at the extreme ends of the rating scale than in the middle ranges
- the `Persona` prompting strategy underperformed the simpler `Basic` strategy in the reported analysis
- ROUGE and BERTScore alone were not sufficient to capture sentiment fidelity, which is why the learned judge model is central to the project

The repository does not include committed experiment outputs or notebook cell outputs, so the report is the authoritative place to review the detailed analysis.

## Notes and Limitations

- This is a research repository organized around notebooks and a report, not a packaged application or production service.
- Generated models, checkpoints, and Excel outputs are intentionally excluded from version control.
- The generation stage depends on external Gemini API access, available quota, and the current availability of the configured model name.
- Exact reruns may differ because the dependency list is unpinned and the LLM-based generation step is API-driven.
- The notebooks are written to resolve paths from either the repository root or the `notebooks/` directory; keeping the repository layout unchanged will make reruns easier.

## Recommended Reading Order

1. [reports/GenAi_Project_Or_and_Daniel.pdf](reports/GenAi_Project_Or_and_Daniel.pdf)
2. [notebooks/01_judge_regression_training.ipynb](notebooks/01_judge_regression_training.ipynb)
3. [notebooks/03_gemini_generation_pipeline.ipynb](notebooks/03_gemini_generation_pipeline.ipynb)
4. [notebooks/02_judge_baseline_classification.ipynb](notebooks/02_judge_baseline_classification.ipynb)
5. [notebooks/04_rouge_bertscore_evaluation.ipynb](notebooks/04_rouge_bertscore_evaluation.ipynb)

For a fast interview review, start with the report for the conclusions, then inspect the regression judge notebook and the Gemini pipeline notebook to see how the project was implemented.
