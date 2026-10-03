# A/B Testing Revisited: Classical vs Bayesian Analysis

## Project Overview

A/B testing is widely used to evaluate whether a change to a product, website, or marketing strategy leads to a meaningful improvement in user behavior.

This project revisits an A/B testing experiment using **Udacity's real-world experiment dataset** and compares two statistical approaches:

* **Classical (Frequentist) A/B Testing**
* **Bayesian A/B Testing**

The primary objective was not only to determine whether the new landing page improved conversion rates, but also to understand how the two statistical frameworks approach uncertainty, evidence, and decision-making differently.

The project follows the complete statistical workflow, from data preparation and exploratory analysis to hypothesis testing, Bayesian posterior estimation, lift analysis, and business interpretation.

---

## Objectives

The main objectives of the project are:

1. Analyze the performance of a control and treatment landing page.
2. Compare conversion rates between the two groups.
3. Perform a Classical/Frequentist hypothesis test.
4. Estimate the posterior distribution using Bayesian inference.
5. Quantify uncertainty around conversion rates and their difference.
6. Estimate the probability that the new landing page provides a meaningful improvement.
7. Compare the interpretations provided by Frequentist and Bayesian approaches.
8. Translate statistical results into practical business recommendations.

---

## Dataset

The project uses the **Udacity A/B Testing dataset**, which contains information about users exposed to different versions of a landing page.

The experiment consists primarily of two groups:

* **Control Group** — users exposed to the existing landing page.
* **Treatment Group** — users exposed to the new landing page.

The main outcome of interest is whether a user converted.

### Key Variables

| Variable       | Description                         |
| -------------- | ----------------------------------- |
| `user_id`      | Unique identifier for each user     |
| `timestamp`    | Time associated with the experiment |
| `group`        | Control or treatment group          |
| `landing_page` | Landing page shown to the user      |
| `converted`    | Whether the user converted          |

---

## Project Workflow

```text
Udacity A/B Testing Dataset
            ↓
       Data Cleaning
            ↓
    Mismatch Correction
            ↓
   Exploratory Data Analysis
            ↓
     Conversion Analysis
            ↓
   ┌─────────────────────┐
   │                     │
   ↓                     ↓
Frequentist            Bayesian
Analysis               Analysis
   │                     │
   ↓                     ↓
Hypothesis Test       Posterior
   │                  Distribution
   ↓                     │
p-value & CI             ↓
   │                  Credible
   │                  Interval
   └──────────┬──────────┘
              ↓
         Lift Analysis
              ↓
      Business Decision
```

---

# 1. Data Cleaning

Before performing statistical analysis, the dataset was examined for inconsistencies and invalid observations.

The cleaning process included:

* Checking missing values.
* Removing duplicate users.
* Examining group assignments.
* Checking consistency between the assigned experimental group and landing page.
* Correcting or excluding mismatched observations where necessary.

This step was important because incorrect treatment assignments can directly affect the validity of an A/B test.

---

# 2. Exploratory Data Analysis

The exploratory analysis focused on understanding the structure of the experiment and comparing the two groups.

Key quantities examined included:

* Number of users in each group.
* Number of conversions.
* Conversion rate.
* Distribution of observations across experimental groups.
* Difference in conversion rates.

The conversion rate for a group is calculated as:

$$
\hat{p} = \frac{\text{Number of Conversions}}
{\text{Total Number of Users}}
$$

The difference in observed conversion rates is:

$$
\hat{p}_T-\hat{p}_C
$$

where:

* \(T\) = Treatment group
* \(C\) = Control group

---

# 3. Classical (Frequentist) A/B Testing

The first analysis uses a traditional hypothesis-testing framework.

### Null Hypothesis

$$
H_0:p_T=p_C
$$

There is no difference in conversion rates between the treatment and control groups.

### Alternative Hypothesis

$$
H_1:p_T\neq p_C
$$

There is a difference in conversion rates between the two groups.

Depending on the testing framework, the analysis uses the observed conversion data to calculate a test statistic and corresponding p-value.

### Interpretation

The p-value measures how compatible the observed data are with the null hypothesis.

If:

$$
p\text{-value}<\alpha
$$

the null hypothesis is rejected.

Otherwise, there is insufficient evidence to reject the null hypothesis.

---

# 4. Bayesian A/B Testing

The Bayesian analysis approaches the problem differently.

Instead of testing a null hypothesis using a p-value, Bayesian inference combines:

$$
\text{Prior Information}+\text{Observed Data}
$$

to obtain a **posterior distribution**.

For conversion probabilities, a Beta distribution provides a natural model:

$$
p\sim Beta(\alpha,\beta)
$$

After observing conversions and non-conversions, the posterior distribution becomes:

$$
p|Data\sim Beta(\alpha+x,\beta+n-x)
$$

where:

* \(x\) = number of conversions
* \(n\) = number of users
* \(\alpha,\beta\) = prior parameters

Separate posterior distributions can then be estimated for the control and treatment groups.

---

# 5. Posterior Lift

One of the key advantages of the Bayesian approach is the ability to directly examine the distribution of the difference between treatment and control conversion rates.

The lift is defined as:

$$
Lift=p_T-p_C
$$

or, when expressed as relative improvement:

$$
Relative\ Lift=
\frac{p_T-p_C}{p_C}
$$

The posterior distribution of the lift provides information about the range of plausible improvements or declines associated with the new landing page.

---

# 6. Uncertainty Quantification

A major focus of the Bayesian analysis is uncertainty.

Rather than simply producing a binary decision such as:

```text
Significant / Not Significant
```

the posterior distribution allows us to ask questions such as:

> What is the probability that the new landing page improves conversion?

and:

> What is the probability that the improvement exceeds a practically meaningful threshold?

This makes the Bayesian framework particularly useful for decision-oriented analysis.

---

# 7. Frequentist vs Bayesian Comparison

| Aspect                  | Frequentist                            | Bayesian                                  |
| ----------------------- | -------------------------------------- | ----------------------------------------- |
| Main framework          | Hypothesis testing                     | Posterior inference                       |
| Key output              | p-value                                | Posterior distribution                    |
| Uncertainty             | Confidence interval                    | Credible interval                         |
| Main question           | Is the data inconsistent with \(H_0\)? | What values are plausible given the data? |
| Decision interpretation | Reject / fail to reject \(H_0\)        | Probability of improvement                |
| Prior information       | Not explicitly incorporated            | Can be incorporated                       |
| Business interpretation | Often indirect                         | More directly decision-oriented           |

---

# 8. Results

Both statistical approaches led to the same overall conclusion for this experiment:

> **The new landing page did not demonstrate a meaningful improvement in conversion rate compared with the existing landing page.**

The observed difference between the groups was not sufficiently strong to support adopting the new landing page based on conversion performance alone.

The Bayesian analysis additionally allowed the uncertainty surrounding the treatment effect to be examined directly through the posterior distribution and lift analysis.

---

# 9. Business Recommendation

Based on the statistical evidence, there is insufficient justification for replacing the existing landing page solely on the basis of conversion performance.

A practical recommendation would therefore be:

* Continue using the existing landing page.
* Avoid interpreting the observed difference as evidence of a meaningful improvement.
* Consider additional experiments with alternative designs.
* Investigate other performance metrics beyond conversion rate.
* Use Bayesian decision analysis in future experiments where quantifying the probability and magnitude of improvement is important.

---

# 10. Key Statistical Concepts

This project provides practical application of:

* A/B Testing
* Hypothesis Testing
* Null and Alternative Hypotheses
* p-values
* Confidence Intervals
* Bayesian Inference
* Prior and Posterior Distributions
* Beta-Binomial Modeling
* Credible Intervals
* Conversion Rate Analysis
* Effect Size
* Lift Analysis
* Uncertainty Quantification
* Statistical Decision-Making

---

# 11. Technologies Used

* **R**
* **RStudio / R Markdown**
* **dplyr** — Data manipulation
* **ggplot2** — Data visualization
* **Base R** — Statistical analysis
* **Bayesian simulation and probability analysis**

---

# 12. Project Structure

```text
AB-Testing-Revisited/
│
├── data/
│   └── ab_data.csv
│
├── analysis/
│   └── AB_Testing_Analysis.Rmd
│
├── results/
│   ├── plots/
│   └── statistical_results/
│
├── README.md
└── requirements/
```

---

# 13. What I Learned

The project helped develop a deeper understanding of the distinction between **statistical significance and practical decision-making**.

The most important takeaway was that two statistical frameworks can analyze the same experiment from different perspectives while reaching a consistent conclusion.

The Frequentist approach provides a formal framework for hypothesis testing, while the Bayesian approach provides a more direct way to quantify uncertainty and reason about the probability and magnitude of potential outcomes.

This comparison helped demonstrate why choosing a statistical framework is not only about obtaining a result, but also about understanding **how that result can be interpreted and used for decision-making**.

---

# 14. Future Improvements

Potential extensions of the project include:

* Incorporating prior business knowledge into the Bayesian model.
* Comparing different prior distributions.
* Performing Bayesian sequential A/B testing.
* Introducing practical significance thresholds.
* Extending the analysis to multiple conversion metrics.
* Performing power and sample-size analysis.
* Incorporating segmentation by user characteristics.
* Comparing Bayesian and Frequentist approaches across multiple simulated experiments.

---

## Authors

**Prantik Dutta**
M.Sc. Statistics and Computing
Banaras Hindu University (BHU)

**Collaborator:** Project partner

---

## Dataset

The analysis uses the A/B testing dataset provided by **Udacity** for educational purposes.

---

## Topics

`A/B Testing` `Statistics` `Bayesian Statistics` `Frequentist Statistics` `Hypothesis Testing` `Bayesian Inference` `R` `Data Science` `Analytics` `Experimentation` `Statistical Modeling`
