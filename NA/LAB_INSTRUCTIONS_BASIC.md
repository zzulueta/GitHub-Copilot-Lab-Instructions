# GitHub Copilot Basic Lab Instructions
**Duration:** 3.5 hours  
**Level:** Beginner (Never used or minimal experience)

---

## Pre-Lab Setup (5 minutes)

### Prerequisites
- This repository cloned locally
- Visual Studio Code installed
- Python 3.8+ installed

### Logging into GitHub Copilot
1. Open VS Code
2. Go to Extensions (Ctrl+Shift+X)
3. Search for "GitHub Copilot" and install it
4. After installation, you should see a prompt to sign in to GitHub Copilot. If not, click on the Copilot icon in the lower right and sign in.
5. Authorize Visual Studio Code to access your GitHub account
6. Once signed in, test the Chat Window:
   - Open Copilot Chat (View → Chat)
   - Type: `Hello Copilot!`
   - You should see a response in the chat window confirming that Copilot is working.

---

## Lab Overview

Welcome to GitHub Copilot! In this lab, you'll learn the fundamentals of using GitHub Copilot to understand, modify, and enhance a Steel Inventory Management API. This is a real-world application used by steel manufacturers to track inventory, calculate dimensions, and manage products.

### What You'll Learn
- [ ] Understanding Copilot's interface (inline suggestions vs. chat)
- [ ] Selecting the right AI model for your task
- [ ] Providing context with #file, #codebase, and drag-drop
- [ ] Using Copilot to set up and run applications
- [ ] Getting onboarded to an existing project
- [ ] Generating code with inline completions
- [ ] Asking Copilot to explain code
- [ ] Fixing simple bugs
- [ ] Adding new features
- [ ] Writing documentation

---

## Part 1: Getting Started with Copilot (30 minutes)

### Exercise 1.1: Understanding the Interface (10 min)

**Task:** Learn the different ways to interact with Copilot

#### Inline Suggestions vs. Chat

1. **Inline Suggestions:**
   - Open `app/models.py`
   - Go to the end of the file (after the SteelProductUpdate class)
   - Type a new comment: `# Function to validate product code format`
   - Press Enter and wait - Copilot will suggest code inline
   - Press `Tab` to accept the suggestion, or `Esc` to dismiss it
   - Try pressing `Alt+]` to see alternative suggestions
   - Review the generated code and understand how it matches your comment


2. **Copilot Chat:**
   - Open Copilot Chat panel (View → Chat or click the chat icon)
   - Use Ask mode for questions, explanations, and code modifications
   
   **Example questions in Ask mode:**
   ```
   What is FastAPI?
   ```
   ```
   What is this file about?
   ```
   
3. **When to use which:**
   - **Inline:** Writing new code, completing functions
   - **Ask mode:** Questions, explanations, code analysis, refactoring, debugging

4. Close the models.py file and do not save changes to it for now.

**Expected Outcome:**
- Understand the difference between inline and Ask mode
- Know when to use each approach
- Comfortable with the Copilot interface

---

### Exercise 1.2: Model Selection (10 min)

**Task:** Learn which AI model to use for different tasks

GitHub Copilot offers different AI models with different strengths:
- **Claude Sonnet 4.5** (Default): Best balance for most tasks
- **GPT-4.1**: Good for general coding tasks
- **Claude Opus 4.7 / GPT-5.5**: Advanced reasoning for complex problems

1. **Find the model selector:**
   - Look at the bottom of the Copilot Chat panel
   - Click on the current model name to see available options

2. **Test different models:**

   Start a new chat. Ask the same question in Ask mode:
   ```
   Explain how the database.py file manages data persistence
   ```
   
   - First try with **Claude Sonnet 4.5**
   - Observe the response - note the style, depth, and accuracy
   - Start a new chat, then switch to **GPT-4.1** and ask again
   - Compare the responses - notice differences in style and depth

3. **When to use each model:**
   - **Claude Sonnet 4.5**: Default choice for most coding tasks
   - **GPT-4.1**: Alternative when you want different perspective
   - **Claude Opus 4.7 / GPT-5.5**: Advanced reasoning for complex problems

4. **Model selection best practice:**
   Start with Claude Sonnet 4.5, only switch if you need specific capabilities.

**Expected Outcome:**
- Know where to find model selector
- Understand different model strengths
- Know when to switch models

---

### Exercise 1.3: Providing Context (10 min)

**Task:** Learn how to give Copilot the right context for better answers

Copilot gives better answers when you provide relevant context. There are several ways to do this:

#### Method 1: Using #file

1. Open Copilot Chat and type `#file` - you'll see a list of files
2. Select specific files to include:
   ```
   #file:models.py Explain the SteelProduct class
   ```
   > Note: You would need to manually type the #file syntax in the chat to manually include files.

3. Try with multiple files:
   ```
   #file:models.py #file:database.py How are products stored and retrieved?
   ```

#### Method 2: Using #codebase

1. For questions about the entire project:
   ```
   #codebase What endpoints are available in this API?
   ```

2. Search within codebase:
   ```
   #codebase Where is the database connection configured?
   ```

#### Method 3: Drag and Drop

1. From the file explorer, drag `app/routers/inventory.py` into Copilot Chat
2. Ask: 
   ```
   What does this file do?
   ```

3. Drag the entire `routers` folder:
   ```
   What routes are defined in this folder?
   ```

#### Method 4: Selecting Code

1. Open `app/utils/steel_utils.py`
2. Select the entire `calculate_weight_kg` function
3. Right-click → Explain
4. Note how it provides a detailed explanation of the selected code in the chat window.
5. You can also use #selection in chat:
   ```
   #selection Explain this function step by step
   ```

#### Best Practices:

- **#file:** When asking about specific files (up to 5-10 files)
- **#codebase:** When searching across entire project
- **Drag-drop:** Quick way to add context visually
- **#selection:** For focused questions about specific code

**Expected Outcome:**
- Comfortable using #file, #codebase, #selection
- Know how to drag-drop for context
- Understand when to use each method

---

## Part 2: Project Onboarding (70 minutes)

### Exercise 2.1: Setting Up the Application with Copilot (15 min)

**Task:** Use Copilot in Ask mode to learn how to set up and run the application

Instead of following manual setup instructions, ask Copilot to guide you through the setup process. This teaches you to use Copilot as your assistant for any new project.

1. **Ask about setup requirements:**
   
   In **Ask mode**:
   ```
   #codebase What do I need to install to run this steel inventory application? 
   What are the setup steps?
   ```
   
   **Expected:** Copilot will tell you about Python requirements, dependencies, and setup steps.

2. **Ask about creating the environment:**
   ```
   #codebase How do I create a Python virtual environment for this project?
   Show me the exact commands for Windows.
   ```
   
   **Follow the steps Copilot provides:**
   - Navigate to steel-inventory-api folder via terminal
   - Create virtual environment
   - Activate it

3. **Ask about installing dependencies:**
   ```
   #codebase What dependencies does this project need? 
   How do I install them?
   ```
   
   **Execute the command Copilot suggests** (likely `pip install -r requirements.txt`)

4. **Ask how to run the application:**
   ```
   #codebase How do I start this FastAPI application? 
   What command should I use?
   ```
   
   **Run the command** (likely `uvicorn app.main:app --reload`)

5. **Verify it's running:**
   ```
   The server started. How can I test if the API is working? 
   Where can I see the API documentation?
   ```
   
   **Visit the URL Copilot provides** (likely http://localhost:8000/docs)

6. **Learn to stop the server:**
   ```
   How do I stop the development server when I'm done?
   ```

**Expected Outcome:**
- Application successfully running
- Understanding that Copilot can guide setup for any project
- Confidence to ask Copilot about unfamiliar setup processes

**Key Takeaway:** Instead of searching documentation or README files, you can ask Copilot to guide you through setup steps for any project.

---

### Exercise 2.2: Learning to Use the Application (10 min)

**Task:** Use Ask mode to learn how to interact with the API

1. **Ask about API capabilities:**
   
   With the app running, in **Ask mode**:
   ```
   #codebase What can I do with this API? 
   What operations are available?
   ```

2. **Ask for usage examples:**
   ```
   Show me examples of how to:
   1. Create a new steel product
   2. Get all products
   3. Update a product
   4. Delete a product
   
   Provide actual API calls I can test.
   ```

3. **Test in the Swagger UI:**
   - Open http://localhost:8000/docs
   - Try the operations Copilot described
   - Create a sample product
   - Retrieve it
   - Update it

4. **Ask about specific features:**
   ```
   #codebase What calculations can this API perform? 
   How do I use the weight calculation features?
   ```

5. **Try a calculation:**
   - Use the /calculations/ endpoints in Swagger UI
   - Test calculating weight for a steel sheet

**Expected Outcome:**
- Understand what the API can do
- Successfully tested API operations
- Comfortable using Swagger UI for testing
- Know how to ask Copilot for usage examples

---

### Exercise 2.3: Understanding the Project (20 min)

**Task:** Use Copilot to get onboarded to the steel-inventory-api codebase

Now that the app is running, dive deeper into understanding how it works.

1. **High-level overview** - In Ask mode:
   ```
   #codebase Analyze this steel inventory management system and provide:
   1. What is the purpose of this application?
   2. What are the main components?
   3. What technologies does it use?
   4. What can users do with this API?
   ```

2. **Understand the data model** - In Ask mode:
   ```
   #file:models.py Explain the SteelProduct model. What information does it track?
   ```

3. **Explore the API endpoints** - Drag `routers` folder into chat:
   ```
   What API endpoints are available? List them with their HTTP methods and purposes.
   ```

4. **Understand calculations** - In Ask mode:
   ```
   #file:steel_utils.py What calculations can this system perform? Provide examples.
   ```

5. **Database operations** - In Ask mode:
   ```
   #file:database.py How is data stored? Is it persistent or in-memory?
   ```

6. **Create a mental map:**
   Ask Copilot to help visualize:
   ```
   #codebase Create a simple diagram showing how the main components interact
   ```

**Expected Outcome:**
- Understanding of what the application does
- Knowledge of main components and their purposes
- Familiarity with available API endpoints
- Confidence to start making changes

---

### Exercise 2.4: Code Exploration with Copilot (15 min)

**Task:** Dive deeper into specific code sections

1. **Trace a request flow:**
   ```
   #codebase When a user makes a GET request to /inventory/, trace the flow:
   1. Which file handles the request?
   2. What function processes it?
   3. How is data retrieved?
   4. What is returned?
   ```

2. **Find specific functionality:**
   ```
   #codebase Where is the weight calculation logic? Show me the exact file and function.
   ```

3. **Understand a complex function:**
   - Open `app/database.py`
   - Find the `update_product` function
   - Select it and ask: 
   ```
   Explain what this function does step by step
   ```

4. **Find dependencies:**
   ```
   #file:main.py What external libraries does this application depend on? What is each used for?
   ```

5. **Discover edge cases:**
   ```
   #file:inventory.py What happens if someone tries to create a product with missing data?
   ```

**Expected Outcome:**
- Ability to navigate codebase with Copilot's help
- Understanding of request flow
- Knowledge of where specific functionality lives

---

### Exercise 2.5: Ask Clarifying Questions (10 min)

**Task:** Practice asking good questions to understand the code

Good questions lead to better answers. Practice different types of questions:

1. **Why questions:**
   ```
   #file:database.py Why is the database using a dictionary instead of a real database?
   ```

2. **How questions:**
   ```
   #file:steel_utils.py How is the steel density determined for different grades?
   ```

3. **What-if questions:**
   ```
   #file:inventory.py What happens if I try to update a product that doesn't exist?
   ```

4. **Comparison questions:**
   ```
   #codebase What's the difference between the /inventory/ endpoint and /calculations/ endpoint?
   ```

5. **Best practice questions:**
   ```
   #file:models.py Is the current data validation approach following FastAPI best practices?
   ```

**Expected Outcome:**
- Comfortable asking different types of questions
- Getting detailed, useful answers
- Understanding not just "what" but "why" and "how"

---

## Part 3: Making Your First Changes (50 minutes)

### Exercise 3.1: Generate Code with Inline Completions (15 min)

**Task:** Use inline suggestions to write new functions

1. **Add a helper function:**
   - Close all files and open `app/utils/steel_utils.py`
   - At the end of the file, add a comment:
   ```python
   # Function to convert dimensions from inches to millimeters
   ```
   - Press Enter and let Copilot suggest the function
   - Review the suggestion, accept with Tab
   
2. **Add data validation:**
   - Open `app/models.py`
   - After the SteelProduct class, type:
   ```python
   # Validator function to check if product code follows format: STL-XXX where X is digit
   ```
   - Accept Copilot's suggestion
   - Observe how it understands the pattern from your comment

3. **Complete a partially written function:**
   - In `steel_utils.py`, start typing:
   ```python
   def calculate_total_weight(products: list):
       """Calculate total weight of multiple steel products"""
       total = 0
       # Loop through products and sum their weights
   ```
   - Let Copilot complete the function

4. **Tips for better inline suggestions:**
   - Write clear, descriptive comments first
   - Use meaningful variable names
   - Break complex tasks into smaller functions
   - Press `Alt+]` to cycle through alternatives

5. Save changes and close the files

**Expected Outcome:**
- Comfortable using inline completions
- Code generated matches your intent
- Functions are properly implemented

---

### Exercise 3.2: Fix Simple Bugs (20 min)

**Task:** Use Copilot to identify and fix bugs in the code

1. **Find bugs with Copilot:**
   - Start a new chat session in Ask mode
   - In Ask mode, drag `app` folder into chat:
   ```
   Review this application for potential bugs. Focus on:
   - Missing error handling
   - Input validation issues
   - Logic errors
   ```

2. **Fix Bug #1: No duplicate checking**
   - Copilot should identify that duplicate product codes aren't prevented
   - In Ask mode:
   ```
   #file:database.py The create_product function doesn't check for duplicate product codes.
   Show me how to fix this by adding validation that raises an HTTPException if code already exists.
   ```
   - Review the suggestion and apply it to `database.py`

3. **Fix Bug #2: Negative quantities allowed**
   - In Ask mode:
   ```
   #file:models.py The quantity field should not allow negative values.
   Add validation to ensure quantity is always >= 0.
   ```
   - Apply the field validator Copilot suggests

4. **Fix Bug #3: Missing update timestamp**
   - Ask Copilot:
   ```
   #file:database.py When updating a product, the last_updated field isn't being set.
   Show me how to fix this.
   ```
   - Implement the fix

5. **Verify fixes:**
   - Ensure all files are saved and close them
   - Start the server: `uvicorn app.main:app --reload`
   - Open http://localhost:8000/docs
   - Test creating a product with negative quantity (should fail)
   - Test creating duplicate products (should fail)

**Expected Outcome:**
- Found and fixed 3 bugs
- Application now has better validation
- Understand how to use Copilot for debugging

---

### Exercise 3.3: Add a New Feature (15 min)

**Task:** Add a new endpoint to search products by location

1. **Plan with Copilot:**
   ```
   #file:inventory.py I want to add an endpoint to search products by warehouse location.
   What should I consider? Give me a step-by-step plan.
   ```

2. **Implement the endpoint:**
   - Open `app/routers/inventory.py`
   - At the end of the file, type:
   ```python
   # GET endpoint to search products by location
   # Example: /inventory/location/Warehouse-A
   ```
   - Let Copilot suggest the implementation
   - Review and accept

3. **Add the database method:**
   - Open `app/database.py`
   - Add:
   ```python
   # Method to get products by location
   ```
   - Let Copilot complete it

4. **Test your feature:**
   - Ensure all files are saved and close them
   - Ensure server is running
   - Go to http://localhost:8000/docs
   - Find your new `/inventory/location/{location}` endpoint
   - Try it with "Warehouse-A"

5. **Refine if needed:**
   If it doesn't work, ask Copilot:
   ```
   The location search endpoint returns an error. Here's the error: [paste error]
   How do I fix this?
   ```

**Expected Outcome:**
- New endpoint successfully added
- Endpoint returns products filtered by location
- Comfortable adding new features with Copilot

---

## Part 4: Documentation and Best Practices (40 minutes)

### Exercise 4.1: Generate Documentation (15 min)

**Task:** Use Copilot to create comprehensive documentation

1. **Add function docstrings:**
   - Open `app/utils/steel_utils.py`
   - Select the entire `calculate_weight_kg` function
   - In Ask mode, ask: 
   ```
   Add a comprehensive docstring with parameters, return value, and example
   ```
   - Review and apply the changes to the file

2. **Generate API documentation:**
   - Open Copilot Chat:
   ```
   #file:inventory.py Create markdown documentation for all endpoints in this file.
   Include: HTTP method, path, parameters, response format, and example.
   ```
   - Copy the generated documentation and save it as `INVENTORY_API_DOCS.md` in the project root

3. **Create a user guide:**
   ```
   #codebase Generate a user guide for this API in markdown format.
   Include:
   - Overview
   - Getting started
   - Available endpoints with examples
   - Common use cases
   ```
   - Copy the generated user guide and save it as `API_USER_GUIDE.md` in the project root

4. **Document the data model:**
   ```
   #file:models.py Create documentation explaining the SteelProduct model fields.
   Format as a table with: Field Name, Type, Description, Example Value.
   ```
   - Copy the generated documentation and save it as `DATA_MODEL_DOCS.md` in the project root

**Expected Outcome:**
- Functions have clear docstrings
- API documentation is comprehensive
- Project has user-friendly guide

---

### Exercise 4.2: Code Quality Improvements (15 min)

**Task:** Use Copilot to improve code quality

1. **Add type hints:**
   - Open `app/database.py`
   - Select a function with incomplete type hints (e.g., `create` function)
   - In Ask mode, ask: `Add complete type hints to this function`
   - Review and apply the suggested changes

2. **Improve error messages:**
   ```
   #file:inventory.py The error messages in this file are generic.
   Suggest more descriptive error messages for each HTTPException.
   ```
   - Review suggestions and apply them

3. **Add input validation:**
   ```
   #file:inventory.py What input validation is missing from the POST /inventory/ endpoint?
   Show me how to add validation for:
   - Product code format (must be STL-XXX)
   - Positive quantity
   - Valid shape types
   ```
   - Implement the suggested validations

4. **Check for best practices:**
   ```
   #codebase Review this codebase for Python and FastAPI best practices.
   What improvements would you recommend?
   ```
   - Note down the recommendations

5. Save changes and close the files

**Expected Outcome:**
- Code has proper type hints
- Better error messages
- More robust validation
- Understanding of best practices

---

### Exercise 4.3: Testing Basics (10 min)

**Task:** Write simple tests with Copilot's help

1. **Understand existing tests:**
   ```
   #file:test_inventory.py Explain what these tests do and what's missing.
   ```

2. **Add a simple test:**
   - Open `tests/test_inventory.py`
   - At the end of the file, type:
   ```python
   def test_create_product_success():
       """Test creating a product with valid data"""
   ```
   - Let Copilot suggest the test implementation
   - Review and accept

3. **Add another test:**
   ```python
   def test_create_product_duplicate_code():
       """Test that duplicate product codes are rejected"""
   ```
   - Let Copilot complete it

4. **Run the tests:**
   ```bash
   pytest tests/test_inventory.py -v
   ```

5. **Fix any failing tests:**
   If tests fail, ask Copilot:
   ```
   This test is failing: [paste error]
   How do I fix it?
   ```

**Expected Outcome:**
- Added 2 new tests
- Tests are passing
- Understand how to use Copilot for testing

---

## Part 5: Practical Scenarios (15 minutes)

### Exercise 5.1: Real-World Task (15 min)

**Task:** Complete a realistic enhancement request

**Scenario:** BlueScope wants to add a "material cost estimation" feature.

**Requirements:**
- Add a `unit_price` field to products (price per kg)
- Create an endpoint to calculate total cost for a product (weight × unit_price × quantity)
- Add validation to ensure unit_price is positive

**Your Task:** Use everything you've learned to implement this feature.

1. **Break down the task:**
   Ask Copilot in Ask mode:
   ```
   I need to add material cost estimation to the steel inventory API.
   Requirements:
   - Add unit_price field to SteelProduct
   - Create endpoint to calculate total cost (weight × price × quantity)
   - Validate unit_price > 0
   
   Give me a step-by-step implementation plan.
   ```

2. **Implement step by step:**
   - Follow Copilot's plan
   - Use inline completions for code
   - Use Ask mode for questions and code modifications

3. **Test your implementation:**
   - Start the server
   - Test in the Swagger UI (http://localhost:8000/docs)
   - Create a product with a unit_price
   - Call the cost calculation endpoint

4. **Document your feature:**
   Ask Copilot to generate documentation for the new endpoint.

**Expected Outcome:**
- Feature fully implemented
- Working correctly
- Properly documented
- Confidence in using Copilot for real tasks

---

## Lab Completion Checklist

### Skills Learned
- [ ] Understand Copilot interface (inline vs. Ask mode)
- [ ] Select appropriate AI models
- [ ] Provide context using #file, #codebase, and drag-drop
- [ ] Use Copilot to set up and run applications
- [ ] Use Copilot for project onboarding
- [ ] Generate code with inline completions
- [ ] Explain existing code
- [ ] Find and fix bugs
- [ ] Add new features
- [ ] Write documentation
- [ ] Improve code quality
- [ ] Write tests

### Deliverables
1. ✅ Fixed 3 bugs (duplicates, negative quantities, update timestamp)
2. ✅ Added location search endpoint
3. ✅ Added cost estimation feature
4. ✅ Generated API documentation
5. ✅ Wrote 2 new tests
6. ✅ Improved code quality (type hints, validation)

---

## Appendix: Quick Reference

### Keyboard Shortcuts
- `Tab` - Accept inline suggestion
- `Esc` - Dismiss suggestion
- `Alt+]` - Next suggestion
- `Alt+[` - Previous suggestion
- `Ctrl+I` - Open inline chat
- `Ctrl+Shift+I` - Open Copilot Chat panel

### Context Syntax
- `#file:path/to/file.py` - Include specific file
- `#codebase` - Search entire project
- Drag and drop - Add files/folders visually
- Select code - Right-click → Copilot → Explain

### Best Practices
1. Write clear comments before code
2. Use descriptive variable names
3. Break complex tasks into steps
4. Always review suggestions
5. Provide context for better results
6. Ask Copilot to explain unfamiliar code

---

**Congratulations!** You've completed the GitHub Copilot Basic Lab. You now have the foundational skills to use Copilot effectively in your daily development work.
