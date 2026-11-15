# AI Programming Foundations Project: Wine Quality Data Workflow

## Project Description

This project is a complete, reproducible data workflow in a single Jupyter Notebook, built with Python, NumPy, Pandas, Matplotlib, and Seaborn. It loads the red and white wine data, cleans it with reusable, documented functions, explores it with an analysis function, and presents four labelled visualizations with a written summary. The workflow is designed as a clean foundation for future machine learning work.

**Dataset:** [Wine Quality — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/186/wine+quality) (6,497 red and white *Vinho Verde* wines, 11 physicochemical features, and an expert quality score).

## Repository Contents

| File / folder | Description |
|---|---|
| `data_workflow.ipynb` | Main notebook: Setup, Ingestion, Cleaning, EDA, Visualizations, Summary |
| `winequality-red.csv`, `winequality-white.csv` | Raw dataset files (unchanged from UCI) |
| `figures/` | The four figures, saved by the notebook |
| `requirements.txt` | Exact package versions used |
| `module_summary.pdf` | Written report with citations |

## How to Run the Project

Requires Python 3.11 or newer (tested with Python 3.13) and Git.

1. **Clone the repository** and open the project folder:
   ```bash
   git clone https://github.com/alexander-kuzmin-us/ai-programming-foundations-projectgit
   cd ai-programming-foundations-project
   ```
2. **Create and activate a virtual environment** (recommended):
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate        # Windows: .venv\Scripts\activate
   ```
3. **Install the dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
4. **Open the notebook:**
   ```bash
   jupyter notebook data_workflow.ipynb
   ```
5. **Run it:** choose **Kernel → Restart & Run All**. The notebook reads the two CSV files from the project folder, so no extra download is needed. It runs from top to bottom without errors and recreates the figures in `figures/`.

## Reproducibility

- **Dependencies:** `requirements.txt` was created from the project's virtual environment with:
  ```bash
  pip freeze > requirements.txt
  ```
  Installing from it with `pip install -r requirements.txt` gives the same package versions that produced the results.
- **Data:** The raw CSV files are committed unchanged. All cleaning happens inside the notebook, so every result can be traced back to the original data.
- **Version control:** Work was done on a `dev` branch with one commit per workflow step (dataset, setup and ingestion, cleaning, EDA, visualizations, summary) and then merged into `main`.

## Bias Awareness

Poor cleaning choices could easily bias this dataset. The clearest example is outlier removal. Red and white wines have very different chemical profiles, so an outlier rule applied to the combined data flags 48% of red wines but only 11% of white wines. Deleting the flagged rows would have quietly removed almost half of the red wines and skewed every result towards white wines. For that reason, outliers are only flagged, not deleted.

Duplicate removal is a second risk. The 1,177 duplicate rows (18%) were removed on the assumption that they are repeated records. If some of them are actually different wines, valid data was lost. Removing them changed the average quality only slightly (from 5.82 to 5.80), but the assumption is stated openly. Finally, 77% of wines score 5 or 6. Any summary of the full dataset mostly describes average wines and says little about the best or worst ones.

## Reflection Questions

**1. How would this workflow change for a machine learning project?**
The data would be split into training and test sets *before* any step that learns from the data, such as computing medians for filling missing values or quartiles for outlier rules, so that no information leaks from the test set. The cleaning functions would be reused as preprocessing steps that are fit on the training data only. The project would also need a clear target (for example, predicting quality or classifying wines as good or not good), a strategy for the imbalanced quality scores, and evaluation metrics chosen to match that goal.

**2. How would you prepare this data for a neural network?**
Neural networks need numeric, similarly scaled inputs, so all 11 features would be standardized, because their ranges differ by several orders of magnitude (density is about 1, total sulfur dioxide is up to 440). Strongly skewed features such as residual sugar and chlorides might be log-transformed first, and `wine_type` would be one-hot encoded. The rare quality scores (3, 8, and 9) would need class weighting, resampling, or grouping into broader classes. The data would then be split into training, validation, and test sets and converted into batched arrays or tensors.

**3. What parts of this workflow could be automated by an AI agent?**
An agent could run the routine steps automatically whenever new data arrives: loading the files, running the data-quality checks (missing values, duplicates, outliers), applying the cleaning pipeline, regenerating the figures, and drafting a short report of what changed. Because the cleaning steps are already separate, documented functions, they could be used as tools by such an agent. Decisions that need judgement, such as whether to delete outliers or how to treat duplicates, should still be reviewed by a person, because, as shown above, these choices can introduce bias.
