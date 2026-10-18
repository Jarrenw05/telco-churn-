# Telecom Customer Churn Project

This is the code for my paper, "Predicting Telecom Customer Churn: Balancing Recall and Precision in Class-Imbalanced Subscriber Data" (ACMSE 2027 short paper).

The project tries to predict which telecom customers will cancel their service. It compares a simple rule (guess "churn" for every month-to-month customer) against four models: logistic regression, decision tree, random forest, and gradient boosting. It also tests a lower decision threshold (0.35 instead of 0.50) to catch more of the customers who leave.

## What's in this repo

- `telco_churn_final.ipynb` - the whole project in one notebook (cleaning the data, splitting it, training the models, picking the threshold, results table, figures)
- `requirements.txt` - the packages and versions needed
- `README.md` - this file

When the notebook runs it also saves two pictures: `figure1_results.png` and `figure2_roc_importance.png`.

## Getting the data

The data file is not in this repo. It is the IBM Telco Customer Churn sample dataset. Download `Telco_customer_churn.xlsx` from the IBM post below and put it in the same folder as the notebook. Keep the file name exactly the same.

https://community.ibm.com/community/user/blogs/steven-macko/2019/07/11/telco-customer-churn-1113

If the link stops working, search for "IBM Telco customer churn sample dataset." IBM says the data is for a made-up telecom company. It has 7,043 customers.

## How to run it

You need Python 3.12.

1. Open a terminal in the project folder and make a virtual environment:
   ```
   python3 -m venv .venv
   source .venv/bin/activate
   ```
   (On Windows use `.venv\Scripts\activate` for the second line.)
2. Install the packages:
   ```
   pip install -r requirements.txt
   ```
3. Make sure `Telco_customer_churn.xlsx` is in the same folder as the notebook.
4. Run the notebook:
   ```
   jupyter nbconvert --to notebook --execute telco_churn_final.ipynb --output executed_notebook.ipynb
   ```
   You can also open the notebook in Jupyter and click Run All.

It takes about 10 seconds. No GPU is needed.

## Results to expect

The test set has 1,409 customers (374 who left and 1,035 who stayed).

| Model | Threshold | Accuracy | Precision | Recall | F1 | Missed churners | False alarms |
|---|---|---|---|---|---|---|---|
| Rule baseline | rule | 0.6529 | 0.4256 | 0.8797 | 0.5737 | 45 | 444 |
| Decision tree | 0.50 | 0.7878 | 0.5945 | 0.6310 | 0.6122 | 138 | 161 |
| Logistic regression | 0.50 | 0.8020 | 0.6426 | 0.5722 | 0.6054 | 160 | 119 |
| Logistic regression | 0.35 | 0.7757 | 0.5607 | 0.7166 | 0.6291 | 106 | 210 |
| Gradient boosting | 0.50 | 0.8034 | 0.6601 | 0.5348 | 0.5908 | 174 | 103 |
| Gradient boosting | 0.35 | 0.7771 | 0.5636 | 0.7112 | 0.6288 | 108 | 206 |

The random forest numbers can be a little different on different computers (the third decimal place). Use whatever the notebook prints on your computer for those rows.

I picked the 0.35 threshold using cross-validation on the training data only. The scores for 0.30 and 0.35 were basically tied (0.6495), so I went with 0.35.

## Settings I used

- Data split: 80% train and 20% test, same random seed (42) every time
- Scaling: Tenure Months, Monthly Charges, and Total Charges are scaled using the training data only
- Logistic regression: C = 1.0, up to 1,000 iterations
- Decision tree: max depth 5
- Random forest: 100 trees, max depth 10
- Gradient boosting: 100 trees, learning rate 0.1, max depth 3
- All models were trained from scratch (nothing pretrained)

## Computer and versions

- Made on an Apple Silicon Mac
- Also tested on Linux (one CPU core, Python 3.12.3)
- Main packages: scikit-learn 1.8.0, pandas 3.0.2, numpy 2.4.4 (full list in `requirements.txt`)

## Credits and AI help

- Libraries: scikit-learn, pandas, NumPy, and Matplotlib
- Data: IBM Telco Customer Churn sample dataset (Macko, 2019)
- AI help: I used Claude (an AI assistant from Anthropic) to help write parts of the notebook code (the cross-validation step, the decision tree and gradient boosting comparison, Figure 2, the cost comparison, and the example customers), to write this README and the requirements file, and to check my paper for errors. My instructor said this was okay. The project idea, the design, the original notebook, and the paper are mine.
