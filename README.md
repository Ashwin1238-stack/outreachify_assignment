# Outreachy Assignment – Developer Automation Tasks

This repository contains the solutions for the Outreachy assignment.  
It demonstrates the use of **Make.com**, **APIs**, and **Python scripting** to automate workflows for data collection, reporting, and notifications.  

---

## 📌 Task 1: Automation in Make.com (Airtable → OpenAI → Google Docs → Slack)

### Goal:
- Generate a list of random contacts in **Airtable**.  
- Summarize the company using **OpenAI (ChatGPT)**.  
- Create a **Google Doc** with the summary.  
- Share the report in a **Slack channel** every day at 7:00 AM.  

### Implementation:
1. **Scheduler** in Make.com runs daily at 7:00 AM.  
2. **Random User API** generates Name + Email.  
3. **Airtable module** stores:  
   - Name  
   - Email  
   - Company  
4. **OpenAI module** creates a 5-sentence summary of the company.  
5. **Google Docs module** generates a document:  

6. **Slack module** posts the Google Doc link in `#daily-reports`.

---

## 📌 Task 2: YouTube Scraper (YouTube API + Google Sheets)

### Goal:
Extract video details from YouTube and store them in **Google Sheets**.  

### Data Captured:
- Video Title  
- Channel Name  
- Description  
- Publish Date  
- View Count  

### Implementation:
- **Option 1 (Make.com):**
  - Use the **YouTube Data API v3** + Google Sheets module.  

- **Option 2 (Python Script):**

```python
from googleapiclient.discovery import build
import pandas as pd

# Set up YouTube API
api_key = "YOUR_API_KEY"
youtube = build('youtube', 'v3', developerKey=api_key)

# Search query
request = youtube.search().list(q="technology", part="snippet", maxResults=5)
response = request.execute()

# Collect video details
videos = []
for item in response['items']:
    if item['id']['kind'] == 'youtube#video':
        video_id = item['id']['videoId']
        title = item['snippet']['title']
        channel = item['snippet']['channelTitle']
        description = item['snippet']['description']
        publish_date = item['snippet']['publishedAt']
        stats = youtube.videos().list(part="statistics", id=video_id).execute()
        views = stats['items'][0]['statistics']['viewCount']
        videos.append([title, channel, description, publish_date, views])

# Convert to DataFrame
df = pd.DataFrame(videos, columns=["Title", "Channel", "Description", "Publish Date", "Views"])
print(df)
Weekly Report:
Leads Contacted: 2345
Replied: 56
Meetings Booked: 4
Positive Replies: 12

# (Optional) Push df to Google Sheets using gspread or Make.com Google Sheets module

