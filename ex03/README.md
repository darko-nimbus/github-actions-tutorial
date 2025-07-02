# Exercise 3: CI using dbt
## 🧱 Step-by-Step Guide

This guide walks you through creating a GitHub Actions workflow that sets up a Python environment, installs dbt dependencies, configures dbt, and runs `dbt compile` and `dbt build` commands.

---

## Objective

Create a GitHub Actions workflow that:
1. Triggers on manual runs (`workflow_dispatch`) and on pull requests targeting the `main` branch  
2. Checks out the repository  
3. Sets up Python 3.12  
4. Installs dbt dependencies from a requirements file  
5. Copies the dbt profile configuration  
6. Runs `dbt compile`  
7. Runs `dbt build` with fail-fast behavior  

---

## Step 1: Create the Workflow File

Create a file `.github/workflows/dbt_CI.yml` in your repository. The name is arbitrary but it must reside in `.github/workflows/` to be recognized.

---

## Step 2: Define the Workflow Name and Triggers

Define the workflow name and triggers to run on:
- Manual trigger via workflow_dispatch  
- Pull requests targeting the `main` branch

<details>
<summary>Hint</summary>

```yaml
name: dbt_CI

on:
  workflow_dispatch:
  pull_request:
    branches:
      - main
```
</details>

# Step 3: Define the Job and Runner

Create a job named CI_job that runs on the Ubuntu latest virtual environment:

```yaml
jobs:
  CI_job:
    runs-on: ubuntu-latest
```

# Step 4: Check Out the Repository
Add a step to checkout your repo content and setup Python. You should be familiar with this by now.

```yaml
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Set up Python
      uses: actions/setup-python@v5
      with:
        python-version: '3.12'
```
> Starting from this point, you code might fail if it doesn't find a dbt repository in the current directory. Don't forget to use exact references or simply to cd into the right directory before every step.

# Step 5: Install Dependencies
In the `ex03` directory, there is a file called `dbt-requirements.txt`. It contains every package you need to install the duckDB dbt-adapter. Use pip to install the contents of this file (don't forget the recursive flag).

# Step 6: Set Up dbt Profile
To run, dbt needs a file called `profiles.yml` with details of the connect. Also, dbt needs it to be placed in a directory called `.dbt` in the home directory. Since we are using duckDB and there is no confidential data, we are doing a quick and dirty:
1. Push the `profiles.yml` file to your repo
2. Create a directory called `.dbt` in the home directory of your runner
3. Copy the file from your repo into the `.dbt` directory

For production use cases, you really should consider using secrets. 

# Step 7: Run dbt Compile
It is a good idea to run `dbt compile` before you even run or build your models. The reason is that compile fails a lot faster and cheaper. Consider this step optional.

# Step 8: Run dbt Build
Now, it is time to test that the code actually works! Ask your runner to do a dbt-build. I am using the `--fail-fast` flag to make it even more strict. 
