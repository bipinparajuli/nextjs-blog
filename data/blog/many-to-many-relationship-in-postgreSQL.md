---
title: How to Create Many To Many Relationships in PostgreSQL
date: '2023-04-02'
tags: ['Software Development', 'PostgreSQL', 'Database']
draft: false
summary:
images: []
---

## How to Create Many To Many Relationships in PostgreSQL

Many To Many Relationships in Postgresql is a relationship in which one or more items in one table can have relations with one or more items in another table. For example, a student can be enrolled in more than one course and a course can belong to more than one student.

![](https://lh5.googleusercontent.com/bCn5Br-cwe5CnPY0B9K90UFVROmv04NTPE_GpeuF5EKJlQYH3yj2xpNbugO_4-z79aYjO3kdXFVvvTmqtkDNmf508TvanJMF8G3T2262xizTJFnZJSm5o6dWl4JveconYRVgYyx9hsnkah9_zgO6N6s)

## Table of Content

- Introduction to Postgres
- Prerequisites
- Run PostgreSQL with docker
- Creating Database
- Creating Tables
- Creating Junction table
- Inserting Data
- Querying Data
- Conclusion

## What is PostgreSQL?

PostgreSQL is a popular open-source relational database management system that offers many advanced features to developers. One of the key features is the ability to create many-to-many relationships between tables. In this blog post, we will explore how to create a many-to-many relationship in PostgreSQL.

## Prerequisites

Before moving into further details it’s expected you have the following Prerequisites:

- Any experience working with PostgreSQL will be helpful.
- Have docker running on your machine.
- Familiar to work with CLI.

## Run PostgreSQL with docker

For all the examples below we will use the official image of PostgreSQL 14. To pull PostgreSQL 14 from the docker hub, you can execute the command **docker pull** command, which will pull images from the docker if unavailable locally.

```

docker pull postgres:14

```

This pulls postgres images locally. Now you can execute the docker run command to run what you have pulled locally. The command is:

```

docker run -e POSTGRES_PASSWORD=password --name=pg -- rm -d -p 5432:5432 postgres:14

```

Now You should have Postgres running in the background. You have run the docker run command with the - - rm flag which will remove the container after it stops. The added - - name parameters add a name to the running container pg. The -p allows us to expose port 5432 locally which the port Postgres run on by default. You have passed the environment variable with the -e parameter. That is POSTGRES_PASSWORD with value password.

Now you can connect to the running database by running the following command:

```

docker exec -u postgres -it pg psql

```

## Creating database

The Postgres server is running and you have access to psql postgres shell to create a database inside the container. For this example, we will be creating a school database. Now create a database by running the following command in psql shell:

```

CREATE DATABASE school;

```

Type \c school to connect to the school database. We were previously connected to postgres default database. If you are following along you should be on the following condition:

![](https://lh3.googleusercontent.com/aGpjtmg7CxtSQu-QXdRe6uo21P6rqTBTSTDk4uvUULWaI-02rcK2zHv_gWhlvkLjmVBvBiah7voPHAT-JYG3-0ujyQ80eh0h2vUBKZoe_VFnlbI7WlUqVteV-EJSPu3eTW_udJl9dW0t3zh9tZVvNok)

## Creating tables

To create many-to-many relations we need the first two tables that will participate in many-to-many relationships in postgres. For this example, we will create two tables students and courses to show that students can enroll to more than one course and each course may have multiple students enrolled in it.

Let’s create student's table first:

```

CREATE TABLE students (

id SERIAL PRIMARY KEY,

name VARCHAR(50) NOT NULL

);

```

Then let’s create a courses table:

```

CREATE TABLE courses (

id SERIAL PRIMARY KEY,

name VARCHAR(50) NOT NULL

);

```

We have successfully created two tables each with id as the primary key and a name field which holds character up to length of 50.

## Creating junction Table

Since relational databases don’t allow implementing a direct many-to-many relationship between two tables So for every many-to-many association, we will need an additional table, known as a junction table. A junction table in the database bridges the table together by referencing the primary key of each table. Now let’s create another table that creates many-to-many relationships using the following command:

```

CREATE TABLE courses_students(course_id INTEGER REFERENCES students(id),

student_id INTEGER REFERENCES courses(id),

CONSTRAINT courses_students_pk PRIMARY KEY(course_id,student_id) );


```

In the above command we’re setting the combination of course_id and student_id must be a unique primary key instead of incrementing ID. The above table will describe the many-to-many relationship with two foreign keys between courses and students.

## Inserting Data

Once you have created two tables and one junction table now you can insert data into them. For this example will insert the student's names virat and john in the student's table and the course name math and science in the courses table from the following table:

```

INSERT INTO students(name) VALUES ('virat'),('john');

```

```

INSERT INTO courses(name) VALUES ('math'),('science');

```

You also have to insert data into the student_courses junction table to create the many-to-many relationship between the students and courses. Let’s Insert data in such a way that a student name virat is enrolled in two courses math and science.

```

INSERT INTO courses_students (student_id,course_id) VALUES (1,1),(1,2);


```

## Querying data

Once the data has been inserted into the tables, you can query it to retrieve information about the many-to-many relationship. For this example, we are going to retrieve all the courses enrolled by virat.

```

SELECT c.name as name FROM courses_students cs INNER JOIN courses c ON c.id = cs.course_id;

```

This query will return the following result:

![](https://lh4.googleusercontent.com/URsiO1MWNCr-yFCXoUNox5Kb2AoQugnW1oBmebNsnyIhI2hUTsU1edy8CjfzqZzWtrcNeoy_lZgnhl8hKMhgtCg3GxnVFQhV8IRxsxY7mpAPSPb26JMAQDbI7ItFvIC2oUZS_CZWFYYR4HmsYOdGI8Q)

## Conclusion

Creating a many-to-many relationship in PostgreSQL involves creating the tables, creating the junction table, inserting data into the tables, and querying the data. By following these steps, you can establish a many-to-many relationship between any two tables in PostgreSQL.
