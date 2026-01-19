#
# Multi-stage build for Spring Boot (Gradle) app
#

FROM eclipse-temurin:21-jdk AS build
WORKDIR /app

# Copy only what we need for a reproducible Gradle build
COPY gradlew gradlew.bat build.gradle settings.gradle /app/
COPY gradle /app/gradle

# Copy sources
COPY src /app/src

# Build a runnable jar (skip tests in CI image build unless you have DB-independent tests)
RUN chmod +x /app/gradlew && /app/gradlew clean bootJar -x test

FROM eclipse-temurin:25-jre AS runtime
WORKDIR /app

ENV TZ=UTC
ENV JAVA_OPTS=""

# Create non-root user for security


COPY --from=build /app/build/libs/*.jar /app/app.jar

EXPOSE 8080


# Use sh -c so JAVA_OPTS is expanded
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar /app/app.jar"]
