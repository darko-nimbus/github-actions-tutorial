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

## Optional: Set Up Branch Protection Rules
When a workflow fails, you might notice that the code still gets pushed to your repository. This creates a problem—broken code ends up in your main branch despite failing tests.

To prevent this, you need to set up a branch protection rule that blocks pushes and merges when tests fail.

> Note: You need admin access to create branch protection rules. If you want to follow along, try this in a repository you own.

Setting up the protection rule:
- Navigate to `Settings > Branches > Rules > Rulesets` in your GitHub repository, then click `New ruleset > New branch ruleset`.

- Configure these settings:
  - Ruleset Name: pytest
  - Enforcement status: Active
  - Target branches: Add target > Include default branch

- Under Rules, check `Require status checks to pass`, then:
  - Add checks: Run Pytest
  - Any source: GitHub Actions
  - Click Create

> Important: "Run Pytest" is just an example name—you can use any name as long as it matches your workflow's job name.

Once you've saved the rule, revert `src/calculator.py` to its last working version, then reintroduce the bug and try pushing to main. If everything is configured correctly, the push will be blocked because the tests are failing.

This ensures that only code that passes all tests can make it into your main branch, keeping your codebase stable and reliable.