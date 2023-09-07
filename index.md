# Optimizing a Fantasy Football Team: A Data Science Approach
## Introduction and Motivations
With the 2023 NFL season quickly approaching, there is one thing on every "hardcore" football fans mind: Fantasy Football. Whether one is just playing for the thrill of the game, or has entered into a competitive league where a "buy-in" is on the line, Fantasy General Managers across the nation are pulling out all the stops in order to optimize their chances at winning big. 
I have entered a "money league" myself, only time will tell whether this is for better or worse, where the potential to take home 240 dollars necessitates a careful week by week strategy for setting my team up for success. While many GM's invest a lot of time and effort into drafting the most ideal team right out of the gate, I unfortunatley had to miss my own draft and attend classes at the University of Pennsylvania. Whether or not my league purposely scheduled the draft for a time where they knew I would be busy is unconfirmed, but I like to believe that they percieved my Fantasy Football prowess as a threat and tried to sandbag me with ESPN's autodraft system. Thats in the past. The best I can do is work with the team I was given and optimize each week.
This blog post aims to take a data-driven approach to optimize a fantasy football team, focusing on the PPR (Point Per Reception) league format.

## Methodology
### Data Collection
The first step in the optimization process was determing what sort of data would be relevant for my purposes. I determined that a reasonably accurate projection for player performance could be created by looking at data gathered in the 2022 Fantasy Season. It was important to find a comprehensive data set for all players in the NFL not just to inform whom I should start within my own team, but to also optimize my ability to pick players off of free agency who would be likely to outperform players that I had already drafted.

### Data Sources

1.) Community-Generated Datasets: Datasets from the Fantasy Football community on platforms like Reddit were considered. However, the validity of these datasets must be carefully considered as they are entered by fans who have their own agendas and may be prone to entering data incorrectly. Additionally, the datasets found off these community platforms were created with a variety of formats placing emphasis on metrics that were not considered or were considered to be irrelavant to my analysis. Key statistcs that were gleaned from these large datasets required extensive cleaning and flitering both manually and with the aid of LLMs (i.e. ChatGPT).

2.) ESPN Projections: Given the lack of "official" peer-reviewed datasets that are freely available, I turned to ESPN for player projections and statistics. The data from ESPN is considered reliable and is widely used in the fantasy football community. Some of these datasets were behind paywalls and EULAs that prevented easy download and assessment. However, enough data was able to be collected, for a more basic analysis, for my purposes. ESPN Statistics can be considered a valid resource because it is in partnership with the NFL, employs a team of experts who have years of experience in the sport industry, and are considered by many to an established brand. 

3.) Manual Data Collection: For specific players who were not present in our chosen dataset or for whom I needed more detailed information, manual data collection was carried out. This was particularly useful for rookies or less-known players. This method included doing general research into college performances as well as any relevant news articles resulting in manual extraction of key data points.

Here is an excerpt from the full set of offense data that was pooled from the sources outlined above.

| Player         | Pos   | Team   |   Passing Yds |   Passing TD |   Interceptions |   Rushing Yds |   Rushing TD |   Receving Yds |   Receptions |   Receving TDs |   Total Fumbles |   2PT Conversion Made |
|:---------------|:------|:-------|--------------:|-------------:|----------------:|--------------:|-------------:|---------------:|-------------:|---------------:|----------------:|----------------------:|
| J. Allen       | QB    | Buf    |          4407 |           36 |              15 |           763 |            6 |              0 |            0 |              0 |               8 |                     3 |
| J. Herbert     | QB    | LAC    |          5014 |           38 |              15 |           302 |            3 |              0 |            0 |              0 |               1 |                     6 |
| P. Mahomes     | QB    | KC     |          4839 |           37 |              13 |           381 |            2 |              0 |            0 |              0 |               9 |                     2 |
| T. Brady       | QB    | TB     |          5316 |           43 |              12 |            81 |            2 |              0 |            0 |              0 |               4 |                     0 |
| J. Burrow      | QB    | Cin    |          4611 |           34 |              14 |           118 |            2 |              0 |            0 |              0 |               5 |                     1 |
| M. Stafford    | QB    | LAR    |          4886 |           41 |              17 |            43 |            0 |              0 |            0 |              0 |               5 |                     2 |
| D. Prescott    | QB    | Dal    |          4449 |           37 |              10 |           146 |            1 |              0 |            0 |              0 |              14 |                     3 |
| A. Rodgers     | QB    | GB     |          4115 |           37 |               4 |           101 |            3 |             -4 |            1 |              0 |               3 |                     0 |
| R. Tannehill   | QB    | Ten    |          3734 |           21 |              14 |           270 |            7 |              0 |            0 |              0 |              10 |                     1 |
| D. Carr        | QB    | LV     |          4804 |           23 |              14 |           108 |            0 |              0 |            0 |              0 |              13 |                     0 |
| J. Hurts       | QB    | Phi    |          3144 |           16 |               9 |           784 |           10 |              0 |            0 |              0 |               9 |                     3 |
| K. Murray      | QB    | Ari    |          3787 |           24 |              10 |           423 |            5 |              7 |            0 |              0 |              13 |                     0 |
| K. Cousins     | QB    | Min    |          4221 |           33 |               7 |           115 |            1 |              0 |            0 |              0 |              12 |                     0 |
| M. Ryan        | QB    | Ind    |          3968 |           20 |              12 |            82 |            1 |              0 |            0 |              0 |              11 |                     1 |
| L. Jackson     | QB    | Bal    |          2882 |           16 |              13 |           767 |            2 |              0 |            0 |              0 |               6 |                     2 |
| C. Wentz       | QB    | Was    |          3563 |           27 |               7 |           215 |            1 |              0 |            0 |              0 |               8 |                     2 |
| T. Heinicke    | QB    | Was    |          3419 |           20 |              15 |           313 |            1 |             -2 |            1 |              0 |               7 |                     1 |
| T. Lawrence    | QB    | Jax    |          3641 |           12 |              17 |           334 |            2 |              0 |            0 |              0 |               9 |                     2 |
| M. Jones       | QB    | NE     |          3801 |           22 |              13 |           129 |            0 |              0 |            0 |              0 |               7 |                     2 |
| J. Garoppolo   | QB    | SF     |          3810 |           20 |              12 |            51 |            3 |              0 |            0 |              0 |               8 |                     1 |
| R. Wilson      | QB    | Den    |          3113 |           25 |               6 |           183 |            2 |              0 |            0 |              0 |               6 |                     1 |
| B. Mayfield    | QB    | Car    |          3010 |           17 |              13 |           134 |            1 |             11 |            0 |              0 |               6 |                     2 |
| J. Goff        | QB    | Det    |          3245 |           19 |               8 |            87 |            0 |              0 |            0 |              0 |               9 |                     4 |
| T. Bridgewater | QB    | Mia    |          3052 |           18 |               7 |           106 |            2 |              0 |            0 |              0 |               1 |                     1 |
| J. Taylor      | RB    | Ind    |             0 |            0 |               0 |          1811 |           18 |            360 |           40 |              2 |               4 |                     0 |
| Z. Wilson      | QB    | NYJ    |          2334 |            9 |              11 |           185 |            4 |              0 |            0 |              0 |               5 |                     2 |
| S. Darnold     | QB    | Car    |          2527 |            9 |              13 |           222 |            5 |              0 |            0 |              0 |               9 |                     1 |
| T. Tagovailoa  | QB    | Mia    |          2653 |           16 |              10 |           128 |            3 |              0 |            0 |              0 |               9 |                     1 |
| D. Mills       | QB    | Hou    |          2664 |           16 |              10 |            44 |            0 |              0 |            0 |              0 |               5 |                     2 |
| D. Jones       | QB    | NYG    |          2428 |           10 |               7 |           298 |            2 |             16 |            1 |              0 |               7 |                     3 |
| C. Kupp        | WR    | LAR    |             0 |            0 |               0 |            18 |            0 |           1947 |          145 |             16 |               0 |                     1 |
| A. Ekeler      | RB    | LAC    |             0 |            0 |               0 |           911 |           12 |            647 |           70 |              8 |               4 |                     2 |
| J. Fields      | QB    | Chi    |          1870 |            7 |              10 |           420 |            2 |              0 |            0 |              0 |              12 |                     0 |
| J. Mixon       | RB    | Cin    |             0 |            0 |               0 |          1205 |           13 |            314 |           42 |              3 |               2 |                     0 |
| N. Harris      | RB    | Pit    |             0 |            0 |               0 |          1200 |            7 |            467 |           74 |              3 |               0 |                     0 |
| D. Samuel      | WR    | SF     |            24 |            1 |               0 |           365 |            8 |           1405 |           77 |              6 |               4 |                     0 |
| J. Conner      | RB    | Ari    |             0 |            0 |               0 |           752 |           15 |            375 |           37 |              3 |               2 |                     0 |
| D. Adams       | WR    | LV     |             0 |            0 |               0 |             0 |            0 |           1553 |          123 |             11 |               0 |                     0 |
| J. Jefferson   | WR    | Min    |            35 |            0 |               0 |            14 |            0 |           1616 |          108 |             10 |               1 |                     0 |
| E. Elliott     | RB    | Dal    |             4 |            0 |               0 |          1002 |           10 |            287 |           47 |              2 |               1 |                     3 |
| N. Chubb       | RB    | Cle    |             0 |            0 |               0 |          1259 |            8 |            174 |           20 |              1 |               2 |                     0 |
| A. Gibson      | RB    | Was    |             0 |            0 |               0 |          1037 |            7 |            294 |           42 |              3 |               6 |                     1 |
| J. Chase       | WR    | Cin    |             0 |            0 |               0 |            21 |            0 |           1455 |           81 |             13 |               2 |                     0 |
| L. Fournette   | RB    | TB     |             0 |            0 |               0 |           812 |            8 |            454 |           69 |              2 |               1 |                     0 |
| A. Kamara      | RB    | NO     |             0 |            0 |               0 |           898 |            4 |            439 |           47 |              5 |               0 |                     0 |
| M. Andrews     | TE    | Bal    |             0 |            0 |               0 |             0 |            0 |           1361 |          107 |              9 |               1 |                     2 |
| C. Patterson   | RB    | Atl    |             0 |            0 |               0 |           618 |            6 |            548 |           52 |              5 |               1 |                     0 |
| D. Harris      | RB    | NE     |             0 |            0 |               0 |           929 |           15 |            132 |           18 |              0 |               2 |                     0 |
| T. Hill        | WR    | Mia    |             0 |            0 |               0 |            96 |            0 |           1239 |          111 |              9 |               2 |                     0 |
| D. Cook        | RB    | Min    |             0 |            0 |               0 |          1159 |            6 |            224 |           34 |              0 |               3 |                     1 |

[The full file is accessible here:](https://github.com/Cjcapiola/Week-1-Fantasy-Football-Optimization/raw/main/New%20Fantasy%202022%20Season%20Totals.xlsx)
### Relevant Metrics

By focusing on just the relevant metrics to the way my PPR League was scored, I was able to further remove an extraneous data and focus on what really mattered.

For example:

* Quarterbacks (QB): Passing yards, Passing touchdowns, Interceptions, 2-point conversions.
* Running Backs (RB): Rushing yards, Rushing touchdowns, Receptions, Receiving yards, 2-point conversions.
* Wide Receivers (WR) and Tight Ends (TE): Receptions, Receiving yards, Receiving touchdowns, 2-point conversions.

I assigned each metric a point value based on typical fantasy football scoring rules. For example, quarterbacks earn 0.04 points for each passing yard and 4 points for each passing touchdown.

### Data Cleaning
Before diving into the analysis, the dataset was thoroughly cleaned and pruned to only include relevant metrics.

```python
# Load the dataset
df = pd.read_excel('path/to/dataset.xlsx')

# Drop irrelevant columns
df.drop(['Irrelevant_Column1', 'Irrelevant_Column2'], axis=1, inplace=True)
```
This code snippet loads the dataset into a Pandas DataFrame and removes irrelevant columns. The drop() method is used to get rid of columns that I do not need for this specific analysis.

### Calculating Per-Game Stats
I then converted season-long statistics into per-game statistics to have a standardized measure for each player.

```python
# Calculate per-game statistics
df['PassingYards_PerGame'] = df['PassingYards'] / 18
df['RushingYards_PerGame'] = df['RushingYards'] / 18
```
This code snippet calculates per-game stats by dividing the total stats by the number of games in the 2022 season (18 games). This allows the analysis to make fair comparisons between players.

## Data Analysis 
### Scoring Projections
The main goal is to project the number of points each player will score in future games based on past performance.

```python
# Calculate fantasy points for passing yards
df['FantasyPoints_Passing'] = df['PassingYards_PerGame'] * 0.04
```
In this snippet, I calculate the fantasy points for passing yards based on the league's scoring settings, which award 0.04 points for each passing yard. Similar calculations were done for other scoring categories.

## Results
Based on my calculations, I can estimate the number of fantasy points each player is expected to score in future games. A predictive model for player performance was generated to optimize the team lineup each week, taking into account the various influencing factors.

### Key Findings

* Starting Quarterback: D. Prescott vs. K. Cousins
  * D. Prescott has consistently outperformed K. Cousins in multiple metrics, including passing yards and touchdowns. Prescott's average fantasy points per game is higher, making him the clear choice for the starting QB position.
* Running Backs: T. Pollard vs. D. Cook
  * D. Cook is the standout choice in this case. He has higher average rushing yards and touchdowns. While Pollard has potential, Cook's proven track record and the fact that he is the primary back for his team make him an essential start.
* Wide Receivers: D. Adams vs. D. Johnson vs. M. Gallup
  * D. Adams is a must-start due to his high target share and touchdown upside.
  * D. Johnson should also be in the starting lineup due to consistent performance in receptions and yards.
  * M. Gallup should be considered for a flex spot, but only if you don't have a better option at RB or TE.
* Tight End: K. Pitts
  * K. Pitts is the clear choice for the TE position. His receiving yards and targets are substantially higher than any other TE option you have.
* Flex: T. Etienne Jr vs. R. Stevenson
  * T. Etienne Jr. has shown promise in both rushing and receiving, making him a valuable flex option, especially in PPR leagues. Stevenson might be a decent play but only if Etienne is facing a particularly tough defense.
* Kicker: G. Gano
  * G. Gano has a strong record in field goal accuracy, especially in the 40-49 yard range, making him a reliable kicker for your team.
* Defense: Browns
  * The Browns Defense has performed consistently in terms of sacks and interceptions. Keep an eye on the matchup, but they are generally a safe bet.
* Waiver Wire Watch
  * QB: R. Wilson and M. Stafford are top considerations if available.
  * RB: D. Foreman and C. Hubbard could provide value if one of your starters is injured.
  * WR: M. Valdes-Scantling and J. Williams have high upside and could be valuable in future weeks.
  * TE: G. Everett and T. Conklin are strong TE options if Pitts is on a bye or injured.

## Conclusion
### Summary
By applying data science techniques to Fantasy Football I introduced a somewhat novel way of approaching this years season. Most people that play Fantasy Football casually draft players and make lineup decsions based on "gut-feelings" or personal biases (i.e. drafting J. Hurts because you live in Philadelphia and are a staunch Eagles supported). By using an LLM and data science techniques, I attempted to remove the subjective nature of the game and optimize my performance based on real performance statistics. However, it is important to note that while probability can be used to inform decisons, Football, and by extension Fantasy Football, is an emotional game. For example, a historically poor team could develop a "chip on their shoulder" and play a certain matchup with a ferocity and passion that results in real game performance, but cannot be accounted for in a projection based off of 2022 statistics. Additionally, negative coverage of a certain team could induce a self-fullfilling prophecy in a player who has scored historically well. Consider negative press surrounding a veteran quarterback that is concerned with the question of "Whether or not he is washed up?". Some player may consider this a challenge and play harder resulting in another decent scoring performance, or they could let doubts creep in and play poorly. The emotional aspect of the game cannot be attributed to hard statistics, however improvments in LLM's ability to analyse press coverage and the development of Neural Nets that better mimic human decsion making is an exciting possibility in this kind of analysis.


### Challenges and Learning Points
* Challenge 1: Data Sourcing
  * Issue: Finding an extensive and reliable dataset for fantasy football was challenging. Many datasets were either behind a paywall, incomplete, or did not provide metrics relevant to a PPR (Points Per Reception) league.
  * Solution: I decided to use a fan-collected dataset from Reddit. While this data source may not be peer-reviewed, it is extensive and aligns closely with the metrics we are interested in. I supplemented this with data manually extracted from ESPN for specific players.
* Challenge 1: Data Cleaning and Preprocessing
  * Issue: The Reddit dataset was extensive but required significant cleaning. It had many columns that were not relevant for our analysis, and some columns were formatted in a way that made automated processing difficult.
  * Solution: I manually identified the relevant columns and used Python's Pandas library to clean the dataset. I also manually provided data for players missing in the dataset, including college statistics for rookies.
* Challenge 3: Missing Data for Specific Positions (Kickers and Defenses)
  * Issue: Our primary dataset did not have information about kickers and defenses, which are also part of a fantasy football team. Defense are typically left out of large scale datasets becuase they quanitfy the play impact of 11 different players. Therefore it is difficult to predict how inuries afflicting one player on the defense, coaching changes within the organization, or various offensive matchup formations affect the scoring impact of Defenses. Kickers also do not have a large source of data because they contribute relativly little into the total score. While they are important, they are not as flashy which leads many fan-created datasets to ommit their statistics.
  * Solution: Data was sourced manually from ESPN News which covers more intangible elements of the game. These articles also included oppinion pieces by "ESPN Insiders" who made comments on how they thought injuries and coaching changes might affect the function of a teams defense. These articles were manually assessed and any relevant information was fed into analysis via natural language.
* Challenge 4: Average Score Calculation
  * Issue: The dataset provided season totals, but I needed per-game averages to make weekly start/sit decisions.
  * Solution: With use of an LLM, I calculated per-game averages using the total season statistics and the number of games played. This data was then used to compute fantasy points based on the my league's scoring system.
* Challenge 5: Computational Limitations, ChatGPT Usage Caps, and Plugin Errors
  * Issue: The usage cap on GPT-4 could be problematic for large datasets, and the advanced data analysis and plugin features required separate conversation threads, complicating the process. The large dataset analyised was also prone to creating computational timeouts.
  * Solution: I broke down the data into smaller chunks and reiterated numerous times through the data cleaning process in order to eliminate any extrenous data which would complicate the LLM's computations. I also manually integrated the results from different conversation threads for a comprehensive analysis allowing me to combine information gleaned from both ChatGPT Pluggin, which allowed analysis of news articles, and ChatGPT Advanced Data Analysis, which allowed for the bulk of computational projection.

### Future Directions
In future work, I aim to improve the ability of the model to predict player performances by integrating more dynamic factors such as weather conditions, day to day player health analysis (based on public domain practice footage), and specific coaching strategies (an analysis of specifc coaches, their coaching record, and play style). The ability of a model like this one will continue to grow as the bugs in LLM are worked out and their ability to webscrape for specific news article and analyse video footage is improved.
