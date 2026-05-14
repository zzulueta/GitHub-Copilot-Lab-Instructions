# GitHub Copilot Advanced Lab Instructions
**Duration:** 3 hours  
**Level:** Advanced (Power Users)

---

## Pre-Lab Setup (15 minutes before lab)

### 1. Prerequisites
- GitHub Copilot license activated
- VS Code with GitHub Copilot extensions installed
- Python 3.9+ installed
- Git configured with GitHub account
- GitHub repository created for this project (you'll push code here)
- Node.js installed (for MCP server creation)

### 2. Clone and Environment Setup
```bash
cd steel-inventory-api
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Verify Setup
```bash
uvicorn app.main:app --reload
```
Visit http://localhost:8000/docs to confirm the API is running.

### 4. GitHub Repository Setup
If you haven't already, create a GitHub repository and push this code:
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

### 5. Install Additional Tools
```bash
pip install pytest-cov black ruff
```

### 6. Enable Copilot Memory
- Open VS Code Settings (Ctrl+,)
- Search for "Copilot Memory"
- Enable memory for both Chat and Tools
- Verify memory is active

### 7. Verify GitHub MCP Server Access
- Check if GitHub MCP Server is available in your Copilot configuration
- Ensure you have GitHub authentication configured
- You'll set this up in detail during Exercise 3.4 if not already configured

---

## Lab Overview

This advanced lab focuses on power-user features: MCP integration, advanced agent workflows, PR automation, code reviews, performance optimization, and security analysis. You'll build enterprise-ready workflows using GitHub Copilot's most sophisticated capabilities.

### Learning Objectives
- [ ] Integrate Model Context Protocol (MCP) servers (custom + GitHub)
- [ ] Build complex multi-agent workflows with handoffs
- [ ] Create advanced skills with progressive loading and external data
- [ ] Implement advanced memory strategies (user, session, repository scopes)
- [ ] Automate PR creation and code reviews using GitHub MCP Server
- [ ] Perform security and performance analysis
- [ ] Implement advanced debugging workflows
- [ ] Build team collaboration patterns

---

## Part 1: MCP Integration (45 minutes)

### Exercise 1.1: Understanding MCP (10 min)

**What is MCP?**
Model Context Protocol (MCP) allows Copilot to connect to external tools, databases, APIs, and services. This extends Copilot's capabilities beyond code generation.

**Use Cases:**
- Access company databases
- Query internal APIs
- Integrate with external services (Jira, Azure, AWS)
- Connect to documentation systems
- Access file systems and tools

1. **Check available MCP servers:**
   ```
   What MCP servers are currently available in my VS Code?
   ```

2. **Learn about MCP capabilities:**
   ```
   Explain how I can use MCP to extend GitHub Copilot's capabilities.
   What are common MCP server examples?
   ```

**Expected Outcome:**
- Understanding of MCP architecture
- Knowledge of potential integrations
- Ready to build custom MCP server

---

### Exercise 1.2: Create a Custom MCP Server (25 min)

**Task:** Build an MCP server that provides steel industry data

**Scenario:** BlueScope needs real-time steel price data and industry standards accessible to Copilot.

1. **Plan the MCP server:**
   In Agent mode:
   ```
   I want to create a custom MCP server for steel industry data that provides:
   1. Current steel prices by grade
   2. ASTM and EN standard specifications
   3. Density values for different steel grades
   4. Carbon content ranges
   
   Provide a plan for building this MCP server in Node.js/TypeScript.
   ```

2. **Create the MCP server project:**
   ```bash
   cd ..
   mkdir steel-data-mcp-server
   cd steel-data-mcp-server
   npm init -y
   ```

3. **Ask Copilot to scaffold the server:**
   In Agent mode:
   ```
   Create a Model Context Protocol server in TypeScript with these tools:
   
   1. get_steel_price(grade: string) -> returns price per kg
   2. get_standard_spec(standard: string) -> returns specification details
   3. get_density(grade: string) -> returns density in kg/m³
   4. get_carbon_content(grade: string) -> returns carbon percentage range
   
   Include sample data for common grades: A36, 304, 316L, A572-50
   Use the @modelcontextprotocol/sdk package.
   ```

4. **Create the server files:**
   Let Copilot generate:
   - `package.json` with dependencies
   - `tsconfig.json`
   - `src/index.ts` with MCP server implementation
   - `src/data/steel-data.ts` with sample data

5. **Build and test:**
   ```bash
   npm install
   npm run build
   ```

6. **Configure MCP in VS Code:**
   - Open VS Code settings
   - Search for "MCP"
   - Add your steel-data-mcp-server configuration
   
   Ask Copilot:
   ```
   Show me how to configure my custom MCP server in VS Code settings.json
   ```

7. **Test the MCP server:**
   Reload VS Code, then in Copilot Chat:
   ```
   Using the steel data MCP server, what is the current price of A36 steel?
   ```
   
   ```
   What's the density of 304 stainless steel according to the MCP server?
   ```

**Expected Outcome:**
- Custom MCP server running
- Copilot can access steel industry data
- Understanding of MCP server architecture

---

### Exercise 1.3: Advanced MCP Integration (10 min)

**Task:** Use MCP data in code generation

1. **Generate price-aware code:**
   In the steel-inventory-api project:
   ```
   Use the steel-data MCP server to create a function that:
   1. Gets the current price for a product's steel grade
   2. Calculates total material cost (price × weight × quantity)
   3. Adds a 15% markup
   4. Returns the selling price
   
   Add this to utils/steel_utils.py
   ```

2. **Create an endpoint using MCP data:**
   ```
   Create an endpoint POST /inventory/{product_id}/update-price that:
   1. Uses MCP server to get current steel price for the product's grade
   2. Updates the unit_price field
   3. Returns the updated product
   
   Add to routers/inventory.py
   ```

3. **Test the integration:**
   - Start the server
   - Use the Swagger UI to test the new endpoint
   - Verify prices match MCP server data

**Expected Outcome:**
- Code generation leveraging external data
- Working integration between API and MCP server
- Real-world MCP use case implemented

---

## Part 2: Advanced Agent Workflows (45 minutes)

### Exercise 2.1: Complex Multi-Agent System (20 min)

**Task:** Build a sophisticated agent system with complex handoffs

1. **Create a procurement agent** in `.github/agents/procurement-agent.agent.md`:
   ```
   /create-agent Create a procurement-agent.agent.md that:
   - Manages steel procurement and purchasing decisions
   - Analyzes inventory levels and triggers reorder recommendations
   - Uses MCP server for current pricing
   - Has access to read, web, and agent tools
   - Can handoff to: steel-expert (for specifications), inventory-manager (for stock checks)
   - Uses Claude Sonnet 4.5
   ```

2. **Create a quality-control agent** in `.github/agents/quality-control.agent.md`:
   ```
   /create-agent Create a quality-control.agent.md that:
   - Performs quality inspections and compliance checks
   - Validates products against ASTM/EN standards
   - Can handoff to: steel-expert (for standard details), quality-inspector (for specific tests)
   - Uses o1-mini for complex reasoning about quality metrics
   - Has access to read and web tools
   ```

3. **Create a logistics-coordinator agent** in `.github/agents/logistics-coordinator.agent.md`:
   ```
   /create-agent Create a logistics-coordinator.agent.md that:
   - Manages warehouse operations and product movements
   - Optimizes storage locations based on product characteristics
   - Coordinates with inventory-manager for stock levels
   - Plans shipment batches
   - Uses Claude Sonnet 4.5
   - Has access to read and agent tools
   ```

4. **Test complex handoff chains:**
   
   **Scenario 1: Procurement Decision**
   Select procurement-agent:
   ```
   We're running low on A36 plates (current stock: 30, minimum: 75).
   Should we reorder? Calculate the order quantity and estimated cost.
   ```
   Expected: Agent checks inventory → handoff to steel-expert for specs → uses MCP for pricing → provides recommendation

   **Scenario 2: Quality Compliance Check**
   Select quality-control:
   ```
   We received a batch of 316L stainless steel coils with these specs:
   - Carbon content: 0.025%
   - Chromium: 17%
   - Nickel: 12%
   
   Do they meet ASTM A240 standards?
   ```
   Expected: Agent handoff to steel-expert for standard specs → performs analysis → provides compliance report

   **Scenario 3: Warehouse Optimization**
   Select logistics-coordinator:
   ```
   We have 150 new steel products arriving:
   - 50 hot-rolled coils (A36)
   - 75 cold-rolled sheets (304)
   - 25 galvanized plates (A653)
   
   Suggest optimal warehouse locations and flag any capacity issues.
   ```
   Expected: Agent handoff to inventory-manager → checks current storage → provides placement plan

**Expected Outcome:**
- Complex multi-agent system operational
- Agents successfully handoff between each other
- Real-world business scenarios handled

---

### Exercise 2.2: Agent Collaboration Patterns (15 min)

**Task:** Implement advanced agent collaboration patterns

1. **Sequential Handoff Pattern:**
   Create a workflow where agents work in sequence.
   
   Select procurement-agent:
   ```
   Process this procurement request:
   1. Check if we need to reorder bars (current: 180, min: 200)
   2. Get specifications and pricing
   3. Verify quality requirements
   4. Plan warehouse placement
   
   Coordinate with other agents as needed.
   ```
   
   Observe: procurement → inventory-manager → steel-expert → quality-control → logistics-coordinator

2. **Parallel Consultation Pattern:**
   Multiple agents provide input simultaneously.
   
   In Agent mode:
   ```
   We're considering switching from A36 to A572-50 for structural applications.
   I need input from:
   - steel-expert (material properties)
   - procurement-agent (cost implications)
   - quality-control (standards compliance)
   
   Provide a comprehensive analysis.
   ```

3. **Supervisory Pattern:**
   Create a master agent that coordinates others.
   
   Create `.github/agents/operations-director.agent.md`:
   ```
   /create-agent Create operations-director.agent.md that:
   - Acts as master coordinator for all operations
   - Can handoff to any other agent
   - Makes final decisions on complex scenarios
   - Has access to all tools: read, web, agent
   - Uses o1-preview for complex decision-making
   ```

   Test the supervisory pattern:
   ```
   We have a critical situation:
   - Major order (500 tons) due in 2 weeks
   - Current stock insufficient
   - Quality issues with last supplier batch
   - Warehouse at 85% capacity
   
   Develop a comprehensive action plan coordinating all departments.
   ```

**Expected Outcome:**
- Understanding of agent collaboration patterns
- Complex workflows orchestrated across multiple agents
- Enterprise-ready agent system

---

### Exercise 2.3: Agent Performance Optimization (10 min)

**Task:** Optimize agent responses and reduce costs

1. **Model selection strategy:**
   Review your agents and optimize model selection:
   ```
   Review all my custom agents in .github/agents/.
   For each agent, evaluate if the current model is optimal or if I should switch to:
   - Claude Sonnet 4.5 (general tasks)
   - GPT-4o (alternative perspective)
   - o1-mini (reasoning tasks)
   - o1-preview (complex reasoning only)
   
   Recommend model changes to reduce costs while maintaining quality.
   ```

2. **Scope optimization:**
   Refine agent descriptions to prevent unnecessary invocations:
   ```
   Review my agent descriptions and suggest improvements to:
   1. Make invocation conditions more specific
   2. Reduce overlap between agents
   3. Clarify when to use each agent
   ```

3. **Handoff optimization:**
   ```
   Analyze the handoff configurations in my agents.
   Identify any circular dependencies or inefficient handoff chains.
   Suggest optimizations.
   ```

**Expected Outcome:**
- Optimized model selection
- Reduced unnecessary agent invocations
- More efficient handoff chains

---

## Part 3: GitHub PR Workflow Automation with MCP (60 minutes)

### Exercise 3.1: Automated PR Creation (15 min)

**Task:** Create pull requests with Copilot assistance

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/performance-optimization
   ```

2. **Implement performance improvements:**
   Ask Copilot in Agent mode:
   ```
   Analyze the steel-inventory-api for performance bottlenecks.
   Suggest and implement optimizations for:
   1. Database queries
   2. Calculation efficiency
   3. Response caching
   4. Batch operations
   ```

3. **Generate semantic commit messages:**
   Configure commit message guidelines (if not already done):
   ```
   Show me how to configure custom commit message generation instructions
   for my team's standards:
   - Use conventional commits (feat:, fix:, perf:, refactor:)
   - Include ticket references: [STEEL-XXX]
   - Limit first line to 50 characters
   - Add detailed explanation in body
   ```

4. **Commit with generated messages:**
   - Go to Source Control
   - Use Copilot to generate commit message
   - Review and commit
   - Push: `git push -u origin feature/performance-optimization`

5. **Create PR with Copilot:**
   In Agent mode:
   ```
   Create a pull request for my feature/performance-optimization branch.
   Generate:
   1. PR title following our conventions
   2. Comprehensive description including:
      - What changed
      - Why these changes were made
      - Performance metrics/improvements
      - Testing completed
      - Breaking changes (if any)
   3. Suggested reviewers based on files changed
   4. Labels (performance, enhancement)
   ```

6. **Execute PR creation:**
   - Copy the generated PR content
   - Go to GitHub and create the PR
   - Or use GitHub CLI: `gh pr create --fill`

**Expected Outcome:**
- Professional PR created
- Comprehensive description generated
- Team standards followed

---

### Exercise 3.2: Advanced Code Review with Copilot (15 min)

**Task:** Perform comprehensive code reviews using Copilot

1. **Review your own PR:**
   In Agent mode:
   ```
   Review the changes in my feature/performance-optimization branch.
   Provide a comprehensive code review covering:
   
   **Code Quality:**
   - Readability and maintainability
   - Adherence to Python/FastAPI best practices
   - Code organization and structure
   
   **Performance:**
   - Are optimizations effective?
   - Any potential performance regressions?
   - Memory usage considerations
   
   **Security:**
   - Input validation
   - SQL injection risks
   - Authentication/authorization issues
   
   **Testing:**
   - Are changes adequately tested?
   - Edge cases covered?
   - What additional tests needed?
   
   **Documentation:**
   - Code comments adequate?
   - API documentation updated?
   - README changes needed?
   
   Format as a GitHub review comment with specific file/line references.
   ```

2. **Address review comments:**
   Based on Copilot's review, fix any issues:
   ```
   Based on your review, implement the critical fixes you identified.
   Prioritize security issues, then performance, then code quality.
   ```

3. **Generate review response:**
   ```
   Generate professional responses to address each review comment:
   - Acknowledge valid concerns
   - Explain design decisions
   - List implemented fixes
   - Note items for future work
   ```

4. **Security-focused review:**
   ```
   #codebase Perform a security audit of this application.
   Check for:
   - OWASP Top 10 vulnerabilities
   - Input validation gaps
   - Authentication/authorization issues
   - Sensitive data exposure
   - Dependency vulnerabilities
   
   Provide actionable recommendations with severity ratings.
   ```

5. **Performance review:**
   ```
   #codebase Analyze this application for performance issues.
   Focus on:
   - O(n²) or worse algorithms
   - Unnecessary database calls
   - Memory leaks
   - Blocking operations
   - Caching opportunities
   
   Suggest specific optimizations with expected impact.
   ```

**Expected Outcome:**
- Comprehensive code review completed
- Security issues identified and addressed
- Performance optimizations validated
- Professional review comments

---

### Exercise 3.3: Automated PR Summary and Updates (10 min)

**Task:** Generate PR summaries and handle updates

1. **Generate PR summary for stakeholders:**
   ```
   Generate an executive summary of my performance-optimization PR for non-technical stakeholders.
   Include:
   - Business impact
   - Key improvements in simple terms
   - Risks and mitigation
   - Rollout plan
   Keep it under 200 words.
   ```

2. **Handle PR update requests:**
   Simulate a reviewer requesting changes:
   ```
   A reviewer requested these changes:
   1. Add input validation for all API endpoints
   2. Implement rate limiting
   3. Add audit logging for all modifications
   
   Create an implementation plan and then implement these changes.
   ```

3. **Generate update comment:**
   After implementing changes:
   ```
   Generate a PR comment summarizing the updates I made:
   - List each reviewer concern
   - Explain how it was addressed
   - Reference specific commits
   - Mention any new tests added
   ```

4. **Prepare merge message:**
   ```
   Generate a comprehensive merge commit message for this PR that:
   - Summarizes all changes
   - Lists performance improvements with metrics
   - Notes breaking changes
   - Includes co-authors if applicable
   ```

**Expected Outcome:**
- Professional PR communication
- Changes clearly documented
- Ready for merge

---

### Exercise 3.4: GitHub MCP Server Integration (20 min)

**What is GitHub MCP Server?**
The GitHub MCP Server allows Copilot to directly interact with GitHub APIs to create PRs, manage issues, perform code reviews, and more - all without leaving VS Code.

**Task:** Automate PR workflows using GitHub MCP Server

#### Part A: Setup GitHub MCP Server (5 min)

1. **Check if GitHub MCP Server is available:**
   
   In Copilot Chat:
   ```
   What MCP servers are currently configured? 
   Is the GitHub MCP Server available?
   ```

2. **Configure GitHub MCP Server (if needed):**
   
   Ask Copilot:
   ```
   How do I configure the GitHub MCP Server in VS Code?
   I need it to access my GitHub account at https://github.com/YOUR_USERNAME/YOUR_REPO
   ```
   
   Follow the configuration steps, which typically include:
   - Installing the GitHub MCP Server extension (if not already installed)
   - Authenticating with GitHub
   - Granting necessary permissions (repo, PR, issues)

3. **Verify authentication:**
   ```
   Using the GitHub MCP Server, check my authentication status and list my repositories.
   ```

**Expected:** Copilot uses GitHub MCP Server to confirm authentication and show your repos.

#### Part B: Automated PR Creation via MCP (5 min)

1. **Create PR directly from Copilot:**
   
   Instead of manually creating a PR on GitHub, use the MCP server:
   ```
   Using the GitHub MCP Server, create a pull request for my feature/performance-optimization branch:
   
   Title: "perf: Optimize database queries and add caching"
   Base: main
   Head: feature/performance-optimization
   
   Description:
   ## Changes
   - Optimized database query performance
   - Implemented response caching layer
   - Added batch processing for bulk operations
   
   ## Performance Improvements
   - API response time: -45%
   - Database query count: -60%
   - Memory usage: -20%
   
   ## Testing
   - Added performance benchmarks
   - All existing tests passing
   - New integration tests for caching
   
   Labels: performance, enhancement
   Assignees: @me
   ```

2. **Verify PR creation:**
   ```
   Using the GitHub MCP Server, get the details of the PR I just created.
   Show me the PR number, URL, and current status.
   ```

3. **Add reviewers programmatically:**
   ```
   Using the GitHub MCP Server, add reviewers to PR #[NUMBER]:
   - Request review from team members who last modified these files
   - Add @senior-architect for security review
   ```

**Expected Outcome:**
- PR created without leaving VS Code
- Reviewers assigned automatically
- Full GitHub integration via MCP

#### Part C: Automated Code Review via MCP (5 min)

1. **Fetch PR for review:**
   ```
   Using the GitHub MCP Server, get the list of open PRs in this repository.
   Fetch the details of PR #[NUMBER] including:
   - Changed files
   - Diff content
   - Existing comments
   ```

2. **Perform automated review:**
   ```
   Using the GitHub MCP Server, analyze the changes in PR #[NUMBER] and post a code review with:
   
   Review focus:
   - Code quality issues
   - Security vulnerabilities
   - Performance concerns
   - Test coverage gaps
   
   Post comments directly to the PR for each issue found.
   Use the COMMENT type for suggestions, REQUEST_CHANGES for critical issues.
   ```

3. **Respond to review comments:**
   ```
   Using the GitHub MCP Server, fetch all review comments on my PR #[NUMBER].
   For each comment, generate a professional response acknowledging the feedback.
   Post the responses to the PR.
   ```

4. **Update PR status:**
   ```
   Using the GitHub MCP Server, update PR #[NUMBER]:
   - Add label: "ready-for-review"
   - Update description to include: "All review comments addressed"
   - Post a comment: "Changes have been made based on review feedback. Ready for final approval."
   ```

**Expected Outcome:**
- Code review posted directly from Copilot
- Comments addressed programmatically
- PR status updated automatically

#### Part D: PR Management and Merging (5 min)

1. **Check PR status and approvals:**
   ```
   Using the GitHub MCP Server, get the approval status for PR #[NUMBER]:
   - How many approvals received?
   - Are there any requested changes?
   - Are all status checks passing?
   - Is it ready to merge?
   ```

2. **Handle merge conflicts (if any):**
   ```
   Using the GitHub MCP Server, check if PR #[NUMBER] has merge conflicts.
   If yes, show me the conflicting files and help me resolve them.
   ```

3. **Merge the PR programmatically:**
   ```
   Using the GitHub MCP Server, merge PR #[NUMBER]:
   - Merge method: squash
   - Commit message: "perf: Optimize database queries and add caching (#[NUMBER])"
   - Delete branch after merge: yes
   ```

4. **Post-merge cleanup:**
   ```
   Using the GitHub MCP Server:
   1. Confirm the PR was merged successfully
   2. Delete the feature/performance-optimization branch
   3. Create a release note draft with the changes
   ```

5. **Automated follow-up:**
   ```
   Using the GitHub MCP Server, create a new issue:
   
   Title: "Monitor performance metrics post-optimization"
   Body: |
     Following the merge of PR #[NUMBER], we should monitor:
     - API response times
     - Cache hit rates
     - Database load
     
     Track metrics for 1 week and report findings.
   
   Labels: monitoring, follow-up
   Assignees: @me
   Milestone: Sprint 12
   ```

**Expected Outcome:**
- PR merged without leaving VS Code
- Automated post-merge tasks completed
- Follow-up issue created
- Complete GitHub workflow automation

#### Part E: Advanced MCP Automation (Bonus)

1. **Create custom PR workflow:**
   ```
   Using the GitHub MCP Server, create an automated workflow:
   
   1. Find all PRs with label "ready-for-merge"
   2. For each PR:
      - Check if all reviews approved
      - Verify CI/CD passed
      - If both true: merge automatically
      - Post notification comment
   3. Generate a summary report
   ```

2. **Bulk PR operations:**
   ```
   Using the GitHub MCP Server:
   1. Find all stale PRs (no activity in 30+ days)
   2. Add label "stale"
   3. Post comment: "This PR has been inactive for 30 days. Please update or close."
   4. List all affected PRs
   ```

**Expected Outcome:**
- Advanced GitHub automation
- Bulk operations via MCP
- Custom workflow creation
- Full understanding of GitHub MCP capabilities

---

**Key Takeaways:**
- GitHub MCP Server eliminates manual GitHub web interactions
- Full PR lifecycle can be managed from VS Code
- Code reviews can be automated and posted directly
- Custom workflows can be built using MCP tools
- Increased productivity through automation

---

## Part 4: Advanced Debugging and Optimization (30 minutes)

### Exercise 4.1: Complex Bug Investigation (15 min)

**Task:** Use Copilot for advanced debugging

1. **Introduce a complex bug:**
   Add this code to `app/routers/calculations.py`:
   ```python
   @router.post("/calculations/batch-weight")
   async def calculate_batch_weight(products: list):
       total_weight = 0
       for product in products:
           # Intentional bugs for debugging exercise
           weight = product['dimensions']['length'] * product['dimensions']['width']
           total_weight += weight
       return {"total_weight": total_weight}
   ```

2. **Debug with Copilot:**
   ```
   This batch weight calculation endpoint is returning incorrect results.
   The test data:
   - Product 1: 304 sheet, 2000x1000x3mm
   - Product 2: A36 plate, 3000x1500x10mm
   Expected: Approximately 100kg total
   Actual: 3000000
   
   Analyze the code and identify all bugs. Explain why they cause wrong results.
   ```

3. **Implement fixes:**
   ```
   Now fix all the bugs you identified. Show me the corrected code with:
   - Proper thickness consideration
   - Density application
   - Unit conversions
   - Type safety
   - Error handling
   ```

4. **Advanced debugging techniques:**
   ```
   Show me how to add comprehensive logging to this function for debugging:
   - Input validation
   - Intermediate calculations
   - Error conditions
   - Performance metrics
   
   Use Python's logging module properly.
   ```

5. **Write debugging tests:**
   ```
   Create tests that would have caught these bugs earlier.
   Include:
   - Unit tests for each calculation step
   - Integration test for the endpoint
   - Edge cases (zero dimensions, missing data, etc.)
   ```

**Expected Outcome:**
- Bugs identified and fixed
- Proper logging implemented
- Preventive tests created
- Advanced debugging skills demonstrated

---

### Exercise 4.2: Performance Profiling and Optimization (15 min)

**Task:** Profile and optimize application performance

1. **Add performance testing:**
   ```
   Create a performance test suite that:
   1. Tests API response times under load
   2. Measures database operation speed
   3. Profiles calculation functions
   4. Tests with various data sizes (10, 100, 1000 products)
   
   Use pytest-benchmark and locust for load testing.
   ```

2. **Profile the application:**
   ```
   Show me how to profile this FastAPI application to identify:
   - Slow endpoints
   - Memory usage patterns
   - CPU bottlenecks
   - Database query performance
   
   Use cProfile and memory_profiler.
   ```

3. **Optimize hot paths:**
   ```
   Based on profiling, optimize the most expensive operations in:
   - steel_utils.py (calculations)
   - database.py (queries)
   - inventory.py (endpoint handlers)
   
   Use techniques like:
   - Caching with functools.lru_cache
   - Batch operations
   - Async/await optimization
   - Algorithmic improvements
   ```

4. **Implement caching strategy:**
   ```
   Implement a caching layer for:
   - Product lookups (cache 5 minutes)
   - Weight calculations (cache by dimensions+grade)
   - List operations (cache 30 seconds)
   
   Use Redis or in-memory caching.
   ```

5. **Measure improvements:**
   ```
   Create before/after performance comparison tests.
   Show improvement metrics for:
   - Response time reduction
   - Memory usage reduction
   - Throughput increase
   - Cache hit rates
   ```

**Expected Outcome:**
- Application profiled
- Performance bottlenecks identified
- Optimizations implemented
- Measurable performance improvements

---

## Part 5: Enterprise Patterns and Best Practices (20 minutes)

### Exercise 5.1: Security Hardening (10 min)

**Task:** Implement enterprise security practices

1. **Security audit:**
   ```
   Perform a comprehensive security audit of the steel-inventory-api.
   
   Check for:
   - Authentication/authorization missing
   - SQL injection vulnerabilities
   - XSS attack vectors
   - CSRF protection
   - Rate limiting
   - Input validation gaps
   - Sensitive data exposure
   - Dependency vulnerabilities
   
   Provide a prioritized remediation plan with code examples.
   ```

2. **Implement authentication:**
   ```
   Add JWT-based authentication to the API:
   1. Create auth router with login endpoint
   2. Add User model and authentication logic
   3. Protect all write endpoints (POST, PUT, DELETE)
   4. Add role-based access (admin, user, readonly)
   5. Include proper password hashing
   
   Use FastAPI security utilities.
   ```

3. **Add input sanitization:**
   ```
   Implement comprehensive input validation and sanitization:
   - Product codes: alphanumeric with hyphens only
   - Quantities: positive integers
   - Dimensions: positive numbers within realistic ranges
   - Grades: whitelist of valid steel grades
   - SQL injection prevention
   ```

4. **Implement audit logging:**
   ```
   Add audit logging for all data modifications:
   - Who made the change
   - What was changed (before/after)
   - When it happened
   - Request metadata (IP, user agent)
   
   Store in audit_log table.
   ```

**Expected Outcome:**
- Security vulnerabilities identified
- Authentication implemented
- Input validation hardened
- Audit trail created

---

### Exercise 5.2: Code Quality and Maintainability (10 min)

**Task:** Implement enterprise code quality standards

1. **Set up linting and formatting:**
   ```
   Configure pre-commit hooks with:
   - Black (code formatting)
   - Ruff (linting)
   - mypy (type checking)
   - pytest (tests must pass)
   
   Show me the .pre-commit-config.yaml file.
   ```

2. **Add comprehensive type hints:**
   ```
   Add complete type hints to all files in the app/ directory.
   Use:
   - Type aliases for complex types
   - Generic types where appropriate
   - Optional for nullable values
   - Union types for alternatives
   
   Ensure mypy passes in strict mode.
   ```

3. **Improve error handling:**
   ```
   Implement a comprehensive error handling strategy:
   - Custom exception classes for different error types
   - Global exception handlers in FastAPI
   - Proper HTTP status codes
   - Detailed error messages for debugging (dev mode)
   - Safe error messages for production
   - Error logging with stack traces
   ```

4. **Add documentation:**
   ```
   Generate comprehensive documentation:
   - OpenAPI/Swagger improvements (better descriptions, examples)
   - Docstrings for all functions (Google style)
   - Architecture documentation (system design)
   - Deployment guide
   - Runbook for common operations
   
   Create docs/ directory with markdown files.
   ```

5. **Create CI/CD pipeline:**
   ```
   Create a GitHub Actions workflow (.github/workflows/ci.yml) that:
   - Runs on push and PR
   - Tests with multiple Python versions (3.9, 3.10, 3.11)
   - Runs linting (black, ruff, mypy)
   - Runs tests with coverage (require >80%)
   - Builds Docker image
   - Deploys to staging on main branch merge
   
   Include quality gates that prevent merge if checks fail.
   ```

**Expected Outcome:**
- Code quality tools configured
- Complete type coverage
- Robust error handling
- Comprehensive documentation
- CI/CD pipeline operational

---

## Part 6: Advanced Copilot Memory Patterns (20 minutes)

### Exercise 6.1: Multi-Scope Memory Strategy (10 min)

**What is Advanced Memory Management?**
At the advanced level, you'll learn to leverage all three memory scopes strategically: user memory (cross-workspace), session memory (conversation-specific), and repository memory (project-specific).

**Task:** Implement a comprehensive memory strategy for enterprise projects

1. **Enable and understand memory scopes:**
   
   In Copilot Chat:
   ```
   Explain the three memory scopes in GitHub Copilot:
   - User memory (/memories/)
   - Session memory (/memories/session/)
   - Repository memory (/memories/repo/)
   
   When should I use each scope?
   ```

2. **Set up user-level standards (cross-project):**
   ```
   Remember these in USER memory:
   
   My coding standards:
   - Use type hints for all Python functions
   - Follow PEP 8 with 88-char line length (Black)
   - Prefer composition over inheritance
   - Write docstrings in Google style
   - Use async/await for I/O operations
   - Implement proper logging (not print statements)
   
   These apply to ALL my Python projects.
   ```

3. **Set up repository-specific conventions:**
   ```
   Remember these BlueScope steel-inventory-api conventions in REPOSITORY memory:
   
   API Conventions:
   - All endpoints use /api/v1/ prefix
   - Datetime fields use ISO 8601 format
   - Weight units: kilograms (kg)
   - Dimension units: millimeters (mm)
   - Pagination: limit=50 default, max=1000
   
   Database Conventions:
   - Product codes format: STL-[A-Z0-9]{3,6}
   - All timestamps in UTC
   - Soft deletes: is_deleted flag
   - Audit fields: created_at, updated_at, created_by, updated_by
   
   Steel Grades:
   - Supported: A36, A572-50, 304, 316L, A653
   - Density lookup in steel_utils.DENSITY_MAP
   - Default safety factor: 1.15
   
   Store in repository memory.
   ```

4. **Use session memory for task context:**
   ```
   Remember for THIS session:
   
   Current task: Implementing supplier integration feature
   Branch: feature/supplier-integration
   Ticket: STEEL-456
   Dependencies: Requires MCP server for supplier API
   Deadline: End of sprint (Friday)
   Reviewer: Senior architect (security focus)
   
   Store in session memory.
   ```

5. **Test memory-driven development:**
   ```
   Generate a new API endpoint for product search following our repository conventions.
   Use the standards from user memory and repository memory.
   ```
   
   Expected: Copilot generates code that follows your coding standards, uses correct units, applies proper format, etc.

**Expected Outcome:**
- Understanding of three-tier memory architecture
- Standards stored appropriately by scope
- Code generation reflects stored conventions
- Reduced need to repeat context

---

### Exercise 6.2: Memory-Driven Code Reviews (10 min)

**Task:** Use memory to enforce team standards in code reviews

1. **Store review criteria in repository memory:**
   ```
   Remember our code review checklist in REPOSITORY memory:
   
   BlueScope Code Review Standards:
   
   Security:
   - [ ] Input validation on all endpoints
   - [ ] SQL injection prevention (parameterized queries)
   - [ ] Authentication required for write operations
   - [ ] Sensitive data not logged
   - [ ] Rate limiting implemented
   
   Performance:
   - [ ] Database queries optimized (no N+1)
   - [ ] Caching used for expensive operations
   - [ ] Async operations for I/O
   - [ ] Pagination for large datasets
   - [ ] Indexes on frequently queried fields
   
   Testing:
   - [ ] Unit tests >80% coverage
   - [ ] Integration tests for endpoints
   - [ ] Edge cases covered
   - [ ] Error handling tested
   - [ ] Performance benchmarks included
   
   Documentation:
   - [ ] Docstrings on public functions
   - [ ] API docs updated
   - [ ] README updated if needed
   - [ ] Breaking changes noted
   ```

2. **Perform memory-aware code review:**
   
   Review your recent changes:
   ```
   Review my latest code changes against the BlueScope Code Review Standards stored in repository memory.
   
   Provide a detailed review with:
   - Checklist items passed ✓
   - Checklist items failed ✗
   - Specific recommendations for fixes
   - Priority level (Critical/High/Medium/Low)
   ```

3. **Generate review comments:**
   ```
   Generate GitHub review comments for any failed checklist items.
   Reference specific files and line numbers.
   Suggest fixes that align with our repository conventions.
   ```

4. **Create PR description using memory:**
   ```
   Generate a pull request description for the supplier integration feature using:
   - Session memory (current task context)
   - Repository memory (conventions and standards)
   - User memory (my coding standards)
   
   Include:
   - What changed (from session context)
   - How it follows our conventions (from repo memory)
   - Testing completed (from review standards)
   - Breaking changes (if any)
   ```

5. **Verify memory persistence:**
   
   Close and reopen VS Code, then:
   ```
   What are the supported steel grades according to our repository standards?
   ```
   
   Expected: Copilot recalls from repository memory.

**Expected Outcome:**
- Code reviews based on stored standards
- Consistent review quality
- Automated checklist verification
- Memory-driven PR generation
- Team standards enforced automatically

---

## Final Challenge: Build a Complete Feature Pipeline (10 minutes)

**Goal:** Implement a complete enterprise feature using all advanced techniques

### Scenario: BlueScope Supplier Integration

BlueScope wants to integrate with external suppliers to automatically check product availability and pricing.

### Requirements:

1. **MCP Server for Supplier API**
   - Mock supplier API via MCP
   - Provides: check_availability, get_quote, place_order

2. **Custom Agent: Supplier Manager**
   - Coordinates supplier interactions
   - Compares quotes from multiple suppliers
   - Makes purchase recommendations

3. **API Integration**
   - New endpoint: POST /procurement/request-quote
   - Gets quotes from suppliers via MCP
   - Returns best option

4. **Complete PR Workflow**
   - Feature branch with semantic commits
   - Comprehensive tests (>90% coverage)
   - Security review passed
   - Performance benchmarks included
   - Full documentation

### Your Task:

Use everything you've learned to implement this feature end-to-end:

1. **Plan (5 min):**
   ```
   Create a comprehensive implementation plan for the supplier integration feature.
   Include: MCP server design, agent configuration, API endpoints, tests, security, documentation.
   ```

2. **Implement (20 min):**
   - Build MCP server
   - Create supplier-manager agent
   - Implement API endpoints
   - Write tests
   - Add security measures
   - Create documentation

3. **Review & PR (5 min):**
   - Self-review with Copilot
   - Create PR using GitHub MCP Server (not manually)
   - Post automated code review via MCP
   - Address review comments
   - Merge via MCP when ready

### Deliverables:

- ✅ Supplier MCP server running
- ✅ Supplier-manager agent configured
- ✅ Quote request endpoint working
- ✅ Tests passing (>90% coverage)
- ✅ Security validated
- ✅ Documentation complete
- ✅ PR created and reviewed via GitHub MCP Server
- ✅ PR merged programmatically via MCP

---

## Lab Completion Checklist

### Advanced Skills Mastered
- [x] MCP server creation and integration (custom + GitHub)
- [x] GitHub MCP Server for PR automation
- [x] Complex multi-agent workflows
- [x] Advanced agent collaboration patterns
- [x] Multi-scope memory strategies (user, session, repository)
- [x] Memory-driven code reviews and standards enforcement
- [x] Automated PR creation, review, and merging via MCP
- [x] Comprehensive code reviews
- [x] Advanced debugging techniques
- [x] Performance profiling and optimization
- [x] Security hardening
- [x] Enterprise code quality standards
- [x] CI/CD pipeline configuration

### Deliverables
1. ✅ Custom MCP server for steel data
2. ✅ GitHub MCP Server integrated and configured
3. ✅ Multi-agent system with handoffs (6+ agents)
4. ✅ Advanced memory configuration (3 scopes)
5. ✅ Automated PR workflows via GitHub MCP
6. ✅ Performance-optimized codebase
7. ✅ Security-hardened application
8. ✅ Comprehensive test coverage (>90%)
9. ✅ Complete documentation
10. ✅ CI/CD pipeline
11. ✅ Professional PRs with automated reviews

---

## Beyond This Lab

### Next Steps for Mastery:

1. **Build Internal Tools:**
   - Create MCP servers for your company's systems
   - Connect GitHub MCP Server to your team's repositories
   - Custom agents for your team's workflows
   - Skills library for domain knowledge
   - Automate repetitive GitHub operations

2. **Optimize Team Workflows:**
   - Automate PR creation and reviews via GitHub MCP
   - Standardize PR templates and automated checks
   - Create review checklists enforced via MCP
   - Set up automated issue triaging
   - Implement automated release workflows

3. **Share Knowledge:**
   - Document patterns that work
   - Create team training materials
   - Build shared agent/skill libraries

4. **Stay Updated:**
   - Follow Copilot release notes
   - Experiment with new features
   - Join Copilot community discussions

### Advanced Topics to Explore:

- **Multi-repo Agents:** Agents that work across repositories
- **GitHub MCP Advanced Workflows:** Automated issue management, release automation, CI/CD integration
- **Custom MCP Servers:** Build MCP servers for Jira, Azure DevOps, AWS, etc.
- **Custom Language Models:** Fine-tuned models for your domain
- **Advanced MCP Patterns:** Streaming, webhooks, real-time data
- **GitHub Apps with MCP:** Create custom GitHub Apps that integrate with Copilot
- **Enterprise Deployment:** Copilot for Business configurations
- **Metrics and Analytics:** Measuring Copilot ROI
- **Security and Compliance:** Meeting enterprise security requirements

---

## Troubleshooting

### MCP Server Issues
**Problem:** MCP server not responding
- Check server is running: `ps aux | grep node`
- Verify configuration in VS Code settings
- Check server logs for errors
- Restart VS Code window

**Problem:** MCP tools not appearing
- Reload VS Code window
- Verify server is in MCP settings
- Check server stdout for initialization messages

### Agent Issues
**Problem:** Agent not being invoked
- Check agent description specificity
- Verify YAML frontmatter is valid
- Check `user-invocable` setting
- Review agent selection criteria

**Problem:** Handoff not working
- Verify target agent exists
- Check handoff configuration syntax
- Ensure target agent has required tools

### Performance Issues
**Problem:** Slow responses
- Check selected model (o1-preview is slower)
- Reduce context size (#file vs #codebase)
- Optimize agent descriptions
- Consider model caching

### PR Workflow Issues
**Problem:** Can't create PR
- Verify GitHub authentication
- Check branch is pushed to remote
- Ensure base branch exists
- Verify repository permissions

### GitHub MCP Server Issues
**Problem:** GitHub MCP Server not available
- Check if GitHub MCP Server extension is installed
- Verify GitHub authentication token is configured
- Reload VS Code window
- Check VS Code output panel for MCP server logs

**Problem:** GitHub operations failing
- Verify GitHub token has correct permissions (repo, PR, issues)
- Check rate limits on GitHub API
- Ensure repository exists and you have access
- Verify branch names are correct

**Problem:** PR creation via MCP fails
- Ensure branch is pushed to remote first
- Check if PR already exists
- Verify base branch exists
- Review GitHub MCP Server logs for detailed error

**Problem:** Automated review posting fails
- Verify you have write access to repository
- Check if PR is in draft mode (may restrict reviews)
- Ensure review comments format is correct
- Check GitHub API rate limits

---

## Appendix: Advanced Tips and Tricks

### MCP Best Practices
1. Keep tool responses concise (<5KB)
2. Use caching for expensive operations
3. Implement proper error handling
4. Add rate limiting for external APIs
5. Log all tool invocations
6. Verify GitHub token permissions before operations

### GitHub MCP Best Practices
1. **Batch Operations:** Group related GitHub actions to reduce API calls
2. **Error Handling:** Always check API responses for failures
3. **Rate Limiting:** Monitor GitHub API rate limits
4. **Idempotency:** Make operations safe to retry
5. **Audit Trail:** Log all GitHub operations for compliance

### Agent Design Principles
1. **Single Responsibility:** Each agent has one clear purpose
2. **Loose Coupling:** Agents work independently
3. **Clear Contracts:** Handoff parameters well-defined
4. **Fail Gracefully:** Handle missing agents/tools
5. **Cost Awareness:** Use appropriate models

### PR Review Checklist
- [ ] Code follows team style guide
- [ ] All tests pass with >80% coverage
- [ ] No security vulnerabilities introduced
- [ ] Performance impact assessed
- [ ] Documentation updated
- [ ] Breaking changes noted
- [ ] Backward compatibility maintained
- [ ] Error handling complete

### Performance Optimization Checklist
- [ ] Profile before optimizing
- [ ] Measure improvements
- [ ] Cache expensive operations
- [ ] Use async where appropriate
- [ ] Optimize database queries
- [ ] Minimize external calls
- [ ] Consider algorithmic complexity

---

**Congratulations!** You've mastered GitHub Copilot's advanced features. You're now equipped to build enterprise-grade applications with AI-assisted development, automate complex workflows, and lead your team in adopting best practices.

## Feedback

Please provide feedback on this lab:
- What worked well?
- What was challenging?
- What topics need more coverage?
- What would you like to see in future labs?

Your feedback helps us improve the training experience for future participants.
