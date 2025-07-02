---
marp: true
backgroundImage: url('themes/background.png')
style: |
    /* Nimbus Intelligence Marpit Theme */

    @font-face {
    font-family: 'New Order';
    src: url('New Order SemiBold.otf') format('opentype');
    font-weight: 600; /* SemiBold weight */
    font-style: normal;
    }

    /* Base slide styling */
    section {
    background-color: #1A1A1A; /* Dark background matching Nimbus Intelligence */
    color: #FFFFFF; /* White text for readability */
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; /* Modern sans-serif font stack */
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 40px;
    height: 100%;
    width: 100%;
    box-sizing: border-box;
    }

    /* Centered titles */
    h1, h2, h3, h4, h5, h6 {
    text-align: center;
    color: #f9945d; /* Teal accent color from Nimbus Intelligence */
    margin: 0.5em 0;
    font-weight: 700;
    font-family: 'New Order', 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }

    /* Specific heading sizes */
    h1 {
    font-size: 4.5em;
    }

    h2 {
    font-size: 3em;
    }

    h3 {
    font-size: 1.5em;
    }

    /* Paragraphs and lists */
    p, ul, ol {
    color: #D1D5DB; /* Light gray for secondary text */
    font-size: 1.2em;
    line-height: 1.6;
    max-width: 80%;
    text-align: left;
    }

    /* Lists */
    ul, ol {
    padding-left: 0;
    list-style-position: inside;
    }

    li {
    margin: 0.5em 0;
    }

    /* Code blocks */
    pre, code {
    background-color: #2D2D2D; /* Slightly lighter dark background for code */
    color: #f9945d; /* Teal for code text */
    border-radius: 5px;
    padding: 10px;
    font-family: 'Fira Code', monospace; /* Monospace font for code */
    }

    /* Inline code */
    code {
    padding: 2px 5px;
    }

    /* Links */
    a {
    color: #f9945d; /* Teal for links */
    text-decoration: none;
    }

    a:hover {
    text-decoration: underline;
    }

    /* Blockquotes */
    blockquote {
    border-left: 4px solid #f9945d;
    padding-left: 1em;
    color: #D1D5DB;
    font-style: italic;
    }

    /* Footer and header (for Marpit directives) */
    header, footer {
    color: #D1D5DB;
    font-size: 0.9em;
    position: absolute;
    width: 100%;
    text-align: center;
    }

    header {
    top: 10px;
    }

    footer {
    bottom: 10px;
    }

    /* Ensure slide content doesn't overlap header/footer */
    section:not(.title) {
    padding-top: 60px;
    padding-bottom: 60px;
    }

    /* Title slide specific styling */
    section.title {
    background: linear-gradient(135deg, #1A1A1A 60%, #f9945d 100%); /* Gradient for title slide */
    display: flex;
    justify-content: center;
    align-items: center;
    }

    section.title h1 {
    font-size: 3em;
    color: #FFFFFF;
    }

    section.title h2 {
    color: #D1D5DB;
    }

    /* Responsive adjustments */
    @media screen and (max-width: 768px) {
    section {
        padding: 20px;
    }

    h1 {
        font-size: 2.5em;
    }

    h2 {
        font-size: 2em;
    }

    p, ul, ol {
        font-size: 1em;
        max-width: 90%;
    }
    }    
---

<!-- footer: ![w:150](https://nimbusintelligence.com/wp-content/uploads/2024/01/logo-white-red.png) -->

# GitHub Actions
Juan Manuel Perafan

---

# What are GitHub Actions?

- Automation tool integrated into GitHub
- Enables CI/CD (Continuous Integration/Continuous Deployment)
- Automates workflows based on events (e.g., push, pull request)

---
![bg fit left](https://blog.secureflag.com/assets/images/code-review.jpg)

- Data pipeline execution
- Data quality checks
- Model training and deployment
- Data Ingestion
- Cleaning stale assets
- Update documentation

---

![bg fit center](https://graphite.dev/images/content/guides/github_actions_code_automation_explained/githubActionsComponents.png)

---

# Core Components

- **Workflow**: A set of automated tasks - **defined in YAML
- **Event**: Triggers the workflow (e.g., - **push, schedule, manual)
- **Job**: A set of steps running on a - **single runner
- **Step**: An individual task (e.g., run a - **script, install dependencies)
- **Action**: Reusable step or script (e.- **g., checkout code, run tests)
- **Runner**: Virtual machine/container executing the job

---

# Getting Started

- Create `.github/workflows/` directory - in your repo
- Add YAML file (e.g., data-pipeline.- yml)
- Define triggers, jobs, and steps
- Use existing actions (e.g., `actions/setup-python`)
- Test and monitor via GitHub’s Actions