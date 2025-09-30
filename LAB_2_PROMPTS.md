# Lab 2 - Cursor Prompts

## Prompt 2.1: Add Task Comments Feature

```
@files internal/models/
@files internal/handlers/
@files internal/services/
@files internal/repositories/
@files .cursorrules

Implement a commenting system for tasks.

Follow all standards defined in .cursorrules file.

Comment Model (internal/models/comment.go):
- ID: UUID (primary key)
- TaskID: UUID (foreign key to tasks)
- AuthorEmail: string (required, valid email)
- Content: string (required, max 2000 chars)
- CreatedAt: timestamp
- UpdatedAt: timestamp
- Database: foreign key to tasks, index on task_id, cascade delete

API Endpoints:

1. POST /api/v1/tasks/:id/comments
   - Body: {"author_email": "user@example.com", "content": "comment text"}
   - Validate: email format, content not empty, max 2000 chars
   - Sanitize content (prevent XSS - remove/escape HTML tags)
   - Return 201 with comment or 400/404

2. GET /api/v1/tasks/:id/comments
   - List all comments for task
   - Order by created_at ASC (oldest first)
   - Return 200 with array or 404 if task not found

3. PUT /api/v1/tasks/:task_id/comments/:comment_id
   - Body: {"content": "updated text"}
   - Authorization: only author can update (check author_email)
   - Return 200 or 403 (forbidden) or 404

4. DELETE /api/v1/tasks/:task_id/comments/:comment_id
   - Authorization: only author can delete
   - Return 204 or 403 or 404

Requirements:
- Follow .cursorrules architecture pattern
- CommentHandler → CommentService → CommentRepository
- Sanitize HTML/script tags in content (XSS prevention)
- Email validation using proper library
- Authorization check: compare author_email in comment with request
- Return 403 if user tries to modify someone else's comment
- Proper error handling and logging per .cursorrules
- Add swagger documentation per .cursorrules standards

Security Checklist:
□ XSS prevention: sanitize content
□ SQL injection: parameterized queries
□ Email validation
□ Authorization checks on update/delete
□ Input length validation
```

---

## Prompt 2.2: Add Activity Tracking

```
@files internal/models/
@files internal/services/task_service.go
@files internal/repositories/
@files .cursorrules

Implement automatic activity tracking for task changes.

Follow all standards in .cursorrules file.

Activity Model (internal/models/activity.go):
- ID: UUID (primary key)
- TaskID: UUID (foreign key to tasks)
- UserEmail: string (who made the change)
- Action: string ["created", "status_changed", "priority_changed", "assigned"]
- FieldName: string (optional - which field changed)
- OldValue: string (optional)
- NewValue: string (optional)
- CreatedAt: timestamp
- Database: foreign key to tasks, index on task_id and created_at

API Endpoint:

GET /api/v1/tasks/:id/activities
- List all activities for task
- Order by created_at DESC (newest first)
- Return 200 with array or 404 if task not found

Auto-Tracking Logic (update TaskService):
1. Before updating task, get current values
2. After update, compare old vs new values
3. Create activity records for each change:
   - Status changed: action="status_changed", old/new values
   - Priority changed: action="priority_changed", old/new values
   - Assignee changed: action="assigned", old/new values
4. When task created: action="created"

Enhanced Task Response:
- Modify GET /api/v1/tasks/:id to include:
  - "comments_count": integer
  - "latest_activity": activity object (optional, most recent)

Implementation Requirements:
- ActivityRepository for database operations
- Integrate into existing TaskService.UpdateTask method
- Activities are immutable (no update/delete endpoints)
- Handle case where multiple fields change in one update
- If activity logging fails, log error but don't fail the update
- Follow .cursorrules architecture and error handling
- Add swagger documentation per .cursorrules

Integration Points:
- TaskService.CreateTask → create "created" activity
- TaskService.UpdateTask → compare old/new, create change activities
- Use transactions if needed for consistency
```

---

## Prompt 2.3: Self-Review with Cursor

```
@git

Review all my changes for Lab 2 against .cursorrules standards.

Check the following:

1. Architecture Compliance
   □ Handlers only handle HTTP concerns?
   □ Business logic in services?
   □ Database access in repositories?
   □ Proper separation of concerns?

2. Security Review
   □ XSS prevention implemented in comments?
   □ SQL injection prevented (parameterized queries)?
   □ Email validation secure?
   □ Authorization checks present on update/delete?
   □ Sensitive data not logged?

3. Code Quality
   □ Error handling follows .cursorrules?
   □ Consistent error JSON format?
   □ Proper HTTP status codes?
   □ All errors logged with context?
   □ No hardcoded values?

4. Edge Cases
   □ What if task is deleted while comments exist?
   □ What if comment content is only whitespace?
   □ What if invalid email format?
   □ What if multiple fields change simultaneously?
   □ Concurrent updates handled?

5. Documentation
   □ Swagger annotations on all new endpoints?
   □ Request/response models documented?
   □ Error responses documented?

Provide:
1. List of issues found with severity (critical/major/minor)
2. Specific code locations with problems
3. Suggested fixes for each issue
```

---

## Prompt 2.4: Generate PR Description

```
@git

Create a professional pull request description for Lab 2 changes.

Include:

1. Summary
   - What features were added
   - Why these features are needed

2. Changes Made
   - List of files added/modified
   - Brief explanation of each change

3. Technical Details
   - Database schema changes
   - New API endpoints
   - Architecture decisions

4. Testing
   - How to test the new features
   - Edge cases covered

5. Security Considerations
   - XSS prevention
   - Authorization implemented
   - Input validation

6. Breaking Changes
   - Any API changes that affect existing clients
   - Migration steps if needed

Format using conventional commits style.
Use clear sections with headers.
Keep it concise but comprehensive.
```
