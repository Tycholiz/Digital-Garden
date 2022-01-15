
a migration is just a script that performs a set of synchronous database interactions
preservation of data during migrations in general is not guaranteed because schema changes such as the deletion of a database column can destroy data
- Therefore, you should never `drop table` in a migration if there is still data in that table.

migrations should always be backwards compatible. This gives us the freedom to rollback the migration, and have a database that is still valid.
- The proper approach to migrations is to expand the schema to work with the old version as well as the new version, then contract it to work only with the new version
- Imagine 2 different users have different versions of an application. Upon signup, the older version asks for first and last name, while the newer asks for middle name as well.

# Migrations without downtime
https://thorben-janssen.com/update-database-schema-without-downtime/
