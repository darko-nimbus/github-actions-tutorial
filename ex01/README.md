[text](../.github)# Exercise 1: Scheduled Data Scraping
## 🧱 Step-by-Step Guide

This guide walks through building a simple GitHub Actions workflow that fetches author data from the Open Library API on a schedule. While GitHub Actions isn't typically used for full data pipelines, it's perfectly reasonable for small-scale tasks like polling an API or running a lightweight scraper. Plus, GitHub's free tier is sufficient for these types of workflows.

## Objective
Create a GitHub Actions workflow that:
1. Runs once per day using schedule trigger (cron)
2. Store the name of your favorite author as a GitHub secret
3. Fetches data from the OpenLibrary Author Search API
4. Saves the API response to a file in the repository
5. Commit and push the file to the repository

---

## Step 1: Create the Workflow File

In your repository, create the file `.github/workflows/ex01.yml`. The filename is arbitrary, but the directory name is important. GitHub will pick up any YAML file placed inside `.github/workflows/`.

---

## Step 2: Define the Workflow Name and Triggers

Add a block to the top of the file that declares:
- An arbitrary name for your workflow such as `ex01`
- A `workflow_dispatch` trigger, allowing manual runs via the UI
- A daily schedule using cron syntax

> There are many other possible triggers (e.g., `push`, `pull_request`, `release`). For this exercise, we ignore GitHub events and use only manual and scheduled triggers.

<details>
<summary>Hints</summary>

```yaml
name: ex01

on:
  workflow_dispatch:
  schedule:
    - cron: '0 8 * * *' # every day at 08:00 UTC
```
</details>

---

## Step 3: Define the Job and Runner

Next, declare a job and specify which OS image it runs on:

```yaml
jobs:
  author_info:
    runs-on: ubuntu-latest
```

- The job is named `author_info`.
- It runs on `ubuntu-latest`, which provides common CLI tools like `curl`, `git`, and `bash` by default.
- You can also run jobs on `windows-latest` or `macos-latest`, but these are slower and consume more credits.

---

## Step 4: Access the Repository

To work with files in your repo, use the official `checkout` action:

```yaml
    steps:
    - name: Check out this repo
      uses: actions/checkout@v4
```

Without this step, the workflow would run in a clean workspace with no access to your code or directories.

---

## Step 5: Create and Use a Secret

You’ll need a secret for the author's name. This avoids hardcoding and lets you change the input without editing the workflow.

1. Go to **Settings > Secrets and variables > Actions**.
2. Add a secret named `AUTHOR_NAME` with the name of your chosen author.

In your workflow, reference this secret via `${{ secrets.AUTHOR_NAME }}`.

> Conventionally, secret names are written in uppercase with underscores.

---

## Step 6: Fetch Author Data

Add a step to fetch author metadata from Open Library. In the same step:
- Access your GitHub Secret and store it as an environmental variable
- Replace spaces with `+` for basic URL encoding.
- Call the Open Library API using `curl -s` (silent mode)
- Writes the output to a JSON file in the `ex01/` directory using the pipe operator `>`
- Give your files a dynamic name. Something like `ex01/$(date +%F)-${AUTHOR}.json`

<details>
<summary>Hints</summary>

```yaml
    - name: Fetch author name from secrets
      env:
        AUTHOR_NAME: ${{ secrets.AUTHOR_NAME }}
      run: |-
        AUTHOR="${AUTHOR_NAME}"
        AUTHOR_ENCODED=$(echo "$AUTHOR" | sed 's/ /+/g')
        curl -s "https://openlibrary.org/search/authors.json?q=${AUTHOR_ENCODED}" > "ex01/$(date +%F)-${AUTHOR}.json"
```
</details>

---

## Step 7: Commit and Push Changes

Finally, commit the new file if it differs from what's already in the repo:
- Create a step that does `git add ex01/*.json`, `git commit`, and `git push`
- Add extra logic to only commit if there are new files added (no need to commit if the rest of the workflow failed)

Optional:
- Find a way to push code to another branch, instead of main directly

Please note, GitHub shouldn't impersonate you when it commits or pushes code. Use the following commands to create a new user:
- `git config --local user.email "action@github.com"`
- `git config --local user.name "GitHub Action"`

<details>
<summary>Hints</summary>

```yaml
    - name: Commit and push changes
      run: |-
        git config --local user.email "action@github.com"
        git config --local user.name "GitHub Action"
        git add ex01/*.json
        git diff --staged --quiet || git commit -m "Update author data - $(date +%F)"
        git push
```
</details>

---

## Optional: Ingest Data to the Cloud
Git repositories are not optimized for storing data. Adding large or growing numbers of files (e.g., JSON snapshots) will eventually slow down operations like clone, fetch, and pull. For anything beyond small-scale testing, it's better to push your output to object storage like Amazon S3 or Google Cloud Storage.

This step assumes you already have a cloud account configured and the appropriate credentials available to the GitHub Actions runner (via secrets).

Here’s a minimal example using Google Cloud:
```yaml
    - id: 'auth'
      name: 'Authenticate to Google Cloud'
      uses: 'google-github-actions/auth@v2'
      with:
        credentials_json: '${{ secrets.GCP_CREDENTIALS }}'

    - id: 'upload-file'
      uses: 'google-github-actions/upload-cloud-storage@v2'
      with:
        path: '/path/to/file'
        destination: '${{ secrets.GCP_BUCKET }}'
```