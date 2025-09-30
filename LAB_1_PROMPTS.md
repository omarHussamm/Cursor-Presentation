# Lab 1 - Cursor Prompts

## Prompt 1.1: Project Setup & Database Configuration

```
Create a Go project structure for a Task Management API.

Technology Stack:
- Go 1.21+ with Gin framework
- GORM for database
- PostgreSQL via Docker

Project Structure:
- cmd/api/main.go (entry point)
- internal/handlers/ (HTTP handlers)
- internal/services/ (business logic)
- internal/repositories/ (database access)
- internal/models/ (data models)
- internal/database/ (DB connection)
- config/ (configuration)
- .env.example (environment variables template)
- docker-compose.yml (PostgreSQL only)
- Makefile (with run, build, docker-up commands)

Architecture Requirements:
- Follow clean architecture: handlers → services → repositories
- Handlers: only HTTP concerns (parse input, return JSON response)
- Services: business logic only, return domain errors
- Repositories: database access only

Code Standards:
- Use Go modules (go.mod)
- PascalCase for exported functions/types
- camelCase for private functions/variables
- JSON tags in snake_case
- Environment variables for all configuration
- Proper error handling with context

Database:
- PostgreSQL connection from environment variables
- Connection pooling configured
- Health check endpoint: GET /health

Deliverables:
- Complete project structure
- Database connection established
- Main.go that starts the server on port 8080
- Makefile with: run, build, docker-up commands
- .env.example with all required variables
```

---

## Prompt 1.2: Implement Task CRUD API

```
@files internal/models/
@files internal/handlers/
@files internal/services/
@files internal/repositories/

Implement complete CRUD API for Task management.

Task Model (internal/models/task.go):
- ID: UUID (primary key)
- Title: string (required, max 200 chars)
- Description: string (optional)
- Status: string enum ["todo", "in_progress", "done"]
- Priority: integer (1-5, 5 is highest)
- AssigneeEmail: string (optional, valid email)
- CreatedAt: timestamp
- UpdatedAt: timestamp
- Add database indexes on status and priority fields

API Endpoints to implement:

1. POST /api/v1/tasks
   - Create new task
   - Validate: title required, status enum, priority 1-5, email format
   - Return 201 with created task or 400 with error

2. GET /api/v1/tasks
   - List all tasks with pagination
   - Query params: page (default 1), page_size (default 20)
   - Return 200 with array of tasks

3. GET /api/v1/tasks/:id
   - Get single task by ID
   - Return 200 with task or 404 if not found

4. PUT /api/v1/tasks/:id
   - Update task (all fields optional)
   - Validate same as create
   - Return 200 with updated task or 404

5. DELETE /api/v1/tasks/:id
   - Delete task
   - Return 204 on success or 404

Architecture Requirements:
- TaskHandler in internal/handlers/task_handler.go
  - Only HTTP concerns: parse request, call service, return response
  - Validate request data
  
- TaskService in internal/services/task_service.go
  - Business logic: validation, transformations
  - Return domain errors (not HTTP errors)
  
- TaskRepository in internal/repositories/task_repository.go
  - Database operations using GORM
  - Use parameterized queries only
  - Handle database errors

Code Standards:
- All errors must be handled and logged with context
- Input validation before processing
- Use GORM for all database operations
- No SQL injection (parameterized queries only)
- Return consistent error JSON: {"error": "message"}
- No hardcoded values

Error Handling:
- Invalid UUID: return 400
- Task not found: return 404  
- Validation errors: return 400 with specific message
- Database errors: log and return 500
```

---

## Prompt 1.3: Add Swagger Documentation

```
@files internal/handlers/task_handler.go

Add Swagger/OpenAPI documentation to all task endpoints.

Requirements:
1. Add swagger annotations to all handler functions
2. Document all request/response models
3. Include example values for all fields
4. Document all possible error responses (400, 404, 500)

Swagger Annotation Format:
- @Summary: Brief description
- @Description: Detailed explanation
- @Tags: Task Management
- @Accept: json
- @Produce: json
- @Param: For path/query/body parameters
- @Success: Success responses with model
- @Failure: Error responses
- @Router: Endpoint path and method

Setup Required:
- Install swag: github.com/swaggo/swag
- Install gin-swagger: github.com/swaggo/gin-swagger
- Add swagger handler to main.go at /swagger/*
- Generate docs command in Makefile: swag init -g cmd/api/main.go

Example for Create Task:
// @Summary Create a new task
// @Description Create a new task with title, description, status, priority
// @Tags Task Management
// @Accept json
// @Produce json
// @Param task body models.Task true "Task object"
// @Success 201 {object} models.Task
// @Failure 400 {object} map[string]string
// @Router /api/v1/tasks [post]

Deliverables:
- All endpoints documented with swagger comments
- Swagger UI accessible at /swagger/index.html
- All models documented with example values
- Can test all endpoints through Swagger UI
```
