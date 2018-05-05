
---
title: "Introduction to Databases"
date: 2018-05-04T22:47:33+05:00
draft: false
comments: false
showSocial: false
keywords: databases, sql, introduction
---
<!--more-->
<!--toc-->

These notes are inspired from the [Lecture 18](https://www.youtube.com/watch?v=Pk1E3Iod7eQ&list=PLRdybCcWDFzCag9A0h1m9QYaujD0xefgM&index=17) and [Lecture 19](https://www.youtube.com/watch?v=DRGr3tFtHiQ&list=PLRdybCcWDFzCag9A0h1m9QYaujD0xefgM&index=18) of UC Berkely's CS162 Operating System course along with some additional resources from internet. I'll try to provide (collect) all references at the end of this post. This post is intended as *scratch pad* for my future self and may not help anyone else much; though the lecture video I linked earlier should definitely help you if you are looking for a Databases Introduction.

# What is a database?
First of all, database is a collection of **code** files i.e. coding and databases are not two distinct concepts (to my younger self). Informally, a database can be described as a collection of code files that provide an interface to interact with data quickly and reliably i.e. instead of replicating (and managing) data-handling code in your program, you can call a library to handle (store, retrieve, retrieve-a-special-subset-of-data; aka a query) data related operations for you. That imported library will provide an interface (higher level calling semantics; function calls etc) to do the operations.

Formally, [John Canny](https://www2.eecs.berkeley.edu/Faculty/Homepages/canny.html) in his lecture has defined a Database as follwoing:

> A large **integrated collection** of data which models the real world entities and (potnetially) the relationships between those entities  e.g. Entities (teams, games etc) Relationships (played-*the-game*, memebr-of-team etc)

For example, {{< hl-text blue >}}Cal (an entity){{< /hl-text >}} {{< hl-text red >}}plays against (a relationship){{< /hl-text >}} {{< hl-text blue >}}Stanford (another entity){{< /hl-text >}} {{< hl-text red >}}in (another relationship){{< /hl-text >}} {{< hl-text blue >}}The Big Game (yet another entity){{< /hl-text >}}.


# How different organizations are using databases?

Organization|DB Size                    | Usage |Desing Constraints       | Keywords | Recommended DB types
------------|---------------------------|-------|-------------------------|----------|---------------------
Yahoo       |`700 TB`                   | matching advertisers with users/consumers |higly available; millions of active users at any given time; service (ad) should be provided, accuracy/quality is secondary | Timeliness, Availability | BASE 
AT&T        |`330 TB`            | managing users data, bills, usage etc. Should a user be allowed to make the call or should be routed to "insufficient balance" message |a user should not be charged for what he/she didn't consume and at the same time should be charged for each text message or internet MB he/she consumed. A (wireless) client can change it's location, can fly to another city or even another country.| Accuracy, Consistency, Durability| ACID 
Austrailian Bureaue of Statistics|`250 TB`| |many queries with big results e.g. how many people are ther in Melbourne region who are younger than 25 years, have a graduate degree and work in Tech secotr. Ability to go through TBs of data in short amount of time. | Fast, Rich Queries| OLAP, ROLAP 


# What is "structured data"?
A {{< hl-text red >}}data model{{< /hl-text >}} is a collection of entities and their relationships.

A {{< hl-text red >}}schema{{< /hl-text >}} is an instance of a data model. It describes the fields in the database; how the database is organized.

A {{< hl-text red >}}relational database{{< /hl-text >}} is the most used {{< hl-text red >}}data model{{< /hl-text >}}. Example of  {{< hl-text red >}}relation{{< /hl-text >}} include tables with rows and columns. Every relation has a {{< hl-text red >}}schema{{< /hl-text >}} which describes the fields in the column.

## Example: A university's database
### Conceptual Schema

- A university has **students**
- Some **classes** are offered by the university
- **Students enroll in classes** 

In a database all these three conceptual items will be modeled as three tables:

- **Student Table** 
`(student_id:string, name:string, email:string, age:int, gpa:float)` 

- **Courses Table** 
`(course_id:string, name:string, credit_hours:int)` 

- **Enrolled Table** 
```python
( concatenate(course_id:string, student_id:string), gpa:float)
 		FOREIGN KEY student_id REFERENCES(is from) Students(table)
 		FOREIGN KEY course_id  REFERENCES(is from) Courses(table)
```


`concatenate(course_id:string, student_id:string)` in last table is calles a **Composite Key**.

The last two lines in *Enrolled* table are **constraints** on the table items.

### External Schema (View)
A View is a virtual table which does not exist as a *real* table in DB but instead is formed as a result of queries and some data operations on the results of queries. For example if you want to create an **Enrollment** table, which is a required information in a university so that instructors and admisntrators can view (know) how many students are enrolled in different courses, it can be done by creating a *virtual table* called formally as a **View**.
```python
course_info(couse_id:string, enrollment:int)
	CREATE VIEW course_info AS
		SELECT course_id, count(*) as enrollment
		FROM Enrolled
		GROUP BY course_id
```

 
## How does `Student` table look like?

student_id|name|email|age|gpa
----------|----|-----|---|---
12040013|Afzal|afzal@univ.edu|24|3.8
15100364|Marry|marry@univ.edu|21|4.0
19232974|Phillip|phillip@univ.edu|18|2.8



{{< alert info >}}
An important point to note is; in storage system (hard disk, RAM etc) the data is organized serially(linearly) i.e. `3132 3034 3030 3133 4166 7a61 6c61 667a 616c 4075 6e69 762e 6564 7532 3433 2e38 #This is the first line of example of Student's table in hexadecimal format` NOT as 2D or 3D arrays. It is the job of DBMS to decide how to store the data on storage system so that it can be accessed efficiently. It is just like a filesystem in operating systems.
{{< /alert >}}

# What is a Database Management System (DBMS)?
> A softwre system designed to **store, manage and facilitate access** to databases.

A DBMS provide:

- **Data Definition Language (DDL)**
	-- Define relations, schema

- **Data Manipulation Langugage (DML)**
	-- Queries to retrieve, analyze and modify data

- **Guarantees** (or no guarantee) about *durability, concurrency, semantics* etc


## Queries

### Example 1
Let's assume we want to get a list of all students who have GPA>3. The process to run this query will be: 


```python
SELECT student_id, name, gpa
FROM Students S
WHERE S.gpa>3
```


{{<mermaid align="center">}}
graph TB;
	D((Database)) ==>|run query on table| A
    A(Students) ==>|all students with GPA>3| B{SELECT} 
    B ==>|return student_id, name, gpa columns| C(Projection)

{{< /mermaid >}}


> It is the job of DBMS to handle query plan generation and optimization as well as to ensure **correct** execution.


### Example 2
Now let's assume we want to answer another question that "How many students are there who take atleast one 4-credit-hour cours?". The process to run this query will be: 

```python
SELECT
	COUNT_DISTINCT(E.student_id)
FROM Enrolled E, Courses C
WHERE E.course_id==C.course_id
	AND C.credit_hours=4
```

{{<mermaid align="center">}}
graph TB;
	D((Database)) ==>|get table| A
	D((Database)) ==>|get table| B

    A(Enrolled) ==>|"Columns=(student_id, course_id, grade)"| C
    B(Courses)  ==>|"Columns=(course_id, credit_hours, course_name)"| C

    C(Join) ==>|"Columns=(student_id, course_id, course_name, grade, credit_hours) <br/>Selection Criterion: credit_hours=4"| E
    E(Select) ==> |"get required columns"| F
    F(Projection) ==>|"student_id"| G
    G(COUNT_DISTINCT) ==> H
    H[output integer]

{{< /mermaid >}}

## Transactions
> A transaction is the {{< hl-text red >}}atomic sequence{{< /hl-text >}} of database actions (reads and writes) that take the database from one {{< hl-text red >}}consistent state{{< /hl-text >}} to another.

{{<mermaid align="center">}}
graph LR;
	A(Consistent State 1) --> B(Consistent State 2)
{{< /mermaid >}}
OR  

> A transaction is the abstract atomic series of actions that the user wants to see. It can be broken down to a series of reads and writes; but should be executed {{< hl-text red >}}ALL or NOTHING{{< /hl-text >}}.


### Consistent
Consistent in above definition means that it satisfies:

- relational constraints
- integrity constraints involving keys from one table to the next
- any other consistency contraints like fields being positive or balance being non-negative

Consistency generally is defined (coded) as per the problem "semantics". For example in a bank's db balance being non-negative is a requirement for consistency. So a set of operations that make the balance of an account negative will violate *this* consistency constraint and such set of actions will be discarded i.e. transaction will be reversed/not-allowed.


> A transaction is applied in either ALL or NOTHING fashion. Since a transaction is defined as an operation that takes the database from one consistent state to another; so after each transaction all rules for consistency are checked, if any of those rules fail the transaction is reversed/rolled-back.


#### Example
Let's assume we are working for a bank and want to write a code for transfering money from a user's saving account to his/her checking account.
{{<mermaid align="center">}}
graph LR;
	A("checking: 200.00 <br/> savings: 1000.00") -->|"transfer $100 from savings account <br/> to checking account"| B("checking: 300.00 <br/> savings: 900.00")
{{< /mermaid >}}
Now, **banking-semantics** force us to ensure that the money should neither {{< hl-text danger >}}evaoprate{{< /hl-text >}} in thin air nor it should be {{< hl-text danger >}}produced{{< /hl-text >}} from thin air in such a transaction. i.e. `Total Assets of a user should remain constant before and after such a transaction.` This constraint will be added in the **consistency criterion** of inter-account bank transfer and is motivated from **banking semantics**.

{{< alert danger no-icon >}}
In general, it is upto the transaction writer (aka the developer) to ensure that transaction is written (coded) in such a way that it *preserves* the (self-defined) *consistency* criterion.
{{< /alert >}}

### Integrity Constraints
A **DBMS** (at max) can provide some (limited) checks on the output of queries which automatically enforce some basic consistency criterion. Such checks are called *Integrity Constraints*. For example, balance must be non-negative. 

These checks are limited to single rows of the table and cannot enforce criterion which depend on multiple rows of the table e.g. the sum of balance between two rows should be constant before and after the transaction.

### Atomicity and Consistency
Any reasonably defined (coded) *transaction* is going to enforce (in transaction code) *that* (total asset is same in inter-accounts transfer) of constraints.

Making the *transaction* **atomic** gurrantees that *when-the-writer-does-the-right-thing*, the outcome is going to be **consistent**. In other words, the writer is responsible for ensuring that an invariant *holds* and then **atomicity** make sure that all of the steps necessary to make *that* (holding the invariant) happen, actually happen.

It is not as easy as it sounds because we want high *performance* systems which means we want *concurrency*; multiple users accessing the database at the same time and doing multiple updates.

## How to handle concurrency?
- Naive approach: {{< hl-text danger >}}lock{{< /hl-text >}} the enrite or part of table for one user at a time to ensure that *things* remain consistent.

- Better approach: {{< hl-text green >}}interlace{{< /hl-text >}} executions (read/writes) of muliple users

### Locks
Reading AND not-mutating a field is a *safe sharable* operation. As many different clients can share the same information. So **locks** are used to prevent writing while someone is reading that value.

#### Reading Locks
Reading locks are sharable i.e. many processes can read same value concurrently.

#### Writing Locks
While same data can be read concurrently, that is not true for writing. There can only be one owner of a writing lock. As soon as anyone gets a writing lock, it prevents anyone else to access (read/write) that data until writing lock is released.

#### Lock Manager
In DBMS, a process called lock-manager is responsible for handling **acquition and release** of locks by keeping a **consistent** (english adjective) table of who has which locks. This table should be **highly available** as every process is accessing this table for each read and write.

Lock Manager is one of the 5 major building blocks of a DBMS.

#### Latching
Low level locking (e.g. locking entire table) is also available in a DBMS and is called latching.

#### Locking Granularity
What level to lock the DB at:

- Database (entire of it)
- Table (entire of it)
- Rows (entire of it)

Almost all DBMS support locking to the row-level.

##### Example: DB or Table level locking
Situtation: A bank's process talking to another bank's server and telling the accounts at the end of business day. 

It makes sense to lock the entire table or DB as number of transactions from a single client are quire high in this case and acquiring and releasing the locks for each read and write is a costly operation. So throughput will increase if we avoid the high acquisition and release cost of locking.



# The ACID properties of Transactions
## Atomicity
> All actions in the transaction happen or none happen.


A transaction:

- might *commit* after completing all of its operations or
- it could abort (or be aborted) after executing some operations and the databse should roll-back all of its effects and restore the db at the same point as was at the begining of that transaction.
-- It means DBMS must log all actions so that it can undo the actions of aborted transaction.

## Consistency
> Transactions maintain data integrity e.g. balance can't be negative, can't schedule meeting on Feb 30th.

Consistency ensures that:

- Data follows *Integrity Constraints*
-- ICs are checked after the transaction and it will fail if the constraints are not satisfied.
-- DB does not understand the semantics of the transaction being done (inter-account transfer should maintain a constant asset profile for each user) so it does not check for it in ICs. The developer must ensure to execute these checks after the transaction and before *committing*.
- If DB was consistent before transaction, it will be consistent after the transaction.

## Isolation
> Execution of one transaction is isolated from that of all other. Steps that one customer does or is doing must not interfere with the steps the other customer does or is doing.

Isolation ensures that:

- Transactions are not interfering with wach other
- Each transaction executes as if it was running by itself
-- It can't see the *partial* results fo another transaction


## Durability
> If a transaction commits, its effects persist despite crashes.

Durability ensures that data should survive in the presence of:

- system crashes
- disk crashes

as well as it ensures that crashes or failures of parts of a DB donot cause partial failures of transaction.

All *committed* updates and only those updates are reflected in the DB:

- some care must be taken to handle the case of a crash occuring during the recovery process

If a transaction hasn't really propagated thoughout the whole system, there must be enough *log* data available to make sure that when it comes up and before it is *actually available* it has redone those updates and propagated them completely.



# The BASE properties of Transactions

## Basic Availability
> The database appears to work most of the time.

## Soft-state
> Data stores do not have to be *write consistent*, nor do different replicas have to be consistent at all times.

## Eventual Consistency
> Data stores exhibit consistency at some later point (e.g. lazily at read time)



# References and Resources
1. [ACM document explaining ACID vs BASE system](http://delivery.acm.org/10.1145/1400000/1394128/p48-pritchett.pdf?ip=110.93.234.9&id=1394128&acc=OPEN&key=55CAB23EF39677E3%2EFE8F10057D3A2BA3%2E4D4702B0C3E38B35%2E6D218144511F3437&__acm__=1525539035_59726f519e86438627120c063f3ffce7)
2. [UC Berkely Lecture#18 (Fall 2013) by John Canny](https://www.youtube.com/watch?v=Pk1E3Iod7eQ&list=PLRdybCcWDFzCag9A0h1m9QYaujD0xefgM&index=17)
3. [UC Berkely Lecture#19 (Fall 2013) by John Canny](https://www.youtube.com/watch?v=DRGr3tFtHiQ&list=PLRdybCcWDFzCag9A0h1m9QYaujD0xefgM&index=18)
4. 



















