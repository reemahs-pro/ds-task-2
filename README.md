import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("covid_data.csv")  # Make sure to match your file name

df.columns = ["Date", "Country", "Confirmed", "Deaths", "Recovered"]
df.dropna(subset=["Confirmed", "Deaths"], inplace=True)

print("Top 5 rows:")
print(df.head())

print("\nSummary statistics:")
print(df.describe())

print("\nNumber of countries in data:", df["Country"].nunique())

print("\nTop 10 Most Affected Countries (by Confirmed Cases):")
print(df.groupby("Country")["Confirmed"].max().sort_values(ascending=False).head(10))

# Line chart - Global confirmed cases over time
world_data = df.groupby("Date")["Confirmed"].sum().reset_index()
plt.figure(figsize=(12, 6))
sns.lineplot(x="Date", y="Confirmed", data=world_data)
plt.xticks(rotation=45)
plt.title("Global Confirmed COVID-19 Cases Over Time")
plt.tight_layout()
plt.show()

# Bar chart - Top 10 countries by deaths
latest = df[df["Date"] == df["Date"].max()]
top_deaths = latest.groupby("Country")["Deaths"].max().sort_values(ascending=False).head(10)
plt.figure(figsize=(10, 6))
sns.barplot(x=top_deaths.values, y=top_deaths.index, palette="Reds_d")
plt.title("Top 10 Countries by COVID-19 Deaths")
plt.xlabel("Deaths")
plt.tight_layout()
plt.show()

# Pie chart - Global recovered vs deaths
recovered = latest["Recovered"].sum()
deaths = latest["Deaths"].sum()
plt.figure(figsize=(6, 6))
plt.pie([recovered, deaths], labels=["Recovered", "Deaths"], autopct="%1.1f%%", colors=["green", "red"])
plt.title("Global Recovered vs Deaths")
plt.show()
# ds-task-2
