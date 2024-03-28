
a JOIN is a query that accesses multiple rows of the same or different tables at one time
```sql
SELECT * FROM student_grades
	JOIN students ON student_grades.student_id = students.id
```
- every JOIN must have an ON
	- if the column names are identical in both tables, you can use USING
- we may alias the tablename for one of 2 reasons: firstly, it's convenient, and secondly we might join to the same table several times, requiring us to distinguish between columns from each different time the table was joined in.
```sql
select bks.starttime as start, facs.name as name
	from
		cd.facilities facs
		inner join cd.bookings bks
			on facs.facid = bks.facid
	where
		facs.facid in (0,1) and
		bks.starttime >= '2012-09-21' and
		bks.starttime < '2012-09-22'
order by bks.starttime;
```
- any time you write `FROM table1, table2` (ie. including 2 tables instead of 1), you are writing a JOIN, even though it doesn't explicitly say `JOIN`
- all filtering of rows and columns should be done before the JOIN

- Joins are fairly expensive, which might be a reason to denormalize.
![83fe4afa6ddc9f8d5b3665cfde631660.png](:/cad05cee711947d682fb8e53e85ab34c)

## Inner Join
- create new tables with common data between the two reference tables.
- Will not include rows where one of the relevant piece of data are missing
	- ex. if joining `students` tables and `cohorts` table, and one student doesn't have a `cohort_id`, then that student won't be included
- The common columns are typically the primary key columns of the first table and foreign key columns of the second table
- with inner joins, order doesn't matter
- the inner join will examine each row in the first table and compare each row's value (specified after `ON`). If these values are equal, the inner join creates a new row that contains columns from both tables
	- ex. `... FROM table1 INNER JOIN table2 ON table1_name_col = table2_name_col`
- ~99% of the time, we want to use an INNER JOIN
- if we made a venn diagram of table1 and table2, an inner join would give us the intersect.
	- ie., performing an inner join will give us the columns(?) in common between the 2 tables
![820988c3645cc0df3da155abc938228f.png](:/066d1e23ea0a45738633ae20ae3f38ee)
- you can join a single table with itself. You may do this if you want to derive information from the table (that is not explicitly stored), while using it to determine
	- ex. below we are getting "the members who have bee recommended by another member". since we are using data within the same table to do the determination, we need to join with itself.
```sql
select
recs.firstname as firstname,
recs.surname as surname
from cd.members mems
join cd.members recs
on recs.memid = mems.recommendedby
order by surname, firstname
```
