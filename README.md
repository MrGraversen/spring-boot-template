# Spring Boot service template

A small, batteries-included starting point for Java microservices.

## Using this template

After creating a repository from this template:

1. Update the project identity in `AGENTS.md` while leaving its template status
   as `UNINITIALIZED`.
2. Choose whether `Runtime execution` should remain `AGENT_ALLOWED` or be set
   to `USER_ONLY` for sensitive or externally connected projects.
3. Rename `apps/example-service` and update the module path in the root
   `pom.xml`, `docker/compose.yaml`, and the GitHub container-image matrix.
4. Replace the Maven coordinates and `io.graversen.exampleservice` Java
   package.
5. Update `spring.application.name`, the container image name, and the HTTP
   port.
6. Change the template status in `AGENTS.md` to `ACTIVE`.
7. Run the permitted verification commands for the selected runtime mode.

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
mvn -ntp verify
mvn -ntp --projects apps/example-service spring-boot:run
```

The application health endpoint is available at
`http://localhost:8080/actuator/health`.

## Run with Docker Compose

```shell
docker compose --file docker/compose.yaml up --build
```

The image is built directly from source using the shared `docker/Dockerfile`.
Each application selects its Maven module through the `APP_MODULE` build
argument. The Dockerfile uses a BuildKit Maven dependency cache and a small
Java runtime image.

## Run infrastructure for an IDE or command-line app

```shell
docker compose --file docker/compose.infrastructure.yaml up -d
```

This template currently has no required external infrastructure. Add only the
databases, brokers, or other dependencies needed by the initialized project to
this file; do not add application services to it. Expose the required local
ports and use simple development-only credentials. Give the Compose project,
services, networks, and volumes short, descriptive names; let Compose derive
container names unless a stable explicit name is required.

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
