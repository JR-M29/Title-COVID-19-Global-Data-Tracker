# Title-COVID-19-Global-Data-Tracker


```
import pandas as pd

Load the dataset
df = pd.read_csv('owid-covid-data.csv')

Check columns
print(df.columns)

Preview rows
print(df.head())

Identify missing values
print(df.isnull().sum())
```


```
Filter countries of interest
countries = ['Kenya', 'USA', 'India']
df_filtered = df[df['location'].isin(countries)]

Drop rows with missing dates/critical values
df_filtered.dropna(subset=['date', 'total_cases', 'total_deaths'], inplace=True)

Convert date column to datetime
df_filtered['date'] = pd.to_datetime(df_filtered['date'])

Handle missing numeric values
df_filtered['total_vaccinations'].fillna(0, inplace=True)
```


```
import matplotlib.pyplot as plt
import seaborn as sns

Plot total cases over time for selected countries
plt.figure(figsize=(10,6))
sns.lineplot(x='date', y='total_cases', hue='location', data=df_filtered)
plt.title('Total Cases Over Time')
plt.xlabel('Date')
plt.ylabel('Total Cases')
plt.show()

Plot total deaths over time
plt.figure(figsize=(10,6))
sns.lineplot(x='date', y='total_deaths', hue='location', data=df_filtered)
plt.title('Total Deaths Over Time')
plt.xlabel('Date')
plt.ylabel('Total Deaths')
plt.show()

Compare daily new cases between countries
plt.figure(figsize=(10,6))
sns.lineplot(x='date', y='new_cases', hue='location', data=df_filtered)
plt.title('Daily New Cases')
plt.xlabel('Date')
plt.ylabel('New Cases')
plt.show()

Calculate the death rate
df_filtered['death_rate'] = df_filtered['total_deaths'] / df_filtered['total_cases']
print(df_filtered['death_rate'].describe())


```
Plot cumulative vaccinations over time for selected countries
plt.figure(figsize=(10,6))
sns.lineplot(x='date', y='total_vaccinations', hue='location', data=df_filtered)
plt.title('Cumulative Vaccinations Over Time')
plt.xlabel('Date')
plt.ylabel('Total Vaccinations')
plt.show()

Compare % vaccinated population
df_filtered['vaccinated_percentage'] = df_filtered['total_vaccinations'] / df_filtered['population']
plt.figure(figsize=(10,6))
sns.barplot(x='location', y='vaccinated_percentage', data=df_filtered)
plt.title('Vaccinated Percentage by Country')
plt.xlabel('Country')
plt.ylabel('Vaccinated Percentage')
plt.show()
```


```
import plotly.express as px

Prepare a dataframe with iso_code, total_cases for the latest date
latest_date = df_filtered['date'].max()
df_latest = df_filtered[df_filtered['date'] == latest_date]

Plot a choropleth showing case density
fig = px.choropleth(df_latest, locations='iso_code', color='total_cases', hover_name='location', title='Total Cases by Country')
fig.show()
