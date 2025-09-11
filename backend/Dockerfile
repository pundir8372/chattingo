# Stage 1: Build with Maven
FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Optional tests / static analysis
FROM eclipse-temurin:21 AS test
WORKDIR /app
COPY --from=build /app/target/*.jar ./app.jar

# Stage 3: Runtime with non-root user
FROM eclipse-temurin:21-jre-alpine AS runtime

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app
COPY --from=build /app/target/*.jar ./app.jar
RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]


