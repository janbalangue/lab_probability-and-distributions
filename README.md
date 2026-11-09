# Probability and Distributions Lab: Beverage Filling Process

A Jupyter notebook lab that uses Bayes' theorem and the normal distribution to answer a quality-control question: does a 12-ounce soft-drink filling line reject too many bottles?

## The scenario

You're a junior quality analyst at a beverage company. Customers notice when a bottle is underfilled by more than 0.3 oz, and overfilling by more than 0.2 oz eats into profits, so bottles below 11.7 oz or above 12.2 oz are rejected. The company wants a rejection rate of 5% or less.

The lab has two parts:

1. **Probability word problems.** Three short scenarios solved with conditional probability and Bayes' theorem.
2. **Fill data analysis.** 1,000 fill readings are used to answer three questions:
   - Does the filling process follow a normal distribution?
   - What are the probabilities of underfilling (below 11.7 oz) and overfilling (above 12.2 oz)?
   - Is the total rejection rate below 5%?

## Results

**The process misses the target. It rejects about 5.7% of bottles, and almost all of them are overfilled.**

### Part 1: Probability word problems

| Scenario | Question | Answer |
| --- | --- | --- |
| 1. Two production lines | A bottle is out of spec. What is the chance it came from Line A? | 50.0% |
| 2. Mechanical issue | Line A shows high volume variability today. What is the chance it has a mechanical issue? | 29.6% |
| 3. Two-step inspection | A bottle fails both measurements. What is the chance it is really underfilled? | 96.4% |

### Part 2: Fill data analysis

| Question | Result |
| --- | --- |
| Does the process follow a normal distribution? | Close enough to model as normal. 71.2%, 96.5%, and 99.2% of fills fall within 1, 2, and 3 standard deviations of the mean, against 68%, 95%, and 99.7% for a normal curve. |
| Probability of underfilling (below 11.7 oz) | 0.13% (z ≈ −3.02) |
| Probability of overfilling (above 12.2 oz) | 5.53% (z ≈ 1.60) |
| Fills within limits | 94.34% |
| Total rejection rate | 5.66%, above the 5% maximum |

The probabilities come from a normal curve with the sample's mean (12.027 oz) and standard deviation (0.108 oz). The mean sits 0.17 oz below the upper limit but 0.33 oz above the lower one, which is why overfills account for nearly all rejections.

## Dataset

`bottle_fills.csv` comes with the lab. It holds 1,000 readings taken every 30 seconds on January 15, 2024, from 08:00:00 to 16:19:30, with no missing values.

| Column | Type | Description |
| --- | --- | --- |
| `timestamp` | String (`YYYY-MM-DD HH:MM:SS`) | Time of the reading |
| `fill_amount` | Float | Fill amount in ounces |

Fill amounts range from 11.611 to 12.889 oz, with a median of 12.028 oz.

## Methods

- Law of total probability and Bayes' theorem (Part 1)
- Independent events, for the two inspection measurements in Scenario 3
- Summary statistics with pandas `describe()`
- A histogram with a fitted normal curve overlay (Matplotlib and SciPy)
- An empirical rule (68-95-99.7) check for normality
- z-scores and the normal CDF (`scipy.stats.norm.cdf`) to turn the rejection limits into probabilities

## Repository contents

| File | Description |
| --- | --- |
| `prob_dist_lab.ipynb` | The completed lab notebook, saved with its outputs |
| `bottle_fills.csv` | Fill data for Part 2 |
| `README.md` | This file |

## Running the notebook

You need Python 3 with NumPy, pandas, Matplotlib, SciPy, and Jupyter. The notebook was tested with Python 3.12.4, NumPy 1.26.4, pandas 2.2.2, SciPy 1.14.0, and Matplotlib 3.9.0.

```bash
git clone https://github.com/janbalangue/lab_probability-and-distributions.git
cd lab_probability-and-distributions

python3 -m venv .venv          # Windows: py -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install numpy pandas scipy matplotlib jupyter

jupyter notebook prob_dist_lab.ipynb
```

Start Jupyter from the repository folder, since the notebook loads `bottle_fills.csv` by a relative path. Then run all cells from top to bottom.

Cells marked `# CodeGrade stepN` are the graded steps from the original course assignment, which was auto-graded with CodeGrade.

## Notes and limitations

- The normality check is informal. The empirical rule percentages show more readings near the mean and more extreme readings than a normal curve predicts, and the largest reading (12.889 oz) is about 8 standard deviations above the mean. The lab accepts the data as "normal enough" for the normal model.
- The rejection probabilities come from the normal model, not from counting bottles. A direct count, which isn't in the notebook, finds 53 of the 1,000 readings outside the limits (5 under, 48 over), or 5.3%. That also misses the 5% target.
