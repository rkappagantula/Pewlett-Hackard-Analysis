# Pewlett-Hackard-Analysis

#### *Employee database analysis on retiring employees using SQL and pgAdmin of postgreSQL*

## Overview
The goal of this project was to determine who will be retiring in the next few years and how many positions will they need to fill. This will help them future-proof by generating a list of all employees eligible for the retirement package and a list of how many job titles are going to be open after that generation of employees retires. 

## Resources
- Orginal datasets:
  - departments.csv
  - dept_emp.csv
  - dept_manager.csv
  - employees.csv
  - salaries.csv
  - titles.csv

- Software:
  - SQL
  - PostgreSQL
  - pgAdmin


## Results
- After creating the unique_titles table by joining the employees and titles tables, filtering them by date of birth and date hired, removing duplicates, and ordering the data points by date hired there are **72458 current employees retiring** as per the above criterion. 


- Out of those employees leaving, there are 25916 Senior Engineers, 24926 Senior Staff, 9285 Engineers, 7636 Staff, 3603 Technique Leaders, 1090 Assistant Engineers, and 2 Managers. 

- Created the mentorship_eligibility table by joining the employees, department employees, and titles tables. In this case, the criterion for the join was that the employees were born in 1965 and that they were currently working at PH, in order for them to apply to the retiring/mentorship package. There were 1,549 employees eligible 


- Out of those eligible employees, there are 420 Engineers, 474 Senior Staff, 250 Staff, 264 Senior Engineers, 77 Technique Leaders, and 64 Assistant Engineers. 


## Summary
As the silver tsunami approaches the idea would be to prepare and be on the look for 16520 employees. This number represents the number of people that are currently working at the company, have been there since 1982 to 1987, and their birth date is between 1960 and 1965 to be eligible to leave work. 

The plan is to offer these people the mentorship program so that they can keep mentoring new employees.

Below query will give us the data for employees leaving the organization by department

```
SELECT DISTINCT ON (emp_no) e.emp_no, d.dept_name, e.first_name, e.last_name, e.birth_date, de.from_date, de.to_date, t.title
INTO employees_leaving_by_dept
FROM employees as e
JOIN dept_emp as de
ON (e.emp_no = de.emp_no)
JOIN titles as t
ON (e.emp_no = t.emp_no)
JOIN departments as d
ON (de.dept_no = d.dept_no)
WHERE (de.to_date = '9999-01-01') AND (e.birth_date BETWEEN '1960-01-01' AND '1965-12-31')
	AND (de.from_date BETWEEN '1982-01-01' AND '1987-12-31')
ORDER BY e.emp_no
```

Below query will provide which departments will be most impacted due to employee retirement

```
SELECT COUNT(first_name) "Count", dept_name
FROM employees_leaving_by_dept
GROUP BY dept_name
ORDER BY "Count" desc;
```


To conclude it all depends on how many retiring employees are willing to stay and enroll in mentorship program. Assuming there is one mentor for 10 new employees. Assuming each year there will be 15,000 employees retiring and 12,000 new employees entering, we would need ~1200 - 1500 mentors for all the departments with primary focus on Developement, Production and Sales as these departments have high employee leaving counts.
