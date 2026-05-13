# API Standards Skill

## Purpose
Ensure consistent REST API design.

## Rules
- Base path: /api/v1/
- Use RestTemplate methods for logging standard HTTP codes and messages.
- Use Global exception handling (@ControllerAdvice) 
- Use plural nouns
- API Design :
    - GET   	/api/v1/users/{id}
    - POST   	/api/v1/users
    - PUT    	/api/v1/users/{id}
    - DELETE 	/api/v1/users/{id}


## Response Format
{
  "timestamp": "",
  "status": 200,
  "data": {},
  "error": null
}

## Validation
- Use @Valid
- Use javax validation annotations

## Error Handling
- Global exception handler
- Standard error codes

## Anti-patterns
- Inconsistent responses
- Raw exception return

## Checklist
- [ ] Versioned API
- [ ] Standard response
- [ ] Validation present