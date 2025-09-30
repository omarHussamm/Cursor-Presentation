---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Using Cursor AI in Development
  90-Minute Technical Workshop on Building a Task Management API with Golang
drawings:
  persist: false
transition: slide-left
title: Using Cursor AI in Development
mdc: true
---

# Using Cursor AI in Development
## Building a Task Management API with Golang

**Technical Workshop**


---
layout: two-cols
---

## Core Features

**Task Management Backend:**
- Create, update, delete tasks
- Filter by status & priority
- Sort & pagination
- Swagger API documentation


::right::

<br>
<br>

**Tech Stack:**
- Golang 1.21+ with Gin
- GORM ORM + PostgreSQL
- Docker for database


---

## This Workshop

**You will learn to:**
- **Set standards first** - Define your rules before AI generates code
- **Review systematically** - Know what to look for in AI-generated code
- **Balance control** - When to trust AI vs when to take the wheel

**And not tot:**
- ❌ Blindly accepting AI suggestions
- ❌ Copy-paste development


## The Golden Rule

#### You are the driver. AI is the copilot. You are responsible for what it does.


---

## Workshop Agenda
<br>

- Introducing Cursor
- Setting Standards (.cursorrules)
- Lab 1: Initialize Task Management API
- Lab 2: Task Filtering Task with Cursor Review Cycle
- MCP Explanation
- Lab 3: Database with MCP
- Additional Cursor Tips & Tricks
- Q&A

---

## Part 1: Introducing Cursor
<br>

- What is Cursor
- Cursor Editing Tools
- Cursor Context Awarness
- Cursor Risks


---


## What is Cursor?
<br>

- A modern code editor built on VS Code, enhanced with native AI features
- Your context-aware AI pair programmer, right in your editor
- Supports multiple AI models for different coding needs
- Easily monitor and manage AI usage in Settings → Models

---

## Cursor Editing Tools
<br>

- Inline Edit
- Autocomplete (Tab)
- Chat Panel Ask Mode
- Chat Panel Agent Mode 
- Terminal Inline Ask

---


## Give AI the Right Context

When asking AI for help, **be specific and point it to the right place.**  
Use "@" commands to guide the AI to the files, folders, or docs you want it to use.

<br>

| Command      | What it does                |
|--------------|----------------------------|
| `@codebase`  | Search whole project (slow) |
| `@files`     | Specific files (fast)       |
| `@folder`    | Directory                   |
| `@git`       | Git history/diffs           |
| `@docs`      | External docs               |

---

## Example: Vague vs. Specific

❌ **Vague:**  
> Fix authentication

✅ **Specific:**  
> @files auth_handler.go  
> Fix JWT token validation to check expiration.  
> Follow error handling in @files error_handler.go

<br>

**Tip:**  
The more context you give, the better the AI can help!

---


## AI can be a powerful tool, but it comes with real risks:
<br>

- Can hallucinate facts or invent APIs
- May use outdated/deprecated libraries
- Security is not guaranteed (e.g., SQL injection, XSS)
- Best practices? Sometimes, but not always
- Can overcomplicate simple problems
- Might ignore your project’s standards


---
layout: center
class: text-center
---

# Lab 1: Initialize the project

---
layout: center
class: text-center
---

# Setting Standards: .cursorrules

---

## .cursorrules - You Define the Standards

### What is .cursorrules?

**File in project root that teaches AI YOUR standards:**
- Architecture patterns you use
- Coding conventions
- Security requirements
- Testing approach
- When AI should ask questions

> **This is how YOU drive. AI follows YOUR rules.**

---
layout: two-cols
---

## Creating Your .cursorrules

### Task: Define Project Standards (10 min)

**Create `.cursorrules` with:**

::left::

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
```

::right::

```markdown
## Security
- Never log sensitive data
- Validate all user input
- Use environment variables for secrets

## API Documentation
- Add Swagger/OpenAPI comments
- Format: @Summary, @Param, @Success
- Keep docs in sync with code

## When Unsure: ASK QUESTIONS
Format: "I need clarification on [topic]. 
Should I [option A] or [option B]?"
```

---

## Review Your .cursorrules

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
layout: center
class: text-center
---

# The Review Process

---
layout: default
class: text-sm
---

## The Review Workflow

### Every Time AI Generates Code:

<div class="grid grid-cols-5 gap-4 mt-4">

**1. READ**
- Don't just accept
- Understand the code

**2. ASK**
- Do I understand?
- Follows .cursorrules?
- Edge cases?

**3. TEST**
- Compiles?
- Runs?
- Handles errors?

**4. VERIFY**
- Solves problem?
- Better approaches?

**5. FIX/REJECT**
- Fix yourself
- Or ask Cursor

</div>

---
layout: two-cols
class: text-sm
---

## Using Specific Context

::left::

### Be Specific:

❌ **Too Broad:**
```
"@codebase add filtering"
```

✅ **Specific:**
```
"@files handlers/task_handler.go 
@files services/task_service.go 

Add task filtering:
- Query params: status, priority
- Validate status enum
- Support combined filters
- Follow .cursorrules"
```

::right::

### For PRs:

```
"@git Review changes.

Check:
- Validation
- Edge cases
- Performance
```

---
layout: two-cols
class: text-sm
---

## Code Review with Cursor

::left::

### Self-Review Prompt:

```
"@git Review my changes for:

1. .cursorrules compliance
2. Edge cases missed
3. Performance issues
4. Security concerns
5. Code quality

Requirements: 
[paste criteria]"
```

::right::

### Generate PR Description:

```
"@git Create PR description:

- What was implemented
- Files changed
- Breaking changes
- Testing approach

Use conventional commits"
```

<br>

**Cursor reviews, YOU decide**

---
layout: center
class: text-center
---

# Lab 2: Task Filtering with Review Cycle
## 20 minutes

---

## Lab 2 - Objective

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
layout: default
class: text-sm
---

## Lab 2 - Part A: Define Requirements

### Acceptance Criteria:

**Feature: Task Filtering and Sorting**

1. **GET /tasks** supports query parameters:
   - `status`: pending/in_progress/completed
   - `priority`: 1-5
   - `sort_by`: created_at/priority/due_date
   - `order`: asc/desc

2. Multiple filters can be combined
3. Invalid status → 400 error
4. Invalid priority → 400 error
5. Default: `created_at desc`
6. Returns paginated results

---
layout: default
class: text-sm
---

## Lab 2 - Part B: Generate Code

### Step 1: Update Task Model

```
@files internal/models/task.go
Ensure: Status, Priority, DueDate fields + indexes
```

### Step 2: Service Layer

```
@files internal/services/task_service.go
Add filtering to GetTasks:
- Accept: status, priority, sortBy, order
- Validate filter values
- Build GORM query
- Return filtered & sorted tasks
```

### Step 3: Handler

```
@files internal/handlers/task_handler.go
Update GET /tasks:
- Parse query params
- Validate (status enum, priority range)
- Call service
- Return 400 for invalid params
```

---
layout: default
class: text-sm
---

## Lab 2 - Part C: Review & Test

### Review Against .cursorrules:

```
□ Follows architecture pattern? (handler → service)
□ Proper error handling with context?
□ Input validation present?
□ Uses parameterized queries?
□ Naming conventions followed?
□ No hardcoded values?
```

### Common Issues:

```go
// ❌ No validation
status := c.Query("status")

// ✅ Validate
if status != "" && !isValidStatus(status) {
    return c.JSON(400, gin.H{"error": "Invalid"})
}
```

```go
// ❌ SQL injection
db.Order(sortBy + " " + order)

// ✅ Safe whitelist
allowedSorts := map[string]bool{"created_at": true}
if !allowedSorts[sortBy] { sortBy = "created_at" }
```

---
layout: default
class: text-sm
---

## Lab 2 - Part D: Generate Tests

### Prompt:

```
@files tests/task_test.go 
Create tests for task filtering:

Test cases:
1. Filter by status - matching tasks only
2. Filter by priority - matching priority
3. Combined filters - both applied
4. Invalid status - 400 error
5. Invalid priority - 400 error
6. Sort by created_at asc - correct order
7. Sort by priority desc - correct order
8. No filters - all user tasks

Use table-driven tests, mock DB, testify
```

---
layout: two-cols
class: text-sm
---

## Lab 2 - Part E: Use Cursor for Review

::left::

### Self-Review:

```
"@git Review my implementation:

Acceptance Criteria:
[paste criteria]

Check:
1. Criteria met?
2. Edge cases?
3. .cursorrules?
4. Performance?
5. Security?"
```

::right::

### PR Description:

```
"@git Create PR description:

Include:
- What changed
- Why
- Testing approach
- Notes for reviewer"
```

---

## Lab 2 - Debrief

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
layout: center
class: text-center
---

# MCP (Model Context Protocol)

---

## What is MCP?

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
layout: two-cols
class: text-sm
---

## ⚠️ MCP Security Dangers

::left::

### Critical Risks:

**MCP can:**
- ✓ Read all data (sensitive!)
- ✓ Modify records
- ✓ Delete data
- ✓ Drop tables

**AI accidents:**
- ❌ "Clean test data" → Deletes users
- ❌ "Optimize" → Drops index

::right::

**Credential Exposure:**

```json
// ❌ BAD
{"env": {
  "DB_PASSWORD": "secret123"
}}

// ✅ GOOD
{"env": {
  "POSTGRES_CONNECTION": "${DATABASE_URL}"
}}
```

---
layout: two-cols
class: text-sm
---

## MCP Security Rules

::left::

### 🛡️ Always:

- □ Use READ-ONLY database user
- □ Never use production credentials
- □ Audit MCP source code first
- □ Official MCP servers only
- □ Least-privilege access
- □ No credentials in config
- □ Test in safe environment

::right::

### Setup Read-Only User:

```sql
CREATE USER cursor_readonly 
WITH PASSWORD 'safe';

GRANT CONNECT ON DATABASE taskdb 
TO cursor_readonly;

GRANT USAGE ON SCHEMA public 
TO cursor_readonly;

GRANT SELECT ON ALL TABLES 
IN SCHEMA public 
TO cursor_readonly;
```

---

## MCP Setup

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

## MCP Best Practices

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
layout: center
class: text-center
---

# Lab 3: Database with MCP
## 15 minutes

---

## Lab 3 - Objective

**Task:** Use MCP to analyze DB and create models

**What You'll Do:**
1. Set up read-only MCP
2. Inspect database schema
3. Generate GORM models from actual DB
4. Find missing indexes
5. Create migration

**Learn:** Safe AI-database interaction

---

## Lab 3 - Setup

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

## Lab 3 - Tasks

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

## Lab 3 - Review

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

## Lab 3 - Debrief

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
layout: center
class: text-center
---

# Critical Lessons

---
layout: two-cols
class: text-sm
---

## You Must Drive

::left::

### The Hierarchy:

1. **YOU** define architecture
2. **YOU** write requirements
3. **AI** generates code
4. **YOU** review everything
5. **YOU** fix issues
6. **YOU** are responsible

::right::

### Clear Prompts = Better Results

**Specify:**
- Technologies & versions
- Architecture patterns
- Error handling
- Naming conventions
- Security requirements
- Performance needs

---
layout: two-cols
class: text-sm
---

## The Bad Fix Problem

::left::

### What Happens:

1. You ask AI to implement X
2. AI tries approach A → fails
3. AI tries approach B → fails
4. AI does HACKY workaround → "works" but terrible

### How to Detect:

- Overly complex
- Multiple queries where one works
- Errors ignored
- Comments like "workaround"
- **Your instinct: "this feels wrong"**

::right::

### Example:

```go
// AI should do:
db.Preload("User").First(&task, id)

// AI got stuck, did this:
db.First(&task, id)  // ❌ Ignore
var user User
db.First(&user, task.UserID)  // ❌ N+1
task.User = user
return &task, nil  // ❌ Always nil
```

---
layout: two-cols
class: text-sm
---

## When AI Can Drive

::left::

### ✅ Low Risk:

**Documentation**
- README, API docs, comments
- Easy to review
- Can't break production

**POCs & Demos**
- Throw-away code
- Speed > quality

::right::

**One-time Scripts**
- Used once
- Reviewed before running

**Boilerplate**
- Project structure
- Initial models
- With thorough review!

---
layout: two-cols
class: text-sm
---

## When YOU Must Drive

::left::

### ⚠️ High Risk:

**Production Business Logic**
- Core features
- Complex algorithms
- Security-critical code
- YOU understand requirements

**Tests (Controversial!)**
- Tests evolve with code
- AI doesn't track context
- Maintaining AI tests is hard
- Better: YOU write, AI helps

::right::

**Architecture Decisions**
- Scaling
- Tech choices
- Patterns
- Long-term implications

**Database Schema**
- Schema affects everything
- Hard to rollback
- AI doesn't know your data

---
layout: two-cols
class: text-sm
---

## The Testing Reality

::left::

### Many Think:
> "AI can write all my tests!"

### Reality:

- ❌ Tests are NOT write-once
- ❌ Business logic changes → tests change
- ❌ AI-written tests you don't understand
- ❌ Maintenance = nightmare
- ❌ End up rewriting anyway

::right::

### Better Approach:

- ✅ YOU write test structure
- ✅ AI helps with test cases
- ✅ AI suggests edge cases
- ✅ YOU maintain ownership

<br>

> If it's hard to maintain when broken, **YOU drive**

---
layout: default
class: text-sm
---

## ⚠️ AI Tool Dependency Warning

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

## Final Wisdom

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
layout: center
class: text-center
---

# Q&A & Resources

---

## Common Questions

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

## Resources

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

## What to Remember

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

## Next Steps

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
layout: center
class: text-center
---

# Thank You!

## Questions?

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

