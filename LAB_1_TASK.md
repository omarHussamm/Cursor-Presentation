# Lab 1 Tasks

## Task 1.1: Project Setup & Database Configuration

### User Story
As a developer, I need a properly structured Next.js full-stack project with PostgreSQL database so that I can build the task management features.

### Description
Initialize a Next.js 15 project following modern Next.js patterns with Docker-based PostgreSQL database and Prisma ORM.

### Technical Requirements
- Next.js 15 with App Router
- pnpm package manager
- TypeScript
- PostgreSQL database via Docker Compose
- Prisma ORM
- Environment-based configuration
- Modern architecture: components → server actions → services → database

### Acceptance Criteria
- [ ] Next.js 15 project initialized with TypeScript
- [ ] pnpm as package manager
- [ ] Project structure follows Next.js 15 conventions:
  ```
  app/
    ├── (dashboard)/      # Dashboard route group
    ├── layout.tsx
    └── page.tsx
  lib/
    ├── actions/          # Server actions
    ├── services/         # Business logic
    ├── db/              # Prisma client
    └── validations/     # Zod schemas
  prisma/
    └── schema.prisma
  components/
    ├── ui/              # Shadcn components
    └── features/        # Feature components
  .env.example
  docker-compose.yml
  ```
- [ ] Shadcn UI initialized with Tailwind CSS
- [ ] `docker-compose.yml` configured with PostgreSQL
- [ ] `.env.example` contains all required environment variables
- [ ] Prisma configured and connected to database
- [ ] `pnpm dev` command starts the application
- [ ] Application renders homepage at `/`

---

## Task 1.2: Implement Task CRUD with UI

### User Story
As a user, I need to create, view, update, and delete tasks through a web interface so that I can manage my work items.

### Description
Build task management feature with Server Actions, Prisma database layer, Zod validation, and React components with Shadcn UI.

### Task Model (Prisma Schema)
```prisma
model Task {
  id          String   @id @default(uuid())
  title       String   @db.VarChar(200)
  description String?  @db.Text
  status      TaskStatus @default(TODO)
  priority    Int      @default(3)
  assigneeName  String?
  assigneeEmail String?
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  @@index([status])
  @@index([priority])
}

enum TaskStatus {
  TODO
  IN_PROGRESS
  DONE
}
```

### Server Actions

**Create Task**
- `createTask(formData)` - Server action
- Zod validation for all fields
- Returns task or validation errors

**Get Tasks**
- `getTasks(page, pageSize)` - Server action
- Returns paginated tasks

**Update Task**
- `updateTask(id, formData)` - Server action
- Validates and updates task

**Delete Task**
- `deleteTask(id)` - Server action
- Deletes task and revalidates

### UI Components

**Task List Page** (`app/(dashboard)/tasks/page.tsx`)
- Display tasks in a simple table/card view
- Pagination controls
- Server Component fetching data

**Task Form** (`components/features/task-form.tsx`)
- Shadcn Form with React Hook Form
- Fields: title, description, status (select), priority (1-5), assignee name, assignee email
- Zod validation on client and server
- Optimistic updates

**Task Item** (`components/features/task-item.tsx`)
- Display task details
- Edit/Delete actions
- Status badge

### Acceptance Criteria

**Data Validation:**
- [ ] Zod schema for task validation
- [ ] Title is required and max 200 characters
- [ ] Status must be one of: TODO, IN_PROGRESS, DONE
- [ ] Priority must be between 1 and 5
- [ ] Email format validated if provided
- [ ] Client-side and server-side validation
- [ ] Error messages displayed in form

**Database Layer:**
- [ ] Prisma schema with Task model
- [ ] Database indexes on `status` and `priority`
- [ ] Type-safe database queries
- [ ] Handles database errors gracefully

**Server Actions:**
- [ ] All actions use `'use server'` directive
- [ ] Actions call service layer for business logic
- [ ] Revalidate paths after mutations
- [ ] Return typed responses with error handling

**UI Components:**
- [ ] Task list displays all tasks with pagination
- [ ] Create task form with all fields
- [ ] Edit task inline or in dialog
- [ ] Delete task with confirmation
- [ ] Loading states with Suspense
- [ ] Error states displayed gracefully
- [ ] Responsive design with Tailwind

**Code Quality:**
- [ ] Server Components for data fetching
- [ ] Client Components only when needed (forms, interactivity)
- [ ] Business logic in service layer (`lib/services/`)
- [ ] Database access via Prisma client (`lib/db/`)
- [ ] All errors handled with try-catch
- [ ] No hardcoded values
- [ ] TypeScript strict mode

**Loading & Error States (Best Practice):**
- [ ] Loading states on form submissions (disabled buttons)
- [ ] Error boundaries for graceful error handling
- [ ] Toast notifications for user feedback
- [ ] Form validation errors displayed inline
