# Build stage
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# Runtime stage
FROM eclipse-temurin:17-jre
WORKDIR /app

COPY --from=builder /app/target/student-app.jar app.jar

EXPOSE 8080

ENV DB_URL=jdbc:mariadb://rds-endpoint:3306/student_db \
    DB_USERNAME=admin

ENTRYPOINT ["java", "-jar", "app.jar"]
