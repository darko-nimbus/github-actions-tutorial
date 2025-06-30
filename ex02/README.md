# Exercise 2: Automated Testing with Pytest
## 🧱 Step-by-Step Guide

This guide walks you through creating a GitHub Actions workflow from scratch to run Pytest on a Python script and prevent code from being pushed or merged if tests fail. We'll build the workflow, explain the nuances of setting up Python, and intentionally introduce a failing test to observe the behavior.

## Objective
Create a GitHub Actions workflow that:
- Triggers on push or pull request to the main branch
- Sets up a Python environment
- Installs Pytest and runs tests on a Python script
- Fails the workflow if any tests fail, blocking merges
- Includes a failing test to demonstrate the failure behavior

---

## Step 1: Inspect Files
Examine the Python script in `ex02/src`. It contains two functions and is straightforward. Next, review the contents of the `ex02/tests` folder.

If you're unfamiliar with Pytest, it's a popular framework for testing Python code. It's easy to learn and highly scalable, making it ideal for serious Python development.

We won't go too deep in this tutorial, but if you want to get familiar with this library, check out this [Pytest tutorial](https://calmcode.io/course/pytest/introduction).

---

## Step 2: Create the Workflow File
In your repository, create the file `.github/workflows/pytest.yml`.

---

## Step 3: Define the Workflow Name and Triggers
Add a block at the top of pytest.yml to:
- Name the workflow (e.g., "Run Pytest")
- Trigger on `push` and `pull_request` events to the main branch

> Tip: Include a workflow_dispatch trigger to allow manual testing of the workflow.

--- 

## Step 4: Define Jobs and Runner
You should be familiar with this step. Configure the workflow to run on ubuntu-latest and use actions/checkout@v4 to access your repository's contents.

---

## Step 5: Set Up Python
Add a step to set up the Python environment:

```yml
  - name: Set up Python
    uses: actions/setup-python@v5
    with:
      python-version: '3.11'
```

The `actions/setup-python@v5` action installs Python and caches dependencies for faster execution. Specifying `3.11` ensures consistency (newer versions that might break compatibility with Pytest or other dependencies).

---

## Step 6: Install Dependencies
Add a step to install Pytest:
```yml
  - name: Install dependencies
    run: |
      python -m pip install --upgrade pip
      pip install pytest
```

Note: Upgrading pip prevents potential dependency issues. Installing pytest enables test execution. The | allows multi-line commands in YAML for clarity.

---

## Step 7: Run Pytest
Add a step to run the tests on your tests file, every time the workflows triggers. 

```yml
    - name: Run tests
      run: |
        pytest tests/test_calculator.py --verbose
```
---

## Step 8: Add a Failing Test
Your workflow should now be complete, and if defined correctly, all tests in the Pytest file should pass. To explore how the workflow handles failures, introduce a bug in the test.

Edit src/calculator.py to ensure a test fails. For example, change `return a + b` to `return 0` in the relevant function.
After modifying the file, commit and push to the main branch.

---

## Step 9: Set Up Branch Protection Rules
You may notice that, despite the workflow failing, the code was still pushed. This is undesirable. 

To prevent pushes or merges to main when tests fail, configure a branch protection rule:
- Navigate to `Settings > Branches` in your GitHub repository.
- Under Branch protection rules, click Add rule or edit the existing rule for main.
- Configure the following:
    - Branch name pattern: main
    - Require a pull request before merging: Enable this to block direct pushes to main.
    - Require status checks to pass before merging: Enable this and select the test job (it appears after the workflow runs once).
- Require branches to be up to date before merging: Enable this to ensure the latest tests run.

After saving the rule, revert `src/calculator.py` to its last working version, then introduce the bug again and attempt to push to main. If configured correctly, the push should be blocked due to the failing tests.