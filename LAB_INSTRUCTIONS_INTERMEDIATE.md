# GitHub Copilot Intermediate Lab Instructions
**Duration:** 3 hours  
**Level:** Intermediate (For users with some development experience)

---

## Pre-Lab Setup (15 minutes before lab)

### 1. Prerequisites
- GitHub Copilot license activated
- VS Code with GitHub Copilot extensions installed
- Python 3.9+ installed
- Git configured
- Basic familiarity with Copilot Chat and inline suggestions

### 2. Clone and Environment Setup
Clone this repository and set up the environment:
```bash
cd steel-inventory-api
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Verify Setup
Start the application to ensure everything works:
```bash
uvicorn app.main:app --reload
```
Visit http://localhost:8000/docs - you should see the API documentation.

### 4. Install Testing Dependencies
```bash
pip install pytest pytest-cov coverage
```

### 5. Initialize Git Repository
```bash
git init
git add .
git commit -m "Initial project scaffold"
git branch -M main
```

### 6. Enable Copilot Features
- Open VS Code Settings (Ctrl+,)
- Search for "Copilot Memory" and enable it
- Verify Agent mode is available in Copilot Chat
- Enable Copilot inline suggestions

---

## Lab Overview

This intermediate lab focuses on professional development workflows with GitHub Copilot. You'll master agent mode for complex tasks, use AI to dramatically increase test coverage, debug efficiently, and create custom instructions to enhance Copilot's effectiveness for your specific needs.


### What You'll Learn
- [ ] Mastering Copilot Agent mode for automatic code editing
- [ ] Understanding when to use Ask mode vs Agent mode vs Plan mode
- [ ] Leveraging Agent mode for multi-file code changes and refactoring
- [ ] Using Copilot to increase test coverage from baseline to 95%+
- [ ] Writing comprehensive test suites with AI assistance
- [ ] Debugging workflows: finding and fixing bugs systematically
- [ ] Using Copilot for error analysis and resolution
- [ ] Creating workspace-specific custom instructions
- [ ] Writing effective custom prompts for repetitive tasks
- [ ] Configuring Copilot behavior for your team's standards

---

## Part 1: Agent Mode Deep-Dive (60 minutes)

### Understanding Agent Mode

**What is Agent Mode?**
Agent mode is Copilot's autonomous problem-solving mode. Both Ask and Agent modes can search and analyze your codebase, but the key difference is:

**Ask Mode:**
- Searches and analyzes files
- Answers questions about your code
- Provides suggestions and code snippets
- You manually copy/paste changes

**Agent Mode:**
- Everything Ask mode does, PLUS:
- Automatically edits files for you
- Makes changes across multiple files
- Executes complex multi-step tasks
- Implements solutions end-to-end

**When to Use Ask Mode:**
- You want suggestions without automatic changes
- Asking questions and learning
- Getting explanations and best practices
- Reviewing options before deciding

**When to Use Agent Mode:**
- You want automatic code changes
- Complex refactoring across multiple files
- Implementing features that touch many components
- Making systematic improvements throughout the codebase

---

### Exercise 1.1: Agent Mode vs Ask Mode (15 min)

**Task:** Compare the two modes on the same task

1. **First, try with Ask mode:**
   
   Open Copilot Chat in **Ask mode** (default), and ask:
   ```
   Find all the places in this codebase where we don't handle errors properly. 
   List each location with the file path and line number.
   ```
   
   **Expected:** Ask mode will search your files and provide an analysis with file paths and line numbers, but it won't make any changes.

2. **Now try with Agent mode:**
   
   Switch to **Agent mode** (toggle at the top of Copilot Chat):
   ```
   Find all the places in this codebase where we don't handle errors properly. 
   Then add proper error handling to all endpoints in routers/inventory.py.
   ```
   
   **Expected:** Agent mode will:
   - Read through your codebase (like Ask mode)
   - Analyze error handling patterns
   - Provide specific file paths and line numbers
   - **AND automatically make the necessary code changes**

3. **Understanding the key difference:**
   
   Ask in **Ask mode**:
   ```
   Add proper error handling to all endpoints in routers/inventory.py. 
   Use HTTPException for client errors and include appropriate status codes.
   ```
   
   **Expected:** Ask mode will provide suggestions and code snippets, but you'll need to copy and apply them manually.
   
   Ask the same thing in **Agent mode**:
   ```
   Add proper error handling to all endpoints in routers/inventory.py. 
   Use HTTPException for client errors and include appropriate status codes.
   ```
   
   **Expected:** Agent mode will automatically edit the file and show you the changes.

**Key Takeaway:**
- **Ask mode:** Can search files and answer questions, but doesn't edit code
- **Agent mode:** Can search files, answer questions, AND automatically edit code across multiple files

---

### Exercise 1.2: Multi-File Refactoring with Agent Mode (25 min)

**Task:** Use Agent mode to refactor across multiple files

1. **Plan a refactoring:**
   
   In **Agent mode**:
   ```
   I want to add a logging system throughout the application. 
   
   First, analyze where logging would be most valuable (endpoints, database operations, 
   error handlers). Don't implement yet - just create a plan.
   ```

2. **Implement the logging system:**
   ```
   Now implement the logging plan:
   1. Create app/utils/logger.py with a configured logger
   2. Add logging to all API endpoints (request/response)
   3. Add logging to database operations
   4. Add logging to error handlers
   5. Use appropriate log levels (INFO, WARNING, ERROR)
   
   Make all necessary changes across all files.
   ```
   
   Agent mode will:
   - Create the new logger.py file
   - Edit multiple files (main.py, routers/*.py, database.py)
   - Add consistent logging throughout
   - Show you all changes made

3. **Verify the implementation:**
   ```bash
   # Start the app
   uvicorn app.main:app --reload
   
   # Make some API requests
   # Check that logging appears in console
   ```

4. **Refine with Agent mode:**
   ```
   Update the logging to:
   - Include request IDs for tracing
   - Add structured logging (JSON format)
   - Log performance metrics (response time)
   ```

5. **Create configuration:**
   ```
   Add a configuration system that allows:
   - Setting log level via environment variable
   - Configuring log output (console vs file)
   - Enabling/disabling performance logging
   
   Update all necessary files.
   ```

**Expected Outcome:**
- Complete logging system implemented
- Multiple files edited consistently
- Understanding of Agent mode's multi-file capabilities
- Professional logging setup

---

### Exercise 1.3: Using Plan Mode (20 min)

**What is Plan Mode?**
Plan mode is Copilot's strategic planning feature that helps you break down complex tasks into actionable steps before implementing. Unlike Agent mode (which executes code changes), Plan mode only creates a structured roadmap that you can review and modify. You then use Agent mode to implement the plan step-by-step.

**When to Use Plan Mode:**
- Complex features requiring multiple steps
- Architecture decisions needing breakdown
- Large refactoring projects
- Features with many dependencies
- When you need a roadmap before coding

**Task:** Use Plan mode to architect a new feature

1. **Access Plan mode:**
   
   In Copilot Chat, look for the **Plan** mode option (alongside Ask and Agent modes).
   
   Switch to **Plan mode**.

2. **Request a feature plan:**
   ```
   I need to add a bulk import feature that allows users to upload a CSV file with multiple steel products and import them all at once.
   
   Create a comprehensive implementation plan including:
   - File upload endpoint design
   - CSV parsing logic
   - Validation requirements
   - Batch processing approach
   - Error handling strategy
   - Testing approach
   ```

3. **Review the generated plan:**
   
   Plan mode will generate a step-by-step breakdown with:
   - Sequential tasks
   - Dependencies between tasks
   - Code snippets for each step
   - Testing recommendations

4. **Switch to Agent mode to execute the plan:**
   
   Plan mode creates the roadmap but doesn't implement it. To execute:
   
   Switch to **Agent mode** and say:
   ```
   Implement step 1 from the plan: Create the file upload endpoint
   ```
   
   Normally you would continue step by step [READ-ONLY]:
   - Test step 1
   - Switch back to Agent mode: "Implement step 2: CSV parsing"
   - Test step 2
   - Continue through each step of the plan

5. **Modify the plan:**
   
   Switch back to **Plan mode** to adjust the roadmap:
   ```
   Update the plan to also include:
   - Progress tracking for large files
   - Rollback on validation errors
   - Duplicate detection during import
   ```
   
   Plan mode will incorporate these requirements into the existing plan. Then you'd use **Agent mode** again to implement the updated steps.

6. **Compare approaches:**
   ```
   Compare two implementation approaches:
   Approach A: Process CSV synchronously, return results immediately
   Approach B: Queue import job, process asynchronously, notify on completion
   
   Which is better for large files (10,000+ products)?
   ```

**Expected Outcome:**
- Understanding of Plan mode capabilities (planning only, no execution)
- Structured approach to complex features
- Ability to break down complex tasks into manageable steps
- Knowledge of when to use Plan mode for planning, then Agent mode for execution

**Comparison Summary:**
- **Ask mode:** Searches files, answers questions, provides suggestions (no automatic edits)
- **Agent mode:** Searches files, answers questions, AND automatically edits code (execution)
- **Plan mode:** Creates strategic plans and roadmaps (no execution - use Agent mode to implement)

---

## Part 2: Test Automation with Copilot - Achieving 95% Coverage (75 minutes)

### Understanding Test Coverage

**What is Test Coverage?**
The percentage of your code that is executed by your tests. Higher coverage means more of your code is tested, reducing bugs.

**Coverage Goals:**
- **60-70%**: Minimum for production
- **80%**: Good coverage
- **95%**: Excellent (our goal!)
- **100%**: Often impractical and unnecessary

---

### Exercise 2.1: Measure Current Coverage (10 min)

1. **Check existing tests:**
   ```bash
   pytest tests/ -v
   ```

2. **Generate coverage report:**
   ```bash
   pytest --cov=app --cov-report=html --cov-report=term
   ```
   
   This creates:
   - Terminal summary showing % coverage per file
   - HTML report in `htmlcov/index.html`

3. **View detailed coverage:**
   ```bash
   # Open the HTML report
   start htmlcov/index.html   # Windows
   ```
   
   The report shows:
   - Lines covered (green)
   - Lines not covered (red)
   - Missing branches

4. **Note your baseline:**
   
   In **Ask mode**:
   ```
   Analyze this coverage report. What's our current overall coverage percentage?
   Which files have the lowest coverage?
   ```
   
   **Write down your current coverage %: ________**

**Expected Baseline:** Likely 20-40% coverage initially

---

### Exercise 2.2: Generate Comprehensive Tests with Agent Mode (30 min)

**Task:** Use Agent mode to systematically increase test coverage

1. **Start with the lowest coverage file:**
   
   In **Agent mode**:
   ```
   I need to increase test coverage for app/routers/inventory.py to at least 95%.
   
   Current coverage is low. Analyze the file and create comprehensive tests for:
   - All endpoints (GET, POST, PUT, DELETE)
   - Success cases
   - Error cases (invalid data, not found, duplicates)
   - Edge cases (empty values, special characters, boundary conditions)
   - All branches and conditions
   
   Add tests to tests/test_inventory.py
   ```

2. **Agent mode will analyze and generate tests:**
   
   It will create tests like:
   ```python
   def test_create_product_success():
       """Test creating a product with valid data"""
       
   def test_create_product_invalid_grade():
       """Test creating product with invalid steel grade"""
       
   def test_create_product_negative_quantity():
       """Test creating product with negative quantity"""
       
   def test_get_product_not_found():
       """Test getting non-existent product"""
       
   def test_update_product_partial():
       """Test partial update of product"""
       
   # ... and many more
   ```

3. **Run the new tests:**
   ```bash
   pytest tests/test_inventory.py -v
   ```

4. **Check coverage improvement:**
   ```bash
   pytest --cov=app.routers.inventory --cov-report=term
   ```
   
   **New coverage for inventory.py: ________%**

5. **Fill remaining gaps:**
   
   If not at 95% yet:
   ```
   The coverage for routers/inventory.py is now at X%. 
   Check the coverage report and add tests for any remaining uncovered lines.
   
   Focus on:
   - Uncovered branches in if/else statements
   - Exception handling paths
   - Edge cases not yet tested
   ```

6. **Repeat for database.py:**
   
   In **Agent mode**:
   ```
   Now increase test coverage for app/database.py to 95%.
   
   Create tests for all database operations:
   - CRUD operations
   - Search and filter functions
   - Data validation
   - Edge cases (empty database, duplicate handling, etc.)
   
   Add tests to tests/test_database.py (create this file if needed)
   ```

7. **Test models.py:**
   ```
   Increase coverage for app/models.py to 95%.
   
   Test all Pydantic models:
   - Valid data creation
   - Validation failures
   - Type conversions
   - Optional fields
   - Default values
   
   Add to tests/test_models.py
   ```

8. **Test utility functions:**
   ```
   Increase coverage for app/utils/steel_utils.py to 95%.
   
   Test all calculation functions:
   - Correct calculations with valid inputs
   - Edge cases (zero values, extreme sizes)
   - Invalid inputs (negative numbers, wrong types)
   - Boundary conditions
   
   Add to tests/test_steel_utils.py
   ```

**Expected Outcome:**
- Comprehensive test files created
- Most individual files at 90-95% coverage
- Hundreds of test cases generated

---

### Exercise 2.3: Achieve 95% Overall Coverage (20 min)

**Task:** Use Agent mode to reach 95% project-wide coverage

1. **Check overall coverage:**
   ```bash
   pytest --cov=app --cov-report=html --cov-report=term-missing
   ```
   
   The `term-missing` flag shows which lines are still uncovered.

2. **Identify gaps systematically:**
   
   In **Agent mode**:
   ```
   Analyze our test coverage report. We're currently at X% overall coverage.
   
   Create a plan to reach 95% by:
   1. Identifying all files below 95%
   2. Listing uncovered lines in each file
   3. Determining what tests are needed for those lines
   4. Prioritizing by impact
   
   Show me the plan before implementing.
   ```

3. **Implement the gap-filling tests:**
   ```
   Execute the plan. Generate all missing tests to bring overall coverage to 95%.
   
   For each uncovered line:
   - Determine what scenario triggers it
   - Write a test for that scenario
   - Ensure the test is meaningful (not just coverage hunting)
   ```

4. **Run full test suite:**
   ```bash
   pytest tests/ -v --cov=app --cov-report=term --cov-report=html
   ```

5. **Verify 95% target:**
   
   Check the terminal output:
   ```
   TOTAL     XXX   XXX    95%
   ```
   
   **Final coverage: ________%**

6. **Document your tests:**
   
   In **Agent mode**:
   ```
   Add a comprehensive docstring to each test file explaining:
   - What aspects of the module are tested
   - Coverage achieved
   - Any areas intentionally not tested (and why)
   ```

**Expected Outcome:**
- Overall test coverage ≥ 95%
- All critical code paths tested
- Fast, reliable test suite
- HTML coverage report showing green across the board

---

### Exercise 2.4: Maintain High Coverage (15 min)

**Task:** Set up processes to maintain 95% coverage

1. **Add coverage enforcement:**
   
   In **Agent mode**:
   ```
   Create a pytest.ini configuration that:
   - Requires minimum 95% coverage for tests to pass
   - Fails the build if coverage drops below 95%
   - Excludes test files from coverage
   - Generates coverage reports automatically
   ```

2. **Verify configuration:**
   ```bash
   pytest
   ```
   
   Should fail if coverage < 95%.

3. **Create a pre-commit hook:**
   ```
   Create a git pre-commit hook that runs tests and checks coverage before allowing commits.
   
   The hook should:
   - Run pytest with coverage
   - Block commit if coverage < 95%
   - Block commit if any tests fail
   - Show clear error message
   ```

4. **Test the hook:**
   ```bash
   # Make a change that reduces coverage
   # Try to commit
   git add .
   git commit -m "test"
   
   # Should be blocked if coverage drops
   ```

5. **Document testing practices:**
   
   In **Agent mode**:
   ```
   Create a TESTING.md file that documents:
   - How to run tests
   - Coverage requirements
   - How to write good tests
   - Testing patterns used in this project
   - How to debug failing tests
   ```

**Expected Outcome:**
- Coverage requirements enforced automatically
- Team standards documented
- Sustainable high-coverage practices

---

## Part 3: Debugging Workflows (40 minutes)

### Exercise 3.1: Systematic Bug Discovery (15 min)

**Task:** Use Copilot to find bugs proactively

1. **Static analysis with Agent mode:**
   
   In **Agent mode**, drag the `app` folder:
   ```
   Perform a comprehensive bug hunt across this codebase.
   
   Look for:
   1. Logic errors (incorrect calculations, wrong conditions)
   2. Runtime errors (null pointer, type errors, index out of range)
   3. Data validation gaps (missing checks, incorrect validation)
   4. Race conditions (if applicable)
   5. Resource leaks (unclosed files, connections)
   6. Error handling gaps (unhandled exceptions)
   
   For each bug found:
   - File path and line number
   - Severity (Critical, High, Medium, Low)
   - Description of the bug
   - Example of how it could manifest
   ```

2. **Prioritize findings:**
   ```
   Of the bugs you found, which are the top 5 most critical that could cause 
   production issues? Explain the impact of each.
   ```

3. **Create bug tickets:**
   ```
   For each of the top 5 bugs, create a bug report in markdown format with:
   - Title
   - Severity
   - Location (file:line)
   - Description
   - Steps to reproduce
   - Expected vs actual behavior
   - Suggested fix
   ```

**Expected Outcome:**
- List of 10-20 potential bugs
- Prioritized by severity
- Documented bug reports
- Proactive bug discovery

---

### Exercise 3.2: Debugging with Copilot (25 min)

**Task:** Use Copilot to fix bugs systematically

1. **Fix Bug #1: Duplicate Product Codes**
   
   In **Agent mode**:
   ```
   There's a bug where we can create products with duplicate product codes.
   
   Step 1: Find where this happens in database.py
   Step 2: Explain why duplicates are allowed
   Step 3: Implement a fix that:
      - Checks for existing product codes before creating
      - Raises a clear error if duplicate exists
      - Returns appropriate HTTP status (409 Conflict)
   Step 4: Add a test that verifies duplicates are prevented
   ```

2. **Fix Bug #2: Negative Quantity Handling**
   ```
   Products can be created with negative quantities, which doesn't make sense.
   
   Fix this by:
   1. Adding validation in the Pydantic model (models.py)
   2. Adding a validator that ensures quantity >= 0
   3. Providing a clear error message
   4. Adding tests for both positive and negative quantities
   ```

3. **Fix Bug #3: Missing Timestamp Updates**
   ```
   When updating a product, the last_updated timestamp isn't being set.
   
   Debug and fix:
   1. Find the update function in database.py
   2. Identify why last_updated isn't being set
   3. Fix it to set last_updated to current datetime on every update
   4. Add a test that verifies timestamps update correctly
   ```

4. **Debug a performance issue:**
   ```
   The GET /inventory endpoint seems slow when there are many products.
   
   Analyze and optimize:
   1. Review the current implementation
   2. Identify performance bottlenecks
   3. Suggest optimizations (caching, pagination, indexing)
   4. Implement the most impactful optimization
   5. Explain the performance improvement
   ```

5. **Use Copilot for error analysis:**
   
   Intentionally break something:
   ```python
   # In database.py, temporarily change:
   def get_product(self, product_id: str):
       return self.products[product_id]  # This will raise KeyError if not found
   ```
   
   Run the app and test:
   ```bash
   uvicorn app.main:app --reload
   # Try to get a non-existent product in browser or with curl
   ```
   
   Copy the error traceback and paste it in **Agent mode**:
   ```
   I'm getting this error: [paste traceback]
   
   1. Explain what's causing this error
   2. Show me the problematic line
   3. Provide a fix with proper error handling
   4. Explain why the fix works
   ```

6. **Debug with context:**
   
   In **Agent mode**, attach relevant files:
   ```
   #file:database.py
   #file:models.py
   #file:routers/inventory.py
   
   I'm trying to update a product but getting a validation error. 
   Walk me through the data flow:
   1. What data comes from the API request?
   2. How is it validated?
   3. Where could validation fail?
   4. What's the most likely cause?
   ```

**Expected Outcome:**
- Multiple bugs fixed systematically
- Understanding of Copilot-assisted debugging
- Faster bug resolution
- Better error handling throughout

---

## Part 4: Custom Instructions and Custom Prompts (45 minutes)

### Understanding Custom Instructions

**What are Custom Instructions?**
Custom instructions tell Copilot how to behave specifically for your project. They can:
- Define coding standards
- Specify frameworks and patterns to use
- Set documentation requirements
- Establish testing practices
- Configure file organization preferences

**Types of Custom Instructions:**
1. **Workspace instructions** (.github/copilot-instructions.md) - Apply to entire project
2. **Inline instructions** - Comments in code files
3. **File-specific instructions** - Apply to specific file types

---

### Exercise 4.1: Create Workspace Custom Instructions (20 min)

**Task:** Create project-wide coding standards

1. **Create the instructions file:**
   
   In **Agent mode**:
   ```
   Create .github/copilot-instructions.md for this Python/FastAPI project.
   
   Include instructions for:
   
   **Code Style:**
   - Use Python type hints for all function parameters and returns
   - Follow PEP 8 naming conventions
   - Use docstrings for all functions (Google style)
   - Maximum line length: 100 characters
   - Use f-strings for string formatting (not %)
   
   **FastAPI Patterns:**
   - Use dependency injection for database access
   - Define response models for all endpoints
   - Use HTTPException for errors with appropriate status codes
   - Add OpenAPI tags to all routers
   - Include example values in models
   
   **Testing:**
   - Every function needs a test
   - Use pytest fixtures for setup/teardown
   - Aim for 95% coverage minimum
   - Test success cases, error cases, and edge cases
   - Use descriptive test names starting with test_
   
   **Documentation:**
   - Add docstrings explaining purpose, parameters, and return values
   - Update README.md when adding new features
   - Comment complex logic
   - Document API endpoints with descriptions and examples
   
   **Error Handling:**
   - Never use bare except clauses
   - Log all errors with context
   - Return user-friendly error messages
   - Use appropriate HTTP status codes
   ```

2. **Test the instructions:**
   
   In **Ask mode**, ask Copilot to create a new function:
   ```
   Create a new endpoint in routers/inventory.py that calculates the total value 
   of all inventory (quantity × unit_price for each product).
   ```
   
   **Expected:** Copilot follows your instructions:
   - Includes type hints
   - Has docstring
   - Uses dependency injection
   - Includes error handling
   - Has response model

3. **Verify instructions are active:**
   
   In **Ask mode**:
   ```
   What coding standards should I follow in this project?
   ```
   
   **Expected:** Copilot cites your custom instructions.

4. **Add project-specific domain knowledge:**
   
   Edit `.github/copilot-instructions.md` to add:
   ```markdown
   ## Steel Industry Domain Knowledge
   
   This is a steel inventory management system. Keep these in mind:
   
   **Common Steel Grades:**
   - A36: Structural steel
   - 304: Stainless steel (corrosion resistant)
   - 316L: Marine-grade stainless (highest corrosion resistance)
   - A572-50: High-strength low-alloy steel
   
   **Steel Shapes:**
   - Plate: Flat sheets
   - Coil: Rolled sheets
   - Beam: I-beams, H-beams
   - Bar: Round, square, or rectangular bars
   - Tube: Hollow circular or rectangular
   
   **Important Validations:**
   - Product codes must be unique
   - Quantities cannot be negative
   - Dimensions must be positive numbers
   - Steel grades should be from the known list
   - Prices must be positive
   
   **Business Rules:**
   - Minimum order quantity: 1
   - Weight calculations are critical for shipping and pricing
   - Last updated timestamps are required for audit trails
   ```

5. **Test domain knowledge:**
   ```
   Create a function that recommends the appropriate steel grade based on use case:
   - Marine/coastal: 316L
   - General corrosion resistance: 304
   - Structural: A36
   - High strength: A572-50
   ```
   
   **Expected:** Copilot uses your domain knowledge to create accurate recommendations.

---

### Exercise 4.2: Create Custom Prompts for Repetitive Tasks (15 min)

**Task:** Create reusable prompts for common tasks

1. **Create a prompts directory:**
   ```bash
   mkdir .vscode/prompts
   ```

2. **Create a test generation prompt:**
   
   Create `.vscode/prompts/generate-tests.md`:
   ```markdown
   # Generate Tests
   
   Create comprehensive tests for the selected code.
   
   Requirements:
   - Test all success scenarios
   - Test all error scenarios (invalid inputs, not found, duplicates)
   - Test edge cases (boundary values, empty inputs, special characters)
   - Use pytest fixtures for setup
   - Use descriptive test names
   - Aim for 95%+ coverage of the tested code
   - Include docstrings explaining what each test does
   
   Test file naming: tests/test_{module_name}.py
   ```

3. **Create a refactoring prompt:**
   
   Create `.vscode/prompts/refactor-function.md`:
   ```markdown
   # Refactor Function
   
   Refactor the selected function following these principles:
   
   1. **Simplify:** Break down complex functions into smaller, single-purpose functions
   2. **Type Safety:** Add type hints to all parameters and return values
   3. **Documentation:** Add or improve docstring with purpose, parameters, returns, and examples
   4. **Error Handling:** Add proper error handling with specific exceptions
   5. **DRY:** Extract repeated logic into helper functions
   6. **Readability:** Use descriptive variable names, add comments for complex logic
   
   After refactoring, explain what changes were made and why.
   ```

4. **Create an endpoint generation prompt:**
   
   Create `.vscode/prompts/create-endpoint.md`:
   ```markdown
   # Create FastAPI Endpoint
   
   Create a new FastAPI endpoint with the following characteristics:
   
   **Structure:**
   - Define in appropriate router file (routers/*.py)
   - Use dependency injection for database access
   - Define Pydantic models for request/response
   - Include OpenAPI documentation (description, tags, responses)
   - Add example values to models
   
   **Error Handling:**
   - Use HTTPException with appropriate status codes
   - 400 for invalid input
   - 404 for not found
   - 409 for conflicts (duplicates)
   - Include descriptive error messages
   
   **Testing:**
   - Generate tests for this endpoint in tests/test_{router}.py
   - Test success case
   - Test all error cases
   - Test input validation
   
   **Documentation:**
   - Add docstring to the endpoint function
   - Update API documentation if needed
   ```

5. **Create a bug fix prompt:**
   
   Create `.vscode/prompts/fix-bug.md`:
   ```markdown
   # Fix Bug
   
   Fix the selected bug following this process:
   
   1. **Understand:** Explain what the bug is and why it occurs
   2. **Locate:** Identify the exact line(s) of code causing the issue
   3. **Fix:** Implement the fix with proper error handling
   4. **Test:** Create a test that:
      - Would fail before the fix
      - Passes after the fix
      - Prevents regression
   5. **Document:** Add comments explaining the fix if the logic is complex
   
   Show me:
   - The problematic code (before)
   - The fixed code (after)
   - The test that validates the fix
   - Explanation of why the fix works
   ```

6. **Use your custom prompts:**
   
   Select a function in your code, then in **Ask mode**:
   ```
   Use the prompt in .vscode/prompts/refactor-function.md
   ```
   
   Or reference it:
   ```
   #file:.vscode/prompts/refactor-function.md Refactor this function: [paste function]
   ```

---

### Exercise 4.3: File-Specific Custom Instructions (10 min)

**Task:** Add inline instructions for specific files

1. **Add instructions to models.py:**
   
   At the top of `app/models.py`, add:
   ```python
   """
   Pydantic Models for Steel Inventory API
   
   COPILOT INSTRUCTIONS:
   - All models must have examples in Config.schema_extra
   - Use Field() for validation (min/max values, regex patterns)
   - Add descriptions to all fields
   - Create separate response models (don't reuse request models)
   - Add validators for business rules (positive quantities, valid grades)
   - Use Optional[] for fields that can be None
   """
   ```

2. **Test the file-specific instruction:**
   
   In **Ask mode**, with models.py open:
   ```
   Add a new model for a Purchase Order:
   - PO number (required, unique string)
   - Product ID (required)
   - Quantity (required, positive integer)
   - Unit price (required, positive decimal)
   - Order date (required, datetime)
   - Delivery date (optional, datetime, must be after order date)
   - Status (required, enum: pending/approved/shipped/delivered)
   ```
   
   **Expected:** Copilot creates the model following all your instructions in the docstring.

3. **Add instructions to database.py:**
   ```python
   """
   In-Memory Database for Steel Inventory
   
   COPILOT INSTRUCTIONS:
   - All methods must validate inputs before processing
   - Raise ValueError for invalid inputs with descriptive messages
   - Check for duplicates before creating (product codes must be unique)
   - Always update last_updated timestamp on modifications
   - Return copies of data, not references (prevent external modification)
   - Add logging for all database operations
   - Use type hints for all methods
   """
   ```

4. **Add instructions to test files:**
   ```python
   """
   Tests for Inventory Router
   
   COPILOT INSTRUCTIONS:
   - Every endpoint needs at least 3 tests: success, error, edge case
   - Use TestClient fixture for API testing
   - Use descriptive test names: test_{endpoint}_{scenario}
   - Add docstrings explaining what each test verifies
   - Test for correct status codes and response structure
   - Test error messages are clear and helpful
   - Aim for 95%+ coverage
   """
   ```

5. **Verify file-specific instructions work:**
   
   Open a file with instructions, then ask Copilot to add something:
   ```
   Add a new database method to search products by price range (min_price, max_price)
   ```
   
   **Expected:** Follows the specific instructions in that file.

---

## Wrap-Up and Best Practices (10 minutes)

### Key Takeaways

1. **Mode Understanding:**
   - **Ask mode:** Great for questions, analysis, and suggestions without automatic edits
   - **Agent mode:** Use when you want Copilot to automatically make code changes
   - **Plan mode:** Use for breaking down complex tasks into structured plans
   - Both Ask and Agent can search your codebase; only Agent makes automatic edits

2. **Test Coverage:**
   - Aim for 95% minimum
   - Use Agent mode to generate comprehensive tests
   - Cover success, error, and edge cases
   - Maintain coverage with automated checks

3. **Debugging:**
   - Use Copilot for systematic bug discovery
   - Provide error context for better help
   - Fix bugs with test-first approach
   - Document fixes to prevent regression

4. **Custom Instructions:**
   - Document your team's standards
   - Include domain knowledge
   - Create reusable prompts for common tasks
   - Use file-specific instructions where needed

### Next Steps

1. **Practice:** Use these techniques in your daily work
2. **Customize:** Adapt instructions to your team's needs
3. **Share:** Document learnings for your team
4. **Iterate:** Refine prompts and instructions based on results

---

## Additional Resources

- [GitHub Copilot Documentation](https://docs.github.com/copilot)
- [FastAPI Documentation](https://fastapi.tiangolo.com)
- [Pytest Documentation](https://docs.pytest.org)
- [Python Testing Best Practices](https://docs.python-guide.org/writing/tests/)

---

## Troubleshooting

**Agent Mode not available:**
- Ensure you have the latest Copilot extension
- Check your Copilot subscription includes agent features
- Restart VS Code

**Tests failing:**
- Check test dependencies are installed
- Ensure app is not running (port conflict)
- Verify database is clean between tests

**Coverage not measuring correctly:**
- Install pytest-cov: `pip install pytest-cov`
- Check pytest.ini configuration
- Ensure test files are named test_*.py

---

**Congratulations!** You've completed the Intermediate Lab and mastered professional development workflows with GitHub Copilot.
