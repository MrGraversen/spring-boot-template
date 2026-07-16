# syntax=docker/dockerfile:1

ARG APP_MODULE=apps/example-service

FROM maven:3.9-eclipse-temurin-25-alpine AS build
ARG APP_MODULE
WORKDIR /workspace

COPY . .
RUN --mount=type=cache,target=/root/.m2 \
    mvn --batch-mode --no-transfer-progress \
        --projects "${APP_MODULE}" \
        --also-make \
        package \
        -DskipTests

FROM eclipse-temurin:25-jre-alpine AS runtime
ARG APP_MODULE

RUN addgroup -S -g 10001 app \
    && adduser -S -D -H -u 10001 -G app app

WORKDIR /application

COPY --from=build --chown=app:app \
    /workspace/${APP_MODULE}/target/*.jar \
    application.jar

USER 10001:10001
EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --start-period=20s --retries=3 \
    CMD wget --quiet --tries=1 --output-document=/dev/null http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["java", "-jar", "application.jar"]
