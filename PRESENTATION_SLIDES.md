# Using Cursor AI in Development
## Building a Task Management API with Golang

**90-Minute Technical Workshop**

---

## Slide 1: The Contract

# YOU are the driver
# AI is the copilot
# YOU are responsible for what ships

---

**This workshop is about:**
✅ Using AI as a powerful **assistant**  
✅ Maintaining **code quality** and **security**  
✅ Knowing when to trust AI vs when to take control  

**NOT about:**
❌ Blindly accepting AI suggestions  
❌ Copy-paste development  
❌ Letting AI make decisions  

---

## Slide 2: What We're Building

### Task Management API

**Tech Stack:**
- Golang 1.21+ with Gin
- GORM ORM + PostgreSQL
- Docker for database

---

## Slide 3: Workshop Flow

```
10 min  - Understanding Cursor
        ↓
10 min  - Setting Standards (.cursorrules)
        ↓
15 min  - Lab 1: Initialize app
        ↓
20 min  - Lab 2: Task filtering with review cycle
        ↓
8 min   - MCP explanation
        ↓
15 min  - Lab 3: Database with MCP
        ↓
5 min   - Critical lessons
        ↓
7 min   - Q&A

Total: 90 minutes
```

---

# Part 1: Understanding Cursor

---

## Slide 4: Cursor Basics

### What is Cursor?
- VS Code fork with native AI
- Context-aware pair programmer
- Multiple AI models available
- Monitor usage: Settings → Models

---

## Slide 5: Four Ways to Use Cursor

### 1. Inline Edit (Cmd/Ctrl + K)
```
Select code → Cmd+K → Describe change → Review → Accept/Reject
```
Fast, targeted edits

### 2. Chat Panel (Cmd/Ctrl + L)
```
Ask questions, get examples, understand code
```
Exploratory learning

### 3. Autocomplete (Tab)
```
Type → AI suggests → Tab to accept
```
Fastest for patterns (but verify!)

### 4. Composer (Cmd/Ctrl + I)
```
Multi-file operations
```
Use sparingly, high mistake rate

---

## Slide 6: Context is Everything

### The @ Commands

```
@codebase    - Search entire project (slow, broad)
@files       - Specific files (fast, targeted)
@folder      - Directory (medium scope)
@git         - Git history/diffs
@docs        - External documentation
```

### Example

❌ **Vague:** "Fix authentication"

✅ **Specific:**
```
"@files auth_handler.go Fix JWT token validation 
to check expiration. Use jwt-go library in go.mod. 
Follow error handling in @files error_handler.go"
```

---

## Slide 7: AI Makes Mistakes

### Common Errors

**Outdated libraries:**
```go
import "github.com/dgrijalva/jwt-go"  // ❌ Deprecated
import "github.com/golang-jwt/jwt/v5" // ✅ Current
```

**Security issues:**
```go
db.Where("id = " + input)      // ❌ SQL injection
db.Where("id = ?", input)      // ✅ Safe
```

**Over-engineering:**
```
Ask: "validate email"
Get: 200 lines of regex, DNS check, disposable detection
Need: validator.IsEmail(email)
```

### Always Review!

---

# Setting Standards: .cursorrules

---

## Slide 8: .cursorrules - You Define the Standards

### What is .cursorrules?

**File in project root that teaches AI YOUR standards:**
- Architecture patterns you use
- Coding conventions
- Security requirements
- Testing approach
- When AI should ask questions

> **This is how YOU drive. AI follows YOUR rules.**

---

## Slide 9: Creating Your .cursorrules

### Task: Define Project Standards (10 min)

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

---

## Slide 10: Review Your .cursorrules

### Checklist:

```
□ Does it define your architecture pattern?
□ Clear coding standards?
□ Security requirements specified?
□ Testing approach defined?
□ Tells AI to ask questions when unclear?
□ Specific to YOUR project?
```

**This file is your contract with AI**

Save it in project root before Lab 1!

---

# Lab 1: Initialize Golang App
## 15 minutes

---

## Slide 11: Lab 1 - Objective

**Goal:** Set up production-ready Go API structure

**What You'll Learn:**
- Writing clear, specific prompts with .cursorrules
- Reviewing AI-generated structure
- Validating setup works
- Ensuring AI follows YOUR standards

**Success Criteria:**
- [ ] Project follows .cursorrules standards
- [ ] Dependencies are appropriate
- [ ] Database setup works
- [ ] You understand every file

---

## Slide 12: Lab 1 - The Prompt

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

---

## Slide 13: Lab 1 - Review Checklist

### Check These Files:

**go.mod**
```bash
□ Correct module name?
□ Dependencies match our needs?
□ Versions are recent?
```

**docker-compose.yml**
```bash
□ PostgreSQL version specified?
□ Volume for data persistence?
□ Correct port mapping?
```

**main.go**
```bash
□ Error handling?
□ Graceful shutdown?
□ Env variables loaded?
□ Follows .cursorrules patterns?
```

**Project Structure**
```bash
□ Matches .cursorrules architecture?
□ Clean separation (handlers/services/database)?
□ Naming conventions followed?
```

---

## Slide 14: Lab 1 - Verify Setup

### Test It:

```bash
# Install dependencies
go mod download

# Verify it compiles
go build -o bin/api cmd/api/main.go

# Start database
docker-compose up -d postgres

# Verify database is running
docker-compose ps
```

**Check: Does structure match YOUR .cursorrules?**

---

## Slide 14b: Lab 1 - Add API Documentation (Optional)

### Make It Easy to Test Endpoints:

**Prompt:**
```
"Add gin-swagger for API documentation:
1. Add swag dependencies
2. Add swagger comments to handlers
3. Generate docs endpoint at /swagger/*
4. Include example in README for running

Follow Go swagger comment format."
```

### Result:
```bash
# Install swag CLI
go install github.com/swaggo/swag/cmd/swag@latest

# Generate docs
swag init -g cmd/api/main.go

# Run server
go run cmd/api/main.go

# Open browser: http://localhost:8080/swagger/index.html
```

**Now you can test endpoints interactively!**

---

## Slide 15: Lab 1 - Key Takeaways

✅ **.cursorrules defines YOUR standards first**  
✅ **AI follows your rules** (not its assumptions)  
✅ **Review everything** against .cursorrules  
✅ **Test that it works**  
✅ **.cursorrules is how YOU drive**  

> ".cursorrules is your contract with AI"

---

# The Review Process

---

## Slide 16: The Review Workflow

### Every Time AI Generates Code:

```
1. READ the code (don't just accept)
   ↓
2. ASK yourself:
   - Do I understand this?
   - Follows .cursorrules patterns?
   - Edge cases handled?
   - Secure? Efficient?
   ↓
3. TEST it
   - Compiles? Runs? Handles errors?
   ↓
4. VERIFY against requirements
   - Solves actual problem?
   - Better approaches?
   ↓
5. FIX or REJECT
   - Fix yourself or ask Cursor to fix
```

---

## Slide 17: Using Specific Context

### Be Specific:

❌ **Too Broad:**
```
"@codebase add filtering to tasks"
```

✅ **Specific:**
```
"@files handlers/task_handler.go 
@files services/task_service.go 
Add task filtering to GET /tasks endpoint:
- Query params: status, priority, due_date
- Validate status is valid enum
- Support multiple filters combined
- Return filtered results
- Follow .cursorrules patterns"
```

### For PRs:

```
"@git Review changes in task filtering.
Check: validation, edge cases, performance."
```

---

## Slide 18: Code Review with Cursor

### Use Cursor to Review Your Own Code:

**Prompt:**
```
"@git Review my changes for:
1. Compliance with .cursorrules standards
2. Edge cases I might have missed
3. Performance issues
4. Security concerns
5. Code quality issues

Task requirements: [paste acceptance criteria]"
```

### Generate PR Description:

**Prompt:**
```
"@git Generate PR description for these changes:
- Explain what was implemented
- List files changed
- Note any breaking changes
- Mention testing approach

Follow conventional commit format."
```

**Remember: Cursor reviews, YOU make final call**

---

# Lab 2: Task Filtering with Review Cycle
## 20 minutes

---

## Slide 19: Lab 2 - Objective

**Task:** Add task filtering and sorting to GET /tasks endpoint

**What You'll Practice:**
1. Clear prompting with acceptance criteria
2. Code review against .cursorrules
3. Identifying issues
4. Testing edge cases
5. Using Cursor for code review
6. Generating PR description

**Focus:** Complete workflow, from requirements to PR

---

## Slide 20: Lab 2 - Part A: Define Requirements

### Acceptance Criteria:

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

---

## Slide 21: Lab 2 - Part B: Generate Code

### Step 1: Update Task Model (if needed)

```
@files internal/models/task.go
Review Task model. Ensure it has:
- Status field with valid values
- Priority field (int)
- DueDate field
- Proper indexes for filtering
```

### Step 2: Service Layer

```
@files internal/services/task_service.go
Add filtering to GetTasks:
- Accept filters: status, priority, sortBy, order
- Validate filter values
- Build GORM query with filters
- Return filtered & sorted tasks
- Follow .cursorrules error handling
```

### Step 3: Handler

```
@files internal/handlers/task_handler.go
Update GET /tasks handler:
- Parse query parameters
- Validate parameters (status enum, priority range)
- Call service with filters
- Return 400 for invalid params
- Follow .cursorrules patterns
```

---

## Slide 22: Lab 2 - Part C: Review & Test

### Review Against .cursorrules:

```
□ Follows architecture pattern? (handler → service)
□ Proper error handling with context?
□ Input validation present?
□ Uses parameterized queries?
□ Naming conventions followed?
□ No hardcoded values?
```

### Common Issues to Check:

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

---

## Slide 23: Lab 2 - Part D: Generate Tests

### Prompt:

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

---

## Slide 24: Lab 2 - Part E: Use Cursor for Review

### Self-Review with Cursor:

```
"@git Review my task filtering implementation:

Acceptance Criteria:
[paste the criteria from Slide 20]

Check for:
1. All acceptance criteria met?
2. Edge cases handled?
3. Follows .cursorrules?
4. Performance issues?
5. Security concerns?"
```

### Generate PR Description:

```
"@git Create PR description for task filtering feature.
Include: what changed, why, testing approach, any notes for reviewer."
```

---

## Slide 25: Lab 2 - Debrief

### Discussion:

**Questions:**
1. Did Cursor's review find issues you missed?
2. How did .cursorrules help?
3. Were acceptance criteria clear enough?
4. Would you ship this to production?

### Key Lessons:

✅ **Clear acceptance criteria = better code**  
✅ **.cursorrules ensures consistency**  
✅ **Cursor reviews help catch issues**  
✅ **Test edge cases, not just happy path**  
✅ **YOU make final quality decision**  

> "Requirements + Standards + Review = Quality"

---

# MCP (Model Context Protocol)

---

## Slide 26: What is MCP?

### Model Context Protocol = Cursor Plugins

```
Cursor AI ←→ MCP Server ←→ External Resource
             (middleware)      (PostgreSQL, APIs)
```

### Use Cases:
- Query database schema
- Find missing indexes
- Generate migrations from DB
- Understand existing structure
- Debug database issues

### Official servers only!

---

## Slide 27: ⚠️ MCP Security Dangers

### Critical Risks:

**Database Access:**
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

**Credential Exposure:**
```json
// ❌ BAD
{"env": {"DB_PASSWORD": "secret123"}}

// ✅ GOOD
{"env": {"POSTGRES_CONNECTION": "${DATABASE_URL}"}}
```

---

## Slide 28: MCP Security Rules

### 🛡️ Always:

```
□ Use READ-ONLY database user
□ Never use production credentials
□ Audit MCP source code first
□ Use only official MCP servers
□ Least-privilege access
□ Don't store credentials in config
□ Test in safe environment
```

### Setup Read-Only User:

```sql
CREATE USER cursor_readonly WITH PASSWORD 'safe';
GRANT CONNECT ON DATABASE taskdb TO cursor_readonly;
GRANT USAGE ON SCHEMA public TO cursor_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO cursor_readonly;
```

---

## Slide 29: MCP Setup

### Installation:

```bash
npm install -g @modelcontextprotocol/server-postgres
```

### Configuration:

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

### Restart Cursor to load config

---

## Slide 30: MCP Best Practices

### ✅ Good Uses:

```
"@mcp-postgres show tables and columns"
"@mcp-postgres find missing indexes on foreign keys"
"@mcp-postgres what's the relationship between tasks and projects?"
"@mcp-postgres generate GORM model for tasks table"
```

### ❌ Avoid:

```
"Clean up old data" (risky!)
"Optimize database" (too vague)
"Fix this table" (AI doesn't know requirements)
```

> Use MCP for **inspection**, not **modification**

---

# Lab 3: Database with MCP
## 15 minutes

---

## Slide 31: Lab 3 - Objective

**Task:** Use MCP to analyze DB and create models

**What You'll Do:**
1. Set up read-only MCP
2. Inspect database schema
3. Generate GORM models from actual DB
4. Find missing indexes
5. Create migration

**Learn:** Safe AI-database interaction

---

## Slide 32: Lab 3 - Setup

### Start Database:

```bash
docker-compose up -d postgres
```

### Create Read-Only User:

```bash
docker-compose exec postgres psql -U postgres -d taskdb

CREATE USER cursor_readonly WITH PASSWORD 'readonly123';
GRANT CONNECT ON DATABASE taskdb TO cursor_readonly;
GRANT USAGE ON SCHEMA public TO cursor_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO cursor_readonly;
```

### Configure MCP:

```bash
echo "DATABASE_URL_READONLY=postgresql://cursor_readonly:readonly123@localhost:5432/taskdb" >> .env

mkdir -p .cursor
# Create mcp.json with config
```

**Restart Cursor**

---

## Slide 33: Lab 3 - Tasks

### Task 1: Inspect Schema

```
@mcp-postgres List all tables with columns, 
types, and constraints
```

```
@mcp-postgres Find missing indexes on foreign keys
```

### Task 2: Generate Models

```
@mcp-postgres Generate GORM models from actual DB:
- All columns with correct types
- GORM tags matching DB structure  
- Relationships (hasMany, belongsTo)
- JSON tags (snake_case)
- Match actual constraints

Save in internal/models/
```

### Task 3: Create Migration

```
@mcp-postgres Create migration to add 
missing index on tasks.user_id

File: internal/database/migrations/002_add_indexes.go
Include Up and Down migrations
```

---

## Slide 34: Lab 3 - Review

### Verify Models Match DB:

```go
□ Column names exact match
□ Data types correct
□ Relationships accurate
□ Constraints match
□ Foreign keys proper
```

### Common Mismatches:

```go
// AI guesses:
Status string

// But DB has:
status task_status_enum

// Fix with enum type:
type TaskStatus string
const (
    StatusPending TaskStatus = "pending"
    ...
)
```

### Test Migration:

```bash
go run cmd/migrate/main.go up
@mcp-postgres "show indexes on tasks"
```

---

## Slide 35: Lab 3 - Debrief

### Was MCP Helpful?

✅ Faster than manual DB inspection  
✅ Generated models match schema  
✅ Found missing indexes  

### Did You Trust It?

⚠️ Should verify with DB queries  
⚠️ Double-check relationships  
⚠️ Test generated code  

### Production Use?

✅ Yes for inspection/analysis  
⚠️ Carefully for code generation  
❌ Never for direct DB modifications  

---

# Critical Lessons

---

## Slide 36: You Must Drive

### The Hierarchy:

```
1. YOU define architecture
   ↓
2. YOU write requirements  
   ↓
3. AI generates code
   ↓
4. YOU review everything
   ↓
5. YOU fix issues
   ↓
6. YOU are responsible
```

### Clear Prompts = Better Results

Specify:
- Technologies & versions
- Architecture patterns
- Error handling
- Naming conventions
- Security requirements
- Performance needs

---

## Slide 37: The Bad Fix Problem

### What Happens:

```
1. You ask AI to implement X
2. AI tries approach A → fails
3. AI tries approach B → fails  
4. AI does HACKY workaround C → "works" but terrible
```

### Example:

```go
// AI should do:
db.Preload("User").First(&task, id)

// AI got stuck, did this instead:
db.First(&task, id)  // ❌ Ignore error
var user User
db.First(&user, task.UserID)  // ❌ N+1 query
task.User = user
return &task, nil  // ❌ Always nil error
```

### How to Detect:

- Overly complex
- Multiple queries where one works
- Errors ignored
- Comments like "workaround"
- **Your instinct: "this feels wrong"**

---

## Slide 38: When AI Can Drive

### ✅ Low Risk (AI can drive):

**Documentation**
- README, API docs, comments
- Easy to review, can't break production

**POCs & Demos**
- Throw-away code
- Speed > quality

**One-time Scripts**
- Used once, reviewed before running

**Boilerplate**
- Project structure, initial models
- With thorough review!

---

## Slide 39: When YOU Must Drive

### ⚠️ High Risk (YOU drive):

**Production Business Logic**
- Core features, complex algorithms
- Security-critical code
- YOU understand requirements

**Tests (Controversial!)**
- Tests evolve with code
- AI doesn't track context changes
- Maintaining AI tests is hard
- Better: YOU write, AI helps edge cases

**Architecture Decisions**
- Scaling, tech choices, patterns
- Long-term implications

**Database Schema**
- Schema affects everything
- Hard to rollback
- AI doesn't know your data

---

## Slide 40: The Testing Reality

### Many Think:
> "AI can write all my tests!"

### Reality:

```
❌ Tests are NOT write-once
❌ Business logic changes → tests change
❌ AI-written tests you don't understand
❌ Maintenance = nightmare
❌ End up rewriting anyway
```

### Better Approach:

```
✅ YOU write test structure
✅ AI helps with test cases
✅ AI suggests edge cases
✅ YOU maintain ownership
```

> If it's hard to maintain when broken, **YOU drive**

---

## Slide 41: ⚠️ AI Tool Dependency Warning (Don't Get Addicted!)

### The 4 Stages (Don't Let This Be You!):

**Stage 1: "Wow, this is amazing!"** 🤩
- Legitimate productivity gains
- Still thinking for yourself

**Stage 2: "Let me just ask Cursor..."** 🤔
- Default for many tasks
- Still feels helpful

**Stage 3: "I can't work without it"** 😰
- Anxiety when unavailable
- Skills starting to atrophy
- "Just one more prompt..."

**Stage 4: "Wait, how do I do this again?"** 😱
- Forgot how to code basic things
- Can't start without AI
- Submitting code you don't understand

### Warning Signs:
```
❌ Opening Cursor 50+ times/day reflexively
❌ Can't debug without asking AI first  
❌ Anxiety when rate-limited
❌ "I'll just regenerate it" instead of fixing
```

### Healthy vs Unhealthy:

**✅ Healthy:** AI augments YOUR abilities • You verify outputs • Skills maintained • **You can work without it**

**❌ Unhealthy:** AI replaces thinking • Blind trust • Skills decay • **Panic without it**

### The Test:
> **"If Cursor went down for a week, could you still deliver quality code?"**

**If NO → You're too dependent! 🚨**

---

## Slide 42: Final Wisdom

### The Rules:

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

### The Contract:

```
🚗 YOU are the driver
🤖 AI is the copilot
📋 YOU set destination
🔍 YOU review the route
✅ YOU are responsible
💪 YOU stay sharp
```

---

# Q&A & Resources

---

## Slide 43: Common Questions

**Q: Will AI replace developers?**  
A: No. Developers who use AI will replace those who don't.

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

**Q: How to prevent shipping bugs?**  
A: Same as human bugs
- Code review, tests, linting, security scans

---

## Slide 44: Resources

### Official:
- cursor.sh/docs
- modelcontextprotocol.io

### Workshop:
- github.com/yourorg/cursor-workshop
  - .cursorrules examples
  - Prompt library
  - Lab solutions

### Community:
- forum.cursor.sh
- discord.gg/cursor

### Shortcuts:
```
Cmd/Ctrl + K    Inline edit
Cmd/Ctrl + L    Chat
Cmd/Ctrl + I    Composer
Tab             Accept
Esc             Reject
```

---

## Slide 45: What to Remember

### Best Practices:

✅ **Clear prompts** with specifics  
✅ **Review everything**  
✅ **Test thoroughly**  
✅ **Use .cursorrules**  
✅ **MCP read-only**  
✅ **AI asks questions**  
✅ **YOU drive** critical code  

### The Philosophy:

> "AI is not here to replace your thinking.  
> It's here to amplify your productivity.  
> The best developers know when to use AI  
> and when to write code themselves."

---

## Slide 46: Next Steps

### This Week:

**Day 1:**
- [ ] Install Cursor
- [ ] Clone workshop repo
- [ ] Try first lab

**Day 2-3:**
- [ ] Create your .cursorrules
- [ ] Practice on real project
- [ ] Document what works

**Day 4-5:**
- [ ] Share with team
- [ ] Gather feedback
- [ ] Iterate and improve

### Remember: Start small, scale gradually

---

## Slide 47: Thank You!

# Questions?

**Stay Connected:**
- Workshop repo: github.com/yourorg/cursor-workshop
- Email: cursor-workshop@company.com
- Slack: #cursor-users

### The Final Reminder:

```
YOU are the driver
AI assists
YOU are responsible
```

**Happy coding! 🚀**

---

# End of Presentation

**Slides:** 48  
**Time:** ~90 minutes  
**Format:** Interactive workshop with 3 hands-on labs  

*Remember: YOU drive. AI assists. Don't become dependent. Stay sharp!*

