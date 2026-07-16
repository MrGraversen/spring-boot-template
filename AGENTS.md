# AGENTS.md

## Purpose

This repository is a template for Java 25 and Spring Boot 4 microservices.
Keep changes small, reusable, and suitable for repositories created from this
template.

## Template activation gate

**Template status: UNINITIALIZED**

While the status above is `UNINITIALIZED`, do not make application,
infrastructure, or workflow changes. Tell the user that the repository still
contains template defaults and ask them to initialize it, or offer to perform
the initialization with them.

Before initialization, agents may only:

- Inspect the repository without changing it.
- Help choose or replace the project identity values listed below.
- Update documentation related to initialization.
- Proceed when the user explicitly asks to initialize the template or
  explicitly authorizes work despite the uninitialized status.

After the project identity has been confirmed and applied, change the status to
`ACTIVE`.

### Project identity

- Service name: `example-service`
- Maven group ID: `io.graversen`
- Java package: `io.graversen.exampleservice`
- Container image: `example-service`
- HTTP port: `8080`

## Repository layout

- `apps/`: deployable Spring Boot applications.
- `libraries/`: shared Java libraries.
- `integrations/`: integration adapters and clients.
- `docker/Dockerfile`: shared Dockerfile for every Java application.
- `compose.yaml`: local development stack.

## Commands

```shell
mvn --batch-mode --no-transfer-progress verify
docker compose up --build
```

To build one application directly:

```shell
docker build --file docker/Dockerfile \
  --build-arg APP_MODULE=apps/example-service \
  --tag example-service:local .
```

## Conventions

- Use Java 25 and the Spring Boot version managed by the root `pom.xml`.
- Add every Maven module to the root reactor.
- Reuse `docker/Dockerfile`; select the application with `APP_MODULE`.
- Add deployable applications to the matrix in
  `.github/workflows/publish-images.yml`.
- Keep `compose.yaml` focused on local development.
- Do not add a Maven Wrapper or Dependabot unless explicitly requested.
- Do not commit generated `target/` directories.

## Template placeholders

When creating a repository from this template, replace the project identity
values above as appropriate for the new service, then set the template status
to `ACTIVE`.

## Definition of done

- `mvn verify` passes.
- Docker or Compose changes are validated with an image build or
  `docker compose config`.
- Documentation and workflow matrices reflect any new application.
