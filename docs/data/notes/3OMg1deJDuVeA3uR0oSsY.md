
In contrast to the way a view is queried, a materialized view actually takes up storage space, making it a proper table (and not just a conceptual table). When you query a materialized view, the result set is cached and stored (or, materialized) as a sort of pseudo table
- can be thought of as a snapshot of a query set
- makes queries more efficient at the expense of space
