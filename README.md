# Bitcoin Time-Series Forecasting with RNN, LSTM and GRU

An educational time-series forecasting project that explores recurrent neural networks for Bitcoin price data, including SimpleRNN, LSTM, GRU, multivariate features, baseline comparisons, and walk-forward validation.

> **Status:** Research and learning project. Model performance and trading results must be verified by running the notebook; this repository does not claim a profitable trading strategy.

## Project goals

- Prepare and inspect historical Bitcoin time-series data.
- Compare SimpleRNN, LSTM, and GRU forecasting approaches.
- Explore return-based targets and multivariate technical features.
- Evaluate models against simple baselines using time-ordered data.
- Investigate walk-forward validation and backtesting assumptions.
- Explain the reasoning behind each notebook stage and the transition to the next experiment.

## Notebook roadmap

The notebook is organized as a progressive investigation: each stage builds on observations from the previous stage and motivates the next experiment. See the section headings and transition notes in the notebook for the detailed narrative.

The later evaluation stages focus on baseline comparisons, time alignment, walk-forward validation, and trading-cost-aware metrics. Treat these results as experimental until the notebook has been run end-to-end against the intended dataset.

## Getting started

### Option 1: Google Colab

1. Open the notebook file in this repository.
2. Select **Open in Colab** or download the notebook and upload it to [Google Colab](https://colab.research.google.com/).
3. Run the cells in order, starting from the top.
4. Follow any data-download instructions in the notebook. If the dataset requires Kaggle authentication, configure your Kaggle credentials in Colab Secrets or use the notebook's documented alternative. Do not commit API tokens, credential files, or downloaded private data to GitHub.

### Option 2: Local environment

Use Python with Jupyter Notebook or JupyterLab. Install dependencies listed in `requirements.txt` (if provided), then launch Jupyter and open the notebook.

Because deep-learning and data dependencies can be version-sensitive, use a clean virtual environment and resolve any package-version issues before running the full experiment.

## Data and reproducibility

- The notebook expects historical Bitcoin data; follow its data-loading instructions.
- Keep observations in chronological order. Do not randomly shuffle time-series samples when creating train, validation, and test splits.
- Fit preprocessing/scaling steps using training data only.
- Compare models on the same test timestamps and target definition.
- Record the dataset version, random seeds, package versions, and experiment configuration when reporting results.
- Do not interpret a backtest as evidence of future profitability. Results can be sensitive to fees, slippage, execution timing, market regime, and the assumptions used.

## Evaluation

Forecasting metrics such as MAE and RMSE measure prediction error, but they do not by themselves establish trading value. Any strategy evaluation should also state its execution assumptions and report relevant portfolio metrics, including transaction costs and drawdown, alongside appropriate benchmarks.

## Repository contents

- `bitcoin-timeseries-rnn-lstm-gru.ipynb` — main notebook with explanations, experiments, and evaluation stages.
- `requirements.txt` — Python dependency list, when included.
- `.gitignore` — excludes local and generated files that should not be versioned.

## Limitations

This is an experimental learning project, not financial advice and not a production trading system. Actual results depend on the data available at run time, package versions, training randomness, and the exact evaluation setup. Run the notebook yourself before relying on any reported metrics.

## Next research directions

Potential follow-up work includes repeated-seed stability checks, additional walk-forward folds, statistical comparisons against stronger baselines, ablation studies for technical features, and more realistic execution and transaction-cost modelling.

## License

No license is specified yet. Unless a license is added, standard copyright applies and reuse permissions are not implicitly granted.
