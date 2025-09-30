# .cursorrules

```
# Task Management API Standards

## Project Context
Task Management API built with Go, Gin, GORM, PostgreSQL

## Architecture
- Clean architecture: handlers → services → repositories
- Handlers: HTTP concerns only (parse input, return response)
- Services: Business logic, return domain errors
- Repositories: Database access

## Code Standards
- Error handling: Always check errors, log with context
- Naming: PascalCase (exported), camelCase (private)
- Database: Use GORM, parameterized queries only
- JSON tags: snake_case
- No hardcoded values - use environment variables or constants

## Testing
- Table-driven tests
- Test error cases, not just happy path
- Mock external dependencies
- Minimum 70% coverage

## Security
- Never log sensitive data (passwords, tokens, emails in logs)
- Validate all user input before processing
- Use environment variables for secrets
- Sanitize user content to prevent XSS
- Use parameterized queries to prevent SQL injection
- Implement proper authorization checks

## API Documentation
- Add Swagger/OpenAPI comments to all handlers
- Format: // @Summary, @Description, @Param, @Success, @Failure
- Keep swagger docs in sync with code
- Document all request/response models
- Include example values in model documentation

## Database
- Use GORM for all database operations
- Always use transactions for multiple operations
- Add indexes on frequently queried fields
- Use foreign key constraints
- Handle concurrent updates properly

## Error Handling
- Return consistent error format: {"error": "message"}
- Log errors with context (function name, operation, inputs)
- Use appropriate HTTP status codes:
  - 400: Bad Request (validation errors)
  - 401: Unauthorized
  - 403: Forbidden
  - 404: Not Found
  - 500: Internal Server Error
- Never expose internal errors to users

## When Unsure: ASK QUESTIONS
Format: "I need clarification on [topic]. Should I [option A] or [option B]?"

Examples:
- "Should user emails be case-sensitive or case-insensitive?"
- "Should deleting a task also delete its comments (cascade) or prevent deletion?"
- "Should the API support sorting? If yes, by which fields?"
```
