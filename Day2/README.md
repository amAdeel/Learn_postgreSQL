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


//This command creates a custom Docker network called pg-network. By default, Docker containers are created in a network called bridge, but if you want containers (like postgres-container and pgadmin-container) to communicate with each other, you often create a custom network.

docker network connect pg-network my-postgres
// this command connect my-postgress to this network...

docker network connect pg-network pgadmin-container
// this connect pgadmin container to this container ,
// now they both comes in same network now they communicate with each other easily 

\dt

// show all the tables in datebase  

INSERT INTO books (Title, Author, ISBN, Publisher, AvailableCopies, TotalCopies)
VALUES ('Chemistry', 'M. Adeel', '0987-15', 'Oxford', 10, 10);
Query joined data:

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

## docker simple command that most important that i learn today.

### 1. docker ps
Docker ps mean showing all runing container . only running container not the stop Container

### 2. docker ps -a
Mean showing all the container which are present in docker which may or may not running showing all the container .

### 3. docker start container-name
start the container that allready exist .

### 4. docker exec -it container-name bash
this command execute the container and "-it" mean integrate with terminal mean open this docker terminal

### 5. psql -U user-name -d datebase-name
this command help us to enter a specific database system , "-U" mean user and "-D" mean datebase .

### 6. \l and \list 
check the list of datebase

### 7. exit  and ctrl+Q 
for exist from the current suitaion to back .

### 8. docker run --name postgres-container -e POSTGRES_USER=your_user -e POSTGRES_PASSWORD=your_password -e POSTGRES_DB=your_database -p 5432:5432 -d postgres
  if container is not already not exist and want to create new one then use this one . "-e" mean environment variable  "-p" is the port on which this container see on browser ,, and  5432:5432 first port is docker and 2nd for local system .

## want to see table and quries on PgAdmin this install 

 ### 9.  docker run --name pgadmin-container -e PGADMIN_DEFAULT_EMAIL=admin@example.com -e PGADMIN_DEFAULT_PASSWORD=admin -p 5050:80 -d dpage/pgadmin4
 
 this command run pgadmin and first create it's image  and same "-e" is here a envirment variable  and dpage is the image and pdadmin4 is the container that created .

 ### 10. Connect PostgreSQL to pgAdmin
##### 1. Open pgAdmin:

Go to http://localhost:5050 in your browser.
Log in with the email and password you set earlier (e.g., admin@example.com and admin).
##### 2. Add a New Server:

On the left side, right-click on "Servers" and choose "Create > Server."
##### 3.Fill in Server Details:

 1. General Tab:
Name: Give your server a name (e.g., PostgresDocker).
 2.  Connection Tab:
Host: Enter localhost (if PostgreSQL and pgAdmin are on the same machine) or the name of your PostgreSQL container if they are in Docker.
Port: Enter 5432 (the port you used for PostgreSQL).
Username: Enter the PostgreSQL username (e.g., your_user).
Password: Enter the PostgreSQL password (e.g., your_password).
##### 4. Save:

Click "Save," and pgAdmin will connect to your PostgreSQL database running in Docker.


### 11. Ensure Both are at same network
 ```bash
     docker network create pg-network
     docker network connect pg-network postgres-container
     docker network connect pg-network pgadmin-container
 ```

 by default docket network are briage and by easily they both communicate for each other we create a common network ("pg-network") and connect other two container with this network .

 ### 12. docker stop pgadmin-container
stop the container 

### Author
#### M. Adeel

