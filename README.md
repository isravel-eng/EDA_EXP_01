# Experiment 1: Exploratory Data Analysis (EDA) on IPL Dataset

## Aim

To perform Exploratory Data Analysis (EDA) on the IPL matches dataset and answer the 15 questions using Pandas in Google Colab.

## Dataset

Upload `matches.csv` to the Colab session or place it in the same working directory as the notebook.


## Algorithm / Procedure
1. Start the program. <br>
2. Import the Pandas library and load matches.csv into a DataFrame. <br> 
3. Display the dataset shape, first five records, column names, and data types.<br>
4. Check the uniqueness of the match ID and identify missing values and duplicate records.<br>
5. Fill missing city values and remove duplicate records.<br>
6. Group the data by season to find match counts and identify the season with the most matches.<br>
7. Count team wins, display the top five winning teams, and filter Chennai Super Kings matches.<br>
8. Analyze toss decisions using frequency counts, percentages, and season-wise cross-tabulation.<br>
9. Identify the top five venues and the matches with the largest winning margins.<br>
10. Count different match results, convert the date column to datetime format, and extract the year.<br>
11. Create a new win_type column to describe match results.<br>
12. Display the results and stop the program.<br>

## Q1. Dataset Understanding

**How many rows and columns are present in the dataset? Display the first 5 records.**

```python
import pandas as pd
df = pd.read_csv("matches.csv")
print("Rows and columns:", df.shape)
display(df.head(5))
```

<img width="1347" height="422" alt="image" src="https://github.com/user-attachments/assets/27ed6dac-ff44-49ac-9db6-9b2da8cc5d38" />



## Q2. Dataset Understanding

**What are the columns and their data types in the dataset?**

```python
print(df.dtypes)
```

<img width="277" height="379" alt="image" src="https://github.com/user-attachments/assets/96514ccb-c77e-4c99-bf7a-72d7e1d9e82c" />


## Q3. Primary Key / Dataset Understanding

**Which column can act as a unique identifier (Primary Key) for each IPL match? Verify whether its values are unique.**

```python
print("Candidate primary key: id")
print("All IDs are unique:", df["id"].is_unique)
print("Duplicate ID count:", df["id"].duplicated().sum())
```

<img width="240" height="68" alt="image" src="https://github.com/user-attachments/assets/389e5424-0d83-44b7-9a0d-c503f6549448" />


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

<img width="336" height="217" alt="image" src="https://github.com/user-attachments/assets/dad93c74-70c9-4c40-8e81-b9f8805b8228" />


## Q5. GroupBy – Season Analysis

**Find the number of matches played in each IPL season and identify the season with the highest number of matches.**

```python
season_matches = df.groupby("season").size().sort_values(ascending=False)
print("Matches played in each season:")
print(season_matches.sort_index())
print("\nSeason with the highest number of matches:")
print(season_matches.head(1))
```
<img width="394" height="446" alt="image" src="https://github.com/user-attachments/assets/8ab97850-fc8e-42c5-bbb1-301b13e34c1f" />


## Q6. GroupBy & Sorting – Team Performance

**Calculate the number of wins for each team and display the Top 5 winning teams in descending order.**

```python
team_wins = df["winner"].value_counts()
print("Wins for each team:")
print(team_wins)
print("\nTop 5 winning teams:")
print(team_wins.head(5))
```

<img width="358" height="545" alt="image" src="https://github.com/user-attachments/assets/acbe26e2-77af-40dc-939a-bc6676ffb64e" />


## Q7. Filtering – Team Analysis

**Filter matches won by Chennai Super Kings and display the match details and total number of CSK wins.**

```python
csk_wins = df[df["winner"] == "Chennai Super Kings"]
display(csk_wins)
print("Total Chennai Super Kings wins:", len(csk_wins))
```

<img width="1164" height="550" alt="image" src="https://github.com/user-attachments/assets/8e955bf3-2e16-4116-8390-c256b5821ffd" />


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

<img width="370" height="106" alt="image" src="https://github.com/user-attachments/assets/e3df3567-9e1e-4985-ace5-9b8022e70ec2" />



## Q9. Cross-Tabulation – Season-wise Toss Analysis

**Perform a season-wise analysis of toss decisions and identify the most preferred toss decision in each season.**

```python
season_toss = pd.crosstab(df["season"], df["toss_decision"])
print("Season-wise toss decisions:")
display(season_toss)
print("\nMost preferred toss decision in each season:")
print(season_toss.idxmax(axis=1))
```

<img width="190" height="548" alt="image" src="https://github.com/user-attachments/assets/709fa932-e544-4c3b-8138-f33dd6019301" />
<img width="322" height="340" alt="image" src="https://github.com/user-attachments/assets/d5e01b5d-dc6f-41b8-8e29-51aa574961d5" />


## Q10. GroupBy & Sorting – Venue Analysis

**Identify the Top 5 IPL venues based on the number of matches played.**

```python
top_venues = df["venue"].value_counts().head(5)
print("Top 5 venues by matches played:")
print(top_venues)
```
<img width="392" height="135" alt="image" src="https://github.com/user-attachments/assets/4e8c63f8-7ab9-4d53-b44f-a7f562421cf7" />



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

<img width="1355" height="557" alt="image" src="https://github.com/user-attachments/assets/182c05ab-a88c-4bac-8271-d75f461224e3" />


## Q12. Value Counts – Match Result Analysis

**Find the frequency of each type of match result, such as runs, wickets, tie, and no result.**

```python
result_frequency = df["result"].fillna("no result").replace({
    "tie": "tie",
    "no result": "no result"
}).value_counts()
print(result_frequency)
```
<img width="1372" height="118" alt="image" src="https://github.com/user-attachments/assets/2e6d0eb9-a5c1-402c-98ae-df9d384a0334" />

## Q13. Data Transformation – Datetime Conversion

**Convert the `date` column to Datetime format and display its data type before and after conversion.**

```python
print("Data type before conversion:", df["date"].dtype)
df["date"] = pd.to_datetime(df["date"], errors="coerce")
print("Data type after conversion:", df["date"].dtype)
```
<img width="254" height="104" alt="image" src="https://github.com/user-attachments/assets/17d31fd5-ec50-40f6-b4cc-03def89c8c9c" />


## Q14. Data Transformation – Year Extraction

**Create a new `year` column by extracting the year from the `date` column.**

```python
df["year"] = df["date"].dt.year
display(df[["date", "year"]].head())
```

<img width="168" height="182" alt="image" src="https://github.com/user-attachments/assets/a5a2e875-b3f3-4a5e-9c80-769c88916f17" />


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

<img width="205" height="295" alt="image" src="https://github.com/user-attachments/assets/e3d91eb6-4b4c-4f46-b147-288999b2dd82" />

## Result

Exploratory Data Analysis was performed on the IPL matches dataset using Pandas. The questions cover dataset understanding, primary-key validation, data cleaning, season and team analysis, toss decisions, venues, winning margins, match results, and date-based transformations.
