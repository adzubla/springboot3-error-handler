# CLAUDE.md

This is a playground for Spring error handling patterns.

The error handling classes are in package `com.example.demo.error`.

The package `com.example.demo.error` contains classes that are designed to exercise every error case.

## Stack

- **Spring Boot 3.5.14**, Java 25, Maven 3.9
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

## Jackson 2 API (Spring Boot 3 ships Jackson 2.x under `com.fasterxml.jackson.*`)

- Package: `com.fasterxml.jackson.core.*` / `com.fasterxml.jackson.databind.*`
- `StreamReadException` → `com.fasterxml.jackson.core.exc.StreamReadException`
- `JsonMappingException` → `com.fasterxml.jackson.databind.JsonMappingException`
- `JsonMappingException.Reference` — use `ref.getFieldName()` (renamed to `getPropertyName()` in Jackson 3)
- `InputCoercionException` is wrapped inside `JsonMappingException` when overflow occurs during POJO mapping — use a cause-chain walk to find it
- `JsonLocation` → `com.fasterxml.jackson.core.JsonLocation`

## Testing

All IT tests: `@SpringBootTest + @AutoConfigureMockMvc + @Import(TestcontainersConfiguration.class)`.

`@AutoConfigureMockMvc` is in `org.springframework.boot.test.autoconfigure.web.servlet` (moved to `org.springframework.boot.webmvc.test.autoconfigure` in Spring Boot 4).

Surefire picks up both `*Tests.java` and `*IT.java`, so `./mvnw test` runs everything. Docker must be available.
