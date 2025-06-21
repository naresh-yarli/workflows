# Reddit Lead Generation Workflow - [Get it for $149](https://yarli.kit.com/products/reddit-leads-flow)

An automated n8n workflow that monitors multiple Reddit subreddits for potential leads, scores them using AI, and saves qualified leads to Google Sheets.

## Overview

This workflow:

- Runs automatically every hour
- Monitors multiple subreddits (n8n, automation, nocode, zapier, Make)-can change these as per need.
- Filters posts containing specific keywords
- Checks for duplicates to avoid reprocessing
- Uses AI to score and qualify leads
- Saves qualified leads to Google Sheets
- Sends alerts for hot leads (optional)

## Prerequisites

1. **n8n** - Self-hosted or cloud instance
2. **Reddit API Credentials** - Client ID and Secret
3. **Google Sheets** - For storing leads
4. **OpenAI API Key** - For lead scoring

## Setup Instructions

### 1. Reddit API Setup

1. Go to https://www.reddit.com/prefs/apps
2. Click "Create App" or "Create Another App"
3. Fill in:
   - Name: Your app name
   - App type: "script"
   - Redirect URI: http://localhost:8080
4. Save your:
   - Client ID (under "personal use script")
   - Client Secret

### 2. Google Sheets Setup

Create a new Google Sheet with these exact column headers in Row 1:

| A       | B    | C      | D     | E     | F    | G       | H      | I   | J         |
| ------- | ---- | ------ | ----- | ----- | ---- | ------- | ------ | --- | --------- |
| Post ID | Date | Author | Title | Score | Type | Urgency | Reason | URL | Subreddit |

### 3. Workflow Configuration

1. **Import the workflow** into n8n
2. **Add credentials**:

   - Reddit API (Basic Auth): Use Client ID as username, Client Secret as password
   - Google Sheets: Connect your Google account
   - OpenAI: Add your API key
   - Slack (optional): Connect for notifications

3. **Update node configurations**:
   - Update Google Sheets document ID in all sheet nodes
   - Modify subreddits and search terms in the "Config" node

## Workflow Components

### Nodes Description

1. **Every Hour** - Schedule trigger that runs hourly
2. **Get All Post IDs** - Loads existing posts from Google Sheets to prevent duplicates
3. **Get Reddit Token** - Authenticates with Reddit API
4. **Config** - Stores configuration (subreddits and search terms)
5. **Each Subreddit** - Splits subreddit list for individual processing
6. **Fetch Posts** - Gets latest 25 posts from each subreddit
7. **Split Posts** - Separates individual posts for processing
8. **Has Content** - Filters out posts without text content
9. **Has Keywords?** - Checks if posts contain search terms
10. **Is New Post?** - Verifies post hasn't been processed before
11. **Format Post** - Structures post data for storage
12. **Score Lead** - AI analysis to score leads (0-100)
13. **Qualified?** - Filters out "Not Qualified" leads
14. **Save Lead** - Stores qualified leads in Google Sheets
15. **Hot Lead?** - Identifies high-priority leads
16. **Slack Alert** - Sends notifications for hot leads (disabled by default)

### Search Keywords

The workflow searches for posts containing these terms(can be replaced if needed):

```
help, stuck, how to, integrate, API, automate, workflow, need, issue, problem,
error, failing, broken, doesn't work, not working, connect, setup, configure,
implement, solution, advice, recommend, suggestion, best way, tutorial, guide,
question, struggling, confused, difficulty, trouble, challenge, obstacle,
looking for, seeking, hire, freelance, consultant, expert, anyone know,
can someone, please help, urgent, ASAP, deadline, budget, paid, willing to pay,
fix, solve, debug, assist
```

### Lead Scoring

The AI scores leads based on:

- **Business need** (40 points max)
- **Technical fit** (30 points max)
- **Urgency** (30 points max)

Leads are classified as:

- **Hot Lead**: High-value opportunities requiring immediate attention
- **Warm Lead**: Qualified leads worth following up
- **Cold Lead**: Lower priority but still qualified
- **Not Qualified**: Filtered out, not saved

## Customization

### Modify Subreddits

Edit the "Config" node and update the `subreddits` value (comma-separated).

### Change Search Terms

Edit the "Config" node and update the `searchTerms` value (comma-separated).

### Adjust AI Scoring

Modify the prompt in the "Score Lead" node to change scoring criteria.

### Enable Notifications

Enable the "Slack Alert" node and configure your Slack webhook.

## Limitations

- Processes only the 25 most recent posts per subreddit
- Checks run hourly (can be adjusted in schedule trigger)
- Reddit API rate limits apply
- Requires active n8n instance to run

## Troubleshooting

### Common Issues

1. **Authentication Errors**

   - Verify Reddit Client ID and Secret
   - Ensure credentials are properly configured in n8n

2. **Google Sheets Errors**

   - Check document ID is correct
   - Verify sheet has proper column headers
   - Ensure Google account has edit permissions

3. **No Leads Found**
   - Check if search terms are too restrictive
   - Verify subreddits are active
   - Ensure posts contain text content (not just links)

### Data Flow

1. Schedule triggers workflow
2. Loads processed post IDs from Google Sheets
3. Authenticates with Reddit
4. Fetches posts from each configured subreddit
5. Filters posts by content and keywords
6. Checks against processed IDs
7. Scores new posts with AI
8. Saves qualified leads to Google Sheets
9. Optionally sends hot lead alerts

## Support

- For professional support and custom automation solutions: info@yarli.com.au

For issues with:

- n8n platform: https://community.n8n.io/
- Reddit API: https://www.reddit.com/dev/api
- OpenAI API: https://platform.openai.com/docs
