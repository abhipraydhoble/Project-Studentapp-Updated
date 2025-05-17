### 2. Setup Backend (Spring Boot)

- Clone the repository:

```bash
git clone https://github.com/rajatpzade/cloudblitz-student-app.git
```

- Install Java 17:

```bash
sudo apt-get update
sudo apt install openjdk-17-jdk -y
```

- Install Maven:

```bash
sudo apt install maven -y
```

- Navigate to the backend directory:

```bash
cd cloudblitz-student-app/backend
```

- Configure database connection:

Edit the file:

```bash
vim src/main/resources/application.properties
```

Add/Update the following content:

```properties
server.port=8080
spring.datasource.url=jdbc:mariadb://<rds-endpoint>:3306/student_db
spring.datasource.username=<db-username>
spring.datasource.password=<db-password>
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

- Build the project:

```bash
mvn clean package
```

- Run the backend application:

```bash
java -jar target/student-registration-backend-0.0.1-SNAPSHOT.jar
```

The backend server will start at:
`http://<your-backend-server-ip>:8080`

---

