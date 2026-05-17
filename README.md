# DSA210 Project  
## League of Legends Team Composition Diversity and Match Outcomes
This repository contains my **DSA210 term project**, which investigates whether **role-diverse and balanced League of Legends team compositions increase match win probability**.
The project combines:
- feature engineering
- exploratory data analysis (EDA)
- visual storytelling
- statistical hypothesis testing
to understand how **team role structure affects competitive outcomes**.

---
#  Game Context
League of Legends is a 5v5 competitive multiplayer online battle arena (MOBA) game.
Each team consists of five champions, and each champion fulfills one or more strategic roles such as:
- damage dealing
- frontline tanking
- utility support
- crowd control
- map pressure
A successful team composition usually requires a balance between these functions.
Because each match outcome is heavily influenced by how well a team’s champions complement each other, **team composition diversity is a highly meaningful variable to analyze statistically**.


# Project Motivation
In League of Legends, team composition is one of the most important strategic factors.
This project explores whether this intuitive game knowledge can be **validated statistically using real ranked match data**.
The main research question is:
> **Do teams with higher role diversity achieve better win rates?**

---

#  Data Source
The primary match dataset was obtained from Kaggle:

- **League of Legends High Elo Ranked Games Dataset**
- Source: https://www.kaggle.com/datasets/datasnaek/league-of-legends
- Total matches in raw dataset: **51,498 matches**
- Time coverage: **Season 9 ranked solo queue matches (2019) in Europe Server**

In addition to the Kaggle match data, champion role metadata was collected using the **official Riot API**, which was used to create `champion_names&roles.json`.
Raw files are stored under `Data/`, while the processed analysis-ready dataset is generated through the pipeline described below.

## Data Construction Pipeline
The final analysis-ready dataset (`team_role_dataset.csv`) was not directly available in raw form.
It was **constructed through multiple preprocessing and feature engineering steps**:

## Dataset Overview
The raw match dataset initially contained **51,498 matches**, corresponding to:
- **102,996 team-level observations**
- 10 champion IDs per match
- 1 binary match winner label

After preprocessing and feature engineering, the final `team_role_dataset.csv` contains:
- **103,080 rows**
- **8 columns**
  - 6 role count features
  - 1 diversity score
  - 1 win label

##  Class Distribution
Because each original match contributes **two team rows** (one winner and one loser), the final dataset is naturally balanced:
- **Wins:** ~50%
- **Losses:** ~50%
This balanced label distribution makes the dataset highly suitable for both statistical testing and future predictive modeling.

### 1) Raw Match Data
The original `games.csv` file contains match-level League of Legends ranked data, including:
- champion IDs for all 10 players
- team separation
- match winner
- player-level metadata

From this file, only the following fields were used:
- Team 1 champion IDs
- Team 2 champion IDs
- match winner

---

### 2) Champion-to-Role Mapping
Using `champion_names&roles.json`, each champion ID was mapped into one of the following functional roles:
- Marksman
- Tank
- Mage
- Assassin
- Support
- Fighter
This allowed champion-level data to be transformed into **role-level team representations**.

---

### 3) Team-Level Aggregation
For each match:
- champions belonging to Team 1 were grouped
- champions belonging to Team 2 were grouped
- role counts were calculated separately for each team

For every team, the following engineered features were generated:
- number of Marksmen
- number of Tanks
- number of Mages
- number of Assassins
- number of Supports
- number of Fighters
- role diversity = number of unique non-zero roles
- binary win outcome

This transformed the original match dataset into a **team-level composition dataset**, where each row represents a single team in a single match.

---

### 4) Final Dataset Structure
The resulting `team_role_dataset.csv` contains:
- one row = one team
- six role count features
- one diversity score
- one win label

This dataset was then used for:
- EDA
- visualization
- role composition frequency analysis
- chi-square hypothesis testing




# Main Findings

## 1) Average Role Counts in Winning Teams
Winning teams most frequently contain:
- ~1 Marksman
- ~1 Mage
- ~1 Support
- multiple Fighters

Fighters appear as the most overrepresented class in winning teams.

![Average Role Counts](Figures/Main%20Findings/Average%20role%20counts%20in%20winning%20teams.png)

---

## 2) Distribution of Role Diversity
Most teams naturally fall into **4 unique roles**, followed by **5-role fully diverse teams**.

This already suggests that ranked players tend to prefer naturally balanced compositions.

![Role Diversity Distribution](Figures/Main%20Findings/Distribution%20of%20Role%20Diversity.png)

---

## 3) Win Rate Trend by Diversity
The clearest result of the project:

> **Win rate increases consistently as role diversity increases.**

Observed win rate trend:
- diversity = 1 → **28.6%**
- diversity = 2 → **44.3%**
- diversity = 3 → **48.0%**
- diversity = 4 → **50.0%**
- diversity = 5 → **51.4%**

This shows a strong monotonic relationship.

![Win Rate Trend](Figures/Main%20Findings/Trend%20Win%20Rate%20by%20Role%20Diversity.png)

---

## 4) Most Common Winning Team Compositions
The most common successful teams almost always include:
- 1 Marksman
- 1 Mage
- 1 Support
- 1–2 Fighters

This aligns strongly with traditional competitive meta logic.

![Top Winning Compositions](Figures/Main%20Findings/Top%2010%20most%20common%20winning%20compositions.png)

---

## 5) High vs Low Diversity Team Structures
### High-diversity winners
Winning high-diversity teams consistently preserve role balance.

![High Diversity Winners](Figures/Main%20Findings/Top%20Winning%20High-Diversity%20Compositions.png)

### Low-diversity teams
Low-diversity teams often over-stack:
- Fighters
- Mages
- occasionally Marksmen

This may reduce utility and macro flexibility.

![Low Diversity Teams](Figures/Main%20Findings/Top%20Low-Diversity%20Team%20Compositions.png)

---

#  Secondary Role Checks
Additional control analyses were performed:

- double Marksman
- support vs no support
- assassin count
- marksman with/without support

These mostly produced **neutral (~50%) win-rate results**, suggesting that:

> **overall diversity matters more than any single role count.**

 ![Support vs No Support](Figures/Secondary%20Role%20Checks/support_vs_no_support.png)

Secondary visualizations are stored in:
`Figures/Secondary Role Checks/`

---
# Hypothesis Testing

To statistically validate the EDA findings, a **chi-square test of independence** was conducted.

The goal was to test whether:

> **team role diversity and match outcome are independent**

or whether diversity has a meaningful relationship with winning.

---

##  Hypotheses
- **H₀ (Null Hypothesis):**  
  Team role diversity and match outcome are independent.

- **H₁ (Alternative Hypothesis):**  
  Team role diversity and match outcome are statistically dependent.

---

##  Why Chi-Square?
A chi-square independence test is appropriate because both variables are **categorical**:

- role diversity → discrete categories (1, 2, 3, 4, 5)
- match outcome → binary category (win / loss)

The test compares the **observed win-loss distribution across diversity levels** against the expected distribution under independence.

---

##  Test Output
- **Chi-square statistic:** `54.179451390686104`
- **Degrees of freedom:** `4`
- **p-value:** `4.826464458492728e-11`

- **Significance level (α):** `0.05`

---

##  Decision
Since:

> **p-value = 4.826464458492728e-11 < 0.05**
the null hypothesis is rejected.

This means that:
> **team role diversity and match outcomes are statistically dependent.**

---

##  Interpretation
The statistical result strongly supports the EDA trend:

- low-diversity teams underperform
- highly diverse teams (4–5 unique roles) achieve better win rates

This confirms that the observed increase in win rate is **not random noise**, but a statistically meaningful pattern.
In short:
> **balanced role diversity significantly improves competitive success probability.**


##  Full Analysis Notebook
See the full notebook here:
[Open the notebook](Notebook/DSA210%20Project%20Data%20Analysis.ipynb)


#  Final Conclusion
The project demonstrates that **overall team role diversity is a statistically significant predictor of match success**.
While individual role counts alone produced mostly neutral findings, team-level balance and diversity consistently improved outcomes.
This suggests that **composition structure matters more than isolated role presence**, supporting both competitive game theory and statistical evidence.
Future work will extend this analysis into **predictive modeling using machine learning methods**.



# Machine Learning Analysis

In this phase of the project, various machine learning models were applied to statistically validate and quantify the insights derived during the Exploratory Data Analysis (EDA) phase.
The primary objective was to go beyond simple outcome prediction and determine the quantitative contribution of specific team composition features to match success.
To achieve this, two complementary modeling approaches were adopted:

- **Team-wise Analysis:** Each team's composition was evaluated as an independent observation based on its own internal features.  
- **Match-wise / Delta Analysis:** The relative strategic advantage was modeled by calculating the feature differences (Δ) between competing teams within the same match.

---

# Model Performance Metrics

The performance outputs of the models on the test dataset are presented below:

### Logistic Regression
- **Accuracy:** 0.52  
- **Precision:** 0.52  
- **Recall:** 0.55  
- **F1-Score:** 0.53  

### Random Forest
- **Train Accuracy:** 0.5185  
- **Test Accuracy:** 0.5141  

### Match-wise Delta Model
- **Accuracy:** 0.52  
- **Precision (Win Class):** 0.52  
- **Recall (Win Class):** 0.63  
- **F1-Score (Win Class):** 0.57  

### 5-Fold Cross Validation
- **Fold Scores:** [0.518, 0.519, 0.518, 0.518, 0.509]  
- **Mean Accuracy:** 0.5167  

---

# Statistical Interpretation of Results

All applied models consistently achieved an accuracy rate in the 51–52% range, performing above the threshold of random guessing (50%).
Based on these data, the following conclusion can be drawn:
> Team composition contains a tangible but limited predictive signal regarding the match outcome.
These findings prove that while composition plays a role in the outcome of a match, it is not sufficient to explain the total variance on its own.

---

# Feature Analysis and Interpretability

## Logistic Regression (Linear Effects)

Top positive coefficients (features that increase win probability):

- **Diversity (Role Variety):** +0.078  
- **Marksman + Support Synergy (Bot Lane Synergy):** +0.060  
- **Double Fighter:** +0.059  
- **Support Presence:** +0.040  

Negative coefficients (effects of over-stacking or poor balance):

- Support: -0.100  
- Assassin: -0.089  
- Fighter: -0.069  
- Mage: -0.063  

### Academic Inference

> The positive coefficient for the diversity variable mathematically validates that balanced team structures achieve higher success rates.
> Regression and delta analyses confirm that role diversity is a more significant determinant of victory than isolated individual champion picks.

---

## Random Forest (Non-linear Effects)

Top feature importance scores:

- **Mage:** 0.149  
- **Assassin:** 0.133  
- **Fighter:** 0.123  
- **Marksman:** 0.101  
- **Tank:** 0.101  

### Technical Insight

While the Mage and Assassin roles emerged as the most important features in the Random Forest model, they received negative coefficients in Logistic Regression.
This discrepancy indicates a non-linear relationship between these roles and victory.

This suggests that:
- Having too few of these roles is ineffective  
- Having too many of these roles is also ineffective  
- Maintaining these roles in an optimal balance is critical for success  

---

# Match-Level Relative (Delta) Analysis

To better model competitive dynamics, differences between teams (Delta) were used instead of absolute values:

- ΔMarksman  
- ΔMage  
- ΔTank  
- ΔSupport  
- ΔFighter  
- ΔDiversity  

### Critical Coefficients

- **ΔDiversity:** +0.075  
- **ΔSupport:** +0.039  
- **ΔMarksman:** +0.010  
- **ΔAssassin:** -0.050  

### Interpretation

> Possessing higher role diversity than the opponent significantly increases the probability of winning.
> Relative team advantage provides more explanatory power than absolute composition data.

---

# Visual Analysis

### 1. Confusion Matrix

![Confusion Matrix](Figures/Main%20Findings/Confusion%20Matrix.png)

- Observation: The model shows a tendency to label matches as "Win"  
- Finding: True Positives = 7170, False Positives = 6879  

This indicates that the model captures general trends but struggles with precise classification.

---

### 2. Delta Diversity vs. Win Rate

![Delta Diversity](Figures/Main%20Findings/Delta%20Diversity.png)

- When ΔDiversity > 0 → win rate > 50%  
- When ΔDiversity < 0 → win rate < 50%  

This confirms that relative diversity advantage improves success probability.

---

# Model Validation and Stability

The mean accuracy of 51.67% obtained through 5-Fold Cross-Validation, along with low variance between folds, proves that the model:

- Maintains a stable structure  
- Does not suffer from overfitting  
- Produces statistically reliable results  

---

# General Evaluation and Conclusion

- Role diversity has a consistent and strong impact on winning  
- Balanced team compositions outperform extreme distributions  
- Delta-based features provide deeper insight than absolute values  

Final Note:

The fact that model performance remains around ~52% reflects the multi-dimensional complexity of League of Legends.

External factors such as:
- player skill  
- gold advantage  
- real-time decision-making  
- map objectives  

play a critical role in determining match outcomes.
---


# How to Reproduce the Analysis
Clone the repository:
git clone https://github.com/aysecamli9/DSA210-Project.git
Install the required libraries:
pip install -r requirements.txt
Open the notebook: Notebook/DSA210 Project Data Analysis.ipynb
Run all notebook cells sequentially.


# Final Report
The complete final report is available in: Final Report.pdf


# AI Assistance Disclosure
AI tools (ChatGPT) were extensively used throughout the project for:
code generation
debugging
notebook structuring
markdown formatting
interpretation support
report organization

The generated outputs mainly consisted of:
Python code snippets
markdown explanations
debugging suggestions
report text drafts
interpretation support

All project decisions, dataset selection, research direction, experimental design, and final evaluations were reviewed and approved by the author.





#  Author
**Ayşe Çamlı**  
Sabancı University — Computer Science & Engineering  
Student ID: **00034456**
