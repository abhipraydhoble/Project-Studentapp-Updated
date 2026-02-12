🎓 StudentApplication – Full-Stack Project Using Docker

🧱 Architecture Overview

Database: MariaDB (AWS RDS)

Backend: Spring Boot (Dockerized)

Frontend: React + Nginx (Dockerized)

Deployment: Docker containers on EC2
---


## Database Setup (MariaDB on AWS RDS)

- Create MariaDB instance using AWS RDS.
- Connect to your RDS instance
<img width="1917" height="740" alt="image" src="https://github.com/user-attachments/assets/3e6a7bfa-f003-49a8-a24f-4043dbbd7763" />

```bash
sudo apt update
sudo apt install mysql-client -y
```
### Login To RDS
````
mysql -h <rds-endpoint> -u <db-username> -p<password>
````
<img width="1887" height="155" alt="image" src="https://github.com/user-attachments/assets/e95cc655-0b14-45ca-9923-a7f93d9f2342" />


### Create the database:

```sql
CREATE DATABASE student_db;
```
```
USE student_db;
```

### Create the students table:

```sql
CREATE TABLE `students` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `course` varchar(255) DEFAULT NULL,
  `student_class` varchar(255) DEFAULT NULL,
  `percentage` double DEFAULT NULL,
  `branch` varchar(255) DEFAULT NULL,
  `mobile_number` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=80 DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
```

### Exit MySQL:

```bash
exit
```
---

## Backend Setup (Spring Boot + Docker)

### Clone project repository:

```bash
git clone https://github.com/abhipraydhoble/studentapp-docker.git
```
### Navigate to the backend directory:

```bash
cd studentapp-docker/Backend
```

### Edit the application properties file:
  ````
  vim src/main/resources/application.properties
  ````
  - add database endpoint
  - username
  - password
<img width="1311" height="130" alt="image" src="https://github.com/user-attachments/assets/56aa4524-3617-4647-a8df-e3ceb659eb32" />

### Edit Backend Dockerfile
- add database endpoint
<img width="1417" height="572" alt="image" src="https://github.com/user-attachments/assets/ca753054-6240-46b6-b509-99f27b7e8ac7" />

```Dockerfile
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

ENV DB_URL=jdbc:mariadb://<rds-endpoint>:3306/student_db \
    DB_USERNAME=admin

ENTRYPOINT ["java", "-jar", "app.jar"]

```


### Now lets create an Backend Image

````
docker build -t backend .
````
### Run Container 
- note change DB_PASSWORD=YourPass

````
docker run -itd --name backend-cont  -e DB_PASSWORD=Passwd123$ -p 8080:8080 backend

````
### Check Running Container
````
docker ps
````
---

## Frontend Setup (React + Docker + Nginx)

### Navigate to the frontend directory:

```bash
cd ../Frontend
```
### Configure Backend API URL
````
vim src/api/userService.js
````
<img width="711" height="573" alt="image" src="https://github.com/user-attachments/assets/cbdc71f4-09e1-4431-94d0-7144cc1ed71e" />

### Edit Frontend Dockerfile
- replace your backend ip:
- **ENV REACT_APP_BACKEND_URL=http://13.60.38.103:8080**
  
```Dockerfile
# ---------- Stage 1: Build React App ----------
FROM node:18-slim AS build

# Set working directory
WORKDIR /app

# Copy package files first (better caching for Docker)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy rest of the project
COPY . .

# Build the React app for production
ENV REACT_APP_BACKEND_URL=http://13.60.38.103:8080
RUN npm run build


# ---------- Stage 2: Serve with Nginx ----------
FROM nginx:alpine

# Remove default nginx index.html
RUN rm -rf /usr/share/nginx/html/*

# Copy build files from first stage (dist instead of build)
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
````
### Build Docker Image
````
docker build -t frontend .
````
### Run Docker Container

````
docker run -itd --name frontend-cont -p 80:80 frontend
````

### Access the Application
**Open browser:
<img width="1907" height="967" alt="image" src="https://github.com/user-attachments/assets/639e9915-9443-4f5e-97d1-fee613bfa2da" />
- note: make sure required ports are allowed in security group
