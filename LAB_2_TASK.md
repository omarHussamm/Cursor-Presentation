# Lab 2 Tasks

## Task 2.1: Add Task Comments Feature

### User Story
As a user, I want to add comments to tasks so that I can collaborate and discuss work items with my team.

### Description
Implement a commenting system where users can add, edit, and delete comments on tasks.

### Comment Model
```
- ID: UUID (primary key)
- TaskID: UUID (foreign key to tasks)
- AuthorEmail: string (required, valid email)
- Content: string (required, max 2000 chars)
- CreatedAt: timestamp
- UpdatedAt: timestamp
```

### API Endpoints

**Add Comment**
- `POST /api/v1/tasks/:id/comments`
- Body: `{ "author_email": "user@example.com", "content": "This is a comment" }`

**List Comments**
- `GET /api/v1/tasks/:id/comments`
- Returns comments in chronological order (oldest first)

**Update Comment**
- `PUT /api/v1/tasks/:task_id/comments/:comment_id`
- Body: `{ "content": "Updated comment" }`

**Delete Comment**
- `DELETE /api/v1/tasks/:task_id/comments/:comment_id`

### Acceptance Criteria

**Data Validation:**
- [ ] Author email is required and valid format
- [ ] Content is required and not empty
- [ ] Content max length 2000 characters
- [ ] HTML/script tags sanitized (XSS prevention)
- [ ] Invalid inputs return 400 with error message

**API Responses:**
- [ ] Create comment returns 201 with comment
- [ ] List returns 200 with array (ordered by created_at ASC)
- [ ] Update returns 200 or 404 if not found
- [ ] Delete returns 204 or 404 if not found
- [ ] Returns 404 if task doesn't exist

**Authorization:**
- [ ] Only comment author can update their comment (403 otherwise)
- [ ] Only comment author can delete their comment (403 otherwise)
- [ ] Author identified by email in request

**Database:**
- [ ] Foreign key constraint to tasks table
- [ ] Index on `task_id` for performance
- [ ] Cascade delete when task is deleted

**Code Quality:**
- [ ] Follows clean architecture pattern
- [ ] Input sanitization for XSS prevention
- [ ] Parameterized queries (no SQL injection)
- [ ] Proper error handling and logging

---

## Task 2.2: Add Activity Tracking

### User Story
As a user, I want to see a history of all changes made to a task so that I can track progress and audit changes.

### Description
Automatically track and log all task modifications (status changes, priority updates, assignee changes) for audit purposes.

### Activity Model
```
- ID: UUID (primary key)
- TaskID: UUID (foreign key to tasks)
- UserEmail: string (user who made the change)
- Action: string ["created", "status_changed", "priority_changed", "assigned"]
- FieldName: string (optional - which field changed)
- OldValue: string (optional)
- NewValue: string (optional)
- CreatedAt: timestamp
```

### API Endpoints

**Get Task Activity Log**
- `GET /api/v1/tasks/:id/activities`
- Returns activities in reverse chronological order (newest first)

### Acceptance Criteria

**Auto-Tracking:**
- [ ] Task creation generates "created" activity
- [ ] Status change generates "status_changed" activity with old/new values
- [ ] Priority change generates "priority_changed" activity with old/new values
- [ ] Assignee change generates "assigned" activity with old/new values
- [ ] Activities created automatically in service layer

**API Response:**
- [ ] Returns 200 with array of activities
- [ ] Ordered by created_at DESC (newest first)
- [ ] Returns 404 if task not found
- [ ] Empty array if no activities

**Database:**
- [ ] Foreign key constraint to tasks table
- [ ] Index on `task_id` and `created_at`
- [ ] Activities are immutable (no update/delete)

**Enhanced Task Response:**
- [ ] `GET /api/v1/tasks/:id` includes `comments_count` field
- [ ] `GET /api/v1/tasks/:id` includes `latest_activity` object

**Code Quality:**
- [ ] Activity creation integrated into update service
- [ ] Tracks previous values before update
- [ ] Handles concurrent updates gracefully
- [ ] Proper error handling if activity logging fails