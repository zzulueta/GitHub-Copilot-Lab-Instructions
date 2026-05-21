# GitHub Copilot Advanced Lab Instructions
**Duration:** 2.5 hours  
**Level:** Advanced

---

## Pre-Lab Setup (15 minutes)

### 1. Prerequisites
- GitHub Copilot license activated
- VS Code with GitHub Copilot extensions installed
- Python 3.9+ installed
- Git configured

### 2. Verify Setup
Check that the steel inventory API is working. In your terminal, navigate to the project directory and start the API:
```bash
cd steel-inventory-api
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```
Visit http://localhost:8000/docs - you should see the API documentation.

---

## Lab Overview

In this advanced lab, you'll extend GitHub Copilot's capabilities by integrating external tools via MCP, delegate work to Copilot cloud agent for automated feature implementation, and use Copilot CLI to complete a real-world FastAPI project with terminal-native workflows.

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

6. In the MCP Servers tab you will see the GitHub MCP server listed but is not yet running. Right-click the GitHub MCP server and select **Start Server**

7. You may be prompted to authenticate with GitHub. 
   - Select Allow to grant permissions to the MCP server
   - You will be redirected to GitHub.com to authorize VS Code. Select **Continue** and complete the authentication flow.

8. After successful authentication in the browser, return to VS Code and you should see the GitHub MCP server status as "Running" in the MCP Servers tab.

9. Now let us see the MCP JSON configuration:
   - Click Extensions (Ctrl+Shift+X) in the sidebar → MCP Servers - INSTALLED → GitHub
   - Select Manage (cog icon) → Show Configuration (JSON)

10. You should see a configuration like this with a Running status:
```json
{
	"servers": {
		"io.github.github/github-mcp-server": {
			"type": "http",
			"url": "https://api.githubcopilot.com/mcp/",
			"gallery": "https://api.mcp.github.com",
			"version": "1.0.4"
		}
	},
	"inputs": []
}
```

#### Step 2: Setup GitHub MCP Server with Personal Access Token (PAT)

1. Go to GitHub.com → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token

2. Set token settings:
   - Name: "github-mcp-server-token"
   - Description: "Token for GitHub MCP server"
   - Expiration: 30 days
   - Repository access: Select the repository you are working on
   - Permissions: 
      - Contents: Read and write
      - Issues: Read and write
      - Pull requests: Read and write
      - Metadata: Read-only

3. Select Generate token twice and copy the generated token to your clipboard

4. In VS Code, open the GitHub MCP server configuration JSON (as in Step 1.9) and modify the configuration:
```json
{
	"inputs": [
		{
			"id": "github-mcp-server-token",
			"type": "promptString",
			"description": "GitHub MCP Personal Access Token",
			"password": true
		}
	],
	"servers": {
		"io.github.github/github-mcp-server": {
			"type": "http",
			"url": "https://api.githubcopilot.com/mcp/",
			"gallery": "https://api.mcp.github.com",
			"version": "1.0.4",
			"headers": {
				"Authorization": "Bearer ${input:github-mcp-server-token}"
			}
		}
	}
}
```

5. Close VS Code and reopen it.

6. Head back to the MCP Servers tab and start the GitHub MCP server again. 

7. You will be prompted to enter the Personal Access Token. Paste the token you copied from GitHub and select Enter.


#### Step 3: Search Similar Repositories

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

#### Step 4: Create Issues for Missing Capabilities

1. Ask Copilot to create issues for interesting features:
   ```
   Use GitHub MCP to create well-documented issues for the top 3 most valuable missing capabilities. Each issue should include:
   - Clear descriptive title
   - Description of the feature
   - Why it's valuable (business benefit)
   - Reference to the repository where you found it
   - Basic acceptance criteria
   ```
   > Note: You would be requested to Allow the creation of the issues on GitHub.com. Select Allow in this session for each issue.

2. Verify the issues were created on GitHub.com

3. Open one of the issues and review the content generated by Copilot.

#### Step 5: Define and Create Low Inventory Check Issue

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

7. Refresh the issue page. Scroll down to the bottom. Copilot will link a Pull Request that will close the issue once the implementation is complete. The Pull Request will have a title like "[WIP] Implement Low Inventory Check System"

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

2. Open the Pull Request created by Copilot

3. Note the following:
   - Copilot would create a checklist in the PR description based on the issue requirements. This helps you track what has been implemented and what is still pending.
   - Once the list is completed, the [WIP] tag in the PR title will be removed, indicating the agent has completed its implementation
   - At the top you see a message "Copilot requested your review on this pull request." Then a button "Add your review" will be available for you to click and provide feedback

4. The description of the PR would be modified by Copilot to include:
   - **Approach explanation** - How the feature was implemented
   - **Code changes** - Key changes made to the codebase
   - **Testing strategy** - What tests were added and why
   - **Questions** - Any clarifications needed from you

5. Review the **Files changed** tab. You should see changes across multiple files.

#### Step 2: Request Improvements

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

1. An eyes emoji will appear at the bottom of your comment, indicating that Copilot will read it

2. At the bottom of your comment, a message will indicate that Copilot will start working on your request. Select View session to see the agent working on your feedback in real-time!

3. Go to the **Commits** tab to review the new commits

**Expected Outcome:**
- Understand cloud agent's implementation approach
- Copilot responds to feedback and makes improvements
- Iterative collaboration with AI agent

---

### Exercise 2.3: Test Cloud Agent Implementation (10 min)

#### Step 1: Review the Pull Request in VS Code

1. Open a new terminal and fetch the PR locally:
```bash
git fetch origin pull/{PR_NUMBER}/head:low-inventory-check
git checkout low-inventory-check
```
> Note: Replace `{PR_NUMBER}` with the actual PR number from GitHub.

2. Test in browser and verify acceptance criteria from original issue:
   - ✓ Visual indicators for low-stock items
   - ✓ Filter to show only low-stock products
   - ✓ Dashboard widget with count
   - ✓ Backend endpoint for low-stock items
   - ✓ Configurable thresholds
   - ✓ Changes in the threshold in the backend should reflect in the frontend without redeploying

3. Run and verify tests:
   ```bash
   pytest tests/test_inventory.py -v -k "low"
   ```

4. If there are any frontend or backend issues, have GitHub Copilot fix them. Use Playwright MCP to automate testing of the UI issues.

5. Once all issues are resolved and tests are passing, move your changes to the PR branch:
```bash
git add .
git commit -m "fix: address review feedback and test failures"
git push origin low-inventory-check
```
> Note: Do this only when you have made some fixes to the code. 

6. Checkout back to main branch:
```bash
git checkout main
```
7. Go back to the PR on GitHub.com and verify that your commits are reflected in the PR.

#### Step 2: Merge the Pull Request

1. Scroll down to the bottom of the PR and click **"Ready for review"**.

2. Click **Approve** → **Merge Pull Request** → **Confirm merge**

3. Delete the branch after merging (GitHub will offer this option)

4. Check that the issue is automatically closed

5. Go back to your VS Code terminal and pull the latest changes:
```bash
git pull origin main
```

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

1. Exit the interactive CLI session (type `exit` or press Ctrl+C).

2. Now demonstrate headless mode with the `-p` flag for programmatic commands. Try asking Copilot to summarize today's commits:

```bash
copilot -p "Summarize today's commits" --allow-all-tools
```
> --allow-all-tools is a permission flag that tells Copilot CLI it can use any available tool without asking for approval.

This runs Copilot in **non-interactive programmatic mode**:
- No approval prompts
- Direct output
- Perfect for automation scripts
- Can be used in CI/CD pipelines

3. Get the list of all database models and their fields:
```bash
copilot -sp "List all database models and their fields"
```
> The `-s` flag (silent) suppresses usage information — only outputs the result.
> Try the same command without `-s` to see the difference in output.

4. Try another example - list all API endpoints defined in the project and save to a file:
```bash
copilot -sp "List all API endpoints defined in this project" > endpoints.txt
```

5. Use Copilot CLI to run the pytest suite and save results to a file:
```bash
copilot -sp "Run pytest and output results" --allow-all-tools > test_results.txt
```   

6. Determine if you have Pull Requests that need review:
```bash
copilot -sp "Do I have any open Pull Requests?"
```   

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
/chronicle standup
```

This shows:
- Timeline of your session
- Commands executed
- Files modified
- Key decisions made
- **Perfect for standup reports!**

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
- Any advantages over VS Code approach

---

### Comparison Summary

**Which approach was faster?**  
**Which produced better code?**  
**When would you use each?**

Key Insights:
- **VS Code Copilot Chat** is better for: Visual code review, learning, complex refactoring
- **Copilot CLI** is better for: Terminal workflows, automation, git operations
- **Both** work well for: Feature implementation, code generation

---

## Lab Completion Checklist

Review your accomplishments:

### Part 1: MCP Integration
- [ ] Installed GitHub MCP server from registry
- [ ] Authenticated GitHub MCP server via browser OAuth
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

### Optional Capstone Challenge
- [ ] Implemented batch traceability with VS Code Copilot Chat
- [ ] Implemented batch traceability with Copilot CLI plan mode
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

**Core Workflow Learned:**
1. Use MCP to research and create feature requirements (GitHub issue)
2. Assign issue to Copilot cloud agent for autonomous implementation
3. Use CLI for terminal-native development and complete features with plan mode
4. Automate git workflows and testing from the command line

---


**Congratulations on completing the Advanced GitHub Copilot Lab!** 🎉

You now have the skills to leverage Copilot's most advanced capabilities across multiple surfaces (IDE, CLI, Cloud) and extend it with external tools via MCP.
