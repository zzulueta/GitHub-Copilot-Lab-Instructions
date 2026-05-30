# GitHub Copilot: Custom Instructions, Prompts, and Agent Control
**Duration:** 1 hour  
**Level:** Intermediate

---

## Pre-Lab Setup

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

This lab teaches you how to **customize and steer GitHub Copilot** to match your workflow and preferences. You'll learn to create persistent instructions, build reusable command templates, and control how Copilot interacts with your code through context and permissions.

### What You'll Learn
- [ ] Create Custom Instructions to persist coding preferences across all work
- [ ] Build file-based instructions that apply to specific patterns
- [ ] Build reusable Custom Prompts for repetitive tasks
- [ ] Invoke prompts with slash commands for instant productivity
- [ ] Steer Copilot with precise context using #file, #codebase, and #selection
- [ ] Configure which tools Copilot can use in Agent mode
- [ ] Understand approval modes: default, bypass, and autopilot concepts

### Why This Matters

**Custom Instructions** = "Always follow these patterns"
- Reduces repetitive context in every prompt
- Ensures consistency across all generated code
- Shares team standards automatically

**Custom Prompts** = "Execute this workflow when I ask"
- Eliminates retyping complex prompts
- Standardizes common development tasks
- Creates sharable command libraries

**Steering with Context & Permissions** = "Work with exactly what I need"
- Precise control over what Copilot sees
- Manage which tools Copilot can use
- Choose between manual control and automation

---

## Part 1: Custom Instructions

### Introduction: Making Your Preferences Persistent

Custom Instructions are markdown files that provide **persistent context** to Copilot. They apply automatically without you mentioning them in every prompt—like having a team member who always remembers your coding standards.

**Two types of instruction files:**

1. **Always-on instructions** - Apply to ALL chat requests in the workspace
   - `.github/copilot-instructions.md` - Workspace-wide coding standards

2. **File-based instructions** - Apply conditionally based on file patterns
   - `*.instructions.md` files stored in `.github/instructions/` folder
   - Use YAML frontmatter with `applyTo` glob patterns to specify when they apply

**Key Concepts:**
- Instructions are combined from multiple sources (workspace, user profile, organization)
- Higher priority: User > Workspace > Organization
- You can also create user-level instructions that apply across all workspaces

---

### Exercise 1.1: Create Always-On Workspace Instructions

**Task:** Define project-wide coding standards that apply to ALL Copilot interactions

1. **Create the instructions file:**
   
   Create a new directory `.github` in your workspace root. Then create a file named `copilot-instructions.md` inside the `.github` folder.
   
2. **Add comprehensive project instructions:**
   
   Copy this content then **SAVE** the file:
   ```markdown
   # Steel Inventory API - Copilot Instructions

   ## Python Code Style
   - Always include type hints for function parameters and return values
   - Write comprehensive docstrings using Google style format
   - Use descriptive variable names (e.g., `steel_grade` not `sg`)
   - Prefer explicit over implicit (PEP 20)

   ## FastAPI Patterns
   - Use dependency injection for database connections
   - Include comprehensive OpenAPI documentation in docstrings
   - Return appropriate HTTP status codes (201 for create, 204 for delete)
   - Use Pydantic models for all request/response bodies
   - Add response_model to all endpoints

   ## Error Handling
   - Raise HTTPException with descriptive messages
   - Include relevant context in error details
   - Use appropriate status codes (400 for validation, 404 for not found, 422 for invalid data)
   - Always validate input data

   ## Testing Conventions
   - Use pytest for all tests
   - Name test functions descriptively: test_<action>_<condition>_<expected_result>
   - Use parametrize for multiple test cases
   - Include docstrings explaining what each test verifies
   - Aim for comprehensive edge case coverage

   ## Steel Industry Domain
   - Valid steel shapes: sheet, plate, coil, bar, tube
   - Common grades: A36, A572, 304, 316, 316L, 4140, 4340
   - Measurements: thickness in mm, width/length in mm, weight in kg
   - Standard density for steel: 7850 kg/m³
   ```

3. **Test the instructions:**
   
   Open Copilot Chat in Ask mode and try:
   ```
   Create a new function to validate steel thickness. It should accept thickness in mm 
   and ensure it's between 0.1 and 500mm.
   ```
   
   - Observe: Copilot should automatically include type hints, docstrings, and raise appropriate errors
   - You didn't mention "add type hints" or "include docstrings" - Copilot applied these patterns automatically!

**Expected Outcome:**
- `.github/copilot-instructions.md` file created
- Understanding that always-on instructions apply to ALL Copilot interactions
- See automatic application of coding standards without explicit prompting

---

### Exercise 1.2: Create File-Based Instructions with Patterns

**Task:** Create specialized instructions that apply only to specific file types or locations

**Important:** File-based instructions use **YAML frontmatter** with an `applyTo` property to define glob patterns.

1. **Create the instructions folder:**
   
   Create directory: `.github/instructions/` in your workspace root

2. **Create router-specific instructions:**
   
   Create file: `.github/instructions/router-conventions.instructions.md`
   
   Copy this content then SAVE the file:
   ```markdown
   ---
   name: 'API Router Conventions'
   description: 'Coding conventions for FastAPI router files'
   applyTo: '**/routers/**/*.py'
   ---
   # API Router Instructions

   ## Endpoint Structure
   - Use router prefix for organization (e.g., router = APIRouter(prefix="/inventory"))
   - Group related endpoints together
   - Use consistent path parameters: {id} for single resources

   ## Response Patterns
   - GET collection: Return List[Model], 200 status
   - GET single: Return Model, 200 status, 404 if not found
   - POST create: Return created Model, 201 status
   - PUT/PATCH update: Return updated Model, 200 status
   - DELETE: Return 204 No Content

   ## Documentation
   - Every endpoint must have summary and description
   - Include example responses in docstrings
   - Document all possible HTTP status codes
   - Add tags for API grouping

   ## Validation
   - Validate all path and query parameters
   - Use Pydantic models for request bodies
   - Check resource existence before operations
   - Return meaningful error messages
   ```

3. **Create test-specific instructions:**
   
   Create file: `.github/instructions/test-conventions.instructions.md`
   
   Copy this content then SAVE the file:
   ```markdown
   ---
   name: 'Test Conventions'
   description: 'Standards for pytest test files'
   applyTo: '**/tests/**/*.py'
   ---
   # Test Instructions

   ## Test Organization
   - Group related tests in classes when appropriate
   - Use descriptive test names that explain the scenario
   - One assertion per test when possible

   ## Test Coverage Requirements
   - Test happy path (valid inputs, expected success)
   - Test edge cases (boundary values, empty inputs)
   - Test error conditions (invalid inputs, not found)
   - Test business logic validation

   ## Test Structure (Arrange-Act-Assert)
   - Arrange: Set up test data and dependencies
   - Act: Call the function/endpoint being tested
   - Assert: Verify expected outcomes

   ## FastAPI Testing
   - Use TestClient for endpoint testing
   - Verify both status codes AND response body content
   - Test request validation (missing fields, invalid types)
   - Ensure test isolation (each test independent)

   ## Parametrized Tests
   - Use pytest.mark.parametrize for multiple similar cases
   - Include test IDs for clarity: @pytest.mark.parametrize(..., ids=["case1", "case2"])
   - Cover all code paths with parameters
   ```

4. **Understanding the frontmatter:**
   - `name`: Display name shown in VS Code UI
   - `description`: Short description (shown on hover)
   - `applyTo`: Glob pattern defining when instructions apply
     - `**/routers/**/*.py` = All Python files in any `routers` folder
     - `**/tests/**/*.py` = All Python files in any `tests` folder
     - `**/*.py` = All Python files in workspace

5. **Test pattern-based instructions:**
   
   Open `steel-inventory-api/app/routers/inventory.py`.
   Select **Agent mode** and try:
   ```
   Create a new endpoint to get products by location.
   ```
   Review the generated code - it should follow the router conventions from your instructions!
   Undo the changes to keep the file clean.

   Open `steel-inventory-api/tests/test_inventory.py`.
   Select **Agent mode** and try:
   ```
   Create a test for the delete product endpoint.
   ```
   Review the generated test - it should follow the test conventions!
   Undo the changes after reviewing.

   - Notice how Copilot applies different patterns based on which file matches the `applyTo` pattern!

**Expected Outcome:**
- Two `.instructions.md` files created in `.github/instructions/` folder
- Understanding of YAML frontmatter and `applyTo` patterns
- See pattern-based application of instructions based on file context

---

### Exercise 1.3: Use the Agent Customizations Editor 

**Task:** Discover and manage instructions through the VS Code UI

1. **Open the Agent Customizations editor:**
   
   - Click the **gear icon** in the Chat window toolbar
   - Select **Instructions** tab

2. **Explore the instructions list:**
   
   You should see:
   - ✅ Copilot Instructions (always-on, from `.github/copilot-instructions.md`)
   - ✅ API Router Conventions (applies to `**/routers/**/*.py`)
   - ✅ Test Conventions (applies to `**/tests/**/*.py`)
   
   - Hover over each to see the description
   - Note the source location (workspace)

3. **Generate instructions with AI (Optional):**
   
   In Copilot Chat **Agent** mode, try the slash command:
   ```
   /create-instruction Always use pathlib.Path instead of os.path for file operations in Python
   ```
   
   - Copilot will ask clarifying questions
   - It will generate an `.instructions.md` file with appropriate `applyTo` pattern
   - Review and save the generated file

**Expected Outcome:**
- Familiarity with the Agent Customizations editor
- Know how to view and manage instructions
- Understanding of `/create-instruction` command

---

### Exercise 1.4: Instructions in Practice

**Task:** Experience the power of "set it and forget it" context

1. **Generate code with minimal prompting:**
   
   In Ask mode, give a brief high-level request:
   ```
   #file:inventory.py Add an endpoint to search products by steel grade.
   ```
   
   - No mention of type hints, docstrings, error handling, or response patterns
   - Copilot should include ALL best practices from your instructions

2. **Compare the difference:**
   
   Notice what you **didn't** have to specify:
   - ✅ Type hints added automatically
   - ✅ Comprehensive docstring included
   - ✅ Proper HTTP status codes used
   - ✅ Response model defined
   - ✅ Error handling included
   - ✅ Router patterns followed

3. **Key Insight:**
   
   Custom Instructions dramatically reduce prompt length while improving output quality. They're especially valuable for:
   - Team consistency
   - Onboarding new developers
   - Maintaining coding standards
   - Domain-specific patterns

**Expected Outcome:**
- Confidence using minimal prompts with instructions
- Understanding the productivity boost from persistent context
- Recognition of when to create instructions for your team

---

## Part 2: Custom Prompts

### Introduction: Reusable Command Templates

Custom Prompts (also called slash commands) are **reusable templates** for common tasks stored as individual `.prompt.md` files. Unlike instructions (passive), prompts are **actively invoked** when you need them.

**Use Custom Prompts for:**
- Repetitive tasks with specific requirements
- Multi-step workflows you execute frequently
- Team-standard procedures
- Complex prompts you don't want to retype

**Prompt File Format:**
- Each prompt is a **separate `.prompt.md` file**
- Stored in `.github/prompts/` folder (or configured location)
- Uses YAML frontmatter for configuration
- Invoked with `/` slash commands in chat (e.g., `/generate-tests`)

**Key Concepts:**
- Prompt files can specify which agent mode to use (`ask`, `agent`, `plan`)
- Can include tool restrictions and model selection
- Support variables and user input with `${input:variableName}` syntax
- Can reference instructions and other workspace files

---

### Exercise 2.1: Create Prompt Files for Common Tasks

**Task:** Build a library of prompt files for development tasks

**Important:** Each prompt is a **separate file** with `.prompt.md` extension.

1. **Create the prompts folder:**
   
   Create directory: `.github/prompts/` in your workspace root
   
2. **Create "Generate Tests" prompt:**
   
   Create file: `.github/prompts/generate-tests.prompt.md`
   
   Copy this content then SAVE the file:
   ```markdown
   ---
   name: generate-tests
   description: Generate comprehensive pytest tests for selected code
   argument-hint: Select a function or endpoint first
   agent: agent
   model: Claude Sonnet 4.5 (copilot)
   tools: [read, edit, search]
   ---
   Analyze the selected code and generate comprehensive pytest tests.

   Requirements:
   - Test happy path with valid inputs
   - Test edge cases (boundary values, empty inputs, None values)
   - Test error conditions (invalid inputs, exceptions)
   - Use pytest.mark.parametrize for multiple similar test cases
   - Include descriptive test names and docstrings
   - Verify both functionality AND error messages
   - Add fixtures if needed for test data setup

   Return the complete test code ready to add to the test file.
   ```

3. **Create "Add Error Handling" prompt:**
   
   Create file: `.github/prompts/add-error-handling.prompt.md`
   
   Copy this content then SAVE the file:
   ```markdown
   ---
   name: add-error-handling
   description: Add comprehensive error handling to selected code
   argument-hint: Select the code to enhance
   agent: agent
   model: Claude Sonnet 4.5 (copilot)
   tools: [read, edit, search]
   ---
   Enhance the selected code with comprehensive error handling.

   Requirements:
   - Identify all possible error conditions
   - Add try-except blocks where appropriate
   - Raise HTTPException with descriptive messages
   - Include relevant context in error details
   - Use appropriate HTTP status codes (400, 404, 422, 500)
   - Add logging for errors
   - Handle edge cases (None, empty, invalid types)
   - Maintain existing functionality

   Return the enhanced code with all error handling added.
   ```

4. **Create "Document API Endpoint" prompt:**
   
   Create file: `.github/prompts/document-endpoint.prompt.md`
   
   Copy this content then SAVE the file:
   ```markdown
   ---
   name: document-endpoint
   description: Add comprehensive OpenAPI docs to an endpoint
   argument-hint: Select an API endpoint function
   agent: agent
   model: Claude Sonnet 4.5 (copilot)
   tools: [read, edit, search]
   ---
   Add comprehensive OpenAPI documentation to the selected FastAPI endpoint.

   Requirements:
   - Add summary (one line description)
   - Add detailed description explaining purpose and behavior
   - Document all parameters (path, query, body)
   - Document all possible response status codes
   - Include example request/response bodies in docstring
   - Add response_model if not present
   - Include notes about validation or business rules
   - Add tags for API organization

   Return the endpoint with complete documentation.
   ```

5. **Understanding the frontmatter:**
   - `name`: Command name after `/` (e.g., `/generate-tests`)
   - `description`: Short description shown in prompt picker
   - `argument-hint`: Hint text shown in chat input
   - `agent`: Which mode to use (`ask`, `agent`, or `plan`)
   - `model`: (Optional) Specific model to use
   - `tools`: (Optional) Restrict which tools are available

**Expected Outcome:**
- Three `.prompt.md` files created in `.github/prompts/` folder
- Understanding of proper prompt file format with YAML frontmatter
- Recognition of when to create custom prompts

---

### Exercise 2.2: Use Prompt Files with Slash Commands

**Task:** Invoke and use your custom prompt files

1. **Use the "Generate Tests" prompt:**
   
   - Open `steel-inventory-api/app/utils/steel_utils.py`
   - Select the `calculate_weight_kg` function
   - Open Copilot Chat
   - Type `/generate-tests` and press Enter
   - Copilot will generate comprehensive tests following your prompt template
   - Review the generated tests - should be comprehensive and follow test instructions

2. **Use the "Add Error Handling" prompt:**
   
   - Open `steel-inventory-api/app/routers/inventory.py`
   - Select a function that needs better error handling (e.g., `get_product`)
   - In Copilot Chat, type `/add-error-handling` and press Enter
   - Observe how it adds validation, error messages, and proper status codes

3. **Use the "Document Endpoint" prompt:**
   
   - Still in `steel-inventory-api/app/routers/inventory.py`
   - Select an endpoint function
   - Type `/document-endpoint` in chat
   - See comprehensive OpenAPI documentation added

4. **Alternative invocation methods:**
   
   - **Type `/` in chat:** See all available prompts listed with autocomplete
   - **Command Palette:** Press `Ctrl+Shift+P` → "Chat: Run Prompt" → Select prompt
   - **Prompt file editor:** Open any `.prompt.md` file → Click play button in title bar to test it

5. **Observe the synergy:**
   
   Notice how **Custom Prompts + Custom Instructions work together**:
   - Prompt defines WHAT to do (generate tests, add error handling)
   - Instructions define HOW to do it (code style, patterns from `.instructions.md`)
   - Result: Consistent, high-quality output every time

**Expected Outcome:**
- Comfortable using `/` slash commands to invoke prompts
- See how prompts + instructions complement each other
- Experience faster execution of common tasks
- Understanding of multiple invocation methods

---

### Exercise 2.3: Use the Agent Customizations Editor for Prompts

**Task:** Manage prompt files through the VS Code UI

1. **Open the Agent Customizations editor:**
   
   - Click the **gear icon** in the Chat window toolbar
   - Select **Prompts** tab

2. **Explore the prompts list:**
   
   You should see:
   - ✅ generate-tests
   - ✅ add-error-handling
   - ✅ document-endpoint
   
   - Hover over each to see the description
   - Note the source location (workspace)
   - Click to open and edit

3. **Test a prompt directly from the editor:**
   
   - Open `.github/prompts/generate-tests.prompt.md`
   - Click the **play button** (▶) in the editor title bar
   - Choose "Run in current chat" or "Run in new chat"
   - This is useful for testing prompts as you develop them

4. **Generate a new prompt with AI (Optional):**
   
   In Copilot Chat **Agent** mode, try the slash command:
   ```
   /create-prompt A prompt that adds pagination to FastAPI endpoints with limit and offset parameters
   ```
   
   - Copilot will ask clarifying questions
   - It will generate a `.prompt.md` file with appropriate frontmatter
   - Review and save the generated file

**Expected Outcome:**
- Familiarity with the Agent Customizations editor for prompts
- Know how to test prompts with the play button
- Understanding of `/create-prompt` command

---

### Exercise 2.4: Instructions vs Prompts Decision Framework

**Task:** Understand when to use each customization type

**Decision Framework:**

Use **Instructions** when:
- ✅ Should apply to ALL code (coding style, patterns)
- ✅ Background context that's always relevant
- ✅ "Always do it this way"
- ✅ Team-wide standards
- ✅ Example: "Always use type hints" or "Follow FastAPI patterns"

Use **Prompts** when:
- ✅ Specific task you invoke intentionally
- ✅ Multi-step workflow with clear steps
- ✅ "Do this when I ask"
- ✅ Repetitive but not universal
- ✅ Example: "Generate tests" or "Add error handling"

Use **Both** when:
- ✅ Prompt defines the task, instructions define the style
- ✅ Common tasks that should follow team standards
- ✅ Example: `/generate-tests` (prompt) uses test conventions (instructions)

**Quick Examples:**

| Scenario | Instructions | Prompts | Both |
|----------|-------------|---------|------|
| Code style (type hints, docstrings) | ✅ | | |
| API patterns (status codes, validation) | ✅ | | |
| Generate tests for function | | | ✅ |
| Add error handling to code | | ✅ | |
| Document API endpoint | | ✅ | |
| Steel industry domain knowledge | ✅ | | |

**Expected Outcome:**
- Strategic thinking about when to create instructions vs prompts
- Understanding the complementary nature of both features
- Confidence in building your customization library

---

## Part 3: Steering Copilot with Context and Permissions

### Introduction: Precise Control Over Copilot

Steering Copilot means giving it exactly the right context and controlling what actions it can take. This part teaches you three ways to steer:

1. **Context Steering** - Control what Copilot sees
2. **Tool Configuration** - Control what Copilot can do
3. **Approval Modes** - Control when Copilot needs permission

---

### Exercise 3.1: Context Steering Methods

**Task:** Master the different ways to provide context to Copilot

Context is how you tell Copilot what to focus on. The more precise your context, the better the results.

#### Method 1: Using #file

1. Open Copilot Chat in Ask mode
2. Select specific files to include:
   ```
   #file:models.py Explain the SteelProduct class
   ```

3. Try with multiple files:
   ```
   #file:models.py #file:database.py How are products stored and retrieved?
   ```

**When to use:** Questions about specific files (up to 5-10 files)

#### Method 2: Using #codebase

1. For questions about the entire project:
   ```
   #codebase What endpoints are available in this API?
   ```

2. Search within codebase:
   ```
   #codebase Where is the database connection configured?
   ```

**When to use:** Searching across the entire project, understanding architecture

#### Method 3: Using #selection

1. Open `app/utils/steel_utils.py`
2. Select the entire `calculate_weight_kg` function
3. In chat, type:
   ```
   #selection Explain this function step by step
   ```

**When to use:** Focused questions about specific code blocks

#### Method 4: Drag and Drop

1. From the file explorer, drag `app/routers/inventory.py` into Copilot Chat
2. Ask: 
   ```
   What does this file do?
   ```

3. Drag the entire `routers` folder:
   ```
   What routes are defined in this folder?
   ```

**When to use:** Quick way to add context visually

#### Context Best Practices

**Be Specific:**
- ❌ "Fix the bug" (no context)
- ✅ `#file:inventory.py` "Fix the validation bug in create_product endpoint"

**Use Multiple Context Methods:**
```
#file:models.py #selection Refactor this class to match the patterns in models.py
```

**Minimize Context for Focus:**
- Close unnecessary files
- Use `#file` instead of `#codebase` when possible
- Select specific code blocks rather than whole files

**Expected Outcome:**
- Comfortable using #file, #codebase, #selection, drag-drop
- Know when to use each method
- Understand how context affects response quality

---

### Exercise 3.2: Tool Configuration in Agent Mode

**Task:** Learn to control which tools Copilot can use in Agent mode

Agent mode can use various tools to complete tasks (reading files, editing code, searching, running terminal commands, etc.). You can configure which tools are available.

#### Understanding Copilot Tools

**Common tools available:**
- **read** - Read files and folders
- **edit** - Modify existing files
- **search** - Search across the codebase
- **createFile** - Create new files
- **execute** - Run terminal commands
- **web** - Fetch information from the web (via MCP servers)
- **agent** - Invoke other custom agents

#### Step 1: View Available Tools

1. Open Copilot Chat and switch to **Agent** mode
2. Select **Configure Tools** beside the model selector
3. You'll see which tools are currently available to Agent mode

#### Step 2: Try Restricted vs Unrestricted

1. **Unrestricted Agent (all tools):**

   Enter the following prompt:
   ```
   #file:inventory.py Add logging to all CRUD operations
   ```
   - Agent can read AND edit the file
   - Changes applied automatically
   - Undo the changes after reviewing to keep the file clean

2. **Restricted Agent (read-only):**
   
   Remove all tools except "read" and "search" from the tool configuration for Agent mode. Then enter the same prompt but with tool restrictions:
   ```
   #file:inventory.py Add logging to all CRUD operations
   ```
   - Agent can only read and analyze
   - Provides recommendations without editing

3. **Compare the difference:**
   - Unrestricted agent made changes directly
   - Restricted agent provided analysis and recommendations without changing code

4. Configure the tools back to the original settings after testing.

#### Step 3: Restricting Tools in Prompts

When you create custom prompts, you can restrict which tools they can use. We demonstrated this in Exercise 2.1 when we created the prompt files. Here's a reminder of how to specify tools in the prompt frontmatter:

```markdown
---
name: read-only-analysis
description: Analyze code without making changes
agent: agent
tools: [read, search]  # Only allows reading and searching, no editing
---
Analyze the codebase for potential improvements.
Do not make any changes, only provide recommendations.
```

**Why restrict tools?**
- Safety: Prevent unintended file modifications
- Focus: Force analysis-only mode
- Control: Ensure specific workflows

#### Step 4: Tool Configuration in Custom Agents

Custom agents (`.agent.md` files) can also specify tools. We can create a custom agent that acts as a code reviewer, only allowed to read and search.

1. Create a directory `.github/agents/` and a file `code-reviewer.agent.md` with the following content:

```markdown
---
name: code-reviewer
description: Reviews code without making changes
tools: [read, search]  # No edit tool
model: Claude Sonnet 4.5
---
Review code for issues and provide recommendations.
Do not modify any files.
```

2. In Copilot Chat, select the `code-reviewer` agent and give it a prompt:
```
Review the create_product endpoint for potential improvements.
```   
- The agent will analyze the code and provide feedback without making changes

**Expected Outcome:**
- Understanding of available Copilot tools
- Know how to restrict tools in prompts
- Recognize when to limit tools for safety or focus
- Ability to configure tools in custom prompts and agents

---

### Exercise 3.3: Understanding Permission Levels

**Task:** Learn how to control agent autonomy using the three permission levels in VS Code

When Copilot Agent wants to take actions (edit files, run commands, access external services), you control how much autonomy it has through the **permissions dropdown** in the chat input area. VS Code provides three permission levels that determine how tool calls and approvals are handled.

#### Three Permission Levels

You'll find the **permissions dropdown** in the Chat view, next to the chat input field (look for the shield icon or dropdown selector). The permission level applies to the current chat session and can be changed at any time.

**1. Default Approvals** (Manual control per action)
- Uses your configured approval settings
- Tools that require approval show a confirmation dialog before they run
- You review and approve each action: "Allow", "Deny", or "Allow in this session"
- The agent might ask clarifying questions if needed
- **Use when:** First time using Agent mode, working with critical files, learning what actions Copilot takes
- **Default setting:** New chat sessions start with this level by default

**2. Bypass Approvals** (Auto-approve with manual intervention option)
- Auto-approves all tool calls without showing confirmation dialogs
- Automatically retries on errors
- The agent might still ask clarifying questions if needed
- You can stop the agent at any time by clicking the stop button
- **Use when:** Performing repetitive trusted tasks, working in a safe environment with version control
- ⚠️ **Security Warning:** Bypasses manual approval prompts, including for potentially destructive actions like file edits and terminal commands. Only use if you understand the security implications.

**3. Autopilot (Preview)** (Fully autonomous operation)
- Auto-approves all tool calls without confirmation dialogs
- **Auto-responds to clarifying questions** (key difference from Bypass Approvals)
- Continues working autonomously until it determines the task is complete (continuous iteration)
- Automatically retries when it encounters errors
- Consumes multiple premium requests as the agent continues working
- **Use when:** Fully automated workflows, well-scoped tasks, trusted operations
- ⚠️ **Security Warning:** Removes all manual intervention points. The agent works completely autonomously. Only use in controlled environments.

> **Tip:** To persist your preferred permission level across sessions, configure the `chat.permissions.default` setting in VS Code.

---

#### The Test Prompt

For the following steps, you'll use the **same prompt** with each permission level to directly compare their behavior:

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
- Write comprehensive tests to verify calculation accuracy for each shape and grade
- Run the tests and ensure they pass
```

This prompt involves multiple tool calls: file editing (endpoint creation), file editing (test creation), and terminal command (running tests). You'll observe how each permission level handles these actions differently.

---

#### Step 1: Experience Default Approvals

1. Open Copilot Chat and switch to **Plan** mode. Start a New Chat session.
2. Locate the **permissions dropdown** in the chat input area
3. Ensure **"Default Approvals"** is selected (this is the default)
4. Enter the test prompt:
   ```
   Enter the test prompt from above
   ```
5. **Observe that it asks for clarifying questions**. Select any of the choices to proceed with the Plan mode flow. 
6. Once Plan mode completes, go to Agent mode and prompt:
   ```
   Execute the plan.
   ```
   - A pytest approval dialog appears before running tests
   - Select "Allow" to approve the action
   - The agent runs the tests and shows results in the terminal
   
7. **After completion, UNDO the changes:**
   - Select **Undo** to undo the file edits
   
**What you learned:**
- You reviewed and approved each individual action
- Full visibility into what the agent is doing
- Complete control over which actions proceed

---

#### Step 2: Experience Bypass Approvals

1. Ensure you've undone all changes from Step 1 (check that the files are back to original state). Create a New Chat session.
2. In Copilot Chat (select in **Plan** mode), click the **permissions dropdown**
3. Select **"Bypass Approvals"** from the dropdown
4. You'll see a confirmation warning about bypassing approvals - click to Enable
5. Enter the **same test prompt**:
   ```
   Enter the test prompt from above
   ```
6. **Observe that it asks for clarifying questions**. Select any of the choices to proceed with the Plan mode flow. 

7. Once Plan mode completes, go to Agent mode and prompt:
   ```
   Execute the plan.
   ```
   - No approval dialogs appear!
   - Copilot automatically edits files and runs terminal commands
   - Actions happen in rapid succession without stopping
   - The agent may still ask you clarifying questions if needed
   - You can click the **stop button** at any time to halt execution

8. **After completion, UNDO the changes:**
   - Select **Undo** to undo the file edits

**What you learned:**
- Actions proceed automatically without individual approvals
- Faster workflow for trusted operations
- You can still stop execution at any time
- The agent may pause for clarifying questions

---

#### Step 3: Experience Autopilot (Preview)

1. Ensure you've undone all changes from Step 2 (check that files are back to original state). Start a New Chat session.
2. In Copilot Chat (select in **Plan** mode), click the **permissions dropdown**
3. Select **"Autopilot (Preview)"** from the dropdown
4. You'll see a confirmation warning about fully autonomous operation - click to confirm
5. Enter the **same test prompt**:
   ```
   Enter the test prompt from above
   ```
6. **Observe that it creates a plan without asking for clarifying questions**. 

7. Once Plan mode completes, go to Agent mode and prompt:
   ```
   Execute the plan.
   ```
   - You get the same experience as Bypass Approvals, but now the agent will also auto-respond to any clarifying questions without pausing for your input

**What you learned:**
- Completely autonomous agent operation
- Agent handles its own questions and continues until task completion
- Multiple iterations and self-correction possible
- Most powerful but least supervised mode

---

#### Step 4: Tool Approval Management (Optional Advanced)

Beyond permission levels, you can configure **which specific tools** are pre-approved or always require confirmation.

1. Open the Command Palette (Ctrl+Shift+P or Cmd+Shift+P)
2. Run: **"Chat: Manage Tool Approval"**
3. **Explore the tool approval interface:**
   - See all available tools grouped by source (MCP servers, extensions, built-in)
   - Notice two types of approvals for each tool:
     - **Pre-approval** ("without approval"): Skip confirmation dialog before tool runs
     - **Post-approval** ("without reviewing result"): Skip reviewing tool output before adding to context
   - You can trust all tools from a specific source, or configure individual tools
4. **Example configurations:**
   - Trust all file read operations without approval
   - Always require approval for terminal commands
   - Auto-approve web fetch tools but always review their results
5. **Close the tool approval interface** (don't make changes unless you want to experiment)

**What you learned:**
- Granular control over individual tool permissions
- Pre-approval vs. post-approval distinction
- How to trust tools from specific sources
- Additional layer of control beyond permission levels

---

#### Step 5: Choosing the Right Permission Level

Based on your hands-on experience, here's when to use each permission level:

| Situation | Recommended Level | Why |
|-----------|------------------|-----|
| First time using Agent mode | Default Approvals | Learn what actions Copilot takes, build trust |
| Refactoring critical code | Default Approvals | Review each change carefully before applying |
| Working with unfamiliar codebase | Default Approvals | Understand the agent's decisions before approving |
| Generating tests for multiple functions | Bypass Approvals | Avoid repetitive clicks for similar safe operations |
| Adding documentation to many files | Bypass Approvals | Streamline repetitive tasks with version control safety net |
| Complex multi-step task in safe environment | Autopilot (Preview) | Let agent work through iterations autonomously |
| Automated CI/CD workflows | Autopilot (Preview) | No human in the loop, fully automated |
| Experimenting with new features | Default Approvals | Stay in control while exploring capabilities |

**Key Decision Factors:**
- **Risk level:** Higher risk = Default Approvals
- **Repetitiveness:** Many similar actions = Bypass Approvals or Autopilot
- **Complexity:** Multi-step tasks requiring iteration = Autopilot
- **Trust level:** Building trust = Default Approvals; Established trust = Bypass/Autopilot
- **Environment:** Production or critical = Default; Development with version control = Bypass/Autopilot

**Expected Outcome:**
- Hands-on experience with all three permission levels using the same task
- Understanding of the permissions dropdown UI in VS Code
- Direct comparison of approval behaviors (manual, auto-approve, fully autonomous)
- Knowledge of when to use each permission level
- Awareness of tool approval management for granular control
- Confidence choosing the appropriate permission level for different scenarios

---

## Lab Summary

Congratulations! You've learned to customize and steer GitHub Copilot for maximum productivity.

### What You Accomplished

**Part 1: Custom Instructions**
- ✅ Created workspace-wide `.github/copilot-instructions.md` for persistent standards
- ✅ Built file-based instructions with `applyTo` patterns for targeted guidance
- ✅ Used Agent Customizations editor to manage instructions
- ✅ Experienced automatic application of coding standards

**Part 2: Custom Prompts**
- ✅ Created reusable `.prompt.md` files for common tasks
- ✅ Invoked prompts with `/` slash commands
- ✅ Configured prompts with agent modes and tool restrictions
- ✅ Understood when to use instructions vs prompts

**Part 3: Steering Copilot**
- ✅ Mastered context methods: #file, #codebase, #selection, drag-drop
- ✅ Configured which tools Copilot can use
- ✅ Experienced three approval modes: default, bypass, autopilot
- ✅ Combined all concepts in a complete workflow

### Key Takeaways

**Custom Instructions = Passive "Always Do This"**
- Automatically applied based on file patterns
- Reduces repetitive context in prompts
- Ensures consistency across all work

**Custom Prompts = Active "Execute This Workflow"**
- Explicitly invoked with slash commands
- Standardizes common development tasks
- Combines with instructions for best results

**Steering = Precise Control**
- Context determines what Copilot sees
- Tools determine what Copilot can do
- Approvals determine when Copilot needs permission

### The Synergy

These three concepts work together powerfully:

1. **Instructions** provide the "how" (coding standards)
2. **Prompts** provide the "what" (task templates)
3. **Steering** provides the "where" and "when" (control)

Example workflow:
```
/generate-tests [Prompt: what task]
  ↓
Uses test-conventions.instructions.md [Instructions: how to do it]
  ↓
With #file:utils.py [Context: where to look]
  ↓
tools: [read, edit] [Tools: what actions allowed]
  ↓
"Allow in this session" [Approval: when to proceed]
  ↓
= Consistent, efficient, controlled test generation!
```

---

## Next Steps

**Continue Building Your Customization Library:**
1. Add more instructions for your specific domain
2. Create prompts for your most common tasks
3. Share your `.github/` folder with your team
4. Refine based on what works in practice

**Explore Advanced Topics:**
- **MCP Servers** - Extend Copilot with external tools (GitHub, Playwright, databases)
- **Custom Agents** - Build specialized agents with handoffs
- **Agent Skills** - Create portable skill libraries following agentskills.io standard
- **Copilot CLI** - Terminal-native workflows with plan mode and programmatic execution

**Share With Your Team:**
- Instructions, prompts, and agents can be committed to your repository
- Team members automatically get the same standards
- Evolve your customizations together over time

---

## Completion Checklist

### Skills Mastered
- [ ] Create workspace-wide instructions with `.github/copilot-instructions.md`
- [ ] Create file-based instructions with `applyTo` patterns
- [ ] Use Agent Customizations editor to manage instructions
- [ ] Create custom prompts with YAML frontmatter
- [ ] Invoke prompts with `/` slash commands
- [ ] Configure agent modes and tools in prompts
- [ ] Use #file, #codebase, #selection for context steering
- [ ] Configure which tools Copilot can use
- [ ] Understand default, bypass, and autopilot approval modes
- [ ] Combine context, tools, and approvals in real workflows

### Deliverables Created
1. ✅ `.github/copilot-instructions.md` - Workspace standards
2. ✅ `.github/instructions/router-conventions.instructions.md` - API patterns
3. ✅ `.github/instructions/test-conventions.instructions.md` - Testing standards
4. ✅ `.github/prompts/generate-tests.prompt.md` - Test generation workflow
5. ✅ `.github/prompts/add-error-handling.prompt.md` - Error handling workflow
6. ✅ `.github/prompts/document-endpoint.prompt.md` - Documentation workflow

### Key Metrics
- **Instructions Created:** 3 (1 always-on, 2 file-based)
- **Prompts Created:** 3 reusable workflows
- **Context Methods Mastered:** 4 (#file, #codebase, #selection, drag-drop)
- **Approval Modes Understood:** 3 (default, bypass, autopilot)
- **Time Investment:** 60 minutes
- **Productivity Gain:** Permanent (instructions and prompts remain useful forever!)

---

**Well done!** You now have a customized GitHub Copilot setup tailored to your workflow and team standards. Keep building your library and sharing with others!
