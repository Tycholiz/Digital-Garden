
`RANK()` lets us assign a rank to every row according to a partition we define with PARTITION BY
- ex. imagine we had a result set of every person in a company along with their salaries. We want to rank the top earning employees by position (eg. top 3 salespeople; top 3 programmers etc). To get this result set, we can RANK, partitioning over the position_title
```sql
select 
    salary
    rank() over (partition by position_title order by salary) from employees
```

The `RANK()` function can be useful for creating top-N and bottom-N reports
