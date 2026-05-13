# CLAUDE.md

This is a playground for Spring error handling patterns.

The error handling classes are in package `com.example.demo.error`.

The package `com.example.demo.error` contains classes that are designed to exercise every error case.

## Stack

- **Spring Boot 4.0.6**, Java 25, Maven 3.9
- **Spring Web MVC** + **Spring Data JPA** + **Spring Validation** + **PostgreSQL**
- **Testcontainers** for integration tests (spins up a real `postgres:latest` container)

## Commands

```bash
# Build
./mvnw clean package

# Run (requires a PostgreSQL instance; see application.properties)
./mvnw spring-boot:run

# Run app via Testcontainers (no local Postgres needed)
./mvnw spring-boot:test-run

# Run all tests
./mvnw test
```

## Jackson 3 API (Spring Boot 4 ships Jackson 3.x under `tools.jackson.*`)

- Package: `tools.jackson.core.*` / `tools.jackson.databind.*` (not `com.fasterxml.jackson.*`)
- `JsonParseException` → `tools.jackson.core.exc.StreamReadException`
- `JsonMappingException` → `tools.jackson.databind.DatabindException`
- `JsonMappingException.Reference` → `tools.jackson.core.JacksonException.Reference`
- `Reference.getFieldName()` → `Reference.getPropertyName()`
- `JsonLocation` → `tools.jackson.core.TokenStreamLocation`

## Testing

All IT tests: `@SpringBootTest + @AutoConfigureMockMvc + @Import(TestcontainersConfiguration.class)`.

`@AutoConfigureMockMvc` is in `org.springframework.boot.webmvc.test.autoconfigure` (moved in Spring Boot 4).

Surefire picks up both `*Tests.java` and `*IT.java`, so `./mvnw test` runs everything. Docker must be available.
