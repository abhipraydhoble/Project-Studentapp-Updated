# ---------- Build stage ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

# Copy pom first (better cache, but still safe)
COPY pom.xml .
RUN mvn dependency:resolve

# Copy source code
COPY src ./src

# Build fat jar
RUN mvn clean package -DskipTests

# ---------- Runtime stage ----------
FROM eclipse-temurin:17-jre

WORKDIR /app

# Copy EXACT jar name (NO wildcard)
COPY --from=builder /app/target/student-app.jar app.jar

EXPOSE 8080

# Environment variables (no localhost)
ENV DB_URL=jdbc:mariadb://database-1.c5g2e4cuwxef.ap-south-1.rds.amazonaws.com:3306/student_db \
    DB_USERNAME=admin

ENTRYPOINT ["java", "-jar", "app.jar"]
