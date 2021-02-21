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
WHERE dept_name=‘Comp. Sci.'AND salary > 80000
```
the code returns all instructors in the Comp. Sci. dept with salary > 80000

### WHERE

``` 
usage:

SELECT name 
FROM instructor
WHERE salary between 90000 and 100000

```
the code returns the names of all instructors with salary between $90,000 and $100,000.

## <a name="Joins"></a> Joins and Cartesian Product`
*  <del> ``` AND``` in WHERE clause, which connects two **relations**. 
* <del> Cartesian Product: multiple targets after the ```FROM  ```.       

the concept is hard to explain, refer the example.
```
SELECT name, course_id
FROM instructor, teaches
WHERE instructor.ID = teaches.ID
```
the code return: For all instructors who have taught some course, find their names and the course ID of the courses they taught

```

SELECT section.course_id, semester, year, title
FROM section, course
WHERE section.course_id= course.course_id AND dept_name=‘Comp. Sci.

```

### Natural Join
Natural join matches tuples with the same values for all common attributes, and retains only one copy of each common column.

```
usage:

SELECT *
FROM instructor NATURAL JOIN teaches;

```

assume there are two relations
Instructor (ID, name, dept_name, salary)
Teaches (ID, course_id, sec_id, semester, year)
```
e.g: 

SELECT name,course_id 
FROM instructor NATURAL JOIN teaches;

which equal to 

SELECT name, course_id
FROM instructor, teaches
WHERE instructor.ID = teaches.ID;

```

the code returns the names of instructors along with the course ID of the courses that they taught.

#### Danger in natural join: Beware of unrelated attributes with same name getting matched incorrectly.

assume there are 4 relations:    
Instructor (ID, name, dept_name, salary)      
Course (course_id, title, dept_name, credits)     
Section (course_id, section_id, semester, year)     
Teaches (ID, course_id, sec_id, semester, year)       

* E.g  List the names of instructors along with the titles of all courses that they teach.

INCORRECT VERSION: Natural join uses course.dept_name= instructor.dept_name    
```
SELECT name, title 
FROM instructor NATURAL JOIN  teaches NATURAL JOIN course;   
```

correct version:
```
SELECT name, title                 
FROM instructor NATURAL JOIN teaches, course 
WHERE teaches.course_id= course.course_id;
```
reason:  This query lists the names of instructors along with the titles of the courses that they teach--ONLY for the courses in the dept they belong to.

## Rename Operation
``` AS ```:
```
usage:

old-name AS new-name

```

```
SELECT ID, name, salary/12 
AS monthly_salary
FROM instructor
```
Oracle does not allow AS

## Ordering the Display of Tuples
ORDER BY
```
usage:

SELECT DISTINCT name
FROM instructor
ORDER BY  name

```

parameters: 
 ```desc``` for descending order or ```asc``` for ascending order, for each attribute; ascending order is the default.
 multiple attributes can be used.

## Set Operations
* Union: 
  
 ```
  e.g: Find courses in Fall 2009 or in Spring 2010.

  (select course_id 
  from section 
  where sem= ‘Fall’ and year = 2009)
  union
  (select course_id from section 
  where sem= ‘Spring’ and year = 2010)

  ```

* intersect

```
e.g: Find courses that ran in Fall 2009 and in Spring 2010.


(select course_id 
from section 
where sem= ‘Fall’ and year = 2009)
intersect
(select course_id from section 
where sem= ‘Spring’ and year = 2010)

```

*  except
```
e.g Find courses that ran in Fall 2009 but not in Spring 2010.

(select course_id 
from section 
where sem= ‘Fall’ and year = 2009)
except
(select course_id from section 
where sem= ‘Spring’ and year = 2010)

```

## NULL
```
select sum (salary )
from instructor
```
1. Above statement ignores null amounts.
2. Result is null if there is no non-null amount.
3. All aggregate operations except count(*) ignore tuples with null values on the aggregated attributes.
4.  collection has only null values, then count returns 0, All other aggregates return null.
### is NULL
used to check for null values.

```
e.g:
select name 
from instructor
where salary is NULL
```
the code returns  Find all instructors whose salary is null."



## logical connectives
OR AND NOT

## aggregate Functions
* avg: average value     usage:```select avg(salary)```
* min: minimum value     usage: ```select count (distinct ID)```  
* find number of tuples         usage:```select count (*)    from course;```
* max:  maximum values     
* sum:  sum of values     
* count:  number of values     

## Queries With GROUP BY and HAVING
syntax:
```
SELECT  [DISTINCT]  target-list
FROM  relation-list
WHERE  qualification
GROUP BY  grouping-list
HAVING  group-qualification

```
The relation that results from the SELECT-FROM-WHERE is grouped according to the values of all those attributes, and any aggregation is applied only within each group.
```
e.g
assume 4 relations: Query: “Find the number of instructors in eachdepartment who teach a course in the Spring 2020 semester.”
Instructor (ID, name, dept_name, salary)
Course (course_id, title, dept_name, credits)
Section (course_id, section_id, semester, year)
Teaches (ID, course_id, sec_id, semester, year)
Department(dept_name, building, budget)

SELECT I.dept_name, Count(distinct T.ID)
FROM Instructor I, Teaches T 
WHERE I.ID= T.ID 
AND T.semester=“Spring”
AND T.year= “2020”
GROUP BY   I.dept_name
```

* Attributes in select clause outside of aggregate functions **MUST** appear in group by list
```
/* erroneous query */
SELECT dept_name, ID, avg(salary)
FROM instructor
GROUP BY dept_name;
```

### Having Clause
```
e.g:

Find the names and average salaries of all departments whose average salary is greater than 60000.

SELECT dept_name, AVG (salary)
FROM instructor
GROUP BY dept_name
HAVING AVG (salary) > 60000
```
* Predicates in the HAVINGclause are applied afterthe formation of groups. whereas predicates in the WHEREclause are applied beforeforming groups.
