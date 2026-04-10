# ⚾ The Survival of the Smartest
### Optimizing MLB Roster Construction through Type-Based Survival Analysis and Surplus WAR Backtesting

## Project Overview
This project investigates the **MLB Aging Curve** to identify veteran players (Age 35+) who successfully defy natural physical decline. By analyzing residuals from 2nd-degree polynomial regression models and conducting historical bWAR backtesting, this study provides a strategic framework for evaluating veteran assets.

## Install

This project requires **Python** and the following libraries installed:
- [pybaseball](https://github.com/jldbc/pybaseball)
- numpy
- pandas
- matplotlib
- seaborn
- scikit-learn
- tqdm

```Python
!pip install pandas numpy matplotlib seaborn tqdm scikit-learn pybaseball
```

Executing the code above will install the relevant packages.

You will also need to have software installed to execute a Jupyter Notebook. [Link for Jupyterlab Instalation](https://jupyter.org/install)

## Files

`notebooks` folder will contain the three following files:

`01_data_collection.ipynb` will collect relevant data associated with the project.

`02_regression.ipynb` generate regressions to investigate outperforming veterans.

`03_war_projection.ipynb` calculates projected 2025 WAR (Wins Above Replacement) and is compared with actual 2025 WAR.

`data`, `images` folder will save csv files and images when the three notebooks are executed.

## Data & Methodology

1. Data Collection & Baseline Modeling

We established a longitudinal performance baseline by integrating three seasons of Statcast data with daily bWAR records.  

Data Source: Statcast pitch/hit metrics (2022–2024) and Baseball-Reference bWAR. 
Biological Age Calculation: Established precise age brackets to model the natural decay of physical tools (Velocity, Exit Velocity, etc.).
Quadratic Regression: We modeled the league-wide aging curve using the equation:
$$y = ax^2 + bx + c$$
This established the Expected Performance for each age group.


2. Identifying "Age-Defying Outliers" (Residual Analysis)

The core of this project is the Residual Analysis. We calculated the difference between a player's actual performance and their expected performance based on the regression model.

Residual ($\epsilon$): $Actual - Expected$  

The Definition: Players with significant positive residuals ($Actual > Expected$) were classified as "Aging Curve Defiers.  
"Archetype Segmentation: We categorized these defiers into two primary groups:
- Physical Outliers: High-residue in Velocity or Exit Velocity (e.g., Giancarlo Stanton).
- Cognitive Outliers: High-residue in Chase% or Plate Discipline (e.g., Discipline-first veterans).

3. Hard-Filter Survival Metric & Backtesting

To validate our findings, we conducted historical backtesting against the 2025 season results.

- Survival Rate Calculation: We verified 2025 actual appearances ($G > 5$) for all identified veterans.

This "Hard-Filter" ensures we distinguish between players who remained rosterable versus those who faced a "Performance Cliff" and were released/retired.

- Surplus WAR Analysis: We calculated the Surplus Value provided by these veterans relative to their projected "aging" versions.
- Type-Based Survival Rate:
$$\text{Survival Rate} = \frac{\text{Active Veterans in 2025 (within archetype)}}{\text{Total Identified Veterans (within archetype)}}$$

4. Strategic Ranking System (Smart Marcel)

- We implemented a Trend-Aware Projection model (Smart Marcel) to penalize "Aging Cliffs" more heavily than standard projection systems. 

- Dynamic Weighting: Applied an 8:2 or 5:4:3 weighting system that detects consecutive drops in physical residuals.

- Roster Optimization: Combined Survival Probability (for stability) and Surplus WAR (for upside) to rank veteran assets for front-office acquisition.


## Key Findings

1. Traits that rely less on physical ability were advantagesous on survival rate

Unlike Fastball velocity or Exit velocity, players who had good Chase% (high for pitchers, low for hitters) survived longer, where the survival rate was 100% for them in calculating Projected 2025 WAR. 

2. Traits that demonstrate floor and ceiling are different in veterans

While survival rate was high on players with good Chase%, the outliers like Chapman or Stanton had physicality-based traits. This implies that players who have outstanding traits could not follow the general trend. 

3. Dramatic rebounds are not very frequent

Although there are some cases where veteran players make a comeback season while their records kept declining, and it becomes even harder when their playstyle depends on physical traits. 


## Limitations

1. Survivor's bias

Players who kept playing in the Major Leagues until the age of 35 must have an outstanding trait or should have proven their value for multiple years.

2. Omission of non-quantifiable traits

Value of veterans not only comes from their statistics, but they have other traits like becoming a clubhouse leader, or being a tutor for young prospects. Since this calculation is fully based on statistical analysis, those non-quantifiable traits are not included in the valuation model.

3. Injury & Health

While there are players who made a rebound season fully based on technical improvements or through changing their team, there are cases where players return from long-term injuries. This makes WAR projection to be less accurate, where the model does not consider whether the player was injured or not.

