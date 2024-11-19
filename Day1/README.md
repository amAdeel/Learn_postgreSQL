# Postgres Setup in Docker
### create some basic table init and manipulate with this how to create how to get the data how to insert a member , how to delete a memeber , how to check capital and lower for not finding any dublicate init  , how to delete a table ,find member on name like (A%) , here down is the all the command code that i done here , All task is done in CMD .


Microsoft Windows [Version 10.0.22631.4460]
(c) Microsoft Corporation. All rights reserved.

C:\Users\AdeelRock>docker --version
Docker version 27.0.3, build 7d4bcd8

C:\Users\AdeelRock>docker run --name my-postgres -e POSTGRES_USER=adeel -e POSTGRES_PASSWORD=1234 -e POSTGRES_DB=mydatabase -p 5432:5432 -d postgres

Unable to find image 'postgres:latest' locally
latest: Pulling from library/postgres
2d429b9e73a6: Pull complete
e8429bf8c751: Pull complete
6f797120cf60: Pull complete
6d3d16629482: Pull complete
5eaadf8c0964: Pull complete
9f868a0e1b86: Pull complete
63e9527d3d4d: Pull complete
08b0635ec300: Pull complete
fae0bc27f45a: Pull complete
637539a522b5: Pull complete
a46125241e83: Pull complete
c58210073ea0: Pull complete
55a824e2a83b: Pull complete
bc5e9b7132d3: Pull complete
Digest: sha256:163763c8afd28cae69035ce84b12d8180179559c747c0701b3cad17818a0dbc5
Status: Downloaded newer image for postgres:latest
2332e83dae7e260bcc233caf82d774d06e54e3f00f0fb69a1b79c412f56b4583

C:\Users\AdeelRock>docker ps

CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                    NAMES
2332e83dae7e   postgres   "docker-entrypoint.s…"   52 seconds ago   Up 47 seconds   0.0.0.0:5432->5432/tcp   my-postgres

C:\Users\AdeelRock>docker exec -it my-postgres bash

root@2332e83dae7e:/# ls
bin  boot  dev  docker-entrypoint-initdb.d  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@2332e83dae7e:/# psql -U adeel -d mydatabase

psql (17.1 (Debian 17.1-1.pgdg120+1))
Type "help" for help.

mydatabase=# CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    class VARCHAR(50)
);

CREATE TABLE

mydatabase=# \dt

         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | students | table | adeel
(1 row)

mydatabase=# INSERT INTO students (name, age, class) VALUES ('Ali', 20, 'BSCS');

INSERT 0 1

mydatabase=# SELECT * FROM students;

 id | name | age | class
----+------+-----+-------
  1 | Ali  |  20 | BSCS
(1 row)

mydatabase=# INSERT INTO students (name,age,class) VALUE ('Adeel','22','BSCS')

mydatabase-# INSERT INTO students (name,age,class) VALUES ('Adeel', '22', 'BSCS')

mydatabase-# \dt

         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | students | table | adeel
(1 row)

mydatabase-# SELECT * FROM students

mydatabase-# SELECT * FROM students;

ERROR:  syntax error at or near "VALUE"
LINE 1: INSERT INTO students (name,age,class) VALUE ('Adeel','22','B...)
                                              ^
mydatabase=# SELECT * FROM students;

 id | name | age | class
----+------+-----+-------
  1 | Ali  |  20 | BSCS
(1 row)

mydatabase=# INSERT INTO students (name,age,class) VALUES ('Adeel', '22', 'BSCS')

mydatabase-# SELECT * FROM students;

ERROR:  syntax error at or near "SELECT"
LINE 2: SELECT * FROM students;
        ^
mydatabase=# INSERT INTO students (name,age,class) VALUES ('Adeel', '22', 'BSCS');

INSERT 0 1

mydatabase=# SELECT * FROM students;

 id | name  | age | class
----+-------+-----+-------
  1 | Ali   |  20 | BSCS
  2 | Adeel |  22 | BSCS
(2 rows)

mydatabase=# CREATE TABLE teachers ( id SERIAL PRIMARY KEY, name VARCHAR(100), age INT, class VARCHAR(100) );

CREATE TABLE

mydatabase=# \dt

         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | students | table | adeel
 public | teachers | table | adeel
(2 rows)

mydatabase=# INSERT INTO teachers (name,age,class) VALUES ('Naveed',22,'BSCS-01');

INSERT 0 1

mydatabase=# SELECT * FROM teachers

mydatabase-# SELECT * FROM teachers;

ERROR:  syntax error at or near "SELECT"
LINE 2: SELECT * FROM teachers;
        ^
mydatabase=# SELECT * FROM teachers;

 id |  name  | age |  class
----+--------+-----+---------
  1 | Naveed |  22 | BSCS-01
(1 row)

mydatabase=# SELECT * FROM students WHERE id=2;

 id | name  | age | class
----+-------+-----+-------
  2 | Adeel |  22 | BSCS
(1 row)

mydatabase=# \q

root@2332e83dae7e:/# ls

bin  boot  dev  docker-entrypoint-initdb.d  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@2332e83dae7e:/# docker exec -it my-postgres bash

bash: docker: command not found
root@2332e83dae7e:/# psql -U adeel -q database
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  database "database" does not exist

root@2332e83dae7e:/# paql -U adeel -q mydatabase

bash: paql: command not found

root@2332e83dae7e:/# psql -U adeel -q mydatabase

mydatabase=# /dt

mydatabase-# \dt

         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | students | table | adeel
 public | teachers | table | adeel
(2 rows)

mydatabase-# SELECT * FROM students

mydatabase-# SELECT * FROM students;

ERROR:  syntax error at or near "/"
LINE 1: /dt
        ^
mydatabase=# ^C

mydatabase=# SELECT * FROM students;

 id | name  | age | class
----+-------+-----+-------
  1 | Ali   |  20 | BSCS
  2 | Adeel |  22 | BSCS
(2 rows)

mydatabase=# SELECT * FROM students LIMIT 2;

 id | name  | age | class
----+-------+-----+-------
  1 | Ali   |  20 | BSCS
  2 | Adeel |  22 | BSCS
(2 rows)

mydatabase=# SELECT * FROM students LIMIT 1;

 id | name | age | class
----+------+-----+-------
  1 | Ali  |  20 | BSCS
(1 row)

mydatabase=# INSERT INTO students(name,age,class) VALUES ('naveed','25','BSIT');

mydatabase=# ^C

mydatabase=# INSERT INTO students (name,age,class) VALUES ('Naveed', '25', 'BSIT');

mydatabase=# SELECT * FROM students;

 id |  name  | age | class
----+--------+-----+-------
  1 | Ali    |  20 | BSCS
  2 | Adeel  |  22 | BSCS
  3 | naveed |  25 | BSIT
  4 | Naveed |  25 | BSIT
(4 rows)

mydatabase=# SELECT * FROM students LIKE 'A%';

ERROR:  syntax error at or near "LIKE"
LINE 1: SELECT * FROM students LIKE 'A%';
                               ^
mydatabase=# ^C

mydatabase=# SELECT * FROM students WHERE name LIKE 'A%';

 id | name  | age | class
----+-------+-----+-------
  1 | Ali   |  20 | BSCS
  2 | Adeel |  22 | BSCS
(2 rows)

mydatabase=# CREATE TABLE moniters (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE,
    age INT,
    class VARCHAR(50)
);

mydatabase=# \dt

         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | moniters | table | adeel
 public | students | table | adeel
 public | teachers | table | adeel
(3 rows)

mydatabase=# INSERT INTO moniters(name,age,class) VALUES ('Ahmad',20,'BSIT');
mydatabase=# SELECT * FROM moniters;
 id | name  | age | class
----+-------+-----+-------
  1 | Ahmad |  20 | BSIT
(1 row)

mydatabase=# INSERT INTO moniters(name,age,class) VALUES ('Ahmad',20,'BSIT');

ERROR:  duplicate key value violates unique constraint "moniters_name_key"
DETAIL:  Key (name)=(Ahmad) already exists.
mydatabase=# ^C
mydatabase=# INSERT INTO moniters(name,age,class) VALUES ('ahmad',20,'BSIT');

mydatabase=# SELECT * FROM moniters;

 id | name  | age | class
----+-------+-----+-------
  1 | Ahmad |  20 | BSIT
  3 | ahmad |  20 | BSIT
(2 rows)

mydatabase=# DROP TABLE moniters;

mydatabase=# \dt

         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | students | table | adeel
 public | teachers | table | adeel
(2 rows)

mydatabase=# CREATE TABLE moniters ( id SERIAL PRIMARY KEY, name VARCHAR(100) UNIQUE^C
mydatabase=# CREATE EXTENTION IF NOT EXIST citext;
ERROR:  syntax error at or near "EXTENTION"
LINE 1: CREATE EXTENTION IF NOT EXIST citext;
               ^
mydatabase=# CREATE EXTENSION IF NOT EXIST citext;
ERROR:  syntax error at or near "EXIST"
LINE 1: CREATE EXTENSION IF NOT EXIST citext;
                                ^
mydatabase=# CREATE EXTENSION IF NOT EXISTS citext;
mydatabase=# CREATE TABLE moniter ( id SERIAL PRIMARY KEY, name VARCHAR(100) CITEXT UNIQUE, class VARCHAR(100) );
ERROR:  syntax error at or near "CITEXT"
LINE 1: ...oniter ( id SERIAL PRIMARY KEY, name VARCHAR(100) CITEXT UNI...
                                                             ^
mydatabase=# ^C
mydatabase=# CREATE TABLE moniter ( id SERIAL PRIMARY KEY, name CITEXT UNIQUE, class VARCHAR(100) );
mydatabase=# \dt
         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | moniter  | table | adeel
 public | students | table | adeel
 public | teachers | table | adeel
(3 rows)

mydatabase=# INSERT INTO moniter(name,class) VALUES ('Adeel','BSCS');
mydatabase=# SELECT * FROM moniter;
 id | name  | class
----+-------+-------
  1 | Adeel | BSCS
(1 row)

mydatabase=# INSERT INTO moniter(name,class) VALUES ('adeel','BSCS');
ERROR:  duplicate key value violates unique constraint "moniter_name_key"
DETAIL:  Key (name)=(adeel) already exists.
mydatabase=# INSERT INTO moniter(name,class) VALUES ('aeel','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Adnan','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain','BSCS');
mydatabase=# SELECT * FROM moniter
mydatabase-# SELECT * FRON moniter;
ERROR:  syntax error at or near "SELECT"
LINE 2: SELECT * FRON moniter;
        ^
mydatabase=# ^C
mydatabase=# SELECT * FROM moniter;
 id |  name   | class
----+---------+-------
  1 | Adeel   | BSCS
  3 | aeel    | BSCS
  4 | Adnan   | BSCS
  5 | Husnain | BSCS
(4 rows)

mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain1','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain2','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain3','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain4','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain5','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain6','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain7','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain8','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain9','BSCS');
mydatabase=# INSERT INTO moniter(name,class) VALUES ('Husnain0','BSCS');
mydatabase=# SELECT * FROM moniter;
 id |   name   | class
----+----------+-------
  1 | Adeel    | BSCS
  3 | aeel     | BSCS
  4 | Adnan    | BSCS
  5 | Husnain  | BSCS
  6 | Husnain1 | BSCS
  7 | Husnain2 | BSCS
  8 | Husnain3 | BSCS
  9 | Husnain4 | BSCS
 10 | Husnain5 | BSCS
 11 | Husnain6 | BSCS
 12 | Husnain7 | BSCS
 13 | Husnain8 | BSCS
 14 | Husnain9 | BSCS
 15 | Husnain0 | BSCS
(14 rows)

mydatabase=# DELETE FROM moniter WHERE name='aeel';
mydatabase=# SELECT * FROM moniter;
 id |   name   | class
----+----------+-------
  1 | Adeel    | BSCS
  4 | Adnan    | BSCS
  5 | Husnain  | BSCS
  6 | Husnain1 | BSCS
  7 | Husnain2 | BSCS
  8 | Husnain3 | BSCS
  9 | Husnain4 | BSCS
 10 | Husnain5 | BSCS
 11 | Husnain6 | BSCS
 12 | Husnain7 | BSCS
 13 | Husnain8 | BSCS
 14 | Husnain9 | BSCS
 15 | Husnain0 | BSCS
(13 rows)

mydatabase=# DELETE FROM moniter WHERE id=(SELECT MAX(id) FROM moniter);
mydatabase=# SELECT * FROM moniter;
 id |   name   | class
----+----------+-------
  1 | Adeel    | BSCS
  4 | Adnan    | BSCS
  5 | Husnain  | BSCS
  6 | Husnain1 | BSCS
  7 | Husnain2 | BSCS
  8 | Husnain3 | BSCS
  9 | Husnain4 | BSCS
 10 | Husnain5 | BSCS
 11 | Husnain6 | BSCS
 12 | Husnain7 | BSCS
 13 | Husnain8 | BSCS
 14 | Husnain9 | BSCS
(12 rows)

mydatabase=#
