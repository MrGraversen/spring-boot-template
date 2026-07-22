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

## Java code quality

Readability is a hard requirement. Optimize code for the next person reading it,
not for line count. Repository formatting and quality checks are part of the
implementation, not a cleanup step at the end.

### Language and naming

- Write code in English unless the user explicitly requests another language.
  This includes identifiers, comments, Javadoc, log messages, exception
  messages, test names, and configuration keys.
- Localized user-facing copy may use another language when that language is an
  explicit product requirement. Keep localized copy out of identifiers and
  internal diagnostics.
- Use descriptive domain names. Avoid single-letter names except for
  conventional, tiny scopes such as numeric indexes or coordinates.
- Never shorten a variable name merely to save horizontal space. Use names that
  preserve the domain meaning at the point of use; write
  `Certificate certificate`, not `Certificate c`.
- Name abstractions after the responsibility they own, not the implementation
  technique they happen to use.

### Structure and control flow

- Use modern Java 25 syntax when it clarifies intent. Do not use a newer
  language feature merely to make code shorter or more clever.
- Always use braces for `if`, `else`, loops, `try`, and `catch` blocks. Put each
  statement on its own line. For example, do not write
  `if (id == null) return;`.
- Do not combine declarations, assignments, or unrelated operations on one
  line. Do not compress methods, constructors, enum declarations, or lambda
  bodies into a single line.
- Prefer straightforward control flow, early validation, and small cohesive
  methods. Avoid nested ternaries, dense fluent chains, and lambdas that hide
  significant business logic.
- Extract a well-named method or type when a block needs explanation. Comments
  should explain why a decision exists, not translate dense code into prose.
- Prefer `final var` for local variables when the initializer makes the type
  clear. Use an explicit type when it communicates important domain meaning or
  makes the code easier to understand.
- Keep side effects visible. Separate persistence, external calls,
  transformation, and rendering when combining them would obscure failure or
  transaction boundaries.
- Prefer the Java Stream API conventional coding in most cases, but extract a
  named method when it is clear that the code is not a simple `map` or `filter`.

### Nullability and Optional

- Treat nullability as an explicit API decision. Use Lombok's `@NonNull` on
  fields and parameters that must never be null, especially at constructor and
  service boundaries.
- Prefer `Optional<T>` as a return type when absence is an expected result.
  Return an empty collection instead of an optional collection.
- Do not use `Optional` as a method parameter. Prefer overloads, a request
  object, or separate methods that make the caller's intent explicit.
- Do not store `Optional` in JPA entities, DTO fields, or configuration
  properties. Keep persistence fields compatible with their framework and
  expose absence deliberately at the boundary.
- Never use `null` as a control-flow shortcut or return it where an empty value,
  `Optional`, or a specific result type communicates the outcome better.

### Lombok

- Use Lombok where it removes mechanical boilerplate without hiding domain
  behavior or framework requirements.
- Prefer `@RequiredArgsConstructor` for dependency injection and immutable
  collaborators. Add `@NonNull` when a generated constructor should enforce a
  runtime null check.
- Never use `@Data` on JPA entities. Do not generate entity-wide setters,
  `toString`, `equals`, or `hashCode` methods.
- Give JPA entities a protected no-argument constructor, normally with
  `@NoArgsConstructor(access = AccessLevel.PROTECTED)`.
- Keep entity state changes behind explicit domain methods. Generate only the
  getters that callers genuinely need, and never generate setters merely for
  convenience.
- Exclude lazy associations and large or sensitive fields from generated
  `toString`, equality, and hash-code logic. Prefer explicit implementations
  when entity identity semantics matter.
- Do not use Lombok builders to bypass invariants. Use a named factory or an
  explicit constructor when object creation has rules.

### Abstractions and temporary implementations

- Avoid surprising, improvised, or framework-fighting implementations. First
  look for the simple, conventional Java or Spring solution.
- A temporary implementation at an external boundary usually belongs behind a
  small named interface or adapter so it can be replaced without leaking
  temporary behavior through the application.
- Do not introduce an abstraction solely to hide confusing code. Simplify the
  code first, then extract a boundary when it has a clear responsibility or a
  plausible second implementation.
- Make fallbacks and temporary behavior explicit in names, configuration, and
  tests. Document why they exist and what will replace them.
- Do not leave dead paths, unused parameters, placeholder branches, or duplicate
  implementations after the real path is established.


## Template placeholders

When creating a repository from this template, replace the project identity
values above as appropriate for the new service, then set the template status
to `ACTIVE`.

## Definition of done

- `mvn verify` passes.
- Docker or Compose changes are validated with an image build or
  `docker compose config`.
- Documentation and workflow matrices reflect any new application.
