
## 🚀 Setup Instructions

### 1. Setup MariaDB (Database Layer)

- Launch a MariaDB instance using AWS RDS.
- Connect to your RDS instance from a Linux terminal:

```bash
sudo apt update
sudo apt install mysql-client -y
mysql -h <rds-endpoint> -u <db-username> -p<password>
```

- Create the database:

```sql
CREATE DATABASE student_db;
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






