NYC School SAT Analysis
=======================
Analysis of NYC school SAT scores using Python and pandas. Notebooks perform data cleaning, exploratory analysis, and answer two focused questions about top-performing schools (math threshold and combined SAT totals).

Repository contents
-------------------
- notebook.ipynb — general exploration and utility analyses.
- project1_school_sat_analysis.ipynb — main pipeline: ingestion, cleaning, answers to the posed questions, and exports.
- schools.csv — source data used by the notebooks (NYC SAT results CSV).

Project tagline
--------------
Analysis of NYC school SAT performance with reproducible Jupyter notebooks.

Data
----
- Source file included: schools.csv (place in repo root or data/ if you prefer).
- If you used an external source (NYC Open Data), add the exact URL and year(s) in the notebook or this README.
- Note about aggregation: notebooks group rows by school_name to produce school-level results (averages/means).

Requirements
------------
- Python 3.8+
- Jupyter / JupyterLab or VS Code Notebook support
- Recommended packages (install via pip):
  pip install pandas numpy matplotlib seaborn jupyterlab
- Optionally pin versions in a requirements.txt if reproducibility is required.

How to run
----------
1. Clone the repo:
   git clone https://github.com/JaKoB-Data-Projects/NYC-school-SAT-Analysis.git
2. Install dependencies (see Requirements).
3. Launch Jupyter:
   jupyter lab
4. Open and run project1_school_sat_analysis.ipynb (run cells top-to-bottom).

Questions answered
------------------

1) Which NYC schools have the best math results?
- Definition used: best math results = average math score >= 80% of the maximum possible math score (800).
- Numeric threshold: 0.8 * 800 = 640.
- Output DataFrame name: best_math_schools
- Columns:
  - school_name (string)
  - average_math (float) — mean math score per school (rounded as desired)
- Sort order: descending by average_math
- Example snippet (adjust column names if your CSV uses different headers):
  df = pd.read_csv('schools.csv')
  best_math_schools = (
      df.groupby('school_name')['math_score']
        .mean()
        .reset_index(name='average_math')
        .query('average_math >= 640')
        .sort_values('average_math', ascending=False)
  )
- Export example:
  best_math_schools.to_csv('outputs/best_math_schools.csv', index=False)

2) What are the top 10 performing schools based on the combined SAT scores?
- Definition: total_SAT = math + reading + writing (per-row), then aggregated per school (mean).
- Output DataFrame name: top_10_schools
- Columns:
  - school_name (string)
  - total_SAT (float/int) — mean combined SAT per school
- Sort order: descending by total_SAT, limited to top 10
- Example snippet:
  df = pd.read_csv('schools.csv')
  df['total_SAT'] = df[['math_score','reading_score','writing_score']].sum(axis=1)
  top_10_schools = (
      df.groupby('school_name')['total_SAT']
        .mean()
        .reset_index()
        .sort_values('total_SAT', ascending=False)
        .head(10)
  )
- Export example:
  top_10_schools.to_csv('outputs/top_10_schools.csv', index=False)

Notes on preprocessing
----------------------
- Ensure numeric columns (math_score, reading_score, writing_score) are numeric (coerce errors to NaN and handle missing values).
- If the CSV contains pre-aggregated school-level averages, adapt the grouping logic (skip per-row aggregation).
- Common preprocessing steps included in project1_school_sat_analysis.ipynb:
  - rename columns for consistency
  - convert score columns to numeric
  - drop or impute missing values (notebook documents chosen approach)
  - compute total_SAT as shown above

Outputs
-------
- Example output files (notebooks save these if cells are executed):
  - outputs/best_math_schools.csv
  - outputs/top_10_schools.csv
  - outputs/*.png or .svg for generated plots

Reproducibility & tips
----------------------
- Run notebooks in order; project1_school_sat_analysis.ipynb contains the full, reproducible pipeline.
- If data is large, consider using a virtual environment and pinning dependency versions.
- Add a requirements.txt (I can provide one on request).

Contributing
------------
- Open an issue to propose changes or improvements.
- Submit pull requests with clear descriptions and, if applicable, updated notebooks or scripts.
- Keep notebooks documented with explanatory markdown and small, focused cells.

Contact / Author
----------------
- JaKoB-Data-Projects (GitHub)

