# Exercise 1: Scheduled Data Scraping

Automate scheduled data collection using GitHub Actions.

## Objective
Create a GitHub Actions workflow that:
1. Runs once per day using schedule trigger (cron)
2. Store the name of your favorite author as a GitHub secret
3. Fetches data from the OpenLibrary Author Search API
4. Saves the API response to a file in the repository
5. Commit and push the file to the repository

## Optional
If you want a harder version:

- Instead of saving the data in the repo, upload it to a storage bucket (e.g., AWS S3)

- You’ll need to configure secrets for credentials and use the appropriate CLI or SDK