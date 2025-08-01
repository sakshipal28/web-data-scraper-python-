# web-data-scraper-python-
A simple python scraper that extracts headlines from  Hindustan Times.

import requests
from bs4 import BeautifulSoup
import pandas as pd

# URL of the website to scrape
url = "https://www.hindustantimes.com/latest-news"

# Send HTTP request to the URL
response = requests.get(url)
if response.status_code != 200:
    print("Failed to retrieve the page")
    exit()

# Parse the content with BeautifulSoup
soup = BeautifulSoup(response.text, "html.parser")

# Find all headlines
headlines = soup.find_all("h3", class_="hdg3")  # Adjust class name if needed

# Store data in a list
news_data = []

for i, headline in enumerate(headlines):
    title = headline.text.strip()
    link = headline.find("a")["href"]
    full_link = "https://www.hindustantimes.com" + link
    news_data.append({"No.": i+1, "Headline": title, "Link": full_link})

# Display on console
for item in news_data:
    print(f"{item['No.']}. {item['Headline']}")
    print(f"Link: {item['Link']}\n")

# Save to CSV
df = pd.DataFrame(news_data)
df.to_csv("news_headlines.csv", index=False, encoding='utf-8-sig')

print("✅ Headlines saved to news_headlines.csv")

