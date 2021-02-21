# Domain Types
## char(n) (or character(n)):
    
 fixed-length character string, with user-specified length.
## varchar(n) (or character varying):
 variable-length character string, with user-specified maximum length.
## int or integer:
 an integer (length is machine-dependent).
## smallint:
 a small integer (length is machine-dependent). IBM Standard 16bit
## numeric(p,d):
Fixed point number, with user-specified precision of pdigits, with ndigits to the right of decimal point. 
  >e.g:  *For example, numeric(6,2) is a number that has 4 digits before the decimal and 2 digits after the decimal.*
## real or double precision:
 floating-point or double-precision floating-point numbers, with machine-dependent precision.
## float(n): 
Floating point number, with user-specified precision of at least **n** digits

# SQL words book
* create table(attribute domain_types,)
* insert into (name) values ('data',);
```
create table instructor(
    ID   char(5),
    name varchar(20)not null,
    dept_name   varchar(20),
    salary    numeric(8,2)
    primary key(ID),
    foreign key (dept_name) references department ) );

insert into instructor  values(‘10211’, ’Smith’, ’Biology’, 66000);
insert into instructor  values(‘10211’, null, ’Biology’, 66000);
```

# Basic integrity constraints in Create Table
## not null:   
  required
## primary key (A1, ..., An ), shortcut:

 ``` course_idvarchar(8) primary key,```
foreign key (Am, ..., An ) references r
```
create table instructor(
    ID   char(5),
    name varchar(20)not null,
    dept_name   varchar(20),
    salary    numeric(8,2))

```
## drop table **student**:    

 Deletes the table and its contents.
## delete from **student**:       

Deletes all contents of table, but retains table.      
## alter table **r** add **A D**    
where **A** is the name of the attribute to be added to relation **r** and **D** is the domain of A. And all tuples in the relation are assigned null as the value for the new attribute.  

## alter table **r** drop **A**     
  where **A** is the name of an attribute of relationr.

# DML: Basic SQL Query Structure 
```
basic grammar:

SELECT A1, A2, A3
FROM R1, R2, R3
WHERE p

```
## Basic SQL Query
### relation-list: 
A list of relation names (possibly with a range-variableafter each name).
### target-list:
A list of attributes of relations in relation-list
### qualification                
## SELECT 
``` SELECTION```  = projection in relational algebra.    

``` 
e.g:
 SELECT name
 FROM instructor
```
 Notice: *SqL is CaSE InseNsitive*

### distinct:
force to eliminate the duplicates
```

usage:

SELECT DISTINCT dept_name
FROM instructor

```

### ALL:
kinda opposite to distinct which force NOT to eliminate duplicate

``` 
usage:

SELECT ALL dept_name 
FROM instructor
```
### asterisk(*)
An asterisk in the select clause denotes “all attributes”

###  others
The SELECT clause can contain arithmetic expressions involving the operations +, –, x, and /, and operating on constants or attributes of tuples.
```
usage: 

SELECT ID, name, salary/12
FROM instructor

```
The code above will return ID, name, and salary divided by 12 from the relation instructor
## FROM
specify the target of the SELECT clause.
target should be a relation.
multiple targets are allowed after FROM, refer [Joins and Cartesian Product](#Joins).
```
usage:

FROM target
```

## WHERE
The WHERE clause specifies boolean algebra conditions that the result must satisfy, corresponding selection in relation algebra.

logical connectives:  and, or, not are allowed.

```
usage:

SELECT name
FROM instructor
WHERE dept_name=‘Comp. Sci.'ANDsalary > 80000
```
the code returns all instructors in the Comp. Sci. dept with salary > 80000

### <a name="Joins"></a> Joins and Cartesian Product
