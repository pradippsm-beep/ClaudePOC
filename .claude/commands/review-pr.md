Review the current branch changes as a pull request for this Spring Boot project.

## Step 1 — Understand the change

```bash
git log --oneline master...HEAD    # how many commits
git diff master...HEAD --stat      # which files changed and how much
git diff master...HEAD             # full diff
```

Read the diff top to bottom before forming any opinion. Note the stated intent
and verify the code actually does what it says.

---

## Step 2 — Correctness

- Does the logic do what it claims? Check for off-by-one errors, null dereferences,
  wrong status codes, and missing edge cases.
- Are REST endpoints returning the right HTTP status and response body?
  - `GET` → `200 OK`, `POST` (create) → `201 Created`, not found → `404`, etc.
- Are any new `@Service`, `@Repository`, or `@Component` beans wired correctly?
- Does changed business logic handle empty input, boundary values, and error paths?
- Are there race conditions or shared mutable state?

---

## Step 3 — Spring Boot conventions

- Controllers use `@RestController` / `@Controller` appropriately.
- Dependency injection uses **constructor injection** — not `@Autowired` field injection.
- `application.properties` changes are intentional and not exposing secrets.
- No hardcoded ports, passwords, credentials, or environment-specific URLs in source.
- `@Transactional` is applied at the service layer, not the controller.
- Exceptions are handled via `@ControllerAdvice` / `@ExceptionHandler`, not swallowed.
- No `System.out.println` — use SLF4J (`private static final Logger log = ...`).

---

## Step 4 — Testing

- Every new endpoint or changed behaviour has a corresponding test in `src/test/`.
- Use `@WebMvcTest` for controller-only tests, `@SpringBootTest` for integration tests.
- Tests assert on **status code**, **response body**, and **content type** — not just that no exception was thrown.
- No test is disabled (`@Disabled`, `@Ignore`) without a comment explaining why.
- Test names describe the scenario: `givenInvalidId_whenGetUser_thenReturns404`.
- No hardcoded sleeps (`Thread.sleep`) in tests — use `Awaitility` for async.

---

## Step 5 — Security

- No SQL injection risk — use JPA / named parameters, never string concatenation in queries.
- No XSS risk — if returning HTML, escape output; prefer JSON APIs.
- No sensitive data (passwords, tokens, PII) logged at any level.
- No secrets committed in `application.properties` — use environment variables or a secrets manager.
- Input validation present on all public endpoints (`@Valid`, `@NotNull`, `@Size`, etc.).
- Authentication / authorisation not accidentally bypassed for new routes.

---

## Step 6 — Code quality

- No dead code, unused imports, or unused variables left behind.
- Method and variable names follow Java conventions (camelCase; classes PascalCase).
- No commented-out code blocks — delete them, git history is the record.
- Methods are focused: ideally < 20 lines; single responsibility.
- No magic numbers or magic strings — use named constants or enums.
- Consistent formatting — matches the existing code style (indentation, braces).

---

## Step 7 — Runtime verification

Start the app and exercise the changed code path at the actual HTTP surface:

```bash
mvn spring-boot:run
```

Then for each changed endpoint:

```bash
curl -si http://localhost:8080/<path>
```

Probe edge cases:
- Wrong HTTP method (expect `405`)
- Missing required fields (expect `400`)
- Unknown routes (expect `404`)
- Valid happy path (expect correct body + status)

---

## Step 8 — Report findings

Group all findings by severity:

| Severity | Meaning |
|----------|---------|
| **Critical** | Must fix before merge — correctness bug, security hole, data loss risk |
| **Warning** | Should fix — missing tests, convention violation, maintainability concern |
| **Suggestion** | Nice to have — style, naming, minor improvement |

For each finding include:
- **File and line** where the issue is
- **What the problem is**
- **Suggested fix** (code snippet where helpful)

End with a clear verdict: **Approve / Request Changes / Needs Discussion**.
