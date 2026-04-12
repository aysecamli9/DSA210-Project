# DSA210 Project
The repository created by Ayşe Çamlı for term project 

# Project Topic
(To be added)

# Data Source
(To be added)

# Author
Ayşe Çamlı
00034456

 
***League of Legends Team Composition Diversity and Match Outcomes***

This project investigates how **team role diversity affects win probability** in League of Legends matches.

Using champion role data and match-level outcomes, I engineered team composition features such as:
- number of Marksmen
- number of Tanks
- number of Mages
- number of Assassins
- number of Supports
- number of Fighters
- role diversity score

The main objective is to understand whether **balanced and diverse team compositions lead to higher win rates**.

Data Source
The project uses:
- `games.csv` → raw League of Legends ranked match data
- `champion_names&roles.json` → champion role mappings
- `team_role_dataset.csv` → processed team-level role composition dataset


Main Findings
  Strong Findings
- Higher **role diversity strongly correlates with increased win rates**
- Teams with **4–5 unique roles perform best**
- Highly successful high-diversity teams usually include:
  - Marksman
  - Mage
  - Support
  - multiple Fighters
- Low-diversity teams often over-stack Fighters or Mages


Secondary Role Checks
Additional tests were conducted for:
- double Marksman
- support vs no support
- assassin count
- Marksman with/without support

These produced mostly **neutral results (~50% win rate)**, which suggests that **overall composition diversity matters more than individual role counts**.


Repository Structure
- `Figures/Main Findings/` → strongest visual insights
- `Figures/Secondary Role Checks/` → control and null-result analyses
- `team_role_dataset.csv` → processed analysis-ready dataset
- `DSA210 Project Proposal.pdf` → initial project proposal

- A chi-square hypothesis test confirmed a statistically significant relationship between role diversity and match outcomes (p < 0.05).
