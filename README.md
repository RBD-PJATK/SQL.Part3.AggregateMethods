# SQL.Part3.AggregateMethods

Here is your SQL script converted to GitHub-flavored Markdown format:

```sql
-- Set date format
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD';

-- Create EMP table
CREATE TABLE EMP (
    EMPNO NUMBER(4) NOT NULL,
    ENAME VARCHAR2(10) NOT NULL,
    JOB VARCHAR2(9) NOT NULL,
    MGR NUMBER(4),
    HIREDATE DATE NOT NULL,
    SAL NUMBER(7, 2) NOT NULL,
    COMM NUMBER(7, 2),
    DEPTNO NUMBER(2),
    CONSTRAINT EMP_PK PRIMARY KEY (EMPNO)
);

-- Create DEPT table
CREATE TABLE DEPT (
    DEPTNO NUMBER(2),
    DNAME VARCHAR2(14),
    LOC VARCHAR2(13),
    CONSTRAINT DEPT_PK PRIMARY KEY (DEPTNO)
);

-- Add foreign key constraint to EMP table
ALTER TABLE EMP ADD CONSTRAINT EMP_DEPT_FK FOREIGN KEY (DEPTNO) REFERENCES DEPT (DEPTNO);

-- Insert data into DEPT table
INSERT INTO DEPT VALUES (10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO DEPT VALUES (20, 'RESEARCH', 'DALLAS');
INSERT INTO DEPT VALUES (30, 'SALES', 'CHICAGO');
INSERT INTO DEPT VALUES (40, 'OPERATIONS', 'BOSTON');

-- Insert data into EMP table
INSERT INTO EMP VALUES (7369, 'SMITH', 'CLERK', 7902, TO_DATE('1980-12-17', 'YYYY-MM-DD'), 800, NULL, 20);
INSERT INTO EMP VALUES (7499, 'ALLEN', 'SALESMAN', 7698, TO_DATE('1981-02-20', 'YYYY-MM-DD'), 1600, 300, 30);
INSERT INTO EMP VALUES (7521, 'WARD', 'SALESMAN', 7698, TO_DATE('1981-02-22', 'YYYY-MM-DD'), 1250, 500, 30);
INSERT INTO EMP VALUES (7566, 'JONES', 'MANAGER', 7839, TO_DATE('1981-04-02', 'YYYY-MM-DD'), 2975, NULL, 20);
INSERT INTO EMP VALUES (7654, 'MARTIN', 'SALESMAN', 7698, TO_DATE('1981-09-28', 'YYYY-MM-DD'), 1250, 1400, 30);
INSERT INTO EMP VALUES (7698, 'BLAKE', 'MANAGER', 7839, TO_DATE('1981-05-01', 'YYYY-MM-DD'), 2850, NULL, 30);
INSERT INTO EMP VALUES (7782, 'CLARK', 'MANAGER', 7839, TO_DATE('1981-06-09', 'YYYY-MM-DD'), 2450, NULL, 10);
INSERT INTO EMP VALUES (7788, 'SCOTT', 'ANALYST', 7566, TO_DATE('1982-12-09', 'YYYY-MM-DD'), 3000, NULL, 20);
INSERT INTO EMP VALUES (7839, 'KING', 'PRESIDENT', NULL, TO_DATE('1981-11-17', 'YYYY-MM-DD'), 5000, NULL, 10);
INSERT INTO EMP VALUES (7844, 'TURNER', 'SALESMAN', 7698, TO_DATE('1981-09-08', 'YYYY-MM-DD'), 1500, 0, 30);
INSERT INTO EMP VALUES (7876, 'ADAMS', 'CLERK', 7788, TO_DATE('1983-01-12', 'YYYY-MM-DD'), 1100, NULL, 20);
INSERT INTO EMP VALUES (7900, 'JAMES', 'CLERK', 7698, TO_DATE('1981-12-03', 'YYYY-MM-DD'), 950, NULL, 30);
INSERT INTO EMP VALUES (7902, 'FORD', 'ANALYST', 7566, TO_DATE('1981-12-03', 'YYYY-MM-DD'), 3000, NULL, 20);
INSERT INTO EMP VALUES (7934, 'MILLER', 'CLERK', 7782, TO_DATE('1982-01-13', 'YYYY-MM-DD'), 1300, NULL, 10);
INSERT INTO EMP VALUES (7935, 'GHOST', 'MANAGER', 7782, TO_DATE('1982-01-13', 'YYYY-MM-DD'), 1300, NULL, NULL);

-- Create SALGRADE table
CREATE TABLE SALGRADE (
    GRADE INTEGER,
    LOSAL NUMBER(7, 2),
    HISAL NUMBER(7, 2)
);

-- Insert data into SALGRADE table
INSERT INTO SALGRADE VALUES (1, 700, 1200);
INSERT INTO SALGRADE VALUES (2, 1201, 1400);
INSERT INTO SALGRADE VALUES (3, 1401, 2000);
INSERT INTO SALGRADE VALUES (4, 2001, 3000);
INSERT INTO SALGRADE VALUES (5, 3001, 9999);

-- Task 1 - join tables

-- 1. Find all departments, employee names and employee numbers of their supervisors from table EMP

SELECT E.DEPTNO, E.ENAME, M.EMPNO AS SUPERVISOR_EMPNO
FROM EMP E
LEFT JOIN EMP M ON E.MGR = M.EMPNO;

-- 2. Print all values from all columns from table EMP.

SELECT * FROM EMP;

-- 3. Calculate annual salary for each employee (column sal keeps an information about one month)

SELECT EMPNO, ENAME, JOB, SAL * 12 AS ANNUAL_SALARY
FROM EMP;

-- 4. Display employee name and his annual salary assuming a raise of 250

SELECT ENAME, (SAL + 250) * 12 AS ANNUAL_SALARY_WITH_RAISE
FROM EMP;

-- 5. Display employee name and his annual salary. Change header of column to Annual:
SELECT ENAME, SAL * 12 AS ANNUAL
FROM EMP;

-- 6. Display employee name and his annual salary. Change header of column to Annual Salary

SELECT ENAME, SAL * 12 AS "Annual Salary"
FROM EMP;

-- 7. Display employee number and name as one column. Change its header to Hired

SELECT EMPNO || ' ' || ENAME AS HIRED
FROM EMP;

-- 8. Display information about each employee in one column in form "Employee with number [EMPNO] works in department [DEPTNO] and earns [SAL] per month" Change header of column to Information about Employee

SELECT 'Employee with number ' || EMPNO || ' works in department ' || DEPTNO || ' and earns ' || SAL || ' per month' AS "Information about Employee"
FROM EMP;

-- 9. For each employee, display its name and annual earnings including commission (column comm)

SELECT ENAME, (SAL * 12) + NVL(COMM, 0) AS ANNUAL_EARNINGS
FROM EMP;

-- 10. Display all department numbers from table EMP

SELECT DEPTNO
FROM EMP;

-- 11. Display all department numbers from table EMP without repetition

SELECT DISTINCT DEPTNO
FROM EMP;

-- 12. Display all mutually different combinations of deptno and job from table EMP

SELECT DISTINCT DEPTNO, JOB
FROM EMP;

-- 13. Display all information from table EMP. Sort results ascending according to the value of column ename

SELECT *
FROM EMP
ORDER BY ENAME ASC;

-- 14. Display all information from table EMP. Sort results descending according to the hiredate

SELECT *
FROM EMP
ORDER BY HIREDATE DESC;

-- 15. Display all information from table EMP. Sort results according to values in columns: deptno (ascending) and sal (descending)

SELECT *
FROM EMP
ORDER BY DEPTNO ASC, SAL DESC;

-- Task 2 - join tables

-- 1. Join data from EMP and DEPT using WHERE clause

SELECT E.EMPNO, E.ENAME, E.JOB, E.SAL, D.DNAME, D.LOC
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO;

-- 2. Join data from EMP and DEPT using INNER JOIN

SELECT E.EMPNO, E.ENAME, E.JOB, E.SAL, D.DNAME, D.LOC
FROM EMP E
INNER JOIN DEPT D ON E.DEPTNO = D.DEPTNO;

-- 3. Select employee name and department name for all employees in alphabetical order

SELECT E.ENAME, D.DNAME
FROM EMP E
INNER JOIN DEPT D ON E.DEPTNO = D.DEPTNO
ORDER BY E.ENAME ASC;

-- 4. Find all employees with their numbers and names of departments they work in

SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E
INNER JOIN DEPT D ON E.DEPTNO = D.DEPTNO;

-- 5. Select employee name, job and department name for all employees who earn more than 1500

SELECT E.ENAME, E.JOB, D.DNAME
FROM EMP E
INNER JOIN DEPT D ON E.DEPTNO = D.DEPTNO
WHERE E.SAL > 1500;

-- 6. Get a list of employees with name, job, salary and earning class (table SALGRADE)

SELECT E.ENAME, E.JOB, E.SAL, S.GRADE
FROM EMP E
INNER JOIN SALGRADE S ON E.SAL
