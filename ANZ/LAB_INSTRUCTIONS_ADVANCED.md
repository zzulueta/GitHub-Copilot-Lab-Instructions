# GitHub Copilot Advanced Lab Instructions
**Duration:** 2.5 hours  
**Level:** Advanced

---

## Pre-Lab Setup (15 minutes before lab)

### 1. Prerequisites
- **GitHub CLI (`gh`)** installed - [Download from github.com/cli/cli](https://github.com/cli/cli#installation)
- **Git configured** and repository pushed to GitHub

### 2. Verify Setup

Check that the steel inventory API is working:
```bash
cd steel-inventory-api
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```
Visit http://localhost:8000/docs - you should see the API documentation.

Install and verify Copilot CLI:
```bash
# Install GitHub Copilot CLI extension
gh extension install github/gh-copilot

# Verify installation
copilot --version
```

**Note:** If you already have it installed, update it with:
```bash
copilot update
```

---

## Lab Overview

In this advanced lab, you'll extend GitHub Copilot's capabilities by integrating external tools via MCP, delegate work to Copilot cloud agent for automated feature implementation, master terminal-native workflows with Copilot CLI, and implement enterprise governance controls.

**Lab Flow:** You'll use GitHub MCP to research similar projects and create a "Low Inventory Check" feature issue, immediately assign it to Copilot cloud agent for implementation, then learn CLI as a powerful terminal-native alternative for DevOps and automation workflows.

### Learning Objectives Checklist
- [ ] Install and configure MCP servers (GitHub, Playwright)
- [ ] Use GitHub MCP to search repositories and create issues
- [ ] Use Playwright MCP to automate UI testing
- [ ] Assign GitHub issues to Copilot cloud agent
- [ ] Review and interact with cloud agent PRs
- [ ] Merge and test cloud agent implementations
- [ ] Understand when to use CLI vs Chat interfaces
- [ ] Master CLI for terminal-native git workflows
- [ ] Use CLI plan mode for DevOps and automation scenarios
- [ ] Configure enterprise governance policies

---

## Part 1: Model Context Protocol (MCP) Integration (40 minutes)

MCP (Model Context Protocol) allows GitHub Copilot to integrate with external tools and services. In this section, you'll install and use MCP servers to extend Copilot's capabilities, search for similar projects, and create a comprehensive feature issue.

### Exercise 1.1: GitHub MCP Server - Repository Search and Issue Creation (20 min)

The GitHub MCP server provides tools for interacting with GitHub.com directly from Copilot. You'll use it to research similar projects and create a comprehensive feature issue.

#### Step 1: Install GitHub MCP Server

1. Open **Copilot Chat** in VS Code

2. Click the **Cog icon** (Open Customizations) in the chat toolbar

3. Select **MCP Servers** tab then select **Browse Marketplace**

4. Type **"GitHub"** in the search bar

5. Select **GitHub** and click **Install**. Select Back to overview

6. Right-click the GitHub MCP server and select **Start Server**

7. You will be prompted to authenticate with Github. 
   - Select Allow to grant permissions to the MCP server
   - You will be redirected to GitHub.com to authorize Visual Studio code. Select **Continue** and complete the authentication flow.

8. After successful authentication in the browser, return to VS Code and you should see the GitHub MCP server status as "Running" in the MCP Servers tab.

9. You can optionally see the MCP json configuration:
   - Click Extensions → MCP Servers - INSTALLED → GitHub
   - Select Manage → Show Configuration (JSON)

10. You should see a configuration like this with a Running status:
```json
"io.github.github/github-mcp-server": {
			"type": "http",
			"url": "https://api.githubcopilot.com/mcp/",
			"gallery": "https://api.mcp.github.com",
			"version": "1.0.4"
		}
```

#### Step 2: Search Similar Repositories

1. In Copilot Chat, enable Agent Mode.

   Select Configure Tools and verify that the GitHub MCP server is enabled.

   Then, ask Copilot to search for similar repositories:
   ```
   Use the GitHub MCP server to search for repositories related to "steel inventory management" or "warehouse inventory system". Find the top 5 repositories with the most stars.
   ```

2. Analyze the top repositories:
   ```
   For the top 3 repositories you found, use GitHub MCP to read their README files and identify key features that our steel inventory API doesn't have yet.
   ```

3. Extract capabilities:
   ```
   Based on the repositories analyzed, list 5-10 missing capabilities that would be valuable for our steel inventory system. For each, briefly explain why it's useful.
   ```

#### Step 3: Create Issues for Missing Capabilities

1. Ask Copilot to create issues for interesting features:
   ```
   Use GitHub MCP to create well-documented issues for the top 3 most valuable missing capabilities. Each issue should include:
   - Clear descriptive title
   - Description of the feature
   - Why it's valuable (business benefit)
   - Reference to the repository where you found it
   - Basic acceptance criteria
   ```
   > Note: You would be requested to Allow the creation of the issues on GitHub.com. Select Allow in this Session for each issue.

2. Verify the issues were created on GitHub.com

3. Open one of the issues and review the content generated by Copilot.

#### Step 4: Define and Create Low Inventory Check Issue

1. Define the feature requirements for the "Low Inventory Check" system.

   Select Plan Mode in Copilot Chat to structure the requirements for this feature.

   Then, provide the following prompt:
   ```
   We need a "Low Inventory Check" feature for our steel inventory system. Help me draft comprehensive requirements for:
   
   Front-end:
   - Visual indicator (red badge/icon) on products below threshold
   - Filter button to show only low-stock items
   - Dashboard widget displaying count of low-stock items
   
   Back-end:
   - inventory_threshold field in SteelProduct model (integer)
   - GET /api/inventory/low-stock endpoint returning products below threshold
   - Configurable global default threshold setting
   - Tests for low inventory logic
   
   Generate a detailed issue description with all requirements, acceptance criteria, and technical specifications.
   ```

2. Create the comprehensive issue.

   Select Agent Mode and ask Copilot to create the issue on GitHub.com:
   ```
   Use GitHub MCP to create an issue titled "Implement Low Inventory Check System" with the detailed requirements you just generated. Make sure it's well-formatted and includes all the specifications.
   ```

3. Verify the issue on GitHub.com - this is the issue we'll assign to Copilot cloud agent in Part 2!

**Expected Outcome:**
- GitHub MCP server installed and authenticated
- 3-4 issues created for missing capabilities from similar repos
- Comprehensive "Low Inventory Check" issue created and ready for Part 2
- Understand how MCP bridges Copilot with external services

---

### Exercise 1.2: Playwright MCP Server for UI Testing (20 min)

**Task:** Install Playwright MCP server and create automated UI tests for the steel inventory web application.

#### Step 1: Install Playwright MCP Server

1. Open **Copilot Chat** in VS Code

2. Click the **Cog icon** (Open Customizations) in the chat toolbar

3. Select **MCP Servers** tab then select **Browse Marketplace**

4. Type **"Playwright"** in the search bar

5. Select **Playwright** and click **Install**. Select Back to overview

6. Right-click the Playwright MCP server and select **Start Server**


#### Step 2: Verify Application is Running

Ensure your steel inventory API is running:
```bash
cd steel-inventory-api
uvicorn app.main:app --reload
```

Visit http://localhost:8000 to confirm the web UI loads.

#### Step 3: Basic UI Smoke Tests

1. Test homepage load.

   Select Agent Mode and ask Playwright MCP server to automate a browser test:
   ```
   Use Playwright MCP server to:
   1. Navigate to http://localhost:8000
   2. Verify the page title contains "Steel Inventory"
   3. Take a screenshot and save it as "homepage.png"
   ```
   > Note: You may be prompted to install chromium. In addition you will be prompted for approvals. Select Allow in this session for each request.

   Verify the screenshot is saved in your project folder.

2. Test product list display:
   ```
   Use Playwright MCP to:
   1. Navigate to http://localhost:8000
   2. Wait for products to load (wait for elements with class "product-card")
   3. Count how many product cards are displayed
   4. Report the count
   ```

#### Step 4: Test Add Product Flow

1. Test the add product modal:
   ```
   Use Playwright MCP to test the add product functionality:
   1. Navigate to http://localhost:8000
   2. Click the "Add Product" button
   3. Verify the modal opens (check for modal dialog)
   4. Take a screenshot of the modal and save as "add-product-modal.png"
   5. Close the modal
   ```

2. Test form fields (without submitting):
   ```
   Use Playwright MCP to:
   1. Click "Add Product" button to open modal
   2. Verify all required form fields are present: grade, shape, thickness, width, length, quantity, location
   3. Fill in sample data:
      - Grade: 304
      - Shape: Sheet
      - Thickness: 0.55
      - Width: 1200
      - Length: 2400
      - Quantity: 100
      - Location: Warehouse-B
   4. Take screenshot as "add-product-filled.png"
   5. Close modal without submitting
   ```

#### Step 5: Generate Test Suite

1. Ask Copilot to create a reusable test file:

   Select Agent Mode and prompt:
   ```
   Based on the Playwright tests we just ran, generate a complete test file 'tests/test_ui_smoke.py' that uses pytest and playwright library to automate these UI tests. Include:
   - Test for homepage loading
   - Test for product list display
   - Test for add product modal
   - Test for form field validation
   - Proper setup and teardown
   ```

2. Review and save the generated test file.

**Expected Outcome:**
- Playwright MCP successfully automates browser testing
- Screenshots captured for validation (check project folder)
- Understand how MCP extends Copilot to browser automation
- Reusable UI test suite created

#### Step 6: Use GitHub MCP to Generate Commit Message and Push Changes

1. Ask GitHub MCP server to generate a semantic commit message for the new test file:

   Select Agent Mode and prompt:
   ```
   Use the GitHub MCP server to generate a semantic commit message for the MCP server configuration and UI test files I just added. Then commit and push the changes
   ```
   > Note: You will be prompted to Allow the commit and push operations on GitHub.com. Select Allow.

2. Verify the commit and push were successful on GitHub.com


---

## Part 2: Copilot Cloud Agent Delegation (35 minutes)

Copilot cloud agent runs on GitHub.com and can autonomously implement features, create PRs, and work in the background even when your local machine is off. In this section, you'll assign the "Low Inventory Check" issue from Part 1 to Copilot and watch it work!

### Exercise 2.1: Enable and Assign Issue to Cloud Agent (10 min)

#### Step 1: Assign Low Inventory Check Issue to Copilot

1. Go to your repository's **Issues** tab

2. Find the **"Implement Low Inventory Check System"** issue you created in Part 1

3. Click on the issue to open it

4. In the sidebar, under **Assignees**, select the Cog icon to manage assignees

5. Select **Copilot** from the dropdown

6. An Assign agent to issue dialog will appear. Select **Assign** to confirm

7. Watch the issue - Copilot will link a Pull Request that will close the issue once the implementation is complete.

#### Step 2: Monitor Initial Progress

1. Click the link to the Pull Request that Copilot creates in order to go to the Pull Request page

2. Watch as Copilot does the following:
   - Creates a checklist of tasks based on the issue requirements
   - Model changes for inventory_threshold
   - Backend endpoint implementation
   - Frontend UI updates
   - Test file creation

3. Scroll down the Pull Request and select View session. The agent runs on GitHub-hosted infrastructure, not your local machine!

**Expected Outcome:**
- Copilot cloud agent enabled for repository
- "Low Inventory Check" issue assigned to Copilot
- Pull Request created automatically
- Session shows agent working on implementation in the cloud
- Understand that cloud agent works independently

---

### Exercise 2.2: Review and Interact with Cloud Agent PR (15 min)

**Task:** Review the cloud agent's implementation and provide feedback.

#### Step 1: Navigate to the Pull Request and Read Copilot's Description

1. Go to repository **Pull requests** tab

2. Open the Pull Request created by Copilot (should mention "Low Inventory Check")

3. Note the following changes:
   - The [WIP] tag in the title has been removed, indicating the agent has completed its initial implementation
   - At the top you see a message "Copilot requested your review on this pull request." Then a button "Add your review" is available for you to click and provide feedback

4. Review Copilot's description on the PR:
   - **Approach explanation** - How the feature was implemented
   - **Code changes** - Key changes made to the codebase
   - **Testing strategy** - What tests were added and why
   - **Questions** - Any clarifications needed from you

5. Review the **Files changed** tab. You should see changes across multiple files.

#### Step 2: Request Improvements

Part A: Provide feedback by commenting on the PR. Try:

1. Go to the **Files changed** tab. Got to models.py where the default inventory threshold field was added.

2. Click a line number in the diff → Click "+" icon

3. Add a comment: "Can we make the default threshold configurable by placing it in the environment file?"

4. Click "Start a review".

5. Scroll to the top and click "Submit Review" → Select "Request changes" → then Submit review.

Part B: Provide general feedback (comment on PR conversation tab):

1. Go to the **Conversation** tab of the PR

2. Scroll to the bottom and add a comment:
```
@copilot Great work! Can you also add:
1. Admin configuration endpoint: PUT /api/config/inventory-threshold for me to change the default threshold without redeploying
2. Dashboard widget showing total count of low-stock items
3. Update the README with information about this feature
```
3. Click "Comment" to submit 

#### Step 3: Wait for Agent Response

1. Copilot will read your review and comment

2. A session will open showing Copilot processing your feedback and making additional commits to address the requested changes

3. Go to the **Commits** tab to review the new commits

**Expected Outcome:**
- Understand cloud agent's implementation approach
- Copilot responds to feedback and makes improvements
- Iterative collaboration with AI agent

---

### Exercise 2.3: Merge and Test Cloud Agent Implementation (10 min)

**Task:** Approve the PR, merge it, and test the Low Inventory Check feature locally.

#### Step 1: Final Review and Approval

1. Review all files in the **Files changed** tab one more time

2. Verify acceptance criteria from original issue:
   - ✓ Visual indicators for low-stock items
   - ✓ Filter to show only low-stock products
   - ✓ Dashboard widget with count
   - ✓ Backend endpoint for low-stock items
   - ✓ Configurable thresholds
   - ✓ Tests included

3. If everything looks good, click **"Ready for review"** to convert from draft

4. Click **"Approve"**

#### Step 2: Merge the PR

1. Click **"Merge pull request"**

2. Confirm the merge

3. Delete the branch after merging (GitHub will offer this option)

4. Check that the issue is automatically closed or updated

#### Step 3: Pull Changes Locally and Test

1. Pull the merged changes:
   ```bash
   git pull origin main
   ```

2. Start the API:
   ```bash
   cd steel-inventory-api
   uvicorn app.main:app --reload
   ```

3. Test in browser:
   - Visit http://localhost:8000
   - Look for products with low inventory indicators (red badge/icon)
   - Test the low-stock filter button
   - Check dashboard widget shows count

4. Test the API endpoint:
   ```bash
   # Windows PowerShell
   Invoke-WebRequest http://localhost:8000/api/inventory/low-stock | Select-Object -Expand Content
   
   # Or use curl
   curl http://localhost:8000/api/inventory/low-stock
   ```

5. Run the tests:
   ```bash
   pytest tests/test_inventory.py -v -k "low"
   ```

6. Verify all tests pass

**Expected Outcome:**
- PR successfully merged
- Low Inventory Check feature working end-to-end
- Visual indicators appear on low-stock products
- API endpoint returns correct data
- All tests passing
- **Complete workflow:** Issue created → Cloud agent implemented → Reviewed → Merged → Tested ✅

**Key Insight:**
This is the power of cloud agents - you created an issue with requirements, and Copilot implemented a complete feature with frontend, backend, and tests while you could have been doing other work!

---

## Part 3: GitHub Copilot CLI (50 minutes)

### Introduction: Why Use CLI vs Chat? (5 min)

You've been using Copilot Chat in VS Code throughout the intermediate lab and Parts 1-2 of this lab. Now let's understand when and why to use the CLI interface.

#### The Main Reason: Terminal-Native AI Without Context Switching

If you're already working in the terminal (git operations, DevOps tasks, system administration), **CLI lets you use Copilot without leaving your terminal workflow**. No need to switch to VS Code or break your flow state.

#### CLI Advantages

✅ **Terminal-native workflows** - Stay in your terminal for git, DevOps, system tasks  
✅ **Automation & scripting** - Use `-p` flag in CI/CD pipelines, pre-commit hooks, batch scripts  
✅ **GitHub.com integration** - Create PRs, manage issues, review code from terminal  
✅ **Plan mode** - Structured multi-step planning with clarifying questions  
✅ **Programmatic interface** - Headless operations, no GUI required  
✅ **Session persistence** - Resume conversations with `--continue`

#### Chat Advantages

✅ **Visual interface** - See diffs, file explorer, syntax highlighting  
✅ **Learning & exploration** - Better for understanding code  
✅ **Inline suggestions** - Real-time as-you-type completions  
✅ **File navigation** - Visual context selection with drag-and-drop  
✅ **Refactoring** - Side-by-side diff view before applying

#### When to Use Which?

| Scenario | Use CLI | Use Chat |
|----------|---------|----------|
| Writing commit messages | ✅ | - |
| Creating PRs from terminal | ✅ | - |
| Git history analysis | ✅ | - |
| CI/CD automation | ✅ | - |
| DevOps/infrastructure tasks | ✅ | - |
| Multi-step feature planning | ✅ | ✅ |
| Learning a new codebase | - | ✅ |
| Code refactoring with visual diff | - | ✅ |
| Inline code completion | - | ✅ |

#### Best Practice: Use Both!

Many developers use CLI 30-40% of the time for terminal-centric work and Chat 60-70% for editor-centric work. They complement each other!

**In the following exercises, you'll learn CLI through real-world scenarios where it shines over Chat.**

---

### Exercise 3.1: Terminal-Native Git Workflow (15 min)

**Scenario:** You're working in the terminal, making code changes, and want to commit and create a PR without leaving the terminal or switching to VS Code.

#### Step 1: Installation and Authentication

1. Open a terminal (PowerShell or WSL)

2. Install GitHub Copilot CLI (if not already installed):
   ```bash
   # Verify GitHub CLI is installed
   gh --version
   
   # If not installed, download from: https://github.com/cli/cli#installation
   # Then install Copilot CLI extension
   gh extension install github/gh-copilot
   ```

3. Verify Copilot CLI installation:
   ```bash
   copilot --version
   ```
   
   You should see output like: `copilot version 1.x.x`

4. Navigate to your project:
   ```bash
   cd steel-inventory-api
   ```

5. Start Copilot CLI:
   ```bash
   copilot
   ```

6. If not logged in, use `/login` and follow the authentication flow:
   - Authorize with your GitHub account
   - Verify you have Copilot Pro or Business subscription

7. When prompted about trusted directories, choose:
   **"Yes, and remember this folder for future sessions"**
   
   **Security Note:** Only trust directories you control. Copilot CLI can read/modify files.

#### Step 2: Make a Small Code Change

1. Exit CLI (type `exit` or press Ctrl+C)

2. Add a helpful docstring to a function:
   ```bash
   # Edit app/main.py and add/improve a docstring
   # Or add a comment explaining a complex section
   ```

3. Stage the change:
   ```bash
   git add app/main.py
   ```

#### Step 3: Use CLI for Semantic Commit Message

Use CLI's programmatic interface (non-interactive with `-p` flag):

```bash
copilot -p "Generate a semantic commit message for my staged changes"
```

This generates a commit message like:
```
docs: add comprehensive docstring to main API entry point
```

Copy the message and commit:
```bash
git commit -m "docs: add comprehensive docstring to main API entry point"
```

**Why CLI here?** You stayed in the terminal. With Chat, you'd need to switch to VS Code.

#### Step 4: Create Feature Branch and Push

Start interactive CLI session:
```bash
copilot
```

In CLI, ask:
```
Help me create a feature branch for documentation improvements and push it to GitHub
```

CLI will suggest commands like:
```bash
git checkout -b docs/improve-docstrings
git push -u origin docs/improve-docstrings
```

Approve and execute the commands.

#### Step 5: Create PR from Terminal

In CLI, ask:
```
Create a pull request for this branch with a good description
```

Copilot will use GitHub integration to create a PR directly from the terminal!

#### Step 6: Review Git History

```
Summarize the last 5 commits in this repository with their purpose
```

CLI analyzes git history and provides a summary without opening any GUI.

**Core Shortcuts to Remember:**

| Shortcut | Action |
|----------|--------|
| `Esc` | Cancel current operation |
| `Ctrl+C` | Cancel/clear/exit |
| `Ctrl+L` | Clear screen |
| `@` | Mention files for context |
| `/` | Show slash commands |
| `?` | Show help |
| `↑` `↓` | Command history |
| `Shift+Tab` | Switch modes (ask/execute ↔ plan) |

**Expected Outcome:**
- CLI authenticated and comfortable with interface
- Generated commit message without leaving terminal
- Created PR from command line
- **Why this matters:** Terminal-native workflow is faster for git operations

---

### Exercise 3.2: DevOps/System Administration Scenario (15 min)

**Scenario:** You need to analyze logs, check system resources, write automation scripts, and set up monitoring - all terminal-based work.

#### Step 1: File System and Log Analysis

1. Start interactive CLI:
   ```bash
   copilot
   ```

2. Analyze recent changes:
   ```
   Find all Python files modified in the last 7 days and show their sizes
   ```

3. Check for potential issues:
   ```
   Search for any TODO or FIXME comments in the Python code and list them with file locations
   ```

#### Step 2: Write Automation Script

Ask CLI to generate a cleanup script:
```
Help me write a bash script that:
- Finds all .pyc files older than 30 days
- Finds all __pycache__ directories
- Safely deletes them with user confirmation
- Logs what was deleted to cleanup.log
```

Review the generated script, save it, and test it.

#### Step 3: Multi-Step Infrastructure Task with Plan Mode

1. Press `Shift+Tab` to switch to **plan mode** (watch status bar change)

2. Enter this complex request:
   ```
   Create a comprehensive monitoring solution for our steel inventory API:
   
   1. Health check script that:
      - Checks if API is running on port 8000
      - Tests the /health endpoint (create it if it doesn't exist)
      - Tests database connectivity
      - Checks disk space
      - Logs results to monitoring.log with timestamp
      
   2. Alert script that:
      - Sends email if service is down
      - Uses environment variables for email config
      
   3. Cron job configuration to run health check every 5 minutes
   
   4. Instructions for setting up the monitoring
   ```

3. CLI will:
   - Ask clarifying questions: "What email service should I use?"
   - Build a structured plan with phases
   - Show you the plan before executing

4. Review the plan structure - notice how it breaks down the complex task

5. Approve the plan and watch CLI execute:
   - Creates health check script
   - Creates alert script
   - Generates cron configuration
   - Provides setup instructions

**Why CLI here?** DevOps work is terminal-native. Plan mode structures complex infrastructure tasks. Chat doesn't have direct shell integration.

**Context Management Commands:**

```
/context
```
Shows current token usage, files in context, available space

```
/compact
```
Manually compress conversation history (happens auto at 95% capacity)

```
/usage
```
Shows premium requests used, session duration, token usage

---

### Exercise 3.3: Automation & CI/CD Scenario (15 min)

**Scenario:** You need to automate code quality checks, batch operations, and integrate AI into CI/CD pipelines.

#### Step 1: Programmatic Usage for Automation

The `-p` flag enables non-interactive mode for scripts and pipelines:

**Generate documentation:**
```bash
copilot -p "Generate a brief API documentation summary from the OpenAPI schema at http://localhost:8000/openapi.json"
```

**Silent mode for scripts** (only output, no usage info):
```bash
copilot -sp "List all API endpoints defined in this project" > endpoints.txt
```

**Pipe input to Copilot:**
```bash
pytest --tb=short 2>&1 | copilot -p "Analyze these test results and create a summary table"
```

#### Step 2: Security Controls for CI/CD

**Allow specific safe commands:**
```bash
copilot --allow-tool='shell(git log)' --allow-tool='shell(git diff)' -p "Check if any commits in the last week added hardcoded secrets or API keys"
```

**Deny destructive operations:**
```bash
copilot --allow-all-tools --deny-tool='shell(rm)' --deny-tool='shell(git push)' -p "Clean up old log files"
```

This is critical for CI/CD: Allow automation but prevent dangerous operations.

**Create a pre-commit hook:**
```bash
copilot --allow-tool='write' -p "Create a pre-commit hook that checks for hardcoded passwords or API keys in staged files"
```

#### Step 3: Batch Operations

Update multiple files safely:
```bash
copilot --allow-tool='write' -p "Update the copyright year to 2026 in all Python files under app/, but show me the changes first"
```

CLI will show a plan, you approve, then it executes.

#### Step 4: Session Management for Long Tasks

Start a complex refactoring:
```bash
copilot -p "Add type hints to all functions in app/routers/ directory"
```

If you need to stop and resume later:
```bash
# Later...
copilot --continue
```

This resumes your last session with full context preserved!

#### Step 5: Model Selection for Cost Optimization

Start CLI and use model command:
```
/model
```

You'll see models with their request multipliers:
- Claude Sonnet 4.5 (1x) - Default, best for complex tasks
- GPT-4 Turbo (0.8x) - Faster, good for simple tasks
- Claude Haiku (0.3x) - Cheapest, quick operations

**Strategy:**
- Use cheaper models for simple tasks (commit messages, summaries)
- Use powerful models for complex reasoning (architecture decisions, debugging)

#### Step 6: Session Insights with Chronicle

```
/chronicle
```

Shows timeline of your session:
- Commands executed
- Files modified
- Key decisions made
- Useful for standup reports!

**Why CLI here?**
- **Scriptable** - Integrates into CI/CD pipelines
- **Headless** - Works without GUI (servers, containers)
- **Security controls** - Fine-grained tool permissions
- **Automatable** - Programmatic interface for batch operations

Chat simply cannot do these things.

**Expected Outcome:**
- Understand CLI's programmatic interface for automation
- Know security flags (`--allow-tool`, `--deny-tool`) for safe automation
- Can use CLI in CI/CD pipelines
- Understand session management and model selection
- **Appreciate when CLI is the right tool vs Chat**

---

## Part 4: Enterprise Governance (20 minutes)

Enterprise governance controls help organizations manage how Copilot is used across teams, ensuring security, compliance, and consistency.

### Exercise 4.1: Configure Enterprise Policies (10 min)

**Note:** Some of these settings require organization or enterprise admin access. For learning purposes, we'll demonstrate the configuration even if you can't fully apply them.

#### Policy 1: Tools Allow-List

Control which tools Copilot CLI can use without manual approval.

1. Create a file `.github/copilot-policy.yml` in your repository:

```yaml
# Copilot Enterprise Governance Policy
version: 1

# Tools that Copilot CLI can use automatically
tools:
  allow:
    - read  # Allow reading files
    - github  # Allow GitHub MCP server tools
    - playwright  # Allow Playwright MCP server tools
  
  deny:
    - shell(rm)  # Prevent deletion commands
    - shell(git push)  # Require manual git push approval
  
  require_approval:
    - write  # Require approval for file modifications
    - shell  # Require approval for other shell commands
```

2. Test this policy in Copilot CLI:
   ```bash
   copilot --deny-tool='shell(rm)' --allow-tool='read'
   ```

3. Try to have Copilot delete a file - it should be blocked:
   ```
   Delete all temporary files in this directory
   ```

#### Policy 2: MCP Servers Allow-List

1. Update `.github/copilot-policy.yml` to control MCP server usage:

```yaml
# MCP Server Policy
mcp:
  enabled: true
  registry_url: "https://github.com/mcp"  # Official MCP registry
  
  allowed_servers:
    - github  # Allow GitHub MCP server
    - playwright  # Allow Playwright MCP server
  
  denied_servers:
    - "*"  # Deny all other servers by default
```

#### Policy 3: Data Residency Configuration

For enterprises with data residency requirements:

```yaml
# Data Residency Policy
data_residency:
  region: "EU"  # or "US"
  
  # Prevent data from leaving specified regions
  enforce_regional_processing: true
```

#### Policy 4: File/Content Exclusions

Prevent Copilot from accessing sensitive files:

```yaml
# Content Exclusions
exclusions:
  files:
    - "**/*.key"
    - "**/*.pem"
    - "**/secrets.yml"
    - "**/.env"
  
  directories:
    - ".git"
    - "node_modules"
    - "venv"
    - "__pycache__"
  
  patterns:
    - "password"
    - "secret_key"
    - "api_key"
    - "private_key"
```

#### Policy 5: Audit Logging

Enable comprehensive audit logs:

```yaml
# Audit Configuration
audit:
  enabled: true
  
  log_events:
    - copilot_requests
    - mcp_tool_usage
    - file_modifications
    - pr_creations
    - agent_delegations
  
  retention_days: 90
  
  export_to:
    - cloudwatch  # AWS CloudWatch
    - splunk      # Splunk
    - datadog     # Datadog
```

**Expected Outcome:**
- Policy file created with governance rules
- You understand how to control tool access
- File exclusions protect sensitive data

---

### Exercise 4.2: Signed Commits and PR Metrics (10 min)

#### Step 1: Enable Signed Commit Requirement

**Note:** This requires repository or organization settings access.

1. Go to your repository **Settings** → **Branches**

2. Add a branch protection rule for `main`:
   - Check **"Require signed commits"**
   - Check **"Require pull request reviews before merging"**

3. Update your `.github/copilot-policy.yml`:

```yaml
# Signed Commit Policy
commits:
  require_signed: true
  require_gpg: true
  
  allowed_signers:
    - "copilot[bot]@github.com"  # Allow Copilot cloud agent
  
  reject_unsigned: true
```

4. Configure GPG signing locally:
   ```bash
   git config --global commit.gpgsign true
   git config --global user.signingkey YOUR_GPG_KEY_ID
   ```

#### Step 2: Configure PR Outcomes Metrics

Track the success rate of Copilot-generated PRs:

1. Update `.github/copilot-policy.yml`:

```yaml
# PR Metrics Configuration
metrics:
  track_pr_outcomes: true
  
  tracked_metrics:
    - pr_acceptance_rate
    - time_to_merge
    - review_cycles
    - test_pass_rate
    - code_quality_score
  
  reporting:
    frequency: weekly
    recipients:
      - team-leads@example.com
      - engineering-managers@example.com
    
    dashboards:
      - github_insights
      - datadog
```

#### Step 3: Review FedRAMP Compliance Settings

For government/enterprise compliance:

```yaml
# FedRAMP Compliance
compliance:
  fedramp_mode: true
  
  requirements:
    - signed_commits
    - audit_logging
    - data_residency_us
    - encrypted_storage
    - access_control
  
  certifications:
    - fedramp_moderate
    - soc2_type2
    - iso27001
```

#### Step 4: Test Policy Enforcement

1. Try to create a commit without signing:
   ```bash
   git config --global commit.gpgsign false
   git commit -m "Test unsigned commit"
   git push
   ```

2. The push should be rejected if signed commit policy is enforced

3. Re-enable signing and try again:
   ```bash
   git config --global commit.gpgsign true
   git commit --amend --no-edit -S
   git push
   ```

**Expected Outcome:**
- Signed commit requirement configured
- PR metrics tracking enabled
- FedRAMP compliance settings understood

**Commit:**
Commit your policy file:
```bash
git add .github/copilot-policy.yml
git commit -m "feat: Add enterprise governance policies"
git push
```

---

## Capstone Challenge (Optional): CLI vs Chat Comparison (20 minutes)

**Goal:** Implement a "Steel Batch Traceability" feature using both Chat and CLI, then compare the approaches.

**Note:** This is an optional challenge. If time is limited, you can skip this and proceed to the Lab Completion Checklist.

### Feature Requirements

Implement a batch tracking system with the following specifications:

**Database Schema:**
- `Batch` model with fields:
  - `id` (auto-generated)
  - `batch_number` (unique, format: BATCH-YYYY-NNNN)
  - `production_date` (date)
  - `quality_grade` (Premium/Standard/Economy)
  - `quantity_produced` (integer)
  - `status` (Active/Completed/Recalled)

**API Endpoints:**
- `POST /batches/` - Create new batch
- `GET /batches/` - List all batches with filtering
- `GET /batches/{id}` - Get batch details
- Link `SteelProduct` to `Batch` (add `batch_id` foreign key)

**Business Logic:**
- Auto-generate batch numbers
- Validate quality grades
- Write comprehensive tests

---

### Approach A: VS Code Copilot Chat (10 minutes)

1. Open VS Code Copilot Chat

2. Provide the complete feature requirements:
   ```
   Implement the batch traceability feature with all the requirements above. Use good coding practices and include tests.
   ```

3. Review and approve the generated code

4. Run tests:
   ```bash
   pytest tests/test_batches.py -v
   ```

**Time this approach and note:**
- How many interactions required?
- Quality of initial code
- Time spent reviewing diffs

---

### Approach B: Copilot CLI Plan Mode (10 minutes)

1. Open Copilot CLI:
   ```bash
   copilot
   ```

2. Switch to **plan mode** (Shift+Tab)

3. Provide the feature requirements:
   ```
   Implement steel batch traceability with:
   [paste full requirements here]
   ```

4. Review the generated plan

5. Approve execution and monitor progress

6. Run tests:
   ```bash
   pytest tests/test_batches.py -v
   ```

**Time this approach and note:**
- Plan structure quality
- Terminal-native workflow benefits
- Any advantages over Chat approach

---

### Comparison Summary

**Which approach was faster?**  
**Which produced better code?**  
**When would you use each?**

Key Insights:
- **Chat** is better for: Visual code review, learning, complex refactoring
- **CLI** is better for: Terminal workflows, automation, git operations
- **Both** work well for: Feature implementation, code generation

---

## Lab Completion Checklist

Review your accomplishments:

### Part 1: MCP Integration
- [ ] Installed GitHub MCP server from registry
- [ ] Configured authentication with `${input:}` secrets
- [ ] Searched GitHub for similar steel inventory repositories
- [ ] Created issues for missing capabilities found in other repos
- [ ] Created comprehensive "Low Inventory Check" issue
- [ ] Installed Playwright MCP server
- [ ] Automated UI testing with Playwright MCP
- [ ] Generated reusable UI test suite

### Part 2: Copilot Cloud Agent
- [ ] Enabled Copilot cloud agent in repository settings
- [ ] Assigned "Low Inventory Check" issue to Copilot
- [ ] Monitored draft PR created by cloud agent
- [ ] Reviewed cloud agent's implementation and comments
- [ ] Provided feedback and requested improvements
- [ ] Merged cloud agent PR successfully
- [ ] Tested Low Inventory Check feature locally
- [ ] All tests passing for new feature

### Part 3: GitHub Copilot CLI
- [ ] Understand when to use CLI vs Chat
- [ ] Authenticated Copilot CLI and understand trust model
- [ ] Used CLI for terminal-native git workflow (commits, PRs)
- [ ] Switched between ask/execute and plan modes with Shift+Tab
- [ ] Completed DevOps/system administration scenarios
- [ ] Generated automation scripts with CLI
- [ ] Used programmatic interface with `-p` flag
- [ ] Applied security controls (`--allow-tool`, `--deny-tool`)
- [ ] Understand session management (`--continue`)
- [ ] Explored model selection (`/model`)

### Part 4: Enterprise Governance
- [ ] Created `.github/copilot-policy.yml` with tools allow-list
- [ ] Configured MCP servers policy
- [ ] Set up file/content exclusions
- [ ] Enabled audit logging configuration
- [ ] Configured signed commit requirements
- [ ] Set up PR outcomes metrics tracking
- [ ] Reviewed compliance settings (FedRAMP)

### Optional Capstone Challenge
- [ ] Implemented batch traceability with Chat
- [ ] Implemented batch traceability with CLI plan mode
- [ ] Compared both approaches
- [ ] Documented findings and insights

---

## Key Takeaways

1. **MCP extends Copilot's reach** - GitHub MCP enables repository search and issue creation; Playwright MCP enables UI testing - integrate any external tool or service
2. **Cloud agents enable async autonomous work** - Assign issues to @copilot and get complete implementations with PRs while you do other work
3. **CLI vs Chat: Choose based on context** - CLI for terminal-native workflows (git, DevOps, automation); Chat for editor-centric work (coding, refactoring)
4. **Terminal-native workflows are powerful** - CLI eliminates context switching for git operations, system administration, and CI/CD integration
5. **Governance ensures safe enterprise adoption** - Control tool access, audit usage, enforce compliance policies at scale
6. **The right tool for the right job** - Chat, CLI, and cloud agents each have strengths - master all three surfaces

**Core Workflow Learned:**
1. Use MCP to research and create feature requirements (GitHub issue)
2. Assign issue to cloud agent for autonomous implementation
3. Use CLI for terminal operations and automation
4. Apply governance policies to ensure security and compliance

---

## Next Steps

- Explore more MCP servers in the [GitHub MCP Registry](https://github.com/mcp)
- Create custom MCP servers for your organization's internal tools
- Define organization-level custom agents in `.github-private/agents/`
- Implement comprehensive governance policies for your team
- Integrate Copilot workflows into your CI/CD pipelines

---

## Resources

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [About GitHub Copilot CLI](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-copilot-cli)
- [Copilot CLI Getting Started](https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-getting-started)
- [Model Context Protocol Docs](https://modelcontextprotocol.io/)
- [GitHub MCP Server Repository](https://github.com/github/github-mcp-server)
- [Copilot CLI Reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference)
- [Enterprise Governance Guide](https://docs.github.com/en/copilot/managing-copilot)
- [Agent Customization Cheat Sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)

---

**Congratulations on completing the Advanced GitHub Copilot Lab!** 🎉

You now have the skills to leverage Copilot's most advanced capabilities across multiple surfaces (IDE, CLI, Cloud), extend it with external tools via MCP, and implement enterprise-grade governance controls.
