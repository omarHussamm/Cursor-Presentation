# Lab 2 Tasks

## Task 2.1: Add Task Comments Feature

### User Story
As a user, I want to add comments to tasks so that I can collaborate and discuss work items with my team.

### Description
Implement a commenting system with Server Actions, Prisma, and UI components where users can add, edit, and delete comments on tasks.

### Comment Model (Prisma Schema)
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

// Add to Task model:
model Task {
  // ... existing fields
  comments    Comment[]
}
```

### Server Actions

**Add Comment**
- `addComment(taskId, formData)` - Server action
- Validates author name, email, content
- Sanitizes content for XSS prevention
- Returns comment or validation errors

**Get Comments**
- `getComments(taskId)` - Server action
- Returns comments ordered by createdAt ASC (oldest first)

**Update Comment**
- `updateComment(commentId, formData)` - Server action
- Checks authorization (email match)
- Returns updated comment or error

**Delete Comment**
- `deleteComment(commentId, userEmail)` - Server action
- Checks authorization (email match)
- Deletes and revalidates

### UI Components

**Task Detail Page** (`app/(dashboard)/tasks/[id]/page.tsx`)
- Display task details
- Show comments list below task
- Add comment form

**Comments List** (`components/features/comments-list.tsx`)
- Display all comments for a task
- Show author name, email, timestamp
- Edit/Delete buttons (only for comment author)

**Comment Form** (`components/features/comment-form.tsx`)
- Shadcn Form with React Hook Form
- Fields: author name, author email, content (textarea)
- Zod validation
- Submit button with loading state

**Comment Item** (`components/features/comment-item.tsx`)
- Display single comment
- Edit mode inline
- Delete confirmation
- Author verification UI

### Acceptance Criteria

**Data Validation:**
- [ ] Zod schema for comment validation
- [ ] Author name is required
- [ ] Author email is required and valid format
- [ ] Content is required and not empty
- [ ] Content max length 2000 characters
- [ ] HTML/script tags sanitized (XSS prevention using DOMPurify or similar)
- [ ] Client-side and server-side validation
- [ ] Error messages displayed in form

**Authorization:**
- [ ] Only comment author can edit their comment
- [ ] Only comment author can delete their comment
- [ ] Author identified by email comparison
- [ ] UI hides edit/delete buttons for non-authors
- [ ] Server action validates authorization
- [ ] Error message if unauthorized

**Database:**
- [ ] Prisma schema with Comment model
- [ ] Foreign key constraint to Task
- [ ] Index on `taskId` for performance
- [ ] Cascade delete when task is deleted
- [ ] Type-safe queries with Prisma

**Server Actions:**
- [ ] All comment actions use `'use server'`
- [ ] Sanitize input before saving
- [ ] Revalidate path after mutations
- [ ] Return typed responses
- [ ] Proper error handling

**UI Components:**
- [ ] Comments displayed on task detail page
- [ ] Add comment form at bottom
- [ ] Comments ordered chronologically (oldest first)
- [ ] Edit comment inline
- [ ] Delete with confirmation dialog
- [ ] Loading states during actions
- [ ] Optimistic updates for better UX
- [ ] Responsive design

**Code Quality:**
- [ ] Server Components for data fetching
- [ ] Client Components for forms/interactivity
- [ ] Business logic in service layer
- [ ] XSS prevention implemented
- [ ] TypeScript strict types
- [ ] No hardcoded values

---
