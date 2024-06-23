# DBMS Book by Korth, Chapter-4, Exercise Problem No. - 4.2

<b>
<pre>
Schemas -
employee (employee-name, street, city)
works (employee-name, company-name, salary)
company (company-name, city)
manages (employee-name, manager-name)
</pre>
  
<hr>
  
## Korth exercise 4.2 Questions

## Table Creation and Insertion of Records

<pre>
drop table manages;

create table manages
  (
   employee_name varchar(20),
   manager_name varchar(20)
  );

insert into manages values('ashoke','dilip');
insert into manages values('sanjay','dilip');
insert into manages values('rakesh','karim');
insert into manages values('manoj','pranab');
insert into manages values('salim','karim');
insert into manages values('sayan','pranab');
insert into manages values('rajib','pranab');

select * from manages;
desc manages;

-- pragma table_info('manages') -- in Sqlite
--********************************************
</pre>

<hr>

<pre>
drop table company;

create table company
  (
   company_name varchar(20),
   city varchar(5)
  );

insert into company values('satyam','mum');
insert into company values('satyam','del');
insert into company values('aol','cal');
insert into company values('vsnl','del');

select * from company;
desc company;
</pre>

<hr>

<pre>
drop table works;

create table works
  (
   employee_name varchar(20),
   company_name varchar(20),
   salary integer
  );

insert into works values('rakesh','satyam',8000);
insert into works values('rajib','aol',15000);
insert into works values('manoj','aol',10000);
insert into works values('pranab','aol',50000);
insert into works values('karim','satyam',35000);
insert into works values('salim','satyam',6000);
insert into works values('sayan','aol',20000);
insert into works values('dilip','vsnl',35000);
insert into works values('ashoke','vsnl',13000);
insert into works values('sanjay','vsnl',30000);

select * from works;
desc works;
</pre>

<hr>

<pre>
drop table employee;

create table employee
  (
   employee_name varchar(20),
   street varchar(6),
   city varchar(5)
  );

insert into employee values('ashoke','vip','cal');
insert into employee values('sanjay','gt','mum');
insert into employee values('rakesh','bt','cal');
insert into employee values('dilip','vip','cal');
insert into employee values('manoj','gt','del');
insert into employee values('pranab','vip','cal');
insert into employee values('salim','vip','del');
insert into employee values('karim','bt','mum');
insert into employee values('sayan','gt','cal');
insert into employee values('rajib','vip','mum');

select * from employee;
desc employee;
</pre>

<hr>

## Queries:

<pre>
--Question A: Find the names of all employees who work for VSNL.
--Question B: Find the names and cities of residents of all employees who work for VSNL.
--Question C: Find the names, street address, cities of residents of all employees who work for AOL and earn more than 20,000.
--Question D: Find all employees in the database who live in the same cities as the companies for which they work.
--Question E: Find all employees in the database who live in the same cities and in the same streets as do their managers.
--Question F: Find all employee in the database who do not work for VSNL.
--Question G: Find all the employee in the database who earn more than every employee of satyam.
--Question H: Assume that the companies may be located in several cities. Find all companies located in every city in which vsnl is located.
--Question I: Find all employees who earn more than the average salary of all employees of their company.
--Question J: Find the company that has the most number of employees.
--Question K: Find the company that has the smallest payroll.
--Question L: Find those companies whose employees earn higher salary,on average, than the average salary of the satyam.
</pre>

## Korth Exercise 4.2 Questions and Answers:

<pre>
-- *********************************************************************************************************

Query-A: Find employees who work for VSNL.
    
SELECT * FROM works;
select employee_name from works where company_name='vsnl';

-- *********************************************************************************************************

Query-B: Find names and cities of VSNL employees.
    
select employee_name, city
from employee e join works w using(employee_name)
where company_name = 'vsnl';

select employee_name, city
from employee natural JOIN works
where company_name = 'vsnl';

select employee_name, city
from employee inner join works using (employee_name)
where company_name = 'vsnl';

-- nested query
select employee_name, city
from employee
where employee_name in
	(select employee_name
	 from works
	 where company_name = 'vsnl');

-- *********************************************************************************************************

Query-C: Find AOL employees with salary > 20,000.
    
select employee_name,street,city
from employee natural JOIN works
where company_name = 'aol' AND salary > 20000;

select employee_name, street, city
from employee
where employee_name IN (
    select employee_name
    from works
    where company_name = 'aol' AND salary > 20000
);
    
-- *********************************************************************************************************

Query-D: Find employees living in same city as their companies in which they work respectively.

select distinct employee_name
from employee natural join works natural join company

select distinct employee_name
from employee e join works using(employee_name) join company c using (company_name)
where e.city = c.city;
    
-- *********************************************************************************************************

Query-E: Find employees living in same city and street as their respective managers.
    
select e.employee_name
from employee e, employee m
where e.city = m.city
  and e.street = m.street
  and (e.employee_name, m.employee_name) in
      (select * from manages);

-- *********************************************************************************************************
	
Query-F: Find employees not working for VSNL.

select employee_name
from works
minus
(select employee_name
from works
where company_name = 'vsnl');

SELECT distinct employee_name
FROM works
WHERE employee_name NOT IN (
    SELECT employee_name
    FROM works
    WHERE company_name = 'vsnl'
);

-- *********************************************************************************************************

Query-G:  Find employees earning more than every employee of Satyam.

select employee_name
from works
where salary > all (
    select salary
    from works
    where company_name = 'satyam'
);

-- *********************************************************************************************************

Query-H: Find companies located in every city where VSNL is located

-- division operation
select company_name
from company c
where company_name <> 'vsnl'
  and not exists (
	(select city
	from company
	where company_name = 'vsnl')
    minus
    (select city
    from company
    where company.city = c.city));

-- *********************************************************************************************************

Query-I: Find employees earning more than their company average payroll. Payroll means total salary given by that company.

select employee_name
from works w
where salary > (
	select avg(salary)
	from works
	where company_name = w.company_name
);

-- *********************************************************************************************************

Query-J: Find company with most number of employees.

select company_name
from works
group by company_name
having count(*) >= all (
	select count(*)
	from works
	group by company_name);

-- *********************************************************************************************************

Query-K: Find company with the smallest payroll. Payroll means total salary given by that company.

select company_name
from works
group by company_name
having sum(salary) <= all (
    select sum(salary)
	from works
	group by company_name);

-- *********************************************************************************************************
    
Query-L: Find companies whose average salary is more than Satyam average salary.

select company_name
from works
group by company_name
having avg(salary) > (
    select avg(salary)
    from works
    where company_name = 'satyam'
);

-- *********************************************************************************************************

Query-M: Find the employee_name with other details having second highest salary
    
select *
from employee
where employee_name in (
	select employee_name
	from works where salary in (
		select s from (
			select rownum rn, e, s
			from (
				select employee_name e, salary s
				from works
				order by salary desc)
    		)
		where rn = 2));
    
-- *********************************************************************************************************

drop table employee;
drop table manages;
drop table company;
drop table works;

-- *********************************************************************************************************

</pre>

</b>
