# Lok_Sabha_Election_2024_Insights
Election Results Analysis and Visualization
This project fetches, processes, and visualizes election results data from the Election Commission of India's website. It extracts data from HTML tables, processes it into a pandas DataFrame, and generates various plots to visualize the election results.

Table of Contents
Installation
Usage
Code Overview
Generating Insights
Plotting
Sample Data
License

Installation
1)Clone the repository:
git clone https://github.com/your-repo/election-results-analysis.git
cd election-results-analysis
2)Install the required packages:
pip install -r requirements.txt

Usage
Run the script using the command below. It will fetch data from the specified URL, process it, and generate plots.
python main.py

Code Overview
The project is divided into several sections, each responsible for a specific task:

1)Fetching and Parsing Data:
The script fetches the HTML content from the Election Commission's website and parses it using BeautifulSoup.

import requests
from bs4 import BeautifulSoup

url = 'https://results.eci.gov.in/PcResultGenJune2024/index.htm'
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')


2)Identifying and Extracting the Election Results Table:
The script identifies the correct table containing election results and extracts the data into a list of headers and rows.


def is_election_table(table):
    headers = [th.text.strip() for th in table.find_all('th')]
    return 'Party' in headers and 'Won' in headers and 'Leading' in headers

# Extract data rows
rows = []
for tr in election_table.find_all('tr')[1:]:  # Skip header row
    row = [td.text.strip() for td in tr.find_all('td')]
    if row:  # Ignore empty rows
        rows.append(row)

3)Creating a DataFrame:
The extracted data is converted into a pandas DataFrame for further processing and visualization.


import pandas as pd

df = pd.DataFrame(rows, columns=headers)
df['Won'] = pd.to_numeric(df['Won'])
df['Leading'] = pd.to_numeric(df['Leading'])
df['Total'] = pd.to_numeric(df['Total'])


Generating Insights
The script generates various insights based on the processed data, such as the winning party, percentage of seats, and more.

total_seats = df['Total'].sum()

# Calculate percentages
df['Percentage'] = (df['Total'] / total_seats) * 100

# Generate insights
insights = []
insights.append('1. The winning party is ' + str(df_sorted.iloc[0]['Party']) + ' with ' + str(df_sorted.iloc[0]['Total']) + ' seats.')
insights.append('2. The winning party secured {:.2f}% of the total seats.'.format(df_sorted.iloc[0]['Percentage']))

Plotting
The script uses matplotlib and seaborn to generate various plots for data visualization.

1)Bar Chart:
plt.figure(figsize=(12, 6))
plt.bar(df_sorted['Party'], df_sorted['Won'])
plt.title('Top Parties by Seats Won')
plt.xlabel('Political Parties')
plt.ylabel('Number of Seats Won')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()


2)Pie Chart:
plt.figure(figsize=(10, 8))
plt.pie(df_sorted['Total'], labels=df_sorted['Party'], autopct='%1.1f%%', startangle=90)
plt.title('Distribution of Seats Among Major Parties')
plt.axis('equal')
plt.tight_layout()
plt.show()


3)Lollipop Chart:
sns.pointplot(x='Seats', y='Party', data=df, join=False, color='darkblue')
plt.hlines(y=df.Party, xmin=0, xmax=df.Seats, color='skyblue', alpha=0.8, linewidth=3)


4)Scatter Plot:
sns.scatterplot(x='Vote Share (%)', y='Seats', data=df, s=200, color='darkblue')
for i, row in df.iterrows():
    plt.annotate(row['Party'], (row['Vote Share (%)'], row['Seats']), xytext=(5, 5), textcoords='offset points')


Sample Data
Sample data used in the script:
data = [
    ['Bharatiya Janata Party - BJP', '240', '0', '240'],
    ['Indian National Congress - INC', '99', '0', '99'],
    ['Samajwadi Party - SP', '37', '0', '37'],
    ['All India Trinamool Congress - AITC', '29', '0', '29'],
    ['Dravida Munnetra Kazhagam - DMK', '22', '0', '22']
]






