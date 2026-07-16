# Spring Boot service template

A small, batteries-included starting point for Java microservices.

## Using this template

After creating a repository from this template:

1. Update the project identity in `AGENTS.md` while leaving its template status
   as `UNINITIALIZED`.
2. Rename `apps/example-service` and update the module path in the root
   `pom.xml`, `compose.yaml`, and the GitHub container-image matrix.
3. Replace the Maven coordinates and `io.graversen.exampleservice` Java
   package.
4. Update `spring.application.name`, the container image name, and the HTTP
   port.
5. Change the template status in `AGENTS.md` to `ACTIVE`.
6. Run `mvn verify` and `docker compose up --build`.

## Baseline

- Java 25
- Spring Boot 4.1.0
- Maven multi-module build
- Multi-stage, non-root container image
- Docker Compose development stack
- Spring Boot Actuator health check

## Run locally

Requires Java 25 and Maven 3.6.3 or newer.

```shell
mvn verify
mvn --projects apps/example-service spring-boot:run
```

The application health endpoint is available at
`http://localhost:8080/actuator/health`.

## Run with Docker Compose

```shell
docker compose up --build
```

The image is built directly from source using the shared `docker/Dockerfile`.
Each application selects its Maven module through the `APP_MODULE` build
argument. The Dockerfile uses a BuildKit Maven dependency cache and a small
Java runtime image.

## Layout

```text
apps/           Deployable Spring Boot applications
integrations/   Integration adapters and clients
libraries/      Shared Java libraries
assets/         Repository-level assets
```

`apps/example-service` is deliberately named for its role instead of a Spring
Boot major version, so the module name remains useful across framework upgrades.

## Automation

- `CI` runs the Maven test suite for pull requests and pushes to `main` or
  `develop`.
- `Publish container images` publishes every application listed in its matrix
  to GHCR on `v*` tags or manual workflow runs.
