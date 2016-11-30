<Question 5>
Write a SQL query using the professor / department / compensation data that outputs the average number of vacation days by department:


```bash
sqlite> SELECT department.department_name, AVG(compensation.vacation_days) AS average_vacation_days
   ...> FROM department
   ...> INNER JOIN professor ON professor.department_id = department.id
   ...> INNER JOIN compensation ON compensation.professor_id = professor.id
   ...> GROUP BY department.department_name
   ...> ORDER BY MAX(compensation.vacation_days) ASC;


  # Result
  department_name  average_vacation_days
  ---------------  ---------------------
  Transfiguration  2.0                  
  Study of Ancien  8.0                  
  Defence Against  9.0                  
  Care of Magical  13.0
```
