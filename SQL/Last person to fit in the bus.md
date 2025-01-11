## Table: Queue

| Column Name  | Type    |
|--------------|---------|
| person_id    | int     |
| person_name  | varchar |
| weight       | int     |
| turn         | int     |

- `person_id` column contains unique values.
- This table contains information about all people waiting for a bus.
- The `person_id` and `turn` columns contain all numbers from 1 to n, where n is the number of rows in the table.
- `turn` determines the order in which the people will board the bus. `turn=1` denotes the first person to board, and `turn=n` denotes the last person to board.
- `weight` represents the weight of each person in kilograms.

The bus has a weight limit of 1000 kilograms, so some people may not be able to board. Only one person can board the bus at any given turn.

## Problem Description

Write a solution to find the `person_name` of the last person who can fit on the bus without exceeding the weight limit. The test cases are designed such that the first person will always fit within the weight limit.

### Example 1:

#### Input:
Queue table:

| person_id | person_name | weight | turn |
|-----------|-------------|--------|------|
| 5         | Alice       | 250    | 1    |
| 4         | Bob         | 175    | 5    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 1         | Winston     | 500    | 6    |
| 2         | Marie       | 200    | 4    |

#### Output:

| person_name |
|-------------|
| John Cena   |

#### Explanation:
The following table is ordered by `turn` for simplicity:

| Turn | person_id | person_name | weight | Total Weight |
|------|-----------|-------------|--------|--------------|
| 1    | 5         | Alice       | 250    | 250          |
| 2    | 3         | Alex        | 350    | 600          |
| 3    | 6         | John Cena   | 400    | 1000         | (last person to board)
| 4    | 2         | Marie       | 200    | 1200         | (cannot board)
| 5    | 4         | Bob         | 175    | ___          |
| 6    | 1         | Winston   


# Solution:
## Solution:

```sql
WITH temp AS (
    SELECT person_name, 
           SUM(weight) OVER(ORDER BY turn ASC) AS total_weight
    FROM Queue
)
SELECT person_name 
FROM temp
WHERE total_weight <= 1000
ORDER BY total_weight DESC
LIMIT 1;