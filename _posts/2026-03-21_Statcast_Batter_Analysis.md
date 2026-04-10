# Statcast Analysis of 2024 MLB Hitters

## Project Overview

In this project, statcast data for the 2024 Major League Baseball(MLB) season will be used to analyze hitters, where they will be divided into four groups (Pure contact, Pure power, Elite hybrid, Role player) and will undergo further analysis within the group. 

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
    

=======

`01_data_collection.ipynb`
`02_feature_importance.ipynb`

These two files in notebooks folder collect relevant data to the project.

`03_feature_analysis.ipynb`
`04_visualization.ipynb`

These two files are the Python notebook where the code is implemented.

`data`
`images`

These two folders involve datafiles (in .csv) and images used in this project.

## Data & Methodology

This project investigates the relationship between hitters' physical traits and at-bat statistics provided through Statcast.

1. Data Sources & Integration
Statcast (via `pybaseball`): We collected 2024 regular-season data focusing on physical hitting metrics: Average Bat Speed and Contact %.

FanGraphs & Fielding Metrics: To capture holistic player value, we merged batting data with WAR (Wins Above Replacement), OAA (Outs Above Average), and Stolen Bases (SB).

Sample Size: The analysis covers all qualified hitters and 30 MLB team rosters, cross-referenced with regular-season win totals and postseason outcomes.


2. The 4-Quadrant Hitter Classification
![Hitter Classification](/assets/images/02_hitter_segmentation_map.png)

We categorized hitters into four strategic groups based on the league-wide median values of Bat Speed and Contact Rate.

These two metrics were selected to capture the fundamental trade-off relationship in hitting mechanics and to distinguish physical inputs from statistical outputs. For instance, a hitter who maximizes their swing effort (High Bat Speed) gains power potential but often at the expense of precision (Low Contact Rate). Furthermore, unlike results-based metrics such as Batting Average or Home Runs, Contact Rate and Bat Speed serve as "pure inputs," representing a player's inherent physical approach rather than the outcome of a plate appearance.

Each of the four quadrants represents a distinct strategic profile:

Type A (Pure Contact): Low Bat Speed / High Contact. Precision-oriented hitters who stabilize the lineup "floor" by minimizing strikeouts (K%) and maintaining a high frequency of balls in play.

Type B (Pure Power): High Bat Speed / Low Contact. High-risk, high-return sluggers who provide raw power (SLG) and "ceiling" at the cost of higher swing-and-miss rates.

Type C (Elite Hybrid): High Bat Speed / High Contact. The "Core" of a championship roster, representing rare MVP-caliber production where the trade-off between power and contact is minimized.

Type D (Role Player): Low Bat Speed / Low Contact. Tactical assets who contribute through non-batting metrics such as Defense (OAA), Baserunning (SB), and Positional Utility.

3. Feature Engineering: Utility Score & ML-Based Ranking

*3.1 - Utility Score*  

To quantify "Roster Flexibility"—a critical asset in a 162-game season—we assigned a numerical value (1–4) to each player’s primary defensive role. This score reflects the tactical versatility a player provides to a manager's lineup construction.

1 Point (Corner Specialists): 1B, 3B, LF, RF. Positions requiring specialized defensive profiles with limited lateral mobility.

2 Points (Field Anchors): CF, C. High-leverage defensive roles that serve as the structural backbone of the field.

3 Points (Keystone Duo): 2B, SS. High-mobility middle infield roles that demand elite range and transition speed.

4 Points (Tactical Multi-Utility): Players capable of rotating through multiple positions (indicated by a / or - in their profile). These players act as "Force Multipliers," allowing the team to cover multiple defensive needs with a single roster spot.

*3.2 - ML-Based Ranking*

Unlike traditional models that use arbitrary weights, we implemented a Random Forest Regressor to determine the optimal weighting for our ranking system. This ensures that our "Rank Score" reflects the actual statistical contribution of each metric to team success.

Objective Weighting: For Types A, B, and C, the model identifies which features (e.g., AVG, SLG, K%) most significantly impact Expected Weighted On-Base Average (xwOBA)—the gold standard for run production.

Addressing Undervaluation (Type D): For Role Players (Type D), the model prioritizes Outs Above Average (OAA) and Stolen Bases (SB). By training the model to recognize how defensive and baserunning metrics prevent runs or create scoring opportunities, we "rediscover" the hidden value of players often overlooked by conventional batting statistics.

The final Rank Score for each player is calculated by multiplying their standardized Z-score by the machine-learning-derived weight ($W$):$$\text{Rank Score} = \sum (\text{Z-score}_{i} \times W_{i})$$

Type A (Precision): High weights on AVG and K% suppression. This identifies elite contact hitters like Mookie Betts who maximize efficiency despite lower raw bat speeds.
Type B (Power): Strong emphasis on Exit Velocity and SLG, highlighting the "Ceiling" of the roster.
Type C (Elite Hybrid): A comprehensive weighting of elite production metrics (xwOBA, OPS) to identify the core superstars of the league.
Type D (Specialist): Prioritizes Defense (OAA) and Speed (SB), providing a statistical justification for the "Utility" role in championship-cal
iber rosters.

5. Bubble Graph Visualization
![Type A](/assets/images/04_vis_type_a.png)
![Type B](/assets/images/04_vis_type_b.png)
![Type C](/assets/images/04_vis_type_c.png)
![Type D](/assets/images/04_vis_type_d.png)
Through multi-dimensional bubble graphs, we compare players within each type using three key metrics that define their strategic value. This visualization allows us to identify trends and "outliers" (High-efficiency players) within each specific hitter profile.

| Hitter Type | X-Axis (Efficiency) | Y-Axis (Output) | Bubble Size (Discipline/Versatility) |
|---|---|---|---|
| Type A (Precision) | AVG (Batting Average) | K% (Strikeout Rate) | Chase % (Plate Discipline) |
| Type B (Power) | Avg Exit Velocity | SLG (Slugging Percentage) | Sweet Spot % (Batted Ball Quality) |
| Type C (Elite Hybrid) | xwOBA (Expected Value) | OPS (On-base + Slugging) | XBH (Extra Base Hits) |
| Type D (Utility) | Stolen Bases (SB) | DRS / OAA (Defensive Value) | Utility Score (Positional Flexibility) |

## Key Findings

1. The Strategic Hierarchy: Beyond Traditional Batting Average
Our analysis of the 2024 MLB season through the 4-Quadrant Model reveals that a player's value is highly dependent on their physical profile. By setting the Median Bat Speed and Contact % as benchmarks, we identified that rosters are composed of distinct strategic assets.

Type C (Elite Hybrid) as the Ceiling: Players in this quadrant provide the highest offensive "ceiling," combining raw power with elite contact.

Type A (Precision) as the Floor: These hitters serve as the "stabilizers" of a roster. By maintaining high contact rates despite lower bat speeds, they minimize strikeouts (K%) and ensure a consistent presence on base, providing the necessary "floor" for a winning team.

2. Redefining the 'Role Player' (Type D)
One of the most significant findings is the hidden value of Type D hitters. While traditional box scores might undervalue them due to lower offensive output, our Custom Rank Score highlights their critical role in "Run Prevention."

Defensive Specialists: High-ranking Type D players excel in OAA (Outs Above Average) and possess a high Utility Score (2.5+).

Force Multipliers: Through elite Baserunning (SB) and the ability to play multiple premium positions (SS, CF, 2B), these players provide tactical flexibility that allows managers to optimize matchups in late-inning scenarios, effectively acting as "force multipliers" for the team's overall win probability.


## Limitations

While the 4-Quadrant Model and Custom Rank Score provide a robust framework for hacking a major league roster, several inherent limitations must be acknowledged:

1. The Volatility of Short-Sample Statcast Data
Context: Physical traits like Bat Speed and Contact % can fluctuate based on a player's health, fatigue, or specific pitcher matchups over a single season (2024). Although it was prevented by implementing a filter of PA>200, there still is a possibility that 

Impact: A player's "Type" classification is a snapshot of their 2024 performance. Long-term career trajectories or aging curves were not incorporated, which may lead to "Type-shifting" in subsequent seasons.

2. The "Unquantifiables" of Baseball
Context: Our model focuses strictly on quantifiable physical and statistical outputs (EV, SB, OAA).

Impact: It ignores critical "soft" factors such as Clubhouse Chemistry, Veteran Leadership, and Managerial Strategy. A Type D player might provide value through mentorship or locker room presence that isn't captured by an OAA or Utility Score.

3. Positional Defensive Weighting Bias
Context: The Utility Score (1-4) is a heuristic designed to measure flexibility, but it may overvalue "versatile but average" defenders over "elite but specialized" ones.

Impact: An elite 1B (Type B/C) might contribute more to run prevention than a mediocre multi-utility player (Type D), yet the Utility Score would favor the latter. Future iterations could integrate Defensive Runs Saved (DRS) more heavily to balance this.

4. Sample Size and Positional Scarcity
Context: Certain quadrants, particularly Type C (Elite Hybrid), have a smaller sample size compared to Type D (Role Players).

Impact: Statistical normalization (Z-score) helps, but the extreme scarcity of "high speed + high contact" players makes it difficult to draw universal conclusions about how to "acquire" such talent within a fixed budget (Salary Cap).

