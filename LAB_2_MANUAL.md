# Lab 2 Manual - Comments & Activity Tracking with Cursor Review

**Time:** ~25 minutes  
**Goal:** Add comments and activity tracking while learning the review cycle with .cursorrules

**Key Difference from Lab 1:** You'll create .cursorrules FIRST, then use it to guide and review your work.

---

## Step 1: YOU - Create .cursorrules File

**File:** `.cursorrules` (in project root)

**What to do:**

1. Create `.cursorrules` in the root of your project

2. Copy the contents from `CURSORRULES.md` (provided in workshop materials)

3. Review what's in it:
   - Stack definition
   - Naming conventions
   - Architecture patterns
   - Security rules
   - Validation requirements

**Why YOU do this:** You need to understand YOUR project standards. This file is YOUR contract with AI.

**Verify:** File exists at root level and Cursor can read it.

---

## Step 2: YOU - Update Prisma Schema for Comments

**File:** `prisma/schema.prisma`

**What to do:**

1. Add Comment model:

```prisma
model Comment {
  id          String   @id @default(uuid())
  taskId      String
  task        Task     @relation(fields: [taskId], references: [id], onDelete: Cascade)
  authorName  String
  authorEmail String
  content     String   @db.VarChar(2000)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@index([taskId])
}
```

2. Update Task model to add relation:

```prisma
model Task {
  // ... existing fields
  comments    Comment[]
}
```

3. Run migration:
```bash
pnpm prisma migrate dev --name add_comments
```

**Why YOU do this:** Schema design is critical. YOU must understand your data model.

---

## Step 3: ASK CURSOR - Create Comment Validation

**Why ask Cursor:** Can reference .cursorrules for patterns.

**Prompt to use:**

```
@files .cursorrules
@files lib/validations/task.ts

Create Zod validation schema for Comment in lib/validations/comment.ts

Follow .cursorrules standards.

Requirements:
- authorName: required string, min 1 char
- authorEmail: required, valid email format
- content: required string, min 1 char, max 2000 chars
- Sanitize content to prevent XSS (remove HTML/script tags)

Export sanitizeContent function and commentSchema.
Export TypeScript type: CommentInput
```

**What Cursor will generate:** `lib/validations/comment.ts`

**YOU review against .cursorrules:**
- ✅ Naming: camelCase for functions, PascalCase for types?
- ✅ XSS prevention implemented?
- ✅ Email validation with Zod .email()?
- ✅ Clear error messages?

---

## Step 4: ASK CURSOR - Create Comment Service

**Why ask Cursor:** CRUD operations similar to Task service.

**Prompt to use:**

```
@files .cursorrules
@files lib/services/task-service.ts
@files lib/db/prisma.ts

Create CommentService in lib/services/comment-service.ts

Follow .cursorrules architecture pattern (same as TaskService).

Methods needed:
- getComments(taskId): fetch all comments for a task, ordered by createdAt ASC
- createComment(taskId, data): create comment with sanitized content
- updateComment(commentId, content, userEmail): update only if author matches
- deleteComment(commentId, userEmail): delete only if author matches

Authorization:
- updateComment and deleteComment must check if comment.authorEmail === userEmail
- If not authorized, throw error with message 'Unauthorized'

Use TypeScript strict types, handle errors by throwing.
```

**What Cursor will generate:** `lib/services/comment-service.ts`

**YOU review against .cursorrules:**
- ✅ Architecture: pure business logic, no HTTP concerns?
- ✅ Authorization checks in update/delete?
- ✅ Sanitization called before saving?
- ✅ Errors thrown with clear messages?
- ✅ TypeScript strict types used?

---

## Step 5: ASK CURSOR - Create Comment Server Actions

**Why ask Cursor:** Similar pattern to task actions, but with authorization.

**Prompt to use:**

```
@files .cursorrules
@files lib/actions/task-actions.ts

Create comment Server Actions in lib/actions/comment-actions.ts

Follow .cursorrules Server Actions pattern.

Actions needed:
1. addComment(taskId, formData) 
   - Parse: authorName, authorEmail, content
   - Validate with commentSchema
   - Call commentService.createComment()
   - revalidatePath(`/tasks/${taskId}`)
   - Return typed response

2. getComments(taskId)
   - Call service
   - Return comments array

3. updateComment(commentId, formData)
   - Get userEmail from formData
   - Parse content only
   - Call service with authorization check
   - revalidatePath
   - Handle 'Unauthorized' error → return { success: false, error: 'Unauthorized' }

4. deleteComment(commentId, userEmail)
   - Call service with authorization
   - revalidatePath
   - Handle unauthorized error

Follow error handling pattern from .cursorrules.
```

**What Cursor will generate:** `lib/actions/comment-actions.ts`

**YOU review against .cursorrules:**
- ✅ 'use server' directive?
- ✅ Try-catch in all actions?
- ✅ Typed returns: { success, data?, error? }?
- ✅ revalidatePath() after mutations?
- ✅ Authorization errors handled?
- ✅ Zod validation before service calls?

---

## Step 6: ASK CURSOR - Create Comment Components

**Why ask Cursor:** UI code, but YOU review against .cursorrules.

**Prompt to use:**

```
@files .cursorrules
@files components/features/task-form.tsx

Create comment UI components following .cursorrules.

1. CommentForm (components/features/comment-form.tsx):
   - Client Component
   - Shadcn Form + React Hook Form + Zod
   - Fields: authorName, authorEmail, content (textarea)
   - Submit: call addComment() action
   - Props: taskId: string
   - Loading state, toast notifications
   - Follow .cursorrules UI/UX standards

2. CommentItem (components/features/comment-item.tsx):
   - Client Component
   - Props: comment, currentUserEmail (for auth check)
   - Display: author name, email, content, timestamp
   - Show Edit/Delete buttons only if comment.authorEmail === currentUserEmail
   - Edit: inline textarea with Save/Cancel
   - Delete: AlertDialog confirmation
   - Use Shadcn components (Card, Button, AlertDialog)

3. CommentsList (components/features/comments-list.tsx):
   - Client Component
   - Props: taskId, currentUserEmail
   - Fetch comments in useEffect or make it Server Component?
   - Display CommentItem for each comment
   - Show "No comments" if empty
```

**What Cursor will generate:** Three comment components

**YOU review against .cursorrules:**
- ✅ Client Components only (forms/interactivity)?
- ✅ 'use client' directive?
- ✅ Shadcn components only?
- ✅ Authorization UI (hide buttons for non-authors)?
- ✅ Loading states implemented?
- ✅ Toast notifications?
- ✅ Confirmation for destructive actions?

---

## Step 7: YOU - Add Comments to Task Detail Page

**File:** `app/(dashboard)/tasks/[id]/page.tsx`

**What to do:**

1. Create the file (Server Component):

```typescript
import { getTaskById } from '@/lib/actions/task-actions'
import { getComments } from '@/lib/actions/comment-actions'
import { notFound } from 'next/navigation'

export default async function TaskDetailPage({
  params,
}: {
  params: { id: string }
}) {
  const task = await getTaskById(params.id)
  
  if (!task) {
    notFound()
  }
  
  const comments = await getComments(params.id)
  
  return (
    <div className="container mx-auto py-8">
      {/* TODO: Display task details */}
      
      {/* TODO: Display CommentsList */}
      
      {/* TODO: Display CommentForm */}
    </div>
  )
}
```

**Why YOU do this:** Understanding Server Component data fetching and page structure.

---

## Step 8: ASK CURSOR - Complete Task Detail Page

**Prompt to use:**

```
@files .cursorrules
@files app/(dashboard)/tasks/[id]/page.tsx
@files components/features/comment-form.tsx
@files components/features/comments-list.tsx

Complete the TaskDetailPage.

Requirements:
- Keep as Server Component
- Display task details (title, description, status, priority, assignee)
- Render CommentsList with fetched comments
- Render CommentForm below comments
- Note: currentUserEmail can be hardcoded as "user@example.com" for demo
- Back button to /tasks
- Follow .cursorrules responsive design
```

**YOU review against .cursorrules:**
- ✅ Server Component for data fetching?
- ✅ Components properly imported?
- ✅ Responsive design with Tailwind?

---

## Step 9: YOU - Test Comments Feature

**What to do:**

1. Navigate to a task detail page

2. Test:
   - ✅ Add comment with your email
   - ✅ Add comment with different email
   - ✅ Try to edit YOUR comment (should work)
   - ✅ Try to edit SOMEONE ELSE'S comment (should fail)
   - ✅ Delete your comment with confirmation
   - ✅ Try XSS: add comment with `<script>alert('xss')</script>` (should be sanitized)
   - ✅ Validation: try empty content, invalid email

**Fix any issues before continuing.**

---

## Step 10: YOU - Add Activity Tracking Schema

**File:** `prisma/schema.prisma`

**What to do:**

1. Add ActivityAction enum and Activity model:

```prisma
enum ActivityAction {
  CREATED
  STATUS_CHANGED
  PRIORITY_CHANGED
  ASSIGNED
  UPDATED
}

model Activity {
  id        String         @id @default(uuid())
  taskId    String
  task      Task           @relation(fields: [taskId], references: [id], onDelete: Cascade)
  userName  String
  userEmail String
  action    ActivityAction
  fieldName String?
  oldValue  String?
  newValue  String?
  createdAt DateTime       @default(now())

  @@index([taskId, createdAt])
}
```

2. Update Task model:

```prisma
model Task {
  // ... existing fields
  activities  Activity[]
}
```

3. Run migration:
```bash
pnpm prisma migrate dev --name add_activities
```

---

## Step 11: ASK CURSOR - Create Activity Service

**Prompt to use:**

```
@files .cursorrules
@files lib/db/prisma.ts

Create ActivityService in lib/services/activity-service.ts

Follow .cursorrules patterns.

Methods:
1. logActivity(taskId, userName, userEmail, action, fieldName?, oldValue?, newValue?)
   - Create activity record
   - Handle errors gracefully (don't throw, just log)
   - Return void

2. compareAndLogChanges(taskId, oldTask, newTask, userInfo)
   - Compare old vs new task
   - If status changed: log STATUS_CHANGED with old/new values
   - If priority changed: log PRIORITY_CHANGED
   - If assigneeEmail changed: log ASSIGNED
   - Call logActivity for each change
   - userInfo: { name: string, email: string }

3. getActivities(taskId)
   - Fetch activities ordered by createdAt DESC (newest first)
   - Return activities array

Use TypeScript strict types.
```

**YOU review against .cursorrules:**
- ✅ Business logic only?
- ✅ Errors handled gracefully (logged, not thrown)?
- ✅ TypeScript types correct?

---

## Step 12: YOU - Integrate Activity Logging into Task Service

**File:** `lib/services/task-service.ts`

**What to do:**

1. Import activity service:
```typescript
import { activityService } from './activity-service'
```

2. Update `createTask` to log CREATED activity:

```typescript
async createTask(data: TaskInput, userInfo: { name: string, email: string }) {
  const task = await prisma.task.create({ data })
  
  // Log activity (don't await, don't fail if it errors)
  activityService.logActivity(
    task.id,
    userInfo.name,
    userInfo.email,
    'CREATED'
  ).catch(err => console.error('Activity logging failed:', err))
  
  return task
}
```

3. Update `updateTask` to compare and log changes:

```typescript
async updateTask(id: string, data: Partial<TaskInput>, userInfo: { name: string, email: string }) {
  const oldTask = await prisma.task.findUnique({ where: { id } })
  if (!oldTask) throw new Error('Task not found')
  
  const updatedTask = await prisma.task.update({ where: { id }, data })
  
  // Log changes (don't fail the update if logging fails)
  activityService.compareAndLogChanges(id, oldTask, updatedTask, userInfo)
    .catch(err => console.error('Activity logging failed:', err))
  
  return updatedTask
}
```

**Why YOU do this:** Critical to understand how to integrate activity tracking without breaking the main feature.

---

## Step 13: ASK CURSOR - Update Task Actions with User Info

**Prompt to use:**

```
@files .cursorrules
@files lib/actions/task-actions.ts

Update createTask and updateTask actions to accept user info.

Changes needed:
1. Add userName and userEmail to FormData parsing
2. Pass { name: userName, email: userEmail } to service methods
3. Keep same error handling pattern

For demo purposes, these can be hardcoded or from form fields.
```

**YOU review:**
- ✅ User info passed to service?
- ✅ Same error pattern maintained?

---

## Step 14: ASK CURSOR - Create Activity Timeline Component

**Prompt to use:**

```
@files .cursorrules

Create ActivityTimeline component in components/features/activity-timeline.tsx

Requirements:
- Client Component
- Props: activities (array)
- Display as vertical timeline
- Each activity shows:
  - Icon based on action type (use lucide-react icons)
  - Formatted description: "John Doe changed status from TODO to IN_PROGRESS"
  - Relative timestamp (use date-fns: "2 hours ago")
  - User name
- If empty: show "No activity yet"
- Use Shadcn components for styling
- Follow .cursorrules responsive design
```

**YOU review:**
- ✅ Client Component?
- ✅ Activity descriptions formatted correctly?
- ✅ Icons for different action types?
- ✅ Responsive design?

---

## Step 15: YOU - Add Activity Timeline to Task Detail

**File:** `app/(dashboard)/tasks/[id]/page.tsx`

**What to do:**

1. Import getActivities:
```typescript
import { getActivities } from '@/lib/actions/activity-actions'
```

2. Fetch activities:
```typescript
const activities = await getActivities(params.id)
```

3. Add ActivityTimeline component to the page

**Test:** Verify activities display when you update a task.

---

## Step 16: YOU - Review EVERYTHING Against .cursorrules

**Use Cursor for self-review:**

**Prompt to use:**

```
@git

Review all my Lab 2 changes against .cursorrules

Check:
1. Architecture compliance
   - Server Components for data fetching?
   - Client Components only for interactivity?
   - Server Actions for mutations?
   - Business logic in services?

2. Security
   - XSS prevention in comments?
   - Authorization checks on update/delete?
   - Email validation?
   - SQL injection prevented (Prisma handles this)?

3. Code quality
   - Naming conventions followed?
   - Error handling per .cursorrules?
   - TypeScript strict types?
   - No hardcoded values?

4. Edge cases
   - What if task deleted while comments exist? (cascade)
   - What if comment content is whitespace?
   - What if invalid email?
   - What if activity logging fails? (doesn't break task operations)

List issues found with severity (critical/major/minor) and specific file locations.
```

**YOU do:**
- Read Cursor's review
- Fix any critical/major issues
- Decide on minor issues

---

## Step 17: ASK CURSOR - Generate PR Description

**Prompt to use:**

```
@git

Create a pull request description for Lab 2 changes.

Include:
1. Summary - What features were added and why
2. Changes Made - Files added/modified with brief explanation
3. Technical Details - Prisma schema changes, new Server Actions, new components
4. Security - XSS prevention, authorization, validation
5. Testing - How to test the features, edge cases covered

Use conventional commits style. Keep concise but comprehensive.
```

**YOU review:** Read the PR description, adjust if needed.

---

## Lab 2 Complete! ✅

**What you built:**
- ✅ .cursorrules file (YOUR standards)
- ✅ Comment system with authorization
- ✅ XSS prevention in comments
- ✅ Activity tracking for task changes
- ✅ Activity timeline UI
- ✅ Complete review cycle with Cursor

**What you learned:**
- How to set standards with .cursorrules
- Security: XSS prevention and authorization
- Activity tracking without breaking features
- Using Cursor to review your own code
- The complete development workflow with AI assistance

**Key takeaway:** YOU drove the architecture, Cursor helped with implementation. YOU reviewed everything against YOUR standards.

---

## Final Check

**Before considering Lab 2 done:**

- [ ] Comments work (create, read, update, delete)
- [ ] Authorization works (can't edit others' comments)
- [ ] XSS prevented (HTML tags removed from comments)
- [ ] Activities logged on task create/update
- [ ] Activity timeline displays correctly
- [ ] All features reviewed against .cursorrules
- [ ] No critical security issues

**Well done!** 🎉

