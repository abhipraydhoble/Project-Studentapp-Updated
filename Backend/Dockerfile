FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests

FROM openjdk:17-jdk-slim

WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENV DB_URL=jdbc:mariadb://localhost:3306/student_db \
    DB_USERNAME=admin

CMD ["java", "-jar", "app.jar"]



# after apply docker build command 

# docker build -t student-backend .
# docker run -e DB_PASSWORD=YourSecret -p 8080:8080 student-backend
