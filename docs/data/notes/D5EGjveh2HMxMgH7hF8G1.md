
pgTAP is a suite of database assertion functions that make it easy to write [[TAP|testing.TAP]]-emitting unit tests 
- These tests can be written in 2 ways:
    - scripting-style unit testing, typical with TAP test frameworks
    - xUnit-style test functions

With pgTAP, we can just compare values directly in the database. There is no need to do any extra work to get the database interface to talk to the database, fetch data, convert it, etc. You just use SQL

Example test:
```sql
-- Start a transaction.
BEGIN;
SELECT plan( 2 );
\set domain_id 1
\set src_id 1

-- Insert stuff.
SELECT ok(
    insert_stuff( 'www.foo.com', '{1,2,3}', :domain_id, :src_id ),
    'insert_stuff() should return true'
);

-- Check for domain stuff records.
SELECT is(
    ARRAY(
        SELECT stuff_id
          FROM domain_stuff
         WHERE domain_id = :domain_id
           AND src_id = :src_id
         ORDER BY stuff_id
    ),
    ARRAY[ 1, 2, 3 ],
    'The stuff should have been associated with the domain'
);

SELECT * FROM finish();
ROLLBACK;
```

## Schema testing
There are a wealth of assertion functions to test the schema
`has_table`, `col_is_pk`
