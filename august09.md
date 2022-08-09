## Corelated Query or Synchronized Query 
- Top Down Approachh {Outer Query (Inner Query)}

### exists and not exists operators

Q - Identify employee detail from employee table in some department.

```sql
create table emp4 (eid int, ename varchar2(1), ead varchar2(10));
create table dept4 (did varchar2(2), dname varchar2(5), eid int);

insert into emp4 values (1, 'A', 'Delhi');
insert into emp4 values (2, 'B', 'Delhi');
insert into emp4 values (3, 'C', 'Cdg');
insert into emp4 values (4, 'D', 'Bang');
insert into emp4 values (5, 'E', 'Mzn');

insert into dept4 values ('D1', 'CSE', 1);
insert into dept4 values ('D2', 'HR', 2);
insert into dept4 values ('D3', 'MKTG', 3);

select * from emp4;
select * from dept4;

select * from emp4 where exists (select * from dept4 where dept4.eid = emp4.eid);
select * from emp4 where not exists (select * from dept4 where dept4.eid = emp4.eid);
```


## Nested Query or Sub Queries 
- Bottom Up Approachh {Inner Query (Outer Query)}

### = and < > operators

Q - Identify employee name who is having maximum salary.

```sql
create table emp5 (eid int, ename varchar2(1), ead varchar2(10), salary number);

insert into emp5 values (1, 'A', 'Delhi', 10000);
insert into emp5 values (2, 'B', 'Delhi', 20000);
insert into emp5 values (3, 'C', 'Cdg', 30000);
insert into emp5 values (4, 'D', 'Bang', 40000);
insert into emp5 values (5, 'E', 'Mzn', 50000);

select * from emp5;

select ename from emp5 where salary = (select max(salary) from emp5);
select ename from emp5 where salary <> (select max(salary) from emp5);
```

### in operator (update and delete in sub queries)

```sql
create table emp7 (eid int, ename varchar2(1), depid varchar2(2), salary number);
create table dept7 (depid varchar2(2), dname varchar2(5));

insert into emp7 values (1, 'A', 'D1', 10000);
insert into emp7 values (2, 'B', 'D2', 20000);
insert into emp7 values (3, 'C', 'D3', 30000);
insert into emp7 values (4, 'D', 'D4', 40000);
insert into emp7 values (5, 'E', 'D5', 50000);

insert into dept7 values ('D1', 'CSE');
insert into dept7 values ('D2', 'HR');
insert into dept7 values ('D3', 'MKTG');

select * from emp7;
select * from dept7;

update emp7 set salary = salary*5 where depid in (select depid from dept7 where dname = 'HR');
delete from emp7 where depid in (select depid from dept7 where dname = 'CSE');

select * from emp7;
```