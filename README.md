# SQL.Part3.AggregateMethods

ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD';

-- DROP TABLE EMP;
-- DROP TABLE DEPT;
-- DROP TABLE SALGRADE;

CREATE TABLE EMP
       (EMPNO NUMBER(4) NOT NULL,
        ENAME VARCHAR2(10) NOT NULL,
        JOB VARCHAR2(9) NOT NULL,
        MGR NUMBER(4),
        HIREDATE DATE NOT NULL,
        SAL NUMBER(7, 2) NOT NULL,
        COMM NUMBER(7, 2),
        DEPTNO NUMBER(2),
        CONSTRAINT EMP_PK PRIMARY KEY (EMPNO));


CREATE TABLE DEPT
       (DEPTNO NUMBER(2),
        DNAME VARCHAR2(14),
        LOC VARCHAR2(13),
        CONSTRAINT DEPT_PK PRIMARY KEY (DEPTNO));

ALTER TABLE EMP ADD CONSTRAINT EMP_DEPT_FK FOREIGN KEY (DEPTNO)
REFERENCES DEPT (DEPTNO);


INSERT INTO DEPT VALUES (10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO DEPT VALUES (20, 'RESEARCH',   'DALLAS');
INSERT INTO DEPT VALUES (30, 'SALES',      'CHICAGO');
INSERT INTO DEPT VALUES (40, 'OPERATIONS', 'BOSTON');


INSERT INTO EMP VALUES
        (7369, 'SMITH',  'CLERK',     7902,
        TO_DATE('1980-12-17','YYYY-MM-DD'),  800, NULL, 20);
INSERT INTO EMP VALUES
        (7499, 'ALLEN',  'SALESMAN',  7698,
        TO_DATE('1981-02-20','YYYY-MM-DD'), 1600,  300, 30);
INSERT INTO EMP VALUES
        (7521, 'WARD',   'SALESMAN',  7698,
        TO_DATE('1981-02-22','YYYY-MM-DD'), 1250,  500, 30);
INSERT INTO EMP VALUES
        (7566, 'JONES',  'MANAGER',   7839,
        TO_DATE('1981-04-02','YYYY-MM-DD'),  2975, NULL, 20);
INSERT INTO EMP VALUES
        (7654, 'MARTIN', 'SALESMAN',  7698,
        TO_DATE('1981-09-28','YYYY-MM-DD'), 1250, 1400, 30);
INSERT INTO EMP VALUES
        (7698, 'BLAKE',  'MANAGER',   7839,
        TO_DATE('1981-05-01','YYYY-MM-DD'),  2850, NULL, 30);
INSERT INTO EMP VALUES
        (7782, 'CLARK',  'MANAGER',   7839,
        TO_DATE('1981-06-09','YYYY-MM-DD'),  2450, NULL, 10);
INSERT INTO EMP VALUES
        (7788, 'SCOTT',  'ANALYST',   7566,
        TO_DATE('1982-12-09','YYYY-MM-DD'), 3000, NULL, 20);
INSERT INTO EMP VALUES
        (7839, 'KING',   'PRESIDENT', NULL,
        TO_DATE('1981-11-17','YYYY-MM-DD'), 5000, NULL, 10);
INSERT INTO EMP VALUES
        (7844, 'TURNER', 'SALESMAN',  7698,
        TO_DATE('1981-09-08','YYYY-MM-DD'),  1500,    0, 30);
INSERT INTO EMP VALUES
        (7876, 'ADAMS',  'CLERK',     7788,
        TO_DATE('1983-01-12','YYYY-MM-DD'), 1100, NULL, 20);
INSERT INTO EMP VALUES
        (7900, 'JAMES',  'CLERK',     7698,
        TO_DATE('1981-12-03','YYYY-MM-DD'),   950, NULL, 30);
INSERT INTO EMP VALUES
        (7902, 'FORD',   'ANALYST',   7566,
        TO_DATE('1981-12-03','YYYY-MM-DD'),  3000, NULL, 20);
INSERT INTO EMP VALUES
        (7934, 'MILLER', 'CLERK',     7782,
        TO_DATE('1982-01-13','YYYY-MM-DD'), 1300, NULL, 10);
INSERT INTO EMP VALUES
        (7935, 'GHOST', 'MANAGER',     7782,
        TO_DATE('1982-01-13','YYYY-MM-DD'), 1300, NULL, NULL);

CREATE TABLE SALGRADE
        (GRADE INTEGER,
         LOSAL NUMBER(7, 2),
         HISAL NUMBER(7, 2));

INSERT INTO SALGRADE VALUES (1,  700, 1200);
INSERT INTO SALGRADE VALUES (2, 1201, 1400);
INSERT INTO SALGRADE VALUES (3, 1401, 2000);
INSERT INTO SALGRADE VALUES (4, 2001, 3000);
INSERT INTO SALGRADE VALUES (5, 3001, 9999);

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

-- COMMIT;


-- Task 2 join tables

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
INNER JOIN SALGRADE S ON E.SAL BETWEEN S.LOSAL AND S.HISAL;


-- 7. Select all information for employees in 3rd earning class

SELECT E.*
FROM EMP E
INNER JOIN SALGRADE S ON E.SAL BETWEEN S.LOSAL AND S.HISAL
WHERE S.GRADE = 3;


-- 8. Find employees who work in Dallas

SELECT E.ENAME
FROM EMP E
INNER JOIN DEPT D ON E.DEPTNO = D.DEPTNO
WHERE D.LOC = 'DALLAS';


-- 9. For each employee select his name, department name and earning class

SELECT E.ENAME, D.DNAME, S.GRADE
FROM EMP E
INNER JOIN DEPT D ON E.DEPTNO = D.DEPTNO
INNER JOIN SALGRADE S ON E.SAL BETWEEN S.LOSAL AND S.HISAL;


-- 10. Find all employees with their numbers and names of departments they work in. Include departments without any employees

SELECT E.EMPNO, E.ENAME, D.DNAME
FROM DEPT D
LEFT JOIN EMP E ON D.DEPTNO = E.DEPTNO;


-- 11. Find all employees with their numbers and names of departments they work in. Include employees without departments

SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E
LEFT JOIN DEPT D ON E.DEPTNO = D.DEPTNO;


-- 12. Find employees (name, department number) from departments 20 and 30

SELECT E.ENAME, E.DEPTNO
FROM EMP E
WHERE E.DEPTNO IN (20, 30);


-- 13. List jobs appearing in departments 10 and 30

SELECT DISTINCT E.JOB
FROM EMP E
WHERE E.DEPTNO IN (10, 30);


-- 14. List jobs appearing in department 10 or 30 (or both)

SELECT DISTINCT E.JOB
FROM EMP E
WHERE E.DEPTNO = 10
   OR E.DEPTNO = 30;


-- 15. List jobs appearing in department 10 but not in department 30

SELECT DISTINCT E.JOB
FROM EMP E
WHERE E.DEPTNO = 10
  AND E.JOB NOT IN (SELECT DISTINCT JOB FROM EMP WHERE DEPTNO = 30);


-- 16. Find employees who earn less than their managers

SELECT E.ENAME AS Employee, M.ENAME AS Manager, E.SAL AS Employee_Salary, M.SAL AS Manager_Salary
FROM EMP E
JOIN EMP M ON E.MGR = M.EMPNO
WHERE E.SAL < M.SAL;


-- 17. For each employee show his name and name of the manager. Sort results by managers name

SELECT E.ENAME AS Employee, M.ENAME AS Manager
FROM EMP E
LEFT JOIN EMP M ON E.MGR = M.EMPNO
ORDER BY M.ENAME;

-- Part 3 - Aggregate Methods

-- 1. Find average salary in company

SELECT ROUND(AVG(SAL), 2) AS AverageSalary
FROM EMP;

-- 2. Find minimum earnings of clerks

SELECT MIN(SAL) AS MinimumEarnings
FROM EMP
WHERE JOB = 'CLERK';

-- 3. Count, how many employees work in department 20

SELECT COUNT(*) AS EmployeeCount
FROM EMP
WHERE DEPTNO = 20;


-- 4. Find average salary for each job

SELECT JOB, AVG(SAL) AS AverageSalary
FROM EMP
GROUP BY JOB;


-- 5. Find average salary for each job except managers

SELECT JOB, AVG(SAL) AS AverageSalary
FROM EMP
WHERE JOB != 'MANAGER'
GROUP BY JOB;


-- 6. Find average salary for each job and department

SELECT JOB, DEPTNO, AVG(SAL) AS AverageSalary
FROM EMP
GROUP BY JOB, DEPTNO;


-- 7. Find maximum salary for each job

SELECT JOB, MAX(SAL) AS MaximumSalary
FROM EMP
GROUP BY JOB;


-- 8. Find average salary for departments with at least 3 employees

SELECT DEPTNO, AVG(SAL) AS AverageSalary
FROM EMP
GROUP BY DEPTNO
HAVING COUNT(*) >= 3;


-- 9. Find jobs with average salary > 3000

SELECT JOB, AVG(SAL) AS AverageSalary
FROM EMP
GROUP BY JOB
HAVING AVG(SAL) > 3000;


-- 10. Find average salary and annual earnings for each job (including commission)

SELECT JOB, AVG(SAL) AS AverageSalary, AVG(SAL * 12 + NVL(COMM, 0)) AS AverageAnnualEarnings
FROM EMP
GROUP BY JOB;


-- 11. Find departments with at least 3 employees

SELECT DEPTNO, COUNT(*) AS EmployeeCount
FROM EMP
GROUP BY DEPTNO
HAVING COUNT(*) >= 3;


-- 12. Check if all employee numbers are mutually different

SELECT COUNT(DISTINCT EMPNO) = COUNT(*) AS AllUniqueEmployeeNumbers
FROM EMP;


-- 13. Find lowest earnings paid by managers to their subordinates. Display manager name, subordinate name and subordinate salary. Sort results ascending by salary

-- SELECT M.ENAME AS ManagerName, E.ENAME AS SubordinateName, E.SAL AS SubordinateSalary
-- FROM EMP E
-- JOIN EMP M ON E.MGR = M.EMPNO
-- WHERE E.SAL = (SELECT MIN(SAL) FROM EMP WHERE MGR = M.EMPNO)
-- ORDER BY E.SAL ASC;


-- 14. Count how many employees work in department located in DALLAS

SELECT COUNT(*) AS EmployeeCount
FROM EMP E
JOIN DEPT D ON E.DEPTNO = D.DEPTNO
WHERE D.LOC = 'DALLAS';


-- 15. Find highest earnings for each salgrade

SELECT GRADE, MAX(HISAL) AS HighestEarnings
FROM SALGRADE
GROUP BY GRADE;


-- 16. Find recurring amounts of earnings and display how many employees receive it (duplicated salaries)

SELECT SAL, COUNT(*) AS EmployeeCount
FROM EMP
GROUP BY SAL
HAVING COUNT(*) > 1;


-- 17. Find average earnings for employees in second salgrade

SELECT AVG(SAL * 12 + NVL(COMM, 0)) AS AverageAnnualEarnings
FROM EMP
WHERE SAL BETWEEN (SELECT LOSAL FROM SALGRADE WHERE GRADE = 2)
              AND (SELECT HISAL FROM SALGRADE WHERE GRADE = 2);


-- 18. Find how many subordinates are assigned to each manager. Include employees with no subordinates

SELECT M.ENAME AS ManagerName, COUNT(E.EMPNO) AS SubordinateCount
FROM EMP M
LEFT JOIN EMP E ON M.EMPNO = E.MGR
GROUP BY M.ENAME;


-- 19. Display sum of salary in first salgrade

SELECT SUM(SAL) AS TotalSalary
FROM EMP
WHERE SAL BETWEEN (SELECT LOSAL FROM SALGRADE WHERE GRADE = 1)
              AND (SELECT HISAL FROM SALGRADE WHERE GRADE = 1);

-- COMMIT;
