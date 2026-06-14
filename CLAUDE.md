# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Maven Spring Boot 3.4.x web application (Java 21) that serves a "Hello Claude!" message. Entry point is `ClaudePocApplication`; the app exposes a single REST endpoint.

## Commands

**Run the app:**
```
mvn spring-boot:run
```

**Build a fat JAR and run it:**
```
mvn package
java -jar target/claudepoc-0.0.1-SNAPSHOT.jar
```

**Run tests:**
```
mvn test
```

**Run a single test class:**
```
mvn test -Dtest=HelloControllerTest
```

## Endpoint

| Method | Path | Response        |
|--------|------|-----------------|
| GET    | `/`  | `Hello Claude!` |

App runs on `http://localhost:8080` by default (configured in `application.properties`).

## Architecture

```
src/main/java/com/example/claudepoc/
    ClaudePocApplication.java   — @SpringBootApplication entry point
    HelloController.java        — @RestController, maps GET /
src/main/resources/
    application.properties      — port and app name config
```

## Notes

- The old `src/Main.java` (bare Java 26 unnamed-class scratch file) is unrelated to the Maven build and can be ignored or deleted.
- Java source compatibility is set to 21 in `pom.xml`; the project compiles and runs correctly on JDK 26+.