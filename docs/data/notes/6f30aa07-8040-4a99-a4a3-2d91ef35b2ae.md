
# Junction tables
Many:Many relationships are achieved by associative tables
- An associative (or junction) table maps two or more tables together by referencing the primary keys of each data table. In effect, it contains a number of foreign keys, each in a many-to-one relationship from the junction table to the individual data tables. The PK of the associative table is typically composed of the FK columns themselves.
