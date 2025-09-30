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
## Building a Task Management App with Next.js

**Technical Workshop**



---

## This Workshop

**You will learn to:**
- **Set standards first** – Define your rules before coding
- **Use context & prompts** – Guide Cursor with clear input
- **Integrate AI** – Make Cursor part of your development workflow

**And not to:**
- ❌ Blindly accepting AI suggestions
- ❌ Copy-paste development


## The Golden Rule

#### You are the driver. AI is the copilot.


---

## Workshop Agenda
<br>

- Introducing Cursor
- Lab 1: Initialize Task Management APP
- Cursor Rules & Review Cycle
- Lab 2: Task Comments with Cursor Review Cycle
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
```
Fix authentication
```

✅ **Specific:**  
```
@files auth_handler.go  
fix JWT token validation to check expiration.  
Follow error handling in @files error_handler.go
```

---


## AI can be a powerful tool, but it comes with risks:
<br>

- May misunderstand your intent or requirements
- May use outdated/deprecated libraries
- Security is not guaranteed (e.g., SQL injection, XSS)
- Best practices? Sometimes, but not always
- Can overcomplicate simple problems
- Might ignore your project’s standards

---
layout: two-cols
---

## Task Management Full-Stack App

**Features:**
- Create, update, delete tasks
- Task comments & collaboration
- Activity tracking & audit log
- Modern, responsive UI


::right::

<br>
<br>

**Tech Stack:**
- Next.js 15 + App Router
- TypeScript + Shadcn UI + Tailwind
- Prisma ORM + PostgreSQL
- Server Actions (no API routes)
- pnpm package manager



---
layout: center
class: text-center
---

# Lab 1: Initialize the project

---

## Part 2: Standards & Review
<br>

- Setting Standards (.cursorrules)
- The Review Process
- Using Cursor for Review

---

## What is .cursorrules?
<br>

**File in project root that teaches AI YOUR standards**

- Architecture patterns
- Naming conventions
- Security rules
- Error handling
- What to avoid

<br>

> **You define standards. AI follows them.**

---

## Creating .cursorrules
<br>

**Keep it simple (150-200 lines):**

- **Stack:** Next.js 15, TypeScript, Prisma, Shadcn
- **Naming:** PascalCase, camelCase, kebab-case
- **Architecture:** Server Components → Actions → Services → DB
- **Security:** Validate client + server, sanitize input
- **Never:** API routes, 'any' type, silent errors

<br>

```markdown
## Naming
- Components: PascalCase (TaskForm)
- Files: kebab-case (task-form.tsx)
- Functions: camelCase (createTask)
```

---

## The Review Process
<br>

**Every time AI generates code:**

1. **📖 READ** - Understand what it does
2. **❓ ASK** - Follows rules? Edge cases?
3. **🧪 TEST** - Does it work?
4. **✅ VERIFY** - Meets requirements?
5. **🔧 FIX** - Improve or reject

<br>

> **AI suggests. YOU decide.**


---

## Using Cursor for Review
<br>

**Self-review prompt:**

```
"@git Review my changes

Check against:
1. .cursorrules compliance
2. Task requirements
3. Security issues
4. Edge cases

List issues with severity"
```

<br>

> **Cursor reviews. YOU ship.**

---
layout: center
class: text-center
---

# Lab 2: Task Comments with Review Cycle


---

## What is MCP (Model Context Protocol)?

MCP is a middleware layer that lets Cursor AI securely access and interact with external resources like databases, APIs, and files by translating AI requests into safe, controlled tools.

<br>
```
Cursor AI ←→ MCP Server ←→ External Resource
             (middleware)      (PostgreSQL, APIs)
```
<br>



---

## MCP Security

<br>


### Risks:
- Reads/modifies/deletes data or files
- AI mistakes can drop tables, wipe data, or leak secrets

<br>


### Security Essentials


- Use **read-only** or least-privilege credentials
- Never use production secrets
- Audit config and source
- Test in safe environments

**Example: Read-Only DB User**

---
layout: center
class: text-center
---

# Lab 3: Create Database MCP


  

---

## Additional Cursor Tips & Tricks
<br>

- Spotting Bad AI Solutions
- When YOU Must Drive
- When AI Can Drive  
- Cursor Addiction Stages

---

## Spotting Bad AI Solutions
<br>


- ❌ Overly complex for simple task
- ❌ Errors being ignored
- ❌ Comments like "workaround"
- ❌ Your gut: This feels wrong

<br>

### What Happens:

1. You ask AI to implement X
2. AI tries approach A → fails
3. AI tries approach B → fails
4. AI does HACKY workaround → "works" but terrible


---

## When YOU Must Drive
<br>

**⚠️ High risk - YOU take control:**

- **Business logic** - Core features, complex algorithms
- **Architecture** - Scaling, tech stack, patterns
- **Database schema** - Hard to change later
- **Security code** - Auth, validation, permissions
- **Tests** - They evolve with your code


---

## When AI Can Drive
<br>

**✅ Low risk - Let AI help:**

- **Documentation** - README, comments, guides
- **Boilerplate** - Setup, config, structure
- **POCs/Demos** - Speed over perfection
- **Scripts** - One-time use, review first



---
layout: two-cols
---

## The Stages of Cursor Addiction (Don't Let This Be You!)


**Stage 1: "Wow, this is amazing!"**
- Legitimate productivity gains
- Still thinking for yourself

**Stage 2: "Let me just ask Cursor..."**
- Default for many tasks
- Still feels helpful

::right::
<br>
<br>
<br>

**Stage 3: "Let me just check with Cursor first..."**
- Always asking AI before thinking
- "Just one more prompt..."

**Stage 4: "I can't start without it"**
- Can't begin tasks without AI
- Submitting code you don't understand







---
layout: center
class: text-center
---

# Thank You!

## Questions?

