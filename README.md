
# Experiment 1: Exploratory Data Analysis (EDA) on IPL Dataset

## Aim

To perform Exploratory Data Analysis (EDA) on the IPL matches dataset and derive insights into matches played per season, top winning teams, toss decisions, and venues hosting the maximum number of matches.

## Algorithm / Procedure

### Step 1: Import Libraries
- Import Pandas for data manipulation and analysis.
- Import Matplotlib and Seaborn for data visualization.

### Step 2: Load the Dataset
- Load the IPL matches dataset using `pd.read_csv()`.
- Display the dataset dimensions using `.shape`.
- Display the first five records using `.head()`.

### Step 3: Analyze Matches per Season
- Group the dataset by season.
- Count the number of matches played in each season.
- Plot a bar chart to visualize the distribution of matches across seasons.

### Step 4: Analyze Top Winning Teams
- Count the occurrences of each team in the `winner` column.
- Select the top five teams with the highest number of wins.
- Visualize the results using a bar chart.

### Step 5: Analyze Toss Decisions
- Count the toss decisions, such as batting and fielding.
- Plot a bar chart to compare the toss decision preferences.

### Step 6: Identify Top Venues
- Count the number of matches played at each venue.
- Sort the venues in descending order of match count.
- Select the top five venues.
- Display the results using a horizontal bar chart.
- Highlight the stadium hosting the maximum number of matches.

### Step 7: Draw Insights
- Observe the distribution of matches across seasons.
- Identify teams with the highest number of wins.
- Compare batting and fielding toss decisions.
- Identify venues hosting the most IPL matches.

## Python Program

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
df = pd.read_csv("matches.csv")

# Display dataset information
print("Dataset Shape:", df.shape)
print(df.head())

# 1. Matches per Season
season_counts = df["season"].value_counts().sort_index()

plt.figure(figsize=(10, 5))
season_counts.plot(kind="bar")
plt.title("Matches per IPL Season")
plt.xlabel("Season")
plt.ylabel("Number of Matches")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# 2. Top 5 Winning Teams
winner_counts = df["winner"].value_counts().head(5)

plt.figure(figsize=(10, 5))
winner_counts.plot(kind="bar")
plt.title("Top 5 Winning Teams")
plt.xlabel("Team")
plt.ylabel("Number of Wins")
plt.xticks(rotation=45, ha="right")
plt.tight_layout()
plt.show()

# 3. Toss Decisions
toss_counts = df["toss_decision"].value_counts()

plt.figure(figsize=(7, 5))
toss_counts.plot(kind="bar")
plt.title("Toss Decision Analysis")
plt.xlabel("Toss Decision")
plt.ylabel("Number of Matches")
plt.tight_layout()
plt.show()

# 4. Top 5 Venues
venue_counts = df["venue"].value_counts().head(5)

plt.figure(figsize=(10, 5))
venue_counts.sort_values().plot(kind="barh")
plt.title("Top 5 IPL Venues by Number of Matches")
plt.xlabel("Number of Matches")
plt.ylabel("Venue")
plt.tight_layout()
plt.show()

# Highlight the venue hosting the maximum matches
if not venue_counts.empty:
    top_venue = venue_counts.index[0]
    max_matches = venue_counts.iloc[0]

    print("\nVenue Hosting Maximum Matches")
    print("Venue:", top_venue)
    print("Number of Matches:", max_matches)
```


## Output Screenshots

### 1. Dataset Preview
Add a screenshot showing the dataset shape and first five records.

### 2. Matches per Season
Add the bar chart showing matches played in each IPL season.

### 3. Top Winning Teams
Add the bar chart showing the five teams with the highest win counts.

### 4. Toss Decision Analysis
Add the bar chart comparing toss decisions.

### 5. Top IPL Venues
Add the horizontal bar chart highlighting the five venues hosting the most matches.

### 6. Maximum-Match Venue
Add a screenshot of the printed venue name and its match count.

## Result

Exploratory Data Analysis was performed successfully on the IPL matches dataset. The analysis visualized matches per season, top winning teams, toss decisions, and the venues hosting the maximum number of matches.
