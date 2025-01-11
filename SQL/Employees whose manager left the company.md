## Table: Employees

| Column Name | Type     |
|-------------|----------|
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
| salary      | int      |

- `employee_id` is the primary key for this table.
- This table contains information about the employees, their salary, and the ID of their manager. Some employees do not have a manager (`manager_id` is null).

### Problem Description

Find the IDs of the employees whose salary is strictly less than $30,000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their `manager_id` set to the manager that left.

Return the result table ordered by `employee_id`.

### Example

#### Input

Employees table:
| employee_id | name      | manager_id | salary |
|-------------|-----------|------------|--------|
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | null       | 31000  |
| 13          | Emery     | null       | 67084  |


# Solution:

```sql
SELECT e.employee_id
FROM employees e
WHERE e.manager_id NOT IN (
    SELECT employee_id FROM employees
) 
AND e.salary < 30000
ORDER BY e.employee_id;
