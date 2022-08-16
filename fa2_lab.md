# LAB CONTENT FOR FA2 OF DBMS

#### Topics to be covered :
1. Correlated Query
2. Nested Query
3. Create View
4. Create Database
5. Backup Database
6. Recovery Database
7. TCL (Transactional Control Language) : Commit, Rollback, SavePoint
8. Priveleges : Grant & Revoke

## Correlated Query or Synchronized Query 
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


## Create View
SYNTAX : 
```
create view <viewname> as select * from tablename where <condition>;
```

Q : (i) Create table, insert values, select * from table.
(ii) Create view v1 with condition c1, create view v2 with condition c2
(iii) Print v1, print v2

```sql
create table chitkara_students (
  s_no int,
  std_name varchar2(10),
  r_no number,
  grp_no int 
);
insert into chitkara_students values (1, 'Nandini', 2110990924, 21);
insert into chitkara_students values (2, 'Muskan', 2110990903, 17);
insert into chitkara_students values (3, 'Nikhil', 2110990944, 17);
insert into chitkara_students values (4, 'Nirbhay', 2110990958, 20);
insert into chitkara_students values (5, 'Naman', 2110990910, 17);
insert into chitkara_students values (6, 'Lipi', 2110990826, 15);
select * from chitkara_students;
create view v1 as select * from chitkara_students where grp_no > 17;
create view v2 as select * from chitkara_students where r_no < 2110990940;
select * from v1;
select * from v2;
```


## Create Database
SYNTAX : 
```
create database <dbname>;
use <dbname>;
```
Now, you can create any no. of tables (create table <tablename>...) after implementation of use command.


## Backup Of Database
SYNTAX :
```
backup database <dbname> to <diskname with disk location> go;
```

EXAMPLE :
```sql
backup database chitkara_students to disk = 'C:\\Anyfolder\\Any.BAK', disk = 'D:\\Anyfolder1\\Any1.BAK'
with password = 'abc@123'
with stats = 1
with description = 'This db is connected with S & F'
go; 
```


## Recovery Of Database
Two Types : 
1. Log Based Recover 
suppose : 5 min -> 1 log file
memory usage is more
2. Checkpoints
after 1 hour, there is save for all files
so, no need to save multiple files when only one can do the recovery just like batch os


## Commit, Rollback
- Create table employee + Insert values 
- Select * from employee;
- Commit; (Means Save) 
- Delete emp_id from employee where age = 21;
- Rollback; (Means Undo)
- Select * from employee;

EXAMPLE : 
```sql
create table chitkara_students (
  s_no int,
  std_name varchar2(10),
  r_no number,
  grp_no int 
);
insert into chitkara_students values (1, 'Nandini', 2110990924, 21);
insert into chitkara_students values (2, 'Muskan', 2110990903, 17);
insert into chitkara_students values (3, 'Nikhil', 2110990944, 17);
insert into chitkara_students values (4, 'Nirbhay', 2110990958, 20);
insert into chitkara_students values (5, 'Naman', 2110990910, 17);
insert into chitkara_students values (6, 'Lipi', 2110990826, 15);
select * from chitkara_students;
commit;
delete from chitkara_students where grp_no = 20;
select * from chitkara_students;
rollback;
select * from chitkara_students;
```


## Save Point
- Begin transaction;
- Set transaction Read Only;
- Create + Insert + Select *
- Savepoint SN1; (save transaction in partial manner)
- Delete emp_id from employee where age = 20;
- Savepoint SN2;
- Rollback to SN1;
- Release savepoint SN1;
- Release savepoint SN2;
- Commit; (save all transactions in complete manner)


## Grant & Revoke Priveleges
- Grant [AlL/SELECT/EXECUTE]
GRANT SELECT ON EMPLOYEE TO PUBLIC/UNAME 
WITH GRANT; (means other person can also grant someone else, this isn't a secure method, this is optional)

- Revoke []
REVOKE SELECT ON EMPLOYEE FROM PUBLIC/UNAME;
