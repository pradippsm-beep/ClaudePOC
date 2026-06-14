Review the current branch changes as a pull request for this Spring Boot project.

## What to check

**Correctness**
- Does the logic do what it claims? Look for off-by-one errors, null dereferences, incorrect status codes.
- Are REST endpoints returning the right HTTP status and response body?
- Are any new `@Service`, `@Repository`, or `@Component` beans wired correctly?

**Spring Boot conventions**
- Controllers use `@RestController` / `@Controller` appropriately.
- Dependency injection uses constructor injection (not field injection) where possible.
- `application.properties` changes are intentional and not exposing secrets.
- No hardcoded ports, passwords, or credentials in source files.

**Testing**
- New endpoints or logic should have a corresponding test in `src/test/`.
- Tests use `@SpringBootTest` or `@WebMvcTest` as appropriate.
- No test is disabled (`@Disabled`, `@Ignore`) without a comment explaining why.

**Security**
- No SQL injection risk (use parameterized queries / JPA).
- No XSS risk in responses.
- No sensitive data logged at INFO or DEBUG level.

**Code quality**
- No dead code or unused imports left behind.
- Method and variable names follow Java conventions (camelCase).
- No commented-out code blocks.

## How to run

```
git diff main...HEAD
```

Then for each changed file, read it fully and apply the checks above. Report findings grouped by severity: **Critical**, **Warning**, **Suggestion**.