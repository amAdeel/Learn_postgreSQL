# PostgreSQL and pgAdmin Docker Setup

This project demonstrates the setup of a PostgreSQL database and pgAdmin using Docker. It includes steps to create and manage a PostgreSQL container, configure a database, and connect it with pgAdmin for visualization.

---

## Prerequisites

1. Docker installed on your system.
2. Basic knowledge of PostgreSQL commands.
3. Internet access to pull images from Docker Hub.

---

## Setup Steps

### 1. Create a PostgreSQL Container

Use the following command to create a PostgreSQL container:

```bash
docker run --name my-postgres \
  -e POSTGRES_USER=adeel \
  -e POSTGRES_PASSWORD=1234 \
  -e POSTGRES_DB=mydatabase \
  -p 5432:5432 \
  -d postgres
Note: If the container name already exists, remove or rename the existing container:

bash
Copy code
docker rm my-postgres
```
### 2. Start the PostgreSQL Container

Start the container using:
```bash
docker start my-postgres
Verify the container is running:

bash
Copy code
docker ps
```

### 3. Access PostgreSQL Container

To interact with PostgreSQL from inside the container:

```bash
Copy code
docker exec -it my-postgres bash
Once inside, access the database using:
```

```bash
Copy code
psql -U adeel -d mydatabase
If the specified database doesn't exist, list existing databases with:

sql
Copy code
\list
```

### 4. Troubleshooting Common Errors
Database Not Found: Ensure the database name matches exactly or create it if missing.
Foreign Key Errors: Verify the referenced keys exist in the parent table.
Command Errors: Use \? in psql for command help.

### 5. pgAdmin Setup

To set up pgAdmin, run:

```bash
Copy code
docker run --name pgadmin-container \
  -e PGADMIN_DEFAULT_EMAIL=admin@example.com \
  -e PGADMIN_DEFAULT_PASSWORD=admin \
  -p 5050:80 \
  -d dpage/pgadmin4
Access pgAdmin in your browser at: http://localhost:5050
```

### 6. Network Configuration
To enable communication between pgAdmin and PostgreSQL:

Create a Docker network:

```bash
Copy code
docker network create pg-network
Connect the containers to the network:

bash
Copy code
docker network connect pg-network my-postgres
docker network connect pg-network pgadmin-container
Database Schema and Operations
Tables in the Database
books: Stores book information.
students: Stores student details.
borrowing: Tracks borrowing activities.
Example Queries:
List all tables:

sql
Copy code
\dt
Insert data into books:

sql
Copy code
INSERT INTO books (Title, Author, ISBN, Publisher, AvailableCopies, TotalCopies)
VALUES ('Chemistry', 'M. Adeel', '0987-15', 'Oxford', 10, 10);
Query joined data:

sql
Copy code
SELECT Students.Name, Books.Title, Borrowing.BorrowDate, Borrowing.DueDate
FROM Borrowing
JOIN Students ON Borrowing.StudentID = Students.StudentID
JOIN Books ON Borrowing.BookID = Books.BookID
WHERE Borrowing.ReturnDate IS NULL;
Common Errors and Solutions
Syntax Errors: Double-check SQL syntax for typos (e.g., REFERENCES instead of REFERNCES).
Data Constraints: Ensure unique constraints and foreign keys are respected.
Column Type Mismatch: Use explicit type casting where required.
Logs
To view container logs:

bash
Copy code
docker logs my-postgres
Cleanup
To stop and remove all containers:

bash
Copy code
docker stop my-postgres pgadmin-container
docker rm my-postgres pgadmin-container
To delete the Docker network:

bash
Copy code
docker network rm pg-network
References
Docker Documentation
PostgreSQL Documentation
pgAdmin Documentation

```



### Author
#### M. Adeel

