
##### Inserting a composite type (a whole row at once)
```sql
insert into stats.daily_registration_historical (registration_date, count_registrations)
select * from app_private.daily_registration;
```

##### Insert all default values
```sql
insert into users default values
```

##### Insert multiple rows
```sql
insert into countries (code) values ('CA'), ('US'), ('FR')
```

##### Bulk insert
Imagine you need to split a table in two, and move half of the rows from the first to the second:
```sql
insert into second_table (col1, col2) select a, b from first_table;
```
