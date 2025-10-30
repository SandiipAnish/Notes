SQl Window Functions



-> allows performing calculations across a set of rows that are related to the current row, without collapsing the result into a single value.

-> used in aggregates, ranking, and running totals.



The "OVER" clause defines the "window" of rows for the calculation.

* PARTITION BY : divide the data into groups.
* ORDER BY     : specify  the order of rows within each group.



The functions that can be applied in a controlled way:

* SUM()
* AVG()
* ROW\_NUMBER()
* RANK()
* DENSE\_RANK()



SYNTAX:

SELECT column\_name1, window\_function(column\_name2) OVER (\[PARTITION BY column\_name3] \[ORDER BY column\_name4]) AS new\_column FROM table\_name;



KEY TERMS:

* window\_function : Any aggregate or ranking function such SUM(), AVG(), ROW\_NUMBER(), etc...
* column\_name1    : Regular columns to be selected in the output.
* column\_name2    : Column on which the window function is applied.
* column\_name3    : Column used for dividing rows into groups (PARTITION BY).
* column\_name4    : Column used to define order of rows within each parititon (ORDER BY).
* new\_column      : Alias for calculated result of the window function.
* table\_name      : table from which data is selected.



TYPES:

-> aggregate window functions

* SUM()
* AVG()
* COUNT()
* MAX()
* MIN()



-> ranking window functions

* RANK()
* DENSE\_RANK()
* ROW\_NUMBER()
* PERCENT\_RANK()



-> these two serve different purposes but share a common ability to perform calculations over a defined set of rows while retaining the original data.



Employees table



Name     Age    Department    Salary

Ramesh   20     Finance       50,000

Suresh   22     Finance       50,000

Ram      28     Finance       20,000

Deep     25     Sales         30,000

Pradeep  22     Sales         20,000



AVG():



SELECT Name, Age, Department, Salary, AVG(Salary) OVER (PARTITION BY Department) AS Avg\_Salary FROM employee;



Name     Age    Department    Salary    Avg\_Salary

Ramesh   20     Finance       50,000    40,000

Suresh   22     Finance       50,000    40,000

Ram      28     Finance       20,000    40,000

Deep     25     Sales         30,000    25,000

Pradeep  22     Sales         20,000    25,000



RANK():



-> It assigns ranks to rows within a partition, with the same rank given to rows with identical values.

-> If two rows share the same rank, the next rank is skipped.



SELECT Name, Department, Salary, RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS emp\_rank FROM employee;



Name     Age    Department    Salary    emp\_rank

Ramesh   20     Finance       50,000      1

Suresh   22     Finance       50,000      1

Ram      28     Finance       20,000      3

Deep     25     Sales         30,000      1

Pradeep  22     Sales         20,000      2



DENSE\_RANK():



-> When ranking rows in SQL, ties can sometimes create gaps in the ranking sequence.

-> DENSE\_RANK() is used to avoid this it assigns the same rank to rows with equal values but continues ranking with the next consecutive number, without skipping.



SELECT Name, Department, Salary, DENSE\_RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS emp\_dense\_rank FROM employee;



Name     Age    Department    Salary   emp\_dense\_Rank

Ramesh   20     Finance       50,000       1

Suresh   22     Finance       50,000       1

Ram      28     Finance       20,000       2

Deep     25     Sales         30,000       1

Pradeep  22     Sales         20,000       2



ROW\_NUMBER():



-> It gives each row a unique number. It numbers rows from one to the total rows.

-> The rows are put into groups based on their values.

-> Each group is called a partition.

-> In each partition, rows get numbers one after another.

-> No two rows have the same number in a partition.



SELECT Name, Department, Salary, ROW\_NUMBER() OVER (PARTITION BY Department ORDER BY Salary DESC) AS emp\_row\_no FROM employee;



Name     Age    Department    Salary    emp\_row\_no

Ramesh   20     Finance       50,000       1

Suresh   22     Finance       50,000       2

Ram      28     Finance       20,000       3

Deep     25     Sales         30,000       1

Pradeep  22     Sales         20,000       2



PERCENT\_RANK():



->It shows the relative position of a row compared to others in the same partition.
-> Formula:



PERFECT\_RANK = (RANK - 1) / (Total Rows in Partition - 1)



SELECT Name, Department, Salary, PERCENT\_RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS emp\_percent\_rank FROM employee;



Name     Age    Department    Salary    emp\_percent\_rank

Ramesh   20     Finance       50,000          0.00

Suresh   22     Finance       50,000          

Ram      28     Finance       20,000

Deep     25     Sales         30,000

Pradeep  22     Sales         20,000



