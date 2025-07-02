# Exercise: Your First GitHub Actions Workflow
## 🧱 Step-by-Step Guide

This guide walks you through creating a basic GitHub Actions workflow. You’ll create the file manually, define a few triggers, and run a simple job that prints metadata and lists files. While it’s not a real build pipeline, it introduces the core mechanics: triggers, jobs, steps, and expressions.

---

## Objective

Set up a GitHub Actions workflow that:
1. Triggers on push (with path filtering), pull request, schedule, and manual run  
2. Prints event and branch metadata  
3. Lists the contents of the repository  
4. Uses expressions and filters effectively

---

## Step 1: Create a Repository

Start by creating a new repository using this template:

1. Go to the [Code tab](/../../) and click **Use this template**
2. Choose your personal GitHub account as the owner
3. Keep the repository **public** to avoid hitting GitHub Actions usage limits

Once created, move to the new repository.

---

## Step 2: Create the Workflow File

In the new repo:

1. Navigate to **Actions**
2. Click **New workflow**
3. Select **set up a workflow yourself**
4. Replace the default file name (`main.yml`) with `github-actions-demo.yml`
5. Remove the starter content — you’ll write the workflow from scratch

---

## Step 3: Add Workflow Name and Triggers

Add the following to define the name and trigger conditions:

- Trigger on:
  - Pushes to `main`, **excluding** changes in `.github/`
  - Pull requests targeting `main`
  - Scheduled runs every Sunday at 06:15 UTC
  - Manual workflow_dispatch trigger

<details>
<summary>Hint</summary>

```yaml
name: GitHub Actions Demo

on:
  push:
    branches: [ main ]
    paths-ignore: [ .github/** ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '15 6 * * 0'
  workflow_dispatch:
```
</details>

---

## Step 4: Define the Job

Declare a job named `Build`, running on the default Ubuntu runner (`ubuntu-latest`):

```yaml
jobs:
  Build:
    runs-on: ubuntu-latest
    steps:
      - run: |
          echo "Triggered by: ${{ github.event_name }}"
          echo "Branch: ${{ github.ref }}"
```

This prints basic metadata about the trigger and branch.

---

## Step 5: Check Out the Repository

To access files in the repo, use the `checkout` action:

```yaml
      - uses: actions/checkout@v3.3.0
```

Without this step, your job won’t have access to the working directory.

---

## Step 6: List Files in the Repo

Add a step to list repository contents:

```yaml
      - name: List files in the repository
        run: |
          echo "Contents of ${{ github.repository }}:"
          tree
```

If `tree` isn’t available in the runner image, replace with `ls -R` as a fallback.

---

## Step 7: Commit and Run the Workflow

1. Commit the `github-actions-demo.yml` file to the `.github/workflows/` directory
2. Trigger the workflow manually from the **Actions** tab
3. Confirm that it does **not** trigger on changes to `.github/**`

---

## Step 8: View Workflow Output

Go to the Actions tab and:

1. Click on the latest run of `GitHub Actions Demo`
2. Expand the `Build` job
3. Check the logs for:
   - Event and branch metadata
   - Repository file listing
   - Environment details under `Set up job`

---

## Step 9: Validate the Triggers

Confirm your workflow triggers behave as expected:

- ✅ Commit a change to `README.md`: should trigger (`push`)
- ✅ Add `[skip ci]` in a commit message: should **not** trigger
- ✅ Create a pull request targeting `main`: should trigger (`pull_request`)
- ✅ Wait for or manually trigger a scheduled run

---

## Summary

You created a basic GitHub Actions workflow with:
- Multiple trigger types (push, PR, cron, manual)
- Use of expressions and path filtering
- A simple job that prints trigger context and inspects the repo

Next: [Write Your First GitHub Action](02-My-first-action.md)  
This will show how to build and reuse custom logic inside GitHub Actions.