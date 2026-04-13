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

# 📂 Data Source
The project uses ranked League of Legends match data and a manually curated champion-to-role mapping file.
Raw data is stored under `Data/`, while the processed analysis-ready dataset is generated through the pipeline described below.

## Data Construction Pipeline
The final analysis-ready dataset (`team_role_dataset.csv`) was not directly available in raw form.
It was **constructed through multiple preprocessing and feature engineering steps**:


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
> ![Support vs No Support](Figures/Secondary%20Role%20Checks/support_vs_no_support.png)

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


---

#  Author
**Ayşe Çamlı**  
Sabancı University — Computer Science & Engineering  
Student ID: **00034456**
