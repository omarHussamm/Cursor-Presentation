# Lab 1 - Cursor Prompts

## Prompt 1.1: Project Setup & Database Configuration

```
Create a Next.js 15 full-stack project for a Task Management application.

Technology Stack:
- Next.js 15 with App Router
- pnpm as package manager
- TypeScript (strict mode)
- Shadcn UI + Tailwind CSS
- PostgreSQL via Docker
- Prisma ORM

Project Structure:
app/
  ├── (dashboard)/        # Route group for dashboard
  │   └── tasks/         # Tasks pages
  ├── layout.tsx
  └── page.tsx           # Homepage
lib/
  ├── actions/           # Server actions
  ├── services/          # Business logic
  ├── db/               # Prisma client & utilities
  └── validations/      # Zod schemas
components/
  ├── ui/               # Shadcn UI components
  └── features/         # Feature-specific components
prisma/
  └── schema.prisma     # Database schema
.env.example
docker-compose.yml

Architecture Requirements:
- Use App Router with Server Components as default
- Server Actions for mutations (no API routes)
- Client Components only when needed (forms, interactivity)
- Architecture flow: components → server actions → services → database
- Services: business logic only
- Database access: Prisma client in lib/db/

Code Standards & Naming Conventions:
- TypeScript strict mode enabled, no 'any' types
- Use 'use server' directive for Server Actions
- Use 'use client' only when necessary (forms, interactivity)
- Async Server Components for data fetching

Naming:
- Components/Types: PascalCase (TaskForm, Task, CreateTaskInput)
- Functions/variables: camelCase (createTask, taskService, userId)
- Files: kebab-case (task-form.tsx, task-service.ts, task-actions.ts)
- Constants: UPPER_SNAKE_CASE (MAX_TITLE_LENGTH, DATABASE_URL)
- Prisma models: PascalCase (Task, Comment)
- Prisma fields: camelCase (createdAt, assigneeName)

Error Handling:
- Always use try-catch in Server Actions
- Return typed responses: { success: boolean, data?: T, error?: string }
- Never silent failures - always inform user or log
- No hardcoded values - use constants or env vars

Setup Steps:
1. Initialize Next.js 15 with TypeScript: npx create-next-app@latest --typescript --tailwind --app --use-pnpm
2. Install and initialize Shadcn UI: pnpm dlx shadcn@latest init
3. Install Prisma: pnpm add prisma @prisma/client
4. Initialize Prisma: pnpm prisma init
5. Create docker-compose.yml with PostgreSQL
6. Configure .env with DATABASE_URL

Deliverables:
- Complete Next.js 15 project structure
- Shadcn UI configured with Tailwind
- Docker Compose with PostgreSQL
- Prisma configured and connected
- pnpm dev starts the application
- Basic homepage renders at /
- .env.example with all required variables
```

---

## Prompt 1.2: Implement Task CRUD with UI

```
@files prisma/schema.prisma
@files lib/actions/
@files lib/services/
@files lib/validations/
@files components/features/
@files app/(dashboard)/tasks/

Implement complete Task CRUD functionality with Server Actions, Prisma, and UI.

Step 1: Prisma Schema (prisma/schema.prisma)

Create Task model:
- id: String @id @default(uuid())
- title: String @db.VarChar(200)
- description: String? @db.Text
- status: TaskStatus enum (TODO, IN_PROGRESS, DONE) @default(TODO)
- priority: Int @default(3)
- assigneeName: String?
- assigneeEmail: String?
- createdAt: DateTime @default(now())
- updatedAt: DateTime @updatedAt
- Add indexes on status and priority

Run: pnpm prisma migrate dev --name init

Step 2: Zod Validation (lib/validations/task.ts)

Create taskSchema with zod:
- title: required, min 1, max 200 chars
- description: optional string
- status: enum ['TODO', 'IN_PROGRESS', 'DONE']
- priority: number between 1-5
- assigneeName: optional string
- assigneeEmail: optional email validation

Step 3: Service Layer (lib/services/task-service.ts)

Create TaskService class with methods:
- getTasks(page, pageSize): fetch paginated tasks
- getTaskById(id): fetch single task
- createTask(data): create new task
- updateTask(id, data): update existing task
- deleteTask(id): delete task

Use Prisma client from lib/db/prisma.ts
Return typed responses with error handling

Step 4: Server Actions (lib/actions/task-actions.ts)

Create server actions with 'use server':

1. createTask(formData: FormData)
   - Parse and validate with Zod
   - Call TaskService.createTask()
   - revalidatePath('/tasks')
   - Return success or validation errors

2. getTasks(page = 1, pageSize = 20)
   - Call TaskService.getTasks()
   - Return tasks array

3. getTaskById(id: string)
   - Call TaskService.getTaskById()
   - Return task or null

4. updateTask(id: string, formData: FormData)
   - Parse and validate with Zod
   - Call TaskService.updateTask()
   - revalidatePath('/tasks')
   - Return updated task or errors

5. deleteTask(id: string)
   - Call TaskService.deleteTask()
   - revalidatePath('/tasks')
   - Return success

Step 5: UI Components

Install Shadcn components:
pnpm dlx shadcn@latest add button form input select textarea label card badge

Create components:

1. Task List Page (app/(dashboard)/tasks/page.tsx)
   - Server Component
   - Fetch tasks using getTasks()
   - Display in cards or table
   - Add pagination controls
   - "Create Task" button

2. Task Form (components/features/task-form.tsx)
   - Client Component ('use client')
   - Use Shadcn Form + React Hook Form + Zod
   - Fields: title (input), description (textarea), status (select), priority (select 1-5), assigneeName (input), assigneeEmail (input)
   - On submit: call createTask() server action
   - Show validation errors
   - Loading state on submit button

3. Task Item (components/features/task-item.tsx)
   - Display task with status badge
   - Edit and Delete buttons
   - Use Shadcn Dialog for edit form
   - Confirmation dialog for delete

Architecture Requirements:
- Server Components for data fetching (no client-side fetch)
- Server Actions for all mutations (no API routes)
- Client Components only for forms and interactivity
- Business logic in service layer (lib/services/)
- Database access via Prisma (lib/db/)
- Zod validation on both client AND server (never trust client)

Code Standards & Naming:
- All server actions use 'use server' directive
- TypeScript strict mode, no 'any' types
- Files: kebab-case (task-form.tsx, task-actions.ts)
- Functions/vars: camelCase (createTask, taskList)
- Components/Types: PascalCase (TaskForm, Task)
- Constants: UPPER_SNAKE_CASE (MAX_PRIORITY)

Validation & Security:
- Validate on client (UX) AND server (security)
- Use Zod schemas in lib/validations/
- Email validation with Zod .email()
- Input length limits (title max 200 chars)
- Sanitize user input if displaying HTML

Error Handling:
- All Server Actions: try-catch with typed returns
- Return: { success: boolean, data?: T, error?: string }
- Revalidate paths after mutations: revalidatePath('/tasks')
- Display errors to user (toast or form errors)
- Never silent failures
- Log errors with context: { action, error, timestamp }

Database (Prisma):
- Use UUIDs: @id @default(uuid())
- Add indexes on: foreign keys, status, priority
- Timestamps: createdAt, updatedAt
- Use select/include for specific fields only

UI/UX:
- Shadcn components only (no custom UI from scratch)
- Responsive design with Tailwind
- Loading states with Suspense boundaries
- Disable buttons during submission
- Error boundaries for graceful error handling
- Toast notifications for user feedback

Deliverables:
- Working task list page at /tasks
- Create, read, update, delete functionality
- Form validation with error messages
- Optimistic UI updates
- Proper loading and error states
```

