# Pewlett-Hackard-Analysis: Technical Report

Bobby’s manager would like to know how many roles will need to be filled as the “silver tsunami” begins to make an impact. They would also like to identify retirement-ready employees who qualify to mentor the next generation of Pewlett Hackard employees. To provide the data, we must create two tables. The table rt_salary depicts the number of current employees who are about to retire, grouped by job title. The second table, table2, lists those employees from Table 1 who are eligible for the mentorship program.

To create the first table, I first joined retirement_info and titles tables with an inner join. Then I did another inner join to add salaries. However, then I ran into a problem due to the duplication of employees. To get the employees' most recent titles, I used the following partitioning code:
SELECT emp_no,
	first_name,
	last_name,
	to_date,
	title,
	salary
FROM rt_salary
(SELECT emp_no,
	first_name,
	last_name,
	to_date,
	title,
	salary, 
 ROW_NUMBER() OVER
 (PARTITION BY (emp_no)
 ORDER BY to_date DESC) rn
 FROM rt_salary) tmp WHERE rn = 1
ORDER BY emp_no;
However, for some reason this did not work, so I moved onto table2. To make table2 I did an inner join between retirement_info and titles tables, this time including the from_date. Then I did a second inner join with this table and the employees table so that I could filter employees with birthdays in 1965. Unfortunately, I ran into a problem of a blank table2 because the inner join could not match duplicating employee numbers.

Consequently, I cannot conclude how many retirees or mentors for Bobby's manager to expect. Below is my ERD:
![EmployeeRD](EmployeeRD.png)
