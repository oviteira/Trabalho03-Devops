FROM gradle:8.6.1-jdk21-alpine AS build
WORKDIR /app
COPY build.gradle settings.gradle gradle.propertiers* ./
COPY gradle ./gradle
RUN gradle --no-daemon build -x test || true
COPY . .
RUN gradle --no-daemon bootJar

# Stage de runtime (apenas a linha COPY mudada)
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app

# copie APENAS o boot jar (que NÃO tem "-plain" no nome)
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]