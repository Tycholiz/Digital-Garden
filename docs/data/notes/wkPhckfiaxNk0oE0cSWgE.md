
We can use postgres views in such a way that it is exposed on the graphql schema to be consumed, as if it were an actual table.

Imagine we have the following:
```sql
CREATE TABLE app_public.films (
  id serial PRIMARY KEY,
  name text,
  release_year int,
  kind text
);

CREATE VIEW comedies AS
    SELECT *
    FROM app_public.films
    WHERE kind = 'Comedy';
```

The view `comedies` is able to be queried directly on the Graphql schema:
```
{
  comedies(first: 20) {
    name
    releaseYear
  }
}
```
