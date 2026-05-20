# GitHub Copilot Advanced Lab Instructions
**Duration:** 2.5 hours  
**Level:** Advanced

---

## Pre-Lab Setup (15 minutes before lab)

### 1. Prerequisites
- **Git configured** and repository pushed to GitHub
- **PowerShell v6 or higher** (for Windows users)
- **Node.js 22 or later** (if installing via npm)

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

**Windows (WinGet - Recommended):**
```powershell
winget install GitHub.Copilot
```

**Alternative: npm (All Platforms):**
```bash
npm install -g @github/copilot
```

**Verify installation:**
```bash
copilot --version
```

You should see output like: `copilot version 1.x.x`

---

## Lab Overview

In this advanced lab, you'll extend GitHub Copilot's capabilities by integrating external tools via MCP, delegate work to Copilot cloud agent for automated feature implementation, and use Copilot CLI to complete a real-world FastAPI project with terminal-native workflows. You'll also implement enterprise governance controls for safe adoption at scale.

**Lab Flow:** You'll use GitHub MCP to research similar projects and create a "Low Inventory Check" feature issue, immediately assign it to Copilot cloud agent for implementation, then use CLI to complete the BlueScope Steel Inventory API by implementing weight calculations, running tests, and automating git workflows — all from the terminal.

### Learning Objectives Checklist
- [ ] Install and configure MCP servers (GitHub, Playwright)
- [ ] Use GitHub MCP to search repositories and create issues
- [ ] Use Playwright MCP to automate UI testing
- [ ] Assign GitHub issues to Copilot cloud agent
- [ ] Review and interact with cloud agent PRs
- [ ] Merge and test cloud agent implementations
- [ ] Complete a real FastAPI project using Copilot CLI
- [ ] Master CLI's plan mode for structured feature implementation
- [ ] Run test-driven development loops from terminal
- [ ] Automate git workflows (commit, branch, PR) with CLI
- [ ] Use programmatic mode for CI/CD automation
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

7. Watch the issue - Copilot will link a Pull Request that will close the issue once the implementation is complete. The Pull Request will have a title like "[WIP] Implement Low Inventory Check System - Work in Progress by Copilot"

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

1. Go to the **Files changed** tab. Go to models.py where the default inventory threshold field was added.

2. Click a line number in the diff → Click "+" icon

3. Add a comment: "Can we make the default threshold configurable by placing it in the environment file?"

4. Click Start a review.

5. Scroll to the top and click Submit Review → Select Request changes → then Submit review.

Part B: Provide general feedback (comment on PR conversation tab):

1. Go to the **Conversation** tab of the PR

2. Scroll to the bottom and add a comment:
```
@copilot Great work! Can you also add:
1. Admin configuration endpoint: PUT /api/config/inventory-threshold for me to change the default threshold without redeploying
2. Dashboard widget showing total count of low-stock items
3. Update the README with information about this feature
```
3. Click Comment to submit 

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

#### Step 1: Final Review, Approval and Merge the PR

1. Review all files in the **Files changed** tab one more time

2. Verify acceptance criteria from original issue:
   - ✓ Visual indicators for low-stock items
   - ✓ Filter to show only low-stock products
   - ✓ Dashboard widget with count
   - ✓ Backend endpoint for low-stock items
   - ✓ Configurable thresholds
   - ✓ Tests included

3. If everything looks good, scroll down to the bottom of the PR and click **"Ready for review"** to convert from draft

4. Click **Approve** → **Merge Pull Request** → **Confirm merge**

5. Delete the branch after merging (GitHub will offer this option)

6. Check that the issue is automatically closed

#### Step 2: Pull Changes Locally and Test

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

4. Test the API endpoint by visiting http://127.0.0.1:8000/docs and trying out the GET /api/inventory/low-stock endpoint. Verify it returns the correct products based on the threshold.
   
5. Run the tests:
   ```bash
   pytest tests/test_inventory.py -v -k "low"
   ```

6. Verify all tests pass

7. If there are any front or backend issues, have GitHub Copilot fix them. Use Playwright MCP to automate testing of the UI issues.

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

## Part 3: GitHub Copilot CLI - Complete the BlueScope Steel Inventory API (70 minutes)

### Overview

In this hands-on lab, you'll use GitHub Copilot CLI to complete a partially implemented steel inventory API. The API has the basic structure in place, but critical features like CRUD operations, weight calculations, and comprehensive tests are missing. You'll learn how CLI's terminal-native interface, plan mode, and programmatic capabilities make it ideal for structured development workflows.

**What's Missing:**
- Complete CRUD operations
- Weight and dimension calculations for different steel shapes
- Comprehensive test suite
- Git workflow automation

---

### Exercise 3.1: Install and Authenticate GitHub Copilot CLI (10 min)

#### Step 1: Install GitHub Copilot CLI

**Windows (WinGet - Recommended):**
```powershell
winget install GitHub.Copilot
```

**Alternative: npm (All Platforms):**
```bash
npm install -g @github/copilot
```

**macOS/Linux (Homebrew):**
```bash
brew install copilot-cli
```

**macOS/Linux (Install Script):**
```bash
curl -fsSL https://gh.io/copilot-install | bash
```

For more installation options, see: [Official Installation Guide](https://docs.github.com/en/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli)

**Verify installation:**
```bash
copilot --version
```

You should see output like: `copilot version 1.x.x`

#### Step 2: Authenticate with GitHub

```bash
copilot
```

If not logged in, use `/login` and follow the authentication flow:
- Authorize with your GitHub account
- Verify you have Copilot Pro or Business subscription

**Expected Outcome:**
- GitHub Copilot CLI installed and authenticated
- Ready to use CLI for terminal-native development

---

### Exercise 3.2: Setup Project and Understand CLI Advantage (5 min)

#### Step 1: Navigate to Project Directory

```bash
cd steel-inventory-api
```

#### Step 2: Start Copilot CLI

```bash
copilot
```

When prompted about trusted directories, choose:
**"Yes, and remember this folder for future sessions"**

**Security Note:** Only trust directories you control. Copilot CLI can read and modify files in trusted directories.

#### Step 3: Understand the CLI Advantage

**Key Concept:** CLI gives Copilot direct access to:
- All your files (no copy-paste needed)
- Your git history and branches
- Your terminal (can run commands and read output)
- Your file system (can search, modify, create files)

---

### Exercise 3.3: Onboard with the Codebase (10 min)

**Goal:** Use CLI to understand the project structure and identify what's implemented vs. what's TODO.

#### Step 1: Project Overview

In the Copilot CLI session, ask:

```
Explain this project's structure and what's already built vs. any TODO
```
> Note: When prompted to allow file access, select "Yes, and add these directories to the allowed list."

CLI will:
- Read the README.md
- Scan the directory structure
- Identify implemented features
- List TODO items

**Expected Response:**
- Project structure overview
- What endpoints are implemented
- What features need to be completed (CRUD operations, weight calculations, tests)

#### Step 2: Deep Dive into Main Entry Point

```
Walk me through main.py and explain how the application is structured
```

CLI will:
- Read `app/main.py`
- Explain the FastAPI application setup
- Describe the router configuration
- Identify dependencies and middleware

#### Step 3: Review Data Models

```
Show me the data models in models.py and explain what fields are defined for steel products
```

**What You're Learning:**
- CLI reads files directly without you opening them
- No context switching to VS Code
- Immediate understanding of codebase structure
- CLI can synthesize information from multiple files

---

### Exercise 3.4: Plan Mode → Implement Weight Calculations (15 min)

**Goal:** Use CLI's exclusive **plan mode** to structure a complex feature implementation before writing any code.

#### Step 1: Enter Plan Mode

Press `Shift+Tab` to switch to **plan mode**

Notice the status bar changes to indicate you're in plan mode.

#### Step 2: Describe the Feature

Enter this prompt:

```
Implement weight and dimension calculations for each steel shape (sheet, coil, plate, bar, tube) based on grade density. 

Requirements:
- Add density property to each steel grade (e.g., 304 stainless = 8000 kg/m³)
- Create calculate_weight() method for each shape type
- Use proper formulas:
  - Sheet: length × width × thickness × density
  - Coil: π × (outer_radius² - inner_radius²) × width × density
  - Plate: length × width × thickness × density
  - Bar: π × radius² × length × density (for round bars)
  - Tube: π × (outer_radius² - inner_radius²) × length × density
- Add these calculations to the SteelProduct model
- Update the API responses to include calculated weight
```

#### Step 3: Review the Plan


1. CLI will ask clarifying questions (e.g., "Should I store density in the database or as constants?"). When prompted for clarifications, select any option of your preference. You can even ask for additional clarifications if needed or their suggestions on how to approach the implementation.

2. CLI will generate a structured multi-step plan:
   - Approach
   - Key Decisions
   - Sequential Phases in Implementation

3. CLI will show you the plan before executing anything

#### Step 4: Approve and Execute

1. Review the plan carefully. If it looks good, select "Accept plan and build on autopilot (recommended)".
> Note: You may be prompted to Enable autopilot mode. Select "Enable all permissions (recommended)".

2. CLI will then:
   - Create or modify files as needed
   - Show you what it's doing at each step
   - Apply changes incrementally

**Why Plan Mode?**
- Forces alignment before code generation
- Breaks complex tasks into manageable steps
- Allows you to catch issues early

---

### Exercise 3.5: Test → Fail → Fix Loop (15 min)

**Goal:** Experience CLI's ability to run tests, read failures, and fix issues — all without leaving the terminal.

#### Step 1: Generate Tests

```
Write pytest tests for the weight calculations in test_weight_calculations.py. Include tests for:
- Each steel shape type (sheet, coil, plate, bar, tube)
- Edge cases (zero dimensions, negative values)
- Density calculations for different steel grades
Then run the tests using pytest
```

CLI will:
1. Create `tests/test_weight_calculations.py`
2. Write comprehensive test cases
3. Run `pytest tests/test_weight_calculations.py -v`
4. Show you the test output

**Expected:** Some tests may fail initially — this is intentional! Copilot will iteratively fix the implementation based on test results.

#### Step 2: Add More Tests

```
Add tests for the complete CRUD operations (Create, Read, Update, Delete) for steel products and run them. Handle edge cases like invalid input and missing fields. 
```

**Why CLI Here?**
- Copilot runs pytest directly
- Reads failure output automatically
- Edits files based on test results
- Re-runs tests to verify fixes
- **Full test-driven development loop without leaving terminal**

---

### Exercise 3.6: Git Workflow + Programmatic Mode (10 min)

**Goal:** Use CLI for complete git workflow automation, then demonstrate headless/scriptable mode.

#### Step 1: Interactive Git Workflow

1. Create a conventional commit message and commit changes.

**Still in the CLI session**, prompt:

```
Stage these changes and write a conventional commit message
```

   CLI will:
   - Run `git add .` (or suggest specific files)
   - Generate a semantic commit message like:
      ```
      feat(calculations): implement weight calculations for all steel shapes
      
      - Add density constants for steel grades
      - Implement shape-specific weight formulas
      - Add comprehensive test suite for calculations
      ```
   - Show you the commit command
   - Perform the commit for you since we are in Autopilot mode

2. Create a new branch and open a PR.

**Still in the CLI session**, prompt:

```
Push to a new branch called weight-calc and open a PR
```

   CLI will:
   - Create branch: `git checkout -b weight-calc`
   - Push: `git push -u origin weight-calc`
   - Create PR using GitHub integration: `gh pr create`
   - Generate PR title and description

3. Go to GitHub.com and review the PR created by CLI.


#### Step 2: Programmatic Mode (Headless)

Exit the interactive CLI session (type `exit` or press Ctrl+C).

**Now demonstrate headless mode with the `-p` flag:**

```bash
copilot -p "Summarize today's commits" --allow-tool='shell(git)'
```

This runs Copilot in **non-interactive programmatic mode**:
- No approval prompts
- Direct output
- Perfect for automation scripts
- Can be used in CI/CD pipelines

**Try another example:**

```bash
copilot -sp "List all API endpoints defined in this project" > endpoints.txt
```

The `-s` flag (silent) suppresses usage information — only outputs the result.

**Why This Matters:**
- Git/GitHub integration without leaving terminal
- **Scriptable** for automation (pre-commit hooks, CI/CD)
- **Headless operations** — no GUI required
- Chat can't do any of this

---

### Exercise 3.7: Wrap & Discussion (5 min)

#### Step 1: Review Session Chronicle

Start CLI again:
```bash
copilot
```

Run the chronicle command:
```
/chronicle
```

This shows:
- Timeline of your session
- Commands executed
- Files modified
- Key decisions made
- **Perfect for standup reports!**

#### Step 2: Security & Governance Discussion

**Approval Prompts:**
- CLI asks for approval before executing destructive operations
- You can configure auto-approval for specific tools

**Security Tradeoffs:**

| Mode | Security | Convenience | Use Case |
|------|----------|-------------|----------|
| `--allow-tool='shell(git log)'` | 🟢 High | Medium | CI/CD with specific commands only |
| `--allow-all-tools` | 🟡 Medium | High | Interactive development |
| `--allow-all-tools --deny-tool='shell(rm)'` | 🟢 High | High | Safe automation (block destructive ops) |

**Example safe CI/CD usage:**
```bash
copilot --allow-tool='shell(git)' --deny-tool='shell(rm)' -p "Generate release notes"
```

#### Step 3: Quick Q&A

**Discussion Questions:**
1. Where would you use CLI in your day-to-day work?
   - Terminal-heavy workflows (git, DevOps, system admin)
   - Automation scripts and CI/CD pipelines
   - When you want AI without context switching

2. When would you prefer Chat over CLI?
   - Visual code review and refactoring
   - Learning new codebases with side-by-side diffs
   - When you need inline code completion

3. What surprised you about plan mode?
   - Structured approach forces better planning
   - Catch issues before code is written
   - Breaks complex tasks into manageable steps

**Expected Outcome:**
- Complete understanding of CLI's capabilities and advantages
- Practical experience with terminal-native development
- Clear understanding of when to use CLI vs Chat

---

### Key Takeaways

✅ **Terminal-Native Development**
- CLI has direct access to files, git, and terminal
- No context switching or copy-paste needed
- Perfect for terminal-centric workflows

✅ **Plan Mode (CLI-Exclusive)**
- Structures complex tasks before execution
- Forces alignment and catches issues early
- Breaks work into manageable phases

✅ **Test-Driven Development Loop**
- Run tests, read failures, fix issues, re-run
- Complete loop without leaving terminal
- Faster iteration than Chat approach

✅ **Git Workflow Automation**
- Generate commit messages, create branches, open PRs
- All from the terminal without switching to browser/VS Code
- GitHub integration built-in

✅ **Programmatic Interface**
- `-p` flag for headless automation
- `--allow-tool` / `--deny-tool` for security
- Perfect for CI/CD pipelines and scripts

✅ **When to Use CLI vs Chat**
- **CLI:** Terminal workflows, automation, git operations, DevOps
- **Chat:** Visual refactoring, learning, inline completion, code review
- **Both:** Feature implementation, code generation

**Core Workflow Completed:**
1. ✅ Understood codebase structure with CLI
2. ✅ Used plan mode to implement complex feature
3. ✅ Ran test-driven development loop
4. ✅ Automated git workflow from terminal
5. ✅ Demonstrated programmatic/scriptable mode

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

### Part 3: GitHub Copilot CLI - BlueScope Steel Inventory API
- [ ] Installed GitHub Copilot CLI
- [ ] Authenticated with GitHub via CLI
- [ ] Set up trusted directory in CLI
- [ ] Understood CLI's direct access to files, git, and terminal
- [ ] Used CLI to onboard with codebase structure
- [ ] Used plan mode (Shift+Tab) to implement weight calculations
- [ ] Reviewed and approved structured implementation plan
- [ ] Completed test-driven development loop (write, run, fix, re-run)
- [ ] Generated and ran pytest tests from CLI
- [ ] Automated git workflow (commit, branch, PR) from terminal
- [ ] Used programmatic mode with `-p` flag for headless operations
- [ ] Reviewed session with `/chronicle` command
- [ ] Understand security tradeoffs (`--allow-tool`, `--deny-tool`)

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
2. **Cloud agents enable async autonomous work** - Assign issues to Copilot and get complete implementations with PRs while you do other work
3. **CLI is terminal-native and powerful** - Direct access to files, git, and terminal eliminates context switching for terminal-centric workflows
4. **Plan mode forces structured thinking** - CLI-exclusive plan mode breaks complex tasks into phases and aligns before executing
5. **Test-driven development loop in terminal** - Run tests, read failures, fix code, re-run — complete TDD cycle without leaving CLI
6. **Programmatic interface enables automation** - Use `-p` flag for headless operations in CI/CD pipelines and scripts
7. **Governance ensures safe enterprise adoption** - Control tool access, audit usage, enforce compliance policies at scale

**Core Workflow Learned:**
1. Use MCP to research and create feature requirements (GitHub issue)
2. Assign issue to Copilot cloud agent for autonomous implementation
3. Use CLI for terminal-native development and complete features with plan mode
4. Automate git workflows and testing from the command line
5. Apply governance policies to ensure security and compliance

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
