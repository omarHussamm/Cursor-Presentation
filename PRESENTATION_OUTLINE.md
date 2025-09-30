# Using Cursor AI in Development - REALISTIC WORKSHOP
## Building a Task Management API with Golang

**Duration:** 90 minutes  
**Format:** Hands-on Technical Workshop  
**Philosophy:** You Drive, AI Assists

---

## Workshop Philosophy

> **CRITICAL:** AI is a tool, not a replacement. You are the driver. You are responsible for what ships.

This workshop teaches you:
- ✅ How to use Cursor effectively as an **assistant**
- ✅ How to set standards FIRST with .cursorrules
- ✅ How to review and validate AI-generated code
- ✅ When to trust AI vs when to take control
- ✅ How to avoid dependency and maintain skills

**NOT teaching:**
- ❌ How to blindly accept AI suggestions
- ❌ Copy-paste development
- ❌ Letting AI make decisions

---

## Table of Contents

1. [Introduction: The Contract](#1-introduction-the-contract-5-min)
2. [Understanding Cursor](#2-understanding-cursor-10-min)
3. [Setting Standards: .cursorrules](#3-setting-standards-cursorrules-10-min)
4. [Lab 1: Initialize Golang App](#4-lab-1-initialize-golang-app-15-min)
5. [The Review Process](#5-the-review-process-no-time-integrated)
6. [Lab 2: Task Filtering with Review Cycle](#6-lab-2-task-filtering-20-min)
7. [MCP (Model Context Protocol)](#7-mcp-model-context-protocol-8-min)
8. [Lab 3: Database with MCP](#8-lab-3-database-with-mcp-15-min)
9. [Critical Lessons](#9-critical-lessons-7-min)
10. [Q&A and Resources](#10-qa-and-resources-7-min)

---

## 1. INTRODUCTION: THE CONTRACT (5 min)

### 1.1 Opening Statement (1 min)

**The Contract:**
```
YOU are the driver
AI is the copilot
YOU are responsible for what ships
```

**Speaker Notes:**
- Start strong: "If you think Cursor will write production code for you, you're wrong."
- Emphasize: "YOU must be in control."
- Set tone: **Augmentation, not automation**

### 1.2 What We're Building (2 min)

**Project:** Task Management API

**Tech Stack:**
- Golang 1.21+ with Gin
- GORM ORM + PostgreSQL
- Docker for database

**Why Simple?**
- Focus on workflow, not complexity
- Real-world patterns
- Database interactions for MCP demo

### 1.3 Workshop Structure (2 min)

```
10 min  - Understanding Cursor
10 min  - Setting Standards (.cursorrules) ✨ NEW!
15 min  - Lab 1: Initialize app
20 min  - Lab 2: Task Filtering
8 min   - MCP explanation
15 min  - Lab 3: Database with MCP
7 min   - Critical lessons + AI dependency warning
7 min   - Q&A

Total: 92 minutes (~90 min)
```

**Ground Rules:**
- Ask questions anytime
- We'll make mistakes to learn
- Laptop ready with Cursor installed

---

## 2. UNDERSTANDING CURSOR (10 min)

### 2.1 Cursor IDE Basics (3 min)

**What is Cursor?**
- VS Code fork with deep AI integration
- Context-aware pair programmer
- Multiple AI models available

**Speaker Notes:**
- Show: Settings → Models → Usage tracking
- Don't get into pricing details (models change frequently)

### 2.2 Core Features (4 min)

**Four Ways to Interact:**

1. **Inline Edit (Cmd/Ctrl + K)**
   - Quick, targeted edits
   - Always review before accepting

2. **Chat Panel (Cmd/Ctrl + L)**
   - Exploratory learning
   - Getting unstuck

3. **Autocomplete (Tab)**
   - Fastest for patterns
   - Can be wrong - verify!

4. **Composer (Cmd/Ctrl + I)**
   - Multi-file operations
   - Use sparingly, high mistake rate

**Live Demo (2 min):**
- Show each feature quickly
- Demonstrate accepting AND rejecting
- Show undo (Cmd+Z works!)

### 2.3 Context Awareness (3 min)

**The @ Commands:**

```
@codebase    - Search entire project (slow, broad)
@files       - Specific files (fast, targeted)
@folder      - Directory (medium scope)
@git         - Git history/diffs
@docs        - External documentation
```

**Example:**

❌ **Vague:** "Fix authentication"

✅ **Specific:**
```
"@files auth_handler.go Fix JWT token validation 
to check expiration. Use jwt-go library in go.mod. 
Follow error handling in @files error_handler.go"
```

**Key Point:**
> Specific context = better results. Vague prompts = bad code.

---

## 3. SETTING STANDARDS: .cursorrules (10 min)

### 3.1 What is .cursorrules? (2 min)

**File in project root that teaches AI YOUR standards:**
- Architecture patterns you use
- Coding conventions
- Security requirements
- Testing approach
- When AI should ask questions

> **This is how YOU drive. AI follows YOUR rules.**

**Speaker Notes:**
- Emphasize: "Set standards BEFORE generating code"
- This is the difference between driving and being driven

### 3.2 Creating Your .cursorrules (6 min)

**Task: Define Project Standards**

**Create `.cursorrules` with:**

```markdown
# Task Management API Standards

## Project Context
Task Management API built with Go, Gin, GORM, PostgreSQL

## Architecture
- Clean architecture: handlers → services → repositories
- Handlers: HTTP concerns only (parse input, return response)
- Services: Business logic, return domain errors
- Repositories: Database access

## Code Standards
- Error handling: Always check errors, log with context
- Naming: PascalCase (exported), camelCase (private)
- Database: Use GORM, parameterized queries only
- JSON tags: snake_case

## Testing
- Table-driven tests
- Test error cases, not just happy path
- Mock external dependencies
- Minimum 70% coverage

## Security
- Never log sensitive data
- Validate all user input
- Use environment variables for secrets

## API Documentation
- Add Swagger/OpenAPI comments to all handlers
- Format: // @Summary, @Description, @Param, @Success, @Failure
- Keep swagger docs in sync with code
- Document all request/response models

## When Unsure: ASK QUESTIONS
Format: "I need clarification on [topic]. 
Should I [option A] or [option B]?"
```

### 3.3 Review Your .cursorrules (2 min)

**Checklist:**
```
□ Does it define your architecture pattern?
□ Clear coding standards?
□ Security requirements specified?
□ Testing approach defined?
□ API documentation requirements?
□ Tells AI to ask questions when unclear?
□ Specific to YOUR project?
```

**This file is your contract with AI**

Save it in project root before Lab 1!

**Speaker Notes:**
- Show before/after code generation with same prompt
- Demonstrate how .cursorrules changes AI behavior

---

## 4. LAB 1: INITIALIZE GOLANG APP (15 min)

### 4.1 Lab Objective (1 min)

**Goal:** Set up production-ready Go API structure with YOUR standards

**What You'll Learn:**
- Writing clear prompts with .cursorrules
- Reviewing AI-generated structure
- Validating setup works
- Ensuring AI follows YOUR standards

**Success Criteria:**
- [ ] Project follows .cursorrules standards
- [ ] Dependencies are appropriate
- [ ] Database setup works
- [ ] You understand every file

### 4.2 Lab Instructions (12 min)

**Step 1: Create Project (4 min)**

**Prompt to use:**
```
Create a Go project structure for Task Management API.

Use .cursorrules file for all standards.

Technology:
- Go 1.21+, Gin, GORM, PostgreSQL
- Follow architecture in .cursorrules

Structure:
- cmd/api/main.go
- internal/handlers, models, services, database
- config/, .env.example, docker-compose.yml (PostgreSQL only), Makefile

Requirements:
- Go modules
- Only necessary dependencies
- Comments explaining directories
- Follow .cursorrules conventions
```

**Execute in Cursor chat after creating .cursorrules**

**Step 2: Review Generated Code (4 min)**

**Live Review Checklist:**

```bash
# 1. Check Go module
cat go.mod
# - Correct module name?
# - Dependencies match our needs?
# - Versions recent?

# 2. Check docker-compose.yml
cat docker-compose.yml
# - PostgreSQL version specified?
# - Volume for data persistence?
# - Correct port mapping?

# 3. Check main.go
cat cmd/api/main.go
# - Error handling?
# - Graceful shutdown?
# - Env variables loaded?
# - Follows .cursorrules patterns?

# 4. Project Structure
# - Matches .cursorrules architecture?
# - Clean separation (handlers/services/database)?
# - Naming conventions followed?
```

**Step 3: Add API Documentation (Optional, 2 min)**

**Prompt:**
```
"Add gin-swagger for API documentation:
1. Add swag dependencies
2. Add swagger comments to handlers
3. Generate docs endpoint at /swagger/*
4. Include example in README

Follow Go swagger comment format."
```

**Result:**
```bash
# Install swag CLI
go install github.com/swaggo/swag/cmd/swag@latest

# Generate docs
swag init -g cmd/api/main.go

# Run server
go run cmd/api/main.go

# Open: http://localhost:8080/swagger/index.html
```

**Step 4: Verify Setup (2 min)**

```bash
# Install dependencies
go mod download

# Verify it compiles
go build -o bin/api cmd/api/main.go

# Start database
docker-compose up -d postgres

# Verify running
docker-compose ps
```

### 4.3 Lab Debrief (2 min)

**Questions:**
- "Does structure match YOUR .cursorrules?"
- "Did AI follow your standards?"
- "Do you understand the setup?"

**Key Takeaways:**
```
✅ .cursorrules defines YOUR standards first
✅ AI follows your rules (not its assumptions)
✅ Review everything against .cursorrules
✅ Test that it works
✅ .cursorrules is how YOU drive
```

> ".cursorrules is your contract with AI"

---

## 5. THE REVIEW PROCESS (Integrated Throughout)

**Note:** Review is integrated into each lab, not a separate section.

**Review Workflow (shown in every lab):**

```
1. READ the code
   ↓
2. ASK: Follows .cursorrules? Edge cases? Secure?
   ↓
3. TEST it
   ↓
4. VERIFY requirements
   ↓
5. FIX or REJECT
```

**Using Specific Context:**

❌ Too Broad: `"@codebase add filtering"`

✅ Specific:
```
"@files handlers/task_handler.go 
@files services/task_service.go 
Add task filtering to GET /tasks:
- Query params: status, priority, sort_by, order
- Validate status is valid enum
- Support combined filters
- Follow .cursorrules patterns"
```

---

## 6. LAB 2: TASK FILTERING WITH REVIEW CYCLE (20 min)

### 6.1 Lab Objective (1 min)

**Task:** Add task filtering and sorting to GET /tasks endpoint

**What You'll Practice:**
1. Clear prompting with acceptance criteria
2. Code review against .cursorrules
3. Identifying issues
4. Testing edge cases
5. Using Cursor for code review
6. Generating PR description

**Focus:** Complete workflow, from requirements to PR

### 6.2 Part A: Define Requirements (2 min)

**Acceptance Criteria:**

```
Feature: Task Filtering and Sorting

As a user, I want to filter my tasks so I can find specific ones quickly.

Acceptance Criteria:
1. GET /tasks supports query parameters:
   - status: filter by status (pending/in_progress/completed)
   - priority: filter by priority (1-5)
   - sort_by: sort field (created_at/priority/due_date)
   - order: sort order (asc/desc)

2. Multiple filters can be combined
3. Invalid status values return 400 with error
4. Priority must be 1-5, else 400 error
5. Default sort: created_at desc
6. Returns paginated results
```

**Speaker Notes:**
- Emphasize clear requirements drive better code
- This replaces authentication (simpler, project-relevant)

### 6.3 Part B: Generate Code (5 min)

**Step 1: Update Task Model (if needed)**
```
@files internal/models/task.go
Review Task model. Ensure it has:
- Status field with valid values
- Priority field (int)
- DueDate field
- Proper indexes for filtering
```

**Step 2: Service Layer**
```
@files internal/services/task_service.go
Add filtering to GetTasks:
- Accept filters: status, priority, sortBy, order
- Validate filter values
- Build GORM query with filters
- Return filtered & sorted tasks
- Follow .cursorrules error handling
```

**Step 3: Handler**
```
@files internal/handlers/task_handler.go
Update GET /tasks handler:
- Parse query parameters
- Validate parameters (status enum, priority range)
- Call service with filters
- Return 400 for invalid params
- Follow .cursorrules patterns
- Add Swagger comments
```

### 6.4 Part C: Review & Test (6 min)

**Review Against .cursorrules:**
```
□ Follows architecture? (handler → service)
□ Proper error handling with context?
□ Input validation present?
□ Uses parameterized queries?
□ Naming conventions followed?
□ No hardcoded values?
□ Swagger comments added?
```

**Common Issues to Check:**

```go
// ❌ Watch for: No validation
status := c.Query("status")  // What if invalid?

// ✅ Should be:
status := c.Query("status")
if status != "" && !isValidStatus(status) {
    c.JSON(400, gin.H{"error": "Invalid status"})
    return
}

// ❌ Watch for: SQL injection in sorting
db.Order(sortBy + " " + order)  // Dangerous!

// ✅ Should be:
allowedSorts := map[string]bool{
    "created_at": true, "priority": true, "due_date": true,
}
if !allowedSorts[sortBy] {
    sortBy = "created_at"  // Safe default
}
```

### 6.5 Part D: Generate Tests (3 min)

**Prompt:**
```
@files tests/task_test.go Create tests for task filtering:

Test cases:
1. Filter by status - returns only matching tasks
2. Filter by priority - returns only matching priority
3. Combined filters - both conditions applied
4. Invalid status - returns 400 error
5. Invalid priority - returns 400 error
6. Sort by created_at asc - correct order
7. Sort by priority desc - correct order
8. No filters - returns all user tasks

Use table-driven tests, mock database, testify/assert
```

### 6.6 Part E: Use Cursor for Review (2 min)

**Self-Review with Cursor:**

```
"@git Review my task filtering implementation:

Acceptance Criteria:
[paste the criteria from Part A]

Check for:
1. All acceptance criteria met?
2. Edge cases handled?
3. Follows .cursorrules?
4. Performance issues?
5. Security concerns?"
```

**Generate PR Description:**

```
"@git Create PR description for task filtering feature.
Include: what changed, why, testing approach, notes for reviewer."
```

### 6.7 Lab Debrief (1 min)

**Questions:**
1. Did Cursor's review find issues you missed?
2. How did .cursorrules help?
3. Were acceptance criteria clear enough?
4. Would you ship this?

**Key Lessons:**
```
✅ Clear acceptance criteria = better code
✅ .cursorrules ensures consistency
✅ Cursor reviews help catch issues
✅ Test edge cases, not just happy path
✅ YOU make final quality decision
```

> "Requirements + Standards + Review = Quality"

---

## 7. MCP (MODEL CONTEXT PROTOCOL) (8 min)

### 7.1 What is MCP? (2 min)

**Model Context Protocol = Cursor Plugins**

```
Cursor AI ←→ MCP Server ←→ External Resource
             (middleware)      (PostgreSQL, APIs)
```

**Use Cases:**
- Query database schema
- Find missing indexes
- Generate migrations from DB
- Debug database issues

**Official servers only!**

### 7.2 Security Dangers (3 min)

**⚠️ CRITICAL: MCP Has Full Access**

**Risk 1: Database Access**
```
MCP can:
✓ Read all data (including sensitive)
✓ Modify records
✓ Delete data
✓ Drop tables

AI might accidentally:
❌ "Clean test data" → Deletes real users
❌ "Optimize table" → Drops index
```

**Risk 2: Credential Exposure**
```json
// ❌ BAD
{"env": {"DB_PASSWORD": "secret123"}}

// ✅ GOOD
{"env": {"POSTGRES_CONNECTION": "${DATABASE_URL}"}}
```

**Security Rules:**
```
🛡️ MCP Security Checklist:

□ Use READ-ONLY database user
□ Never use production credentials
□ Audit MCP source code first
□ Use only official MCP servers
□ Least-privilege access
□ Don't store credentials in config
□ Test in safe environment
```

### 7.3 Setting Up PostgreSQL MCP (2 min)

**Installation:**
```bash
npm install -g @modelcontextprotocol/server-postgres
```

**Configuration:**
```json
// .cursor/mcp.json
{
  "mcpServers": {
    "postgres": {
      "command": "mcp-server-postgres",
      "args": ["--read-only"],
      "env": {
        "POSTGRES_CONNECTION": "${DATABASE_URL_READONLY}"
      }
    }
  }
}
```

**Create Read-Only User:**
```sql
CREATE USER cursor_readonly WITH PASSWORD 'safe';
GRANT CONNECT ON DATABASE taskdb TO cursor_readonly;
GRANT USAGE ON SCHEMA public TO cursor_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO cursor_readonly;
```

### 7.4 Best Practices (1 min)

**✅ Good Uses:**
```
"@mcp-postgres show tables and columns"
"@mcp-postgres find missing indexes on foreign keys"
"@mcp-postgres generate GORM model for tasks table"
```

**❌ Avoid:**
```
"Clean up old data" (risky!)
"Optimize database" (too vague)
"Fix this table" (AI doesn't know requirements)
```

> Use MCP for **inspection**, not **modification**

---

## 8. LAB 3: DATABASE WITH MCP (15 min)

### 8.1 Lab Objective (1 min)

**Task:** Use MCP to analyze DB and create models

**What You'll Do:**
1. Set up read-only MCP
2. Inspect database schema
3. Generate GORM models from actual DB
4. Find missing indexes
5. Create migration

**Learn:** Safe AI-database interaction

### 8.2 Lab Setup (3 min)

**Step 1: Start Database**
```bash
docker-compose up -d postgres
docker-compose ps
```

**Step 2: Create Read-Only User**
```bash
docker-compose exec postgres psql -U postgres -d taskdb

CREATE USER cursor_readonly WITH PASSWORD 'readonly123';
GRANT CONNECT ON DATABASE taskdb TO cursor_readonly;
GRANT USAGE ON SCHEMA public TO cursor_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO cursor_readonly;
\q
```

**Step 3: Configure MCP**
```bash
echo "DATABASE_URL_READONLY=postgresql://cursor_readonly:readonly123@localhost:5432/taskdb" >> .env

mkdir -p .cursor
# Create mcp.json with config
```

**Step 4: Restart Cursor**

### 8.3 Lab Tasks (9 min)

**Task 1: Inspect Schema (3 min)**

```
@mcp-postgres List all tables with columns, types, and constraints
```

```
@mcp-postgres Find missing indexes on foreign keys
```

**Task 2: Generate Models (3 min)**

```
@mcp-postgres Generate GORM models from actual DB:
- All columns with correct types
- GORM tags matching DB structure
- Relationships (hasMany, belongsTo)
- JSON tags (snake_case)
- Match actual constraints

Save in internal/models/
```

**Review:**
```
□ Column names exact match
□ Data types correct
□ Relationships accurate
□ Constraints match
□ Foreign keys proper
```

**Task 3: Create Migration (3 min)**

```
@mcp-postgres Create migration to add missing index on tasks.user_id

File: internal/database/migrations/002_add_indexes.go
Include Up and Down migrations
```

**Test:**
```bash
go run cmd/migrate/main.go up
@mcp-postgres "show indexes on tasks"
```

### 8.4 Lab Debrief (2 min)

**Was MCP Helpful?**
- ✅ Faster than manual inspection
- ✅ Models match schema
- ✅ Found missing indexes

**Did You Trust It?**
- ⚠️ Verify with DB queries
- ⚠️ Double-check relationships
- ⚠️ Test generated code

**Production Use?**
- ✅ Yes for inspection/analysis
- ⚠️ Carefully for code generation
- ❌ Never for direct DB modifications

---

## 9. CRITICAL LESSONS (7 min)

### 9.1 You Must Drive (2 min)

**The Hierarchy:**
```
1. YOU define architecture
2. YOU write requirements
3. AI generates code
4. YOU review everything
5. YOU fix issues
6. YOU are responsible
```

**Clear Prompts = Better Results**

Specify:
- Technologies & versions
- Architecture patterns
- Error handling
- Security requirements

### 9.2 The Bad Fix Problem (1 min)

**What Happens:**
```
1. You ask AI for feature X
2. AI tries approach A → fails
3. AI tries approach B → fails
4. AI does HACKY workaround → "works" but terrible
```

**How to Detect:**
- Overly complex code
- Multiple queries where one works
- Errors ignored
- Comments like "workaround"
- Your instinct: "this feels wrong"

**What to Do:**
- Start fresh with clearer prompt
- Research proper solution
- Write it yourself if needed

### 9.3 When AI Can Drive vs When YOU Drive (1 min)

**✅ AI Can Drive (low risk):**
- Documentation (README, API docs)
- POCs & Demos
- One-time scripts
- Boilerplate (with review!)

**⚠️ YOU Must Drive (high risk):**
- Production business logic
- Tests (they evolve!)
- Architecture decisions
- Database schema/migrations

**The Testing Reality:**
```
Many think: "AI can write all my tests!"

Reality:
- Tests are NOT write-once
- Logic changes → tests change
- If you don't understand tests, maintenance = nightmare

Better:
- YOU write test structure
- AI helps with test cases
- AI suggests edge cases
- YOU maintain ownership
```

### 9.4 AI Dependency Warning (2 min)

**⚠️ Don't Get Addicted!**

**The 4 Stages:**

**Stage 1: "Wow!"** 🤩 → **Stage 2: "Let me ask..."** 🤔 → **Stage 3: "Can't work without it"** 😰 → **Stage 4: "Wait, how?"** 😱

**Warning Signs:**
```
❌ Opening Cursor 50+ times/day reflexively
❌ Can't debug without asking AI first
❌ Anxiety when rate-limited
❌ "I'll just regenerate" instead of fixing
❌ Forgot how to do basic things
```

**Healthy vs Unhealthy:**

**✅ Healthy:**
- AI augments YOUR abilities
- You verify outputs
- Skills maintained
- **You can work without it**

**❌ Unhealthy:**
- AI replaces thinking
- Blind trust
- Skills decay
- **Panic without it**

**The Test:**
> **"If Cursor went down for a week, could you still deliver quality code?"**

**If NO → You're too dependent! 🚨**

**Speaker Notes:**
- This is funny but serious
- Share real example if you have one
- Emphasize maintaining skills

### 9.5 Final Wisdom (1 min)

**The Rules:**
```
1. Clear prompts with exact specs
2. Review EVERYTHING AI generates
3. Test thoroughly before shipping
4. Use .cursorrules for standards
5. MCP read-only is safer
6. Make AI ask questions when unclear
7. YOU drive critical code
8. Maintain your skills WITHOUT AI
```

**The Contract:**
```
🚗 YOU are the driver
🤖 AI is the copilot
📋 YOU set destination
🔍 YOU review the route
✅ YOU are responsible
💪 YOU stay sharp
```

---

## 10. Q&A AND RESOURCES (7 min)

### 10.1 Common Questions (4 min)

**Q: Will AI replace developers?**

A: No. Developers who use AI effectively will replace those who don't.

**Q: How much can I trust it?**

A: Trust but verify. Always.
- Documentation: 80%
- Boilerplate: 70%
- Business logic: 40%
- Security: 20%
- Architecture: 10%

**Q: Is my code safe?**

A: Configure privacy (Settings → Privacy)
- Never paste credentials/PII in chat

**Q: How to prevent bugs?**

A: Same as human bugs
- Code review, tests, linting, security scans

### 10.2 Resources (2 min)

**Official:**
- cursor.sh/docs
- modelcontextprotocol.io

**Workshop:**
- github.com/yourorg/cursor-workshop
  - .cursorrules examples
  - Prompt library
  - Lab solutions

**Community:**
- forum.cursor.sh
- discord.gg/cursor

**Shortcuts:**
```
Cmd/Ctrl + K    Inline edit
Cmd/Ctrl + L    Chat
Cmd/Ctrl + I    Composer
Tab             Accept
Esc             Reject
```

### 10.3 Next Steps (1 min)

**This Week:**
- Day 1: Install Cursor, clone repo, try first lab
- Day 2-3: Create .cursorrules, practice on real project
- Day 4-5: Share with team, iterate

**Remember: Start small, scale gradually**

---

## APPENDIX: Quick Reference

### Keyboard Shortcuts
```
Cmd/Ctrl + K    Inline edit
Cmd/Ctrl + L    Chat panel
Cmd/Ctrl + I    Composer
Tab             Accept
Esc             Reject
Cmd/Ctrl + Z    Undo
```

### Review Checklist
```
Before accepting AI code:

□ Understand what it does?
□ Follows .cursorrules patterns?
□ Errors handled properly?
□ Edge cases covered?
□ Secure (no secrets, injection)?
□ Efficient (no N+1 queries)?
□ Tests included?
□ Can you debug this?
□ Can you maintain it?
□ Solves actual requirement?
```

---

**Workshop Version:** 2.0  
**Last Updated:** 2024-09-30  
**Duration:** 90 minutes  
**Level:** Intermediate  
**Prerequisites:** Basic Go knowledge, Cursor installed

**Slides:** 48 total
