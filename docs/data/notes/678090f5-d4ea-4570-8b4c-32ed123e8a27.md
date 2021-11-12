
### Generate_series
```sql
INSERT INTO app_public.entities (id)
SELECT gen_random_uuid()
    FROM generate_series(1, 10) x
;

INSERT INTO app_public.organizations (id, entity_id, owner_id, display_name)
SELECT gen_random_uuid() AS id, e.id AS entity_id, gen_random_uuid() AS owner_id, random() AS name
    FROM app_public.entities e
;
```

# UE Resources
[Use generate_series](https://www.citusdata.com/blog/2018/03/14/fun-with-sql-generate-sql/)
[For fake data generation, have a look at pyramation's faker]
