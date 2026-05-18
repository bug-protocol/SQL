# Question - 2: 
## Relational Databases:-

1. Stores data in the form of tables,, rows and columns ---> relations between tables exist

Rows - > records  
Columns ->> Attributes/fields of the data  

Primary Key ---> For uniquely identifying a table  
Foreign key --> Links one table to another  

---

# Oracle ==> First SQL based relational DB -> 1979

# Need of oracle arose because previously existing databases had many limitations.

===> Initially we had  
# Hierarchical Databases  
# Network Databases  

used to have tree or complex pointer structures

# IBM db2 ==> IBM was researching on relational Databases long before Oracle was commercialised.

(System R) => The name of the research project  

# IBM still released it because Oracle alone would not be suffice to complete the needs of the market

---

# Sybase => Earlier the Databases were incorporated in Applications. During 70's and 80's.

Later on during 90's the rise of PCs led to an client - server architecture. Where Database was stored in a centralised server room and employee could have used application on his PC reducing the cost and increasing the flexibility.

# Sybase differentiated ===> Client and Database

---

# Rise of MySQL ==>  

During the internet revolution ===> Websites and users exploded  
Startups and companies needed a cheap and scalable database.

Came MySQL and PostgreSQL

---

# MySQL and PostgreSQL were ====>  

# free/ open-source  
# fast  
# easy to host  
# easy to learn  

---

# LAMP STACK REVOLUTION ==>

MySQL became part of famous LAMP stack revolution ==> Linux  
                                                     Apache  
                                                     MySQL  
                                                     PHP/ Python/ Perl  

# The famous firms=== Wordpress, Early Facebook used MySQL heavily

---

# PostgreSQL ===>

# Mysql was focussing on simplicity and speed  
PostgreSQL focussed on====>

# More advanced and academically strong

# complex queries were helpful because ===> REAL world business were complex and interconnected.

Simple queries might not be helpful

# Today there might not be much difference between MySQL and Postgres but Earlier

Mysql lacked

- subqueries
- transactions
- foreign keys
- window functions
- recursive CTEs
- strict ACID behavior

---

# Late 2000 ====> NoSQL Revolution

--> Websites blew up => millions of users

Came NoSQL databases

---

# Two Databases arose earlier during this era==>

Google => Bigtable => inspired => Cassandra and HBase  
Amazon Dynamo => inspired => DynamoDB and Riak

---

# First popular modern NoSQL

MongoDB => MongoDB became popular because traditional relational databases were struggling with the new kind of internet applications emerging in the 2000s.

---

# Problems with Relational DBs ==>

## 1 . Rigid Schema

# Table structure was initially predefined….now suppose you gotta add a column. You would need to change the migrations…write queries like Alter table, etc…etc.

While in NoSQL no fixed structure was required => no fixed schema defined

---

## 2. Scaling

# SQL needed more CPU, storage and RAM

MongoDB introduced ==> Horizontal Scaling

---

## 3. Joins Became Expensive at Huge Scale

MongoDB encourages storing related data together

---

# Rise of Cloud Databases

Amazon, Google, Microsoft started offering DBaaS (Database as a Service)

---

# Rise of Real Time Databases…youtube streaming, stocks..

Comes - Apache Kafka, Apache Spark

---

# Redis ->> Solved the problem of speed

# Now we're moving towards Vector Databases