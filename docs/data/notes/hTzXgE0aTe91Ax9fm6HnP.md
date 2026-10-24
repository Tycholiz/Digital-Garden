
While inner and outer joins involve starting with one table and appending rows onto it, CROSS JOINs involve taking 2+ tables and generating a cartesian coordinate table as a result of it.
![](/assets/images/2021-05-21-17-12-27.png)

Effectively, this gives us a combination of each row in T1 and T2
- In the above image, T1 has 3 rows and T2 has 3 rows, so the result set of the CROSS JOIN has 9 (3 * 3) rows
