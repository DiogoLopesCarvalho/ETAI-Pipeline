# Dataset -- COMPAS Recidivism (ProPublica)
20260626       Diogo Lopes de Carvalho

Week 4:
Results After Preprocessing:

Logistic Regression:
Train accuracy: 0.676
Test accuracy:  0.658
Gap (train - test): +0.018


Decision Tree:
Train accuracy: 0.686
Test accuracy:  0.601
Gap (train - test): +0.086

In the logistic regression, the performance gap descreased fron 0.023 to 0.018, concluding that the results remained identical, with a slight increase in test accuracy.
Regarding the decision tree, the training accuracy decreased drastically (from 0.799 to 0.686), indicating that the model was previously memorizing the training set. With the Week 4 preprocessing, the tree fits the training set much less, and while the test accuracy adjusted slighly downward (from 0.604 to 0.601), the overfitting gap was massively reduced (from 0.196 to 0.086).

Dummy:
Train accuracy: 0.549
Test accuracy:  0.550
Gap (train - test): -0.001

The 0.549 training accuracy tells us that the majority class makes up about 55% of the dataset. A dummy model doesn't actually memorize patterns, so it is imposible for it to overfit. This is confirmed by the near zero (or zero) difference between train and test accuracy. Besides this, the near zero gap also proves that the train/test split is perfectly stratified.

Random Forest:
Train accuracy: 0.728
Test accuracy:  0.652
Gap (train - test): +0.075

The random forest achieved a training accuracy of 0.728 and a test accuracy of 0.652, resulting in a controlled overfitting gap of 0.075. It outperforms the single decision tree and the dummy, though it remains slightly behind the logistic regression in overall stability and test performance. 

The Logistic Regression is still the best model.

Week 3:
Results After Cleaning:

Logistic Resgression:
Train accuracy: 0.678
Test accuracy:  0.655
Gap (train - test): +0.023

Decision Tree:
Train accuracy: 0.799
Test accuracy:  0.604
Gap (train - test): +0.196

In the logistic regression, while the training accuracy remained almost identical, the test accuracy had a slight drop, introducing a minor gap. This suggests that the raw data might have contained "noise" that artificially inflated the test performance.
About the decision tree, the training accuracy decreased, indicating that the decision tree was memorizing invalid values. With the data cleaning, the tree fits the training set slightly less, but the test accuracy also adjusted downward, keeping the overfitting gap relatively stable.
Despite this changes, logistic regression remains the superior model.


Week 2:

Logistic Regression:
Train accuracy: 0.679
Test accuracy:  0.680
Gap (train - test): -0.001

Decision Tree:
Train accuracy: 0.829
Test accuracy:  0.628
Gap (train - test): +0.201

The best model is the logistic regression since the test accuracy is better than in the decision tree. The decision tree overfitted (train accuracy much better than the test accuracy).

## The problem

In 2016, ProPublica investigated COMPAS, a risk-assessment algorithm
actually used by courts in Broward County, Florida, to help inform
bail and sentencing decisions. COMPAS scores a defendant's likelihood
of reoffending on a 1-10 scale; judges could see that score when
deciding, among other things, whether someone should be released
before trial. ProPublica obtained COMPAS's scores for thousands of
defendants and matched them against what actually happened over the
following two years, then published the data.

This dataset is that data: each row is one defendant, with their
demographics and criminal history at the time of screening, COMPAS's
own risk score for them, and whether they were actually rearrested
within two years.

**Your task:** predict `two_year_recid` -- will this person be
rearrested within two years? -- from the case facts. Once you have a
model, the more interesting question is the one ProPublica actually
asked: is it equally accurate for everyone, or does it get things
wrong more often, in a particular direction, for some groups than
others? `race` is deliberately excluded from the model's own inputs
(see `config.yaml` and `src/preprocessing.py`) so it can be used
afterward purely to check this, in `src/evaluate.py`.

Before any of that: look at the data first. It comes from a real
system with real data-entry and record-keeping quirks -- don't assume
every column is clean or consistent just because it loads without
error.

## Data dictionary

| column | type | description | notable values |
|--------|------|--------------|------------------|
| `id` | identifier | internal record id | not a model feature |
| `sex` | categorical | defendant's sex | `Male`, `Female` |
| `age` | numeric | defendant's age (years) at screening | |
| `age_cat` | categorical | age bucket | `Less than 25`, `25 - 45`, `Greater than 45` |
| `race` | categorical | defendant's race, as recorded | `African-American`, `Caucasian`, `Hispanic`, `Asian`, `Native American`, `Other`; excluded from model features, used only to audit fairness |
| `juv_fel_count` | numeric | number of prior juvenile felony offenses | |
| `juv_misd_count` | numeric | number of prior juvenile misdemeanor offenses | |
| `juv_other_count` | numeric | number of other prior juvenile offenses | |
| `juvenile_total` | numeric | total juvenile offenses | |
| `priors_count` | numeric | number of prior adult offenses | |
| `prior_offenses` | numeric | number of prior offenses | |
| `age_in_months` | numeric | age expressed in months | |
| `c_charge_degree` | categorical | degree of the current charge | `F` (felony), `M` (misdemeanor) |
| `decile_score` | numeric | COMPAS's own risk score | 1 (lowest risk) to 10 (highest risk); excluded from model features, used only for comparison |
| `score_text` | categorical | COMPAS's own risk category | `Low`, `Medium`, `High`; excluded from model features, used only for comparison |
| `two_year_recid` | binary | **target** -- was this person rearrested within two years? | `0` = no, `1` = yes |

Source: derived from [propublica/compas-analysis](https://github.com/propublica/compas-analysis) (the data behind the "Machine Bias" investigation). Personally-identifying columns (name, date of birth, case numbers, charge descriptions) were removed.
