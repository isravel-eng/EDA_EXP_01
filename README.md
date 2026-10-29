# Experiment 1: Exploratory Data Analysis (EDA) on IPL Dataset

## Aim

To perform Exploratory Data Analysis (EDA) on the IPL matches dataset and answer the 15 questions using Pandas in Google Colab.

## Dataset

Upload `matches.csv` to the Colab session or place it in the same working directory as the notebook.

## Q1. Dataset Understanding

**How many rows and columns are present in the dataset? Display the first 5 records.**

```python
import pandas as pd
df = pd.read_csv("matches.csv")
print("Rows and columns:", df.shape)
display(df.head(5))
```

## Q2. Dataset Understanding

**What are the columns and their data types in the dataset?**

```python
print(df.dtypes)
```

## Q3. Primary Key / Dataset Understanding

**Which column can act as a unique identifier (Primary Key) for each IPL match? Verify whether its values are unique.**

```python
print("Candidate primary key: id")
print("All IDs are unique:", df["id"].is_unique)
print("Duplicate ID count:", df["id"].duplicated().sum())
```

## Q4. Data Cleaning – Missing Values & Duplicates

**Identify the columns containing missing values, handle missing values in `city`, and remove duplicate records.**

```python
missing_values = df.isnull().sum()
print("Missing values by column:")
print(missing_values[missing_values > 0])

df["city"] = df["city"].fillna("Unknown")
df = df.drop_duplicates().reset_index(drop=True)

print("Shape after cleaning:", df.shape)
print("Missing values in city:", df["city"].isnull().sum())
print("Duplicate records remaining:", df.duplicated().sum())
```

## Q5. GroupBy – Season Analysis

**Find the number of matches played in each IPL season and identify the season with the highest number of matches.**

```python
season_matches = df.groupby("season").size().sort_values(ascending=False)
print("Matches played in each season:")
print(season_matches.sort_index())
print("\nSeason with the highest number of matches:")
print(season_matches.head(1))
```

## Q6. GroupBy & Sorting – Team Performance

**Calculate the number of wins for each team and display the Top 5 winning teams in descending order.**

```python
team_wins = df["winner"].value_counts()
print("Wins for each team:")
print(team_wins)
print("\nTop 5 winning teams:")
print(team_wins.head(5))
```

## Q7. Filtering – Team Analysis

**Filter matches won by Chennai Super Kings and display the match details and total number of CSK wins.**

```python
csk_wins = df[df["winner"] == "Chennai Super Kings"]
display(csk_wins)
print("Total Chennai Super Kings wins:", len(csk_wins))
```

## Q8. Value Counts – Toss Analysis

**Find the frequency and percentage of each toss decision and identify the most preferred decision.**

```python
toss_frequency = df["toss_decision"].value_counts()
toss_percentage = df["toss_decision"].value_counts(normalize=True).mul(100).round(2)
toss_analysis = pd.DataFrame({
    "Frequency": toss_frequency,
    "Percentage": toss_percentage
})
print(toss_analysis)
print("Most preferred toss decision:", toss_frequency.idxmax())
```

## Q9. Cross-Tabulation – Season-wise Toss Analysis

**Perform a season-wise analysis of toss decisions and identify the most preferred toss decision in each season.**

```python
season_toss = pd.crosstab(df["season"], df["toss_decision"])
print("Season-wise toss decisions:")
display(season_toss)
print("\nMost preferred toss decision in each season:")
print(season_toss.idxmax(axis=1))
```

## Q10. GroupBy & Sorting – Venue Analysis

**Identify the Top 5 IPL venues based on the number of matches played.**

```python
top_venues = df["venue"].value_counts().head(5)
print("Top 5 venues by matches played:")
print(top_venues)
```

## Q11. Aggregation & Sorting – Winning Margin Analysis

**Find the match with the largest winning margin and display the Top 10 matches with the largest winning margins.**

```python
margin_df = df[df["result_margin"].notna()].copy()
margin_df["result_margin"] = pd.to_numeric(margin_df["result_margin"], errors="coerce")
margin_df = margin_df.dropna(subset=["result_margin"])

display(margin_df.nlargest(10, "result_margin"))
print("Match with the largest winning margin:")
display(margin_df.nlargest(1, "result_margin"))
```

## Q12. Value Counts – Match Result Analysis

**Find the frequency of each type of match result, such as runs, wickets, tie, and no result.**

```python
result_frequency = df["result"].fillna("no result").replace({
    "tie": "tie",
    "no result": "no result"
}).value_counts()
print(result_frequency)
```

## Q13. Data Transformation – Datetime Conversion

**Convert the `date` column to Datetime format and display its data type before and after conversion.**

```python
print("Data type before conversion:", df["date"].dtype)
df["date"] = pd.to_datetime(df["date"], errors="coerce")
print("Data type after conversion:", df["date"].dtype)
```

## Q14. Data Transformation – Year Extraction

**Create a new `year` column by extracting the year from the `date` column.**

```python
df["year"] = df["date"].dt.year
display(df[["date", "year"]].head())
```

## Q15. Data Transformation – Column Creation

**Create a new `win_type` column with meaningful descriptions for runs, wickets, tie, and no result.**

```python
win_type_map = {
    "runs": "Won by Runs",
    "wickets": "Won by Wickets",
    "tie": "Tie",
    "no result": "No Result"
}
df["win_type"] = df["result"].map(win_type_map).fillna("No Result")
display(df[["result", "win_type"]].head(10))
```

## Result

Exploratory Data Analysis was performed on the IPL matches dataset using Pandas. The questions cover dataset understanding, primary-key validation, data cleaning, season and team analysis, toss decisions, venues, winning margins, match results, and date-based transformations.
