# Cursor AI Workshop
## Building a Task Management App with Next.js

**90-Minute Technical Workshop**  
**Philosophy:** YOU drive, AI assists. You are responsible for what ships.

---

## 📁 Main Files

### 🎯 **PRESENTATION_SLIDES.md** (USE THIS!)
**48 slides | 90 minutes**

The workshop presentation - ready to deliver.

**What's Inside:**
- Introduction & Understanding Cursor (10 min)
- Setting Standards (.cursorrules) - NEW! (10 min)
- Lab 1: Initialize App + Swagger (15 min)
- Lab 2: Task Filtering (20 min)
- MCP Security & Setup (8 min)
- Lab 3: Database with MCP (15 min)
- Critical Lessons + AI Dependency Warning (7 min)
- Q&A (7 min)

---

### 📚 **PRESENTATION_OUTLINE.md** (Speaker Notes)

Complete workshop guide with:
- Step-by-step lab instructions
- Exact prompts to use
- Review checklists
- Common mistakes to catch
- Speaker notes & timing

---

### 📖 **Reference Files**

**PRESENTATION_SLIDES.md** - Original 100 slides (reference only)

---

## 🚀 Quick Start

### Running the Presentation

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Run the presentation in development mode:**
   ```bash
   npm run dev
   ```
   The presentation will be available at `http://localhost:3030`

3. **Build for production:**
   ```bash
   npm run build
   ```
   Output will be in the `dist/` directory

4. **Export as PDF:**
   ```bash
   npm run export
   ```

### Prerequisites
- Cursor installed
- Node.js 18+ installed
- pnpm installed (`npm install -g pnpm`)
- Docker & Docker Compose
- Basic TypeScript/React knowledge
- Node.js (for running the presentation)

### For Presenters
1. Run the presentation: `npm run dev`
2. Read `PRESENTATION_SLIDES.md`
3. Study `PRESENTATION_OUTLINE.md`
4. Practice labs 1-3
5. Prepare prompts & .cursorrules example

### For Participants
1. Follow `PRESENTATION_SLIDES.md`
2. Create .cursorrules FIRST
3. Review everything AI generates
4. Test your code

---

## 📊 Workshop Overview

### The Contract
```
🚗 YOU are the driver
🤖 AI is the copilot
📋 YOU set destination (.cursorrules)
🔍 YOU review the route
✅ YOU are responsible
💪 YOU stay sharp
```

### 3 Hands-On Labs

**Lab 1: Initialize App (15 min)**
- Create .cursorrules (YOUR standards)
- Generate Next.js 15 project
- Set up Prisma + Shadcn UI
- Build task CRUD with UI
- Add loading & error states

**Lab 2: Comments & Activity Tracking (20 min)**
- Define acceptance criteria
- Add comments with authorization
- Implement activity tracking
- Security review (XSS prevention)
- Self-review with Cursor
- Generate PR description

**Lab 3: Database with MCP (15 min)**
- Set up read-only PostgreSQL MCP
- Inspect database schema
- Generate Prisma models
- Find missing indexes

---

## ⚠️ Key Warnings

### AI Dependency (The 4 Stages)
1. "Wow, this is amazing!" 🤩
2. "Let me just ask Cursor..." 🤔
3. "I can't work without it" 😰
4. "Wait, how do I do this again?" 😱

**The Test:** "Could you work without Cursor for a week?"  
**If NO → You're too dependent!**

### When to Drive
**YOU Drive:** Production logic, tests, architecture, database schema  
**AI Can Drive:** Docs, POCs, one-time scripts, boilerplate (with review!)

---

## 📝 Key Concepts

### .cursorrules
File in project root that teaches AI YOUR standards:
- Architecture patterns
- Code conventions
- Security requirements
- Testing approach
- **Critical:** Create BEFORE generating code!

### MCP (Model Context Protocol)
Cursor plugins for databases/APIs/file systems.  
**Security:** Use read-only access, official servers only!

### Review Workflow
```
1. READ the code
2. ASK: Follows .cursorrules? Secure? Edge cases?
3. TEST it
4. VERIFY requirements
5. FIX or REJECT
```

---

## ⌨️ Cursor Essentials

**Shortcuts:**
```
Cmd/Ctrl + K    Inline edit
Cmd/Ctrl + L    Chat panel
Cmd/Ctrl + I    Composer (multi-file)
Tab             Accept
Esc             Reject
```

**Context Commands:**
```
@files          Specific files (recommended!)
@folder         Directory
@git            Git diffs
@codebase       Entire project (slow)
```

---

## ✅ Review Checklist

Before accepting AI code:
```
□ Understand what it does?
□ Follows .cursorrules?
□ Errors handled properly?
□ Edge cases covered?
□ Secure (no secrets, SQL injection)?
□ Efficient (no N+1 queries)?
□ Can you debug/maintain it?
```

---

## 🎯 The 8 Rules

1. Clear prompts with exact specs
2. Review EVERYTHING AI generates
3. Test thoroughly before shipping
4. Use .cursorrules for standards
5. MCP read-only is safer
6. Make AI ask questions when unclear
7. YOU drive critical code
8. Maintain your skills without AI

---

## 📚 Resources

**Official:**
- cursor.sh/docs
- modelcontextprotocol.io
- nextjs.org/docs
- prisma.io/docs
- ui.shadcn.com

**Community:**
- forum.cursor.sh
- discord.gg/cursor

---

## 🎓 What You'll Learn

1. Set standards FIRST with .cursorrules
2. Write effective prompts with context
3. Review AI code systematically
4. Know when to trust AI vs take control
5. Use MCP safely for database inspection
6. Avoid AI dependency
7. Apply professional code review workflow

---

## 💡 Final Thoughts

> "AI amplifies your productivity. The best developers know when to use AI and when to write code themselves."

**Remember:**
- .cursorrules FIRST
- Review against YOUR rules
- Don't become dependent
- YOU are responsible
- Stay sharp!

---

**Workshop Version:** 2.0  
**Duration:** 90 minutes | **Slides:** 48 | **Level:** Intermediate

**Happy coding! 🚀**