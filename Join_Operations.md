# Join Operations in Oracle

1. The purpose of a join is to combine the data across tables.
2. A join is actually performed by the where clause which combines the specified rows of tables.
3. If a join involves in more than two tables then Oracle joins first two tables based on the joins condition and then compares the result with the next table and so on.

* **Online Oracle Platform:**
  * https://rextester.com/l/oracle_online_compiler
  * https://livesql.oracle.com/

* **All types of Join operations are:**

<ol start = "1.">
  <li><b> Equi Join </b></li>
  <li><b> Non-Equi Join </b></li>
  <li><b> Self Join </b></li>
  <li><b> Natural Join </b></li>
  <li><b> Cross Join </b></li>
  <li><b> Outer Join </b></li>
    <ol start = "a.">
    <li><b> Left outer join</b></li>
    <li><b> Right outer join</b></li>
    <li><b> Full outer join</b></li>
    </ol>
  <li><b> Inner Join </b></li>
</ol>
<b>
<pre>
-- Creating Tables:
drop table department;
create table department (deptno number, dname varchar2(15), loc varchar2(10));
insert into department values(10,'Inventory','Hyderabad');
insert into department values(20,'Finance','Bangalore');
insert into department values(30,'HR','Mumbai');
insert into department values(50,'IT','Delhi');
describe department;
select * from department;
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/4a0dab9d-ec1d-4365-b8fd-34e25974e0e2)

<pre>
drop table employee;
create table employee (empno number, ename varchar2(15), job varchar2(10), mgr number, deptno number);
insert into employee values(111,'Saketh','Analyst',444,10);
insert into employee values(222,'Sudha','Clerk',333,20);
insert into employee values(333,'Jagan','Manager',111,10);
insert into employee values(444,'Madhu','Engineer',222,40);
describe employee;
select * from employee;
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/2a343d4e-72ac-4044-aaca-274ef6be0da2)

<pre>
-- 1. Equi Join
-- A join which contains an equal to '=' operator in the join condition.
select * from employee e, department d where e.deptno = d.deptno;
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/b6373a1b-e7c8-4e95-9aed-481e72087cce)

<pre>
-- Using clause
select * from employee e join department d using(deptno);
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/a46aca0c-8c6a-4626-9991-80e5f9e7e596)

<pre>
-- On clause
select * from employee e join department d on(e.deptno = d.deptno);
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/2cddd50a-7097-4a5b-bde5-de5493771408)

<pre>
-- 2. Non-Equi Join
-- A join which contains an operator other than equal to '=' in the join condition.
select * from employee e, department d where e.deptno > d.deptno;
-- Or
select * from employee e join department d on(e.deptno > d.deptno);
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/7fe5cb9e-48d1-42f2-94db-1fb9c7fc96c9)

<pre>
-- 3. Self Join
-- Joining the table itself is called self join.
select * from employee e1, employee e2 where e1.empno = e2.mgr;
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/235992ae-e40f-4aa9-80ae-dbcd953c5657)

<pre>
-- 4. Natural Join
-- Natural join compares all the common columns.
select * from employee natural join department;
-- Note – Output of the query is same as we did in equi-join with the query –
select * from employee e join department d using(deptno);
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/2c4d5b23-fd1d-4aac-8c94-6b84d3e2a57b)

<pre>
-- 5. Cross Join
-- This will give the cross product.
select * from employee cross join department;
-- Same output as
select * from employee, department;
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/611c0a23-3a5b-4c7f-8d63-7b22cb9a14a0)

<pre>
-- 6. Outer Join
-- Outer join gives the non-matching records along with matching records.
--     6 a. Outer Join - Left outer
--          All records from the left table and maching records from the right table
select * from employee e left outer join department d on (e.deptno = d.deptno);
--          Or
select * from employee e, department d where e.deptno = d.deptno(+);
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/137587c8-eab8-440f-94a0-bd8093ae891d)

<pre>
--     6 b. Outer Join - Right outer
--           Maching records from the left table and all records from the right table
select * from employee e right outer join department d on (e.deptno = d.deptno);
--          Or
select * from employee e, department d where e.deptno(+) = d.deptno;
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/14b4c4be-83d7-4576-93f3-b3e45ae633f6)

<pre>
--     6 c. Outer Join - Full outer
--          This will display all the matching records and the non-matching records from both tables.
select * from employee e full outer join department d on (e.deptno = d.deptno);
</pre>

![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/fbb5b3ec-1022-408c-9827-8e43f1d30eba)

<pre>
-- 7. Inner Join
-- This will display all the records that have matched.
select * from employee inner join department using (deptno);
-- Note – Output of the query is same as we did in equi-join with the query –
select * from employee join department using(deptno);
-- and natural join with the query –
select * from employee natural join department;
</pre>


![image](https://github.com/toarnabtrainer/Oracle_Notes/assets/111301975/a34131b0-2925-4e41-9d8b-a71fa951c6d5)

<pre>
-- Deleting both the tables department and employee
drop table department;
drop table employee;
</pre>

<pre>
-- All queries in one place
drop table department;
create table department (deptno number, dname varchar2(15), loc varchar2(10));
insert into department values(10,'Inventory','Hyderabad');
insert into department values(20,'Finance','Bangalore');
insert into department values(30,'HR','Mumbai');
insert into department values(50,'IT','Delhi');
describe department;
select * from department;
drop table employee;
create table employee (empno number, ename varchar2(15), job varchar2(10), mgr
number, deptno number);
insert into employee values(111,'Saketh','Analyst',444,10);
insert into employee values(222,'Sudha','Clerk',333,20);
insert into employee values(333,'Jagan','Manager',111,10);
insert into employee values(444,'Madhu','Engineer',222,40);
describe employee;
select * from employee;
select * from employee e, department d where e.deptno = d.deptno;
select * from employee e join department d using(deptno);
select * from employee e join department d on(e.deptno = d.deptno);
select * from employee e, department d where e.deptno > d.deptno;
select * from employee e1, employee e2 where e1.empno = e2.mgr;
select * from employee natural join department;
select * from employee cross join department;
select * from employee e left outer join department d on (e.deptno = d.deptno);
select * from employee e, department d where e.deptno = d.deptno(+);
select * from employee e right outer join department d on (e.deptno = d.deptno);
select * from employee e, department d where e.deptno(+) = d.deptno;
select * from employee e full outer join department d on (e.deptno = d.deptno);
select * from employee inner join department using (deptno);
drop table department;
drop table employee;
</pre>
</b>
