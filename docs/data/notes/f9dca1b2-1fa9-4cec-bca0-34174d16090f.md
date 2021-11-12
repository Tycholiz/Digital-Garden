
### row_to_json
turns the table into json
- each row becomes a json object, and the cumulative of these rows makes up the json array
- If the query `select * from nugget` gives us the data, then the whole below query gives us that same data as json
```
select row_to_json(nugget)
from (select * from "nugget") as nugget
```
### array_agg
It aggregates its argument into a PostgreSQL array
- accepts a set of values and returns an array. Each arg becomes an element in the array
	- we can determine the order of the array with ORDER BY
- ex. Imagine we had 2 many:many tables (`actors` and `movies`) connected through a junction table `movies_actors`.
	- We are interested in viewing which actors were in which movie.
	- Assuming that the `movies` table is the table we are joining from, if we were to query the movies, we'd get duplicate rows with the same movie and actor names (since each row is a combination of the two, effectively having the same amount of rows as the junction table).
![d26ee15403ed820f7c410d859e7c5e56.png](:/20ede76c8a314c528f1b4716c372db0d)
	- What we want instead is to show a different movie in each row, and consolidate the actors appearing in that movie into an array
![939e260f1cc0aab3f7f7e6477164e5ac.png](:/9d7a1c247b064dc285cd0597a477d28a)

### json_agg
- similar to `array_agg`, except that it puts the elements into a json array
- we could use `json_agg(json_build_object())` if we wanted to build an array of objects

Before
```sql
Select
    title,
    first_name || ' ' || last_name as name
from film
join film_actor using (film_id)
join actor using (actor_id)
group by title, first_name, last_name
order by title;
```

After
```sql
SELECT
     title,
     ARRAY_AGG (
	     first_name || ' ' || last_name
	     ORDER BY first_name
     ) actors
 FROM
     film
 INNER JOIN film_actor USING (film_id)
 INNER JOIN actor USING (actor_id)
 GROUP BY
     title
 ORDER BY
     title;
```

### json_build_object
Allows us to provide key-value pairs, thereby returning us an object
- if we want to provide an array as the value of a key (ex. `buckets` property of a `nuggets` object), we can provide a SELECT statement in place of the value
```sql
SELECT
    json_build_object(
        'id',id,
        'name',name,
        'comments',(
            SELECT json_agg(json_build_object(
                'id', comments.id,
                'mood', comments.mood,
                'subject', comments.subject,
                'content', comments.content,
                'created_at', comments.created_at
            ))
            FROM user_comment_map JOIN comments ON comment_id=comments.id
            WHERE user_id=users.id
        )
    )
```
this works because

### json_strip_nulls
- lets us exclude fields where the value is null
```sql
SELECT json_strip_nulls(json_build_object('name', p.name, 'birthday', p.birthday))
FROM person p;
```

### json_populate_record
- allows us to give postgres a string of json, which it will then convert into SQL row-format.
- the first arg we pass to

### jsonb_pretty
print json pretty in output

## jsonb manipulation methods

### jsonb_set & jsonb_insert
With these methods, it is not possible to add values at the root of the json object.

signature:
```
jsonb_set/jsonb_insert(target         jsonb,
          path           text[],
          new_value      jsonb,
          create_missing boolean default true)
```
#### jsonb_insert
Allows us to insert into JSON array (while preserving all of the original values)
