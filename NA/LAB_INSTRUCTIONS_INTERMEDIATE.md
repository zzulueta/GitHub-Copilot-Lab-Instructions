# GitHub Copilot Intermediate Lab Instructions
**Duration:** 3 hours  
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
- [ ] Advanced prompting strategies for better code generation
- [ ] Persisting your preferences with Custom Instructions and Prompts
- [ ] Creating reusable prompt templates for common tasks
- [ ] Comprehensive test automation with Copilot
- [ ] Multi-file debugging and root cause analysis
- [ ] Safe refactoring patterns with AI assistance
- [ ] Using Plan mode for complex feature design
- [ ] Leveraging Agent mode for autonomous implementation

### Learning Objectives

By the end of this lab, you will:
1. Write prompts that consistently produce high-quality code
2. Create Custom Instructions to persist coding preferences across all work
3. Build reusable Custom Prompts for repetitive tasks
4. Generate comprehensive test suites efficiently
5. Debug complex issues spanning multiple files
6. Refactor code safely with test verification
7. Plan complex features before implementation
8. Use Agent mode to autonomously complete multi-step tasks

---

## Part 1: Effective Prompting Strategies (15 minutes)

### Introduction: The Art of Prompting

The quality of Copilot's output depends heavily on how you ask. Intermediate users need to master **prompt engineering** to get consistent, high-quality results. This module teaches you strategies that separate basic Copilot usage from expert-level productivity.

**Key Principle:** Specific, constrained prompts with clear context produce better code than vague requests.

---

### Exercise 1.1: Vague vs. Specific Prompts (5 min)

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

### Exercise 1.2: Iterative Refinement (5 min)

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

### Exercise 1.3: Using Examples in Prompts (5 min)

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

## Part 1.5: Persisting Your Prompting Strategy (30 minutes)

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

### Section A: Custom Instructions (15 minutes)

Custom Instructions are markdown files that provide **persistent context** to Copilot. They apply automatically without you mentioning them in every prompt.

**Instruction file types:**
- `.github/copilot-instructions.md` - Workspace-level (entire project)
- `<directory>/.instructions.md` - Directory-level (specific folders)
- `<file>.instructions.md` - File-level (specific files)

---

### Exercise 1.5.1: Create Workspace-Level Instructions (6 min)

**Task:** Define project-wide coding standards for the steel inventory API

1. **Create the instructions file:**
   
   Create a new file: `.github/copilot-instructions.md` in your workspace root

2. **Add comprehensive project instructions:**
   
   Copy this content:
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
- Understanding that instructions apply to ALL Copilot interactions
- See automatic application of coding standards

---

### Exercise 1.5.2: Directory-Specific Instructions (6 min)

**Task:** Create specialized instructions for different parts of the codebase

1. **Create router-specific instructions:**
   
   Create file: `steel-inventory-api/app/routers/.instructions.md`
   
   ```markdown
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

2. **Create test-specific instructions:**
   
   Create file: `steel-inventory-api/tests/.instructions.md`
   
   ```markdown
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

3. **Test directory-specific instructions:**
   
   Open a file in `steel-inventory-api/tests/` and ask Copilot:
   ```
   Create a test for the delete product endpoint
   ```
   
   Then open a file in `steel-inventory-api/app/routers/` and ask:
   ```
   Create a new endpoint to get products by location
   ```
   
   - Notice how Copilot applies different patterns based on which directory you're in!

**Expected Outcome:**
- Two `.instructions.md` files created in specific directories
- Understanding of instruction scope and hierarchy
- See context-aware suggestions based on directory

---

### Exercise 1.5.3: Instructions in Practice (3 min)

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

### Section B: Custom Prompts (15 minutes)

Custom Prompts are **reusable templates** for common tasks. Unlike instructions (passive), prompts are **actively invoked** when you need them.

**Use Custom Prompts for:**
- Repetitive tasks with specific requirements
- Multi-step workflows you execute frequently
- Team-standard procedures
- Complex prompts you don't want to retype

**Accessing Custom Prompts:**
- Type `@` in Copilot Chat and select from prompt library
- Use Quick Pick (Ctrl+Shift+P → "Copilot: Use Custom Prompt")
- Prompts can include parameters and context

---

### Exercise 1.5.4: Create Reusable Prompt Templates (7 min)

**Task:** Build a library of common development tasks

1. **Create the prompts file:**
   
   Create file: `.prompts.md` in your workspace root

2. **Add general-purpose prompts:**
   
   ```markdown
   # Custom Prompts for Steel Inventory API

   ## Generate Comprehensive Tests
   ```prompt
   title: Generate Comprehensive Tests
   context: selection
   ---
   Analyze the selected function/endpoint and generate comprehensive pytest tests.

   Requirements:
   - Test happy path with valid inputs
   - Test edge cases (boundary values, empty inputs, None values)
   - Test error conditions (invalid inputs, exceptions)
   - Use parametrize for multiple similar test cases
   - Include descriptive test names and docstrings
   - Verify both functionality AND error messages
   - Add fixtures if needed for test data setup

   Return the complete test code ready to add to the test file.
   ```

   ## Add Error Handling
   ```prompt
   title: Add Comprehensive Error Handling
   context: selection
   ---
   Enhance the selected code with comprehensive error handling.

   Requirements:
   - Identify all possible error conditions
   - Add try-except blocks where appropriate
   - Raise HTTPException with descriptive messages
   - Include relevant context in error details
   - Use appropriate HTTP status codes
   - Add logging for errors
   - Handle edge cases (None, empty, invalid types)
   - Maintain existing functionality

   Return the enhanced code with all error handling added.
   ```

   ## Document API Endpoint
   ```prompt
   title: Document API Endpoint
   context: selection
   ---
   Add comprehensive OpenAPI documentation to the selected endpoint.

   Requirements:
   - Add summary (one line description)
   - Add detailed description explaining purpose and behavior
   - Document all parameters (path, query, body)
   - Document all possible response status codes
   - Include example request/response bodies
   - Add response_model if not present
   - Include notes about validation or business rules
   - Add tags for API organization

   Return the endpoint with complete documentation.
   ```

   ## Optimize Database Query
   ```prompt
   title: Optimize Database Query
   context: selection
   ---
   Analyze and optimize the selected database query for performance.

   Requirements:
   - Identify N+1 query problems
   - Add appropriate eager loading (.options(joinedload()))
   - Add indexes recommendations in comments
   - Reduce number of database round trips
   - Use efficient filtering and pagination
   - Add query result caching if beneficial
   - Maintain existing functionality
   - Add comments explaining optimizations

   Return the optimized query with explanatory comments.
   ```
   ```

3. **Add steel-industry specific prompts:**
   
   Append to `.prompts.md`:
   ```markdown
   ## Add Steel Grade Validation
   ```prompt
   title: Add Steel Grade Validation
   context: selection
   ---
   Add comprehensive steel grade validation to the selected code.

   Valid steel grades:
   - Carbon Steel: A36, A572, A992
   - Stainless Steel: 304, 304L, 316, 316L, 321, 410
   - Alloy Steel: 4140, 4340, 8620

   Requirements:
   - Accept grades case-insensitive
   - Normalize to uppercase
   - Validate against allowed list
   - Return tuple: (is_valid, normalized_grade, category)
   - Raise ValueError with helpful message for invalid grades
   - Include all valid grades in error message

   Return the code with validation added.
   ```

   ## Create Inventory Report Endpoint
   ```prompt
   title: Create Inventory Report Endpoint
   context: file
   ---
   Create a new GET endpoint for inventory reporting.

   Endpoint: GET /inventory/report
   Query Parameters:
   - group_by: Optional[str] = "grade" (grade, shape, location)
   - include_totals: bool = True
   - min_quantity: Optional[int] = None

   Response should include:
   - Grouped inventory data
   - Total quantity per group
   - Total weight per group
   - Number of unique products per group
   - Overall totals if include_totals=True

   Requirements:
   - Follow all router patterns from .instructions.md
   - Use proper response model
   - Add comprehensive documentation
   - Include error handling
   - Optimize database query

   Return the complete endpoint ready to add to the router.
   ```
   ```

**Expected Outcome:**
- `.prompts.md` file created with 6 reusable prompts
- Understanding of prompt syntax (title, context, requirements)
- Recognition of when to create custom prompts

---

### Exercise 1.5.5: Use Custom Prompts (5 min)

**Task:** Invoke and apply your custom prompts

1. **Use the "Generate Comprehensive Tests" prompt:**
   
   - Open `steel-inventory-api/app/utils/steel_utils.py`
   - Select the `validate_grade` function
   - Open Copilot Chat
   - Type `@` and select "Generate Comprehensive Tests" from the prompt library
   - Or use Quick Pick: Ctrl+Shift+P → "Copilot: Use Custom Prompt" → select prompt
   - Review the generated tests - should be comprehensive and follow test instructions

2. **Use the "Add Error Handling" prompt:**
   
   - Open `steel-inventory-api/app/routers/inventory.py`
   - Select a function that needs better error handling
   - Invoke the "Add Comprehensive Error Handling" prompt via `@` mention
   - Observe how it adds validation, error messages, and proper status codes

3. **Use the steel-specific prompt:**
   
   - Open `steel-inventory-api/app/routers/inventory.py`
   - Invoke "Add Steel Grade Validation" prompt
   - See how domain-specific prompts capture your business logic

4. **Observe the synergy:**
   
   Notice how **Custom Prompts + Custom Instructions work together**:
   - Prompt defines WHAT to do (generate tests, add error handling)
   - Instructions define HOW to do it (code style, patterns)
   - Result: Consistent, high-quality output every time

**Expected Outcome:**
- Comfortable invoking custom prompts via @ mentions
- See how prompts + instructions complement each other
- Experience faster execution of common tasks

---

### Exercise 1.5.6: Build Your Prompt Library (3 min)

**Task:** Think strategically about reusable prompts

1. **Identify repetitive tasks in your workflow:**
   
   Discuss with your group or reflect:
   - What tasks do you do repeatedly?
   - What complex prompts do you type often?
   - What team standards should be codified?

2. **Add one custom prompt based on your work:**
   
   Think about the steel inventory project. What task might you repeat?
   Examples:
   - "Add Pagination to Endpoint"
   - "Create Migration Script"
   - "Add Request Logging"
   - "Generate Mock Data"
   
   Add your own prompt to `.prompts.md`

3. **Key Decision Framework:**
   
   **When to use Instructions vs. Prompts:**
   
   Use **Instructions** when:
   - ✅ Should apply to ALL code (coding style, patterns)
   - ✅ Background context that's always relevant
   - ✅ "Always do it this way"
   
   Use **Prompts** when:
   - ✅ Specific task you invoke intentionally
   - ✅ Multi-step workflow
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

**Custom Instructions (.instructions.md)**
- Provide persistent context automatically
- Define coding style, patterns, and standards
- Scope: workspace, directory, or file level
- Passive - always applied in background

**Custom Prompts (.prompts.md)**
- Create reusable templates for common tasks
- Define specific workflows and requirements
- Invoke actively when needed
- Active - used when you call them

**Key Takeaways:**
1. Instructions reduce prompt length while improving consistency
2. Prompts eliminate repetitive typing for common tasks
3. Together they form a powerful productivity system
4. Both features enhance ALL Copilot modes (Ask, Plan, Agent)

**Best Practice:** Start with instructions for your coding style, then add prompts as you identify repetitive tasks.

**Throughout the rest of this lab:**
- Your instructions will automatically guide Copilot's suggestions
- You can invoke your custom prompts whenever relevant
- Notice how both features improve your efficiency in Parts 2-6

---

## Part 2: Test Automation with Copilot (25 minutes)

### Introduction: Testing with AI Assistance

Comprehensive testing is critical for maintainable code, but writing tests can be tedious. Copilot excels at generating test cases, especially for edge cases you might miss. This module teaches you to leverage Copilot for **test-driven development** and **comprehensive test coverage**.

---

### Exercise 2.1: Parametrized Tests for Multiple Scenarios (8 min)

**Task:** Generate comprehensive parametrized tests for weight calculations

The `calculate_weight_kg` function in `steel_utils.py` needs thorough testing across different shapes and edge cases.

1. **Ask Copilot to generate parametrized tests:**
   
   Open Copilot Chat in Ask mode:
   ```
   #file:steel_utils.py #file:test_inventory.py 
   
   I need comprehensive parametrized tests for the calculate_weight_kg function.
   
   Requirements:
   1. Use pytest.mark.parametrize
   2. Test all supported shapes: sheet, plate
   3. Include edge cases: minimum dimensions, maximum dimensions, zero thickness
   4. Test the unsupported shapes (coil, bar, tube) to ensure NotImplementedError
   5. Test ValueError for missing width on sheet/plate
   6. Verify weight calculations are accurate (provide expected values)
   7. Name the test function test_calculate_weight_parametrized
   
   Add these tests to test_inventory.py.
   ```

2. **Review and apply the generated tests:**
   - Copy the generated test code
   - Add it to `tests/test_inventory.py`
   - Make sure imports are included (pytest, calculate_weight_kg)

3. **Run the tests:**
   
   In the terminal:
   ```bash
   cd steel-inventory-api
   pytest tests/test_inventory.py::test_calculate_weight_parametrized -v
   ```

4. **Verify coverage:**
   ```
   How many test cases did you generate? Do they cover all edge cases?
   ```

**Expected Outcome:**
- Parametrized test suite with 8-10 test cases
- Tests pass for implemented shapes (sheet, plate)
- Tests correctly verify NotImplementedError for unimplemented shapes
- Understanding of parametrized testing benefits

---

### Exercise 2.2: Comprehensive CRUD Test Suite (8 min)

**Task:** Generate a complete test suite for inventory operations

1. **Generate tests for all CRUD operations:**
   
   In Ask mode:
   ```
   #file:inventory.py #file:test_inventory.py #file:database.py
   
   Generate a comprehensive test suite for all inventory CRUD operations.
   
   Tests needed:
   1. test_create_product_success - Valid product creation
   2. test_create_product_duplicate_code - Should prevent duplicates (currently a bug!)
   3. test_get_product_by_id_success - Retrieve existing product
   4. test_get_product_by_id_not_found - 404 for non-existent ID
   5. test_update_product_success - Update quantity and location
   6. test_update_product_not_found - 404 for non-existent ID
   7. test_delete_product_success - Successful deletion
   8. test_delete_product_not_found - 404 for non-existent ID
   9. test_list_all_products - Returns list of products
   
   Use FastAPI TestClient and include:
   - Proper status code assertions
   - Response body validation
   - Test isolation (each test should be independent)
   
   Add these to test_inventory.py.
   ```

2. **Review the generated tests:**
   - Check that each test is properly isolated
   - Verify status code assertions
   - Ensure response validation is thorough

3. **Add the tests to test_inventory.py:**
   - Place them after the existing tests
   - Ensure all imports are present

4. **Run the CRUD test suite:**
   ```bash
   pytest tests/test_inventory.py -v -k "test_create or test_get or test_update or test_delete or test_list"
   ```

5. **Analyze failures:**
   - Note which tests fail (hint: duplicate checking isn't implemented yet!)
   - This demonstrates how tests reveal bugs

**Expected Outcome:**
- Complete CRUD test suite with 9 tests
- Understanding of test isolation principles
- Identified the duplicate product bug through testing

---

### Exercise 2.3: Testing Error Handling and Validation (5 min)

**Task:** Generate tests for edge cases and error conditions

1. **Ask Copilot to focus on error scenarios:**
   
   In Ask mode:
   ```
   #file:inventory.py #file:models.py
   
   Create tests for validation and error handling:
   
   1. test_create_product_invalid_shape - Shape not in allowed list
   2. test_create_product_negative_quantity - Quantity < 0
   3. test_create_product_zero_thickness - Thickness = 0
   4. test_create_product_missing_width - Sheet without width
   5. test_update_product_negative_quantity - Update with negative quantity
   
   Each test should:
   - Verify appropriate HTTP status code (400 or 422)
   - Check that error message is descriptive
   - Ensure database state is unchanged after error
   
   Add to test_inventory.py.
   ```

2. **Add and run the error handling tests:**
   ```bash
   pytest tests/test_inventory.py -v -k "invalid or negative or zero or missing"
   ```

3. **Note which validations are missing:**
   - Some tests may fail because validation isn't implemented
   - This is good! Tests drive development

**Expected Outcome:**
- 5 error handling tests created
- Understanding of validation testing
- Identified missing validation logic

---

### Exercise 2.4: Identifying Untested Code Paths (4 min)

**Task:** Use Copilot to find gaps in test coverage

1. **Ask about coverage gaps:**
   
   In Ask mode:
   ```
   #file:inventory.py #file:test_inventory.py
   
   Analyze the test coverage for inventory.py. What code paths or scenarios are NOT tested yet?
   
   Consider:
   - Boundary conditions
   - Error paths
   - Different data combinations
   - Edge cases in business logic
   ```

2. **Review Copilot's analysis:**
   - What untested scenarios did it identify?
   - Are there any surprising gaps?

3. **Generate tests for one gap:**
   
   Pick one untested scenario and ask:
   ```
   Generate a test for [specific untested scenario you identified]
   ```

**Expected Outcome:**
- Awareness of test coverage gaps
- Know how to use Copilot for coverage analysis
- Additional tests for previously untested paths

---

### Module 2 Summary

You've learned to:
1. **Generate parametrized tests** for multiple scenarios efficiently
2. **Create comprehensive CRUD test suites** with proper isolation
3. **Test error conditions** and validation logic
4. **Identify coverage gaps** with Copilot's help

**Key Insight:** Tests should be generated alongside code, not as an afterthought. Use Copilot to make testing faster and more thorough.

---

## Part 3: Advanced Debugging Workflows (20 minutes)

### Introduction: Multi-File Debugging

Real bugs rarely exist in isolation. They often span multiple files, involve complex interactions, and require understanding the full context. This module teaches you to use Copilot for **systematic debugging** across your codebase.

---

### Exercise 3.1: Debug the calculate_area_m2 Bug (6 min)

**Task:** Investigate and fix the None width bug with full analysis

The `calculate_area_m2` function in `steel_utils.py` has a bug when width is None.

1. **Start with investigation:**
   
   In Ask mode:
   ```
   #file:steel_utils.py I found a bug in calculate_area_m2. When width_mm is None 
   (which is valid for shapes like 'bar' or 'tube'), the function crashes.
   
   Please:
   1. Explain why this bug occurs
   2. Identify all shapes that could trigger this bug
   3. Show me where this function is called from
   4. Suggest a fix that handles None width appropriately
   5. Should area calculation even make sense for shapes without width?
   ```

2. **Analyze Copilot's response:**
   - Does it identify the root cause?
   - Does it consider the broader context (which shapes have width)?
   - Is the suggested fix appropriate?

3. **Ask for implementation:**
   ```
   Based on your analysis, implement a fix that:
   - Raises a ValueError with a clear message for shapes without width
   - Includes a docstring explaining which shapes are valid
   - Add a note about which shapes have width (sheet, plate, coil) vs. not (bar, tube)
   ```

4. **Apply the fix:**
   - Update the `calculate_area_m2` function in `steel_utils.py`
   - Save the file

5. **Create a regression test:**
   ```
   #file:steel_utils.py Create a test that verifies calculate_area_m2 raises 
   ValueError when width is None, and succeeds when width is provided.
   Add to test_inventory.py.
   ```

**Expected Outcome:**
- Bug fixed with proper validation
- Understanding of root cause
- Regression test in place
- Appreciation for context in debugging

---

### Exercise 3.2: Trace Request Flow for Failed Validation (5 min)

**Task:** Debug why negative quantities aren't being rejected

A user reported they can create products with negative quantities, which shouldn't be allowed.

1. **Start by tracing the request flow:**
   
   In Ask mode:
   ```
   #codebase When a user creates a product with quantity=-10, 
   trace the complete request flow:
   
   1. Which endpoint receives the request?
   2. What model validates the input?
   3. What database method stores it?
   4. Where should validation happen?
   5. Why isn't negative quantity being rejected?
   
   Show me the exact code path and identify the missing validation.
   ```

2. **Review the flow analysis:**
   - Does Copilot identify all the layers?
   - Where is validation missing?

3. **Ask for the fix:**
   ```
   #file:models.py Add validation to the SteelProduct and SteelProductCreate models 
   to ensure quantity is always >= 0. Use Pydantic's Field validator.
   
   Also add validation for SteelProductUpdate to prevent updating to negative quantity.
   ```

4. **Apply the fix and test:**
   - Update `models.py`
   - Start the server: `uvicorn app.main:app --reload`
   - Try to create a product with negative quantity in Swagger UI
   - Verify it's rejected with 422 status

**Expected Outcome:**
- Traced bug through multiple files
- Identified exact validation gap
- Implemented model-level validation
- Verified fix in running application

---

### Exercise 3.3: Debug Incomplete Weight Calculations (5 min)

**Task:** Investigate why weight calculation fails for coils, bars, and tubes

1. **Ask Copilot about the issue:**
   
   In Ask mode:
   ```
   #file:steel_utils.py #file:calculations.py 
   
   Users are getting "NotImplementedError" when calculating weight for 
   coils, bars, and tubes. 
   
   Tasks:
   1. Explain what calculations are needed for each shape:
      - Coil: rolled sheet, treat as sheet
      - Bar: solid circular cross-section (use thickness as diameter)
      - Tube: hollow circular cross-section (need inner and outer diameter)
   2. Provide the mathematical formulas
   3. Implement calculate_weight_kg for these three shapes
   4. Consider: how do we handle tube inner diameter (we only have thickness)?
   ```

2. **Review the proposed solution:**
   - Are the formulas correct?
   - Is the tube implementation reasonable given our data model?

3. **Implement the fix:**
   
   If the tube implementation needs more data:
   ```
   For tubes, we don't have inner diameter. What's a reasonable approach?
   Should we:
   a) Assume thickness is wall thickness and need to add an inner_diameter field?
   b) Assume thickness is the full diameter and calculate as solid bar?
   c) Make an assumption about wall thickness percentage?
   
   Recommend the best approach for a production system.
   ```

4. **Apply the implementation:**
   - Update `calculate_weight_kg` in `steel_utils.py`
   - Add comments explaining assumptions

5. **Test it:**
   - Use the `/calculations/weight` endpoint in Swagger UI
   - Test with coil, bar, and tube shapes

**Expected Outcome:**
- Weight calculations implemented for all shapes
- Understanding of geometric calculations
- Awareness of data model limitations
- Documented assumptions in code

---

### Exercise 3.4: Create Bug Report with Reproduction Steps (4 min)

**Task:** Document the duplicate product code bug properly

1. **Ask Copilot to help create a bug report:**
   
   In Ask mode:
   ```
   #file:database.py #file:inventory.py 
   
   Create a detailed bug report for the missing duplicate product code validation.
   
   Include:
   1. Bug title and severity
   2. Steps to reproduce
   3. Expected behavior
   4. Actual behavior
   5. Root cause (which file and function)
   6. Suggested fix location
   7. Impact analysis (what could go wrong?)
   8. Sample curl commands to reproduce
   
   Format as a GitHub issue in Markdown.
   ```

2. **Review the bug report:**
   - Is it detailed enough for another developer?
   - Does it include reproduction steps?
   - Is the suggested fix clear?

3. **Key Learning:**
   - Good bug reports save debugging time
   - Reproduction steps are critical
   - Root cause analysis prevents partial fixes

**Expected Outcome:**
- Professional bug report created
- Understanding of bug documentation best practices
- Clear reproduction steps for testing

---

### Module 3 Summary

You've learned to:
1. **Investigate bugs systematically** with root cause analysis
2. **Trace request flow** across multiple files
3. **Debug incomplete implementations** with proper formulas
4. **Document bugs professionally** with reproduction steps

**Key Insight:** Debugging is about understanding context, not just fixing symptoms. Use Copilot to trace flows and analyze root causes.

---

## Part 4: Code Quality & Refactoring Patterns (30 minutes)

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

## Part 5: Plan Mode for Complex Tasks (25 minutes)

### Introduction: Strategic Planning with Copilot

**Plan mode** is designed for research, design, and strategic thinking. Unlike direct coding or Agent mode, Plan mode focuses on **breaking down complex tasks, researching approaches, and creating actionable roadmaps** before writing code.

**When to use Plan mode:**
- Complex features requiring multiple steps
- Unfamiliar technologies or patterns
- Need to understand trade-offs between approaches
- Want to design before implementing
- Multi-file changes with dependencies

---

### Exercise 5.1: Plan Batch Operations Endpoint (8 min)

**Task:** Design a comprehensive batch operations API

**Scenario:** BlueScope wants to add batch operations to create, update, or delete multiple products in a single API call.

1. **Switch to Plan mode:**
   - Open Copilot Chat
   - Select **Plan mode** from the mode selector

2. **Ask for a comprehensive plan:**
   
   In Plan mode:
   ```
   #codebase I need to add batch operations to the steel inventory API.
   
   Requirements:
   - POST /inventory/batch - Create multiple products at once
   - PATCH /inventory/batch - Update multiple products by ID
   - DELETE /inventory/batch - Delete multiple products by ID
   
   Design considerations:
   - What if some operations succeed and others fail?
   - Should it be transactional (all-or-nothing)?
   - How to return detailed results for each operation?
   - What's the maximum batch size?
   - How to handle duplicate product codes in the batch?
   - Performance implications for large batches?
   
   Create a detailed implementation plan including:
   1. API endpoint design (request/response models)
   2. Error handling strategy
   3. Validation approach
   4. Database changes needed
   5. Testing strategy
   6. Step-by-step implementation order
   7. Potential edge cases
   ```

3. **Review the plan:**
   - Does it address all your requirements?
   - Are the steps actionable?
   - Does it consider edge cases?

4. **Iterate on the plan:**
   
   If something is unclear or you want more detail:
   ```
   I like the plan, but can you elaborate on the error handling strategy?
   
   Specifically:
   - Should partial success be allowed?
   - How should the response format look for mixed success/failure?
   - Should failed operations roll back successful ones?
   ```

5. **Refine until satisfied:**
   - Keep asking questions until the plan is complete
   - Don't implement yet - focus on design

**Expected Outcome:**
- Comprehensive plan for batch operations
- Clear API design with request/response models
- Error handling strategy defined
- Implementation steps identified
- Ready to implement (in next module!)

---

### Exercise 5.2: Design Low-Stock Alert System (7 min)

**Task:** Plan a notification system for low inventory

**Scenario:** The warehouse manager needs automatic alerts when products fall below threshold quantities.

1. **Create a new Plan mode session:**
   
   In Plan mode:
   ```
   #codebase Design a low-stock alert system for the inventory API.
   
   Features needed:
   1. Endpoint to get all products below a threshold
   2. Configurable thresholds per product or per warehouse
   3. Alert severity levels (low, critical)
   4. Historical tracking (when did stock go low?)
   5. Notification mechanism (email, webhook, or both?)
   
   Research and plan:
   - Data model changes needed
   - New endpoints required
   - Background job for checking stock levels?
   - Integration points for notifications
   - Configuration storage
   - Testing approach
   
   Consider:
   - Should thresholds be per-product or global?
   - How often should we check for low stock?
   - What if the same product triggers alerts multiple times?
   - Do we need alert history?
   
   Provide a phased implementation plan (MVP first, then enhancements).
   ```

2. **Review the phased approach:**
   - Does the MVP deliver value?
   - Are enhancements prioritized correctly?
   - Is the plan realistic?

3. **Ask about specific technical decisions:**
   ```
   For the notification mechanism, what are the pros/cons of:
   1. Synchronous webhook on every update
   2. Background job checking periodically
   3. Event-driven architecture with message queue
   
   Which should we choose for this application?
   ```

4. **Document the final plan:**
   - Note the key decisions
   - Save the implementation order
   - Identify dependencies

**Expected Outcome:**
- Complete alert system design
- Phased implementation plan (MVP + enhancements)
- Technical decisions made with rationale
- Clear next steps for implementation

---

### Exercise 5.3: Plan Advanced Filtering with Pagination (5 min)

**Task:** Design a robust search and filter system

**Scenario:** Users need to search inventory with multiple filters and paginated results.

1. **Plan the filtering system:**
   
   In Plan mode:
   ```
   #file:inventory.py #file:models.py
   
   Design an advanced filtering and pagination system for GET /inventory/.
   
   Filter capabilities:
   - By grade (multiple)
   - By shape (multiple)
   - By location (multiple)
   - By quantity range (min/max)
   - By dimension ranges
   - By last updated date range
   - Text search in product_code
   
   Pagination:
   - Limit and offset
   - Or cursor-based?
   - Default page size: 20
   - Maximum page size: 100
   
   Sorting:
   - By any field
   - Ascending or descending
   - Default: last_updated descending
   
   Plan:
   1. Query parameter design
   2. Filter logic in database layer
   3. Response format with pagination metadata
   4. Performance considerations
   5. Validation of filter parameters
   
   Show me the request/response model and implementation steps.
   ```

2. **Review the query parameter design:**
   - Is it intuitive?
   - Does it support all requirements?
   - Is it RESTful?

3. **Ask about implementation challenges:**
   ```
   What are the biggest challenges in implementing this filtering system?
   How do we keep the code maintainable as filters grow?
   ```

**Expected Outcome:**
- Complete filtering and pagination design
- Query parameter structure defined
- Implementation challenges identified
- Ready for implementation

---

### Exercise 5.4: Iterate on a Plan Based on Constraints (5 min)

**Task:** Refine a plan when new constraints emerge

**Scenario:** After planning batch operations, you learn the database will be migrated to PostgreSQL soon.

1. **Revisit the batch operations plan:**
   
   In Plan mode (same chat as Exercise 5.1):
   ```
   New information: We're migrating from in-memory database to PostgreSQL 
   within 2 months.
   
   How should this affect the batch operations implementation?
   
   Consider:
   - Should we design for transactions now?
   - Will the implementation need significant changes?
   - Should we wait for PostgreSQL migration?
   - Or implement with future migration in mind?
   
   Revise the implementation plan to account for this.
   ```

2. **Review the revised plan:**
   - Does it balance current needs with future changes?
   - Is there a migration path?

3. **Key Learning:**
   - Plans should be flexible
   - Consider future constraints early
   - Sometimes it's worth waiting or phasing differently

**Expected Outcome:**
- Revised plan accounting for database migration
- Understanding of how to adjust plans for new constraints
- Balance between current needs and future changes

---

### Module 5 Summary

You've learned to:
1. **Use Plan mode** for complex feature design
2. **Create phased implementation plans** (MVP first)
3. **Design APIs** with clear request/response models
4. **Adapt plans** when constraints change

**Key Insight:** Planning before coding saves time. Use Plan mode to think through complexity before writing a single line of code.

---

## Part 6: Agent Mode Deep-Dive (35 minutes)

### Introduction: Autonomous Development with Agent Mode

**Agent mode** is Copilot's most autonomous mode. It can read files, write code, run commands, and make decisions on its own. Unlike Plan mode (research) or Ask mode (answers), Agent mode **executes** multi-step tasks with minimal supervision.

**When to use Agent mode:**
- Implementing a clear plan from Plan mode
- Multi-file changes with known requirements
- Repetitive but complex tasks
- Refactoring across multiple files
- When you want to review final results rather than guide each step

**Important:** Agent mode works best with clear instructions and constraints. You'll still review and approve changes.

---

### Exercise 6.1: Implement Batch Operations with Agent (12 min)

**Task:** Use Agent mode to implement the batch operations plan from Exercise 5.1

1. **Switch to Agent mode:**
   - Open Copilot Chat
   - Select **Agent mode** from the mode selector

2. **Give Agent the implementation task:**
   
   In Agent mode:
   ```
   #codebase Implement the batch operations endpoints we planned earlier.
   
   Requirements (based on our plan):
   1. Create POST /inventory/batch endpoint for creating multiple products
      - Accept list of SteelProductCreate objects
      - Return list of results (success/failure for each)
      - Continue processing even if some fail
      - Response format: { "successful": [...], "failed": [...] }
   
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
      - Partial failures
      - Validation errors
      - Empty batches
      - Oversized batches
   
   Implementation constraints:
   - Add new models to models.py for batch requests/responses
   - Add new endpoints to inventory.py router
   - Update database.py if needed for batch operations
   - Add all tests to test_inventory.py
   - Follow existing code style and patterns
   
   Please implement all of this, then provide a summary of changes made.
   ```

3. **Monitor Agent progress:**
   - Agent will show what it's doing in real-time
   - Watch as it reads files, writes code, creates tests
   - You can stop it at any time if it goes off track

4. **Review Agent's changes:**
   - Read through all modified files
   - Check if the implementation matches requirements
   - Verify tests are comprehensive

5. **Test the implementation:**
   ```bash
   # Run tests
   pytest tests/test_inventory.py -v -k "batch"
   
   # Start server
   uvicorn app.main:app --reload
   ```

6. **Test in Swagger UI:**
   - Go to http://localhost:8000/docs
   - Try POST /inventory/batch with 3 products (2 valid, 1 duplicate)
   - Verify partial success handling
   - Try PATCH /inventory/batch
   - Try DELETE /inventory/batch

7. **If Agent made mistakes:**
   
   In the same Agent mode chat:
   ```
   The batch create endpoint has an issue: [describe the problem]
   
   Please fix this by [specific correction needed]
   ```

**Expected Outcome:**
- Batch operations fully implemented
- Tests passing
- Endpoints working in Swagger UI
- Understanding of Agent mode workflow

---

### Exercise 6.2: Implement Low-Stock Alerts Autonomously (10 min)

**Task:** Let Agent implement the low-stock alert system MVP

1. **Start a new Agent mode session:**
   
   In Agent mode:
   ```
   #codebase Implement the MVP for the low-stock alert system we planned.
   
   MVP Features:
   1. GET /inventory/low-stock endpoint
      - Query parameter: threshold (default: 50)
      - Returns products with quantity < threshold
      - Sorted by quantity ascending (most critical first)
      - Include percentage below threshold
      - Include calculated "days until stockout" (assume 5 units/day usage)
   
   2. Add a LowStockProduct response model with:
      - All SteelProduct fields
      - current_quantity: int
      - threshold: int
      - percentage_below: float
      - days_until_stockout: float
   
   3. Add method to database.py:
      - get_low_stock(threshold: int) -> List[SteelProduct]
   
   4. Create tests for:
      - Default threshold
      - Custom threshold
      - Empty results when no low stock
      - Correct sorting
      - Correct calculations
   
   Implementation constraints:
   - Add model to models.py
   - Add endpoint to inventory.py
   - Add database method to database.py
   - Add tests to test_inventory.py
   - Use existing code patterns
   
   Implement all of this end-to-end.
   ```

2. **Let Agent work:**
   - Don't interrupt unless it's clearly going wrong
   - Observe how it plans its own steps

3. **Review and test:**
   ```bash
   pytest tests/test_inventory.py -v -k "low_stock"
   ```

4. **Test in Swagger UI:**
   - Try GET /inventory/low-stock
   - Try with different threshold values
   - Verify calculations are correct

**Expected Outcome:**
- Low-stock endpoint working
- Calculations correct
- Tests passing
- Autonomous implementation successful

---

### Exercise 6.3: Add Filtering with Agent (8 min)

**Task:** Implement basic filtering for the inventory list endpoint

1. **Use Agent mode for enhancement:**
   
   In Agent mode:
   ```
   #file:inventory.py #file:database.py
   
   Enhance GET /inventory/ endpoint with filtering capabilities.
   
   Add query parameters:
   - grade: Optional[str] - filter by steel grade
   - shape: Optional[str] - filter by shape
   - location: Optional[str] - filter by warehouse location
   - min_quantity: Optional[int] - minimum quantity
   - max_quantity: Optional[int] - maximum quantity
   
   Implementation:
   1. Update get_all_products endpoint to accept these parameters
   2. Add get_filtered() method to database.py that applies filters
   3. Return filtered results
   4. Handle case where no filters are provided (return all)
   
   Testing:
   - Test each filter individually
   - Test combining multiple filters
   - Test with no filters
   - Test with no matching results
   
   Add to existing files following the current patterns.
   ```

2. **Review Agent's implementation:**
   - Check the filtering logic
   - Verify it handles all cases

3. **Test the filtering:**
   ```bash
   pytest tests/test_inventory.py -v -k "filter"
   ```

4. **Test in Swagger UI:**
   - Filter by grade: ?grade=A36
   - Filter by location: ?location=Warehouse-A
   - Combine filters: ?grade=304&shape=sheet
   - Test with no matches

**Expected Outcome:**
- Filtering working for all parameters
- Can combine multiple filters
- Tests comprehensive
- Agent handled end-to-end implementation

---

### Exercise 6.4: Handle Agent Errors and Course-Correct (5 min)

**Task:** Learn to guide Agent when it makes mistakes

**Scenario:** Intentionally give Agent an ambiguous instruction to see how to correct it.

1. **Give Agent an ambiguous task:**
   
   In Agent mode:
   ```
   #file:inventory.py Add sorting to the inventory list endpoint.
   ```
   
   (Note: This is vague - no sort fields specified, no direction specified)

2. **Observe what Agent does:**
   - What assumptions does it make?
   - What did it choose to sort by?

3. **Provide correction:**
   ```
   Good attempt, but I need more specific sorting:
   
   - Add query parameter: sort_by (field name)
   - Add query parameter: sort_order (asc or desc)
   - Supported sort fields: product_code, quantity, last_updated, grade, location
   - Default: sort by last_updated descending
   - Validate sort_by is a supported field
   - Return 400 if invalid sort_by value
   
   Please update the implementation.
   ```

4. **Verify the correction:**
   - Check if Agent fixed the issues
   - Test in Swagger UI

5. **Key Learning:**
   - Vague instructions lead to assumptions
   - Be specific with Agent mode
   - Easy to course-correct in the same conversation

**Expected Outcome:**
- Understanding that Agent needs clear instructions
- Ability to correct Agent's path
- Sorting properly implemented

---

### Module 6 Summary

You've learned to:
1. **Use Agent mode** for autonomous multi-step implementation
2. **Implement complete features** end-to-end with minimal guidance
3. **Monitor and guide** Agent during execution
4. **Course-correct** when Agent makes wrong assumptions

**Key Insight:** Agent mode is powerful for execution when you know what you want. Use it to implement plans from Plan mode, not to figure out what to build.

---

## Lab Completion Checklist

### Skills Mastered
- [ ] Write specific, constrained prompts for better results
- [ ] Use iterative refinement to improve code generation
- [ ] Apply few-shot prompting with examples
- [ ] Generate comprehensive parametrized test suites
- [ ] Test error conditions and edge cases
- [ ] Debug issues spanning multiple files
- [ ] Trace request flow for root cause analysis
- [ ] Extract validation logic into focused modules
- [ ] Add comprehensive error handling
- [ ] Implement structured logging
- [ ] Refactor safely with test verification
- [ ] Use Plan mode for complex feature design
- [ ] Create phased implementation plans
- [ ] Adapt plans when constraints change
- [ ] Use Agent mode for autonomous implementation
- [ ] Guide and course-correct Agent mode

### Deliverables Completed
1. ✅ Enhanced validation with validators module
2. ✅ Comprehensive test suite (parametrized, CRUD, error handling)
3. ✅ Fixed bugs: area calculation, validation gaps, incomplete implementations
4. ✅ Refactored code with extracted validators and logging
5. ✅ Batch operations endpoints (create, update, delete)
6. ✅ Low-stock alerts endpoint with calculations
7. ✅ Filtering and sorting for inventory list
8. ✅ Professional bug reports and documentation

### Key Metrics
- **Test Coverage:** Significantly increased from basic lab
- **Code Quality:** Improved with validation, logging, error handling
- **Features Added:** 5+ new endpoints and capabilities
- **Bugs Fixed:** 6+ issues resolved
- **Refactoring:** Multiple modules improved

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
   - Share your .instructions.md and .prompts.md files with your team
   - Version control your custom configurations

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
