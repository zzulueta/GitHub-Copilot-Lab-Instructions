# GitHub Copilot Intermediate Lab Instructions
**Duration:** 2.5 hours  
**Level:** Intermediate (Completed Basic Lab or equivalent experience)

---

## Prerequisites

**Before starting this lab, you should have:**
- ✅ Completed the GitHub Copilot Basic Lab (LAB_INSTRUCTIONS_BASIC.md)
- ✅ Understanding of Copilot's inline suggestions and Chat modes
- ✅ Familiarity with #file, #codebase, and context selection
- ✅ The steel-inventory-api application running successfully
- ✅ Basic understanding of FastAPI and Python testing

**If you haven't completed the basic lab**, review these key concepts:
- How to use Copilot Chat (Ask mode) vs. inline completions
- Providing context with #file, #codebase, and drag-drop
- Model selection (Claude Sonnet 4.5, GPT-4.1, etc.)
- Basic API testing in Swagger UI

---

## Running the Application

Before starting the lab exercises, ensure the steel inventory API is running:

### Step 1: Navigate to the API Directory
Open a terminal and change to the steel-inventory-api directory:
```bash
cd steel-inventory-api
```

### Step 2: Create and Activate a Virtual Environment
Create a virtual environment to isolate project dependencies:

**On Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**On macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

You should see `(venv)` appear in your terminal prompt, indicating the virtual environment is active.

### Step 3: Install Dependencies
Run the following command to install required Python packages:
```bash
pip install -r requirements.txt
```

### Step 4: Start the FastAPI Server
Run the following command to start the server:
```bash
uvicorn app.main:app --reload
```

The server should start on `http://localhost:8000`

### Step 5: Verify the Application is Running

- **Web Interface:** Open your browser to [http://localhost:8000](http://localhost:8000)
- **API Documentation:** Access the interactive Swagger UI at [http://localhost:8000/docs](http://localhost:8000/docs)
- **Health Check:** Visit [http://localhost:8000/health](http://localhost:8000/health) to confirm the API is responding

You should see the BlueScope Steel Inventory Management interface. Keep the server running in a terminal window throughout the lab exercises.

---

## Lab Overview

Welcome to the **Intermediate GitHub Copilot Lab**! You'll learn advanced techniques for using Copilot as a powerful development partner. This lab focuses on **advanced workflows, autonomous coding, and strategic problem-solving** with AI assistance.

Building on the steel inventory management system from the basic lab, you'll master techniques that professional developers use daily to maximize productivity with Copilot.

### What You'll Learn
- [ ] Understanding and using Plan mode and Agent mode effectively
- [ ] Advanced prompting strategies for better code generation
- [ ] Persisting your preferences with Custom Instructions and Prompts
- [ ] Creating reusable prompt templates for common tasks
- [ ] Automated test generation with Plan → Agent workflows
- [ ] Multi-file debugging with strategic analysis
- [ ] Safe refactoring with AI assistance
- [ ] Complex feature implementation using multiple modes

### Learning Objectives

By the end of this lab, you will:
1. Know when to use Plan mode vs. Agent mode vs. Ask mode
2. Write prompts that consistently produce high-quality code
3. Create Custom Instructions to persist coding preferences across all work
4. Build reusable Custom Prompts (slash commands) for repetitive tasks
5. Automate test generation using Plan → Agent workflows
6. Debug complex issues with strategic analysis before fixing
7. Refactor code safely with automated verification
8. Implement complex features by combining all Copilot modes

---

## Part 1: Understanding Copilot Modes (10 minutes)

### Introduction: Plan and Agent Modes

In the basic lab, you learned **Ask mode** - the conversational mode where Copilot generates code for you to review and apply. Now you'll learn two additional modes that enable more advanced workflows:

**Plan mode** - Strategic planning and research before coding  
**Agent mode** - Autonomous code execution and file modifications

Understanding when to use each mode is key to maximizing your productivity with Copilot.

---

### Exercise 1.1: Try Plan Mode (4 min)

**Task:** Experience how Plan mode helps design before coding

**Scenario:** You need to add filtering and pagination to the inventory endpoint.

1. **Switch to Plan mode:**
   
   - Open Copilot Chat
   - Select **Plan mode** from the mode selector dropdown

2. **Ask for a design plan:**
   
   In Plan mode:
   ```
   #file:inventory.py #file:models.py
   
   Plan how to add filtering and pagination to the GET /inventory/ endpoint.
   
   Filtering by:
   - grade (steel grade)
   - shape (product shape)
   - location (warehouse location)
   
   Pagination:
   - limit and offset parameters
   - default page size: 20
   - maximum page size: 100
   
   What changes are needed and in what order?
   ```

3. **Review Plan mode's response:**
   
   Notice how Plan mode:
   - ✅ Breaks down the task into steps
   - ✅ Identifies which files need changes
   - ✅ Suggests the implementation order
   - ✅ Considers edge cases and validation
   - ✅ Provides a roadmap WITHOUT writing code yet

4. **Key insight:**
   
   Plan mode doesn't write code - it helps you **think strategically** before coding. Use it when:
   - Designing complex features
   - Unfamiliar with the technology
   - Need to understand dependencies
   - Want to explore different approaches

**Expected Outcome:**
- Understanding of Plan mode's strategic planning capabilities
- See how it breaks down complex tasks
- Know when to use Plan mode

---

### Exercise 1.2: Try Agent Mode (4 min)

**Task:** Experience how Agent mode autonomously modifies files

**Scenario:** The functions in steel_utils.py need comprehensive docstrings.

1. **Switch to Agent mode:**
   
   - In Copilot Chat
   - Select **Agent mode** from the mode selector

2. **Give Agent an autonomous task:**
   
   In Agent mode:
   ```
   #file:steel_utils.py
   
   Add comprehensive Google-style docstrings to all functions in steel_utils.py.
   
   Include:
   - Function description
   - Args with types
   - Returns with type
   - Raises (if applicable)
   - Example usage
   ```

3. **Watch Agent work:**
   
   Notice how Agent mode:
   - ✅ Reads the file automatically
   - ✅ Makes decisions about what to document
   - ✅ Modifies the file directly
   - ✅ Shows you progress in real-time
   - ✅ Completes the task without further input

4. **Review the changes:**
   
   - Open `steel-inventory-api/app/utils/steel_utils.py`
   - See the added docstrings
   - Agent made the changes for you!

5. **Key insight:**
   
   Agent mode **executes autonomously** and modifies files. Use it when:
   - Requirements are clear
   - Task is routine (documentation, formatting, refactoring)
   - Multi-file changes needed
   - You want to review results, not guide every step

**Expected Outcome:**
- Understanding of Agent mode's autonomous execution
- See real-time file modifications
- Know when to use Agent mode
- Confidence that changes can be reviewed/undone

---

### Exercise 1.3: Mode Selection Guide (2 min)

**Task:** Learn when to use each mode

**The Three Modes:**

| Mode | Purpose | When to Use | Output |
|------|---------|-------------|--------|
| **Ask** | Generate code for review | Need to review before applying<br>Learning/exploring<br>Specific code snippets | Code in chat<br>You copy/paste |
| **Plan** | Strategic planning | Complex features<br>Unfamiliar technology<br>Need design first<br>Explore approaches | Plan/roadmap<br>No code yet |
| **Agent** | Autonomous execution | Clear requirements<br>Multi-file changes<br>Routine tasks<br>Implement a plan | Direct file edits<br>Autonomous work |

**Common Workflows:**

1. **Simple task:** Ask mode → Copy/paste
2. **Complex feature:** Plan mode → Review plan → Agent mode → Verify
3. **Routine task:** Agent mode → Review changes
4. **Learning:** Ask mode → Understand → Agent mode → Apply

**Decision Tree:**

```
Do you need to design/think first?
├─ YES → Use Plan mode
└─ NO → Do you want code to review or automatic changes?
    ├─ Review first → Use Ask mode
    └─ Automatic changes → Use Agent mode
```

**Throughout this lab:**
- You'll practice all three modes
- Learn which mode fits which scenario
- Build intuition for mode selection

**Expected Outcome:**
- Clear mental model of three modes
- Decision framework for choosing modes
- Ready to apply modes throughout the lab

---

## Part 2: Effective Prompting Strategies (15 minutes)

### Introduction: The Art of Prompting

The quality of Copilot's output depends heavily on how you ask. Intermediate users need to master **prompt engineering** to get consistent, high-quality results. This module teaches you strategies that separate basic Copilot usage from expert-level productivity.

**Key Principle:** Specific, constrained prompts with clear context produce better code than vague requests.

---

### Exercise 2.1: Vague vs. Specific Prompts (5 min)

**Task:** Learn how prompt specificity impacts code quality

1. **Start with a vague prompt:**
   
   Open Copilot Chat in Ask mode:
   ```
   #file:steel_utils.py Add validation for steel grades
   ```
   
   - Observe the response - likely generic validation
   - Note what assumptions Copilot makes

2. **Now try a specific, constrained prompt:**
   
   Start a new chat and try:
   ```
   #file:steel_utils.py I need to enhance the validate_grade function with these requirements:
   
   1. Accept steel grades: A36, A572, 304, 316, 316L, 4140, 4340
   2. Grades should be case-insensitive
   3. Return a tuple: (is_valid: bool, normalized_grade: str, grade_category: str)
   4. Categories: "Carbon Steel" for A-series, "Stainless Steel" for 300-series, "Alloy Steel" for 4000-series
   5. Raise ValueError with descriptive message for invalid grades
   6. Add comprehensive docstring with examples
   
   Show me the complete implementation.
   ```
   
   - Compare the two responses
   - Note how specificity yields production-ready code

3. **Key Takeaway:**
   
   Specific prompts with explicit requirements produce better code than vague requests. Always include:
   - Expected inputs and outputs
   - Edge cases to handle
   - Return types and error handling
   - Code style preferences

**Expected Outcome:**
- Understand impact of prompt specificity
- Can articulate requirements clearly
- Recognize when to add more constraints

---

### Exercise 2.2: Iterative Refinement (5 min)

**Task:** Learn to refine prompts based on Copilot's responses

**Scenario:** You need a function to check low stock levels, but the first attempt isn't quite right.

1. **Initial prompt:**
   
   In Ask mode:
   ```
   #file:database.py Add a method to get low stock products
   ```
   
   - Review what Copilot suggests
   - Identify what's missing or incorrect

2. **Refine based on the response:**
   
   In the same chat, continue:
   ```
   Good start, but please modify it to:
   - Accept a threshold parameter (default 50 units)
   - Return products sorted by quantity (lowest first)
   - Include the percentage below threshold in the result
   - Add type hints for all parameters and return values
   ```

3. **Further refinement if needed:**
   ```
   Can you also add a property that calculates days_until_stockout 
   assuming an average daily usage of 5 units per day?
   ```

4. **Key Principle:** Don't expect perfection on the first try. Iterate by:
   - Reviewing the initial output
   - Identifying gaps or issues
   - Refining in the same conversation with specific corrections
   - Building on what works

**Expected Outcome:**
- Comfortable with iterative refinement
- Know how to build on previous responses
- Can identify and articulate needed improvements

---

### Exercise 2.3: Using Examples in Prompts (5 min)

**Task:** Learn few-shot prompting with concrete examples

**Scenario:** You want consistent formatting for API response messages across all endpoints.

1. **Use examples to guide Copilot:**
   
   In Ask mode:
   ```
   #file:inventory.py I want to standardize error responses in this file.
   
   Here are examples of the format I want:
   
   Good example:
   {
     "error": "ProductNotFound",
     "message": "Product with ID 123 not found",
     "details": {
       "product_id": 123,
       "available_ids": [1, 2, 3, 4]
     }
   }
   
   Another good example:
   {
     "error": "InvalidQuantity", 
     "message": "Quantity must be positive",
     "details": {
       "provided_quantity": -5,
       "minimum_allowed": 0
     }
   }
   
   Please:
   1. Create a helper function to generate these structured error responses
   2. Update all HTTPException calls in this file to use this format
   3. Ensure each error includes relevant context in the details field
   ```

2. **Observe the pattern matching:**
   - Copilot should follow your example structure
   - The output should be consistent with your examples
   - All errors should include the three-part structure

3. **Key Principle:** Examples are powerful constraints. Use them to:
   - Show desired code style
   - Demonstrate expected structure
   - Illustrate edge case handling
   - Define naming conventions

**Expected Outcome:**
- Know how to provide examples in prompts
- Understand few-shot prompting technique
- Can guide style and structure with examples

---

### Module 1 Summary

You've learned three powerful prompting strategies:

1. **Specificity** - Detailed requirements produce better code
2. **Iteration** - Refine prompts based on initial output  
3. **Examples** - Show, don't just tell, what you want

**Best Practice:** Start specific, iterate as needed, and use examples for consistency.

---

## Part 2.5: Persisting Your Prompting Strategy (40 minutes)

### Introduction: Making Your Preferences Persistent

In Part 1, you learned how to craft effective prompts. But what if you want Copilot to **always** follow certain patterns without repeating yourself every time? This is where **Custom Instructions** and **Custom Prompts** come in.

**Two complementary features:**
- **Custom Instructions** = Persistent context ("Always do X") - PASSIVE background context
- **Custom Prompts** = Reusable templates ("When I ask, do Y") - ACTIVE shortcuts

**Why use them:**
- Consistency across all your code
- Reduce repetitive context in prompts
- Share best practices with your team
- Build a library of proven patterns

This module teaches you to set up both features for maximum productivity.

---

### Section A: Custom Instructions (20 minutes)

Custom Instructions are markdown files that provide **persistent context** to Copilot. They apply automatically without you mentioning them in every prompt.

**Two types of instruction files:**

1. **Always-on instructions** - Apply to ALL chat requests in the workspace
   - `.github/copilot-instructions.md` - Workspace-wide coding standards

2. **File-based instructions** - Apply conditionally based on file patterns
   - `*.instructions.md` files stored in `.github/instructions/` folder (or other configured locations)
   - Use YAML frontmatter with `applyTo` glob patterns to specify when they apply

**Key Concepts:**
- Instructions are combined from multiple sources (workspace, user profile, organization)
- Higher priority: User > Workspace > Organization
- You can also create user-level instructions that apply across all workspaces

---

### Exercise 2.5.1: Create Always-On Workspace Instructions (6 min)

**Task:** Define project-wide coding standards that apply to ALL Copilot interactions

1. **Create the instructions file:**
   
   Create a new file: `.github/copilot-instructions.md` in your workspace root
   
   **Tip:** You can use the slash command `/instructions` in Copilot Chat to quickly access the instructions editor.

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
   - Compare to Part 1 results without instructions

4. **Key Observation:**
   
   You didn't mention "add type hints" or "include docstrings" - Copilot applied these patterns automatically from your instructions!

**Expected Outcome:**
- `.github/copilot-instructions.md` file created
- Understanding that always-on instructions apply to ALL Copilot interactions
- See automatic application of coding standards

---

### Exercise 2.5.2: Create File-Based Instructions with Patterns (8 min)

**Task:** Create specialized instructions that apply only to specific file types or locations

**Important:** File-based instructions use **YAML frontmatter** with an `applyTo` property to define glob patterns.

1. **Create the instructions folder:**
   
   Create directory: `.github/instructions/` in your workspace root

2. **Create router-specific instructions:**
   
   Create file: `.github/instructions/router-conventions.instructions.md`
   
   **Note the YAML frontmatter:**
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
   
   Open a file in `steel-inventory-api/app/routers/` and ask Copilot:
   ```
   Create a new endpoint to get products by location
   ```
   
   Then open a file in `steel-inventory-api/tests/` and ask:
   ```
   Create a test for the delete product endpoint
   ```
   
   - Notice how Copilot applies different patterns based on which file matches the `applyTo` pattern!
   - Check the **References** section in the chat response to see which instruction files were applied

**Expected Outcome:**
- Two `.instructions.md` files created in `.github/instructions/` folder
- Understanding of YAML frontmatter and `applyTo` patterns
- See pattern-based application of instructions

---

### Exercise 2.5.3: Use the Agent Customizations Editor (3 min)

**Task:** Discover and manage instructions through the VS Code UI

1. **Open the Agent Customizations editor:**
   
   - Option 1: Type `/instructions` in Copilot Chat input
   - Option 2: Press `Ctrl+Shift+P` and run "Chat: Configure Instructions"
   - Option 3: Click the gear icon in Chat view and select Instructions tab

2. **Explore the instructions list:**
   
   You should see:
   - ✅ Steel Inventory API - Copilot Instructions (always-on)
   - ✅ API Router Conventions (applies to `**/routers/**/*.py`)
   - ✅ Test Conventions (applies to `**/tests/**/*.py`)
   
   - Hover over each to see the description
   - Note the source location (workspace)

3. **Generate instructions with AI (Optional):**
   
   In Copilot Chat, try the slash command:
   ```
   /create-instruction Always use pathlib.Path instead of os.path for file operations in Python
   ```
   
   - Copilot will ask clarifying questions
   - It will generate an `.instructions.md` file with appropriate `applyTo` pattern
   - Review and save the generated file

4. **Check diagnostics (Optional):**
   
   - Right-click in Chat view → Select "Diagnostics"
   - View all loaded instruction files
   - See if any errors or warnings

**Expected Outcome:**
- Familiarity with the Agent Customizations editor
- Know how to view and manage instructions
- Understanding of `/create-instruction` command

---

### Exercise 2.5.4: Instructions in Practice (3 min)

**Task:** Experience the power of "set it and forget it" context

1. **Generate code with minimal prompting:**
   
   In Ask mode, give a brief high-level request:
   ```
   #file:inventory.py Add an endpoint to get low stock products below a threshold
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

---

### Section B: Custom Prompts (20 minutes)

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
- Prompt files can specify which agent to use (`ask`, `agent`, `plan`)
- Can include tool restrictions and model selection
- Support variables and user input with `${input:variableName}` syntax
- Can reference instructions and other workspace files

---

### Exercise 2.5.5: Create Prompt Files for Common Tasks (9 min)

**Task:** Build a library of prompt files for development tasks

**Important:** Each prompt is a **separate file** with `.prompt.md` extension.

1. **Create the prompts folder:**
   
   Create directory: `.github/prompts/` in your workspace root
   
   **Tip:** You can use the slash command `/prompts` in Copilot Chat to quickly access the prompts editor.

2. **Create "Generate Tests" prompt:**
   
   Create file: `.github/prompts/generate-tests.prompt.md`
   
   **Note the YAML frontmatter:**
   ```markdown
   ---
   name: generate-tests
   description: Generate comprehensive pytest tests for selected code
   argument-hint: Select a function or endpoint first
   agent: ask
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
   
   ```markdown
   ---
   name: add-error-handling
   description: Add comprehensive error handling to selected code
   argument-hint: Select the code to enhance
   agent: ask
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
   
   ```markdown
   ---
   name: document-endpoint
   description: Add comprehensive OpenAPI docs to an endpoint
   argument-hint: Select an API endpoint function
   agent: ask
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

5. **Create "Steel Grade Validation" domain-specific prompt:**
   
   Create file: `.github/prompts/add-grade-validation.prompt.md`
   
   ```markdown
   ---
   name: add-grade-validation
   description: Add steel grade validation to selected code
   argument-hint: Select code that needs grade validation
   agent: ask
   ---
   Add comprehensive steel grade validation to the selected code.

   Valid steel grades:
   - Carbon Steel: A36, A572, A992
   - Stainless Steel: 304, 304L, 316, 316L, 321, 410
   - Alloy Steel: 4140, 4340, 8620

   Requirements:
   - Accept grades case-insensitive
   - Normalize to uppercase
   - Validate against allowed list above
   - Return tuple: (is_valid: bool, normalized_grade: str, category: str)
   - Raise ValueError with helpful message for invalid grades
   - Include all valid grades in error message
   - Add type hints and docstring

   Return the code with validation added.
   ```

6. **Understanding the frontmatter:**
   - `name`: Command name after `/` (e.g., `/generate-tests`)
   - `description`: Short description shown in prompt picker
   - `argument-hint`: Hint text shown in chat input
   - `agent`: Which mode to use (`ask`, `agent`, or `plan`)
   - `model`: (Optional) Specific model to use
   - `tools`: (Optional) Restrict which tools are available

**Expected Outcome:**
- Four `.prompt.md` files created in `.github/prompts/` folder
- Understanding of proper prompt file format with YAML frontmatter
- Recognition of when to create custom prompts

---

### Exercise 2.5.6: Use Prompt Files with Slash Commands (5 min)

**Task:** Invoke and use your custom prompt files

1. **Use the "Generate Tests" prompt:**
   
   - Open `steel-inventory-api/app/utils/steel_utils.py`
   - Select the `validate_grade` function
   - Open Copilot Chat
   - Type `/generate-tests` and press Enter
   - Review the generated tests - should be comprehensive and follow test instructions

2. **Use the "Add Error Handling" prompt:**
   
   - Open `steel-inventory-api/app/routers/inventory.py`
   - Select a function that needs better error handling
   - In Copilot Chat, type `/add-error-handling`
   - Observe how it adds validation, error messages, and proper status codes

3. **Use the "Document Endpoint" prompt:**
   
   - Open `steel-inventory-api/app/routers/inventory.py`
   - Select an endpoint function
   - Type `/document-endpoint` in chat
   - See comprehensive OpenAPI documentation added

4. **Alternative invocation methods:**
   
   - **Command Palette:** Press `Ctrl+Shift+P` → "Chat: Run Prompt" → Select prompt
   - **Prompt file editor:** Open any `.prompt.md` file → Click play button in title bar to test it
   - **Type `/` in chat:** See all available prompts listed

5. **Observe the synergy:**
   
   Notice how **Custom Prompts + Custom Instructions work together**:
   - Prompt defines WHAT to do (generate tests, add error handling)
   - Instructions define HOW to do it (code style, patterns from `.instructions.md`)
   - Result: Consistent, high-quality output every time

**Expected Outcome:**
- Comfortable using `/` slash commands to invoke prompts
- See how prompts + instructions complement each other
- Experience faster execution of common tasks

---

### Exercise 2.5.7: Use the Agent Customizations Editor for Prompts (3 min)

**Task:** Manage prompt files through the VS Code UI

1. **Open the Agent Customizations editor:**
   
   - Option 1: Type `/prompts` in Copilot Chat input
   - Option 2: Press `Ctrl+Shift+P` and run "Chat: Configure Prompt Files"
   - Option 3: Click the gear icon in Chat view and select Prompts tab

2. **Explore the prompts list:**
   
   You should see:
   - ✅ generate-tests
   - ✅ add-error-handling
   - ✅ document-endpoint
   - ✅ add-grade-validation
   
   - Hover over each to see the description
   - Note the source location (workspace)
   - Click to open and edit

3. **Test a prompt directly from the editor:**
   
   - Open `.github/prompts/generate-tests.prompt.md`
   - Click the **play button** (▶) in the editor title bar
   - Choose "Run in current chat" or "Run in new chat"
   - This is useful for testing prompts as you develop them

4. **Generate a new prompt with AI (Optional):**
   
   In Copilot Chat, try the slash command:
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

### Exercise 2.5.8: Build Your Prompt Library (3 min)

**Task:** Think strategically about reusable prompts

1. **Identify repetitive tasks in your workflow:**
   
   Discuss with your group or reflect:
   - What tasks do you do repeatedly?
   - What complex prompts do you type often?
   - What team standards should be codified?

2. **Add one custom prompt based on your work:**
   
   Think about the steel inventory project. What task might you repeat?
   Examples:
   - Add pagination to endpoints
   - Create database migration scripts
   - Add request logging
   - Generate mock data
   
   Create a new `.prompt.md` file in `.github/prompts/` for one of these tasks.
   Use the `/create-prompt` command or create it manually.

3. **Key Decision Framework:**
   
   **When to use Instructions vs. Prompts:**
   
   Use **Instructions** when:
   - ✅ Should apply to ALL code (coding style, patterns)
   - ✅ Background context that's always relevant
   - ✅ "Always do it this way"
   
   Use **Prompts** when:
   - ✅ Specific task you invoke intentionally
   - ✅ Multi-step workflow with clear steps
   - ✅ "Do this when I ask"
   
   Use **Both** when:
   - ✅ Prompt defines the task, instructions define the style
   - ✅ Common tasks that should follow team standards

**Expected Outcome:**
- Strategic thinking about when to create instructions vs prompts
- Started building your personal/team prompt library
- Understanding the complementary nature of both features

---

### Part 1.5 Summary

You've learned to persist your prompting strategy with two powerful features:

**Custom Instructions (`.instructions.md`)**
- Provide persistent context automatically
- Define coding style, patterns, and standards
- Two types: always-on (workspace-wide) and file-based (pattern-matched with `applyTo`)
- Passive - applied automatically based on context

**Custom Prompts (`.prompt.md`)**
- Create reusable slash commands for common tasks
- Each prompt is a separate file in `.github/prompts/` folder
- Invoked with `/` slash commands (e.g., `/generate-tests`)
- Use YAML frontmatter to configure agent, model, and tools
- Active - used when you explicitly call them

**Key Takeaways:**
1. Instructions reduce prompt length while improving consistency
2. Prompts eliminate repetitive typing for common tasks
3. Together they form a powerful productivity system
4. Both features enhance ALL Copilot modes (Ask, Plan, Agent)
5. Use `/instructions` and `/prompts` to configure via UI
6. Use `/create-instruction` and `/create-prompt` to generate with AI

**Best Practice:** Start with instructions for your coding style, then add prompts as you identify repetitive tasks.

**Throughout the rest of this lab:**
- Your instructions will automatically guide Copilot's suggestions
- You can invoke your custom prompts with `/` commands whenever relevant
- Notice how both features improve your efficiency in Parts 2-6

---

## Part 3: Test Automation with Copilot (25 minutes)

### Introduction: True Test Automation

Test automation means letting Copilot handle the entire test creation workflow - planning what's needed and automatically adding tests to files. This is more efficient than generating code in chat and manually copying it.

**Workflow:** Plan Mode → Agent Mode for fully automated test creation

---

### Exercise 3.1: Automate Parametrized Tests (8 min)

**Task:** Use Plan → Agent workflow to automatically create parametrized tests

**Mode:** Plan mode → Agent mode  
**Why:** Plan identifies all test cases, Agent writes them to file

1. **Plan the tests:**
   
   Switch to **Plan mode** and ask:
   ```
   #file:steel_utils.py #file:test_inventory.py
   
   Analyze the calculate_weight_kg function and plan comprehensive parametrized tests.
   
   What test cases are needed for:
   - All supported shapes (sheet, plate)
   - Edge cases (minimum/maximum dimensions, zero thickness)
   - Unsupported shapes (coil, bar, tube) - should raise NotImplementedError
   - Missing width for shapes that require it
   - Accurate weight calculations
   
   Create a test plan with specific test cases and expected values.
   ```
   
   - Review the plan - does it cover all scenarios?

2. **Implement with Agent:**
   
   Switch to **Agent mode** and ask:
   ```
   #file:test_inventory.py #file:steel_utils.py
   
   Implement the parametrized test plan for calculate_weight_kg.
   
   Requirements:
   - Use pytest.mark.parametrize
   - Name: test_calculate_weight_parametrized
   - Add all test cases from the plan
   - Include test IDs for clarity
   - Add to test_inventory.py
   ```
   
   - Watch Agent add tests to the file automatically

3. **Run and verify:**
   ```bash
   pytest tests/test_inventory.py::test_calculate_weight_parametrized -v
   ```

**Expected Outcome:**
- Complete test automation workflow
- 8-10 parametrized test cases automatically added
- Understanding of Plan → Agent for test creation

---

### Exercise 3.2: Automate CRUD Test Suite (8 min)

**Task:** Use Plan → Agent workflow for comprehensive CRUD testing

**Mode:** Plan mode → Agent mode  
**Why:** Complex test suite needs planning before implementation

1. **Plan comprehensive CRUD tests:**
   
   Switch to **Plan mode**:
   ```
   #file:inventory.py #file:database.py #file:test_inventory.py
   
   Plan a comprehensive test suite for all CRUD operations.
   
   Identify:
   - All CRUD endpoints that need testing
   - Success cases for each operation
   - Error cases (not found, validation failures, duplicates)
   - Edge cases
   - What fixtures or test data setup is needed
   
   Create a structured test plan.
   ```
   
   - Review the plan
   - Note which tests reveal bugs (e.g., duplicate product codes)

2. **Implement with Agent:**
   
   Switch to **Agent mode**:
   ```
   Implement the CRUD test plan in test_inventory.py.
   
   Requirements:
   - Use FastAPI TestClient
   - Test isolation (each test independent)
   - Assert both status codes AND response body
   - Descriptive test names
   - Add all tests to test_inventory.py
   ```
   
   - Watch Agent create the complete test suite

3. **Run and analyze:**
   ```bash
   pytest tests/test_inventory.py -v -k "crud or create or update or delete"
   ```
   
   - Note which tests fail (reveals missing features)
   - This demonstrates tests driving development

**Expected Outcome:**
- 9-10 CRUD tests automatically added
- Some failures revealing bugs (expected!)
- Test-driven development approach

---

### Exercise 3.3: Automate Validation Tests (5 min)

**Task:** Direct automation with Agent mode

**Mode:** Agent mode (direct automation)  
**Why:** Clear requirements, no planning needed

**Direct automation with Agent:**

Switch to **Agent mode**:
```
#file:inventory.py #file:models.py #file:test_inventory.py

Add validation error tests to test_inventory.py:

1. test_create_product_invalid_shape - Shape not in allowed list
2. test_create_product_negative_quantity - Quantity < 0  
3. test_create_product_zero_thickness - Thickness = 0
4. test_create_product_missing_width - Sheet without width
5. test_update_product_negative_quantity - Update with invalid quantity

Each test should:
- Verify 422 status code
- Check error message is descriptive
- Ensure database unchanged after error

Add all tests to test_inventory.py.
```

**Run tests:**
```bash
pytest tests/test_inventory.py -v -k "invalid or negative or zero or missing"
```

- Some may fail (validation not yet implemented)
- This is good - tests drive implementation!

**Expected Outcome:**
- 5 validation tests automatically added
- Understanding of Agent mode for straightforward tasks
- Tests revealing missing validation

---

### Exercise 3.4: Analyze Coverage with Plan Mode (4 min)

**Task:** Use Plan mode for analysis and strategic thinking

**Mode:** Plan mode  
**Why:** Analysis and strategic thinking

Switch to **Plan mode**:
```
#file:inventory.py #file:test_inventory.py

Analyze test coverage for inventory.py endpoints.

What scenarios are NOT tested yet?
- Boundary conditions?
- Different data combinations?
- Business logic edge cases?
- Performance considerations?

Provide a prioritized list of missing test coverage.
```

Review the analysis:
- What gaps did Plan mode identify?
- Which are most critical?
- Save this for future test implementation

**Expected Outcome:**
- Comprehensive coverage analysis
- Prioritized test gaps
- Strategic view of testing needs

---

### Part 3 Summary

You've learned **test automation workflows:**

1. **Plan → Agent:** Complex test suites (Ex 3.1, 3.2)
   - Plan identifies all cases
   - Agent implements automatically
   
2. **Direct Agent:** Simple test additions (Ex 3.3)
   - Clear requirements
   - No planning needed
   
3. **Plan for Analysis:** Coverage gaps (Ex 3.4)
   - Strategic thinking
   - Prioritization

**Key Insight:** True automation means Plan → Agent, not Ask → manual copy/paste!

---

## Part 3: Advanced Debugging Workflows (20 minutes)

## Part 4: Advanced Debugging Workflows (20 minutes)

### Introduction: Strategic Debugging

Debugging workflows benefit from strategic analysis (Plan mode) followed by automated fixes (Agent mode). This prevents quick fixes that miss root causes and ensures comprehensive solutions.

**Workflow:** Plan Mode → Agent Mode for systematic debugging

---

### Exercise 4.1: Debug calculate_area_m2 Bug (6 min)

**Task:** Use Plan → Agent workflow to understand and fix bugs systematically

**Mode:** Plan mode → Agent mode  
**Why:** Understand root cause before fixing

**Scenario:** The `calculate_area_m2` function crashes when width_mm is None.

1. **Analyze the bug with Plan:**
   
   Switch to **Plan mode**:
   ```
   #file:steel_utils.py
   
   There's a bug in calculate_area_m2 - it crashes when width_mm is None.
   
   Analyze:
   1. Why does this bug occur?
   2. Which shapes can have None width? Which require width?
   3. Where is this function called from?
   4. What's the appropriate fix?
   5. Should we calculate area for shapes without width?
   6. What validation/error handling is needed?
   
   Provide a comprehensive fix plan.
   ```
   
   - Review the root cause analysis
   - Understand which shapes need width
   - Review the proposed solution

2. **Implement fix with Agent:**
   
   Switch to **Agent mode**:
   ```
   #file:steel_utils.py
   
   Implement the fix for calculate_area_m2 based on the analysis:
   
   - Add validation for None width
   - Raise ValueError with clear message for shapes without width
   - Update docstring explaining which shapes are valid
   - Add note about which shapes have width (sheet, plate, coil) vs not (bar, tube)
   ```
   
   - Watch Agent apply the fix

3. **Create regression test:**
   
   Still in **Agent mode**:
   ```
   #file:test_inventory.py
   
   Add regression tests for calculate_area_m2:
   - Test with valid width (should succeed)
   - Test with None width (should raise ValueError)
   - Test error message is descriptive
   ```

4. **Verify:**
   ```bash
   pytest tests/test_inventory.py -v -k "area"
   ```

**Expected Outcome:**
- Bug fixed with proper validation
- Root cause understood
- Regression test prevents recurrence
- Plan → Agent debugging workflow

---

### Exercise 4.2: Trace and Fix Request Flow Bug (5 min)

**Task:** Debug multi-file bugs with comprehensive analysis

**Mode:** Plan mode → Agent mode  
**Why:** Multi-file bugs need comprehensive analysis

**Scenario:** Users report they can create products with negative quantities.

1. **Trace the flow with Plan:**
   
   Switch to **Plan mode**:
   ```
   #codebase
   
   When a user creates a product with quantity=-10, trace the request flow:
   
   1. Which endpoint receives the request?
   2. What model validates the input?
   3. What database method stores it?
   4. Where should validation happen?
   5. Why isn't negative quantity being rejected?
   6. What's the proper fix location?
   
   Provide the exact code path and identify the missing validation.
   ```
   
   - Review the flow analysis
   - Identify validation gap
   - Understand layered architecture

2. **Fix with Agent:**
   
   Switch to **Agent mode**:
   ```
   #file:models.py
   
   Add validation to prevent negative quantities:
   
   - SteelProductCreate: quantity must be >= 0
   - SteelProductUpdate: quantity must be >= 0 (if provided)
   - Use Pydantic Field validators
   - Add descriptive error messages
   ```
   
   - Watch Agent add validation

3. **Test the fix:**
   - Start server: `uvicorn app.main:app --reload`
   - Try creating product with quantity=-10 in Swagger UI
   - Verify 422 status with clear error message

**Expected Outcome:**
- Bug traced through multiple layers
- Validation added at proper layer (model)
- Understanding of request flow
- Verified in running application

---

### Exercise 4.3: Debug and Implement Missing Features (5 min)

**Task:** Use Plan → Agent for complex implementations

**Mode:** Plan mode → Agent mode  
**Why:** Complex implementation needs design first

**Scenario:** Weight calculation fails for coils, bars, and tubes (NotImplementedError).

1. **Plan the implementation:**
   
   Switch to **Plan mode**:
   ```
   #file:steel_utils.py #file:calculations.py
   
   Users get NotImplementedError for coils, bars, and tubes.
   
   Plan the implementation:
   1. What calculations are needed for each shape?
      - Coil: rolled sheet, treat as sheet
      - Bar: solid circular cross-section (thickness = diameter)
      - Tube: hollow circular cross-section (need inner and outer diameter)
   2. What are the mathematical formulas?
   3. For tubes, we only have thickness - what's a reasonable approach?
   4. What assumptions should we document?
   5. What validation is needed?
   
   Provide formulas and implementation approach.
   ```
   
   - Review the formulas
   - Understand the tube limitation
   - Decide on reasonable assumptions

2. **Implement with Agent:**
   
   Switch to **Agent mode**:
   ```
   #file:steel_utils.py
   
   Implement calculate_weight_kg for coil, bar, and tube:
   
   - Coil: treat as sheet (same calculation)
   - Bar: solid cylinder using thickness as diameter
   - Tube: [use approach from plan - document assumption]
   - Add comments explaining formulas
   - Add comments documenting assumptions (especially for tube)
   - Update docstring with all supported shapes
   ```
   
   - Watch Agent implement calculations

3. **Test in Swagger:**
   - Use `/calculations/weight` endpoint
   - Test with coil shape
   - Test with bar shape
   - Test with tube shape
   - Verify reasonable results

**Expected Outcome:**
- Complete weight calculations implemented
- Documented assumptions
- Understanding of geometric calculations
- All shapes now supported

---

### Exercise 4.4: Document Bug with Plan Mode (4 min)

**Task:** Use Plan mode for analysis and documentation

**Mode:** Plan mode  
**Why:** Analysis and documentation task

**Scenario:** Document the duplicate product code bug for the team.

Switch to **Plan mode**:
```
#file:database.py #file:inventory.py

Create a detailed bug report for the missing duplicate product code validation.

Include:
1. Bug title and severity level
2. Steps to reproduce (with example curl commands)
3. Expected behavior
4. Actual behavior (what happens now)
5. Root cause analysis (which file and function)
6. Suggested fix location and approach
7. Impact analysis (what could go wrong in production)
8. Related code that might have similar issues

Format as a professional GitHub issue in Markdown.
```

Review the bug report:
- Is it detailed enough for another developer?
- Are reproduction steps clear?
- Is root cause analysis accurate?
- Save this for future implementation

**Expected Outcome:**
- Professional bug report created
- Understanding of proper bug documentation
- Clear path to fix for development team
- Reusable template for future bugs

---

### Part 4 Summary

You've learned **debugging workflows with Copilot:**

1. **Plan → Agent for bug fixes** (Ex 4.1, 4.2, 4.3)
   - Plan analyzes root cause
   - Agent implements the fix
   - Prevents band-aid solutions
   
2. **Plan for documentation** (Ex 4.4)
   - Professional bug reports
   - Clear communication
   
3. **Multi-file debugging**
   - Trace through layers
   - Fix at the right place
   - Comprehensive solutions

**Key Workflow:**
```
Bug Report → Plan (analyze) → Agent (fix) → Verify → Document
```

**Key Insight:** Strategic debugging means understanding *why* before fixing *what*. Plan mode prevents hasty fixes that miss root causes!

---

## Part 5: Code Quality & Refactoring Patterns (30 minutes)

### Introduction: Safe Refactoring with AI

Refactoring improves code structure without changing behavior. With Copilot, you can refactor confidently by generating tests first, then using AI to suggest improvements. This module teaches **safe refactoring patterns** with test verification.

---

### Exercise 4.1: Extract Validation Logic (10 min)

**Task:** Refactor scattered validation into a dedicated module

Currently, validation logic is mixed throughout the codebase. Let's extract it properly.

1. **Analyze current validation:**
   
   In Ask mode:
   ```
   #codebase Where is validation logic currently located in this application?
   Identify all places where we validate:
   - Product codes
   - Steel grades  
   - Quantities
   - Dimensions
   - Shapes
   
   Is validation consistent across all locations?
   ```

2. **Plan the refactoring:**
   ```
   I want to create app/validators.py with all validation logic.
   
   Design a validators module with:
   1. validate_product_code(code: str) -> tuple[bool, str]
   2. validate_grade(grade: str) -> tuple[bool, str, str] 
   3. validate_quantity(quantity: int) -> tuple[bool, str]
   4. validate_dimensions(length, width, thickness, shape) -> tuple[bool, str]
   5. validate_shape(shape: str) -> tuple[bool, str]
   
   Each should return (is_valid, message/normalized_value).
   Include comprehensive docstrings and unit tests.
   
   Show me the complete validators.py file.
   ```

3. **Create the validators module:**
   - Create `app/validators.py`
   - Paste the generated code
   - Review for completeness

4. **Update existing code to use validators:**
   ```
   #file:validators.py #file:database.py 
   
   Update database.py's create method to use the new validators.
   Before creating a product:
   1. Validate the product code
   2. Validate the grade
   3. Validate the quantity
   4. Raise HTTPException with appropriate status code if validation fails
   
   Show me the updated create method.
   ```

5. **Apply changes and test:**
   - Update `database.py`
   - Run existing tests: `pytest tests/test_inventory.py -v`
   - Verify tests still pass (refactoring shouldn't break functionality)

**Expected Outcome:**
- New validators module with centralized validation
- Database using validators for consistency
- Tests still passing after refactoring
- Cleaner separation of concerns

---

### Exercise 4.2: Add Comprehensive Error Handling (8 min)

**Task:** Improve error handling in steel_utils.py

1. **Ask Copilot to analyze current error handling:**
   
   In Ask mode:
   ```
   #file:steel_utils.py Analyze error handling in this file.
   
   What could go wrong with:
   1. Negative dimensions?
   2. Extremely large values (overflow)?
   3. Invalid shape strings?
   4. Division by zero scenarios?
   
   What errors are not being caught?
   ```

2. **Implement comprehensive error handling:**
   ```
   Update calculate_weight_kg to:
   1. Validate all dimensions are positive
   2. Validate dimensions are within reasonable ranges (e.g., < 100,000 mm)
   3. Raise ValueError with specific messages for each error case
   4. Add input validation before calculations
   5. Include examples of valid and invalid inputs in docstring
   
   Show me the updated function.
   ```

3. **Apply the changes:**
   - Update `steel_utils.py`
   - Save the file

4. **Generate tests for error cases:**
   ```
   Create tests for all the new error conditions in calculate_weight_kg.
   Add to test_inventory.py.
   ```

5. **Run the error handling tests:**
   ```bash
   pytest tests/test_inventory.py -v -k "weight"
   ```

**Expected Outcome:**
- Robust error handling in calculations
- Descriptive error messages
- Tests verifying all error conditions
- Understanding of defensive programming

---

### Exercise 4.3: Add Logging to Critical Operations (7 min)

**Task:** Add structured logging for debugging and monitoring

1. **Ask Copilot about logging strategy:**
   
   In Ask mode:
   ```
   #file:database.py #file:inventory.py
   
   I want to add logging to this application for production monitoring.
   
   Design a logging strategy:
   1. What operations should be logged?
   2. What log levels should be used (INFO, WARNING, ERROR)?
   3. What information should each log include?
   4. Should we log sensitive data?
   
   Then show me how to:
   - Set up Python logging in the application
   - Add logs to database CRUD operations
   - Add logs to API endpoints
   - Format logs with timestamps, operation, and context
   ```

2. **Implement logging in database.py:**
   ```
   Add logging to InMemoryDB class:
   - Log INFO when products are created (include product_code)
   - Log INFO when products are updated (include id and what changed)
   - Log INFO when products are deleted (include id)
   - Log WARNING when duplicate product codes are attempted
   - Log ERROR if any operation fails
   
   Show me the updated database.py with logging.
   ```

3. **Apply logging:**
   - Update `database.py` with logging
   - Add `import logging` at the top
   - Configure logger: `logger = logging.getLogger(__name__)`

4. **Test logging:**
   - Run the server with logging visible: `uvicorn app.main:app --reload --log-level info`
   - Create a product in Swagger UI
   - Check terminal for log output

**Expected Outcome:**
- Structured logging in place
- Appropriate log levels used
- Useful context in each log message
- Production-ready logging practices

---

### Exercise 4.4: Verify Refactoring with Tests (5 min)

**Task:** Ensure all refactoring hasn't broken functionality

1. **Run the complete test suite:**
   ```bash
   pytest tests/test_inventory.py -v
   ```

2. **Check for failures:**
   - If tests fail, ask Copilot:
   ```
   This test is failing after refactoring: [paste test name and error]
   
   #file:validators.py #file:database.py #file:test_inventory.py
   
   What did I break? How do I fix it without reverting the refactoring?
   ```

3. **Verify application still works:**
   - Start server: `uvicorn app.main:app --reload`
   - Test in Swagger UI: http://localhost:8000/docs
   - Create, read, update, and delete a product
   - Calculate weight for different shapes

4. **Review what you've refactored:**
   - Extracted validators
   - Added error handling
   - Implemented logging
   - Maintained test coverage

**Expected Outcome:**
- All tests passing
- Application working correctly
- Code is cleaner and more maintainable
- Confidence in refactoring process

---

### Module 4 Summary

You've learned to:
1. **Extract scattered logic** into focused modules
2. **Add comprehensive error handling** with validation
3. **Implement structured logging** for production
4. **Verify refactoring** with automated tests

**Key Insight:** Refactor with tests as your safety net. Tests give you confidence that behavior hasn't changed.

---

## Part 6: Complex Feature Implementation (30 minutes)

### Introduction: Combining Modes for Complex Work

Complex features require the full spectrum of Copilot modes: Plan for design, Agent for implementation, and Ask for refinement. This module brings together everything you've learned.

**Workflow:** Plan → Agent → Verify → Refine

---

### Exercise 6.1: Plan Batch Operations Feature (10 min)

**Task:** Design a comprehensive batch operations API

**Mode:** Plan mode  
**Why:** Complex feature needs thorough planning

**Scenario:** The warehouse needs to process multiple products in a single API call (bulk imports, bulk updates).

1. **Switch to Plan mode and design the feature:**
   
   ```
   #codebase I need to add batch operations to the steel inventory API.
   
   Requirements:
   - POST /inventory/batch - Create multiple products at once
   - PATCH /inventory/batch - Update multiple products by ID
   - DELETE /inventory/batch - Delete multiple products by ID
   
   Design considerations:
   - What if some operations succeed and others fail?
   - Should it be transactional (all-or-nothing) or allow partial success?
   - How to return detailed results for each operation?
   - What's the maximum batch size?
   - How to handle duplicate product codes in the batch?
   - Performance implications for large batches?
   
   Create a detailed implementation plan including:
   1. API endpoint design (request/response models)
   2. Error handling strategy (partial success approach)
   3. Validation approach
   4. Database changes needed
   5. Testing strategy
   6. Step-by-step implementation order
   7. Potential edge cases
   ```

2. **Review and refine the plan:**
   - Does it address all requirements?
   - Are error handling strategies clear?
   - Is the implementation order logical?

3. **Ask for clarification if needed:**
   ```
   I like the plan. Can you elaborate on the partial success approach?
   
   Specifically:
   - What should the response format look like?
   - How should we handle rollback scenarios?
   - Should we validate all items before processing any?
   ```

**Expected Outcome:**
- Comprehensive implementation plan
- Clear API design with request/response models
- Error handling strategy defined
- Ready to implement

---

### Exercise 6.2: Implement with Agent Mode (15 min)

**Task:** Let Agent implement the batch operations plan

**Mode:** Agent mode  
**Why:** Clear plan exists, ready for autonomous execution

1. **Switch to Agent mode and give implementation task:**
   
   ```
   #codebase Implement the batch operations endpoints based on our plan.
   
   Requirements:
   1. Create POST /inventory/batch endpoint for creating multiple products
      - Accept list of SteelProductCreate objects
      - Return list of results with success/failure for each
      - Continue processing even if some fail
      - Response format: { "successful": [...], "failed": [{product, error}] }
   
   2. Create PATCH /inventory/batch endpoint for updating multiple products
      - Accept list of { "id": int, "updates": SteelProductUpdate }
      - Same response format as create
   
   3. Create DELETE /inventory/batch endpoint
      - Accept list of product IDs
      - Same response format
   
   4. Add comprehensive error handling
   5. Add validation for batch size (max 100 items)
   
   6. Create tests for all three endpoints covering:
      - Successful batch operations
      - Partial failures (some succeed, some fail)
      - Validation errors
      - Empty batches
      - Oversized batches
   
   Implementation constraints:
   - Add new models to models.py for batch requests/responses
   - Add new endpoints to inventory.py router
   - Update database.py if needed for batch operations
   - Add all tests to test_inventory.py
   - Follow existing code style and patterns
   
   Implement all of this end-to-end.
   ```

2. **Monitor Agent progress:**
   - Watch as it reads files and makes changes
   - You can stop it if it goes off track

3. **Review Agent's changes:**
   - Check all modified files
   - Verify implementation matches plan

4. **Run tests:**
   ```bash
   pytest tests/test_inventory.py -v -k "batch"
   ```

5. **Start server and test in Swagger UI:**
   ```bash
   uvicorn app.main:app --reload
   ```
   
   - Go to http://localhost:8000/docs
   - Try POST /inventory/batch with mixed valid/invalid products
   - Verify partial success handling works

6. **If issues arise, use Ask or Agent to fix:**
   
   In Ask mode:
   ```
   The batch create has an issue: [describe problem]
   How should I fix this?
   ```
   
   Or in Agent mode:
   ```
   Fix the batch create issue: [describe problem]
   ```

**Expected Outcome:**
- Batch operations fully implemented
- Tests passing
- Partial success handling working
- All three endpoints functional

---

### Exercise 6.3: Test and Refine (5 min)

**Task:** Verify complete functionality and refine as needed

1. **Comprehensive testing in Swagger UI:**
   
   Test POST /inventory/batch:
   - Batch with all valid products
   - Batch with all invalid products
   - Mixed batch (some valid, some duplicate codes, some invalid data)
   - Empty batch
   - Oversized batch (> 100 items)
   
   Test PATCH /inventory/batch:
   - Valid updates
   - Updates with non-existent IDs
   - Mixed valid/invalid
   
   Test DELETE /inventory/batch:
   - Valid deletions
   - Non-existent IDs
   - Mixed

2. **Review response format:**
   - Is success/failure clear?
   - Are error messages helpful?
   - Can you tell which specific items failed and why?

3. **If refinements needed:**
   
   Use Ask mode to understand what to change:
   ```
   The error messages for failed batch items aren't descriptive enough.
   How can I improve them to include:
   - Which item failed (by product_code or index)
   - Specific validation error
   - Example of correct format
   ```
   
   Then use Agent mode to apply the improvements.

**Expected Outcome:**
- Fully tested batch operations
- Clear success/failure reporting
- Helpful error messages
- Production-ready feature

---

### Part 6 Summary

You've implemented a complex feature using the complete workflow:

1. **Plan mode:** Strategic design and architecture
2. **Agent mode:** Autonomous implementation
3. **Ask mode:** Refinement and fixes
4. **Verification:** Comprehensive testing

**Key Insight:** Complex features are easier when you break them into: Design → Implement → Verify → Refine. Each step uses the right mode for the job!

---

## Part 7: Putting It All Together (10 minutes)

### Introduction: Your Turn!

Time to demonstrate everything you've learned. You'll implement a feature with minimal guidance, choosing the right modes and workflows yourself.

---

### Exercise 7.1: Low-Stock Alert System - You Choose! (10 min)

**Task:** Implement a low-stock alert system using the modes YOU think are appropriate

**Scenario:** The warehouse manager needs automatic alerts when products fall below threshold quantities.

**Requirements:**
1. GET /inventory/low-stock endpoint
   - Query parameter: threshold (default: 50)
   - Returns products with quantity < threshold
   - Sorted by quantity ascending (most critical first)
   - Include additional calculated fields:
     - percentage_below: float (how far below threshold)
     - severity: string ("critical" if < 20% of threshold, "warning" otherwise)

2. Add a LowStockProduct response model

3. Add method to database.py: get_low_stock(threshold: int)

4. Create comprehensive tests

**Your Decisions:**
- Should you start with Plan mode to design it? Or go straight to implementation?
- Use Agent mode for implementation? Or Ask mode with manual application?
- How will you test it?
- What edge cases should you consider?

**Instructions:**
1. **Choose your approach** - there's no single right answer!
2. **Implement the feature** using whatever modes make sense
3. **Test thoroughly** - unit tests AND Swagger UI
4. **Refine as needed** - use Ask/Agent to fix issues

**Guiding Questions:**
- Is the requirement clear enough to skip planning?
- Is this a good candidate for autonomous Agent implementation?
- Do you need to research anything first?

**Verify Your Solution:**
- [ ] Endpoint returns correct products below threshold
- [ ] Default threshold of 50 works
- [ ] Custom thresholds work
- [ ] Percentage calculation is correct
- [ ] Severity logic is correct (critical vs warning)
- [ ] Products sorted by quantity ascending
- [ ] Tests cover all scenarios
- [ ] Works in Swagger UI

**Time Challenge:** Can you complete this in 10 minutes using what you've learned?

**Expected Outcome:**
- Working low-stock endpoint
- Comprehensive tests
- Confidence in mode selection
- Demonstration of learned skills
- Pride in autonomous work!

---

### Part 7 Summary

You've demonstrated:
- **Autonomous mode selection** - choosing the right tool for the job
- **Independent implementation** - completing features with minimal guidance
- **Complete workflow** - design, implement, test, verify
- **Production-ready code** - with tests and error handling

**Congratulations!** You've completed the GitHub Copilot Intermediate Lab. You can now:
- Use Plan, Agent, and Ask modes effectively
- Automate test creation
- Debug systematically
- Implement complex features
- Work autonomously with AI assistance

---



## Lab Completion Checklist

### Skills Mastered
- [ ] Understand when to use Plan, Agent, and Ask modes
- [ ] Write specific, constrained prompts for better results
- [ ] Use iterative refinement to improve code generation
- [ ] Apply few-shot prompting with examples
- [ ] Create Custom Instructions for persistent coding standards
- [ ] Build Custom Prompts for reusable slash commands
- [ ] Automate test generation with Plan → Agent workflow
- [ ] Generate comprehensive parametrized test suites
- [ ] Test error conditions and edge cases
- [ ] Debug systematically with Plan → Agent workflow
- [ ] Trace bugs through multiple files
- [ ] Implement fixes with Agent mode autonomously
- [ ] Extract and refactor code safely
- [ ] Add comprehensive error handling
- [ ] Implement structured logging
- [ ] Refactor safely with test verification
- [ ] Design complex features with Plan mode
- [ ] Implement complex features with Agent mode
- [ ] Choose appropriate modes for different tasks

### Deliverables Completed
1. ✅ Custom Instructions for project coding standards
2. ✅ Custom Prompts library (slash commands)
3. ✅ Comprehensive automated test suite
4. ✅ Fixed bugs systematically (area calculation, validation, weight calculations)
5. ✅ Refactored code with improved structure
6. ✅ Batch operations feature (create, update, delete)
7. ✅ Low-stock alerts endpoint
8. ✅ Professional documentation and bug reports

### Key Metrics
- **Modes Mastered:** Plan, Agent, Ask - all three
- **Test Coverage:** Significantly increased from basic lab
- **Code Quality:** Improved with validation, logging, error handling
- **Features Added:** 5+ new endpoints and capabilities
- **Bugs Fixed:** 6+ issues resolved
- **Workflow Efficiency:** Plan → Agent automation mastered

---

## Appendix: Intermediate Tips and Best Practices

### Keyboard Shortcuts (Review)
- `Tab` - Accept inline suggestion
- `Esc` - Dismiss suggestion
- `Alt+]` - Next suggestion
- `Alt+[` - Previous suggestion
- `Ctrl+I` - Open inline chat
- `Ctrl+Shift+I` - Open Copilot Chat panel

### Mode Selection Guide

| Mode | Use When | Best For |
|------|----------|----------|
| **Ask** | Quick questions, explanations | Learning, understanding code |
| **Plan** | Complex features, design decisions | Research, architecture, planning |
| **Agent** | Clear implementation tasks | Execution, multi-file changes |

### Prompt Engineering Patterns

#### Pattern 1: Constraint-Heavy Prompt
```
#file:example.py Create a function that:
- Input: [specific types]
- Output: [specific format]
- Validates: [specific conditions]
- Raises: [specific exceptions]
- Style: [specific patterns]
- Include: [specific elements]
```

#### Pattern 2: Example-Driven Prompt
```
#file:example.py I need consistent formatting.

Good example:
[paste example code]

Bad example:
[paste what to avoid]

Apply this pattern to [specific files/functions].
```

#### Pattern 3: Iterative Refinement
```
Initial: Create a function for [basic description]
↓
Refine: Add [specific requirement] to the function
↓
Refine: Also handle [edge case]
↓
Finalize: Document with [specific details]
```

### Testing Best Practices with Copilot

1. **Test First, Code Second**
   - Ask Copilot to generate tests before implementation
   - Tests define expected behavior clearly

2. **Parametrize Everything**
   - Use `@pytest.mark.parametrize` for multiple scenarios
   - Ask Copilot to generate comprehensive parameter lists

3. **Test Edge Cases**
   - Explicitly ask for edge case tests
   - Include negative tests (what should fail)

4. **Independent Tests**
   - Each test should be runnable independently
   - Use fixtures for setup/teardown

### Debugging Workflow with Copilot

1. **Reproduce** → Ask Copilot to help create reproduction steps
2. **Trace** → Use #codebase to trace request flow
3. **Isolate** → Ask for root cause in specific files
4. **Fix** → Request fix with tests
5. **Verify** → Run tests to confirm fix
6. **Document** → Create bug report for records

### Refactoring Safely

1. **Tests First** → Ensure good test coverage before refactoring
2. **Small Steps** → Refactor incrementally, test after each change
3. **One Thing at a Time** → Extract method, then rename, then optimize
4. **Ask for Review** → Have Copilot review refactored code
5. **Verify Behavior** → All tests should still pass

### Plan Mode Best Practices

- **Research first** → Ask about approaches before choosing
- **Phased planning** → MVP first, then enhancements
- **Consider trade-offs** → Ask about pros/cons of each approach
- **Document decisions** → Keep the plan for reference
- **Iterate freely** → Planning is cheap, refine until clear

### Agent Mode Best Practices

- **Clear requirements** → Specific is better than vague
- **Set constraints** → Tell Agent what NOT to do
- **Monitor progress** → Watch for wrong turns early
- **Review everything** → Agent is good, not perfect
- **Correct quickly** → Don't let Agent go too far wrong
- **Use with Plan** → Implement plans from Plan mode

### Common Pitfalls to Avoid

❌ **Vague prompts** → Be specific about requirements  
❌ **No context** → Always use #file or #codebase  
❌ **Blind acceptance** → Review all generated code  
❌ **No testing** → Always generate tests with code  
❌ **Large changes without tests** → Refactor with test safety net  
❌ **Agent for exploration** → Use Plan mode for design, Agent for execution  
❌ **Ignoring errors** → Fix issues immediately, don't accumulate debt  

### Advanced Context Techniques

1. **Multi-file context:**
   ```
   #file:models.py #file:database.py #file:inventory.py
   How do these three files interact?
   ```

2. **Codebase search:**
   ```
   #codebase Where is validation logic currently implemented?
   ```

3. **Selective context:**
   ```
   [Select specific function in editor]
   #selection Refactor this function following DRY principles
   ```

4. **Architecture questions:**
   ```
   #codebase Describe the overall architecture and data flow
   ```

---

## Next Steps

**Congratulations!** You've completed the GitHub Copilot Intermediate Lab. You now have advanced skills for:

- Engineering effective prompts
- Creating persistent Custom Instructions and reusable Custom Prompts
- Generating comprehensive tests
- Debugging complex issues
- Refactoring safely
- Planning strategically with Plan mode
- Implementing autonomously with Agent mode

### Continue Learning

1. **Practice on Real Projects**
   - Apply these techniques to your actual work
   - Refine your custom instructions based on your team's standards
   - Expand your custom prompt library with project-specific tasks
   - Experiment with different prompt styles

2. **Expand Your Custom Setup**
   - Add more domain-specific instructions for your industry
   - Create prompts for your most repetitive tasks
   - Share your `.instructions.md` and `.prompt.md` files with your team
   - Version control your custom configurations (`.github/instructions/` and `.github/prompts/`)

3. **Advanced Integrations**
   - Integrate custom instructions with CI/CD pipelines
   - Create workspace-specific patterns for different project types
   - Build organization-wide prompt libraries
   - Combine with other VS Code extensions

4. **Share Knowledge**
   - Teach these techniques to your team
   - Demonstrate the productivity gains from custom instructions/prompts
   - Collaborate on team-wide prompt libraries
   - Document your most effective patterns

4. **Stay Updated**
   - Copilot evolves rapidly
   - New models and modes are added
   - Follow GitHub Copilot updates

### Feedback

This lab is continuously improved. Share your experience:
- What worked well?
- What was confusing?
- What exercises would you add?
- How did this help your workflow?

---

**You're now an intermediate Copilot user!** Use these skills to code faster, debug smarter, and build better software with AI assistance.
