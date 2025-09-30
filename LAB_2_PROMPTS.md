# Lab 2 - Cursor Prompts

## Prompt 2.1: Add Task Comments Feature

```
@files prisma/schema.prisma
@files lib/actions/
@files lib/services/
@files lib/validations/
@files components/features/
@files app/(dashboard)/tasks/[id]/
@files .cursorrules

Implement a commenting system for tasks with Server Actions and UI.

Follow all standards defined in .cursorrules file.

Step 1: Update Prisma Schema (prisma/schema.prisma)

Add Comment model:
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

Update Task model - add relation:
```prisma
model Task {
  // ... existing fields
  comments    Comment[]
}
```

Run: pnpm prisma migrate dev --name add-comments

Step 2: Zod Validation (lib/validations/comment.ts)

Create commentSchema:
- authorName: required string, min 1 char
- authorEmail: required email format
- content: required, min 1, max 2000 chars
- Sanitize content to prevent XSS (remove HTML/script tags)

Step 3: Service Layer (lib/services/comment-service.ts)

Create CommentService with methods:
- getComments(taskId): fetch comments for task (ordered by createdAt ASC)
- createComment(taskId, data): create new comment
- updateComment(commentId, content, userEmail): update if authorized
- deleteComment(commentId, userEmail): delete if authorized
- checkAuthorization(commentId, userEmail): verify comment ownership

Step 4: Server Actions (lib/actions/comment-actions.ts)

1. addComment(taskId: string, formData: FormData)
   - Parse and validate with Zod
   - Sanitize content (use DOMPurify or regex to strip HTML)
   - Call CommentService.createComment()
   - revalidatePath(`/tasks/${taskId}`)
   - Return success or errors

2. getComments(taskId: string)
   - Call CommentService.getComments()
   - Return comments array ordered oldest first

3. updateComment(commentId: string, formData: FormData)
   - Get current user email from formData
   - Check authorization: comment.authorEmail === userEmail
   - If not authorized: return { success: false, error: 'Unauthorized' }
   - Update comment content
   - revalidatePath()
   - Return success or error

4. deleteComment(commentId: string, userEmail: string)
   - Check authorization
   - If not authorized: return error
   - Delete comment
   - revalidatePath()
   - Return success

Step 5: UI Components

Install Shadcn components:
pnpm dlx shadcn@latest add textarea avatar alert-dialog

1. Task Detail Page (app/(dashboard)/tasks/[id]/page.tsx)
   - Server Component
   - Fetch task with comments using getTaskById()
   - Display task details
   - Render CommentsList component
   - Render CommentForm component

2. Comments List (components/features/comments-list.tsx)
   - Client Component
   - Display comments chronologically (oldest first)
   - Each comment shows: author name, email, content, timestamp
   - Edit/Delete buttons only visible to comment author
   - Use Avatar for author initials

3. Comment Form (components/features/comment-form.tsx)
   - Client Component with Shadcn Form
   - Fields: authorName (input), authorEmail (input), content (textarea)
   - Zod validation
   - On submit: call addComment() server action
   - Clear form after success
   - Show toast notification

4. Comment Item (components/features/comment-item.tsx)
   - Display single comment
   - Edit mode: inline textarea
   - Delete: confirmation dialog ("Are you sure?")
   - Only show edit/delete if email matches current user
   - Format timestamp as relative time (e.g., "2 hours ago")

Step 6: Security Implementation

1. XSS Prevention:
```typescript
// lib/utils/sanitize.ts
export function sanitizeContent(content: string): string {
  // Remove HTML tags and script content
  return content
    .replace(/<script[^>]*>.*?<\/script>/gi, '')
    .replace(/<[^>]+>/g, '')
    .trim();
}
```

2. Authorization Check:
```typescript
// In server action
const comment = await prisma.comment.findUnique({ where: { id: commentId } });
if (comment.authorEmail !== userEmail) {
  return { success: false, error: 'Unauthorized' };
}
```

3. UI Authorization:
```typescript
// Only show edit/delete if user is author
{comment.authorEmail === currentUserEmail && (
  <div>
    <Button onClick={handleEdit}>Edit</Button>
    <Button onClick={handleDelete}>Delete</Button>
  </div>
)}
```

Requirements:
- Follow .cursorrules architecture
- Server Actions for all comment operations
- XSS prevention: sanitize all content
- Email validation with Zod
- Authorization: only author can edit/delete
- Return 403-equivalent error if unauthorized
- Proper error handling and toast notifications
- Client Components for forms/interactivity
- Server Components for data fetching

Security Checklist:
□ XSS prevention: sanitize content before save
□ SQL injection: Prisma handles this automatically
□ Email validation with Zod
□ Authorization checks in server actions
□ Input length validation (max 2000 chars)
□ UI hides edit/delete from non-authors

Deliverables:
- Comment model in Prisma
- Server actions for CRUD operations
- Comments displayed on task detail page
- Add/edit/delete functionality with authorization
- XSS prevention implemented
- Toast notifications for feedback
- Confirmation dialog for delete
```

---
