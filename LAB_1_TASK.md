# Lab 1 Tasks

## Task 1.1: Project Setup & Database Configuration

### User Story
As a developer, I need a properly structured Go API project with PostgreSQL database so that I can build the task management features.

### Description
Initialize a Go project following clean architecture principles with Docker-based PostgreSQL database.

### Technical Requirements
- Go 1.21+ with Gin framework
- PostgreSQL database via Docker Compose
- Environment-based configuration
- Clean architecture structure: handlers → services → repositories

### Acceptance Criteria
- [ ] Go modules initialized (`go.mod`, `go.sum`)
- [ ] Project structure follows clean architecture:
  ```
  cmd/api/main.go
  internal/
    ├── handlers/
    ├── services/
    ├── repositories/
    ├── models/
    └── database/
  config/
  .env.example
  docker-compose.yml
  Makefile
  ```
- [ ] `docker-compose.yml` configured with PostgreSQL
- [ ] `.env.example` contains all required environment variables
- [ ] Database connection established and tested
- [ ] `make run` command starts the application
- [ ] Application responds with health check at `GET /health`

---

## Task 1.2: Implement Task CRUD API

### User Story
As a user, I need to create, view, update, and delete tasks so that I can manage my work items.

### Description
Build REST API endpoints for task management with proper validation and error handling.

### Task Model
```
- ID: UUID (primary key)
- Title: string (required, max 200 chars)
- Description: string (optional)
- Status: enum ["todo", "in_progress", "done"]
- Priority: integer (1-5, where 5 is highest)
- AssigneeEmail: string (optional, valid email)
- CreatedAt: timestamp
- UpdatedAt: timestamp
```

### API Endpoints

**Create Task**
- `POST /api/v1/tasks`
- Body: `{ "title": "string", "description": "string", "status": "todo", "priority": 3, "assignee_email": "user@example.com" }`

**List Tasks**
- `GET /api/v1/tasks?page=1&page_size=20`

**Get Task**
- `GET /api/v1/tasks/:id`

**Update Task**
- `PUT /api/v1/tasks/:id`
- Body: Same as create (all fields optional)

**Delete Task**
- `DELETE /api/v1/tasks/:id`

### Acceptance Criteria

**Data Validation:**
- [ ] Title is required and max 200 characters
- [ ] Status must be one of: "todo", "in_progress", "done"
- [ ] Priority must be between 1 and 5
- [ ] Email format validated if provided
- [ ] Invalid requests return 400 with clear error message

**API Responses:**
- [ ] Create returns 201 with created task
- [ ] List returns 200 with paginated array
- [ ] Get returns 200 with task or 404 if not found
- [ ] Update returns 200 with updated task or 404
- [ ] Delete returns 204 or 404

**Data Layer:**
- [ ] Database indexes on `status` and `priority`
- [ ] Uses parameterized queries (GORM)
- [ ] Proper foreign key constraints
- [ ] Handles database errors gracefully

**Code Quality:**
- [ ] Handlers only handle HTTP concerns
- [ ] Business logic in service layer
- [ ] Database access in repository layer
- [ ] All errors logged with context
- [ ] No hardcoded values

---

## Task 1.3: Add API Documentation

### User Story
As an API consumer, I need interactive API documentation so that I can understand and test the endpoints.

### Description
Add Swagger/OpenAPI documentation to all task management endpoints.

### Acceptance Criteria
- [ ] Swagger annotations on all endpoints
- [ ] All request/response models documented
- [ ] Example values provided for each field
- [ ] Error responses documented (400, 404, 500)
- [ ] Swagger UI accessible at `/swagger/index.html`
- [ ] Can test all endpoints through Swagger UI
- [ ] Documentation stays in sync with code