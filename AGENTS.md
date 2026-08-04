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
- Help choose or replace the project identity and project profile values listed
  below.
- Update documentation related to initialization.
- Proceed when the user explicitly asks to initialize the template or
  explicitly authorizes work despite the uninitialized status.

The user must explicitly confirm the project identity and set both project
profile values to `true` or `false` before coding may begin. The AI must not
infer these values.

Initialization is complete only when:

- All project identity values have been replaced consistently.
- No unintended template placeholders or `example-service` references remain.
- Package paths, Maven coordinates, module names, container image names, and
  workflow configuration agree.
- Both project profile values have been explicitly set.
- The smallest relevant build or test verification passes.
- The template status is changed to `ACTIVE`.

### Project identity

- Service name: `example-service`
- Maven group ID: `io.graversen`
- Java package: `io.graversen.exampleservice`
- Container image: `example-service`
- HTTP port: `8080`

### Project profile

- Commercial: `UNSET`
- Business-critical: `UNSET`

## Project profile guidance

The values are independent. Use them to scale engineering effort to the
project's actual risk.

- Both `false`: Treat as a personal or hobby project. Prefer the simplest
  solution that works; avoid speculative abstractions, infrastructure, and
  ceremony.
- `Commercial: true`: Expect real users, data, security, and supportability,
  but keep the solution proportionate unless it is also business-critical.
- `Business-critical: true`: Apply stronger testing, resilience,
  observability, security, and recoverability regardless of commercial status.

Basic correctness, security, and data safety apply in every project.

## Engineering philosophy

Build the smallest complete solution that satisfies the current requirement.

Prefer clarity, conventional Spring solutions, and direct control flow over
speculative flexibility. Treat abstractions, dependencies, configuration,
infrastructure, retries, caching, asynchronous processing, and extra layers
as costs that require a current requirement or demonstrated risk.

Do not optimize for hypothetical future consumers. A diligent engineer makes
trade-offs explicit, preserves correctness, and leaves a coherent change.

## AI working method

- Inspect relevant code, configuration, tests, and existing patterns before editing.
- Clarify acceptance criteria, assumptions, and non-goals before material changes.
- Ask when ambiguity affects architecture, security, data, or external behavior.
  Otherwise, state a reasonable assumption and proceed.
- Keep changes focused and do not perform unrelated refactors.
- Verify the smallest meaningful scope, review the final diff, and report what
  changed, what was verified, and any remaining risks.

## Blockers and escalation

Use the project's intended mechanisms first. Do not pursue completion at any cost.

- Do not extract, copy, or patch classes from dependency JARs to bypass a missing
  or incompatible dependency.
- Do not introduce opaque one-off scripts, modify local dependency caches, patch
  generated artifacts, bypass Maven's normal lifecycle, or weaken checks merely
  to force progress.
- If a blocker cannot be resolved with a straightforward, supported change and
  it blocks progress, stop and ask the user.
- When escalating, report the error, relevant evidence, conventional approaches
  attempted, and the decision or permission needed.

## Repository layout

- `apps/`: deployable Spring Boot applications.
- `libraries/`: shared Java libraries.
- `integrations/`: integration adapters and clients.
- `docker/Dockerfile`: shared Dockerfile for every Java application.
- `docker/compose.yaml`: full local stack, including applications built from
  this repository.
- `docker/compose.infrastructure.yaml`: external infrastructure only, for
  applications run from IntelliJ or the command line.

## Commands

```shell
mvn --batch-mode -ntp verify
docker compose --file docker/compose.yaml up --build
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
- Keep `docker/compose.yaml` focused on the complete local development stack.
- Put databases, brokers, and other external dependencies in
  `docker/compose.infrastructure.yaml`; do not place application services in
  that file.
- Do not add a Maven Wrapper or Dependabot unless explicitly requested.
- Do not commit generated `target/` directories.

## Commit messages

- Prefer a gitmoji subject over Conventional Commit prefixes.
- Keep commit subjects concise and descriptive.
- Add detail lines only when they clarify important changes or decisions. Keep
  them brief and high-value; omit them when the subject is sufficient.

## Verification strategy

- Start with the smallest meaningful verification for the change. Run targeted
  unit tests for the changed behavior; do not run the entire Maven suite by
  default.
- Run `mvn verify` when the change has broad risk, such as a large refactor,
  reactor or build-configuration change, cross-module change, or before a
  release. Do not repeat an equivalent successful check without a new reason.
- Use `-ntp` for Maven commands unless artifact-transfer output is needed to
  diagnose a dependency download problem.
- Use Maven's `-T` option when independent reactor modules make parallel work
  likely to help. Do not add threads to a single-module or plugin-bound build
  merely by habit.
- Reuse incremental build output and dependency caches. Do not run `clean`
  unless stale generated output or a build problem specifically calls for it.
- Validate Docker and Compose proportionally: use `docker compose ... config`
  for Compose-only changes, and build an image when the Dockerfile, build
  context, or packaged application changes. Start containers only when runtime
  wiring or behavior needs verification.

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

- Prefer Lombok for mechanical boilerplate when it does not hide domain
  behavior or framework requirements.
- Prefer `@RequiredArgsConstructor` for constructor injection and immutable
  collaborators. Add `@NonNull` when a generated constructor should enforce a
  runtime null check.
- Use `@Getter` for deliberate read access and `@Slf4j` for class logging;
  do not write manual logger declarations or use field injection.
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

### Logging

- Use parameterized log messages; do not concatenate values into log strings.
- Never log passwords, tokens, secrets, or sensitive personal data.
- Keep logs useful and contextual without duplicating the same event at multiple
  layers.

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

### Formatting and review

- Use two spaces for Java indentation. Do not use tabs.
- Target a maximum line width of 120 columns. Do not optimize for a narrow
  editor viewport or treat a shorter limit as a reason to fragment otherwise
  readable code.
- Keep declarations, record components, method signatures, method calls, and
  exception construction on one line when the complete expression fits within
  120 columns and remains readable. In particular, do not wrap a line merely
  because a formatter prefers an earlier syntactic break.
- When an expression genuinely exceeds 120 columns or becomes difficult to
  scan, wrap it by logical unit. Prefer one argument, chained operation, or
  condition per continuation line rather than arbitrary breaks.

## API engineering

Treat HTTP APIs as durable contracts. Prefer predictable, conventional designs
over bespoke abstractions.

### Specification first

- The version-controlled OpenAPI specification is the source of truth. Change
  the specification before changing server or client behavior.
- Give every operation a unique, stable, descriptive `operationId`, normally a
  verb followed by the domain noun, such as `getOrder`, `listOrders`, or
  `createOrder`.
- Use the same domain language in paths, operation names, schemas, fields,
  Java code, clients, documentation, and user interfaces. Avoid synonyms and
  unexplained abbreviations.
- Define required fields, nullability, validation constraints, formats, status
  codes, security requirements, and error responses in the specification.
- Preserve backward compatibility. Make a breaking contract change only with
  an explicit versioning or migration decision.

### Generated server and clients

- Generate Spring Boot server interfaces and transport models from the OpenAPI
  specification. Implement generated interfaces or delegates; never generate
  business or domain logic.
- Generate React, TypeScript, and other API clients from the same specification.
  Prefer generated Feign clients for Java consumers.
- Never edit generated code manually. Pin the generator version and shared
  configuration so generation is deterministic and reproducible.
- Keep generated transport types at the API boundary. Map them to domain types
  instead of exposing entities or internal models through the contract.
- Wrap a generated client only to add domain ergonomics or cross-cutting
  behavior. Do not duplicate or fork its generated request and response types.
- A contract change is not complete until server generation, client generation,
  compilation, and relevant tests succeed.

### REST design

- Prefer resource-oriented, plural noun paths and use HTTP methods and status
  codes according to their standard semantics.
- Use path parameters for resource identity and query parameters for filtering,
  sorting, and pagination. Paginate collections that can grow without bound.
- Use custom action endpoints only when the domain operation does not fit a
  practical resource-oriented model. Do not force awkward abstractions merely
  to appear RESTful.
- Apply idempotency semantics deliberately and use one consistent error response
  model across the API.
- Choose the simplest contract that expresses the domain clearly and can evolve
  without surprising consumers.

### API model naming

- Every API-facing request, response, and shared schema type must end with
  `DTO`, for example `OrderDTO`, `CreateOrderRequestDTO`, or `ErrorDTO`.
- Apply the suffix once, consistently, either in schema names or through shared
  generator configuration. Do not hand-rename generated model classes.

## Spring Boot layering

- Use a simple package structure centered on `configuration`, `data`,
  `service`, and `util`. Add other packages only when they express a clear
  responsibility, such as `facade` or an API boundary.
- Organize `data` and `service` vertically by domain: for example,
  `data.product` contains product entities and repositories, while
  `service.product` contains product service-layer types.
- Keep JPA entities and Spring Data repositories in `data.<domain>`. Define
  entity indexes and unique constraints explicitly with JPA annotations.
- Keep service-layer behavior in `service.<domain>`. Annotate each service
  class with both `@Service` and class-level `@Transactional`.
- A service may depend on at most one repository. Keep each
  service-repository vertical autonomous.
- When an operation genuinely needs multiple services or repositories,
  coordinate it in a `facade` class. Do not turn one service into a general
  cross-domain orchestration layer.

### Identity and service operations

- Persist a technical primary key as `Long`, but use a stable UUID represented
  as `String` when domain objects reference or identify one another outside the
  persistence boundary. Do not expose or couple domain behavior to primary
  keys.
- A domain service should normally expose a small, consistent baseline: for
  `Product`, `createNew(CreateProduct)`, `get(String)`, `getAll()`,
  `getAll(Pageable)`, `deleteAll()`, and `delete(String)`.
- Model domain-specific changes as named command types and explicit methods.
  For example, change a product name with `update(UpdateProductName)`, rather
  than a generic setter or an ambiguous update payload.
- Keep API DTOs at the API boundary. Map them to service commands and domain
  types; service command types do not need a `DTO` suffix unless they are also
  API-facing models.

## Runtime configuration and optional integrations

- A Spring Boot application must run directly from IntelliJ and with the
  documented command-line command. Give ordinary properties sane local
  defaults; only secrets such as passwords and external API keys may require
  user-supplied values.
- Hide external integrations and other high-complexity behavior behind a small
  domain-facing interface. Provide an explicit mode or `enabled` property so a
  simple local, passthrough, or disabled implementation remains available.
- Use `@ConditionalOnProperty` to activate the implementation selected by the
  configuration. Keep the property name, supported values, default behavior,
  and fallback implementation clear in configuration and documentation.
- Apply this pattern where it has a practical payoff: for example AI models,
  CRM systems, authentication providers, and expensive processing pipelines.
  Do not add a strategy layer for straightforward application code.
- Keep configuration names shallow and domain-oriented. Avoid deeply nested
  property trees or configuration types that add ceremony without clarity.

## Template placeholders

When creating a repository from this template, replace the project identity
values above as appropriate for the new service, then set the template status
to `ACTIVE`.

## Definition of done

- The smallest relevant test or verification command passes; run `mvn verify`
  when the verification strategy requires it.
- Docker or Compose changes are validated with an image build or
  `docker compose --file docker/compose.yaml config`.
- Documentation and workflow matrices reflect any new application.
