===============================================================================
OMX STOCKHOLM LARGE CAP vs SMALL CAP
Portfolio Analysis Notebook
===============================================================================

Overview
--------
This project analyzes month-end returns for OMX Stockholm Large Cap and OMX
Stockholm Small Cap indices and evaluates mixed portfolios across a grid of
Small Cap weights. It computes summary statistics, Sharpe ratios, rolling-window
optimal weights, and maximum drawdowns. The notebook also produces plots for
cumulative log returns and rolling optimal allocations.

What you get
------------
- Cleaned and aligned month-end series for Large Cap, Small Cap, and risk-free
- Monthly log returns and excess returns
- Summary stats: mean, volatility, correlation
- Portfolio grid (0% to 100% Small Cap in 10% steps)
- Rolling-window best Sharpe weight by horizon (1Y, 3Y, 5Y)
- Max Drawdown (MDD) per portfolio with peak and trough timing
- Plots:
  - Cumulative log returns for selected portfolios
  - Optimal Small Cap weight over time per horizon

-------------------------------------------------------------------------------
Project structure
-------------------------------------------------------------------------------

Recommended layout:

  .
  ├── data/
  │   ├── OMX Stockholm Large Cap PI Historical Data.csv
  │   ├── OMX Stockholm Small Cap Historical Data.csv
  │   └── rf.csv
  ├── omx_portfolio_notebook.ipynb
  ├── requirements.txt
  └── README.txt

Notes:
- .venv/ is created locally and should not be committed.
- If your filenames differ, update the paths in the notebook.

-------------------------------------------------------------------------------
Data inputs
-------------------------------------------------------------------------------

Put these files in the data/ folder:

1) OMX Stockholm Large Cap PI Historical Data.csv
   Expected columns:
   - Date
   - Price

2) OMX Stockholm Small Cap Historical Data.csv
   Expected columns:
   - Date
   - Price

3) rf.csv  (risk-free rate)
   Expected columns:
   - Date
   - Value

rf.csv reading assumptions:
- File uses semicolon separator (;)
- Dates are parsed with dayfirst=True
- Value is interpreted as an annualized percentage rate and converted to an
  exact monthly return in the notebook

-------------------------------------------------------------------------------
Quick start (Mac or Linux)
-------------------------------------------------------------------------------

1) Create a virtual environment
   python3 -m venv .venv

2) Activate it
   source .venv/bin/activate

3) Install dependencies
   python -m pip install --upgrade pip
   pip install -r requirements.txt

4) Start Jupyter
   jupyter lab

5) Open and run
   omx_portfolio_notebook.ipynb

Tip:
If you use VS Code, select the .venv interpreter and run the notebook there.

-------------------------------------------------------------------------------
Notebook workflow
-------------------------------------------------------------------------------

High-level steps inside the notebook:

1) Load CSV files
2) Parse dates and coerce numeric columns
3) Sort and set Date index
4) Resample to month-end (ME)
5) Merge Large Cap, Small Cap, and risk-free series
6) Compute:
   - Risk-free monthly return
   - Monthly log returns for Large and Small Cap
   - Excess returns
7) Evaluate portfolio grid and summarize:
   - Annualized mean return
   - Annualized volatility
   - Annualized Sharpe ratio
8) Rolling-window analysis:
   - For each window, compute annualized Sharpe for all portfolios
   - Select the weight that maximizes Sharpe
   - Summarize how often each weight wins and its mean Sharpe
9) Max Drawdown:
   - Compute drawdown from compounded wealth (exp of cumulative log returns)
   - Report MDD and dates of peak and trough

-------------------------------------------------------------------------------
Configuration
-------------------------------------------------------------------------------

Edit these in the notebook:

- START_DATE, END_DATE
  Controls the sample period used for the analysis.

- weights
  The portfolio grid for Small Cap weight.
  Default is 0.0 to 1.0 in steps of 0.1.

- HORIZONS_MONTHS
  Rolling-window horizons in months.
  Default: 1Y=12, 3Y=36, 5Y=60.

-------------------------------------------------------------------------------
Common issues and fixes
-------------------------------------------------------------------------------

1) Resample frequency error
   Example:
     'M' is no longer supported for offsets. Please use 'ME' instead.

   Fix:
     df.resample('ME').last()

2) File not found
   Fix checklist:
   - Confirm the notebook is run from the project root
   - Confirm data/ exists
   - Confirm filenames match exactly (spaces and capitalization)

3) Numeric conversion problems
   If Price or Value columns contain unexpected symbols, check the cleaning
   section and adjust the replace logic for your locale formatting.

-------------------------------------------------------------------------------
Reproducibility
-------------------------------------------------------------------------------

Install dependencies with:

  pip install -r requirements.txt

To pin exact versions later:

  pip freeze > requirements.txt

-------------------------------------------------------------------------------
Outputs
-------------------------------------------------------------------------------

Typical notebook outputs include:
- Portfolio summary table (annualized mean, volatility, Sharpe)
- Rolling-window frequency tables per horizon
- Max drawdown table with peak date, trough date, and drawdown length
- Plots for cumulative log returns and rolling optimal weights

-------------------------------------------------------------------------------
License
-------------------------------------------------------------------------------

Add a license here if you plan to distribute this project publicly.
