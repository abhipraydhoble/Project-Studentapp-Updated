# Project using docker 



# 1. Setup MariaDB 

- Create MariaDB instance using AWS RDS.
- Connect to your RDS instance f:

```bash
sudo apt update
sudo apt install mysql-client -y
```
**Login To RDS**
````
mysql -h <rds-endpoint> -u <db-username> -p<password>
````

- Create the database:

```sql
CREATE DATABASE student_db;
```
```
USE student_db;
```

- Create the students table:

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

- Exit MySQL:

```bash
exit
```

# 2. Setup Backend (Spring Boot)

- Clone the repository:

```bash
git clone https://github.com/abhipraydhoble/Project-Studentapp_Updated.git
```
- Navigate to the backend directory:

```bash
cd studentapp_updated/backend
```

#### edit
  ````
  vim src/main/resources/application.properties
  ````
  - add database endpoint
  - username
  - password

#### edit Dockerfile
- add database endpoint

```Dockerfile
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests

FROM eclipse-temurin:21-jdk


WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENV DB_URL=jdbc:mariadb://localhost:3306/student_db \
    DB_USERNAME=admin

CMD ["java", "-jar", "app.jar"]
```


### after apply docker build command 

````
docker build -t student-backend .
````
## now run backend container
- note change DB_PASSWORD=YourSecret

````
docker run -itd --name back-app  -e DB_PASSWORD=Navin#db -p 8080:8080 student-backend
````
## check running cont using
````
docker ps
````


# 3. Frontend

- Navigate to the frontend directory:

```bash
cd ../Frontend
```
### connect backend to frontend
- add backend IP into "src/api/userService.js"

## now back to frontend dir and edit dockerfile
- replace your backend ip ````ENV REACT_APP_BACKEND_URL=http://13.60.38.103:8080````
  
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
### build docker image
````
docker build -t student-frontend .
````
### run docker cont

````
docker run -itd --name studentapp -p 80:80 student-frontend
````

# 4. Copy Instance ip and Access Studentapp
<img width="1918" height="950" alt="image" src="https://github.com/user-attachments/assets/c58a3d59-d9c3-4901-a28a-652535c478dc" />
