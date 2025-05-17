FROM mariadb:latest
# Set environment variables for MariaDB
ENV MYSQL_ROOT_PASSWORD=rootpassword  
ENV MYSQL_DATABASE=student_db
ENV MYSQL_USER=admin
ENV MYSQL_PASSWORD=adminpassword

# Copy the initialization script (optional)
COPY init.sql /docker-entrypoint-initdb.d/

# Expose MariaDB port
EXPOSE 3306
