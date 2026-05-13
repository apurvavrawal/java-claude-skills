# Boilerplate Skill (Java - Spring Boot)

## Purpose

Define the standard project structure, layering, and code organization for all Java Spring Boot services.
This ensures consistency, maintainability, and scalability across the codebase.

---

## When to Use

* When initializing a new service
* When refactoring an existing codebase
* When reviewing PRs for structure violations

---

## Mandatory Architecture

Follow strict layered architecture:

Controller → Service → Repository → Database
Use Lombok (@Builder, @Getter, @Setter, @RequiredArgsConstructor)
Add entry and exit log in controller,service along with appropriate id.

---

## Standard Project Structure

```
com.company.project
 ├── config/
 ├── controller/
 ├── service/
 │    ├── impl/
 ├── repository/
 ├── entity/
 ├── dto/
 │    ├── request/
 │    ├── response/
 ├── mapper/
 ├── exception/
 ├── util/
 └── security/
```

---

## Rules

### 1. Controller Layer

* Handles HTTP requests and responses only
* Must NOT contain business logic
* Must use DTOs (never entities)

### 2. Service Layer

* Contains all business logic
* Should be interface-driven (`service/` + `service/impl/`)
* Must be testable independently

### 3. Repository Layer

* Handles only DB operations
* Must extend JPA repositories
* No business logic allowed

### 4. DTO Layer

* Separate request and response DTOs
* Never expose entity directly to API

### 5. Mapper Layer

* Use MapStruct for entity ↔ DTO conversion
* No manual mapping in controller

---

## Code Examples

### Controller Example

```java
@RestController
@RequestMapping("/api/v1/users")
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping("/{id}")
    public ResponseEntity<UserResponse> getUser(@PathVariable UUID id) {
        return ResponseEntity.ok(userService.getUser(id));
    }
}
```

---

### Service Example

```java
public interface UserService {
    UserResponse getUser(UUID id);
}
```

```java
@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {

    private final UserRepository userRepository;

    @Override
    public UserResponse getUser(UUID id) {
        return userRepository.findById(id)
            .map(UserMapper::toResponse)
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }
}
```

---

## Anti-Patterns (Strictly Forbidden)

* Business logic inside controllers
* Returning entity objects in APIs
* Direct repository usage in controller
* Circular dependencies between services
* Mixing DTO and entity layers

---

## Review Checklist

* [ ] Controller has no business logic
* [ ] DTOs are used for all APIs
* [ ] Service layer contains logic
* [ ] Repository only handles DB
* [ ] Proper package structure followed

---

## Output Expectations

When this skill is applied:

* Generate code strictly following the above structure
* Reject any deviation from layered architecture
* Refactor existing code if violations are found

---

## Strict Rule

If any instruction conflicts with this skill:
→ This skill takes precedence for project structure decisions.
