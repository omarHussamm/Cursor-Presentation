# Lab 1 Manual - Task CRUD with UI

**Note:** Task 1.1 (Project Setup) is already complete. This manual covers Task 1.2.

---

## Task 1.2: Implement Task CRUD with UI

**Time:** ~20 minutes  
**Goal:** Build task management with Server Actions, Prisma, Zod validation, and UI components

**Important:** .cursorrules is NOT yet created. You'll create it in Lab 2.

---

## Step 1: YOU - Create Prisma Schema

**File:** `prisma/schema.prisma`

**What to do:**

1. Open `prisma/schema.prisma`

2. Add the Task model with TaskStatus enum:

```prisma
enum TaskStatus {
  TODO
  IN_PROGRESS
  DONE
}

model Task {
  id            String     @id @default(uuid())
  title         String     @db.VarChar(200)
  description   String?    @db.Text
  status        TaskStatus @default(TODO)
  priority      Int        @default(3)
  assigneeName  String?
  assigneeEmail String?
  createdAt     DateTime   @default(now())
  updatedAt     DateTime   @updatedAt

  @@index([status])
  @@index([priority])
}
```

3. Run the migration:
```bash
pnpm prisma migrate dev --name add_task_model
```

4. Generate Prisma client:
```bash
pnpm prisma generate
```

**Verify:** Check that migration ran successfully and client generated.

---

## Step 2: ASK CURSOR - Create Zod Validation Schema

**Why ask Cursor:** Repetitive typing, Cursor can generate Zod schema from Prisma model.

**Prompt to use:**

```
Create a Zod validation schema for the Task model in lib/validations/task.ts

Requirements:
- title: required string, min 1 char, max 200 chars
- description: optional string
- status: enum with values 'TODO', 'IN_PROGRESS', 'DONE'
- priority: number, must be integer between 1 and 5 (use z.coerce for FormData)
- assigneeName: optional string
- assigneeEmail: optional, must be valid email format or empty string

Also export the TypeScript type: export type TaskInput = z.infer<typeof taskSchema>
```

**What Cursor will generate:** `lib/validations/task.ts` file

**YOU review:**
- ✅ All fields have proper validation
- ✅ Email validation is correct
- ✅ Priority uses z.coerce.number() (FormData returns strings)
- ✅ Type is exported

---

## Step 3: YOU - Create Prisma Client Utility

**File:** `lib/db/prisma.ts`

**What to do:**

1. Create the file `lib/db/prisma.ts`

2. Add Prisma singleton pattern:

```typescript
import { PrismaClient } from '@prisma/client'

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined
}

export const prisma = globalForPrisma.prisma ?? new PrismaClient()

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

**Why:** Prevents multiple Prisma instances in development (hot reload).

---

## Step 4: ASK CURSOR - Create Task Service

**Why ask Cursor:** Boilerplate CRUD operations, Cursor can scaffold this quickly.

**Prompt to use:**

```
@files lib/db/prisma.ts

Create a TaskService in lib/services/task-service.ts

Requirements:
- Export an object with methods (not a class)
- Methods: getTasks(page, pageSize), getTaskById(id), createTask(data), updateTask(id, data), deleteTask(id)
- Use Prisma client from lib/db/prisma.ts
- getTasks: use skip/take for pagination, return { tasks, total }
- All methods should be async
- Handle errors by throwing them (let Server Actions handle)
- Use TypeScript strict types
- For createTask and updateTask, accept data matching TaskInput type from validations

Example structure:
export const taskService = {
  async getTasks(page: number = 1, pageSize: number = 20) {
    // implementation
  },
  // ... other methods
}
```

**What Cursor will generate:** `lib/services/task-service.ts`

**YOU review:**
- ✅ All CRUD methods present
- ✅ Using Prisma correctly
- ✅ Pagination logic correct (skip = (page - 1) * pageSize)
- ✅ Returns typed responses
- ✅ No try-catch (errors thrown, not caught)

---

## Step 5: YOU - Create Server Actions Structure

**File:** `lib/actions/task-actions.ts`

**What to do:**

1. Create the file with 'use server' directive:

```typescript
'use server'

import { revalidatePath } from 'next/cache'
import { taskSchema } from '@/lib/validations/task'
import { taskService } from '@/lib/services/task-service'
import { z } from 'zod'

// We'll add actions here
```

2. Create the basic structure for `createTask`:

```typescript
export async function createTask(formData: FormData) {
  try {
    // TODO: Parse formData with Zod
    // TODO: Call taskService.createTask()
    // TODO: Revalidate path
    // TODO: Return success response
  } catch (error) {
    // TODO: Handle Zod errors
    // TODO: Return error response
  }
}
```

**Why YOU do this:** Understanding Server Actions pattern is critical. You need to know this structure.

---

## Step 6: ASK CURSOR - Complete Server Actions

**Why ask Cursor:** Repetitive error handling pattern, similar structure for all actions.

**Prompt to use:**

```
@files lib/actions/task-actions.ts

Complete all Server Actions in this file.

For createTask:
- Parse FormData fields (title, description, status, priority, assigneeName, assigneeEmail) with taskSchema
- Call taskService.createTask(data)
- revalidatePath('/tasks')
- Return { success: true, data: task } on success
- If Zod error: return { success: false, error: error.errors }
- Other errors: return { success: false, error: 'Failed to create task' }

Also create these actions with same error handling pattern:
- getTasks(page = 1, pageSize = 20): call service, return tasks
- getTaskById(id: string): call service, return task or null
- updateTask(id: string, formData: FormData): parse with Zod, update, revalidate, return response
- deleteTask(id: string): delete task, revalidate, return { success: true }

All actions should:
- Use try-catch
- Return typed responses: { success: boolean, data?: T, error?: string }
- Revalidate '/tasks' after mutations
```

**What Cursor will generate:** Complete Server Actions

**YOU review:**
- ✅ All actions have 'use server' at top
- ✅ FormData parsing uses .get() method
- ✅ Zod validation happens before service call
- ✅ revalidatePath() called after mutations
- ✅ Error handling for Zod vs general errors
- ✅ Typed return objects

---

## Step 7: YOU - Install Shadcn Components

**What to do:**

1. Install the components you'll need:

```bash
pnpm dlx shadcn@latest add button form input select textarea label card badge toast alert-dialog
```

2. Verify they're in `components/ui/`

**Why YOU do this:** Quick command, you should know what components you need.

---

## Step 8: ASK CURSOR - Create Task Form Component

**Why ask Cursor:** Complex form with React Hook Form + Zod integration.

**Prompt to use:**

```
Create a TaskForm component in components/features/task-form.tsx

Requirements:
- Client Component ('use client')
- Use Shadcn Form with React Hook Form
- Use taskSchema from lib/validations/task for validation
- Form fields:
  - title: Input (required)
  - description: Textarea (optional)
  - status: Select with options TODO, IN_PROGRESS, DONE
  - priority: Select with options 1-5
  - assigneeName: Input (optional)
  - assigneeEmail: Input (optional, type email)
- On submit: call createTask() Server Action from lib/actions/task-actions
- Handle loading state (disable button, show loading text)
- Use toast for success/error notifications
- Reset form on success
- Display validation errors inline using Shadcn Form components

Props: onSuccess?: () => void (optional callback)
```

**What Cursor will generate:** `components/features/task-form.tsx`

**YOU review:**
- ✅ 'use client' directive at top
- ✅ React Hook Form integrated with Zod
- ✅ All fields present with correct types
- ✅ Loading state implemented
- ✅ Toast notifications on success/error
- ✅ Form resets after success
- ✅ Validation errors displayed

---

## Step 9: YOU - Create Task List Page Structure

**File:** `app/(dashboard)/tasks/page.tsx`

**What to do:**

1. Create the file (Server Component):

```typescript
import { getTasks } from '@/lib/actions/task-actions'

export default async function TasksPage() {
  const result = await getTasks(1, 20)
  
  return (
    <div className="container mx-auto py-8">
      <h1 className="text-3xl font-bold mb-6">Tasks</h1>
      
      {/* TODO: Add "Create Task" button/dialog */}
      
      {/* TODO: Display tasks */}
      
      {/* TODO: Add pagination */}
    </div>
  )
}
```

**Why YOU do this:** Understanding Server Component data fetching is essential.

---

## Step 10: ASK CURSOR - Complete Task List Page

**Why ask Cursor:** Repetitive UI code, can scaffold quickly.

**Prompt to use:**

```
@files app/(dashboard)/tasks/page.tsx
@files components/features/task-form.tsx

Complete the TasksPage component.

Requirements:
- Keep it as Server Component (async)
- Fetch tasks using getTasks()
- Display "Create Task" button that opens a Dialog with TaskForm
- Show tasks in Card components (use Shadcn Card)
- Each task card shows:
  - Title (bold)
  - Description (if exists)
  - Status badge (use Shadcn Badge, color-coded)
  - Priority (display as number or stars)
  - Assignee name/email (if exists)
  - Edit and Delete buttons
- Delete button: use AlertDialog for confirmation, calls deleteTask action
- Edit functionality: can be simple for now (just show in dialog)
- If no tasks: show "No tasks yet" message
- Add basic pagination controls (Previous/Next buttons)
- Responsive design with Tailwind

Note: Dialog needs to be a Client Component, create a separate TaskCreateDialog component
```

**What Cursor will generate:** Complete page with task list

**YOU review:**
- ✅ Server Component fetching data
- ✅ Tasks displayed in cards
- ✅ Create dialog works
- ✅ Delete confirmation works
- ✅ Status badges color-coded
- ✅ Responsive layout
- ✅ Loading states mentioned in best practices

---

## Step 11: YOU - Test Everything

**What to do:**

1. Start the dev server:
```bash
pnpm dev
```

2. Navigate to `/tasks`

3. Test each operation:
   - ✅ Create a task (valid data)
   - ✅ Create with validation errors (empty title, invalid email)
   - ✅ View tasks in list
   - ✅ Edit a task
   - ✅ Delete a task (with confirmation)

4. Check for:
   - ✅ Form resets after creation
   - ✅ Toast notifications appear
   - ✅ Loading states show during submission
   - ✅ Validation errors display inline
   - ✅ Page revalidates after mutations

**Fix any issues you find before moving to Lab 2.**

---

## Step 12: YOU - Quick Review Against Best Practices

**Checklist:**

### Architecture
- ✅ Server Components for data fetching?
- ✅ Client Components only for forms/interactivity?
- ✅ Server Actions for mutations?
- ✅ Business logic in service layer?
- ✅ Database access via Prisma?

### Validation
- ✅ Zod validation on client AND server?
- ✅ Validation errors displayed to user?
- ✅ Email format validated?

### Error Handling
- ✅ Try-catch in all Server Actions?
- ✅ Typed error responses?
- ✅ User-friendly error messages?

### Code Quality
- ✅ TypeScript strict mode, no 'any'?
- ✅ Files named with kebab-case?
- ✅ Functions/vars in camelCase?
- ✅ Components in PascalCase?

---

## Lab 1.2 Complete! ✅

**What you built:**
- ✅ Prisma schema with Task model
- ✅ Zod validation
- ✅ Task service (business logic)
- ✅ Server Actions (mutations)
- ✅ Task form with React Hook Form
- ✅ Task list page with CRUD functionality

**What you learned:**
- How to structure Next.js 15 projects
- Server Components vs Client Components
- Server Actions for mutations
- Zod validation on both sides
- Shadcn UI integration

**Next:** Move to Lab 2 where you'll create .cursorrules and add comments/activity tracking!

